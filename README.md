# Exercise 1

# Exercise 2
## 2.1 
The order_items table is a _junction table_. It contains the primary keys of other tables as foreign keys. In our case it allows to connect the orders and products tables. 

## 2.2
```{sql}
SELECT customers.country, avg(products.price*orderdetails.quantity) as meanordercost
FROM OrderDetails, Products, Orders, Customers
WHERE orderdetails.productid = products.productid
AND orders.orderid = orderdetails.orderid
AND orders.customerid = customers.customerid
GROUP BY customers.country;
```


# Exercise 3