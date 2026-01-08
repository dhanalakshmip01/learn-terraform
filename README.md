# 🚀 Learn Terraform: Zero to Production

_A Complete 14-Day Hands-on Terraform Course_

This course is designed to take you from **absolute beginner** to **production-ready Terraform engineer**.  
You will not just learn Terraform commands — you will learn **how Terraform is used in industries**.

The course follows a **clear structure**:
- 12 days of core Terraform concepts
- 1 full project day
- 1 full interview-preparation day

---

## 🎯 Course Objectives

By the end of this course, you will be able to:

- Understand **why Terraform exists** and what problems it solves
- Write **clean, readable, and reusable Terraform code**
- Understand Terraform **internals and workflow**
- Manage Terraform **state safely in team environments**
- Design **modular and scalable infrastructure**
- Handle **multiple environments** (dev / prod)
- Apply **security best practices** in Terraform
- Avoid **real-world Terraform anti-patterns**
- Confidently answer **Terraform interview questions**
- Build and explain **one complete Terraform project**

---

## ⏱️ Course Format

- **Duration:** 14 Days  
- **Class time:** **~1.5 hours per day**
- **Audience:** Beginners (cloud basics already completed)  
- **Approach:** Concept → Hands-on → Real-world relevance  

---

## 📅 Course Structure (Day-wise)

---

## 📅 Day 1 – Terraform Foundations & IaC Basics

### You Will Learn
- What is Infrastructure as Code (IaC)
- Problems with manual infrastructure
- Declarative vs Imperative approach
- What Terraform is (and what it is NOT)
- Terraform vs Ansible vs CloudFormation (high-level)
- Terraform alternatives (CloudFormation, ARM/Bicep, Pulumi)

### Hands-On
- Install Terraform on Windows / Linux / Mac
- Verify installation using `terraform version`
- Understand Terraform binary and PATH
- Run first basic Terraform command

### Outcome
You understand **why Terraform is needed and how to install it**.



## 📅 Day 2: The Terraform Core (Syntax, Logic & State)

**Goal:** Understand how Terraform speaks, how it connects to the cloud, and how it remembers what it built.

### 📚 Learning Objectives:

* **HCL Syntax Foundations:** Understanding Blocks vs. Arguments.
* **Provider Ecosystem:** How Terraform interacts with Cloud APIs (AWS, Azure, GCP).
* **Terraform State Deep-Dive:** Understanding the `.tfstate` file as the "Source of Truth."
* **The 4-Step Core Workflow:** Mastering `init`, `plan`, `apply`, and `destroy`.

---

## 📅 Day 3: Practical Deployment & Professional Structure

**Goal:** Build infrastructure, manage resource relationships, and organize code like a DevOps Pro.

### 📚 Learning Objectives:

* **The "First Build" Demo:** Deploying a live EC2 Instance from scratch.
* **Resource Interdependency:** Mastering **Resource References** (`type.name.attribute`) to link Security Groups and Instances.
* **Internal Inspection:** Using `terraform console` to query live resource data and the State file.
* **Production File Structure:** Breaking down a single `main.tf` into specialized files:
* `provider.tf`
* `main.tf`
* `variables.tf`
* `outputs.tf`


---

## 📅 Day 4 – Variables in Terraform (Deep Dive)

### You Will Learn
- Input variables
- Variable types (string, number, bool, list, map, object)
- Default values
- `terraform.tfvars`
- Variable precedence
- Local values (`locals`)

### Hands-On
- Convert hardcoded values into variables
- Use `.tfvars` files
- Use `locals` for reuse

### Outcome
You can **parameterize infrastructure properly**.

---

## 📅 Day 5 – Outputs & Terraform Dependency Graph

### You Will Learn
- What output values are
- Why outputs are important
- Terraform dependency graph
- Implicit dependencies
- Execution order

### Hands-On
- Create output values
- Observe dependency resolution using `terraform plan`

### Outcome
You understand **resource relationships and flow**.

---

## 📅 Day 6 – Resources & Meta-Arguments

### You Will Learn
- Resource lifecycle basics
- Meta-arguments:
  - `count`
  - `for_each`
- Difference between `count` and `for_each`

### Hands-On
- Create multiple resources using `count`
- Repeat using `for_each`
- Compare behaviors

### Outcome
You can **scale resources dynamically**.

---

## 📅 Day 7 – Dependencies & Lifecycle Rules

### You Will Learn
- `depends_on`
- `lifecycle` block
- Resource replacement behavior

### Hands-On
- Add explicit dependencies
- Prevent accidental resource deletion
- Control recreation using lifecycle rules

### Outcome
You can **manage safe infrastructure changes**.

---

## 📅 Day 8 – Terraform State (Core Concepts)

### You Will Learn
- What Terraform state is
- Why state is critical
- State drift
- Risks of local state

### Hands-On
- Inspect `terraform.tfstate`
- Understand state vs real resources

### Outcome
You understand **Terraform’s most critical concept**.

---

## 📅 Day 9 – Remote Backend & State Locking

### You Will Learn
- Local vs remote state
- Remote backend benefits
- State locking
- Concurrent runs problem

### Hands-On
- Configure remote backend (conceptual or real)
- Observe state locking behavior

### Outcome
You understand **team-safe Terraform usage**.

---

## 📅 Day 10 – Advanced State Management & Import

### You Will Learn
- `terraform state` commands
- `terraform import`
- Managing existing infrastructure

### Hands-On
- Use `state list` and `state show`
- Import an existing resource into Terraform

### Outcome
You can **handle real-world Terraform scenarios**.

---

## 📅 Day 11 – Terraform Modules (Core Concepts)

### You Will Learn
- What modules are
- Root vs child modules
- Inputs and outputs
- Folder structure

### Hands-On
- Create a reusable module
- Call module from root configuration

### Outcome
You can **write reusable Terraform code**.

---

## 📅 Day 12 – Environments, Security & Best Practices

### You Will Learn
- Managing multiple environments
- Folder-based env structure
- Environment-specific tfvars
- Terraform workspaces (limitations)
- Security best practices
- Terraform anti-patterns
- Provisioners (why to avoid)

### Hands-On
- Create dev/prod environments
- Apply security best practices
- Run `terraform fmt` and `terraform validate`

### Outcome
You understand **production-grade Terraform practices**.

---

## 🧱 Day 13 – Terraform Project (Full Session)

### Hands-On Project
- Build a complete Terraform infrastructure
- Use variables, outputs, modules
- Organize code professionally
- Apply best practices

### Outcome
You have **one complete Terraform project**.

---

## 🎤 Day 14 – Terraform Interview Questions (Full Session)

### Hands-On (Interactive)
- Answer interview questions live
- Explain Terraform project end-to-end
- Discuss real-world scenarios
- Mock interview style discussion

### Outcome
You are **interview-ready for Terraform**.

---

## ⭐ Final Outcome of the Course

After completing this course, you will be able to:

- Design Terraform projects from scratch
- Write clean, maintainable Terraform code
- Work safely with Terraform state
- Manage multiple environments
- Confidently face Terraform interviews
- Showcase a real Terraform project

---

🚀 **Welcome to your Terraform journey — from Zero to Production.**
