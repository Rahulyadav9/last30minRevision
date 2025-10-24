Excellent 👍 — here’s a **complete AWS DynamoDB interview guide (30-minute version)** for **experienced developers (7+ years)** — covering **theory, indexing, queries, data modeling, and real-world examples**.

---

## 🧠 **1. What is DynamoDB?**

Amazon **DynamoDB** is a **fully managed NoSQL database** service that provides:

* High performance (single-digit millisecond latency)
* Horizontal scalability
* Automatic replication across multiple AZs
* Serverless (no provisioning required)

It stores **data as key-value or document-based items**.

---

## ⚙️ **2. Core Concepts**

| Concept                      | Description                               |
| ---------------------------- | ----------------------------------------- |
| **Table**                    | Collection of items (like a table in SQL) |
| **Item**                     | A single record (like a row)              |
| **Attribute**                | A field (like a column)                   |
| **Primary Key**              | Uniquely identifies each item             |
| **Partition Key (Hash Key)** | Determines partition location             |
| **Sort Key (Range Key)**     | Used for sorting and querying             |

---

## 🧩 **3. Example Table**

**Table Name:** `Users`

| userId (PK) | email (SK)                                    | name  | age | city  |
| ----------- | --------------------------------------------- | ----- | --- | ----- |
| 1           | [rahul@example.com](mailto:rahul@example.com) | Rahul | 28  | Delhi |
| 2           | [radhe@example.com](mailto:radhe@example.com) | Radhe | 30  | Pune  |

---

## 🔑 **4. Key Types**

* **Partition Key only:** Each item has a unique key (like userId).
* **Composite Key (Partition + Sort Key):** Items with same partition key can have multiple records, sorted by sort key (like userId + timestamp).

---

## 🧮 **5. Difference between DynamoDB and SQL**

| Feature      | DynamoDB               | SQL                |
| ------------ | ---------------------- | ------------------ |
| Schema       | No fixed schema        | Fixed schema       |
| Scaling      | Auto horizontal        | Manual vertical    |
| Joins        | Not supported          | Supported          |
| Queries      | Based on keys          | Based on any field |
| Transactions | Supported (since 2018) | Native support     |

---

## 🧾 **6. Creating a Table (AWS CLI Example)**

```bash
aws dynamodb create-table \
  --table-name Users \
  --attribute-definitions \
      AttributeName=userId,AttributeType=S \
  --key-schema AttributeName=userId,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST
```

---

## ⚡ **7. Insert (PutItem)**

```js
import AWS from "aws-sdk";
const dynamo = new AWS.DynamoDB.DocumentClient();

const params = {
  TableName: "Users",
  Item: {
    userId: "1",
    name: "Rahul",
    age: 28,
    city: "Delhi"
  }
};

await dynamo.put(params).promise();
```

---

## 📖 **8. Read (GetItem)**

```js
const params = {
  TableName: "Users",
  Key: { userId: "1" }
};

const result = await dynamo.get(params).promise();
console.log(result.Item);
```

---

## ✏️ **9. Update (UpdateItem)**

```js
const params = {
  TableName: "Users",
  Key: { userId: "1" },
  UpdateExpression: "set age = :age",
  ExpressionAttributeValues: { ":age": 29 },
  ReturnValues: "UPDATED_NEW"
};

await dynamo.update(params).promise();
```

---

## ❌ **10. Delete (DeleteItem)**

```js
const params = {
  TableName: "Users",
  Key: { userId: "1" }
};

await dynamo.delete(params).promise();
```

---

## 🔍 **11. Query vs Scan**

| Operation | Description                                   | Use Case                         |
| --------- | --------------------------------------------- | -------------------------------- |
| **Query** | Retrieves items based on primary key (faster) | Get all users in city “Delhi”    |
| **Scan**  | Reads all items in a table (slow)             | Full table analysis or reporting |

