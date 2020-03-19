# Type and Schema

## User-defined type

```sql
create type Dollars as numeric(12,2) ;  
create table department
              (dept_name varchar (20),
               building varchar (15),
               budget Dollars);
```

### Create new domain

```sql
create domain Dollars as numeric(12, 2) not null;
create domain Pounds as numeric(12,2);
Create table instructor
				(  ID char(5) primary key,
				   name varchar(20),  
		  		   dept_name varchar(20),                                   
	   		   salary Dollars,                                   
	   		   comm Pounds 
				);
```

区别：Type即使底层类型相同也不能互相复制，domain可以



### Large-Object Type

**blob**: **binary large object** - object is a large collection of uninterpreted binary data (whose interpretation is left to an application outside of the database system)

**clob**: **character large object** - object is a large collection of character data

vWhen a query returns a large object, **a pointer is returned** rather than the large object itself.



### Catalogs

相当于用户的库？



# Integrity Constraints

域完整性()、实体完整性（主键的约束、unique）、参照完整性（外键的约束）和用户定义的完整性约束(not null，check())。

## 域完整性

```sql
create domain hourly-wage numeric(5,2)
constraint value-test check(value > = 4.00);
```

## 参照完整性

外键

Let *r*1(*R*1) and *r*2(*R*2) be relations with primary keys *K*1 and *K*2 respectively.

The subset a of R2 is a *foreign key* referencing *K*1 in relation *r*1, if for every *t*2 in *r*2 there *must* be a tuple *t*1 in *r*1 such that *t*1[*K*1] = *t*2[a].

E.G.

Assume there exists relation r and s:  r(A, B, C),    s(B, D) ,  we say attribute B is foreign key from relation r, and r is called referencing relation (参照关系) , and s is called referenced relation (被参照关系). 
     e.g.   ◆学生(学号,姓名,性别,专业号,年龄) ---参照关系
     专业(专业号,专业名称) ---被参照关系 (目标关系)
      其中属性专业号称为关系学生的外码。
◆instructor(ID,name,dept_name,salary) --- referencing relation 
  department(dept_name,building,budget) --- referenced relation
**参照关系中外码的值必须在被参照关系中实际存在，或为null.**



