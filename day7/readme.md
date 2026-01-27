# 📦 Terraform Modules — Complete Notes (Clear + Practical)

---

# 🔹 What is a Terraform Module?

A Terraform module is:

> A reusable, self-contained collection of Terraform configuration files that defines a set of related infrastructure resources.

In simple words:

> Module = Reusable Terraform code block

---

# 🔹 Why Do We Need Modules?

Without modules:

* Code duplication
* Hard to maintain
* Environment inconsistency
* More errors

With modules:

✔ Reuse code
✔ Standardize infrastructure
✔ Easy updates
✔ Clean structure
✔ Team collaboration

---

# 🔹 Types of Terraform Modules

Terraform has **3 types of modules**:

---

## 1️⃣ Root Module

This is:

> The main working directory where you run Terraform commands.

Example:

```
terraform apply
```

Whatever folder you are in = **root module**

---

## 2️⃣ Child Modules

These are:

> Reusable modules called by root module.

Example:

```
VPC module
EC2 module
RDS module
```

---

## 3️⃣ Registry Modules

Prebuilt modules from:

* Terraform Registry
* GitHub
* Private module registries

Example:

```
terraform-aws-modules/vpc/aws
```

---

# 🔹 Basic Module Structure

---

## Typical Module Layout

```
modules/ec2/
│
├── main.tf
├── variables.tf
├── outputs.tf
```

---

### main.tf

Contains resource definitions.

---

### variables.tf

Input parameters for module.

---

### outputs.tf

Export values from module.

---

# 🔹 How Modules Work (Flow)

---

## Step 1 — Root Module Calls Child Module

```hcl
module "web_server" {
  source = "./modules/ec2"

  instance_type = "t3.micro"
}
```

---

## Step 2 — Child Module Creates Resources

```hcl
resource "aws_instance" "this" {
  instance_type = var.instance_type
}
```

---

## Step 3 — Terraform Connects Them

Terraform:

* Passes inputs
* Creates resources
* Returns outputs

---

# 🔹 Module Input Variables

Modules receive input using:

```
variable blocks
```

---

### Child Module Variable

```hcl
variable "instance_type" {
  type = string
}
```

---

### Root Module Passing Value

```hcl
module "web" {
  instance_type = "t3.micro"
}
```

---

# 🔹 Module Outputs

Modules expose values using:

```
output blocks
```

---

### Child Module Output

```hcl
output "instance_id" {
  value = aws_instance.this.id
}
```

---

### Root Module Access Output

```hcl
module.web.instance_id
```

---

# 🔹 Why Outputs Are Important

Outputs allow:

✔ Module chaining
✔ Dependency sharing
✔ Resource linking

Example:

VPC module output → EC2 module input.

---

# 🔹 Real Production Module Flow

```
Network Module
      ↓ outputs subnet_id
Compute Module
      ↓ outputs instance_id
Load Balancer Module
```

---

# 🔹 Module Source Types

Terraform supports multiple module sources:

---

## Local Path

```hcl
source = "./modules/vpc"
```

---

## Git Repository

```hcl
source = "git::https://github.com/org/vpc-module.git"
```

---

## Terraform Registry

```hcl
source = "terraform-aws-modules/vpc/aws"
```

---

# 🔹 Module Versioning (Very Important)

For registry modules:

```hcl
version = "3.14.0"
```

Why?

✔ Prevent breaking changes
✔ Stable deployments
✔ Controlled upgrades

---

# 🔹 Modules + Environment Pattern (Best Practice)

---

## Production Folder Structure

```
terraform/
│
├── modules/
│   ├── vpc/
│   ├── ec2/
│   └── rds/
│
└── environments/
    ├── dev/
    ├── qa/
    └── prod/
```

---

Each environment:

* Calls same modules
* Uses different values
* Uses separate backend

---

# 🔹 Benefits of Module-Based Architecture

---

| Benefit         | Description                |
| --------------- | -------------------------- |
| Reusability     | Write once, use everywhere |
| Consistency     | Same infra standard        |
| Maintainability | Easy updates               |
| Scalability     | Add environments easily    |
| Testing         | Test modules independently |

---

# 🔹 Module Best Practices

---

## ✅ Keep Modules Small

One responsibility per module.

Example:

* VPC module
* EC2 module
* DB module

---

## ✅ Avoid Hardcoding

Always use variables.

---

## ✅ Use Outputs Properly

Expose only necessary values.

---

## ✅ Version Your Modules

Lock registry module versions.

---

## ✅ Document Modules

Add README.md inside modules.

---

# 🔹 Common Mistakes

---

❌ Writing all infra in root
❌ Hardcoding values inside module
❌ Mixing environment logic inside module
❌ No outputs
❌ No version pinning

---

# 🔹 Interview Question Tip

---

## Q: Why modules are important in Terraform?

### Best Answer:

> Modules allow reuse, standardization, and separation of concerns. They make Terraform code scalable, maintainable, and production-ready by avoiding duplication and enabling consistent infrastructure patterns.

---

# 🧠 Summary (Quick Revision)

---

### Terraform Module Is:

Reusable Terraform code unit.

---

### Module Has:

* main.tf → Resources
* variables.tf → Inputs
* outputs.tf → Outputs

---

### Used For:

* Code reuse
* Multi-env support
* Infrastructure standardization

---

# 🎯 One Line Definition

> Terraform modules are reusable building blocks that allow you to organize, standardize, and scale infrastructure code efficiently.

---


