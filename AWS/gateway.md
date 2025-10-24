Here’s a **complete 30-minute AWS API Gateway interview Q&A** — focused for experienced developers (5–8 years) with practical examples including **read/write operations, integrations, and Lambda usage** 👇

---

## 🚀 **1. What is Amazon API Gateway?**

**Answer:**
AWS API Gateway is a **fully managed service** that lets you create, publish, maintain, monitor, and secure **REST, HTTP, and WebSocket APIs** at any scale.
It acts as a **“front door”** for your backend services (like AWS Lambda, EC2, or DynamoDB).

---

## ⚙️ **2. Key Features**

| Feature                                                | Description                                                 |
| ------------------------------------------------------ | ----------------------------------------------------------- |
| **Integration with Lambda, EC2, or any HTTP endpoint** | Directly connects API Gateway to backend logic.             |
| **Authentication & Authorization**                     | Supports IAM roles, Cognito, and custom authorizers.        |
| **Caching**                                            | Reduces backend load and improves latency.                  |
| **Throttling & Rate Limiting**                         | Controls API request limits.                                |
| **Custom Domain Names**                                | Enables user-friendly URLs.                                 |
| **Stage Variables**                                    | Used for environment-based configuration (dev, test, prod). |

---

## 📘 **3. Basic Architecture**

**Client → API Gateway → Lambda (or EC2, DynamoDB) → Response**

---

## 🧩 **4. Example: Creating a Simple API with Lambda**

### Step 1️⃣: Create Lambda Function

```javascript
// Lambda function (index.js)
exports.handler = async (event) => {
  const name = event.queryStringParameters?.name || "Guest";
  return {
    statusCode: 200,
    body: JSON.stringify({ message: `Hello, ${name}!` }),
  };
};
```

### Step 2️⃣: Deploy via API Gateway

* Go to AWS Console → API Gateway → Create REST API
* Create a resource `/hello`
* Create a **GET method** → integrate with the Lambda function above
* Deploy API to a **stage** (e.g., `dev`)

✅ **Test URL Example:**

```
GET https://abcd1234.execute-api.ap-south-1.amazonaws.com/dev/hello?name=Rahul
```

**Response:**

```json
{ "message": "Hello, Rahul!" }
```

---

## 🗂️ **5. Read/Write Example with DynamoDB**

### 🧠 Lambda Function Integrated with API Gateway

```javascript
import AWS from "aws-sdk";
const dynamo = new AWS.DynamoDB.DocumentClient();

export const handler = async (event) => {
  const body = JSON.parse(event.body);
  
  if (event.httpMethod === "POST") {
    // Write operation
    await dynamo.put({
      TableName: "Users",
      Item: { userId: body.userId, name: body.name },
    }).promise();
    return { statusCode: 201, body: JSON.stringify({ message: "User created!" }) };
  }

  if (event.httpMethod === "GET") {
    // Read operation
    const data = await dynamo.get({
      TableName: "Users",
      Key: { userId: event.queryStringParameters.userId },
    }).promise();
    return { statusCode: 200, body: JSON.stringify(data.Item) };
  }

  return { statusCode: 400, body: JSON.stringify({ error: "Invalid Request" }) };
};
```

✅ **Integrate this Lambda with two methods in API Gateway:**

* `POST /user` → Create new user
* `GET /user?userId=1` → Fetch user

---

## 🔐 **6. Authentication Options**

| Method                 | Description                                        |
| ---------------------- | -------------------------------------------------- |
| **IAM Roles/Policies** | Restrict API access using AWS credentials.         |
| **Cognito User Pools** | For user-based login/authentication.               |
| **Custom Authorizer**  | Lambda that verifies tokens (JWT, OAuth2, etc.).   |
| **API Keys**           | Simple authentication mechanism for internal apps. |

---

## 📊 **7. API Gateway Caching**

