# Learn Terraform  🚀
_A Complete 8-Day Hands-on Terraform Course_

This repository contains a **step-by-step Terraform training program** designed to take students from **absolute beginner** to **production-ready Terraform engineer**.  
The course focuses on **real-world AWS use cases**, **best practices**, and **industry-standard workflows**.

---

## 📅 Day 1: Terraform Fundamentals & Setup

### What is Terraform?
Terraform is an **Infrastructure as Code (IaC)** tool used to define, provision, and manage cloud infrastructure using code.

Key concepts:
- Infrastructure as Code (IaC)
- Cloud-agnostic (AWS, Azure, GCP)
- Version-controlled infrastructure
- Repeatable & scalable deployments
- Immutable infrastructure

### Terraform Architecture
Understanding the core components:
- Terraform CLI
- Providers (AWS, Azure, GCP)
- Resources
- State file (`terraform.tfstate`)

### Installing Terraform (MacOS, Linux, Windows)
Steps covered:
- Download Terraform binary
- Add Terraform to PATH
- Verify installation
```bash
terraform version

Terraform Core Commands

Learn the Terraform lifecycle:

terraform init – Initialize the working directory

terraform plan – Preview infrastructure changes

terraform apply – Deploy infrastructure

terraform destroy – Remove infrastructure


Terraform File Structure

Basic files used in a Terraform project:

provider.tf

main.tf

variables.tf

outputs.tf


Writing Your First Terraform Code

Create your first Terraform configuration using AWS:

provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "demo" {
  ami           = "ami-0abcd1234"
  instance_type = "t2.micro"
}

Terraform Workflow

Typical Terraform workflow:

1. Write Terraform code


2. terraform init


3. terraform plan


4. terraform apply




---

📅 Day 2: Variables, Outputs & State

Terraform Variables

Variables make Terraform configurations reusable and dynamic.

Usage example:

instance_type = var.instance_type

Variable Types

string

number

bool

list(string)

map(string)

object


Terraform Outputs

Expose important resource information:

output "instance_ip" {
  value = aws_instance.demo.public_ip
}

Terraform State File

Stored as terraform.tfstate

Tracks real infrastructure

Contains sensitive data

Should never be edited manually


Terraform State Commands

terraform state list

terraform state show <resource>

terraform state rm <resource>


Remote State (Recommended)

Store state remotely (S3)

Enable encryption

Enable versioning

Prevent state corruption



---

📅 Day 3: Resources, Modules & Deployment

Resource Dependencies

Terraform automatically detects dependencies. Manual dependency:

depends_on = [aws_security_group.sg]

Meta-Arguments

count

for_each

depends_on

lifecycle


Lifecycle Rules

Control resource behavior:

lifecycle {
  prevent_destroy = true
  ignore_changes  = [tags]
}

Terraform Modules (Most Important Topic)

Reusable infrastructure components

Cleaner and maintainable code

DRY principle (Don’t Repeat Yourself)


Module structure:

modules/ec2/
├── main.tf
├── variables.tf
└── outputs.tf

Using Terraform Registry

https://registry.terraform.io

VPC modules

EC2 modules

EKS modules

RDS modules


Environment Separation

dev

stage

prod



---

📅 Day 4: Collaboration & Backend Configuration

Git & Version Control

Infrastructure as code with Git

Branching strategy

Code reviews for Terraform


Handling Sensitive Data

Use .gitignore

Never commit:

.terraform/

terraform.tfstate

*.tfvars



Terraform Backends

Local backend

Remote backend

Why backends are important


S3 Backend Configuration

Centralized state storage

Team collaboration

Secure access


State Locking with DynamoDB

Prevent concurrent Terraform runs

Ensure state consistency



---

📅 Day 5: Provisioners & Bootstrapping

Understanding Provisioners

Execute scripts on resources

Use only when necessary


Local-exec & Remote-exec

local-exec: runs locally

remote-exec: runs on remote resource


Provisioners on Create & Destroy

when = create

when = destroy


Failure Handling

on_failure

Timeouts

Retry behavior



---

📅 Day 6: Managing Environments with Workspaces

Terraform Workspaces

Multiple environments using same code

Separate state per workspace


Workspace Commands

terraform workspace list

terraform workspace new dev

terraform workspace select prod


Workspaces vs Folder-based Environments

Pros and cons

When to use which approach



---

📅 Day 7: Security & Advanced Terraform

Terraform Security Best Practices

Least privilege IAM

Secure state storage

Secret management


HashiCorp Vault Overview

Centralized secrets management

Dynamic credentials


Integrating Terraform with Vault

Vault provider

Fetch secrets securely

Avoid secrets in code and state



---

📅 Day 8: Production-Grade Terraform & Best Practices

Industry-Standard Project Structure

Root modules

Child modules

Environment isolation


Code Quality & Validation

terraform fmt

terraform validate

Clean, readable code


Terraform in CI/CD

Plan & apply separation

Manual approvals

Automated deployments


Terraform Best Practices Summary

Use modules everywhere

One module = one responsibility

Version your modules

Always use remote state

Never hardcode secrets

Review terraform plan carefully



---

🎯 Course Outcome

By the end of this course, students will be able to:

Design Terraform projects from scratch

Build reusable modules

Manage multiple environments

Work with remote state safely

Deploy production-ready infrastructure



---

⭐ Happy Terraforming!

---

If you want next:
- 📁 **Exact repo folder structure**
- 🧪 **Day-wise labs**
- 🎓 **Interview questions**
- 🏆 **Real production Terraform examples**

Just say the word 👌
