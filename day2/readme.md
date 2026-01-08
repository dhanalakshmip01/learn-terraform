# 🚀 Terraform Fundamentals - Day 2

This module covers the core architecture, syntax, and lifecycle of Terraform. Understanding these concepts is essential before writing production-grade Infrastructure as Code (IaC).

## 📑 Topics Covered
1. **HCL Basics:** Blocks, Arguments, and Resource Syntax.
2. **Architecture:** CLI, Providers, and the State File.
3. **Workflow:** The 4-step lifecycle (`init`, `plan`, `apply`, `destroy`).
4. **File Anatomy:** Understanding the generated files and folders.

## 🛠️ Terraform Workflow
To deploy infrastructure, we follow the standard industry workflow:
1. **Initialize** (`terraform init`) - Prepare the workspace.
2. **Plan** (`terraform plan`) - Preview changes.
3. **Apply** (`terraform apply`) - Execute changes.
4. **Destroy** (`terraform destroy`) - Cleanup resources.

## ⚠️ Important Note on State
The `terraform.tfstate` file is the **Source of Truth**. 
- **Never** edit this file manually.
- **Never** delete this file if infrastructure is live.
- **Git Strategy:** Ensure `.tfstate` files are added to your `.gitignore`.



# 📚 Student Study Notes: Terraform Architecture & HCL

## 1️⃣ What is HCL? (The Language)

**HCL** stands for **HashiCorp Configuration Language**.

* **Type:** **Declarative**. You define the **goal** (End State), and Terraform handles the **steps** (Execution).
* **Mental Comparison:**
* **Imperative (Scripting):** "Go to the store, buy milk, bring it home."
* **Declarative (HCL):** "I want milk in my fridge."



---

## 2️⃣ The Anatomy of HCL

HCL is built using two primary structures: **Blocks** and **Arguments**.

### A. The Block (The Container)

Used to group related information together.

* **Syntax:** `block_type "label1" "label2" { ... }`
* **Example:** `resource "aws_instance" "web" { ... }`

### B. The Argument (The Setting)

Key-value pairs that define specific properties inside a block.

* **Syntax:** `key = "value"`
* **Example:** `instance_type = "t2.micro"`

### C. Resource Syntax (The "Big Three")

```hcl
resource "aws_instance" "my_server" {
  ami           = "ami-0abcd1234"
  instance_type = "t2.micro"
}

```

1. **Block Type:** Always `resource`.
2. **Resource Type:** `aws_instance` (Defined by the Cloud Provider).
3. **Local Name:** `my_server` (Internal reference used within your code).

---

## 3️⃣ The Core Engine & Architecture

### 🧠 The Brain: Terraform CLI

A **single binary file**. It doesn't build servers; it **reads your files** and **orchestrates** the work.

* **Logic over Action:** It calculates dependencies and tells plugins what to do.

### 🤝 The Hands: Providers

Terraform doesn't speak "AWS" or "Azure" natively; it speaks HCL.

* **The Translators:** Providers translate HCL into specific API calls.
* **Automatic Fetching:** Running `init` downloads these plugins into the hidden `.terraform/` folder.

### 🎟️ The Building Blocks: Resources

The smallest unit of infrastructure.


---

## 4️⃣ The Memory: Terraform State

The `terraform.tfstate` file is a JSON map of your infrastructure.

* **Source of Truth:** It prevents duplicate work. If the state says a server exists, Terraform won't build it again.
* **Mapping:** It maps your local names (`my_web_server`) to real Cloud IDs (`i-0abc12345`).

| Feature | Description |
| --- | --- |
| **File Name** | `terraform.tfstate` |
| **Format** | JSON (Plain text) |
| **Rule** | **NEVER** edit manually. |

---

## 5️⃣ The Lifecycle: Standard Workflow

| Step | Command | Analogy | Outcome |
| --- | --- | --- | --- |
| **A. Setup** | `terraform init` | Chef gathering ingredients. | Creates `.terraform/` folder. |
| **B. Preview** | `terraform plan` | Looking at the menu/price. | Shows `+`, `~`, `-` symbols. |
| **C. Execute** | `terraform apply` | Cooking the meal. | Builds infrastructure & updates State. |
| **D. Cleanup** | `terraform destroy` | Cleaning the kitchen. | Deletes all managed resources. |

---

## 6️⃣ File Anatomy: What's in my folder?

| File / Folder | Created By | Purpose | Git? |
| --- | --- | --- | --- |
| **`main.tf`** | **You** | Your HCL code. | ✅ **Yes** |
| **`.terraform/`** | `init` | Provider plugins. | ❌ **No** |
| **`.terraform.lock.hcl`** | `init` | Version locking. | ✅ **Yes** |
| **`terraform.tfstate`** | `apply` | The Memory. | ❌ **No** |

---

## 7️⃣ Deep Dive: Plan Output Symbols

* **`+` create:** A brand-new resource will be built.
* **`~` update-in-place:** A setting will change (e.g., updating a Tag).
* **`+/-` replace:** Terraform must **delete** and **re-create** the resource (e.g., changing a VM's subnet).
* **`-` destroy:** The resource will be removed.


