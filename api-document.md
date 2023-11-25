# API Document

## Basic graph pattern

### add\_fact

> **add\_fact(self, s, p, o, p\_prefix)**

Add a \<s, p, o, prefix> triple to the query.

**Parameters:**

* s(str): the subject of the triple&#x20;
* p(str): the predicate of the triple
* o(str): the object of the triple
* p\_prefix(str): the prefix of predicate

**example:**

* Question:&#x20;

> What is the manufacturer of 2T Stalker

* Program:

```python
a=PyQL()
a.add_fact('Q222823', 'P176', 'x0' ,'wdt')
```

* SPARQL:

```sparql
SELECT DISTINCT * {
	wd:Q222823 wdt:P176 ?x0.
}
```

### add\_quantity

> **add\_quantity(entity, prop, tag,time=False)**

This function is used to acquire the value of a quantity property.

**Parameters:**

* entity(str): the entity
* prop(str): the quantity property you need to get
* tag(str):  An identifier used to name variables. Ultimately the variable with the property value of this quantity property is the tag.
* time(int/boolean/str): The constraint of time.
  * int: Limit the time to this year.
  * boolean: True means need to acquire the time，False means not need to acquire the time.
  * str: use the function to\_date to set the entire yyyy-mm-dd

**example:**

* Question:&#x20;

> What is the GDP of French economy in the year 2007

* Program:

```python
a=PyQL()
a.add_quantity('Q8057','P4010','x0',2007)
```

* SPARQL:

```sparql
SELECT DISTINCT ?x0 {
	wd:Q8057 p:P4010 ?statement_x0.
	?statement_x0 psv:P4010 ?value_st_x0.
	?value_st_x0 wikibase:quantityAmount ?x0.
	?statement_x0 pq:P585 ?time_x0.
	FILTER(YEAR(?time_x0) = 2007).
}
```



### add\_quantity\_by\_qualifier

> **add\_quantity\_by\_qualifier(self,entity,main\_prop,main\_obj,qualifier\_prop,tag)**

Acquire the value of a quantity property which acts as a qualifier.

For example, the entity iPhone 12 has a property 'made from material' of which the value can be steel, glass and so on. The property mass is the qualifier for each material.&#x20;

**Parameters:**

* entity(str):  the entity, such as iPhone12
* main\_prop(str): the property of the statement, such as made from material
* main\_obj(str): the value of the main\_prop, such as steel.
* qualifier\_prop(str): the quantity property which acts as a qualifier. For example, mass.
* tag(str): An identifier used to name variables. Ultimately the variable with the property value of this quantity property is the tag.

**example:**

* Question:&#x20;

> What is the duration of Soyuz MS-21's time on the moon

* Program:

```python
a=PyQL()
a.add_quantity_by_qualifier('Q100375849','P793','x1','P2047','x0')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x0 {
	wd:Q100375849 p:P793 ?statement_x0.
	?statement_x0 ps:P793 ?x1.
	?statement_x0 pqv:P2047 ?value_st_x0.
	?value_st_x0 wikibase:quantityAmount ?x0.
}
```



### add\_quantity\_with\_qualifier

> **add\_quantity\_with\_qualifier(self,entity,main\_prop,qualifier\_prop,qualifier\_obj,tag)**

Acquire the value of a quantity property with a qualifire limiting other peoperties&#x20;

For example, the computer performance of Nvidia GeForce RTX 3090 has 4 statements which are limited by the qualifier uses. Their qualifier values are single-precision floating-point format, half-precision floating-point format, double-precision floating-point format and half-precision floating-point format seperately.

**Parameters:**

* entity(str): the entity, such as Nvidia GeForce RTX 3090
* main\_prop(str): the quantity property of the statement, such as computer performance
* qualifier\_prop(str): the property which acts as a qualifier, such as uses
* qualifier\_obj(str): the value of qualifier property, such as single-precision floating-point format.
* tag(str): An identifier used to name variables. Ultimately the variable with the property value of this quantity property is the tag.

**example:**

* Question:&#x20;

> What is the solubility of Dihydrogen disulfide in water

* Program:

