# Exercise 1

# Exercise 2
- Can you explain the purpose of the orders_items table? 
The order_items table is a _junction table_. It contains the primary keys of other tables as foreign keys. In our case it allows to connect the orders and products tables. 

- Can you write a SQL query to find the average order cost per country?
```mysql
SELECT customers.country, avg(products.price*orderdetails.quantity) as meanordercost
FROM OrderDetails, Products, Orders, Customers
WHERE orderdetails.productid = products.productid
AND orders.orderid = orderdetails.orderid
AND orders.customerid = customers.customerid
GROUP BY customers.country;
```

- Can you write a SQL query to find the name of the highest price product sold to an Italian customer?
```mysql 
SELECT products.productname, products.price
FROM OrderDetails, Products, Orders, Customers
WHERE orderdetails.productid = products.productid
AND orders.orderid = orderdetails.orderid
AND orders.customerid = customers.customerid
AND customers.country = "Italy"
AND products.price = (
  SELECT max(products.price) as maxprice
  FROM OrderDetails, Products, Orders, Customers
  WHERE orderdetails.productid = products.productid
  AND orders.orderid = orderdetails.orderid
  AND orders.customerid = customers.customerid
  AND customers.country = "Italy"
);
```
- How could you optimize the orders table assuming you have to manage a lot of queries?
There are two ways to optimize the orders table:
  - Add an index on the orderdate column
  - Add an index on the customerid column
The first one would be useful if we have to run queries that filter by orderdate. The second one would be useful if we have to run queries that filter by customerid. 

- What would you change if the amount of data is too big to run these queries? (suppose to have 500TB of data)

Since that amount of data is too big to be stored in a single database, the solution would be to use a distributed database, such as Hadoop, Cassandra or MongoDB. 


# Exercise 3