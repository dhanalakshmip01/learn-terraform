# 📅 Day 3: Practical Deployment & Professional Structure

**Goal:** Master the transition from writing code to managing live infrastructure. You will learn to deploy, link, and organize resources like a DevOps Engineer.

---

## 1️⃣ Step 1: The "First Build" Configuration

Create a file named `main.tf`. This code defines the specific versions and the hardware we want to build in AWS.

```hcl
# 1. Version Settings
terraform {
  required_version = ">= 1.1.0" # Minimum CLI version allowed

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0" # Use any version from 5.0 to 5.9
    }
  }
}

# 2. Provider Configuration
provider "aws" {
  region = "ap-south-1" # Mumbai Region
}

# 3. Resource Definitions
resource "aws_instance" "web-server" {
  ami           = "ami-0f5ee92e2d63afc18"
  instance_type = "t3.micro"

  tags = {
    Name = "Web-Server-Demo"
  }
}

resource "aws_instance" "app-server" {
  ami           = "ami-0f5ee92e2d63afc18"
  instance_type = "t3.micro"

  tags = {
    Name = "App-Server-Demo"
  }
}

```

### 🔍 Understanding the Code

* **Logical Names:** `web-server` and `app-server` are internal names for Terraform. You use these to refer to the resources later in your code.
* **Resource Type:** `aws_instance` is defined by the AWS Provider. It tells Terraform exactly what API to call.

---

## 2️⃣ Step 2: The Action (Workflow Deep-Dive)

When you run the commands, Terraform performs specific background tasks:

1. **`terraform init`**: Terraform reads the `required_providers` block and downloads the AWS plugin into a hidden `.terraform/` folder.
2. **`terraform plan`**: Terraform compares your code against the **State File** (which is currently empty) and calculates that it needs to create **2 resources**.
3. **`terraform apply`**: Terraform executes the plan, creates the EC2s in the AWS Mumbai region, and generates the `terraform.tfstate` file.

---

## 3️⃣ Step 3: Inspecting the State & Console

Now that the servers are live, we need to see how Terraform "remembers" them.

* **The State File (`terraform.tfstate`)**: Open this file. It is a JSON document containing every detail AWS assigned to your server, such as its **Private IP**, **Instance ID**, and **MAC Address**.
* **The Console**: Use `terraform console` to query your infrastructure without opening the AWS Dashboard.
* *Try this:* Type `aws_instance.web-server.public_ip` to see your server's IP address.



---

## 4️⃣ Step 4: Interdependency (Resource Linking)

In production, an EC2 instance is usually linked to a **Security Group**. We use **References** to create a connection.

**The Concept:** Never hardcode an ID. Use the reference: `aws_security_group.my_sg.id`.
**The Result:** Terraform builds a "Dependency Graph." It realizes it must build the Security Group **first** so the ID is available for the EC2 instance.

---

## 5️⃣ Step 5: Professional Project Structure

As your project grows, keeping everything in one `main.tf` becomes messy. Professionals split the code into specialized files:

| File Name | Responsibility |
| --- | --- |
| **`terraform.tf`** | Version constraints for Terraform and Providers. |
| **`provider.tf`** | The `provider "aws"` configuration and region. |
| **`main.tf`** | The actual resources (EC2, S3, SG). |
| **`outputs.tf`** | Values you want to see on your screen after deployment. |
| **`variables.tf`** | Values that change (like AMI IDs or Instance sizes). |

---
## 6 Step 6: Code Quality & Best Practices
Before a DevOps engineer pushes code to Git, they run these "Pre-flight" commands:

terraform fmt: Automatically fixes indentation and alignment to match HashiCorp standards.

terraform validate: Checks the configuration for internal consistency and syntax errors.

---

**Next Task:** Would you like me to prepare a **"Lab Challenge"** where you have to split your current `main.tf` into the professional 4-file structure?