* You can **enable cache per method or stage**.
* Example: Cache GET responses for 60 seconds.
* It reduces DynamoDB/Lambda calls and improves performance.

```bash
aws apigateway update-stage \
  --rest-api-id <api-id> \
  --stage-name dev \
  --patch-operations op=replace,path=/cacheClusterEnabled,value=true
```

---

## 🧩 **8. Stages and Deployment**

| Term                | Description                                   |
| ------------------- | --------------------------------------------- |
| **Stage**           | Represents an environment (e.g., dev, prod).  |
| **Deployment**      | Versioned snapshot of your API configuration. |
| **Stage Variables** | Used to switch backend endpoints dynamically. |

Example variable:

```json
{
  "lambdaAlias": "dev"
}
```

Use inside integration URI:

```
arn:aws:lambda:ap-south-1:123456789012:function:MyFunction:${stageVariables.lambdaAlias}
```

---

## 🧱 **9. Throttling and Rate Limiting**

Used to prevent abuse or accidental overload.

Example configuration:

* **Rate limit:** 100 requests per second
* **Burst limit:** 200 requests

---

## 🧾 **10. Monitoring and Logging**

* Integrated with **CloudWatch Logs & Metrics**
* Logs include:

  * API latency
  * Integration latency
  * 4xx / 5xx errors
  * Request count

Enable with:

```bash
aws apigateway update-stage \
  --rest-api-id <api-id> \
  --stage-name dev \
  --patch-operations op=replace,path=/methodSettings/*/*/logging/loglevel,value=INFO
```

---

## 🧠 **11. Common Interview Questions**

| Question                                                         | Short Answer                                                                            |
| ---------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **What types of APIs does API Gateway support?**                 | REST, HTTP, and WebSocket APIs                                                          |
| **What is the difference between REST and HTTP API in Gateway?** | HTTP APIs are faster and cheaper, REST APIs have more features (Auth, transformations). |
| **How do you handle CORS in API Gateway?**                       | Enable CORS in method settings (OPTIONS preflight request).                             |
| **Can you deploy multiple versions of the same API?**            | Yes, using different **stages** (dev, test, prod).                                      |
| **What is Integration Type?**                                    | It defines the backend target — Lambda, HTTP, AWS service, or Mock.                     |
| **How do you test API Gateway locally?**                         | Use Postman or `curl` with the deployed endpoint.                                       |

---

## 🧩 **12. Integration with CloudFormation**

Here’s a **CloudFormation snippet** to create a simple API Gateway + Lambda:

```yaml
Resources:
  MyLambdaFunction:
    Type: AWS::Lambda::Function
    Properties:
      Handler: index.handler
      Role: arn:aws:iam::123456789012:role/LambdaRole
      Runtime: nodejs18.x
      Code:
        ZipFile: |
          exports.handler = async (event) => {
            return { statusCode: 200, body: JSON.stringify({ message: "Hello from Lambda!" }) };
          }

  MyApiGateway:
    Type: AWS::ApiGateway::RestApi
    Properties:
      Name: MyDemoAPI

  MyApiResource:
    Type: AWS::ApiGateway::Resource
    Properties:
      ParentId: !GetAtt MyApiGateway.RootResourceId
      PathPart: hello
      RestApiId: !Ref MyApiGateway

  MyApiMethod:
    Type: AWS::ApiGateway::Method
    Properties:
      HttpMethod: GET
      ResourceId: !Ref MyApiResource
      RestApiId: !Ref MyApiGateway
      AuthorizationType: NONE
      Integration:
        Type: AWS
        IntegrationHttpMethod: POST
        Uri: !Sub arn:aws:apigateway:${AWS::Region}:lambda:path/2015-03-31/functions/${MyLambdaFunction.Arn}/invocations
```

---

Would you like me to add **a full real-world example** (API Gateway + Lambda + DynamoDB + Cognito authentication) — like what’s asked in senior-level system design rounds?
