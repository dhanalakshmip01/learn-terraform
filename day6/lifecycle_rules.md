
# 🔹 Lifecycle Rules – Making Infrastructure Safe

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


