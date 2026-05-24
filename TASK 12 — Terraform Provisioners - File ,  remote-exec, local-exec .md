# 🟣 TASK 12 — Terraform Provisioners (File, remote-exec, local-exec)

## 🎯 Goal of This Task

By the end of this task, you will:

* Understand Terraform provisioners
* Run scripts after resource creation
* Copy files to EC2
* Execute remote commands on EC2
* Trigger local automation
* Explain real-world post-deployment automation

---

# 🧠 Real-Time Scenario (Interview Context)

> “In my project, after provisioning EC2 instances using Terraform, we used provisioners to automatically install required packages and deploy application setup scripts without manual intervention.”

---

# ⚠️ WHAT ARE PROVISIONERS?

> Provisioners allow you to execute scripts or commands on a local machine or remote instance after resource creation.

---

# 🏗️ PROVISIONER TYPES

| Type        | Purpose                       |
| ----------- | ----------------------------- |
| file        | Copy files to remote server   |
| remote-exec | Run commands on remote server |
| local-exec  | Run commands on local machine |

---

# 📦 REAL ARCHITECTURE FLOW

```text id="provflow"
Terraform Apply
      ↓
EC2 Created in AWS
      ↓
Provisioner Triggered
      ↓
File copied / Script executed
      ↓
Server configured automatically
```

---

# 🧩 STEP 1 — Create Project

```bash id="prov1"
mkdir terraform-provisioners
cd terraform-provisioners
```

---

# ⚙️ STEP 2 — main.tf (EC2 + Provisioners)

```hcl id="provcode"
provider "aws" {
  region = "ap-south-1"
}

resource "aws_instance" "web" {
  ami           = "ami-0f58b397bc5c1f2e8"
  instance_type = "t2.micro"
  key_name      = "my-key"  # REQUIRED for SSH

  connection {
    type        = "ssh"
    user        = "ubuntu"
    private_key = file("my-key.pem")
    host        = self.public_ip
  }

  provisioner "file" {
    source      = "script.sh"
    destination = "/home/ubuntu/script.sh"
  }

  provisioner "remote-exec" {
    inline = [
      "chmod +x /home/ubuntu/script.sh",
      "sudo bash /home/ubuntu/script.sh"
    ]
  }

  tags = {
    Name = "Provisioner-EC2"
  }
}
```

---

# 📄 STEP 3 — Create Script File

```bash id="scriptfile"
vim script.sh
```

### Add:

```bash id="script"
#!/bin/bash
sudo apt update -y
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
echo "Terraform Provisioner Success" > /var/www/html/index.html
```

---

# 🧠 EXPLANATION

---

## 🔹 file provisioner

* Copies script to EC2

---

## 🔹 remote-exec

* Runs commands on EC2 after creation

---

## 🔹 connection block

* Defines SSH connection to EC2

---

# 🚀 STEP 4 — Terraform Commands

```bash id="provinit"
terraform init
```

```bash id="provplan"
terraform plan
```

```bash id="provapply"
terraform apply
```

Type:

```text id="yesprov"
yes
```

---

# 🎉 EXPECTED RESULT

After apply:

* EC2 created
* Nginx installed automatically
* Web server running
* Page accessible via public IP

---

# 🌐 TEST OUTPUT

Open browser:

```text id="testurl"
http://<public-ip>
```

You should see:

```
Terraform Provisioner Success
```

---

# 🧠 INTERVIEW EXPLANATION (VERY IMPORTANT)

> “Terraform provisioners are used to execute scripts on local or remote machines after resource creation. In my project, we used remote-exec provisioners to install Nginx and configure servers automatically after EC2 provisioning.”

---

# 🔥 REAL USE CASES

✔ Software installation after EC2 creation
✔ Bootstrapping servers
✔ Running configuration scripts
✔ Application deployment automation
✔ Post-deployment setup

---

# ⚠️ BEST PRACTICE (IMPORTANT INTERVIEW POINT)

👉 Provisioners are NOT recommended for large-scale production automation

Instead use:

* Ansible
* User data scripts
* Configuration management tools

---

# 🧯 COMMON ERRORS & FIXES

---

## ❌ Error 1: SSH connection failed

### Fix:

* Check security group (port 22 open)
* Verify key pair
* Correct private key path

---

## ❌ Error 2: Permission denied

### Fix:

```bash id="permfix"
chmod 400 my-key.pem
```

---

## ❌ Error 3: Script not executing

### Fix:

* Check file path
* Ensure executable permission

---

# 💬 INTERVIEW QUESTIONS FROM TASK 12

---

## ❓ What are Terraform provisioners?

> “Provisioners are used to execute scripts or commands on local or remote machines after resource creation.”

---

## ❓ Types of provisioners?

* file
* remote-exec
* local-exec

---

## ❓ Why are provisioners not preferred?

* Not idempotent
* Hard to manage at scale
* Better tools exist (Ansible, cloud-init)

---

## ❓ Real use case?

> “Used for bootstrapping EC2 instances after provisioning.”

---

# 🔥 REAL PROJECT EXPLANATION

> “In my project, we used Terraform remote-exec provisioners to install and configure Nginx automatically after EC2 creation, enabling quick server readiness for application deployment.”

---

# 🧠 WHAT YOU LEARNED IN TASK 12

✔ File provisioner
✔ Remote-exec
✔ Local-exec
✔ Post-deployment automation
✔ SSH-based execution
✔ Real DevOps automation flow

---

# 🚀 NEXT STEP (IMPORTANT)

Now you are moving into **advanced Terraform logic level**

Next:

# 🟣 TASK 13 — Dynamic Blocks (Advanced Infrastructure Automation)

You will learn:

* Dynamic resource creation
* Loops in Terraform
* Clean scalable architecture
* Real production patterns

---

Just say:

👉 **TASK 13**

and we will move into **advanced Terraform coding patterns used in enterprise DevOps systems**.
