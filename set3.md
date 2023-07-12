<!-- mongoDB schema -->

{
  "_id": ObjectId(),
  "driver_id": ObjectId(),
  "passenger_id": ObjectId(),
  "start_location": String,
  "end_location": String,
  "distance": Number,
  "ride_time": Number,
  "fare": Number
}




**Problem 28:**

- **Prerequisite**: Understand how to order data in SQL / MongoDB
- **Problem**: Write a query to fetch all rides, ordered by **`fare`** in descending order.

SELECT *
FROM Rides
ORDER BY fare DESC;


db.Rides.find().sort({ fare: -1 });


**Problem 29:**

- **Prerequisite**: Understand using math operations in SQL / MongoDB
- **Problem**: Write a query to calculate the total **`distance`** and total **`fare`** for all rides.

SELECT SUM(distance) AS total_distance, SUM(fare) AS total_fare
FROM Rides;


db.Rides.aggregate([
  { $group: { _id: null, total_distance: { $sum: "$distance" }, total_fare: { $sum: "$fare" } } }
]);


**Problem 30:**

- **Prerequisite**: Understand how to use the AVG function in SQL / MongoDB's aggregate functions
- **Problem**: Write a query to calculate the average **`ride_time`** of all rides.

SELECT AVG(ride_time) AS average_ride_time
FROM Rides;


db.Rides.aggregate([
  { $group: { _id: null, average_ride_time: { $avg: "$ride_time" } } }
]);




**Problem 31:**

- **Prerequisite**: Understand using string patterns in SQL (LIKE clause) / using regex in MongoDB
- **Problem**: Write a query to fetch all rides whose **`start_location`** or **`end_location`** contains 'Downtown'.

SELECT *
FROM Rides
WHERE start_location LIKE '%Downtown%' OR end_location LIKE '%Downtown%';


db.Rides.find({
  $or: [
    { start_location: { $regex: 'Downtown', $options: 'i' } },
    { end_location: { $regex: 'Downtown', $options: 'i' } }
  ]
});


**Problem 32:**

- **Prerequisite**: Understand how to use the COUNT function in SQL / MongoDB's aggregate functions
- **Problem**: Write a query to count the number of rides for a given **`driver_id`**.

SELECT COUNT(*) AS ride_count
FROM Rides
WHERE driver_id = <driver_id>;


db.Rides.count({ driver_id: ObjectId("<driver_id>") });



**Problem 33:**

- **Prerequisite**: Understand data updating in SQL / MongoDB
- **Problem**: Write a query to update the **`fare`** of the ride with **`id`** 4.

UPDATE Rides
SET fare = <new_fare>
WHERE id = 4;



db.Rides.updateOne({ _id: ObjectId("4") }, { $set: { fare: <new_fare> } });


**Problem 34:**

- **Prerequisite**: Understand using GROUP BY in SQL / MongoDB's aggregate functions
- **Problem**: Write a query to calculate the total **`fare`** for each **`driver_id`**.

SELECT driver_id, SUM(fare) AS total_fare
FROM Rides
GROUP BY driver_id;


db.Rides.aggregate([
  { $group: { _id: "$driver_id", total_fare: { $sum: "$fare" } } }
]);


**Problem 35:**

- **Prerequisite**: Understand data deletion in SQL / MongoDB
- **Problem**: Write a query to delete the ride with **`id`** 2.

DELETE FROM Rides
WHERE id = 2;


db.Rides.deleteOne({ _id: ObjectId("2") });