```python
a=PyQL()
a.add_quantity_with_qualifier('Q170591','P2177','P2178','Q283','x0')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x0 {
	wd:Q170591 p:P2177 ?statement_x0.
	?statement_x0 psv:P2177 ?value_st_x0.
	?value_st_x0 wikibase:quantityAmount ?x0.
	?statement_x0 pq:P2178 wd:Q283.
}
```



### add\_type\_constrain

> **add\_type\_constrain(self, type\_id, new\_var)**

add a type constraint to a variable

**Parameters:**

* type\_id(str): ID of the type
* new\_var(str): the entity to be constrained

**example:**

* Question: Get all the Pan Am Games.
* Program:

```python
a=PyQL()
a.add_type_constrain('Q230186','x0')
a.set_answer("x0")
```

* SPARQL:

```sparql
SELECT DISTINCT ?x0 {
	?x0 wdt:P31/wdt:P279* wd:Q230186.
}
```



### add\_filter

> **add\_filter(self, compare\_obj1, operator, compare\_obj2)**

Given two comarison variables and an operator, add a filter.

**Parameters:**

* compare\_obj1(str): comparison variable 1
* operator(str): operator
* compare\_obj2(str): comparison variable 2

**example:**

* Question:

> Get all the Pan Am Games held after 2001 (not including 2001).

* Program:

```python
a=PyQL()
a.add_type_constrain('Q230186', 'x0')
a.add_time('x0', 'x1')
a.add_filter(year('x1'), '>', 2001)
a.set_answer("x0")
```

* SPARQL:

```sparql
SELECT DISTINCT ?x0 {
	?x0 wdt:P31/wdt:P279* wd:Q230186.
	
	?x0 wdt:P585 ?x1.
	
	FILTER(YEAR(?x1) > 2001).
}
```



### add\_bind

> **add\_bind(self, equation, var\_name)**

add a bind expression to assign the result of the expression equation to the variable var\_name.

**Parameters:**

* equation(str): The expression to be binded. Bracket is not necessary. It will be added automatically.
* var\_name(str): The expression will be assigned to this variable.

**example:**

* Question:

> What is the number of deaths and injuried individuals in Atlanta spa shootings?

* Program:

```python
a=PyQL()
a.add_quantity('Q105982031','P1120','x0')
a.add_quantity('Q105982031','P1339','x1')
a.add_bind(add('x0', 'x1'), 'x2')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x2 {
	wd:Q105982031 p:P1120 ?statement_x0.
	?statement_x0 psv:P1120 ?value_st_x0.
	?value_st_x0 wikibase:quantityAmount ?x0.
	
	wd:Q105982031 p:P1339 ?statement_x1.
	?statement_x1 psv:P1339 ?value_st_x1.
	?value_st_x1 wikibase:quantityAmount ?x1.
	
	BIND( ((?x0 + ?x1)) AS ?x2 )
}
```



### add\_assignment

> **add\_assignment(self,var\_list,new\_var)**

add a values clause to generate a new variable new var of which the value includes all entities in var\_list.

**Parameters:**

* equation(list): an entities list, such as \['Q123', 'Q186']
* new\_var(str): the new variable

**example:**

* Question:

> Among BMW N57 and Renault E-Type engine, who has the highest compression ratio?

* Program:

```python
a=PyQL()
a.add_assignment(['Q796629','Q3866356'],'x2')
a.add_quantity('x2','P1247','x0')
a.add_max('x0','x2')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x2 {
	Values ?x2 {wd:Q796629 wd:Q3866356}
	?x2 p:P1247 ?statement_x0.
	?statement_x0 psv:P1247 ?value_st_x0.
	?value_st_x0 wikibase:quantityAmount ?x0.
}
ORDER BY DESC(?x0)
LIMIT 1
```



### add\_sub\_query

> **add\_sub\_query(self,\*sub\_query)**

add a sub query to the sparql of this PyQL instance

**Parameters:**

* sub\_query(PyQL): the PyQL instance which needs to be added as a sub\_query

**example:**

* Question:&#x20;

> By how much is the average lowest air pressure of a category 5 hurricane lower than that of a category 3 hurricane?

* Program:

