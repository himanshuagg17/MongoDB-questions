**Problem 36:**

- **Prerequisite**: Understand using the MAX and MIN functions in SQL / using sort and limit in MongoDB
- **Problem**: Write a query to find the ride with the highest and lowest **`fare`**.

SELECT *
FROM Rides
WHERE fare = (SELECT MAX(fare) FROM Rides)
   OR fare = (SELECT MIN(fare) FROM Rides);


db.Rides.find().sort({ fare: -1 }).limit(1);
db.Rides.find().sort({ fare: 1 }).limit(1);



**Problem 37:**

- **Prerequisite**: Understand using the GROUP BY clause in SQL / using aggregate in MongoDB
- **Problem**: Write a query to find the average **`fare`** and **`distance`** for each **`driver_id`**.

SELECT driver_id, AVG(fare) AS average_fare, AVG(distance) AS average_distance
FROM Rides
GROUP BY driver_id;


db.Rides.aggregate([
  { $group: { _id: "$driver_id", average_fare: { $avg: "$fare" }, average_distance: { $avg: "$distance" } } }
]);


**Problem 38:**

- **Prerequisite**: Understand using HAVING clause in SQL / using match in MongoDB's aggregate pipeline
- **Problem**: Write a query to find **`driver_id`** that have completed more than 5 rides.

SELECT driver_id
FROM Rides
GROUP BY driver_id
HAVING COUNT(*) > 5;


db.Rides.aggregate([
  { $group: { _id: "$driver_id", total_rides: { $sum: 1 } } },
  { $match: { total_rides: { $gt: 5 } } },
  { $project: { _id: 1 } }
]);




**Problem 39:**

- **Prerequisite**: Understand the use of INNER JOIN in SQL / Lookup in MongoDB
- **Problem**: Assuming there is another collection/table called **`Drivers`** with **`driver_id`** and **`name`** fields, write a query to find the name of the driver with the highest **`fare`**.

SELECT d.name
FROM Rides r
JOIN Drivers d ON r.driver_id = d.driver_id
WHERE r.fare = (SELECT MAX(fare) FROM Rides);


db.Rides.aggregate([
  { $sort: { fare: -1 } },
  { $limit: 1 },
  { $lookup: { from: "Drivers", localField: "driver_id", foreignField: "driver_id", as: "driver" } },
  { $unwind: "$driver" },
  { $project: { _id: 0, name: "$driver.name" } }
]);


**Problem 40:**

- **Prerequisite**: Understand the concept of subqueries in SQL / using multiple stages in MongoDB's aggregate pipeline
- **Problem**: Write a query to find the top 3 drivers who have earned the most from fares. Return the drivers' ids and total earnings.

SELECT driver_id, SUM(fare) AS total_earnings
FROM Rides
GROUP BY driver_id
ORDER BY total_earnings DESC
LIMIT 3;


db.Rides.aggregate([
  { $group: { _id: "$driver_id", total_earnings: { $sum: "$fare" } } },
  { $sort: { total_earnings: -1 } },
  { $limit: 3 },
  { $project: { _id: 0, driver_id: "$_id", total_earnings: 1 } }
]);


**Problem 41:**

- **Prerequisite**: Understand date and time functions in SQL / MongoDB
- **Problem**: Assuming there's a **`ride_date`** field of date type in the **`Rides`** table / collection, write a query to find all rides that happened in the last 7 days.

SELECT *
FROM Rides
WHERE ride_date >= DATE(NOW()) - INTERVAL 7 DAY;


db.Rides.find({
  ride_date: { $gte: new Date(new Date().getTime() - (7 * 24 * 60 * 60 * 1000)) }
});


**Problem 42:**

- **Prerequisite**: Understand the concept of NULL values and how to handle them in SQL / MongoDB
- **Problem**: Write a query to find all rides where the **`end_location`** is not set.

SELECT *
FROM Rides
WHERE end_location IS NULL;


db.Rides.find({ end_location: null });


**Problem 43:**

- **Prerequisite**: Understand the use of complex mathematical operations in SQL / MongoDB
- **Problem**: Write a query to calculate the fare per mile for each ride and return the ride ids and their fare per mile, ordered by fare per mile in descending order.


SELECT id, fare / distance AS fare_per_mile
FROM Rides
ORDER BY fare_per_mile DESC;


db.Rides.aggregate([
  { $project: { id: 1, fare_per_mile: { $divide: ["$fare", "$distance"] } } },
  { $sort: { fare_per_mile: -1 } }
]);


**Problem 44:**

- **Prerequisite**: Understand the use of multiple JOINs in SQL / multiple Lookups in MongoDB
- **Problem**: Assuming there's another collection/table **`Passengers`** with **`passenger_id`** and **`name`** fields, write a query to return a list of all rides including the driver's name and passenger's name.

SELECT r.*, d.name AS driver_name, p.name AS passenger_name
FROM Rides r
JOIN Drivers d ON r.driver_id = d.driver_id
JOIN Passengers p ON r.passenger_id = p.passenger_id;


db.Rides.aggregate([
  { $lookup: { from: "Drivers", localField: "driver_id", foreignField: "driver_id", as: "driver" } },
  { $lookup: { from: "Passengers", localField: "passenger_id", foreignField: "passenger_id", as: "passenger" } },
  { $unwind: "$driver" },
  { $unwind: "$passenger" },
  { $project: { _id: 0, driver_name: "$driver.name", passenger_name: "$passenger.name", ...<other fields> } }
]);


**Problem 45:**

- **Prerequisite**: Understand how to alter table schemas in SQL / adding and modifying fields in MongoDB documents
- **Problem**: Write a query to add a **`tip`** field to the **`Rides`** table / collection.

ALTER TABLE Rides
ADD COLUMN tip DECIMAL(6,2);


db.Rides.updateMany({}, { $set: { tip: null } });