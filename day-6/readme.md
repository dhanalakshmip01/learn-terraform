# Day 6 – Advanced Resource Logic (Meta-Arguments & Lifecycles)

## 📌 Overview

Until now, Terraform learned **what to create**.
On Day 6, we learn **how Terraform should behave** while creating, updating, and deleting infrastructure.

This day focuses on writing **scalable**, **safe**, and **production-ready** Terraform code by using:

* **Meta-Arguments** → For scaling resources dynamically
* **Lifecycle Rules** → For protecting and controlling critical infrastructure

---

# 🔹 Part 1: Meta-Arguments – Scaling Infrastructure

Meta-arguments are special Terraform keywords that change **resource behavior**, not resource configuration.

They help eliminate copy-paste and allow **dynamic infrastructure creation**.

---

## ✅ `count` – Creating Identical Resources

### What is `count`?

`count` creates **multiple identical copies** of a resource from a single block.

Terraform treats them as **indexed resources**.

---

### Example: Create 3 EC2 Instances

```hcl
resource "aws_instance" "web" {
  count         = 3
  ami           = "ami-12345"
  instance_type = "t3.micro"

  tags = {
    Name = "web-${count.index}"
  }
}
```

---

### What Terraform Creates

```
aws_instance.web[0]
aws_instance.web[1]
aws_instance.web[2]
```

---

### When to Use `count`

Use `count` when:

* Resources are identical
* Order does not matter
* You only need quantity

Example use cases:

* Temporary test servers
* Load test nodes
* Batch compute workers

---

### ⚠ Problem with `count`

If you remove a resource in the middle:

```
Index shift happens
Terraform recreates resources
```

This can cause downtime.

---

## ✅ `for_each` – Creating Distinct Resources (Production Preferred)

### What is `for_each`?

`for_each` creates multiple resources using **named keys instead of numbers**.

This makes Terraform safer and more predictable.

---

### Example: Create Subnets Using Names

```hcl
resource "aws_subnet" "this" {
  for_each = {
    public  = "10.0.1.0/24"
    private = "10.0.2.0/24"
  }

  cidr_block = each.value

  tags = {
    Name = each.key
  }
}
```

---

### What Terraform Creates

```
aws_subnet.this["public"]
aws_subnet.this["private"]
```

---

### Why `for_each` is Better Than `count`

| Feature         | count             | for_each            |
| --------------- | ----------------- | ------------------- |
| Identity        | Index based       | Name based          |
| Deletion impact | Can shift indexes | Only removes target |
| Production safe | ❌ Risky           | ✅ Safe              |
| Recommended     | No                | Yes                 |

---

### When to Use `for_each`

Use `for_each` when:

* Resources have different configs
* Each resource has a name
* Production infrastructure is involved

Example use cases:

* Subnets
* Storage accounts
* EC2 with different roles
* IAM users
* Microservice resources


# Terraform `for_each` With Variables – Complete Practical Guide

This section explains how **`for_each` works with input variables**, how to design variables correctly, and how Terraform converts variable data into **multiple real infrastructure resources**.

This is the **production-standard way** to create scalable infrastructure.

---

# 🔹 What Problem Are We Solving?

Without variables, `for_each` looks like this:

```hcl
for_each = {
  web = "10.0.1.0/24"
  db  = "10.0.2.0/24"
}
```

This is **hardcoded**.

In real projects, we want:

* Different values per environment (dev, qa, prod)
* Configurable infrastructure
* Reusable Terraform code

So we use:

> **Variables + for_each = Dynamic Infrastructure**

---

# 🧠 Mental Model

Think of it like this:

```
terraform.tfvars → Variable Input
        ↓
for_each loop
        ↓
Terraform creates multiple resources
```

Each entry in the variable becomes **one real resource**.

---

# 🔹 Example 1: Simple for_each With Variable (Basic)

## 🎯 Goal

