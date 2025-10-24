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

If you want, I can make a **“super short one-page MongoDB cheat sheet”** with **all commands and operators in one glance**, perfect for last-minute 5-min revision.

Do you want me to make that?