```python
a = PyQL()
a.add_quantity('x4', 'P2532', 'x2')
a.add_fact('x4', 'P31', 'Q63100611', 'wdt')
a.add_avg('x2', 'x1')
b = PyQL()
b.add_quantity('x5', 'P2532', 'x3')
b.add_fact('x5', 'P31', 'Q63100595', 'wdt')
b.add_avg('x3', 'x0')
c = PyQL()
c.add_bind(sub('x0', 'x1'), 'x6')
c.add_sub_query(a, b)
```

* SPARQL:

```sparql
SELECT DISTINCT ?x6 {
	{
		SELECT (AVG(?x2) AS ?x1 )  {
			?x4 p:P2532 ?statement_x2.
			?statement_x2 psv:P2532 ?value_st_x2.
			?value_st_x2 wikibase:quantityAmount ?x2.
			
			?x4 wdt:P31 wd:Q63100611.
		}
		
	}
	{
		SELECT (AVG(?x3) AS ?x0 )  {
			?x5 p:P2532 ?statement_x3.
			?statement_x3 psv:P2532 ?value_st_x3.
			?value_st_x3 wikibase:quantityAmount ?x3.
			
			?x5 wdt:P31 wd:Q63100595.
		}
		
	}
	BIND( ((?x0 - ?x1)) AS ?x6 )
}
```

## Aggreggation

### add\_max

> **add\_max(self, max\_obj, return\_obj='\*',offset=0,limit=1)**

Calculate the maximum value of max\_obj

**Parameters:**

* max\_obj(str): the variable of which the maximum value needs to be counted
* return\_obj(str): the variable to return. It can be \*
* offset(str): the number in offset. For example, to get the second biggest one, set offset=2
* limit(str): the number in limit. For example, to get the three biggest one, set limit=3, offset=0

**example:**

* Question:

> Among BMW N57 and Renault E-Type engine, who has the highest compression ratio?

* Program:

```python
a=PyQL()
a.add_assignment(['Q796629','Q3866356'],'x2')
a.add_quantity('x2','P1247','x0')
a.add_max('x0','x2')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x2 {
	Values ?x2 {wd:Q796629 wd:Q3866356}
	?x2 p:P1247 ?statement_x0.
	?statement_x0 psv:P1247 ?value_st_x0.
	?value_st_x0 wikibase:quantityAmount ?x0.
}
ORDER BY DESC(?x0)
LIMIT 1
```

### add\_min

> **add\_min(self, min\_obj, return\_obj='\*',offset=0,limit=1)**

Calculate the minimum value of min\_obj

**Parameters:**

* max\_obj(str): the variable of which the minimum value needs to be counted
* return\_obj(str): the variable to return. It can be \*
* offset(str): the number in offset. For example, to get the second smallest one, set offset=2
* limit(str): the number in limit. For example, to get the three smallest one, set limit=3, offset=0

**example:**

* Question: Among BMW N57 and Renault E-Type engine, who has the lowest compression ratio?
* Program:

```python
a=PyQL()
a.add_assignment(['Q796629','Q3866356'],'x2')
a.add_quantity('x2','P1247','x0')
a.add_min('x0','x2')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x2 {
	Values ?x2 {wd:Q796629 wd:Q3866356}
	?x2 p:P1247 ?statement_x0.
	?statement_x0 psv:P1247 ?value_st_x0.
	?value_st_x0 wikibase:quantityAmount ?x0.
}
ORDER BY (?x0)
LIMIT 1
```

### add\_avg

> **add\_avg(self,avg\_var, new\_var, group\_obj=None)**

calculate the average value of variable avg\_var. The parameter new\_var is the variable of the calculated average value.

**Parameters:**

* avg\_var(str): the variable which needs to be averaged
* new\_var(str): the variable of the calculated average value
* group\_obj(str): the variable which needs to be put in a group by

**example:**

* Question:

> Among BMW N57 and Renault E-Type engine, what is the average compression ratio?

* Program:

```python
a=PyQL()
a.add_assignment(['Q796629','Q3866356'],'x2')
a.add_quantity('x2','P1247','x0')
a.add_avg('x0','x1')
```

* SPARQL:

```sparql
SELECT (AVG(?x0) AS ?x1 )  {
	Values ?x2 {wd:Q796629 wd:Q3866356}
	?x2 p:P1247 ?statement_x0.
	?statement_x0 psv:P1247 ?value_st_x0.
	?value_st_x0 wikibase:quantityAmount ?x0.
}
```



