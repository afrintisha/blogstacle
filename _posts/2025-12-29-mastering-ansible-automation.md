---
title: "Mastering Infrastructure Automation with Ansible & Semaphore"
date: 2025-12-29 12:05:00 +0600
categories: [Automation, Ansible, Semaphore]
summary: "Taking the next step: How I used Ansible to configure the servers I built with Terraform, and my experience managing it all with the Ansible Semaphore GUI."
image:
  path: /assets/img/covers/2025-12-29-ansible-automation.jpg
  alt: "Ansible automation code and Semaphore dashboard"
readingTime: 8
tags: [ansible, devops, semaphore, gui, hardening, docker, automation]
author: afrin
---

# From Provisioning to Configuration

In my previous post, I shared how I used **Terraform** to build my cloud infrastructure. It gave me the power to spin up servers in minutes. But once those servers were running, I realized I had a new problem: they were blank slates.

SSH-ing into each new server to install Docker, set up firewalls, and create users was tedious and manual—exactly the kind of work I wanted to avoid. I needed a tool to manage the _configuration_ of my infrastructure.

## 🎨 The House Analogy: Why Ansible?

If Terraform is the Builder that creates the house, **Ansible is the Interior Designer & Plumber**.

- It comes into the finished house (the server Terraform built).
- It paints the walls (installs software packages).
- It installs the specific stove you want (configures services like Docker or Nginx).
- It sets up the WiFi (secures the network).
- It configures the **"inside"** (the Software).

That is when I dived deep into **Ansible**.

## 💡 Why Ansible?

Chosen for its professional simplicity, Ansible brings two massive benefits to my workflow:

1.  **Agentless**: It works over standard SSH. I don't need to install special agents on my target servers.
2.  **Idempotent**: This means I can run the same playbook multiple times without breaking anything. Ansible checks the state first—if Nginx is already installed, it does nothing. This prevents **"config drift"** and ensures consistency.
3.  **Simple Architecture**:
    - **Inventory**: Defines the hosts (e.g., specific IP addresses).
    - **Patterns**: Groups hosts by role (e.g., `web`, `db`) so I can target specific modifications.
    - **Playbooks**: A "book of tasks" written in YAML that declares the desired end state.

---

## 📋 The Ansible-Semaphore Project

I created the **[Ansible-Semaphore](https://github.com/afrintisha/ansible-semaphore)** project (which you can find in my `ansible-semaphore` folder) to solve this. The goal was to treat the server configuration as code, just like I did with the infrastructure.

The project handles two critical tasks now :

### 1. VPS Hardening (`hardening.yml`)

As soon as Terraform hands me an IP address via it's output, this playbook establishes a secure baseline. It automatically updates packages, configures `ufw` firewalls and `fail2ban`, creates secure users, and locks down SSH.

### 2. Docker Environment Setup (`docker_setup.yml`)

Since most of my apps run in containers, this playbook turns a fresh OS into a Docker host. It handles GPG keys, repositories, and permissions, so I'm ready to deploy containers immediately.

---

## 🖥️ Visualising Automation: Ansible Semaphore

I eventually wanted a better way to visualize my jobs and manage deployments. I didn't want to just run scripts from my terminal forever; I wanted a dashboard.

This led me to **Ansible Semaphore**, a beautiful, open-source web GUI for Ansible. I currently host this instance on my **Lewsions** server (`altitude-sg-a1-02`) located in **Singapore**.

### Why Semaphore?

After mastering the basics of Ansible playbooks, I installed Semaphore to take my skills to the intermediate level. It acts as the control tower for my automation:

- **Visual Dashboard**: I can see which playbooks succeeded or failed at a glance.
- **Job History**: It keeps a log of every run, so I know exactly what changed and when.
- **Scheduling**: I can schedule maintenance playbooks (like updating packages) to run automatically.
- **Inventory Management**: It acts as a central dynamic inventory for all the servers I provisioned with Terraform.

I have spent considerable time configuring Semaphore to work seamlessly with my playbooks. Understanding how to link the GUI with the underlying code gave me a much deeper appreciation for how enterprise automation platforms work.

---

## 🚀 The Complete Workflow

My infrastructure journey now looks like this:

My infrastructure journey now follows a professional "Hand-off Pattern":

1.  **Terraform (The Builder)**: Talks to the Cloud APIs (AWS/Linode) to create the raw "shell" infrastructure. It helps me spin up a VPC and an EC2 instance.
2.  **The Hand-off**: Terraform Cloud exports the new server's details (like the public IP) in a structured **JSON format**, which I can easily pipe into my automation tools.
3.  **Ansible (The Designer)**: I add that IP to my Ansible **Inventory** (managed by Semaphore). Ansible then SSHs into the server to install Docker, secure the OS, and deploy my apps.
4.  **Ansible Semaphore**: Orchestrates and visualizes these playbooks via a web interface.

This separation of duties—provisioning vs. configuration—is a key DevOps concept that learning these tools in this specific order has helped me understand.

---

## 🗺️ What's Next?

This project is very much alive. I have a roadmap of features I want to implement as I learn more:

- **Complex Orchestration**: Building workflows in Semaphore to deploy multi-service stacks.
- **CI/CD Pipelines**: Automatically testing my playbooks to ensure they work before I run them against production servers.

Automation has been a game-changer for me. It’s not just about saving time; it’s about understanding exactly how your systems are configured and being able to recreate them at will.
