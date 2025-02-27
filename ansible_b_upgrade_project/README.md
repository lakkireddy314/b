# Ansible b_upgrade Project

## Overview
This Ansible project automates the upgrade process for **IBM WebSphere Application Server, WebSphere Plugins, IBM HTTP Server, and IBM BAW (Business Automation Workflow)** while ensuring **backup, rollback, and post-installation configurations**.

## New Feature: `vars/` Folder
The **b_upgrade** role now includes a **`vars/` folder**, which **centralizes all configuration variables** such as:
- Installation directories
- Target upgrade versions
- Repository URLs

### **Why is the `vars/` folder important?**
✅ **Modular & Cleaner Code** – Variables are centralized in `vars/main.yml` rather than being spread across task files.  
✅ **Easier Customization** – You can modify `vars/main.yml` without editing task files.  
✅ **Consistent Upgrades** – Ensures all installations use predefined versions and paths.  

## Directory Structure

```
ansible_b_upgrade_project/
├── roles/
│   └── b_upgrade/
│       ├── defaults/main.yml
│       ├── vars/main.yml       # Centralized variable management
│       ├── tasks/
│       │   ├── main.yml
│       │   ├── precheck.yml
│       │   ├── pre_upgrade.yml
│       │   ├── check_installed.yml
│       │   ├── check_baw_installed.yml
│       │   ├── available_versions.yml
│       │   ├── available_baw_versions.yml
│       │   ├── no_upgrade_check.yml
│       │   ├── no_baw_upgrade_check.yml
│       │   ├── backup.yml
│       │   ├── backup_baw.yml
│       │   ├── upgrade_websphere.yml
│       │   ├── upgrade_plugins.yml
│       │   ├── upgrade_ibm_http.yml
│       │   ├── upgrade_baw.yml
│       │   ├── rollback.yml
│       │   ├── rollback_baw.yml
│       │   ├── post_upgrade.yml
│       │   ├── post_upgrade_baw.yml
│       │   ├── baw_post_config.yml
│       │   ├── report.yml
│       │   ├── post_actions.yml
│       ├── templates/upgrade_version_report.j2
├── inventory/
│   ├── dev/inventory.yml
│   ├── intg/inventory.yml
│   ├── perf/inventory.yml
│   ├── prod/inventory.yml
│   ├── dev/group_vars/
│   │   ├── BAW.yml
│   │   ├── dev.yml
│   │   ├── unit.yml
│   ├── intg/group_vars/intg.yml
│   ├── perf/group_vars/perf.yml
│   ├── prod/group_vars/prod.yml
├── playbooks/
│   ├── upgrade_websphere.yml
├── README.md
```

## **vars/main.yml** – Variables Centralized Here

```yaml
---
# Default variables for b_upgrade role

# WebSphere variables
websphere_version: "9.0.5"
websphere_install_dir: "/opt/IBM/WebSphere/AppServer"

# WebSphere Plugins variables
websphere_plugins_version: "9.0.5"
websphere_plugins_install_dir: "/opt/IBM/WebSphere/Plugins"

# IBM HTTP Server variables
ibm_http_version: "9.0.5"
ibm_http_install_dir: "/opt/IBM/HTTPServer"

# IBM BAW variables
baw_version: "24.0.0.1"
baw_install_dir: "/opt/IBM/BAW"

# Repositories
websphere_next_repo: "http://repo.example.com/websphere"
websphere_plugins_next_repo: "http://repo.example.com/websphere_plugins"
ibm_http_next_repo: "http://repo.example.com/ibm_http"
baw_next_repo: "http://repo.example.com/baw"
```

## **How Does the `vars/` Folder Improve the Role?**
- Instead of defining variables **inside tasks**, they are now **included globally** in `tasks/main.yml` using:
  ```yaml
  - include_vars: vars/main.yml
  ```
- This **simplifies maintenance** and **improves readability**.

## Running the Playbook for a Specific Environment

To run the **b_upgrade** role for a specific environment:

#### **For Dev (including Unit)**
```sh
ansible-playbook -i inventory/dev/inventory.yml playbooks/upgrade_websphere.yml
```

#### **For Integration (Intg)**
```sh
ansible-playbook -i inventory/intg/inventory.yml playbooks/upgrade_websphere.yml
```

#### **For Performance (Perf)**
```sh
ansible-playbook -i inventory/perf/inventory.yml playbooks/upgrade_websphere.yml
```

#### **For Production (Prod)**
```sh
ansible-playbook -i inventory/prod/inventory.yml playbooks/upgrade_websphere.yml
```

## Features of the `b_upgrade` Role

✅ **Modular and Structured** role  
✅ **Supports multiple environments**  
✅ **Automatic rollback if upgrade fails**  
✅ **Prevents unnecessary upgrades**  
✅ **Detailed reporting for tracking**  
✅ **Post-installation configurations for IBM BAW**  

---

This project is now fully structured, documented, and ready for use. The addition of the `vars/` folder makes it even **more flexible and easier to manage**.