### add\_sum

> **add\_sum(self,sum\_var, new\_var, group\_obj=None)**

calculate the sum of the variable sum\_var. The parameter new\_var is the variable of the calculated sum value.

#### Parameters:

* sum\_var(str): the variable which needs to be summed
* new\_var(str): the variable of the calculated sum value
* group\_obj(str): the variable which needs to be put in a group by

**example:**

* Question:

> What are the female population of all the communes of France in 2017?

* Program:

```python
a=PyQL()
a.add_type_constrain('Q484170','x0')
a.add_quantity('x0','P1539','x1',2017)
a.add_sum("x1","x2")
```

* SPARQL:

```sparql
SELECT (SUM(?x1) AS ?x2 )  {
	?x0 wdt:P31/wdt:P279* wd:Q484170.
	
	?x0 p:P1539 ?statement_x1.
	?statement_x1 psv:P1539 ?value_st_x1.
	?value_st_x1 wikibase:quantityAmount ?x1.
	?statement_x1 pq:P585 ?time_x1.
	FILTER(YEAR(?time_x1) = 2017).
	
}
```



### add\_count

> **add\_count(self,count\_obj,new\_var, group\_obj=None)**

calculate the average value of variable count\_obj. The parameter new\_var is the variable of the calculated average value. It can only be used in the final step of a complete query or subquery. After use, either the entire query ends or it is treated as a subquery.&#x20;

**Parameters:**

* count\_obj(str): the variable which needs to be counted
* new\_var(str): the variable of the calculated counting value
* group\_obj(str): the variable which needs to be put in a group by

**example:**

* Question:

> What is the number of all the communes of France?

* Program:

```python
a=PyQL()
a.add_type_constrain('Q484170','x0')
a.add_count("x0","x1")
```

* SPARQL:

```sparql
SELECT (COUNT(DISTINCT ?x0) AS ?x1)  {
	?x0 wdt:P31/wdt:P279* wd:Q484170.
}
```

### add\_rank

> **add\_rank(self, rank\_var, var\_list,new\_var)**

calculate the rank of rank\_var's value among var\_list

**Parameters:**

* rank\_var(str): the variable of which the rank needs to be calculated
* var\_list(str): the rank is calculated in this list which includes rank\_var
* new\_var(str): the variable of the rank result

**example:**

* Question:

> What is Italy's population ranking among all the sovereign states in 2020?

* Program:

```python
a=PyQL()
a.add_type_constrain('Q3624078','x0')
a.add_quantity("x0", "P1082", "x1", 2020)
a.add_quantity("Q38","P1082","x2",2020)
a.add_rank("x2","x1","x3")
```

* SPARQL:

```sparql
SELECT (COUNT(DISTINCT ?x1) +1 AS ?x3) {
	?x0 wdt:P31/wdt:P279* wd:Q3624078.
	
	?x0 p:P1082 ?statement_x1.
	?statement_x1 psv:P1082 ?value_st_x1.
	?value_st_x1 wikibase:quantityAmount ?x1.
	?statement_x1 pq:P585 ?time_x1.
	FILTER(YEAR(?time_x1) = 2020).
	
	
	wd:Q38 p:P1082 ?statement_x2.
	?statement_x2 psv:P1082 ?value_st_x2.
	?value_st_x2 wikibase:quantityAmount ?x2.
	?statement_x2 pq:P585 ?time_x2.
	FILTER(YEAR(?time_x2) = 2020).
	
	
	FILTER(?x2 < ?x1).
}
```

## Boolean

### add\_compare

> **add\_compare(self, obj1, op, obj2)**

Determine whether obj1 and obj2 satisfies the size relationship represented by op. It can only be used in the final step of a complete query.

**Parameters:**

* obj1(str): comparison variable 1
* op(str): operator
* obj2(str): comparison variable 2

**example:**

* Question:

> Is Italy's population more than France's population in 2020?

* Program:

```python
a = PyQL()
a.add_quantity("Q142", "P1082", "x1", 2020)
a.add_quantity("Q38", "P1082", "x2", 2020)
a.add_compare("x2",">","x1")
```

* SPARQL:

