# 📝 Day 3: Terraform Language (HCL) Study Notes

## 1. What is HCL?

* **Definition:** HashiCorp Configuration Language.
* **Type:** **Declarative** (You define the *goal*, Terraform handles the *steps*).
* **Comparison:** * *Imperative (Scripting):* "Go to the store, buy milk, bring it home."
* *Declarative (HCL):* "I want milk in my fridge."



---

## 2. The Anatomy of HCL

HCL is made of two main parts: **Blocks** and **Arguments**.

### A. The Block (The Container)

Groups information together.

* **Syntax:** `block_type "label1" "label2" { ... }`
* **Example:** `resource "aws_instance" "web" { ... }`

### B. The Argument (The Setting)

Key-value pairs that define the properties.

* **Syntax:** `key = "value"`
* **Example:** `instance_type = "t2.micro"`

---

## 3. Resource Block Syntax (The "Big Three")

When you write a resource, you are filling out three specific fields:

```hcl
resource "aws_instance" "my_server" {
  ami           = "ami-0abcd1234"
  instance_type = "t2.micro"
}

```

1. **Block Type:** Always `resource`.
2. **Resource Type:** `aws_instance` (What is it? Provided by the Cloud Provider).
3. **Local Name:** `my_server` (Your name for it. Used for internal references).

---

## 4. Resource Referencing (The "Linking" Logic)

To connect two resources, use the format: **Type.Name.Attribute**.

### Example: Connecting an EC2 to a Security Group

```hcl
# Create a Security Group
resource "aws_security_group" "web_sg" {
  name = "allow-traffic"
}

# Create EC2 and "Point" to the SG
resource "aws_instance" "web" {
  ami                    = "ami-123"
  vpc_security_group_ids = [aws_security_group.web_sg.id] # <--- REFERENCE
}

```

* **Implicit Dependency:** Terraform sees the reference and automatically builds the Security Group **before** the EC2.

---

## 5. Professional File Organization

Terraform reads all `.tf` files in a folder as if they were one giant file. We split them for **human readability**.

| Filename | Purpose |
| --- | --- |
| `provider.tf` | Defines the connection (AWS, Azure, Google). |
| `main.tf` | The primary resource definitions. |
| `variables.tf` | Input definitions (The "Inputs"). |
| `outputs.tf` | Information to display after code runs (The "Results"). |

---

## 6. Essential "Day 3" Commands

| Command | Action |
| --- | --- |
| `terraform fmt` | **Auto-formats** your code to look clean/aligned. |
| `terraform validate` | Checks if your **syntax** is correct without calling the cloud. |
| `terraform console` | Opens an **interactive terminal** to test expressions/logic. |

---
