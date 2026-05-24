# 🟣 TASK 13 — Dynamic Blocks (Advanced Terraform Automation)

## 🎯 Goal of This Task

By the end of this task, you will:

* Understand dynamic blocks in Terraform
* Avoid repetitive code
* Create scalable security groups
* Build production-style flexible configurations
* Explain advanced Terraform logic in interviews

---

# 🧠 Real-Time Scenario (Interview Context)

> “In our project, instead of writing multiple ingress rules manually in security groups, we used dynamic blocks in Terraform to generate rules dynamically based on input variables. This reduced duplication and improved scalability.”

---

# ⚠️ WHAT IS A DYNAMIC BLOCK?

> A dynamic block allows you to generate repeated nested blocks using loops in Terraform.

---

# 🏗️ WHY WE NEED IT?

Without dynamic blocks:

* Repeated code ❌
* Hard to scale ❌
* Not reusable ❌

With dynamic blocks:

* Clean code ✅
* Scalable infra ✅
* Input-driven configuration ✅

---

# 📦 REAL ARCHITECTURE USE CASE

```text id="dynflow"
Input List (Ports)
      ↓
Terraform Dynamic Block
      ↓
Security Group Rules Generated
      ↓
EC2 Access Configured Automatically
```

---

# 🧩 STEP 1 — Create Project

```bash id="dyn1"
mkdir terraform-dynamic-blocks
cd terraform-dynamic-blocks
```

---

# ⚙️ STEP 2 — main.tf

We will create a **dynamic Security Group**

```hcl id="dyncode"
provider "aws" {
  region = "ap-south-1"
}

variable "ingress_ports" {
  type    = list(number)
  default = [22, 80, 443]
}

resource "aws_security_group" "dynamic_sg" {
  name        = "dynamic-sg"
  description = "Security group using dynamic blocks"

  vpc_id = "vpc-xxxxxxxx"  # replace with your VPC ID

  dynamic "ingress" {
    for_each = var.ingress_ports
    content {
      from_port   = ingress.value
      to_port     = ingress.value
      protocol    = "tcp"
      cidr_blocks = ["0.0.0.0/0"]
    }
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

---

# 🧠 EXPLANATION (VERY IMPORTANT)

---

## 🔹 variable ingress_ports

List of ports:

```text id="ports"
22, 80, 443
```

---

## 🔹 dynamic block

```hcl id="dynblock"
dynamic "ingress"
```

This means:

> “Create multiple ingress rules automatically.”

---

## 🔹 for_each

Loops through list of ports

---

## 🔹 ingress.value

Each value from list:

* 22
* 80
* 443

---

# 🚀 STEP 3 — Terraform Commands

```bash id="dyninit"
terraform init
```

```bash id="dynplan"
terraform plan
```

```bash id="dynapply"
terraform apply
```

Type:

```text id="yesdyn"
yes
```

---

# 🎉 RESULT

Terraform automatically creates:

* SSH rule (22)
* HTTP rule (80)
* HTTPS rule (443)

WITHOUT manual duplication.

---

# 🧠 INTERVIEW EXPLANATION (VERY IMPORTANT)

> “Dynamic blocks in Terraform are used to generate repeated nested configurations using loops. In my project, we used dynamic blocks to create multiple security group rules based on input variables, making the infrastructure scalable and reusable.”

---

# 🔥 REAL PROJECT BENEFITS

✔ No repetitive code
✔ Easy port management
✔ Environment flexibility
✔ Scalable security groups
✔ Cleaner Terraform structure

---

# 🧯 COMMON ERRORS & FIXES

---

## ❌ Error 1: Invalid for_each type

### Fix:

* Must be list or set

---

## ❌ Error 2: Missing value reference

### Fix:

* Use ingress.value correctly

---

## ❌ Error 3: VPC ID missing

### Fix:

* Replace with actual VPC ID

---

# 💬 INTERVIEW QUESTIONS FROM TASK 13

---

## ❓ What is a dynamic block?

> “A dynamic block generates repeated nested blocks in Terraform using loops.”

---

## ❓ Why use dynamic blocks?

* Avoid duplication
* Improve scalability
* Input-driven infrastructure

---

## ❓ Difference between for_each and dynamic block?

| for_each           | dynamic block          |
| ------------------ | ---------------------- |
| Used for resources | Used inside blocks     |
| Creates resources  | Creates nested configs |

---

## ❓ Real use case?

> “Creating multiple security group rules dynamically based on input list of ports.”

---

# 🔥 REAL PROJECT EXPLANATION

> “In my project, we used Terraform dynamic blocks to create security group rules dynamically based on environment requirements. This helped us avoid repetitive code and made the infrastructure highly scalable.”

---

# 🧠 WHAT YOU LEARNED IN TASK 13

✔ Dynamic blocks
✔ Loops in Terraform
✔ Scalable security groups
✔ Input-driven infrastructure
✔ Advanced Terraform logic
✔ Interview-ready explanation

---

# 🚀 NEXT STEP (VERY IMPORTANT)

Now you are entering **senior DevOps Terraform automation level**

Next:

# 🟣 TASK 14 — Terraform Data Sources (Real Cloud Integration)

You will learn:

* Fetch existing AWS resources
* Avoid hardcoding AMIs
* Real production automation patterns
* Advanced dynamic infrastructure design

---

Just say:

👉 **TASK 14**

and we will move into **real enterprise Terraform data-driven architecture used in production systems**.
