# Introduction of PyQL

PyQL (Pythonic Query Language for SPARQL), a logical form written in Python as a reasoning step representation for numerical reasoning question(NRQ).

PyQL encapsulates various SPARQL syntax elements, such as Basic Graph Patterns, Assignments, Filters, Aggregations, and Subqueries.

A PyQL is a sequence of commands: $$\{c_{1},c_{2}, ..., c_{n}\}$$, where $$c_{i}$$ either initializes a PyQL object or calls a function on the object.&#x20;

```python
# initializes a PyQL object
a=PyQL() 
# call a function to add type constrain 
a.add_type_constrain('Q7325635', 'x1')
# get the value of a quantity property of x1. This value will be saved in x2.
a.add_quantity('x1','P4176','x2')
# filter the value of x2
a.add_filter('x2','>',6000)
# get the value of a quantity property of x1. This value will be saved in x3.
a.add_quantity('x1','P2052','x3')
# get the average value among all the values of variable x3. The average value will be saved in x4.
a.add_max('x3','x4')
```

When write a PyQL, you need to first initialize a PyQL object and sequentially add functions to construct the whole query.

Each function represents a reasoning step such as stating the relation between two entities or computing the average.

A valid PyQL can directly generate an executable SPARQL query.

<figure><img src=".gitbook/assets/微信图片_20231124174307.png" alt=""><figcaption></figcaption></figure>
