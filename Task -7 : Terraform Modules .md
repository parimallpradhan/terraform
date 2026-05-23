**Task 7 (Terraform Modules)** properly, we should not stop at explaining modules. We should actually build the complete project structure:

```text
terraform-project/
│
├── main.tf
├── variables.tf
├── outputs.tf
│
└── modules/
    ├── vpc/
    ├── ec2/
    └── security-group/
```

Let's implement the complete structure step-by-step.

---

# 🟣 Project Goal

Create:

* VPC
* Public Subnet
* Internet Gateway
* Route Table
* Security Group
* EC2 Instance

Using reusable Terraform modules.

---

# 📁 Directory Structure

```text
terraform-project/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── terraform.tfvars
│
└── modules/
    │
    ├── vpc/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    ├── security-group/
    │   ├── main.tf
    │   ├── variables.tf
    │   └── outputs.tf
    │
    └── ec2/
        ├── main.tf
        ├── variables.tf
        └── outputs.tf
```

---

# STEP 1 — Create Folder Structure

```bash
mkdir -p terraform-project/modules/{vpc,security-group,ec2}

cd terraform-project

touch main.tf variables.tf outputs.tf terraform.tfvars

touch modules/vpc/{main.tf,variables.tf,outputs.tf}

touch modules/security-group/{main.tf,variables.tf,outputs.tf}

touch modules/ec2/{main.tf,variables.tf,outputs.tf}
```

---

# 🟣 MODULE 1 — VPC MODULE

---

## modules/vpc/variables.tf

```hcl
variable "vpc_cidr" {}
variable "subnet_cidr" {}
variable "availability_zone" {}
```

---

## modules/vpc/main.tf

```hcl
resource "aws_vpc" "this" {
  cidr_block = var.vpc_cidr

  tags = {
    Name = "demo-vpc"
  }
}

resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.this.id
  cidr_block              = var.subnet_cidr
  availability_zone       = var.availability_zone
  map_public_ip_on_launch = true

  tags = {
    Name = "public-subnet"
  }
}

resource "aws_internet_gateway" "igw" {
  vpc_id = aws_vpc.this.id
}

resource "aws_route_table" "public" {
  vpc_id = aws_vpc.this.id

  route {
    cidr_block = "0.0.0.0/0"
    gateway_id = aws_internet_gateway.igw.id
  }
}

resource "aws_route_table_association" "public" {
  subnet_id      = aws_subnet.public.id
  route_table_id = aws_route_table.public.id
}
```

---

## modules/vpc/outputs.tf

```hcl
output "vpc_id" {
  value = aws_vpc.this.id
}

output "subnet_id" {
  value = aws_subnet.public.id
}
```

---

# 🟣 MODULE 2 — SECURITY GROUP MODULE

---

## modules/security-group/variables.tf

```hcl
variable "vpc_id" {}
```

---

## modules/security-group/main.tf

```hcl
resource "aws_security_group" "web_sg" {
  name   = "web-sg"
  vpc_id = var.vpc_id

  ingress {
    description = "SSH"

    from_port = 22
    to_port   = 22

    protocol = "tcp"

    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    description = "HTTP"

    from_port = 80
    to_port   = 80

    protocol = "tcp"

    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port = 0
    to_port   = 0

    protocol = "-1"

    cidr_blocks = ["0.0.0.0/0"]
  }
}
```

---

## modules/security-group/outputs.tf

```hcl
output "security_group_id" {
  value = aws_security_group.web_sg.id
}
```

---

# 🟣 MODULE 3 — EC2 MODULE

---

## modules/ec2/variables.tf

```hcl
variable "ami_id" {}
variable "instance_type" {}
variable "subnet_id" {}
variable "security_group_id" {}
```

---

## modules/ec2/main.tf

```hcl
resource "aws_instance" "app" {
  ami           = var.ami_id
  instance_type = var.instance_type

  subnet_id = var.subnet_id

  vpc_security_group_ids = [
    var.security_group_id
  ]

  tags = {
    Name = "terraform-app"
  }
}
```

---

## modules/ec2/outputs.tf

```hcl
output "instance_id" {
  value = aws_instance.app.id
}

output "public_ip" {
  value = aws_instance.app.public_ip
}
```

---

# 🟣 ROOT MODULE

---

## variables.tf

```hcl
variable "region" {}
variable "ami_id" {}
variable "instance_type" {}
```

---

## terraform.tfvars

Replace AMI according to your region.

```hcl
region        = "ap-south-1"
ami_id        = "ami-xxxxxxxxxxxx"
instance_type = "t2.micro"
```

---

## main.tf

```hcl
provider "aws" {
  region = var.region
}

module "vpc" {
  source = "./modules/vpc"

  vpc_cidr          = "10.0.0.0/16"
  subnet_cidr       = "10.0.1.0/24"
  availability_zone = "ap-south-1a"
}

module "security_group" {
  source = "./modules/security-group"

  vpc_id = module.vpc.vpc_id
}

module "ec2" {
  source = "./modules/ec2"

  ami_id            = var.ami_id
  instance_type     = var.instance_type
  subnet_id         = module.vpc.subnet_id
  security_group_id = module.security_group.security_group_id
}
```

---

## outputs.tf

```hcl
output "instance_id" {
  value = module.ec2.instance_id
}

output "public_ip" {
  value = module.ec2.public_ip
}

output "vpc_id" {
  value = module.vpc.vpc_id
}
```

---

# 🚀 Deploy

Initialize:

```bash
terraform init
```

Validate:

```bash
terraform validate
```

Preview:

```bash
terraform plan
```

Deploy:

```bash
terraform apply
```

Destroy:

```bash
terraform destroy
```

---

# 🧠 Interview Explanation

> "To improve reusability and maintainability, I structured Terraform code into separate modules for VPC, Security Groups, and EC2. The root module orchestrates these components by passing outputs from one module as inputs to another. This modular approach allows teams to reuse infrastructure components across multiple environments and simplifies maintenance."

This implementation is much closer to what you'd be expected to build and explain in a real DevOps project than a single flat Terraform file.