```js
// Query
const params = {
  TableName: "Users",
  KeyConditionExpression: "userId = :uid",
  ExpressionAttributeValues: { ":uid": "1" }
};

// Scan
const params = {
  TableName: "Users",
  FilterExpression: "age > :age",
  ExpressionAttributeValues: { ":age": 25 }
};
```

---

## 📊 **12. Indexes**

### **Global Secondary Index (GSI)**

* Allows querying with a different key.
* Example: Query by `city`.

```bash
AttributeDefinitions:
  - AttributeName: city
    AttributeType: S
GlobalSecondaryIndexes:
  - IndexName: CityIndex
    KeySchema:
      - AttributeName: city
        KeyType: HASH
```

### **Local Secondary Index (LSI)**

* Same partition key as base table, but different sort key.

---

## ⚙️ **13. Provisioned vs On-Demand Capacity**

| Mode            | Description                                            |
| --------------- | ------------------------------------------------------ |
| **Provisioned** | You define read/write capacity units                   |
| **On-Demand**   | AWS auto scales capacity as per load (pay per request) |

---

## 🧩 **14. Streams**

**DynamoDB Streams** capture changes (insert/update/delete) in real time.
They can trigger **AWS Lambda** for real-time processing.

Example use case:

* Log user updates in S3 or Elasticsearch via Lambda.

---

## 🔐 **15. IAM Policy Example**

```json
{
  "Effect": "Allow",
  "Action": [
    "dynamodb:PutItem",
    "dynamodb:GetItem",
    "dynamodb:UpdateItem"
  ],
  "Resource": "arn:aws:dynamodb:ap-south-1:123456789012:table/Users"
}
```

---

## 🧱 **16. Data Modeling Best Practices**

✅ Choose **partition key carefully** — avoid “hot” partitions.
✅ Use **composite keys** to organize time-series or related data.
✅ Use **GSIs** for flexible querying.
✅ Keep items small (recommended < 400 KB).
✅ Use **BatchWrite** for bulk inserts.

---

## 🧮 **17. Batch Operations**

Insert multiple items:

```js
const params = {
  RequestItems: {
    Users: [
      { PutRequest: { Item: { userId: "2", name: "Radhe" } } },
      { PutRequest: { Item: { userId: "3", name: "Seema" } } }
    ]
  }
};
await dynamo.batchWrite(params).promise();
```

---

## 🧾 **18. Conditional Writes**

Prevent overwriting if item already exists:

```js
const params = {
  TableName: "Users",
  Item: { userId: "1", name: "Rahul" },
  ConditionExpression: "attribute_not_exists(userId)"
};
```

---

## ⚡ **19. DynamoDB + Lambda Example**

Use Lambda to store new user records when an API is called.

```js
export const handler = async (event) => {
  const data = JSON.parse(event.body);
  await dynamo.put({
    TableName: "Users",
    Item: { id: data.id, name: data.name }
  }).promise();

  return { statusCode: 200, body: "User saved!" };
};
```

---

## 📈 **20. Monitoring and Performance**

* Use **CloudWatch** metrics (Read/Write capacity, throttling).
* **Enable Auto Scaling** for provisioned tables.
* **Use DAX (DynamoDB Accelerator)** for in-memory caching.

---

## 🧩 **21. Real-Time Scenario**

> Your system needs to store millions of IoT sensor readings per minute. What AWS solution would you use?

✅ **Answer:**
Use DynamoDB (on-demand mode) for high write throughput.
Partition key = deviceId, sort key = timestamp.
Enable Streams to trigger Lambda for analytics.

---

## 🏁 **22. Advantages**

✅ Serverless and fully managed
✅ Auto scaling
✅ Strong consistency (optional)
✅ High performance
✅ Easy AWS integration (Lambda, API Gateway, S3)

---

Would you like me to include a **DynamoDB vs MongoDB** comparison next?
That’s a **frequent question** for developers with both backend and NoSQL experience.
