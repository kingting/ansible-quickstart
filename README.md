
---

# **Ansible Corporate Project Repository**

Welcome to the Ansible repository for managing automation within the corporate environment. This document provides all necessary setup instructions, prerequisites, and usage guidelines to help you quickly get started with Ansible, Molecule, and AWX.

---

## **Table of Contents**
1. [Prerequisites](#prerequisites)
2. [Setup Instructions](#setup-instructions)
3. [Project Structure](#project-structure)
4. [Inventory Management](#inventory-management)
5. [Using Ansible Collections](#using-ansible-collections)
6. [Ansible Galaxy](#ansible-galaxy)
7. [Running Playbooks](#running-playbooks)
8. [Testing with Molecule](#testing-with-molecule)
9. [Using AWX](#using-awx)
10. [Troubleshooting](#troubleshooting)

---

## **1. Prerequisites**

Ensure you have the following tools installed on your machine or remote server:
- **Python 3.8+**
- **Ansible 2.11+**
- **Molecule** (for testing)
- **Docker** (preferred Molecule driver)
- **AWX/Tower** CLI (optional, for AWX integration)

### Install Prerequisites
1. Install Python dependencies:
   ```bash
   pip install ansible molecule ansible-lint
   ```

2. Install Docker (if not already installed):
   ```bash
   sudo apt-get install docker.io
   sudo systemctl start docker
   sudo systemctl enable docker
   ```

3. Install AWX CLI:
   ```bash
   pip install awxkit
   ```

---

## **2. Setup Instructions**

1. Clone this repository:
   ```bash
   git clone https://github.com/<your_company>/ansible-corporate-project.git
   cd ansible-corporate-project
   ```

2. Install required Ansible collections:
   ```bash
   ansible-galaxy collection install -r collections/requirements.yml
   ```

3. Ensure Docker is running if using Molecule for testing:
   ```bash
   docker info
   ```

4. Verify the setup by running:
   ```bash
   ansible --version
   molecule --version
   ```

---

## **3. Project Structure**
The repository is organized as follows:
```
ansible-corporate-project/
├── collections/             # Custom and external Ansible collections
│   ├── requirements.yml     # List of required collections
├── inventory/               # Inventory files (static or dynamic)
│   ├── production/          # Production inventory
│   └── development/         # Development inventory
├── roles/                   # Ansible roles
│   ├── role1/
│   └── role2/
├── playbooks/               # Playbooks for workflows
├── tests/                   # Testing with Molecule
│   ├── integration/
│   └── unit/
└── README.md                # Documentation
```

---

## **4. Inventory Management**

### Static Inventory
Static inventory files are located under the `inventory/` directory. Example:
```ini
[webservers]
web01 ansible_host=192.168.1.10 ansible_user=admin

[dbservers]
db01 ansible_host=192.168.1.20 ansible_user=admin
```

### Dynamic Inventory
If using dynamic inventory, ensure the inventory script or plugin is properly configured in `ansible.cfg`.

Run the following command to test the inventory:
```bash
ansible-inventory -i inventory/development/ --list
```

---

## **5. Using Ansible Collections**

This project relies on both custom and external collections. The required collections are listed in `collections/requirements.yml`.

### Install Collections
```bash
ansible-galaxy collection install -r collections/requirements.yml
```

### List Installed Collections
```bash
ansible-galaxy collection list
```

---

## **6. Ansible Galaxy**

### Publish Custom Collections
To publish your custom collection:
```bash
ansible-galaxy collection build
ansible-galaxy collection publish <path_to_collection_tarball>
```

### Build and Test Custom Collections
```bash
ansible-galaxy collection build
molecule test
```

---

## **7. Running Playbooks**

### Run a Playbook
To execute a playbook:
```bash
ansible-playbook -i inventory/development/ playbooks/deploy.yml
```

### Use a Specific Role
To run a playbook with a specific role:
```yaml
- name: Example Playbook
  hosts: all
  tasks:
    - name: Include role
      include_role:
        name: role1
```
Run:
```bash
ansible-playbook -i inventory/development/ playbooks/example.yml
```

---

## **8. Testing with Molecule**

### Run Molecule Tests for a Role
1. Navigate to the role directory:
   ```bash
   cd roles/role1
   ```

2. Initialize Molecule (if not already done):
   ```bash
   molecule init scenario -d docker
   ```

3. Run the tests:
   ```bash
   molecule test
   ```

---

## **9. Using AWX**

### Login to AWX
Use the AWX CLI (`awx`) to interact with the AWX server:
```bash
awx login -u <username> -p <password> -k --conf.host https://<awx_server_url>
```

### Launch a Job Template
To launch a job template in AWX:
```bash
awx job_templates launch --name "Deploy Application" --monitor
```

### Sync Projects in AWX
If you’ve made updates to this repository, sync the associated AWX project:
```bash
awx projects update --name "Ansible Corporate Project"
```

---

## **10. Troubleshooting**

### Common Issues
1. **Docker Permission Denied**:
   Ensure your user is added to the `docker` group:
   ```bash
   sudo usermod -aG docker $USER
   ```

2. **Ansible Collection Errors**:
   Reinstall required collections:
   ```bash
   ansible-galaxy collection install -r collections/requirements.yml --force
   ```

3. **Molecule Test Failures**:
   Check the logs:
   ```bash
   molecule converge
   molecule verify
   ```

4. **AWX CLI Issues**:
   Verify your AWX server URL and credentials.

---
