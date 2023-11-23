# API Document

## Basic graph pattern

#### add\_fact(self, s, p, o, p\_prefix)

Add a \<s, prefix: p, o> triple to the query.

#### add\_quantity(entity, prop, tag,time=False)

This function is used to acquire the value of a quantity property.

**Parameters:**

* entity(str): the entity
* prop(str): the quantity property you need to get
* tag(str):  An identifier used to name variables. Ultimately the variable with the property value of this quantity property is the tag.
* time(int/boolean/str): The constraint of time.
  * int: Limit the time to this year.
  * boolean: True means need to acquire the time，False means not need to acquire the time.
  * str: use the function to\_date to set the entire yyyy-mm-dd

#### add\_quantity\_by\_qualifier(self,entity,main\_prop,main\_obj,qualifier\_prop,tag)

Acquire the value of a quantity property which acts as a qualifier.

For example, the entity iPhone 12 has a property 'made from material' of which the value can be steel, glass and so on. The property mass is the qualifier for each material.&#x20;

**Parameters:**

* entity(str):  the entity, such as iPhone12
* main\_prop(str): the property of the statement, such as made from material
* main\_obj(str): the value of the main\_prop, such as steel.
* qualifier\_prop(str): the quantity property which acts as a qualifier. For example, mass.
* tag(str): An identifier used to name variables. Ultimately the variable with the property value of this quantity property is the tag.

#### add\_quantity\_with\_qualifier(self,entity,main\_prop,qualifier\_prop,qualifier\_obj,tag)

Acquire the value of a quantity property with a qualifire limiting other peoperties&#x20;

For example, the computer performance of Nvidia GeForce RTX 3090 has 4 statements which are limited by the qualifier uses. Their qualifier values are single-precision floating-point format, half-precision floating-point format, double-precision floating-point format and half-precision floating-point format seperately.

**Parameters:**

* entity(str): the entity, such as Nvidia GeForce RTX 3090
* main\_prop(str): the quantity property of the statement, such as computer performance
* qualifier\_prop(str): the property which acts as a qualifier, such as uses
* qualifier\_obj(str): the value of qualifier property, such as single-precision floating-point format.
* tag(str): An identifier used to name variables. Ultimately the variable with the property value of this quantity property is the tag.

#### add\_type\_constrain(self, type\_id, new\_var)

add a type constraint to a variable

**Parameters:**

* type\_id(str): ID of the type
* new\_var(str): the entity to be constrained

#### add\_filter(self, compare\_obj1, operator, compare\_obj2)

Given two comarison variables and an operator, add a filter.

**Parameters:**

* compare\_obj1(str): comparison variable 1
* operator(str): operator
* compare\_obj2(str): comparison variable 2

#### add\_bind(self, equation, var\_name)

add a bind expression to assign the result of the expression equation to the variable var\_name.

**Parameters:**

* equation(str): The expression to be binded. Bracket is not necessary. It will be added automatically.
* var\_name(str): The expression will be assigned to this variable.

#### add\_assignment(self,var\_list,new\_var)

add a values clause to generate a new variable new var of which the value includes all entities in var\_list.

**Parameters:**

* equation(list): an entities list, such as \['Q123', 'Q186']
* new\_var(str): the new variable

## Aggreggation

#### add\_max(self, max\_obj, return\_obj='\*',offset=0,limit=1)

Calculate the maximum value of max\_obj

**Parameters:**

* max\_obj(str): the variable of which the maximum value needs to be counted
* return\_obj(str): the variable to return. It can be \*
* offset(str): the number in offset. For example, to get the second biggest one, set offset=2
* limit(str): the number in limit. For example, to get the three biggest one, set limit=3, offset=0

#### add\_min(self, min\_obj, return\_obj='\*',offset=0,limit=1)

Calculate the minimum value of min\_obj

**Parameters:**

* max\_obj(str): the variable of which the minimum value needs to be counted
* return\_obj(str): the variable to return. It can be \*
* offset(str): the number in offset. For example, to get the second smallest one, set offset=2
* limit(str): the number in limit. For example, to get the three smallest one, set limit=3, offset=0

#### add\_avg(self,avg\_var, new\_var, group\_obj=None)

calculate the average value of variable avg\_var. The parameter new\_var is the variable of the calculated average value.

**Parameters:**

* avg\_var(str): the variable which needs to be averaged
* new\_var(str): the variable of the calculated average value
* group\_obj(str): the variable which needs to be put in a group by

#### add\_sum(self,sum\_var, new\_var, group\_obj=None)

calculate the sum of the variable sum\_var. The parameter new\_var is the variable of the calculated sum value.

#### Parameters:

* sum\_var(str): the variable which needs to be summed
* new\_var(str): the variable of the calculated sum value
* group\_obj(str): the variable which needs to be put in a group by

#### add\_count(self,count\_obj,new\_var, group\_obj=None)

calculate the average value of variable count\_obj. The parameter new\_var is the variable of the calculated average value. It can only be used in the final step of a complete query or subquery. After use, either the entire query ends or it is treated as a subquery.&#x20;

**Parameters:**

* count\_obj(str): the variable which needs to be counted
* new\_var(str): the variable of the calculated counting value
* group\_obj(str): the variable which needs to be put in a group by

#### add\_rank(self, rank\_var, var\_list,new\_var)

calculate the rank of rank\_var's value among var\_list

**Parameters:**

* rank\_var(str): the variable of which the rank needs to be calculated
* var\_list(str): the rank is calculated in this list which includes rank\_var
* new\_var(str): the variable of the rank result

## Boolean

#### add\_compare(self, obj1, op, obj2)

Determine whether obj1 and obj2 satisfies the size relationship represented by op. It can only be used in the final step of a complete query.

**Parameters:**

* obj1(str): comparison variable 1
* op(str): operator
* obj2(str): comparison variable 2

## Arithmetic

These functions are used alone. They do not belong to an PyQL instance.

#### add

aaa

#### sub

#### mul

#### div

#### abs



## Other

#### add\_sub\_query(self,\*sub\_query)

add a sub query

**Parameters:**

* sub\_query: the PyQL instance which needs to be added as a sub\_query



#### add\_time

#### add\_start\_time

#### add\_end\_time



