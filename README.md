# V-Server Setup and Deployment

This repository documents the step-by-step setup of a Linux V-Server for the Developer Akademie project.  
It includes secure SSH configuration, NGINX web server setup, and GitHub SSH integration.

---

## 📚 Table of Contents
1. [Project Overview](#project-overview)
2. [Server Configuration](#server-configuration)
3. [GitHub Integration](#github-integration)
4. [Testing and Validation](#testing-and-validation)
5. [Repository Structure](#repository-structure)
6. [Checklist](#checklist)

---

## 🧾 Project Overview
The goal of this project was to configure a secure Linux V-Server, install and customize the NGINX web server,  
and establish a secure connection to GitHub for version control and deployment automation.

---

## ⚙️ Server Configuration
The complete setup and configuration steps are documented in  
[`docs/server-setup.md`](docs/server-setup.md)

This includes:
- SSH key generation and secure server access  
- Password login deactivation  
- NGINX installation and custom configuration  
- Alias and SSH configuration for multiple connections

---

## 🔑 GitHub Integration
The guide also covers:
- Configuring Git with username and email  
- Creating a server-specific SSH key for GitHub  
- Linking the public key to your GitHub account  
- Verifying the SSH connection with GitHub

---

## ✅ Testing and Validation
After setup:
- SSH access via key works successfully  
- Password login is disabled  
- NGINX displays a custom HTML page on port `8081`  
- GitHub SSH authentication confirms successful key registration  

---

## 📁 Repository Structure
```
v-server-setup/
│
├── README.md
├── .gitignore
├── docs/
│   ├── server-setup.md
│   └── Checklist-V-Server.pdf
```

---

## 🧩 Checklist
The official **Developer Akademie Checklist (PDF)** is included under `docs/`.  


---

© 2025 – Project by Ognjen Manojlovic
