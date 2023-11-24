# Introduction of PyQL

PyQL (Pythonic Query Language for SPARQL), a logical form written in Python as a reasoning step representation for numerical reasoning question(NRQ).

PyQL encapsulates various SPARQL syntax elements, such as Basic Graph Patterns, Assignments, Filters, Aggregations, and Subqueries.

A PyQL is a sequence of commands: $$\{c_{1},c_{2}, ..., c_{n}\}$$, where $$c_{i}$$ either initializes a PyQL object or calls a function on the object.&#x20;

> a=PyQL() # initializes a PyQL object&#x20;
>
> a.add\_type\_constrain(xxxxxx) ＃call a function to add type constrain

When write a PyQL, you need to first initialize a PyQL object and sequentially add functions to construct the whole query.

Each function represents a reasoning step such as stating the relation between two entities or computing the average.

A valid PyQL can directly generate an executable SPARQL query.

<figure><img src=".gitbook/assets/微信图片_20231124174307.png" alt=""><figcaption></figcaption></figure>