插入检查：被引用的表必须存在关联元组(学生的department必须存在



删除检查：reference的relation中删除元组，需在引用的表中检查是否有关联的元组(department里还有学生，不能删除、或得把那些学生一起删除)



更新检查：同insert



**外键关联的定义**

```sql
create table course
           (course_id varchar (8),
            title varchar (50),
            dept_name varchar (20),
            credits numeric (2,0) check (credits > 0),
            primary key (course_id ),
            foreign key (dept_name) references department)
```



### cascade

要删除/更新department，但是里面还有学生，把那些学生一起删除/更新

```sql
foreign key(dept_name) references department
	[ on delete cascade]
	[ on update cascade]
	. . . ) ;
	[on delete set null]
    [on delete set default]
```



if a cascading update or delete causes a constraint violation that cannot be handled by a further cascading operation, the system aborts the transaction. (如果在链式删除(一个表影响多个表，多个表影响更多表)中有某些数据的完整性被破坏，则整个事务会放弃)





## Assertion 断言

for complex check condition on several relations

```sql
CREATE ASSERTION <assertion-name>
CHECK <predicate>;
```

When an assertion is made, the system tests it for validity on every update that may violate the assertion. 每次更新都会检查assertion是否成立(predicate成立无事，不成立会报错)
This testing may introduce a significant amount of overhead; hence assertions should be used with great care.(负载很高)



E.g.

```sql
create assertion credits_earned_constraint check
           (not exists (select ID	-- 不存在“学分不等于“上过课且没挂科的”课程学分之和的”学生
                        from student
                        where tot_cred <> (select sum(credits)
                                           from takes natural join course
                                           where student.ID= takes.ID
                                           and grade is not null and grade <> ’F’ );
create assertion check not exists	-- 不存在同时上多节课的老师
      ( select ID, name, section_id, semester, year,time_slot_id, count(distinct building, room_number)
        from instructor natural join teaches natural join section
        group by (ID, name, section_id, semester, year,   
                  time_slot_id)
        having count(building, room_number) > 1);

```



## Trigger 触发器

作出某些修改之后会自动触发对数据库的修改



```sql
-- To ensure referential integrity on the time_slot_id  attribute of the section relation.
create trigger timeslot_check1 after insert on section
            referencing new row as nrow	-- 标准语句
            for each row
            when (nrow.time_slot_id not in (
                        select time_slot_id
                        from time_slot)) /* time_slot_id not present in time_slot */
            begin
                rollback	-- 回滚插入
            end;
create trigger timeslot_check2 after delete on time_slot
            referencing old row as orow	-- 标准语句
            for each row
            when (orow.time_slot_id not in (
                              select time_slot_id
                              from time_slot) /* last tuple for time_slot_id deleted from time_slot */
                              and orow.time_slot_id in (
                                             select time_slot_id
                                             from section)) /* and time_slot_id still referenced from section*/
          begin
            rollback
          end;
```



### External World Actions

不能直接对外操作，但是可以操作一个seperate的表来反映

PPT4.32

```sql
create trigger reorder-trigger after update of level on inventory
    referencing old row as orow, new row as nrow
    for each row
         when nrow.level < = (select level	-- 更新之后不足
			                       from minlevel
			                      where minlevel.item = nrow.item)
                    and orow.level > (select level	-- 更新之前还够，确保仅本次操作越界
			                            from minlevel
		                                    where minlevel.item = orow.item)
   begin
		insert into orders	-- 每个商品有自己的重新订货量
		        (select item, amount
		          from reorder
		          where reorder.item = orow.item)
   end
```



```sql
-- SQL Server
-- balance不能为负值，因此若为负的会自当创建一个借款账户
-- Inserted，deleted 相当于前法的nrow，(称为过渡表，transition table
create trigger overdraft-trigger on account
for update as 
if  inserted.balance < 0
begin
	insert into borrower
		(select customer-name,account-number
     	from depositor, inserted
     	where inserted.account-number = depositor.account-number)
    insert into loan values(inserted.account-number, 
                            inserted.branch-name, -inserted.balance)
    update account set balance = 0
    from account, inserted
    where account.account-number = inserted.account-number
end
-- MySQL
Create or replace trigger secure_student before insert or delete or update on student 
    Begin
        IF ( to_char(sysdate, ‘DY’) in (‘星期六’, ‘星期日’))
        OR (to_char(sysdate, ‘HH24’) NOT Between 8 and 17 )
        THEN  raise_application_error (-20506, ‘你只能在上班时间修改数据’);
		END IF;
    END;
drop trigger <触发器名>；
```

#### Trigger的用途

早期用途

* maintaining summary data (e.g. total salary of each department)
* Replicating databases by recording changes to special relations (called change or delta relations) and having a separate process that applies the changes over to a replica. 

There are better ways of doing these now:

* Databases today provide built in materialized view(物化视图)  facilities to maintain summary data;
* Databases provide built-in support for replication.



## 找不同

* Check：表内部的检查
* Assertion：多张表检查
* Trigger：可多张检查，可修改多张



# Authorization

**Database system level**

Authentication (验证) and authorization (授权) mechanisms allow specific users access only to required data

We concentrate on authorization in the rest of this chapter

**Operating system level**

Operating system super-users can do anything they want to the database! Good operating system level security is required.

**Network level**: must use encryption to prevent

Eavesdropping (unauthorized reading of messages)

Masquerading  (pretending to be an authorized user or sending messages supposedly from authorized users)

**Physical level**

Physical access to computers allows destruction of data by intruders;  traditional lock-and-key security is needed

Computers must also be protected from floods, fire, etc.  More in Chapter 17 (Recovery)

**Human level**

Users must be screened to ensure that an authorized users do not give access to intruders

Users should be trained on password selection and secrecy



## Grant

```sql
with grant option -- 可以把权限再给别人
```

## Revoke

```sql
REVOKE <privilege list> ON <relation name or view name>	 FROM <user list> [ restrict | cascade ]
-- 缺省：cascade把这个人，和给这个人赋出去的一起收回
-- restrict这个人赋出去的不收回了
```

## Limitation 局限性

* SQL does not support authorization at a tuple level
    * E.g. we cannot restrict students to see only (the tuples storing) their own grades by grant.
* With the growth in Web access to databases, database accesses come primarily from application servers.
    * End users don't have database user ids, they are all mapped to the same database user id;
* All end-users of an application (such as a web application) may be mapped to a single database user.
* The task of authorization in above cases falls on the application program, with no support from SQL
    * Benefit: fine grained authorizations, such as to individual tuples, can be implemented by the application.
    * Drawback: Authorization must  be done in application code, and may be dispersed all over an application.
    * Checking for absence of authorization loopholes becomes very difficult since it requires reading large amounts of application code.

# Audit Trails

