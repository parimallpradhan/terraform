# 🟣 TASK 16 — Terraform with Jenkins CI/CD Pipeline

## 🎯 Goal of This Task

By the end of this task, you will:

* Automate Terraform using Jenkins
* Run init → plan → apply in pipeline
* Add approval stage (manual gate)
* Understand real production CI/CD flow
* Explain end-to-end automation in interviews

---

# 🧠 Real-Time Scenario (Interview Context)

> “In my project, we integrated Terraform with Jenkins to automate infrastructure provisioning. Every commit triggered a pipeline that ran terraform init, plan, and apply with manual approval before production deployment.”

---

# 🏗️ CI/CD ARCHITECTURE

```text id="cicdflow"
Git Push
   ↓
Jenkins Pipeline Trigger
   ↓
terraform init
   ↓
terraform validate
   ↓
terraform plan
   ↓
Manual Approval (Production Gate)
   ↓
terraform apply
   ↓
Infrastructure Created
```

---

# 🧩 STEP 1 — Install Jenkins Plugins (Important)

In Jenkins:

Go to:

* Manage Jenkins → Plugins

Install:

* Pipeline
* Git
* AWS Credentials
* Terraform (optional plugin)

---

# 🧩 STEP 2 — Configure AWS Credentials

In Jenkins:

* Manage Jenkins → Credentials
* Add:

  * AWS_ACCESS_KEY_ID
  * AWS_SECRET_ACCESS_KEY

---

# 🧩 STEP 3 — Create Jenkins Pipeline Job

* New Item → Pipeline
* Name: `terraform-cicd`

---

# ⚙️ STEP 4 — Jenkinsfile (MOST IMPORTANT)

Create `Jenkinsfile` in your repo:

```groovy id="jenkinsfile"
pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'main',
                url: 'https://github.com/your-repo/terraform-code.git'
            }
        }

        stage('Terraform Init') {
            steps {
                sh 'terraform init'
            }
        }

        stage('Terraform Validate') {
            steps {
                sh 'terraform validate'
            }
        }

        stage('Terraform Plan') {
            steps {
                sh 'terraform plan'
            }
        }

        stage('Manual Approval') {
            steps {
                input message: 'Approve Terraform Apply?'
            }
        }

        stage('Terraform Apply') {
            steps {
                sh 'terraform apply -auto-approve'
            }
        }
    }

    post {
        success {
            echo 'Terraform Deployment Successful'
        }
        failure {
            echo 'Terraform Deployment Failed'
        }
    }
}
```

---

# 🧠 STEP 5 — Pipeline Explanation

---

## 🔹 Checkout Code

Pulls Terraform code from Git

---

## 🔹 Terraform Init

Initializes backend + providers

---

## 🔹 Terraform Validate

Checks syntax errors

---

## 🔹 Terraform Plan

Shows execution plan

---

## 🔹 Manual Approval

👉 VERY IMPORTANT for production safety

---

## 🔹 Terraform Apply

Deploys infrastructure

---

# 🚀 STEP 6 — Run Pipeline

In Jenkins:

* Click Build Now

---

# 🎉 EXPECTED FLOW

```text id="jenkinsrun"
Git Push → Jenkins Trigger → Plan → Approval → Apply → AWS Infra Created
```

---

# 🧠 INTERVIEW EXPLANATION (VERY IMPORTANT)

> “In my project, we integrated Terraform with Jenkins to automate infrastructure provisioning. The pipeline included stages like init, validate, plan, and apply, with a manual approval gate before production deployment to ensure safety and control.”

---

# 🔥 REAL PROJECT BENEFITS

✔ Fully automated infrastructure
✔ No manual Terraform execution
✔ Controlled production deployments
✔ Approval-based release system
✔ CI/CD best practice

---

# 🧯 COMMON ERRORS & FIXES

---

## ❌ Error 1: Terraform not found

### Fix:

* Install Terraform on Jenkins node

---

## ❌ Error 2: AWS authentication failed

### Fix:

* Check Jenkins credentials

---

## ❌ Error 3: Plan fails in pipeline

### Fix:

* Validate Terraform locally first

---

# 💬 INTERVIEW QUESTIONS FROM TASK 16

---

## ❓ Why integrate Terraform with Jenkins?

> “To automate infrastructure provisioning as part of CI/CD pipeline.”

---

## ❓ What are pipeline stages for Terraform?

* init
* validate
* plan
* apply

---

## ❓ Why manual approval stage?

> “To prevent accidental production deployments.”

---

## ❓ What is best practice in Terraform CI/CD?

* Remote backend
* Approval gates
* Separate environments

---

# 🔥 REAL PROJECT EXPLANATION

> “In my project, we implemented Jenkins pipelines to automate Terraform deployments. Every code push triggered a pipeline that executed Terraform commands and required manual approval before applying changes to production infrastructure.”

---

# 🧠 WHAT YOU LEARNED IN TASK 16

✔ Terraform CI/CD integration
✔ Jenkins pipeline automation
✔ Approval-based deployments
✔ Production-grade workflows
✔ Real DevOps automation pattern
✔ End-to-end infrastructure delivery

---

# 🚀 NEXT STEP (VERY IMPORTANT)

Now you are entering **advanced production architecture level**

Next:

# 🟣 TASK 17 — Terraform + S3 Backend + Jenkins Integration (Real Enterprise Setup)

You will learn:

* Full production pipeline architecture
* Remote state + CI/CD integration
* Enterprise DevOps workflow
* Multi-environment automation

---

Just say:

👉 **TASK 17**

and we will move into **real enterprise-level Terraform CI/CD architecture used in top companies (Accenture, TCS, AWS environments)**.
