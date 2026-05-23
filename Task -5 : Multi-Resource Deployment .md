# 🟣 TASK 5 — Multi-Resource Deployment (EC2 inside VPC)

## 🎯 Goal of This Task

By the end of this task, you will:

* Launch EC2 inside your custom VPC (from TASK 4)
* Create Security Group (allow SSH/HTTP)
* Understand real network placement
* Connect compute + networking
* Explain full AWS architecture in interviews

---

# 🧠 Real-Time Scenario (Interview Context)

> “In my project, after creating VPC using Terraform, we deployed EC2 instances inside private/public subnets with security groups to control inbound traffic. This formed the base layer for application hosting.”

---

# 🏗️ Architecture We Are Building

```text id="arch5"
VPC (10.0.0.0/16)
        ↓
Public Subnet
        ↓
Security Group (SSH + HTTP)
        ↓
EC2 Instance (Web Server)
        ↓
Internet Access via IGW
```

---

# 📦 STEP 1 — Create Project Directory

```bash id="task5dir"
mkdir terraform-ec2-vpc
cd terraform-ec2-vpc
```

---

# 📄 STEP 2 — Create main.tf

```bash id="task5file"
vim main.tf
```

---

# ⚙️ STEP 3 — Terraform Code (EC2 inside VPC)

👉 IMPORTANT: We are combining VPC + EC2 + Security Group

```hcl id="task5code"
provider "aws" {
  region = "ap-south-1"
}

# ----------------------------
# VPC
# ----------------------------
resource "aws_vpc" "main_vpc" {
  cidr_block = "10.0.0.0/16"

  tags = {
    Name = "DevOps-Main-VPC"
  }
}

# ----------------------------
# Public Subnet
# ----------------------------
resource "aws_subnet" "public_subnet" {
  vpc_id                  = aws_vpc.main_vpc.id
  cidr_block              = "10.0.1.0/24"
  map_public_ip_on_launch = true
  availability_zone       = "ap-south-1a"

  tags = {
    Name = "Public-Subnet"
  }
}

# ----------------------------
# Internet Gateway
# ----------------------------
resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.main_vpc.id

  tags = {
    Name = "IGW"
  }
}

# ----------------------------
# Route Table
# ----------------------------
resource "aws_route_table" "public_rt" {
  vpc_id = aws_vpc.main_vpc.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }

  tags = {
    Name = "Public-RT"
  }
}

# ----------------------------
# Route Table Association
# ----------------------------
resource "aws_route_table_association" "assoc" {
  subnet_id      = aws_subnet.public_subnet.id
  route_table_id = aws_route_table.public_rt.id
}

# ----------------------------
# Security Group
# ----------------------------
resource "aws_security_group" "web_sg" {
  name        = "web-sg"
  description = "Allow SSH and HTTP"
  vpc_id      = aws_vpc.main_vpc.id

  ingress {
    description = "SSH"
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTP"
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "Web-SG"
  }
}

# ----------------------------
# EC2 Instance
# ----------------------------
resource "aws_instance" "web_ec2" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"

  subnet_id              = aws_subnet.public_subnet.id
  vpc_security_group_ids = [aws_security_group.web_sg.id]

  tags = {
    Name = "Terraform-Web-EC2"
  }
}
```

---

# 🧠 STEP 4 — Explanation (VERY IMPORTANT)

---

## 🔹 VPC

Creates isolated network

---

## 🔹 Subnet

Where EC2 will be placed

---

## 🔹 Internet Gateway

Gives internet access

---

## 🔹 Route Table

Routes traffic outside VPC

---

## 🔹 Security Group

Controls:

* SSH (22)
* HTTP (80)

---

## 🔹 EC2 Instance

* Launched inside VPC
* Connected to subnet + SG

---

# 🚀 STEP 5 — Terraform Commands

---

## Initialize

```bash id="init5"
terraform init
```

---

## Validate

```bash id="validate5"
terraform validate
```

---

## Plan

```bash id="plan5"
terraform plan
```

---

## Apply

```bash id="apply5"
terraform apply
```

Type:

```text id="yes5"
yes
```

---

# 🎉 EXPECTED RESULT

In AWS Console:

✔ VPC created
✔ Subnet created
✔ Internet Gateway attached
✔ Route table configured
✔ Security Group created
✔ EC2 instance running

---

# 🌐 STEP 6 — Test EC2

Get Public IP:

```bash id="ip5"
terraform output
```

OR from AWS Console.

---

## SSH into instance:

```bash id="ssh5"
ssh -i key.pem ubuntu@<public-ip>
```

---

# 🧠 INTERVIEW EXPLANATION (VERY IMPORTANT)

> “In my project, I deployed EC2 instances inside a custom VPC created using Terraform. The instance was placed in a public subnet with internet gateway access. Security groups were configured to allow SSH and HTTP traffic. This setup is a typical web server architecture used in production environments.”

---

# 🔥 REAL-TIME VALUE

✔ Full-stack AWS infrastructure
✔ Networking + compute integration
✔ Production-like architecture
✔ Security group understanding
✔ Real DevOps design experience

---

# 🧯 COMMON ERRORS & FIXES

---

## ❌ Error 1: Instance not reachable

### Fix:

* Check Security Group (port 22 open)
* Check public IP enabled

---

## ❌ Error 2: No internet access

### Fix:

* Route table missing 0.0.0.0/0 → IGW

---

## ❌ Error 3: Subnet not public

### Fix:

* map_public_ip_on_launch = true

---

# 💬 INTERVIEW QUESTIONS FROM TASK 5

---

## ❓ Why attach EC2 to VPC?

> “To isolate and secure infrastructure within a controlled network environment.”

---

## ❓ What is Security Group?

> “It acts as a virtual firewall controlling inbound and outbound traffic.”

---

## ❓ Difference between Security Group and NACL?

| Security Group | NACL         |
| -------------- | ------------ |
| Instance level | Subnet level |
| Stateful       | Stateless    |

---

## ❓ Why use public subnet?

> “To allow internet-facing resources like web servers.”

---

# 🔥 REAL PROJECT EXPLANATION

> “We created a full infrastructure using Terraform where EC2 instances were deployed inside custom VPCs with proper networking, security groups, and internet access for hosting applications.”

---

# 🧠 WHAT YOU LEARNED IN TASK 5

✔ EC2 inside VPC
✔ Security Groups
✔ Real networking integration
✔ Route tables + IGW usage
✔ Production architecture
✔ Terraform multi-resource dependency

---

# 🚀 NEXT STEP (VERY IMPORTANT)

Now you are entering **intermediate → advanced DevOps level**

Next task:

# 🟠 TASK 6 — Remote Backend (S3 + DynamoDB Locking)

You will learn:

* Why state file is dangerous locally
* Remote backend setup
* S3 storage
* DynamoDB locking
* Team collaboration in Terraform

---

Just say:

👉 **TASK 6**

and we will move into **real enterprise Terraform (used in companies like AWS, Accenture, TCS, etc.)**
