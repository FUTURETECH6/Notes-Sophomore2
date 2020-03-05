# DDL

## Create

```sql
create table insturctor(
    ID char(5),
    name varchar(20) not full,	-- 指定not full
    primary key(ID)	-- super-key, candidate
	);
```

### Integrity Constraints

* not null
* primary key
    * 可以写成`XXX primary key,`或`XXX, primary key(XXX)`
* check(P), where P is a predicate
    * check(salary >= 0)

### Variable

* char(n)：定长
* varchar(n)：变长
* int
* smallint
* numeric(p,d)：p位定点数，其中小数点右边有d位
* real, double：单双精度浮点数
* float(n)：精度至少为n的浮点数

### Domain

不同的函数：

* SqlServer: Char(65), substring(s, start, length), getdate(), datalength( ‘ abc ’), ……
* Oracle: chr(65), substr(s, start, length), sysdate , length( ‘ abc ’), to_char(sysdate, ’ yyyy/mm/dd ’) 得： 2007/02/28, to_date( ’ 07/02/27 ’, ’ yy/mm/dd ’ ), ……

相同的函数：

Abs()( 绝对值 )，exp()( 指数 )，round()(四舍五入)，sin()，cos()

## Drop & Alter

```mysql
drop table instructor

alter table r add A D;
alter table r add (A1 D1, A2 D2, ..., An Dn);
alter table r drop A D	-- 许多DBMS不支持(安全考虑
alter table r modify (ID char(10), salary not full)	-- 加属性或添加限制
```