```sparql
SELECT ?answer {
	wd:Q142 p:P1082 ?statement_x1.
	?statement_x1 psv:P1082 ?value_st_x1.
	?value_st_x1 wikibase:quantityAmount ?x1.
	?statement_x1 pq:P585 ?time_x1.
	FILTER(YEAR(?time_x1) = 2020).
	
	
	wd:Q38 p:P1082 ?statement_x2.
	?statement_x2 psv:P1082 ?value_st_x2.
	?value_st_x2 wikibase:quantityAmount ?x2.
	?statement_x2 pq:P585 ?time_x2.
	FILTER(YEAR(?time_x2) = 2020).
	
	
	BIND( (IF(?x2 > ?x1, "TRUE", "FALSE")) AS ?answer )
}
```

## Other

### add\_time

> **add\_time(self, entity, new\_var)**

Get the point of time property of entity. It adds a triple \<entity, wdt:P585, new\_var>

**Parameters:**

* entity(str): The entity whose point of time is needed.
* new\_var(str): The variable which represents the point of time value of the entity

**example:**

* Question:

> Get all the Pan Am Games held after 2001 (not including 2001).

* Program:

```python
a=PyQL()
a.add_type_constrain('Q230186', 'x0')
a.add_time('x0', 'x1')
a.add_filter(year('x1'), '>', 2001)
a.set_answer("x0")
```

* SPARQL:

```sparql
SELECT DISTINCT ?x0 {
	?x0 wdt:P31/wdt:P279* wd:Q230186.
	
	?x0 wdt:P585 ?x1.
	
	FILTER(YEAR(?x1) > 2001).
}
```

### add\_start\_time

> **add\_start\_time(self, entity,new\_var)**

Get the start time property of entity. It adds a triple \<entity, wdt:P580, new\_var>

**Parameters:**

* entity(str): The entity whose start time is needed.
* new\_var(str): The variable which represents the start time value of the entity

**example:**

* Question:

> What is the start time of Efficacy and Safety Study of Mongersen (GED-0301) for the Treatment of Subjects With Active Crohn's Disease?

* Program:

```python
a = PyQL()
a.add_start_time('Q64216670', 'x0')
a.set_answer('x0')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x0 {
	wd:Q64216670 wdt:P580 ?x0.
}
```

### add\_end\_time

> **add\_end\_time(self, entity, new\_var)**

Get the end time property of entity. It adds a triple \<entity, wdt:P582, new\_var>

**Parameters:**

* entity(str): The entity whose end time is needed.
* new\_var(str): The variable which represents the end time value of the entity

**example:**

* Question:

> What is the start time of Efficacy and Safety Study of Mongersen (GED-0301) for the Treatment of Subjects With Active Crohn's Disease?

* Program:

```python
a = PyQL()
a.add_end_time('Q64216670', 'x0')
a.set_answer('x0')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x0 {
	wd:Q64216670 wdt:P582 ?x0.
}
```

### set\_answer

> **set\_answer(self,answer='\*')**

Set the return value of the final query. It can only be used at last.

**Parameters:**

* answer(str): The return value of this query.

**example:**

* Question:

> What is the start time of Efficacy and Safety Study of Mongersen (GED-0301) for the Treatment of Subjects With Active Crohn's Disease?

* Program:

```python
a = PyQL()
a.add_end_time('Q64216670', 'x0')
a.set_answer('x0')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x0 {
	wd:Q64216670 wdt:P582 ?x0.
}
```

## Arithmetic

These functions are used inside an add\_bind. They are not member functions of PyQL class.

### add

> add(\*para\_list)

It creates an addition expression which adds up every element in para\_list.

**example:**

* Question:

> What is the number of deaths and injuried individuals in Atlanta spa shootings?

* Program:

```python
a=PyQL()
a.add_quantity('Q105982031','P1120','x0')
a.add_quantity('Q105982031','P1339','x1')
a.add_bind(add('x0', 'x1'), 'x2')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x2 {
	wd:Q105982031 p:P1120 ?statement_x0.
	?statement_x0 psv:P1120 ?value_st_x0.
	?value_st_x0 wikibase:quantityAmount ?x0.
	
	wd:Q105982031 p:P1339 ?statement_x1.
	?statement_x1 psv:P1339 ?value_st_x1.
	?value_st_x1 wikibase:quantityAmount ?x1.
	
	BIND( ((?x0 + ?x1)) AS ?x2 )
}
```

