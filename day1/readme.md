

# 📅 Day 1: Terraform Foundations & Infrastructure as Code (IaC)

Welcome to **Day 1** of the **Terraform: Zero to Production** course.

Today, we build the **core foundation**. If you master these concepts today, the rest of the Terraform ecosystem will feel intuitive and easy to navigate.

> [!IMPORTANT]
> This day focuses on **conceptual clarity** and the **mental model** of IaC, not just memorizing commands.

---

## 🎯 Day 1 Objectives

By the end of today, you will be able to:

* Define **Infrastructure as Code (IaC)** and its benefits.
* Explain the **Terraform Architecture** (CLI, Providers, Resources).
* Execute the standard **Terraform Workflow**.
* Write and run your **first configuration file**.
* Distinguish between **Declarative** and **Imperative** models.

---

## 🧠 What is Infrastructure as Code (IaC)?

### The "Old Way": Manual Infrastructure

Traditionally, infrastructure was built by "ClickOps" (manually clicking in the AWS/Azure console) or running ad-hoc scripts. This led to:

* ❌ **Human Error:** Forgetting a checkbox or security group rule.
* ❌ **Configuration Drift:** Dev, Staging, and Prod environments slowly becoming different.
* ❌ **No History:** No way to see who changed what and when.

### The "New Way": IaC

**Infrastructure as Code** means managing and provisioning your infrastructure through **machine-readable definition files** rather than manual tools.

**Key Benefits:**

* **Speed:** Provision thousands of resources in minutes.
* **Version Control:** Store your infra in **Git**.
* **Consistency:** The exact same code creates the exact same environment every time.

---

## 🚀 What is Terraform?

Terraform is an open-source **Infrastructure as Code** tool created by HashiCorp. It allows you to define both cloud and on-prem resources in human-readable configuration files that you can version, reuse, and share.

### Key Characteristics

1. **Cloud Agnostic:** Use the same workflow for AWS, Azure, GCP, Kubernetes, or even Cloudflare.
2. **Declarative:** You describe the **Final State**, and Terraform figures out how to get there.
3. **Stateful:** Terraform keeps track of what it built so it can update or delete it later.

---

## ⚖️ Declarative vs. Imperative

Understanding this distinction is vital for any DevOps engineer.

| Approach | Analogy | Description |
| --- | --- | --- |
| **Imperative** | *A Recipe* | You define the exact steps: "Step 1: Create VPC, Step 2: Create Subnet." |
| **Declarative** | *A Thermostat* | You define the goal: "I want the temperature to be 72°F." Terraform adjusts the heat/AC automatically. |

**Terraform is Declarative.** You don't tell it to "Create a VM"; you tell it "I want a VM to exist," and it handles the API calls to make it happen.

---

## 🧱 Terraform Architecture

Terraform is composed of three main parts that work together:

1. **Terraform CLI:** The local tool you install to run commands.
2. **Providers:** Plugins (like drivers) that allow Terraform to talk to specific APIs (AWS, Azure, etc.).
3. **State File (`terraform.tfstate`):** A JSON file that acts as Terraform's "source of truth," mapping your code to real-world resources.

---

## 🔄 The Terraform Workflow (The "Big Four")

This is the standard cycle you will repeat every day as a DevOps Engineer:

1. **`terraform init`**: Prepares the directory and downloads the necessary **Providers**.
2. **`terraform plan`**: A "dry run." It shows you what will happen without actually changing anything.
3. **`terraform apply`**: Executes the changes to reach the desired state.
4. **`terraform destroy`**: Safely removes all infrastructure managed by the code.

---

## 💻 Hands-on: Your First Configuration

### 1. Install & Verify

Ensure Terraform is installed on your machine.

```bash
terraform version

```

### 2. Create your Project

```bash
mkdir terraform-day1 && cd terraform-day1
touch main.tf

```

### 3. Write the Code

Copy this into `main.tf`. This code tells Terraform: *"I want to use AWS in North Virginia, and I want one t2.micro instance."*

```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_instance" "my_first_server" {
  ami           = "ami-0c55b159cbfafe1f0" # Note: Ensure this AMI is valid for your region
  instance_type = "t2.micro"

  tags = {
    Name = "Terraform-Day1-Demo"
  }
}

```

### 4. Execute

Run these in order:

* `terraform init`
* `terraform plan`
* `terraform apply` (Type `yes` when prompted)

---

## ⚠️ Safety Rules

* **Review your Plans:** Always read the output of `terraform plan` before hitting apply.
* **Protect the State:** Never manually edit `terraform.tfstate`. If you lose this file, Terraform "forgets" what it built.
* **Cost Awareness:** Resources created in the cloud cost money. Always `terraform destroy` your lab work!

---

## 📝 Practice Tasks

* [ ] Install Terraform on your local machine.
* [ ] Initialize a directory using `terraform init`.
* [ ] Create a `main.tf` file and successfully run `terraform plan`.
* [ ] (Optional) If you have cloud credentials, run `terraform apply` and verify the resource in your console.

---
