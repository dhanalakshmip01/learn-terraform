Excellent 👍 Day 9 is where students move from **"Terraform user" → "Production Terraform Engineer"**.

Below are **clean, structured, production-grade notes** matching your teaching style and previous days.

You can directly use this as **training material / README / slides content**.

---

# 📅 Day 9 – Environments, Security & Best Practices

## Production-Grade Terraform Practices

---

# 📌 Overview

Until now, we learned how to:

* Write Terraform code
* Use modules
* Scale resources
* Manage state

On Day 9, we focus on:

> **How to run Terraform safely in real production environments.**

This includes:

* Multi-environment management
* Security practices
* Avoiding dangerous patterns
* Industry best practices

---

# 🔹 Part 1 — Managing Multiple Environments

---

## What Is an Environment?

An environment represents a deployment stage:

```
dev → qa → staging → prod
```

Each environment usually differs in:

* Resource size
* Replicas
* Permissions
* Cost
* Risk level

---

## Why Environments Matter

You never deploy directly to production.

You follow:

```
Test → Validate → Promote → Deploy
```

Terraform must support this workflow safely.

---

# 🔹 Part 2 — Terraform Workspaces (And Their Limitations)

---

## What Are Workspaces?

Workspaces allow multiple state files in the same directory.

Example:

```bash
terraform workspace new dev
terraform workspace new prod
```

Terraform stores separate state per workspace.

---

## How Workspaces Work

Same codebase:

```
main.tf
```

Different state files:

```
dev state
prod state
```

---

## Why Workspaces Are Limited For Production

---

### ❌ Risk of Human Error

Easy to apply prod changes accidentally.

---

### ❌ Same Backend Shared

All environments use:

* Same bucket
* Same IAM permissions

---

### ❌ Hard CI/CD Integration

Pipelines must manage workspace switching.

---

## When Workspaces Are Acceptable

Use for:

* Learning
* Sandbox
* Temporary testing

Avoid for:

* Production systems
* Enterprise environments

---

# 🔹 Part 3 — Folder-Based Environment Structure (Industry Standard)

---

## Recommended Folder Structure

```
terraform/
│
├── modules/
│   ├── network/
│   ├── compute/
│   └── database/
│
└── environments/
    ├── dev/
    ├── qa/
    └── prod/
```

---

## How This Works

Each environment has:

* Its own backend
* Its own state
* Its own tfvars

But all reuse the same modules.

---

## Example Deployment Flow

---

### Deploy Dev

```bash
cd environments/dev
terraform apply
```

---

### Deploy Prod

```bash
cd environments/prod
terraform apply
```

---

## Benefits

---

| Feature             | Benefit             |
| ------------------- | ------------------- |
| State isolation     | No cross-env risk   |
| Security separation | Different IAM roles |
| CI/CD friendly      | Easy pipelines      |
| Audit safe          | Clear boundaries    |

---

# 🔹 Part 4 — Environment-Specific tfvars

---

## What Are tfvars Used For?

tfvars allow:

> Same Terraform code with different environment values.

---

## Example

### dev.tfvars

```hcl
instance_type = "t3.micro"
replicas      = 1
```

---

### prod.tfvars

```hcl
instance_type = "t3.large"
replicas      = 3
```

---

Terraform code stays same.

Only values change.

---

# 🔹 Part 5 — Terraform Security Best Practices

---

## ✅ Use Remote Backend

Never store production state locally.

Use:

* S3 + DynamoDB
* Azure Blob
* Terraform Cloud

---

## ✅ Enable State Locking

Prevents:

* Parallel apply
* State corruption

---

## ✅ Encrypt State Storage

Enable encryption on:

* S3 buckets
* Blob storage

---

## ✅ Restrict IAM Permissions

Use:

* Least privilege principle
* Separate roles for dev and prod

---

## ✅ Never Store Secrets In Code

Avoid:

❌ Hardcoded secrets
❌ tfvars secrets
❌ Git commits

Use:

* AWS Secrets Manager
* Azure Key Vault
* Vault

---

## ✅ Protect Critical Resources

Use lifecycle rules:

```hcl
prevent_destroy = true
```

For:

* Databases
* Storage
* DNS zones

---

# 🔹 Part 6 — Terraform Anti-Patterns (What NOT To Do)

---

## ❌ Using Local State in Production

Causes:

* Team conflicts
* Data loss
* No locking

---

## ❌ Mixing Environments in One State

Never deploy:

```
dev + prod in same state
```

---

## ❌ Hardcoding Values

Avoid:

```
Subnet IDs
Passwords
AMI IDs
```

Use variables and data sources.

---

## ❌ Large Monolithic Terraform Files

Instead:

* Use modules
* Separate responsibilities

---

# 🔹 Part 7 — Provisioners (Why To Avoid)

---

## What Are Provisioners?

Provisioners execute scripts after resource creation.

Example:

* Install packages
* Copy files

---

## Why They Are Discouraged

---

### ❌ Not Declarative

Terraform is declarative. Provisioners are imperative.

---

### ❌ Unreliable

* SSH failures
* Network issues
* Script errors

---

### ❌ Not Idempotent

Re-running Terraform can break systems.

---

## What To Use Instead

---

| Use Case      | Recommended Tool |
| ------------- | ---------------- |
| Bootstrapping | cloud-init       |
| App config    | Ansible          |
| Custom images | Packer           |
| Secrets       | Secret managers  |

---

# 🔹 Part 8 — Hands-On Lab Concepts

---

## Lab 1 — Create Dev/Prod Environments

Students should:

* Create folder-based env structure
* Use same module
* Apply different tfvars

---

## Lab 2 — Apply Security Best Practices

Tasks:

* Use remote backend
* Enable locking
* Enable encryption
* Restrict permissions

---

## Lab 3 — Validate Code

---

### terraform fmt

Formats code automatically.

```bash
terraform fmt
```

---

### terraform validate

Checks syntax and config correctness.

```bash
terraform validate
```

---

# 🔹 Outcome Of Day 9

After completing Day 9, students will:

---

✅ Design multi-environment Terraform projects
✅ Understand production risks
✅ Apply security best practices
✅ Avoid Terraform anti-patterns
✅ Build enterprise-grade IaC setups

---

# 🎯 Final Summary (Teaching Notes)

---

### Environments

> Always isolate environments with separate state.

---

### Security

> Terraform state is sensitive — protect it.

---

### Best Practices

> Use modules, remote backend, locking, least privilege.

---

### Anti-patterns

> Avoid local state, hardcoding, provisioners.

---

# 🧠 One Line Summary

> Day 9 teaches how to run Terraform safely, securely, and professionally in real production environments.

---

If you'd like, I can also provide:

* Day 9 lab assignment
* Real folder structure Git repo
* Production checklist PDF
* Interview questions for Day 9 topics
* CI/CD pipeline integration example
* Teaching slides outline

Just say 👍
