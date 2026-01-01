# 🚀 Learn Terraform

*A Complete 8-Day Hands-on Terraform Course*

This repository contains a **step-by-step Terraform training program** designed to take students from **absolute beginner** to **production-ready Terraform engineer**. The course focuses on **real-world AWS use cases**, **best practices**, and **industry-standard workflows**.

---

## 📅 Day 1: Terraform Fundamentals & Setup

### What is Terraform?

Terraform is an **Infrastructure as Code (IaC)** tool used to define, provision, and manage cloud infrastructure using code.

**Key Concepts:**

* Infrastructure as Code (IaC)
* Cloud-agnostic (AWS, Azure, GCP)
* Version-controlled infrastructure
* Repeatable & scalable deployments
* Immutable infrastructure

### Terraform Architecture

* **Terraform CLI:** The command-line tool.
* **Providers:** Plugins for cloud platforms (AWS, Azure, GCP).
* **Resources:** The building blocks (EC2, S3, VPC).
* **State file:** The `terraform.tfstate` file that tracks your infra.

### Installation & First Steps

1. **Download:** Get the binary for your OS (MacOS, Linux, Windows).
2. **Path:** Add to your system PATH and verify:
```bash
terraform version

```



### Core Workflow & Commands

1. **Write:** Create your `.tf` files.
2. **Initialize:** `terraform init` (Downloads providers).
3. **Plan:** `terraform plan` (Preview changes).
4. **Apply:** `terraform apply` (Deploy infrastructure).
5. **Destroy:** `terraform destroy` (Cleanup).

### First Code Example (AWS)

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "demo" {
  ami           = "ami-0abcd1234"
  instance_type = "t2.micro"
}

```

---

## 📅 Day 2: Variables, Outputs & State

### Terraform Variables

Variables make code reusable. Types include: `string`, `number`, `bool`, `list`, `map`, and `object`.

### Terraform Outputs

Expose important resource data to the terminal:

```hcl
output "instance_ip" {
  value = aws_instance.demo.public_ip
}

```

### Understanding State

* Tracks real-world infrastructure.
* **State Commands:** `list`, `show`, `rm`.
* **Remote State:** Using S3 for team collaboration and security.

---

## 📅 Day 3: Resources, Modules & Deployment

### Meta-Arguments

* `count` & `for_each`: Create multiple resources.
* `depends_on`: Handle manual dependencies.
* `lifecycle`: Control creation/destruction behavior.

### Terraform Modules

The most important topic for scaling.

* **DRY:** Don't Repeat Yourself.
* **Structure:** Root vs. Child modules.
* **Registry:** Using pre-built modules from the [Terraform Registry](https://registry.terraform.io).

---

## 📅 Day 4: Collaboration & Backend Configuration

### Version Control

* Using Git for Infrastructure.
* Managing `.gitignore` (excluding `.terraform/` and `.tfstate`).

### Remote Backends

* **S3 Backend:** Storing state centrally.
* **State Locking:** Using **DynamoDB** to prevent concurrent runs and state corruption.

---

## 📅 Day 5: Provisioners & Bootstrapping

### Provisioner Types

* `local-exec`: Run scripts on your local machine.
* `remote-exec`: Run scripts on the target resource.

### Configuration

* **Timing:** `when = create` vs `when = destroy`.
* **Error Handling:** Using `on_failure` to continue or fail.

---

## 📅 Day 6: Managing Environments with Workspaces

### Terraform Workspaces

Allows you to manage multiple environments (Dev, Stage, Prod) using the same code base by isolating state files.

**Commands:**

```bash
terraform workspace list
terraform workspace new dev
terraform workspace select prod

```

---

## 📅 Day 7: Security & Advanced Terraform

### Security Best Practices

* IAM Least Privilege.
* Secure State storage.
* **HashiCorp Vault:** Centralized secret management.
* Integrating Vault with Terraform to fetch dynamic credentials.

---

## 📅 Day 8: Production-Grade Terraform

### Code Quality

* `terraform fmt`: Automatic code formatting.
* `terraform validate`: Syntax and logic checks.

### CI/CD Integration

* Automating Terraform via pipelines.
* **Manual Approvals:** Ensuring safety before `apply`.
* **Summary of Best Practices:** Versioning modules, avoiding hardcoded secrets, and careful plan reviews.

---

## 🎯 Course Outcome

By the end of this course, students will be able to:

* Design Terraform projects from scratch.
* Build reusable modules.
* Manage multiple environments and remote state.
* Deploy production-ready infrastructure safely.


