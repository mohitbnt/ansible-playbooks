```markdown
# Automated WordPress Deployment on Amazon Linux 2023

An end-to-end infrastructure-as-code (IaC) project built with Ansible to automatically provision and configure a multi-role **LAMP Stack** (Linux, Apache, MariaDB, PHP 8.x) hosting a custom **WordPress** instance on Amazon Linux 2023 EC2 instances.

---

## Project Architecture & Role Breakdown

The project follows a modular, 4-role Ansible structure to ensure clean separation of concerns and high reusability:

```text
wordpress-hosting/
├── ansible.cfg                 # Control node execution configuration
├── site.yml                    # Master playbook orchestrator
├── group_vars/
│   └── webservers/
│       ├── vars.yml            # Domain settings and PHP/MariaDB versioning
│       └── vault.yml           # Encrypted database secrets (Ansible Vault)
├── inventories/
│   ├── hosts                   # Static inventory (for testing)
│   └── aws_ec2.yml             # AWS Dynamic Inventory plugin configuration
└── roles/
    ├── common/                 # Package updates, pip, and PyMySQL setup
    ├── web/                    # Apache (httpd), PHP-FPM 8.x, SSL certs & VirtualHost
    ├── db/                     # MariaDB engine setup, root credentials, DB & user provisioning
    └── wordpress/              # Core download, extraction, and wp-config.php templating

```

---

## Features

* **Modular Architecture:** Infrastructure components are cleanly isolated into 4 Ansible roles (`common`, `web`, `db`, `wordpress`).
* **AWS Dynamic Inventory:** Leverages `amazon.aws.aws_ec2` to dynamically discover running AWS Spot/EC2 instances by resource tags.
* **Secure Secrets Management:** Uses `ansible-vault` to protect sensitive database credentials and SSL private keys.
* **SSL/TLS VirtualHost Deployment:** Configures HTTP to HTTPS permanent redirection and custom VirtualHost templates.
* **Production-Grade Handlers:** Includes handler flushes (`ansible.builtin.meta: flush_handlers`) for immediate service startup during database provisioning.

---

## Prerequisites

### Control Node

* **Operating System:** Linux / WSL
* **Python:** Python 3.x with `boto3`, `botocore`, and `pymysql` installed
* **Ansible Collections:**
```bash
ansible-galaxy collection install amazon.aws community.mysql

```


* **System Dependencies:**
```bash
sudo apt update && sudo apt install -y python3-boto3 python3-botocore

```



### Managed Nodes (Targets)

* **OS:** Amazon Linux 2023
* **SSH Access:** EC2 key pair configured on the control node (`~/.ssh/id_rsa`)
* **Tags:** EC2 instances tagged with `Environment: production`

---

## Configuration & Usage

### 1. Set Up Vault Passwords

Encrypt sensitive database credentials inside `group_vars/webservers/vault.yml`:

```bash
ansible-vault edit group_vars/webservers/vault.yml

```

Required variables inside `vault.yml`:

```yaml
db_root_password: "YourSuperSecureRootPassword"
db_password: "YourWordPressUserPassword"

```

### 2. Verify AWS Dynamic Inventory

Ensure your local AWS CLI credentials are active (`aws configure`), then verify that Ansible discovers your running EC2 instances:

```bash
ansible-inventory --graph

```

### 3. Test Connectivity

```bash
ansible all -m ping

```

### 4. Execute the Playbook

Run the orchestration playbook against your target host group:

```bash
ansible-playbook site.yml

```

---

## Verification

Once execution completes successfully:

1. Open your browser and navigate to `https://<YOUR-EC2-PUBLIC-IP>` or `https://<YOUR-DOMAIN>`.
2. Verify that the WordPress installation wizard page is displayed.
3. Complete the site setup wizard to initialize the database tables.

```

```
