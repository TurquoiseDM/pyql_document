---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# API Document

## Acquire quantity properties

### add\_quantity(entity, prop, tag,time=False, unit=False)

This function is used to cquire the value of a quantity property.

#### Parameters:

* entity(str): the entity
* prop(str): the quantity property you need to get
* tag(str):  An identifier used to name variables. Ultimately the variable with the property value of this quantity property is the tag.
* time(int/boolean/str): The constraint of time.
  * int: Limit the time to this year.
  * boolean: True means need to acquire the time，False means not need to acquire the time.
  * str: use the function to\_date to set the entire yyyy-mm-dd
* unit(boolean): Whether to convert the unit. True for yes. False for no. The converted unit is si\_value\_tag and si\_unit\_tag.

### add\_quantity\_by\_qualifier(self,entity,main\_prop,main\_obj,qualifier\_prop,tag,unit=False)

Acquire the value of a quantity property which acts as a qualifier.

For example, the entity iPhone 12 has a property 'made from material' of which the value can be steel, glass and so on. The property mass is the qualifier for each material.&#x20;

#### Parameters:

* entity(str):  the entity, such as iPhone12
* main\_prop(str): the property of the statement, such as made from material
* main\_obj(str): the value of the main\_prop, such as steel.
* qualifier\_prop(str): the quantity property which acts as a qualifier. For example, mass.
* tag(str): An identifier used to name variables. Ultimately the variable with the property value of this quantity property is the tag.
* unit(boolean): Whether to convert the unit. True for yes. False for no. The converted unit is si\_value\_tag and si\_unit\_tag.

### add\_quantity\_with\_qualifier(self,entity,main\_prop,qualifier\_prop,qualifier\_obj,tag,unit=False)

获取数值属性值，但是有qualifier约束其他属性

例如3090的性能，有三个数值记录，分别被（应用场景：单精度），（应用场景：半精度），（应用场景：双精度），修饰。

#### Parameters:

* entity(str): 实体 例如3090
* main\_prop(str): statement的属性,主属性,这里是数值属性，例如性能
* qualifier\_prop(str): qualifier属性，例如应用场景 applies to part
* qualifier\_obj(str): qualifier属性的取值，例如单精度，半精度，双精度
* tag(str): 标识符，用于给变量命名，最终这个数值属性的属性值的变量就是tag
* unit(boolean): 单位转换。True表示要转换，False表示不转换。转换后的变量是si\_value\_tag和si\_unit\_tag

### add\_quantity\_by\_qualifier\_answer(self, entity, main\_prop, main\_obj, qualifier\_prop, tag, unit=False)

answer版本的add\_quantity\_by\_qualifier，当数值属性的取值要作为答案的时候，指定他为答案，作为最后一条命令使用

### add\_quantity\_with\_qualifier\_answer(self, entity, main\_prop, qualifier\_prop, qualifier\_obj, tag, unit=False)

answer版本的add\_quantity\_with\_qualifier，当数值属性的取值要作为答案的时候，指定他为答案，作为最后一条命令使用

### add\_quantity\_answer(self,entity, prop, tag,time=False, unit=False)

answer版本的add\_quantity，当数值属性的取值要作为答案的时候，指定他为答案，作为最后一条命令使用

### add\_avg(self,avg\_var, new\_var, group\_obj=None)

计算avg\_var变量的平均值 ,new\_var是最终avg代表的变量的名字

#### Parameters:

* avg\_var(str): 需要做平均的变量
* new\_var(str): 平均结果的变量
* group\_obj(str): 需要做group by的变量

### add\_sum(self,sum\_var, new\_var, group\_obj=None)

计算sum\_var变量的和 ,new\_var是最终sum代表的变量的名字

#### Parameters:

* sum\_var(str): 需要做平均的变量
* new\_var(str): 平均结果的变量
* group\_obj(str): 需要做group by的变量

### add\_count(self,count\_obj,new\_var, group\_obj=None)

计算count\_obj的数量，只能在完整查询或者子查询最后一步使用，使用后要么整个查询结束，要么作为一个子查询。

#### Parameters:

* count\_obj(str): 需要做计数的变量
* new\_var(str): 计数结果的变量
* group\_obj(str): 需要做group by的变量

### add\_rank(self, rank\_var, var\_list,new\_var)

计算rank\_var的数值在var\_list中的排名

#### Parameters:

* rank\_var(str): 需要计算排名的变量
* var\_list(str): 在这个变量列表中计算排名，包含rank\_var
* new\_var(str): 排名结果的变量

### add\_max(self, max\_obj, return\_obj='\*',offset=0,limit=1)

计算max\_obj的最大值

#### Parameters:

* max\_obj(str): 需要计算最大值的变量
* return\_obj(str): 需要返回的变量，可以是\*
* offset(str): 排名第几，例如第二大的，offset=2
* limit(str):从最大的第几个开始返回，例如前三大的，limit=3,offset=0

### add\_min(self, min\_obj, return\_obj='\*',offset=0,limit=1)

计算min\_obj的最小值

#### Parameters:

* max\_obj(str): 需要计算最小值的变量
* return\_obj(str): 需要返回的变量，可以是\*
* offset(str): 排名第几，例如第二小的，offset=2
* limit(str):从最小的第几个开始返回，例如前三小的，limit=3,offset=0

### add\_compare(self, obj1, op, obj2)

obj1和obj2是否满足op表示的大小关系，只能用在最外层查询的最后一步

#### Parameters:

* obj1(str): 比较对象1
* op(str): 运算符
* obj2(str): 比较对象2

### add\_sub\_query(self,\*sub\_query)

添加子查询

#### Parameters:

* sub\_query: 需要添加为子问题的PyQL对象

### \_\_set\_answer(self,answer)

指定SPARQL最终返回的变量

#### Parameters:

* answer(str): 需要返回的变量

### add\_end\_time(self, entity, new\_var)





