# 🚀 Learn Terraform: Zero to Production

_A Complete 8-Day Hands-on Terraform Course_

This repository contains a **structured, industry-grade Terraform training program** designed to take students from **absolute beginner** to **production-ready Terraform engineer**.

The course focuses on:
- Real-world infrastructure patterns
- Industry best practices
- Interview-ready concepts
- Clean, scalable Terraform code

---

## 🎯 Course Objectives

By the end of this course, students will be able to:

- Understand Terraform internals and workflow
- Write clean and reusable Terraform code
- Manage Terraform state safely in team environments
- Design modular and scalable infrastructure
- Handle multiple environments (Dev / Stage / Prod)
- Apply Terraform securely in production
- Integrate Terraform with CI/CD pipelines
- Avoid common real-world Terraform anti-patterns

---

## 📅 Course Structure (Day-Wise)

---

## 📅 Day 1: Terraform Foundations & IaC Mindset

### What You’ll Learn
- What is Infrastructure as Code (IaC)
- Problems with manual infrastructure
- Declarative vs Imperative tools
- Terraform vs Ansible vs CloudFormation (high level)
- Terraform core components:
  - CLI
  - Providers
  - Resources
  - State file (introduction)

### Hands-On
- Install Terraform (Linux / Mac / Windows)
- `terraform init`, `plan`, `apply`, `destroy`
- First Terraform configuration
- Understanding Terraform workflow internally

---

## 📅 Day 2: Terraform Language (HCL), Variables & Outputs

### What You’ll Learn
- Terraform configuration language (HCL)
- Blocks vs arguments
- Expressions and references
- Variable types:
  - string, number, bool
  - list, map, object
- Using `.tfvars` files
- Output values
- Using `terraform console`

### Hands-On
- Parameterize infrastructure using variables
- Expose useful outputs (IPs, IDs, names)

---

## 📅 Day 3: Resources, Meta-Arguments & Lifecycle

### What You’ll Learn
- Terraform dependency graph
- Meta-arguments:
  - `count`
  - `for_each`
  - `depends_on`
  - `lifecycle`
- Resource replacement behavior
- Safe creation and deletion patterns

### Hands-On
- Create multiple resources dynamically
- Compare `count` vs `for_each`
- Control resource recreation

---

## 📅 Day 4: Terraform State & Remote Backends (Deep Dive)

### What You’ll Learn
- What Terraform state really is
- State drift and its risks
- Local vs remote state
- State locking and concurrency issues
- Backend configuration
- State commands:
  - `terraform state list`
  - `terraform state show`
  - `terraform state rm`
  - `terraform state mv`
- Importing existing infrastructure

### Hands-On
- Configure remote backend (e.g., S3)
- Enable state locking
- Import existing resources into Terraform

---

## 📅 Day 5: Terraform Modules & Code Reusability

### What You’ll Learn
- Why modules are critical in real projects
- Root module vs child modules
- Input and output flow between modules
- Writing clean, reusable modules
- Using Terraform Registry modules
- Module versioning basics

### Hands-On
- Convert existing infrastructure into a module
- Reuse the same module with different inputs

---

## 📅 Day 6: Managing Multiple Environments (Industry Approach)

### What You’ll Learn
- Dev / Stage / Prod separation strategies
- Folder-based environment structure (industry standard)
- Environment-specific `tfvars`
- Why Terraform workspaces are limited
- When workspaces are acceptable
- Environment isolation best practices

### Hands-On
- Create separate environment folders
- Use the same modules across environments
- Configure separate backends per environment

---

## 📅 Day 7: Security, Secrets & Terraform Best Practices

### What You’ll Learn
- IAM least-privilege principles
- Avoiding hardcoded secrets
- Securing Terraform state
- Using environment variables for credentials
- Sensitive variables
- Introduction to secret management (Vault – conceptual)

### Hands-On
- Mark variables as sensitive
- Secure backend access
- Follow security-first Terraform patterns

---

## 📅 Day 8: Production-Grade Terraform & CI/CD

### What You’ll Learn
- Running Terraform in CI/CD pipelines
- Separating `plan` and `apply`
- Manual approvals for production
- Storing and reviewing plan artifacts
- Terraform formatting and validation
- Versioning Terraform and providers
- Common Terraform anti-patterns

### Provisioners (Awareness)
- What provisioners are
- Types: `local-exec`, `remote-exec`
- Why provisioners are discouraged in production
- Recommended alternatives:
  - cloud-init / user_data
  - Packer
  - CI/CD pipelines

---

## 🧱 Running Project (Throughout the Course)

Students will gradually build a **real-world Terraform project**:

- Provision core infrastructure
- Convert code into reusable modules
- Configure remote backend
- Separate environments
- Secure credentials
- Simulate production workflow

---

## 🎓 Who This Course Is For

- Beginners learning Terraform from scratch
- DevOps engineers upgrading to production-grade Terraform
- Cloud engineers preparing for interviews
- Anyone who wants to manage infrastructure professionally using code

---

## ⭐ Final Outcome

After completing this course, students will be able to:
- Confidently design Terraform projects
- Write clean, maintainable Terraform code
- Work with teams safely using remote state
- Deploy infrastructure following real-world best practices
- Answer Terraform interview questions with confidence

---

🚀 **Welcome to your Terraform journey — from Zero to Production.**
