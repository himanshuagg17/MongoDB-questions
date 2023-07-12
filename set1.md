**Problem 3:**

- **Prerequisite**: Understand basic data fetching in SQL / MongoDB
- **Problem**: Write a query to fetch all data from the **`Customers`** table / collection.

select * from customers

db.customers.find();


**Problem 4:**

- **Prerequisite**: Understand how to select specific fields in SQL / MongoDB
- **Problem**: Write a query to select only the **`name`** and **`email`** fields for all customers.

SELECT name, email FROM customers;


db.customer.find({}, { name: 1, email: 1, _id: 0 });


**Problem 5:**

- **Prerequisite**: Understand basic WHERE clause in SQL / MongoDB's find method
- **Problem**: Write a query to fetch the customer with the **`id`** of 3.

SELECT * FROM customers WHERE id = 3;


db.customers.find({ _id: ObjectId("3") });

**Problem 6:**

- **Prerequisite**: Understand using string patterns in SQL (LIKE clause) / using regex in MongoDB
- **Problem**: Write a query to fetch all customers whose **`name`** starts with 'A'.

SELECT * FROM customers WHERE name LIKE 'A%';

db.customers.find({ name: /^A/ });

**Problem 7:**

- **Prerequisite**: Understand how to order data in SQL / MongoDB
- **Problem**: Write a query to fetch all customers, ordered by **`name`** in descending order.

SELECT * FROM customers ORDER BY name DESC;


db.customers.find().sort({ name: -1 });


**Problem 8:**

- **Prerequisite**: Understand data updating in SQL / MongoDB
- **Problem**: Write a query to update the **`address`** of the customer with **`id`** 4.

UPDATE customers SET address = 'New Address' WHERE id = 4;

db.customers.updateOne({ _id: ObjectId("4") }, { $set: { address: 'New Address' } });


**Problem 9:**

- **Prerequisite**: Understand how to limit results in SQL / MongoDB
- **Problem**: Write a query to fetch the top 3 customers when ordered by **`id`** in ascending order.

SELECT * FROM customers ORDER BY id ASC LIMIT 3;

db.customers.find().sort({ id: 1 }).limit(3);


**Problem 10:**

- **Prerequisite**: Understand data deletion in SQL / MongoDB
- **Problem**: Write a query to delete the customer with **`id`** 2.

DELETE FROM customers WHERE id = 2;

db.customers.deleteOne({ _id: ObjectId("2") });

**Problem 11:**

- **Prerequisite**: Understand how to count rows / documents in SQL / MongoDB
- **Problem**: Write a query to count the number of customers.

SELECT COUNT(*) FROM customers;

db.customers.count();

**Problem 12:**

- **Prerequisite**: Understand how to skip rows / documents in SQL / MongoDB
- **Problem**: Write a query to fetch all customers except the first two when ordered by **`id`** in ascending order.

SELECT * FROM customers ORDER BY id ASC OFFSET 2;

db.customers.find().sort({ id: 1 }).skip(2);

**Problem 13:**

- **Prerequisite**: Understand filtering with multiple conditions in SQL / MongoDB
- **Problem**: Write a query to fetch all customers whose **`id`** is greater than 2 and **`name`** starts with 'B'.

SELECT * FROM customers WHERE id > 2 AND name LIKE 'B%';

db.customers.find({ id: { $gt: 2 }, name: /^B/ });

**Problem 14:**

- **Prerequisite**: Understand how to use OR conditions in SQL / MongoDB
- **Problem**: Write a query to fetch all customers whose **`id`** is less than 3 or **`name`** ends with 's'.

SELECT * FROM customers WHERE id < 3 OR name LIKE '%s';

db.customers.find({ $or: [ { id: { $lt: 3 } }, { name: /s$/ } ] });

**Problem 15:**

- **Prerequisite**: Understand how to use NULL checks in SQL / MongoDB
- **Problem**: Write a query to fetch all customers where the **`phone_number`** field is not set or is null.

SELECT * FROM customers WHERE phone_number IS NULL;

db.customers.find({ phone_number: { $exists: false } });