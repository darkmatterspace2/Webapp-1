📘 Firebase Realtime Database — Query Guide (SQL Comparison)

This guide explains how to fetch, filter, and query data from Firebase Realtime Database, using examples similar to SQL queries to help beginners understand quickly.

Firebase is not SQL — but many SQL-like operations can be simulated using its query methods.

📂 Basic Firebase Query Methods

Firebase Realtime Database provides these core query tools:

orderByChild("field")

orderByKey()

orderByValue()

equalTo(value)

startAt(value)

endAt(value)

limitToFirst(n)

limitToLast(n)

🔹 1. Fetch All Data
SQL
SELECT * FROM users;

Firebase
db.ref("users").once("value");

🔹 2. WHERE condition (equals)
SQL
SELECT * FROM users WHERE age = 25;

Firebase
db.ref("users")
  .orderByChild("age")
  .equalTo(25)
  .once("value");

🔹 3. WHERE age > / < (Greater / Lesser)
SQL
SELECT * FROM users WHERE age > 18;

Firebase
db.ref("users")
  .orderByChild("age")
  .startAt(18)
  .once("value");

SQL
SELECT * FROM users WHERE age < 18;

Firebase
db.ref("users")
  .orderByChild("age")
  .endAt(18)
  .once("value");

🔹 4. LIMIT (Top or Bottom N)
SQL
SELECT * FROM users LIMIT 10;

Firebase (First 10 records)
db.ref("users")
  .limitToFirst(10)
  .once("value");

Firebase (Last 10 records)
db.ref("users")
  .limitToLast(10)
  .once("value");

🔹 5. ORDER BY field
SQL
SELECT * FROM users ORDER BY age;

Firebase
db.ref("users")
  .orderByChild("age")
  .once("value");

🔹 6. Fetch Record by ID
SQL
SELECT * FROM users WHERE id = 15;

Firebase
db.ref("users/15").once("value");

🔹 7. BETWEEN (Range Query)
SQL
SELECT * FROM users WHERE age BETWEEN 20 AND 30;

Firebase
db.ref("users")
  .orderByChild("age")
  .startAt(20)
  .endAt(30)
  .once("value");

🔹 8. LIKE Search (prefix match)

Realtime Database does not support full text search, but prefix search can be simulated.

SQL
SELECT * FROM users WHERE name LIKE 'A%';

Firebase
db.ref("users")
  .orderByChild("name")
  .startAt("A")
  .endAt("A\uf8ff")
  .once("value");

📌 Example Data Structure (Recommended)

Your data should look like this:

{
  "users": {
    "-Abc123": { "name": "John", "age": 25 },
    "-Def456": { "name": "Maya", "age": 22 },
    "-Gh789":  { "name": "Ravi", "age": 30 }
  }
}


This structure allows all the above queries to work properly.

🧪 Full Example (Get users age = 25)
db.ref("users")
  .orderByChild("age")
  .equalTo(25)
  .once("value")
  .then(snapshot => {
    snapshot.forEach(child => {
      console.log(child.key, child.val());
    });
  });

📘 SQL vs Firebase — Quick Comparison Table
SQL	Firebase
SELECT * FROM table	ref("table").once("value")
WHERE field = x	orderByChild("field").equalTo(x)
WHERE field > x	orderByChild("field").startAt(x)
WHERE field < x	orderByChild("field").endAt(x)
ORDER BY field	orderByChild("field")
ORDER BY field DESC LIMIT 10	orderByChild("field").limitToLast(10)
LIMIT 10	limitToFirst(10)
LIKE 'A%'	startAt("A").endAt("A\uf8ff")
🎉 Summary

Firebase Realtime Database is not SQL, but you can perform:

Basic filtering

Sorting

Range queries

Prefix search

Limit queries

By combining orderBy*, startAt, endAt, equalTo, and limitTo*.