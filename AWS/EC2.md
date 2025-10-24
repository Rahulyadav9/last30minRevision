Excellent 👌 — here’s a **complete AWS EC2 (Elastic Compute Cloud) interview guide (30 min)** designed for **experienced developers (7+ years)** — including theory, configuration, SSH connection, scaling, pricing, and example use cases.

---

## 🧠 **1. What is Amazon EC2?**

**Amazon EC2 (Elastic Compute Cloud)** is a web service that provides **resizable compute capacity in the cloud**.
You can **launch virtual machines (instances)** to run applications, host APIs, or backend services.

✅ Think of EC2 as your **virtual server** in AWS.

---

## ⚙️ **2. Key Features of EC2**

* **Scalable:** Launch or terminate instances on demand.
* **Customizable:** Choose OS, CPU, storage, and network settings.
* **Secure:** Control access via key pairs, security groups, and IAM roles.
* **Elastic IPs:** Assign static public IPs to instances.
* **Integrated:** Works with S3, RDS, CloudWatch, and Lambda.

---

## 💻 **3. Steps to Launch an EC2 Instance**

1. Go to **AWS Console → EC2 → Launch Instance**
2. Choose an **Amazon Machine Image (AMI)** (e.g., Ubuntu, Amazon Linux)
3. Choose an **Instance Type** (e.g., `t2.micro`, `t3.medium`)
4. Configure network (VPC, Subnet, Security Group)
5. Add storage (EBS volume)
6. Create or select a **key pair (.pem file)**
7. Launch → Connect via SSH

---

## 🧩 **4. How to Connect to EC2 (Linux Instance)**

After downloading your `.pem` file:

```bash
chmod 400 mykey.pem
ssh -i "mykey.pem" ec2-user@<Public-IP>
```

For **Ubuntu instance**:

```bash
ssh -i "mykey.pem" ubuntu@<Public-IP>
```

---

## 🔐 **5. What are Security Groups?**

They act as **virtual firewalls** controlling inbound and outbound traffic.

**Example Rules:**

| Type  | Protocol | Port | Source    |
| ----- | -------- | ---- | --------- |
| SSH   | TCP      | 22   | My IP     |
| HTTP  | TCP      | 80   | 0.0.0.0/0 |
| HTTPS | TCP      | 443  | 0.0.0.0/0 |

---

## 💾 **6. EBS (Elastic Block Store)**

EBS = Persistent storage for EC2.

* **Root Volume:** OS storage
* **Additional Volumes:** Attach/detach anytime
* **Snapshots:** Backup of EBS volumes (stored in S3)

**Example:**

```bash
aws ec2 create-snapshot --volume-id vol-1234567890 --description "Backup"
```

---

## 🧱 **7. EC2 Instance Types**

| Family                    | Purpose              | Example |
| ------------------------- | -------------------- | ------- |
| **General Purpose**       | Balanced CPU & RAM   | t2, t3  |
| **Compute Optimized**     | High CPU performance | c5, c6  |
| **Memory Optimized**      | For in-memory DBs    | r5, r6  |
| **Storage Optimized**     | For I/O heavy tasks  | i3, d2  |
| **Accelerated Computing** | ML, GPU tasks        | p3, g4  |

---

## 🔁 **8. Elastic Load Balancer (ELB) + Auto Scaling**

* **ELB:** Distributes incoming traffic to multiple EC2 instances.
* **Auto Scaling:** Automatically increases/decreases EC2 count based on CPU or traffic.

**Example Use Case:**
When traffic spikes, EC2 Auto Scaling launches new instances automatically.

---

## 🧮 **9. Pricing Models**

| Model               | Description             | Example Use          |
| ------------------- | ----------------------- | -------------------- |
| **On-Demand**       | Pay per hour/second     | Short-term workloads |
| **Reserved**        | 1 or 3-year commitment  | Consistent usage     |
| **Spot Instances**  | Bid for unused capacity | Flexible workloads   |
| **Dedicated Hosts** | Physical servers        | Compliance needs     |

---

## 🧾 **10. EC2 Metadata Service**

Access instance metadata using:

```bash
curl http://169.254.169.254/latest/meta-data/
```

Useful to get instance ID, security group, region, etc.

---

## 🔄 **11. Elastic IP**

A **static public IP** that stays with your account (not instance).
You can attach/detach it to EC2 instances anytime.

---

## 🧰 **12. Example: Deploy a Node.js App on EC2**

```bash
# Update and install Node.js
sudo apt update
sudo apt install nodejs npm -y

# Clone your repo
git clone https://github.com/user/myapp.git
cd myapp
npm install

# Start your app
node server.js
```

Make sure port `3000` is open in your **Security Group**.

---

## 🔄 **13. EC2 Lifecycle**

* **Pending → Running → Stopping → Stopped → Terminated**

You can **stop/start** instances without losing data on EBS volumes.

---

## 🧩 **14. IAM Roles with EC2**

Instead of storing credentials, assign **IAM roles** to EC2 for secure access to AWS services.

Example: Allow EC2 to access S3

```json
{
  "Effect": "Allow",
  "Action": ["s3:*"],
  "Resource": "*"
}
```

---

## 📜 **15. EC2 User Data Script**

Used for **bootstrapping** (automating setup when instance launches).

Example (installs Apache automatically):

```bash
#!/bin/bash
sudo apt update
sudo apt install apache2 -y
sudo systemctl start apache2
```

---

## 🔍 **16. Monitoring with CloudWatch**

* Track CPU, network, memory (custom metric)
* Set alarms for high CPU utilization
* Trigger Auto Scaling or notification (SNS)

---

## ⚡ **17. Real-Time Scenario**

> Your EC2 instance is frequently going down due to high CPU usage. How do you handle it?

✅ **Answer:**

* Set up **Auto Scaling group**
* Attach a **Load Balancer**
* Configure **CloudWatch alarm** (CPU > 80%) to add more instances automatically

---

## 🧩 **18. Backup and Recovery**

* Take **EBS Snapshots** regularly.
* Use **AMI (Amazon Machine Image)** to create new instances with same setup.

---

## 🚀 **19. Integrations**

| Service            | Integration with EC2              |
| ------------------ | --------------------------------- |
| **S3**             | Store static files, logs          |
| **RDS**            | Host DB, connect from EC2         |
| **Lambda**         | Trigger EC2 functions via events  |
| **CloudFormation** | Automate EC2 infrastructure setup |

---

## 💡 **20. Advantages of EC2**

✅ Flexible and Scalable
✅ Cost-effective
✅ Easy to integrate with AWS services
✅ Full control over OS and network
✅ Secure via IAM and security groups

---