Create multiple EC2 instances using names from tfvars.

---

## Step 1 — Define Variable (variables.tf)

```hcl
variable "instance_names" {
  description = "Names of EC2 instances"
  type        = set(string)
}
```

---

## Step 2 — Provide Values (terraform.tfvars)

```hcl
instance_names = [
  "web",
  "api",
  "worker"
]
```

---

## Step 3 — Use for_each (main.tf)

```hcl
resource "aws_instance" "this" {

  for_each = var.instance_names

  ami           = "ami-0abcd"
  instance_type = "t3.micro"

  tags = {
    Name = each.key
  }
}
```

---

## 🔍 What Terraform Creates

Terraform expands this internally:

```
aws_instance.this["web"]
aws_instance.this["api"]
aws_instance.this["worker"]
```

---

## 🧠 Understanding each.key and each.value

Because we used a SET:

| Expression | Value |
| ---------- | ----- |
| each.key   | web   |
| each.value | web   |

For sets → key and value are same.

---

# 🔹 Example 2: Production Pattern (Different Config Per Resource)

Now let’s build a **real-world setup**.

---

## 🎯 Goal

Create multiple EC2 instances with different:

* Instance types
* Subnets
* Disk sizes
* Tags

---

# ✅ Step 1 — Define Variable Using map(object)

### variables.tf

```hcl
variable "ec2_instances" {
  description = "EC2 configuration map"

  type = map(object({
    ami           = string
    instance_type = string
    subnet_id     = string
    volume_size   = number
    tags          = map(string)
  }))
}
```

---

# ✅ Step 2 — Provide Values (terraform.tfvars)

```hcl
ec2_instances = {

  web = {
    ami           = "ami-0aaa"
    instance_type = "t3.micro"
    subnet_id     = "subnet-111"
    volume_size   = 20

    tags = {
      Role = "web"
      Env  = "prod"
    }
  }

  api = {
    ami           = "ami-0bbb"
    instance_type = "t3.medium"
    subnet_id     = "subnet-222"
    volume_size   = 50

    tags = {
      Role = "api"
      Env  = "prod"
    }
  }

  worker = {
    ami           = "ami-0ccc"
    instance_type = "t3.large"
    subnet_id     = "subnet-333"
    volume_size   = 100

    tags = {
      Role = "worker"
      Env  = "prod"
    }
  }
}
```

---

# ✅ Step 3 — Use for_each (main.tf)

```hcl
resource "aws_instance" "this" {

  for_each = var.ec2_instances

  ami           = each.value.ami
  instance_type = each.value.instance_type
  subnet_id     = each.value.subnet_id

  root_block_device {
    volume_size = each.value.volume_size
  }

  tags = merge(
    each.value.tags,
    {
      Name = each.key
    }
  )
}
```

---

# 🔍 What Terraform Creates

Terraform creates:

```
aws_instance.this["web"]
aws_instance.this["api"]
aws_instance.this["worker"]
```

Each resource has **its own configuration**.

---

# 🧠 Understanding each.key vs each.value

### each.key

This is the **map key**:

```
web
api
worker
```

Used for:

* Resource identity
* Naming
* Stable references

---

### each.value

This is the **full configuration object**:

```hcl
{
  ami = "ami-0aaa"
  instance_type = "t3.micro"
  subnet_id = "subnet-111"
  volume_size = 20
  tags = {...}
}
```

Used to configure resource fields.

---

# 🔹 Referencing for_each Resources

### Example: Get API instance ID

```hcl
aws_instance.this["api"].id
```

---

### Example: Get all instance IDs

```hcl
values(aws_instance.this)[*].id
```

---

# 🔹 Why Variables + for_each Is Production Standard

| Feature           | Benefit                         |
| ----------------- | ------------------------------- |
| Reusable code     | Same Terraform for all envs     |
| Config driven     | Change tfvars, not code         |
| No duplication    | One resource block              |
| Safe deletion     | Only targeted resources removed |
| Environment ready | Dev / QA / Prod separation      |

