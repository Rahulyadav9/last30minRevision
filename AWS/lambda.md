---

## 🧠 **1. What is AWS Lambda?**

AWS **Lambda** is a **serverless compute service** that runs your code in response to events without provisioning or managing servers.

✅ You only pay for the compute time your code uses.
✅ It automatically scales based on the number of requests.

---

## ⚙️ **2. How does Lambda work?**

**Flow:**

1. An **event** triggers Lambda (e.g., S3 upload, API Gateway call).
2. Lambda **executes your function code** in a container.
3. The container is **automatically managed** (start, scale, shut down).

---

## 🏗️ **3. Lambda Architecture**

**Components:**

* **Handler:** The function entry point.
* **Event Source:** S3, DynamoDB, SNS, SQS, API Gateway, etc.
* **Execution Role (IAM):** Defines permissions for the function.
* **Environment Variables:** Config values for the function.
* **Layers:** Reusable code or libraries shared across Lambdas.

---

## 💻 **4. Example Lambda Function (Node.js)**

**Basic Example:**

```js
exports.handler = async (event) => {
  console.log("Event:", event);
  return {
    statusCode: 200,
    body: JSON.stringify({ message: "Hello from Lambda!" }),
  };
};
```

**Trigger via API Gateway →**

* You can expose this Lambda as an HTTP endpoint.

---

## 📦 **5. Lambda + S3 Example (Real-world Use Case)**

**Use Case:** Automatically process an image when uploaded to S3.

### Step 1: Create S3 Bucket

Upload event will trigger Lambda.

### Step 2: Lambda Function

```js
import AWS from 'aws-sdk';
const s3 = new AWS.S3();

export const handler = async (event) => {
  const record = event.Records[0];
  const bucket = record.s3.bucket.name;
  const key = record.s3.object.key;
  
  console.log(`Processing new file: ${key} from bucket: ${bucket}`);
  
  const file = await s3.getObject({ Bucket: bucket, Key: key }).promise();
  
  console.log("File size:", file.ContentLength);
  return { statusCode: 200 };
};
```

### Step 3: Add **Event Trigger**

In AWS console → S3 → **Properties → Event notifications → Add event**
Select `s3:ObjectCreated:*` and target your Lambda.

---

## 🧰 **6. Lambda + DynamoDB Example**

**Use Case:** Write to DynamoDB whenever an API call happens.

```js
import AWS from 'aws-sdk';
const dynamo = new AWS.DynamoDB.DocumentClient();

export const handler = async (event) => {
  const data = JSON.parse(event.body);
  await dynamo.put({
    TableName: "UserTable",
    Item: { id: data.id, name: data.name }
  }).promise();

  return { statusCode: 200, body: JSON.stringify({ message: "User saved" }) };
};
```

---

## ⚡ **7. Lambda Trigger Sources**

| Event Source                 | Description                    |
| ---------------------------- | ------------------------------ |
| **API Gateway**              | REST or HTTP endpoint trigger  |
| **S3**                       | File upload/delete trigger     |
| **DynamoDB Streams**         | DB change trigger              |
| **SNS / SQS**                | Messaging event trigger        |
| **CloudWatch Events / Logs** | Schedule or log-based triggers |
| **EventBridge**              | Application event routing      |

---

## 🧱 **8. What is Lambda Layer?**

Used to include **external libraries or shared code** across multiple Lambda functions.
Example: `axios`, `sharp`, or custom utils.

---

## 🧩 **9. Environment Variables**

Store config like DB credentials, URLs, API keys:

```bash
DB_HOST=mydb.amazonaws.com
DB_USER=admin
```

Access in code:

```js
process.env.DB_HOST
```

---

## 🧾 **10. Lambda Function Timeout**

Default: **3 seconds**, max: **15 minutes**
Increase in configuration if your task (like video processing) takes longer.

---

## 📜 **11. Lambda Concurrency**

* **Reserved Concurrency:** Guarantees number of concurrent executions.
* **Provisioned Concurrency:** Pre-warms instances to avoid cold starts.

---

## 🧮 **12. Read/Write Example (S3 + Lambda)**

### ✅ Upload File (Write)

Lambda triggered when a file is uploaded:

```js
export const handler = async (event) => {
  const s3 = new AWS.S3();
  const record = event.Records[0];
  const bucket = record.s3.bucket.name;
  const key = record.s3.object.key;

  const data = await s3.getObject({ Bucket: bucket, Key: key }).promise();
  console.log("File content:", data.Body.toString());
};
```

### ✅ Write Processed Data (Write)

```js
await s3.putObject({
  Bucket: "processed-bucket",
  Key: `processed/${key}`,
  Body: "Processed file content"
}).promise();
```

---

## 🔐 **13. IAM Role for Lambda**

Defines **permissions** that Lambda has.
Example: Allow reading/writing S3.

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": ["s3:GetObject", "s3:PutObject"],
    "Resource": "arn:aws:s3:::my-bucket/*"
  }]
}
```

---

## 💰 **14. Lambda Pricing**

* 1 million requests/month free
* Charged based on **invocations + compute time** (GB-seconds)

---

## 🧩 **15. Interview Scenario**

> You want to resize images uploaded to S3 and store results in another bucket. How will you design it?

✅ **Answer:**

1. Create an **S3 upload bucket**
2. Add event trigger for `s3:ObjectCreated`
3. Lambda downloads image → resizes (using `sharp`) → uploads to output bucket
4. Use IAM policy to allow S3 read/write

---

## 🚀 **16. Advantages of Lambda**

* No server management
* Auto-scaling
* Cost-efficient
* Integrates easily with AWS ecosystem
* Supports multiple runtimes (Node.js, Python, Go, Java, .NET)

---

## ⚠️ **17. Limitations**

* Max 15 min runtime
* 50 MB deployment limit (250 MB with layers)
* Cold starts for inactive functions
* Limited local storage (`/tmp` = 512MB)

---

Would you like me to add a **real-world microservice example** — e.g., *“User registration API using Lambda + API Gateway + DynamoDB”* — that’s a common system design round topic for 7+ years of experience?
