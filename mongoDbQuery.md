Got it! Here’s a **concise last-30-min MongoDB interview “cheat sheet”** with **high-frequency queries and concepts** you can memorize quickly.

---

## **1️⃣ Basic CRUD**

```js
// Insert
db.customers.insertOne({name:"Rahul", age:28, dept:"Engineering"});
db.customers.insertMany([{name:"Seema", age:30}, {name:"Radhe", age:25}]);

// Find
db.customers.find();                 // All documents
db.customers.find({age: {$gt:25}}); // age > 25
db.customers.find({}, {name:1, age:1}); // Only name & age

// Update
db.customers.updateOne({name:"Rahul"}, {$set:{age:29}});
db.customers.updateMany({dept:"Sales"}, {$inc:{age:1}});

// Delete
db.customers.deleteOne({name:"Radhe"});
db.customers.deleteMany({age:{$lt:20}});
```

---

## **2️⃣ Aggregation**

```js
// Count customers per department
db.customers.aggregate([
  {$group: {_id:"$dept", count:{$sum:1}}}
]);

// Average age per department
db.customers.aggregate([
  {$group: {_id:"$dept", avgAge:{$avg:"$age"}}}
]);

// Match + Group + Sort
db.customers.aggregate([
  {$match:{age:{$gt:25}}},
  {$group:{_id:"$dept", total:{$sum:1}}},
  {$sort:{total:-1}}
]);
```

---

## **3️⃣ Indexing**

```js
// Create index
db.customers.createIndex({name:1});    // Ascending
db.customers.createIndex({age:-1});    // Descending

// Unique index
db.customers.createIndex({email:1}, {unique:true});

// Drop index
db.customers.dropIndex("name_1");
```

---

## **4️⃣ Query Operators**

| Operator  | Meaning             | Example                                   |
| --------- | ------------------- | ----------------------------------------- |
| `$eq`     | Equal               | `{age:{$eq:28}}`                          |
| `$ne`     | Not equal           | `{age:{$ne:28}}`                          |
| `$gt`     | Greater than        | `{age:{$gt:25}}`                          |
| `$gte`    | Greater or equal    | `{age:{$gte:25}}`                         |
| `$lt`     | Less than           | `{age:{$lt:30}}`                          |
| `$lte`    | Less or equal       | `{age:{$lte:30}}`                         |
| `$in`     | In array            | `{dept:{$in:["Sales","Eng"]}}`            |
| `$nin`    | Not in array        | `{dept:{$nin:["Sales"]}}`                 |
| `$and`    | AND conditions      | `{$and:[{age:{$gt:25}}, {dept:"Sales"}]}` |
| `$or`     | OR conditions       | `{$or:[{age:{$lt:20}}, {dept:"Sales"}]}`  |
| `$exists` | Field exists or not | `{email:{$exists:true}}`                  |

---

## **5️⃣ Sorting & Limiting**

```js
db.customers.find().sort({age:1});  // Ascending
db.customers.find().sort({age:-1}); // Descending
db.customers.find().limit(5);       // First 5 docs
db.customers.find().skip(2).limit(3); // Pagination
```

---

## **6️⃣ Views**

```js
// Create a view
db.createView("youngCustomers", "customers", [{$match:{age:{$lt:30}}}]);

// Query view
db.youngCustomers.find();
```

---

## **7️⃣ Embedded Docs & References**

```js
// Embedded
db.customers.insertOne({
  name:"Rahul",
  orders:[{item:"Laptop", amount:1000},{item:"Mouse", amount:50}]
});

// Reference
db.orders.insertOne({item:"Laptop", amount:1000, customer_id:ObjectId("...")});
```

---

## **8️⃣ Transactions (Replica Set)**

```js
const session = db.getMongo().startSession();
session.startTransaction();

db.customers.updateOne({name:"Rahul"}, {$set:{age:30}}, {session});
db.orders.insertOne({item:"Keyboard", customer_id:ObjectId("...")}, {session});

session.commitTransaction();
session.endSession();
```

---

## **9️⃣ Sharding & Replication Concept**

* **Replication:** Copies same data → high availability.
* **Sharding:** Splits data → horizontal scaling.

---

This covers the **most commonly asked MongoDB queries & concepts for a quick interview revision**:

* CRUD
* Aggregation
* Indexing
* Operators
* Views
* Transactions
* Sharding vs Replication

---

Absolutely! Here’s a **quick MongoDB vs SQL query comparison** for **last-minute interview prep**, covering the most commonly asked operations.

---

## **1️⃣ Select / Find**

| SQL                                             | MongoDB                                                        |
| ----------------------------------------------- | -------------------------------------------------------------- |
| `SELECT * FROM customers;`                      | `db.customers.find();`                                         |
| `SELECT name, age FROM customers WHERE age>25;` | `db.customers.find({age: {$gt: 25}}, {name:1, age:1, _id:0});` |

