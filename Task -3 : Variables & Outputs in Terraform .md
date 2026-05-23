# 🟡 TASK 3 — Variables & Outputs in Terraform

## 🎯 Goal of This Task

By the end of this task, you will:

* Understand input variables in Terraform
* Remove hardcoding from code
* Pass dynamic values at runtime
* Understand output values
* Build reusable Terraform configurations
* Be able to explain this confidently in interviews

---

# 🧠 Real-Time Scenario (Interview Context)

> “In my project, we avoided hardcoding values like AMI IDs and instance types. Instead, we used Terraform variables and tfvars files to make infrastructure reusable across dev, QA, and production environments.”

---

# 🏗️ Why Variables Are Important?

Without variables:

* Code is static ❌
* Not reusable ❌
* Hard to manage environments ❌

With variables:

* Dynamic infrastructure ✅
* Environment-based deployment ✅
* Reusable modules ✅
* Industry best practice ✅

---

# 📦 TASK FLOW

```text id="flow3"
variables.tf → Define Inputs
        ↓
terraform.tfvars → Provide Values
        ↓
main.tf → Use Variables
        ↓
outputs.tf → Display Results
        ↓
terraform apply
```

---

# 🛠️ STEP 1 — Create Project Directory

```bash id="task3dir"
mkdir terraform-ec2-variables
cd terraform-ec2-variables
```

---

# 📄 STEP 2 — Create Variables File

```bash id="varfile"
vim variables.tf
```

### Add this:

```hcl id="varcode"
variable "region" {
  description = "AWS region"
  type        = string
  default     = "ap-south-1"
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "ami_id" {
  description = "AMI ID for EC2"
  type        = string
}
```

---

# 🧠 Explanation

## 🔹 variable "region"

Defines AWS region dynamically

## 🔹 variable "instance_type"

Defines EC2 size (t2.micro, t2.small, etc.)

## 🔹 variable "ami_id"

No default → must be passed externally

---

# 📄 STEP 3 — Create terraform.tfvars File

```bash id="tfvars"
vim terraform.tfvars
```

### Add:

```hcl id="tfvarsdata"
region        = "ap-south-1"
instance_type = "t2.micro"
ami_id        = "ami-0f58b397bc5c1f2e8"
```

---

# 📄 STEP 4 — Create main.tf

```bash id="mainfile"
vim main.tf
```

### Add:

```hcl id="maincode"
provider "aws" {
  region = var.region
}

resource "aws_instance" "devops_ec2" {
  ami           = var.ami_id
  instance_type = var.instance_type

  tags = {
    Name = "Terraform-EC2-Variable"
  }
}
```

---

# 📤 STEP 5 — Create outputs.tf

```bash id="outputfile"
vim outputs.tf
```

### Add:

```hcl id="outputcode"
output "instance_id" {
  value = aws_instance.devops_ec2.id
}

output "public_ip" {
  value = aws_instance.devops_ec2.public_ip
}
```

---

# 🧠 What Outputs Do?

Outputs show:

* Instance ID
* Public IP
* Resource details after deployment

---

# 🚀 STEP 6 — Terraform Workflow

## Initialize

```bash id="init3"
terraform init
```

---

## Validate

```bash id="validate3"
terraform validate
```

---

## Plan

```bash id="plan3"
terraform plan
```

---

## Apply

```bash id="apply3"
terraform apply
```

Type:

```text id="yes3"
yes
```

---

# 🎉 EXPECTED OUTPUT

After apply:

```text id="output3"
instance_id = i-0abcd1234
public_ip   = 3.110.xxx.xxx
```

---

# 🧠 INTERVIEW EXPLANATION

> “We used Terraform variables to avoid hardcoding values like AMI ID, region, and instance type. This allowed us to reuse the same code across multiple environments like dev, QA, and production by simply changing tfvars files. Outputs were used to fetch instance details like public IP and instance ID after provisioning.”

---

# 🔥 REAL-TIME BENEFITS

✔ Environment flexibility
✔ Code reusability
✔ Secure configuration
✔ Easy maintenance
✔ Standard DevOps practice

---

# 🧯 COMMON ERRORS & FIXES

---

## ❌ Error 1: Missing variable value

```text id="err1"
No value for required variable
```

### Fix:

* Add value in terraform.tfvars

---

## ❌ Error 2: Invalid AMI

```text id="err2"
InvalidAMIID.NotFound
```

### Fix:

* Use correct region AMI

---

## ❌ Error 3: Variable not defined

```text id="err3"
Reference to undeclared variable
```

### Fix:

* Ensure variable exists in variables.tf

---

# 💬 INTERVIEW QUESTIONS FROM TASK 3

---

## ❓ What are Terraform variables?

> “Variables allow dynamic input values instead of hardcoding in configuration files.”

---

## ❓ What is tfvars file?

> “tfvars file is used to define variable values externally for different environments.”

---

## ❓ Why use outputs?

> “Outputs are used to extract useful information like instance ID, IP address after provisioning.”

---

## ❓ Advantage of variables?

* Reusability
* Flexibility
* Environment management
* Clean code

---

# 🔥 REAL PROJECT EXAMPLE YOU CAN SAY

> “In my project, we used variables and tfvars files to manage multiple environments. For example, dev and production environments used different instance types and AMIs, but same Terraform codebase.”

---

# 🧠 WHAT YOU LEARNED IN TASK 3

✔ Input variables
✔ tfvars usage
✔ Output values
✔ Dynamic infrastructure
✔ Environment-based deployment
✔ Production-level Terraform structure

---

# 🚀 NEXT STEP

Now you are moving from basic → intermediate → real DevOps level.

Next is very important:

# 🟠 TASK 4 — Create VPC using Terraform (REAL NETWORKING SETUP)

You will learn:

* VPC creation
* Subnets (public/private)
* Internet Gateway
* Route tables
* Real AWS networking
* Interview architecture questions

---

Just say:

👉 **TASK 4**

and we will build a full real-world AWS network like production DevOps engineers.
