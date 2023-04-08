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

The orders table could be optimized by adding an index on the id_customer column. This would allow to speed up the queries that filter by id_customer. 
Depending on the queries that are run on the table, it could also be useful to add an index on the orders.id column.

- What would you change if the amount of data is too big to run these queries? (suppose to have 500TB of data)

Since that amount of data is too big to be stored in a single database, the solution would be to use a distributed database, such as MongoDB. 
This would allow to distribute the data on multiple machines, and to run queries in parallel. 


# Exercise 3
This exercise is about data management and Machine Translation. You have to answer questions by commenting on your answers.
Suppose you work for a company that offers Machine Translation to its customers.
You have access to a large amount of translation data (i.e., parallel files containing a sentence in a source language and the corresponding translation in the target language). Some of this data comes from public datasets, others have been produced by professional translators working for your own company.
Questions:

> How would you design a data pipeline for a Machine Translation system? (e.g. necessary steps, main challenges, etc.)
We can divide the pipeline into the following steps:
1. **Data collection.** This phase will be addressed in the next question.
2. **Data preprocessing.**  The preprocessing phase includes cleaning the data (e.g. removing special characters, removing punctuation, etc.), tokenization, and normalization (e.g. lowercasing, etc.). This phase includes also the splitting of the data into training, validation, and test sets. The design of the test set is crucial, since it will be used to evaluate the performance of the system. For instance, we could have multiple test sets, each one containing sentences from a different language. This would allow to monitor the performance of the system on different languages. Another idea could be to have a test set containing sentences with specific characteristics (e.g. containing profanity, etc.). 
3. **Model training.** In this phase we train a neural model for Machine Translation. The model is trained on the training set, and the performance is monitored on the validation set. The training phase should integrate all the best practices for training neural models, such as early stopping, etc.
4. **Model evaluation.** The model is evaluated on the test set. The evaluation metrics are usually BLEU, METEOR, etc.

- What would you do to collect new and good quality data from the web? Assume you want to use them to train a neural model for Machine Translation.

- Suppose that low quality translations created by the system are post-edited by professional translators. How would you use this process to monitor the quality of the Machine Translation system?