### sub

> **sub(para1, para2)**

It creates an subtraction expression of para1 subtacting para2.

**example:**

* Question:

> How much more is France's population than Italy's population in 2020?

* Program:

```python
a = PyQL()
a.add_quantity("Q142", "P1082", "x1", 2020)
a.add_quantity("Q38", "P1082", "x2", 2020)
a.add_bind(sub('x1','x2'),'x3')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x3 {
	wd:Q142 p:P1082 ?statement_x1.
	?statement_x1 psv:P1082 ?value_st_x1.
	?value_st_x1 wikibase:quantityAmount ?x1.
	?statement_x1 pq:P585 ?time_x1.
	FILTER(YEAR(?time_x1) = 2020).
	
	
	wd:Q38 p:P1082 ?statement_x2.
	?statement_x2 psv:P1082 ?value_st_x2.
	?value_st_x2 wikibase:quantityAmount ?x2.
	?statement_x2 pq:P585 ?time_x2.
	FILTER(YEAR(?time_x2) = 2020).
	
	
	BIND( ((?x1 - ?x2)) AS ?x3 )
}
```

### mul

> **mul(\*para\_list)**

It creates an multiplication expression which multiplies every element in para\_list.

**example:**

* Question:

> What is a half of the population of France in 2020?

* Program:

```python
a = PyQL()
a.add_quantity("Q142", "P1082", "x1", 2020)
a.add_bind(mul('x1', 0.5), 'x2')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x2 {
	wd:Q142 p:P1082 ?statement_x1.
	?statement_x1 psv:P1082 ?value_st_x1.
	?value_st_x1 wikibase:quantityAmount ?x1.
	?statement_x1 pq:P585 ?time_x1.
	FILTER(YEAR(?time_x1) = 2020).
	
	
	BIND( (?x1 * 0.5) AS ?x2 )
}
```

### div

> **div(para1, para2)**

It creates an division expression of para1 dividing para2.

**example:**

* Question:

> How many times the population of France is that of Italy in 2020?

* Program:

```python
a = PyQL()
a.add_quantity("Q142", "P1082", "x1", 2020)
a.add_quantity("Q38", "P1082", "x2", 2020)
a.add_bind(div('x1','x2'),'x3')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x3 {
	wd:Q142 p:P1082 ?statement_x1.
	?statement_x1 psv:P1082 ?value_st_x1.
	?value_st_x1 wikibase:quantityAmount ?x1.
	?statement_x1 pq:P585 ?time_x1.
	FILTER(YEAR(?time_x1) = 2020).
	
	
	wd:Q38 p:P1082 ?statement_x2.
	?statement_x2 psv:P1082 ?value_st_x2.
	?value_st_x2 wikibase:quantityAmount ?x2.
	?statement_x2 pq:P585 ?time_x2.
	FILTER(YEAR(?time_x2) = 2020).
	
	
	BIND( (?x1 / ?x2) AS ?x3 )
}
```

### abs

> **abs(para)**

It creates an expression which is abs(para)

**example:**

* Question:

> What is the difference between Italy's population and France's population in 2020?

* Program:

```python
a = PyQL()
a.add_quantity("Q142", "P1082", "x1", 2020)
a.add_quantity("Q38", "P1082", "x2", 2020)
a.add_bind(sub('x2', 'x1'), 'x3')
a.add_bind(abs('x3'),'x4')
```

* SPARQL:

```sparql
SELECT DISTINCT ?x4 {
	wd:Q142 p:P1082 ?statement_x1.
	?statement_x1 psv:P1082 ?value_st_x1.
	?value_st_x1 wikibase:quantityAmount ?x1.
	?statement_x1 pq:P585 ?time_x1.
	FILTER(YEAR(?time_x1) = 2020).
	
	
	wd:Q38 p:P1082 ?statement_x2.
	?statement_x2 psv:P1082 ?value_st_x2.
	?value_st_x2 wikibase:quantityAmount ?x2.
	?statement_x2 pq:P585 ?time_x2.
	FILTER(YEAR(?time_x2) = 2020).
	
	
	BIND( ((?x2 - ?x1)) AS ?x3 )
	
	BIND( (ABS(?x3)) AS ?x4 )
}
```

