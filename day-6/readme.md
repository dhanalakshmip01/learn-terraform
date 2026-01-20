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

---

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


