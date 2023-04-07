# Exercise 1

# Exercise 2
- Can you explain the purpose of the orders_items table? 
The order_items table is a _junction table_. It contains the primary keys of other tables as foreign keys. In our case it allows to connect the orders and products tables. 

- Can you write a SQL query to find the average order cost per country?
```mysql
SELECT customers.country, AVG(orders.order_cost) AS avg_order_cost
FROM customers
LEFT JOIN orders ON customers.id = orders.id_customer;
```

- Can you write a SQL query to find the name of the highest price product sold to an Italian customer?
```mysql 
SELECT TOP 1 products.name 
FROM (
    (
        orders_items
        INNER JOIN products 
        ON orders_items.id_product = products.id
    )
    INNER JOIN orders
    ON orders_items.id_order = orders.id
)
INNER JOIN customers
ON orders.id_customer = customers.id
WHERE customers.country = "Italy"
ORDER BY products.price DESC;
```

- How could you optimize the orders table assuming you have to manage a lot of queries?
There are two ways to optimize the orders table:
  - Add an index on the orderdate column
  - Add an index on the customerid column
The first one would be useful if we have to run queries that filter by orderdate. The second one would be useful if we have to run queries that filter by customerid. 

- What would you change if the amount of data is too big to run these queries? (suppose to have 500TB of data)

Since that amount of data is too big to be stored in a single database, the solution would be to use a distributed database, such as MongoDB. 


# Exercise 3
This exercise is about data management and Machine Translation. You have to answer questions by commenting on your answers.
Suppose you work for a company that offers Machine Translation to its customers.
You have access to a large amount of translation data (i.e., parallel files containing a sentence in a source language and the corresponding translation in the target language). Some of this data comes from public datasets, others have been produced by professional translators working for your own company.
Questions:
- How would you design a data pipeline for a Machine Translation system? (e.g. necessary steps, main challenges, etc.)
A possible data pipeline design could be

- What would you do to collect new and good quality data from the web? Assume you want to use them to train a neural model for Machine Translation.
- Suppose that low quality translations created by the system are post-edited by professional translators. How would you use this process to monitor the quality of the Machine Translation system?