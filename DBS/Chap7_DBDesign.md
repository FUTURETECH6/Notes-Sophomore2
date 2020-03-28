# Entity Sets

The real world  can be modeled as:

* a collection of entities (实体), 
* relationships (联系) among entities.

Entities have attributes (属性)

* Example: student has id, name, age, sex and address

An entity set is a set of entities of the same type that share the same properties.

* Example: set of all students, companies, trees, holidays, customers, accounts, loans
* 一个实体集包含多个同类实体

## Attributes

**Domain** (域, value set)– the set of permitted values for each attribute 

Attribute types:

* Simple and composite attributes (简单和复合属性，如sex, name).
* Single-valued and multi-valued attributes(单值和多值属性)
    * E.g. multivalued attribute: phone-numbers (多个电话号码)
* Derived attributes (派生属性) 
    * Can be computed from other attributes
    * E.g.  age, given date of birth
    * versus base attributes or stored attributes (基属性，存储属性)

# Relationship Sets

A relationship is an association among several entities (是二个或多个不同类实体之间的关联)

A relationship set is a set of relationship of the same type. example:`advisor( s_ID,  i_ID)`

Formally, a relationship set is a mathematical relation among n >= 2 entities, each taken from entity sets `{(e1, e2, … en) | e1 \in E1, e2 \in E2, …, en \in En}` where (e1, e2, …, en) is a relationship，Ei  is entity set. Example: 	(98988, 76766) \in advisor, where  98988 \in student,  76766 \in instructor, where 98988 \in student, 76766 \in instructor

**Degree** of a relationship set



# Keys

* superkey超码：一个或多个属性的**集合**，可以唯一地区分不同的元组
* candidate key候选码：任意真子集都不是超码，如`{ID}, {name, dept_name}`是，而`{ID, name}`不是候选码
* primary key主码：人为选出用来区分元组的候选码



# E-R Diagram

•Lines link entity sets to relationship sets.

•Dashed lines link attributes of a relationship set to the relationship set.

•Double lines indicate total participation of an entity in a relationship set.

•Double diamonds represent identifying relationship sets linked to weak entity sets

表示方式看PPT7.18

## Roles

Recursive relationship set 自环联系集：一个ES通过一个RsS连到自己

## Aggregation

符号总结PPT7.45

# Extended E-R Features



# Design of an E-R Database Schema



# Reduction of an E-R Schema to Tables

