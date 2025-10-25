
Here’s a **complete last-minute 30-minute CloudFormation interview Q&A** (ideal for 5–8 years of experience) 👇

---

## 🧩 **1. What is AWS CloudFormation?**

**Answer:**
CloudFormation is an **Infrastructure as Code (IaC)** service that allows you to define and provision AWS infrastructure using **YAML or JSON templates**.
Instead of manually creating resources from the console, you describe them in a template, and CloudFormation automatically provisions and configures them.

---

## ⚙️ **2. Key Components**

| Component           | Description                                                |
| ------------------- | ---------------------------------------------------------- |
| **Template**        | YAML/JSON file defining AWS resources.                     |
| **Stack**           | A collection of resources created from a template.         |
| **Change Set**      | Preview of changes before applying updates.                |
| **Drift Detection** | Detects configuration changes made outside CloudFormation. |
| **Nested Stack**    | Stack inside another stack (helps modularize templates).   |

---

## 📄 **3. Basic Template Structure**

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: Simple EC2 instance

Resources:
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      InstanceType: t2.micro
      ImageId: ami-0c55b159cbfafe1f0
```

✅ **Run using AWS CLI:**

```bash
aws cloudformation create-stack --stack-name MyEC2Stack --template-body file://ec2.yaml
```

---

## 📦 **4. Parameters, Mappings, and Outputs**

### Example:

```yaml
Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    AllowedValues: [t2.micro, t3.micro]
    Description: EC2 Instance type

Mappings:
  RegionMap:
    us-east-1:
      AMI: ami-0c55b159cbfafe1f0
    ap-south-1:
      AMI: ami-0a23ccb2cdd9286bb

Resources:
  EC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId: !FindInMap [RegionMap, !Ref 'AWS::Region', AMI]
      InstanceType: !Ref InstanceType

Outputs:
  InstanceId:
    Description: The EC2 instance ID
    Value: !Ref EC2Instance
```

---

## 🧠 **5. Difference Between CloudFormation and Terraform**

| Feature          | CloudFormation | Terraform                |
| ---------------- | -------------- | ------------------------ |
| Language         | YAML/JSON      | HCL                      |
| Scope            | AWS only       | Multi-cloud              |
| State Management | Managed by AWS | Manual or remote backend |
| Cost             | Free           | Free (open source)       |
| Drift Detection  | Yes            | Yes (via refresh)        |

---

## 🔁 **6. Update Stack (Change Set)**

Before updating resources:

```bash
aws cloudformation create-change-set --stack-name MyStack --change-set-name UpdateV1 --template-body file://update.yaml
```

Then apply changes:

```bash
aws cloudformation execute-change-set --change-set-name UpdateV1 --stack-name MyStack
```

---

## 🧩 **7. Read and Write Example using CloudFormation**

Let’s create an **S3 bucket** and an **IAM Role** with read/write access:

```yaml
Resources:
  MyS3Bucket:
    Type: AWS::S3::Bucket
    Properties:
      BucketName: my-cloudformation-demo-bucket

  S3AccessRole:
    Type: AWS::IAM::Role
    Properties:
      RoleName: CFNReadWriteRole
      AssumeRolePolicyDocument:
        Version: '2012-10-17'
        Statement:
          - Effect: Allow
            Principal:
              Service: [lambda.amazonaws.com]
            Action: sts:AssumeRole
      Policies:
        - PolicyName: S3ReadWriteAccess
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - s3:GetObject
                  - s3:PutObject
                Resource: !Sub arn:aws:s3:::${MyS3Bucket}/*
```

✅ This template:

* Creates an S3 bucket.
* Grants a role read/write permissions to that bucket.

---

## 🚨 **8. CloudFormation Rollback**

If resource creation fails, CloudFormation automatically **rolls back** all resources to maintain stack integrity.

You can disable rollback with:

```bash
aws cloudformation create-stack --stack-name test --template-body file://test.yaml --disable-rollback
```

---

## 📊 **9. CloudFormation Stack Lifecycle**

1. Create Stack
2. Update Stack
3. Delete Stack
4. Drift Detection
5. Rollback (if failed)

---

## 🧾 **10. Common Interview Questions**

* What are **intrinsic functions**?
  → Functions like `!Ref`, `!Sub`, `!GetAtt`, `!FindInMap`, `!Join`.

* How do you handle dependencies between resources?
  → Use `DependsOn`.

* How do you import an existing resource?
  → Use `import existing resource` or `stack import`.

* How do you modularize CloudFormation templates?
  → Using **Nested Stacks** or **StackSets**.

---

Would you like me to include **a full mini-project template** (e.g., EC2 + S3 + Lambda via CloudFormation) that you can mention in interviews?
