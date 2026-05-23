# 🟠 TASK 4 — Create VPC using Terraform (AWS Networking Setup)

## 🎯 Goal of This Task

By the end of this task, you will:

* Create a VPC using Terraform
* Understand CIDR blocks
* Create public subnet
* Attach Internet Gateway
* Configure route table
* Associate subnet with route table
* Explain VPC architecture in interviews confidently

---

# 🧠 Real-Time Scenario (Interview Context)

> “In my project, we created isolated VPCs for dev, QA, and production environments using Terraform to ensure secure and segregated networking for workloads like EC2, EKS, and RDS.”

---

# 🏗️ AWS Architecture We Will Build

```text id="vpcflow"
VPC (10.0.0.0/16)
        ↓
Public Subnet (10.0.1.0/24)
        ↓
Internet Gateway
        ↓
Route Table (0.0.0.0/0 → IGW)
        ↓
Internet Access Enabled
```

---

# 📦 STEP 1 — Create Project Directory

```bash id="vpcdir"
mkdir terraform-vpc
cd terraform-vpc
```

---

# 📄 STEP 2 — Create main.tf

```bash id="vpcfile"
vim main.tf
```

---

# ⚙️ STEP 3 — VPC Terraform Code

Paste this:

```hcl id="vpccode"
provider "aws" {
  region = "ap-south-1"
}

# Create VPC
resource "aws_vpc" "main_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "DevOps-VPC"
  }
}

# Create Public Subnet
resource "aws_subnet" "public_subnet" {
  vpc_id                  = aws_vpc.main_vpc.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = true
  availability_zone       = "ap-south-1a"

  tags = {
    Name = "Public-Subnet"
  }
}

# Create Internet Gateway
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main_vpc.id

  tags = {
    Name = "DevOps-IGW"
  }
}

# Create Route Table
resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.main_vpc.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }

  tags = {
    Name = "Public-Route-Table"
  }
}

# Associate Route Table with Subnet
resource "aws_route_table_association" "public_assoc" {
  subnet_id      = aws_subnet.public_subnet.id
  route_table_id = aws_route_table.public_rt.id
}
```

---

# 🧠 EXPLANATION (VERY IMPORTANT FOR INTERVIEW)

---

## 🔹 VPC

* Virtual Private Cloud
* Isolated network in AWS
* Foundation of all infrastructure

---

## 🔹 Subnet

* Logical division of VPC
* Public subnet → internet access

---

## 🔹 Internet Gateway

* Enables internet access for VPC resources

---

## 🔹 Route Table

* Defines traffic rules
* 0.0.0.0/0 → Internet Gateway

---

## 🔹 Association

* Links subnet to route table

---

# 🚀 STEP 4 — Terraform Commands

---

## Initialize

```bash id="init4"
terraform init
```

---

## Validate

```bash id="validate4"
terraform validate
```

---

## Plan

```bash id="plan4"
terraform plan
```

---

## Apply

```bash id="apply4"
terraform apply
```

Type:

```text id="confirm4"
yes
```

---

# 🎉 EXPECTED RESULT

In AWS Console you will see:

* VPC created
* Public subnet created
* Internet Gateway attached
* Route table configured

---

# 🧠 INTERVIEW EXPLANATION (VERY IMPORTANT)

> “We created a VPC using Terraform with CIDR 10.0.0.0/16. Inside VPC, we created a public subnet, attached an Internet Gateway, and configured a route table to allow internet access. This setup is used as base networking for EC2, EKS, and RDS deployments.”

---

# 🔥 REAL-TIME PROJECT VALUE

✔ Secure network isolation
✔ Multi-environment architecture
✔ Foundation for Kubernetes/EKS
✔ Production-grade networking
✔ Scalable cloud design

---

# 🧯 COMMON ERRORS & FIXES

---

## ❌ Error 1: Invalid CIDR

```text id="errcidr"
InvalidCIDRBlock
```

### Fix:

* Use valid CIDR like 10.0.0.0/16

---

## ❌ Error 2: Availability Zone mismatch

```text id="erraz"
InvalidAvailabilityZone
```

### Fix:

* Use correct AZ like ap-south-1a

---

## ❌ Error 3: Route table not working

### Fix:

* Ensure IGW attached properly
* Check route 0.0.0.0/0

---

# 💬 INTERVIEW QUESTIONS FROM TASK 4

---

## ❓ What is VPC?

> “VPC is a logically isolated virtual network in AWS where we deploy cloud resources.”

---

## ❓ Why VPC is important?

* Security isolation
* Network control
* Multi-tier architecture

---

## ❓ Difference between public and private subnet?

| Public Subnet        | Private Subnet      |
| -------------------- | ------------------- |
| Internet access      | No direct internet  |
| Used for web servers | Used for DB/backend |

---

## ❓ What is Internet Gateway?

> “It allows communication between VPC resources and the internet.”

---

## ❓ What is route table?

> “It defines network traffic rules for subnet routing.”

---

# 🔥 REAL PROJECT EXAMPLE YOU CAN SAY

> “In my project, we created VPC architecture using Terraform to isolate environments. Each environment had separate VPC, public/private subnets, and routing configurations to ensure security and scalability.”

---

# 🧠 WHAT YOU LEARNED IN TASK 4

✔ VPC creation
✔ Subnet design
✔ Internet Gateway
✔ Route tables
✔ AWS networking basics
✔ Terraform networking resources
✔ Production architecture understanding

---

# 🚀 NEXT STEP

Now you are moving into **real infrastructure engineering level**.

Next task:

# 🟣 TASK 5 — Multi-Resource Deployment (EC2 inside VPC)

You will learn:

* EC2 inside VPC
* Security Groups
* Real production architecture
* Networking + compute integration
* Interview scenario questions

---

Just say:

👉 **TASK 5**

and we will build a complete real-world AWS system like a production DevOps engineer.