---

## **2️⃣ Insert**

| SQL                                                           | MongoDB                                                                     |
| ------------------------------------------------------------- | --------------------------------------------------------------------------- |
| `INSERT INTO customers (name, age) VALUES ('Rahul', 28);`     | `db.customers.insertOne({name:"Rahul", age:28});`                           |
| `INSERT INTO customers VALUES (1,'Rahul',28),(2,'Seema',30);` | `db.customers.insertMany([{name:"Rahul", age:28},{name:"Seema", age:30}]);` |

---

## **3️⃣ Update**

| SQL                                               | MongoDB                                                    |
| ------------------------------------------------- | ---------------------------------------------------------- |
| `UPDATE customers SET age=29 WHERE name='Rahul';` | `db.customers.updateOne({name:"Rahul"}, {$set:{age:29}});` |
| `UPDATE customers SET age=age+1 WHERE age>25;`    | `db.customers.updateMany({age:{$gt:25}}, {$inc:{age:1}});` |

---

## **4️⃣ Delete**

| SQL                                         | MongoDB                                    |
| ------------------------------------------- | ------------------------------------------ |
| `DELETE FROM customers WHERE name='Rahul';` | `db.customers.deleteOne({name:"Rahul"});`  |
| `DELETE FROM customers WHERE age<20;`       | `db.customers.deleteMany({age:{$lt:20}});` |

---

## **5️⃣ Aggregate / Group By**

| SQL                                                   | MongoDB                                                                   |
| ----------------------------------------------------- | ------------------------------------------------------------------------- |
| `SELECT dept, COUNT(*) FROM customers GROUP BY dept;` | `db.customers.aggregate([{$group:{_id:"$dept", count:{$sum:1}}}]);`       |
| `SELECT dept, AVG(age) FROM customers GROUP BY dept;` | `db.customers.aggregate([{$group:{_id:"$dept", avgAge:{$avg:"$age"}}}]);` |

---

## **6️⃣ Joins / Lookup**

| SQL                                                                           | MongoDB                                                                                                           |
| ----------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `SELECT c.name, o.item FROM customers c JOIN orders o ON c.id=o.customer_id;` | `db.customers.aggregate([{$lookup:{from:"orders", localField:"_id", foreignField:"customer_id", as:"orders"}}]);` |

---

## **7️⃣ Sorting & Limiting**

| SQL                                                 | MongoDB                                       |
| --------------------------------------------------- | --------------------------------------------- |
| `SELECT * FROM customers ORDER BY age ASC LIMIT 5;` | `db.customers.find().sort({age:1}).limit(5);` |

---

## **8️⃣ Transactions**

| SQL                                      | MongoDB                                                                                                                          |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `START TRANSACTION; UPDATE ...; COMMIT;` | `const session=db.getMongo().startSession(); session.startTransaction(); ... session.commitTransaction(); session.endSession();` |

---

## **9️⃣ Views**

| SQL                                                                    | MongoDB                                                                    |
| ---------------------------------------------------------------------- | -------------------------------------------------------------------------- |
| `CREATE VIEW young_customers AS SELECT * FROM customers WHERE age<30;` | `db.createView("youngCustomers", "customers", [{$match:{age:{$lt:30}}}]);` |

---

### **Key Takeaways**

* **SQL** uses tables, rows, joins; **MongoDB** uses collections, documents, aggregation pipelines.
* **Joins** in MongoDB are done via `$lookup`.
* **GROUP BY** in SQL → `$group` in MongoDB.
* **Transactions, Views, Sorting, Limiting** all have equivalents in MongoDB, but syntax is different.

---


# MongoDB – Why Order Matters in Compound Index

## Q: Why does order matter in compound index?

### A:
Because MongoDB follows the **Leftmost Prefix Rule**.

Indexes are stored in B-Tree order starting from the **first field**.  
MongoDB can only efficiently use the index if the query uses the leading fields.

---

## Example

### Index:
{ name: 1, age: 1 }

Sorted internally like:
(name → age)

---

### ✅ Works (Uses Index)
- { name: "Rahul" }
- { name: "Rahul", age: 25 }

### ❌ Does NOT Work Efficiently
- { age: 25 }

Reason:
Index is sorted by `name` first.
Without `name`, MongoDB must scan many records.

---

## Leftmost Prefix Rule

For index:
{ a:1, b:1, c:1 }

✔ { a }
✔ { a, b }
✔ { a, b, c }

✖ { b }
✖ { c }
✖ { b, c }

---

## Golden Rule for Designing Compound Index

1. Equality fields (=) first
2. Range fields ($gt, $lt) next
3. Sorting fields last

---

## One-Line Interview Answer

Order matters because MongoDB uses B-Tree indexes and follows the leftmost prefix rule, meaning queries must include the leading fields of the index to use it efficiently.
