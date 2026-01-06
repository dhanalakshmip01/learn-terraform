# 📚 Day 2 Study Notes: Architecture & Workflow

## 1️⃣ The Core Engine: Terraform CLI

The CLI is a **single binary file**. It doesn't come with a massive suite of installers; it’s just one program that acts as the "brain" of your operations.

* **Logic over Action:** The CLI doesn't know how to build a server. It only knows how to **read your files** and **orchestrate** other plugins to do the work.
* **Declarative Nature:** You tell the CLI what you want (the "What"), and it figures out the steps (the "How").

---

## 2️⃣ The Translators: Providers

If Terraform is the "Brain," **Providers** are the "Hands." Terraform cannot speak "AWS" or "Azure" natively. It speaks **HCL** (HashiCorp Configuration Language).

* **The Translation Layer:** Providers translate Terraform's generic instructions into specific API calls that clouds understand.
* **Automatic Fetching:** When you run `init`, Terraform looks at your code, sees you want "AWS," and downloads the AWS Provider plugin into a hidden folder (`.terraform/`).
* **Scope:** Each provider is restricted to its own ecosystem. The Google provider cannot touch AWS resources.

---

## 3️⃣ The Building Blocks: Resources

A **Resource** is the smallest unit of infrastructure. In your code, a resource is a *declaration*, but in the real world, it is a *physical or virtual object*.

> **Mental Model:** Think of a Resource as a **Ticket**. If you have a ticket for a seat at a theater, the ticket represents the seat. If you tear up the ticket (delete the code), the theater (Terraform) knows that seat should no longer be reserved for you.

---

## 4️⃣ The Memory: Terraform State

This is the most critical part of Terraform’s internal logic. The `terraform.tfstate` file is a JSON map.

* **The Source of Truth:** If your code says "10 Servers" but the State file says "10 Servers already exist," Terraform will do **nothing**.
* **Mapping:** It maps your local resource names (e.g., `my_web_server`) to the real IDs assigned by the cloud (e.g., `i-0abc12345`).

| Feature | Description |
| --- | --- |
| **File Name** | `terraform.tfstate` |
| **Format** | JSON (Plain text) |
| **Golden Rule** | **NEVER** edit this file manually. You will break the mapping. |

---

## 5️⃣ The Lifecycle: The Standard Workflow

Every Terraform engineer follows these four steps in order. Missing a step usually results in an error.

### Step A: `terraform init` (The Setup)

* **What happens:** Prepares the workspace.
* **Analogy:** Like a chef gathering ingredients and sharpening knives before cooking.
* **Outcome:** Creates the `.terraform` folder and locks in provider versions.

### Step B: `terraform plan` (The Preview)

* **What happens:** Terraform compares your **Code** vs. **State** vs. **Actual Cloud**.
* **Output:** It shows `+` (create), `~` (update), or `-` (delete).
* **Safety:** This is a read-only command. It costs $0 and changes nothing.

### Step C: `terraform apply` (The Execution)

* **What happens:** Terraform sends the API calls to the provider.
* **The Prompt:** It will ask you to type `yes` to confirm.
* **Outcome:** Real infrastructure is built, and the **State file is updated**.

### Step D: `terraform destroy` (The Cleanup)

* **What happens:** The reverse of `apply`.
* **Usage:** Used to save costs in lab environments or to decommission an old app.


## 6️⃣ What’s in my Folder? (File Anatomy)

As students run the workflow, Terraform generates specific files. Knowing what these are helps with troubleshooting.

| File / Folder | Created By | Purpose | Should you Commit to Git? |
| --- | --- | --- | --- |
| **`main.tf`** | **You** | Your infrastructure code. | **Yes** |
| **`.terraform/`** | `init` | Holds the downloaded Provider plugins. | **No** |
| **`.terraform.lock.hcl`** | `init` | Locks the exact version of the provider used. | **Yes** |
| **`terraform.tfstate`** | `apply` | The "Memory" (Source of Truth). | **No** (Usually) |

---

## 7️⃣ Deep Dive: The Plan Output Symbols

When students run `terraform plan`, they need to "read" the symbols. This is a common point of confusion for beginners.

* **`+` create**: Terraform will create a brand-new resource.
* **`~` update-in-place**: Terraform will change a setting on an existing resource (e.g., changing a tags).
* **`+/-` replace**: Terraform must **delete** the old resource and **create** a new one (happens if you change something unchangeable, like a VM's Availability Zone).
* **`-` destroy**: The resource will be removed.


