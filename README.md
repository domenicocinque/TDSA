# Exercise 1
The first exercise is in the file `notebook.ipynb`.

# Exercise 2
> Can you explain the purpose of the orders_items table? 

The order_items table is a _junction table_. It contains the primary keys of other tables as foreign keys. In our case it allows to connect the orders and products tables.

> Can you write a SQL query to find the average order cost per country?

```mysql
SELECT customers.country, AVG(orders.order_cost) AS avg_order_cost
FROM customers
LEFT JOIN orders ON customers.id = orders.id_customer;
```

> Can you write a SQL query to find the name of the highest price product sold to an Italian customer?

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

> How could you optimize the orders table assuming you have to manage a lot of queries?

The orders table could be optimized by adding an index on the id_customer column. This would allow to speed up the queries that filter by id_customer. 
Depending on the queries that are run on the table, it could also be useful to add an index on the orders.id column.

> What would you change if the amount of data is too big to run these queries? (suppose to have 500TB of data)

Since that amount of data is too big to be stored in a single database, the solution would be to use a distributed database, such as MongoDB. 
This would allow to distribute the data on multiple machines, and to run queries in parallel. 


# Exercise 3

> How would you design a data pipeline for a Machine Translation system? (e.g. necessary steps, main challenges, etc.)

We can divide the pipeline into the following steps:
1. **Data collection.** Will be addressed in the next question.
2. **Data preprocessing.** The first part of the preprocessing phase is data cleaning. It involves removing duplicates, removing sentences that are too long or too short, removing sentences that are not encoded properly (e.g. UTF-8, etc.), etc. Then we have normalization and tokenization. The normalization phase includes steps such as lowercasing and standardization of punctuation. Finally, the tokenization phase splits the sentences into tokens and pads/truncates them to the size required by the model. Bear in mind that all the preprocessing steps are language-dependent. 
3. **Data splitting.** This phase includes splitting the data into training, validation, and test sets. We can have multiple test sets containing sentences from different languages to monitor the language-specific performance and detect catastrophic forgetting. Another idea could be to have a test set containing sentences with specific characteristics that can raise problems in the deployment phase (e.g. containing profanities, etc.). After this phase, the data is ready to be fed to the model. 


Some of the main challenges may include:
- **Data quality.** The quality of the data is crucial for the system's performance. In particular, this is crucial for the data coming from the web, since they may contain errors. 
- **Data diversity.** Since we offer a multilingual translation service, we need to be sure we have enough data for all the languages we support. This can be difficult since some languages are underrepresented on the web. 

> What would you do to collect new and good-quality data from the web? Assume you want to use them to train a neural model for Machine Translation.

The problem of collecting new data from the web boils down to the identification of potential sources. Apart from the well-known continuously updated collections such as [OPUS](https://opus.nlpl.eu) or [WMT](http://www.statmt.org/wmt20/translation-task.html), we need to identify websites with multilingual content, such as institutional websites, books translated in multiple languages, Wikipedia, etc. We can also consider using monolingual datasets using techniques such as back-translation. In this phase, we need special attention to the terms of service of each source. All of this can be achieved using standard web scraping techniques.

> Suppose that low-quality translations created by the system are post-edited by professional translators. How would you use this process to monitor the quality of the Machine Translation system?

To monitor the quality of the Machine Translation system using post-edited translations by professional translators, we need first to establish quality metrics. We can pair the classic BLEU or METEOR scores with some human evaluation scores such as adequacy and fluency. This could allow us to see through the limitations of algorithmic evaluation methods. Then, the post-edited translation could be included in the dataset in a data-augmentation fashion for the following retraining of the model. 

The information gathered from this process should be used to provide feedback to the MT development team: this feedback will be useful to identify areas for improvement and weaknesses of the model.

As a final thought, the availability of human translators could be exploited by introducing Reinforcement Learning from Human Feedback (RLHF), since it has been shown to be effective in training LLMs. 

