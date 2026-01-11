## Day 4: Parameterization & Visibility (Variables + Outputs)

In professional DevOps, we **never hardcode values**. This session focuses on turning static scripts into dynamic, reusable templates by separating "logic" (the resources) from "data" (the settings).

---

### 1. Input Variables: The "Arguments" of Infrastructure

Variables allow you to use the same code for Dev, Staging, and Production by simply changing the inputs. Inside `variables.tf`, you define the "arguments" your code will accept.

* **Data Types:**
* **String:** A sequence of characters, like names or IDs (e.g., `"t2.micro"`).
* **Number:** Numeric values for counts or sizes (e.g., `10` or `80`).
* **Bool:** Boolean values representing `true` or `false` (e.g., `assign_public_ip = true`).
* **List:** An ordered collection of values (e.g., `["us-east-1a", "us-east-1b"]`).
* **Map:** A group of key-value pairs (e.g., `{ env = "dev", dept = "it" }`).


* **Validation & Security:**
* **Validation Rules:** You can add a `validation` block with a `condition` and an `error_message` to ensure users provide the correct data format.
* **Sensitive Flags:** Set `sensitive = true` to prevent passwords or keys from being printed in the terminal logs.



#### Example: `variables.tf`

```hcl
variable "instance_type" {
  type        = string
  description = "The size of the instance"
  default     = "t2.micro"
}

variable "instance_count" {
  type    = number
  default = 1
}

variable "db_password" {
  type      = string
  sensitive = true
}

variable "project_tags" {
  type = map(string)
  default = {
    Project = "VPC-Project"
  }
}

```

---

### 2. Variable Precedence: "Who Wins?"

When a variable is defined in multiple places, Terraform follows a strict hierarchy to decide which value to use.

**The Hierarchy (Highest Priority to Lowest):**

1. **Command Line Option (`-var`):** Explicitly passing a value during execution (e.g., `terraform apply -var="instance_type=t2.large"`).
2. **Variable File (`-var-file`):** Using a specific file like `custom.tfvars`.
3. **Automatic Variable File (`terraform.tfvars`):** Terraform automatically looks for this file to load values.
4. **Environment Variables:** Values set in your OS prefixed with `TF_VAR_` (e.g., `export TF_VAR_instance_type="t2.small"`).
5. **Default Values:** The fallback value defined within the variable block itself.

---

### 3. Output Values: The "Return" of Infrastructure

Outputs act as the "Return Values" of your code. They extract specific information from the **State File** after a resource is created.

* **Usage:** You define outputs in `outputs.tf` to display data like Public IPs or DNS names for the user’s reference.
* **Querying:** You can view these at any time by running `terraform output`.

#### Example: `outputs.tf`

```hcl
output "public_ip" {
  value       = aws_instance.web_server.public_ip
  description = "The public IP address of the web server"
}

output "instance_id" {
  value = aws_instance.web_server.id
}

```

---

### 4. Locals: Your Private Variables

While **Variables** are external inputs, **Locals** are internal "computed" constants used to keep code **DRY** (Don't Repeat Yourself).

* **Purpose:** Use them to store complex logic or repeated strings (like a naming convention) so you only have to update them in one place.

#### Example: `main.tf` with Locals

```hcl
locals {
  service_name = "frontend"
  owner        = "dev-ops-team"
  
  # Combining variables and strings into a local
  full_name = "${local.service_name}-${var.environment}"
}

resource "aws_instance" "web" {
  ami           = var.ami_id
  instance_type = var.instance_type
  
  tags = {
    Name  = local.full_name
    Owner = local.owner
  }
}

```

---

### 💼 Case Study: "GlobalShop Inc." Environment Scaling

**The Problem:** GlobalShop had separate folders for `Dev`, `Test`, and `Prod`. Every change required manual copy-pasting, leading to **Configuration Drift** where Production settings didn't match Development.

**The Solution:**

1. **Parameterization:** They replaced hardcoded values with variables (e.g., `var.env_name`).
2. **Tfvars Strategy:** They used one master `main.tf` and separate `.tfvars` files for each environment.
3. **The Result:** To deploy to Production, they simply ran `terraform apply -var-file="prod.tfvars"`. This ensured infrastructure was identical across all stages, only differing in scale.

---

### 🛠 Hands-On Activity

1. **The Parameterized VM:** Replace hardcoded `ami` and `instance_type` in your EC2 code with variables.
2. **The `.tfvars` File:** Create a `terraform.tfvars` file to hold your actual values.
3. **Sensitive Variable:** Create a `db_password` variable with `sensitive = true` and observe how it is masked during `apply`.
4. **The Output Command:** Create an output that prints a pre-formatted SSH string: `"ssh -i key.pem ec2-user@${aws_instance.web.public_ip}"`.
