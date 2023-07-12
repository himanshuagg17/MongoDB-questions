**Problem 18:**

- **Prerequisite**: Understand how to order data in SQL / MongoDB
- **Problem**: Write a query to fetch all restaurants, ordered by **`average_rating`** in descending order.

SELECT *
FROM Restaurants
ORDER BY average_rating DESC;


db.Restaurants.find().sort({ average_rating: -1 });


**Problem 19:**

- **Prerequisite**: Understand filtering with multiple conditions in SQL / MongoDB
- **Problem**: Write a query to fetch all restaurants that offer **`delivery_available`** and have an **`average_rating`** of more than 4.

SELECT *
FROM Restaurants
WHERE delivery_available = true AND average_rating > 4;


db.Restaurants.find({
  delivery_available: true,
  average_rating: { $gt: 4 }
});


**Problem 20:**

- **Prerequisite**: Understand how to use NULL checks in SQL / MongoDB
- **Problem**: Write a query to fetch all restaurants where the **`cuisine_type`** field is not set or is null.

SELECT *
FROM Restaurants
WHERE cuisine_type IS NULL OR cuisine_type = '';


db.Restaurants.find({ $or: [{ cuisine_type: null }, { cuisine_type: '' }] });


**Problem 21:**

- **Prerequisite**: Understand how to count rows / documents in SQL / MongoDB
- **Problem**: Write a query to count the number of restaurants that have **`delivery_available`**.

SELECT COUNT(*)
FROM Restaurants
WHERE delivery_available = true;


db.Restaurants.count({ delivery_available: true });




**Problem 22:**

- **Prerequisite**: Understand using string patterns in SQL (LIKE clause) / using regex in MongoDB
- **Problem**: Write a query to fetch all restaurants whose **`location`** contains 'New York'.

SELECT *
FROM Restaurants
WHERE location LIKE '%New York%';


db.Restaurants.find({ location: { $regex: 'New York', $options: 'i' } });



**Problem 23:**

- **Prerequisite**: Understand how to use the AVG function in SQL / MongoDB's aggregate functions
- **Problem**: Write a query to calculate the average **`average_rating`** of all restaurants.


SELECT AVG(average_rating)
FROM Restaurants;


db.Restaurants.aggregate([
  { $group: { _id: null, avgRating: { $avg: "$average_rating" } } }
]);


**Problem 24:**

- **Prerequisite**: Understand how to limit results in SQL / MongoDB
- **Problem**: Write a query to fetch the top 5 restaurants when ordered by **`average_rating`** in descending order.


SELECT *
FROM Restaurants
ORDER BY average_rating DESC
LIMIT 5;


db.Restaurants.find().sort({ average_rating: -1 }).limit(5);


**Problem 25:**

- **Prerequisite**: Understand data deletion in SQL / MongoDB
- **Problem**: Write a query to delete the restaurant with **`id`** 3.

DELETE FROM Restaurants
WHERE id = 3;


db.Restaurants.deleteOne({ _id: ObjectId("3") });
