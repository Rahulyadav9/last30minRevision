

---

## **1️⃣ Basics**

**Q1. What is MongoDB?**

* A **NoSQL document database** that stores data in **JSON-like BSON documents**.
* Schema-less, scalable, and high-performance.

**Q2. Difference between SQL and MongoDB?**

| Feature      | SQL (Relational) | MongoDB (NoSQL)                     |
| ------------ | ---------------- | ----------------------------------- |
| Schema       | Fixed            | Dynamic                             |
| Data storage | Tables/Rows      | Collections/Documents               |
| Joins        | Supported        | Limited (via $lookup)               |
| Transactions | ACID supported   | Multi-document ACID supported (v4+) |

---

## **2️⃣ Data Model**

**Q3. What is a collection and document?**

* **Document:** Basic unit of data, like a JSON object.
* **Collection:** Group of documents, like a table in SQL.

**Example:**

```json
{
  "_id": ObjectId("123"),
  "name": "Rahul",
  "age": 28,
  "dept": "Engineering"
}
```

**Q4. What is `_id` in MongoDB?**

* Every document has a unique `_id` field (primary key).
* Auto-generated as `ObjectId` if not provided.

---

## **3️⃣ CRUD Operations**

**Q5. Insert document**

```js
db.customers.insertOne({name: "Rahul", age: 28});
```

**Q6. Find document**

```js
db.customers.find({age: {$gt: 25}});
```

**Q7. Update document**

```js
db.customers.updateOne({name: "Rahul"}, {$set: {age: 29}});
```

**Q8. Delete document**

```js
db.customers.deleteOne({name: "Rahul"});
```

---

## **4️⃣ Indexing & Performance**

**Q9. What is an index?**

* Index improves query performance.
* Example: `db.customers.createIndex({name: 1});`

**Q10. Types of Indexes**

* Single field
* Compound index
* Unique index
* Text index
* TTL index (Time-to-Live for documents)

---

## **5️⃣ Aggregation**

**Q11. What is aggregation in MongoDB?**

* Process of **computing results from multiple documents** using the **aggregation pipeline**.

**Example: Count customers by dept**

```js
db.customers.aggregate([
  {$group: {_id: "$dept", count: {$sum: 1}}}
]);
```

**Q12. Key stages in aggregation**

* `$match` → filter
* `$group` → group & aggregate
* `$project` → include/exclude fields
* `$sort` → sort results
* `$limit` / `$skip` → pagination

---

## **6️⃣ Advanced Concepts**

**Q13. What is sharding?**
| **Term**       | **Explanation**                                                                                                                                                              |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Definition** | **Sharding** is the process of **splitting large datasets into smaller, faster, and more manageable pieces (called *shards*)** that are distributed across multiple servers. |
| **Purpose**    | To **scale horizontally** — handle more data and higher traffic by using multiple machines instead of one big one.                                                           |
| **Why needed** | When a single MongoDB server can no longer store or process all data efficiently (e.g., large e-commerce, social media, IoT apps).                                           |

**Q14. What is replication?**

* Copies of data across multiple nodes (primary + secondaries).
* Provides **high availability**.

**Q15. Difference between MongoDB and Mongoose**

* MongoDB → database.
* Mongoose → Node.js ODM (Object Data Modeling) library.

**Q16. Transactions in MongoDB**

* Supported for **multi-document operations** in replica sets (v4+).
* Use `startSession()`, `withTransaction()` in Node.js driver.

**Q17. Difference between `$set`, `$inc`, `$unset`**

* `$set`: Update field value
* `$inc`: Increment field
* `$unset`: Remove field

---

## **7️⃣ Miscellaneous**

**Q18. What is BSON?**

* Binary JSON format used internally by MongoDB.
* Supports additional types like `Date`, `ObjectId`, `Binary`.

**Q19. Difference between embedded documents and references**

* **Embedded:** Store related data inside one document (fast reads).
* **References:** Store ObjectId of another document (normalized, saves space).

**Q20. Pros of MongoDB**

* Flexible schema
* Horizontal scaling
* High performance for read/write
* Good for big data and analytics

---