---

# 🔴 Common Mistakes

---

## ❌ Using list instead of map

Bad:

```hcl
list(object)
```

Causes unstable indexing.

Good:

```hcl
map(object)
```

Stable identity.

---

## ❌ Changing keys later

Changing:

```
web → web-server
```

Terraform will:

* Destroy old
* Create new

Keys must be stable.

---

## ❌ Hardcoding values

Avoid:

```hcl
instance_type = "t3.micro"
```

Prefer:

```hcl
instance_type = each.value.instance_type
```

---


> Terraform for_each combined with variables allows dynamic infrastructure creation using map-based configuration. Each key becomes a stable resource identity while values define resource-specific configuration, making this approach scalable and production safe.

---

# 🎯 Final Summary

Using `for_each` with variables allows you to:

* Create multiple resources dynamically
* Customize each resource
* Avoid copy-paste
* Build reusable Terraform modules
* Manage environments cleanly

---

🚀 **This pattern is the backbone of enterprise Terraform architecture.**

# 🔹 Part 2: Lifecycle Rules – Making Infrastructure Safe

Lifecycle rules control **how Terraform manages resource changes**.

They protect important resources and prevent downtime.

---

## ✅ `prevent_destroy` – Resource Protection

### What it Does

Prevents Terraform from deleting a resource.

Even if someone runs:

```
terraform destroy
```

Terraform will **fail instead of deleting**.

---

### Example: Protect Production Database

```hcl
resource "aws_db_instance" "prod_db" {

  engine = "postgres"

  lifecycle {
    prevent_destroy = true
  }
}
```

---

### Real Use Cases

Apply to:

* Databases
* Storage buckets
* DNS zones
* Production secrets

---

## ✅ `create_before_destroy` – Zero Downtime Replacement

### Default Terraform Behavior

```
Destroy old resource
Create new resource
```

This causes downtime.

---

### With create_before_destroy

Terraform changes order:

```
Create new resource
Switch traffic
Delete old resource
```

---

### Example: EC2 Upgrade Without Downtime

```hcl
resource "aws_instance" "web" {

  instance_type = "t3.medium"

  lifecycle {
    create_before_destroy = true
  }
}
```

---

### Real Use Cases

Use for:

* Web servers
* Load balancer targets
* Application VMs
* Certificates

---

## ✅ `ignore_changes` – Avoid Configuration Conflicts

### Problem It Solves

Terraform always tries to match configuration.

If autoscalers or admins modify something, Terraform tries to revert it.

---

### Example: Ignore Tag Changes

```hcl
resource "aws_instance" "web" {

  tags = {
    Name = "web-server"
  }

  lifecycle {
    ignore_changes = [tags]
  }
}
```

---

### Example: Ignore Autoscaling Size

```hcl
lifecycle {
  ignore_changes = [desired_capacity]
}
```

---

### Real Use Cases

Use when:

* Autoscalers manage size
* Operations teams update tags
* External systems modify config

---

# 🔥 Combined Production Example

```hcl
resource "aws_instance" "prod_app" {

  instance_type = "t3.large"

  lifecycle {
    prevent_destroy       = true
    create_before_destroy = true
    ignore_changes        = [tags]
  }
}
```

---

### What This Achieves

| Feature        | Result        |
| -------------- | ------------- |
| Deletion       | Blocked       |
| Replacement    | Zero downtime |
| Manual changes | Ignored       |

This is **production-grade behavior**.

---

# 🧠 Key Takeaways (Important)

### Meta-Arguments

* `count` → identical resources (testing only)
* `for_each` → named resources (production standard)

---

### Lifecycle Rules

* `prevent_destroy` → protects critical infra
* `create_before_destroy` → avoids downtime
* `ignore_changes` → avoids drift conflicts

---


