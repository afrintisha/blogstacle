---
title: "Building Multi-Cloud Infrastructure with Terraform"
date: 2025-12-29 12:00:00 +0600
categories: [Infrastructure as Code, Terraform]
summary: "Where it all began: My journey into Infrastructure as Code (IaC) starts with Terraform, managing resources across AWS, Linode, and Oracle Cloud."
image:
  path: /assets/img/covers/terraform.jpg
  alt: "Terraform Infrastructure"
readingTime: 7
tags: [terraform, aws, linode, oracle, cloud, devops, iac]
author: afrin
---

# The First Step: Defining Infrastructure as Code

My journey into advanced cloud engineering began with a single realization: clicking through web consoles is not scalable. Whether it was the AWS Console or the Linode Cloud Manager, manually creating instances was fine for learning the basics, but I wanted something more professional, reproducible, and robust.

That's when I discovered **Terraform**. It was the first major tool I learned in my Infrastructure as Code (IaC) journey, and it completely changed how I think about servers.

I created the **[Multi-Cloud Terraform Infrastructure](https://github.com/afrintisha/terraform-infrastructure)** project (located in my `terraform-infrastructure` folder) to serve as the foundation of my cloud operations. The goal was to maintain a centralized hub where I could define my entire cloud footprint—across multiple providers—in code.

## 🏠 The House Analogy: Why Terraform?

The easiest way to understand Terraform's role is to think of it as the **Real Estate Developer & Builder**.

- It buys the land (Cloud Provider resources).
- It lays the foundation (VPCs, Networks).
- It builds the walls and connects the pipes (Compute Instances, Load Balancers).
- It creates the **"shell"** (the Infrastructure).

This helps distinguish it from tools like Ansible, which come in later to do the "interior design" (software configuration).

---

## 🚀 Project Overview

I didn't want to be locked into a single provider's workflow. By using Terraform, I can manage resources across **AWS**, **Linode**, and **Oracle Cloud Infrastructure (OCI)** using a single, unified language (HCL).

This project represents the "Hardware" phase of my workflow. Before I can configure a server (which I'll do later with Ansible), I need to create it.

---

## 📂 Architecture

To keep things organized and modular, I structured the repository by provider. This separation of concerns allows me to manage each cloud environment independently while sharing the same best practices.

```plaintext
terraform-infrastructure/
├── aws/          # AWS configurations
├── linode/       # Linode Compute Instances and Networking
├── oracle/       # OCI VCNs, Compute, and Compartments
```

### 🟠 AWS (Amazon Web Services)

For AWS, I focused on the foundational elements of account management. The configuration handles **AWS Budgets** to keep costs in check (crucial for learning!), **IAM** policies for security, and core resource provisioning.

### 🟢 Linode (Akamai Connected Cloud)

Linode is great for its simplicity. My Terraform code here automates the deployment of compute instances and network configurations, allowing me to spin up a VPS for my projects in seconds.

### 🔴 Oracle Cloud Infrastructure (OCI)

Oracle Cloud offers a generous free tier, making it perfect for homelabs. Here, I'm automating the setup of Virtual Cloud Networks (VCNs) and compute resources, ensuring a robust network architecture from the start.

---

## 🛠️ Why Terraform First?

Learning Terraform first was a strategic choice. It offers unique advantages that define modern DevOps:

Learning Terraform first was a strategic choice. It offers unique advantages that define modern DevOps:

1.  **State Management (The Big Difference)**: Terraform is **Stateful**. It keeps a "State File" that remembers exactly what it built. If I tell Terraform "I want 3 servers" and I already have 2, it checks its state, realizes I only need one more, and creates just that one.
2.  **Provisioning Focus**: Its primary job is **Provisioning**—creating resources like VPCs, Subnets, and Databases by talking directly to Cloud APIs.
3.  **Declarative Style**: I define the _end state_ ("I want 5 servers"), and Terraform figures out the logic to get there.
4.  **Provider Agnostic**: The workflow is consistent regardless of the cloud provider. I can use the same commands for AWS, Linode, and Oracle.

The workflow is incredibly satisfying:

1. **`terraform init`**: Initialize the provider plugins.
2. **`terraform plan`**: Preview exactly what will happen.
3. **`terraform apply`**: Watch the infrastructure build itself.

Once `terraform apply` finishes, I have raw, running servers. But they are empty. They need software, users, firewalls, and configurations. This realization naturally led me to my next learning milestone: **Ansible**.

---

## ☁️ Powering Up with Terraform Cloud

I took my setup a step further by using **Terraform Cloud**. Not only does it securely store my state file remotely (no more `terraform.tfstate` on my local disk!), but it also simplifies the automation pipeline.

The feature I rely on most is the **Outputs**. Terraform Cloud provides all my infrastructure details—like Server IPs and Database endpoints—in a clean, structured **JSON format**. This isn't just for reading; it's the critical link that allows me to programmatically pass data to Ansible for the next stage of configuration.

## 📍 Roadmap

This project is the bedrock of my stack. My roadmap includes:

- **New Providers**: Integrating Google Cloud Platform (GCP) and Azure to broaden my multi-cloud expertise.
- **Complex Architectures**: Moving beyond simple instances to deploying full multi-tier application architectures entirely via code.

---

## 💭 Final Thoughts

Building infrastructure with code feels like a superpower. It brings the software engineering discipline—version control, code review, testing—to the world of operations.

If you are just starting with cloud infrastructure, I highly recommend picking up Terraform first. It forces you to think about your architecture clearly and logically.

In my next post, I'll explain how I solved the "configuration" problem using Ansible. Stay tuned!
