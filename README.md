# Ansible Infrastructure Automation & Playbooks

A collection of production-ready Ansible playbooks, roles, and automation projects for provisioning, configuring, and managing cloud infrastructure.

---

## Repository Structure

Each directory contains a self-contained Ansible project with its own inventory, playbooks, roles, and documentation:

```text
.
├── wordpress-hosting/      # Multi-role LAMP stack & WordPress on Amazon Linux 2023
└── README.md               # Main repository documentation

```

---

## Featured Projects

### 1. [WordPress Hosting on Amazon Linux 2023](https://www.google.com/search?q=./wordpress-hosting/&utm_source=gemini)

* **Description:** Fully automated LAMP stack (Apache, MariaDB, PHP 8.x) and WordPress deployment using modular Ansible roles (`common`, `web`, `db`, `wordpress`).
* **Tech Stack:** Ansible, Amazon Linux 2023, AWS EC2 Dynamic Inventory, MariaDB, Apache, PHP-FPM, Ansible Vault.
* **Documentation:** See [`wordpress-hosting/README.md`](https://www.google.com/search?q=./wordpress-hosting/README.md&utm_source=gemini) for full architecture and setup guide.

---

## Prerequisites

Before running any playbooks in this repository, ensure your control node has Ansible and required collections installed:

```bash
# Core tools
pip install ansible

# Required collections
ansible-galaxy collection install amazon.aws community.mysql

```

---

## Usage

Navigate to any specific project directory and follow its local `README.md` instructions:

```bash
cd wordpress-hosting/
ansible-inventory --graph
ansible-playbook site.yml

```

```

---

### Suggested Git Commands for Your First Commit

```bash
# Initialize git repository (if not already done at root)
git init

# Add all files
git add .

# Run your first commit
git commit -m "feat: initial commit for ansible playbooks repo with wordpress-hosting project"

```