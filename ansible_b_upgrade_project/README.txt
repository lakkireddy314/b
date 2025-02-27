# b_upgrade Role - Detailed Guide

## Overview

The `b_upgrade` role is designed to **automate the upgrade process for IBM WebSphere Application Server, WebSphere Plugins, IBM HTTP Server, and IBM BAW (Business Automation Workflow) while ensuring safe execution with backup and rollback mechanisms**.

This role follows **best practices** to ensure **only necessary upgrades occur, automatic rollback in case of failures, and post-installation configurations for IBM BAW**.

---

## Key Features

✅ **Automated WebSphere, Plugins, IBM HTTP Server, and IBM BAW Upgrades**  
✅ **Selective Execution** – Only upgrades installed components  
✅ **Automatic Backup Before Upgrade**  
✅ **Rollback if Upgrade Fails**  
✅ **Pre-upgrade Checks & Post-upgrade Validation**  
✅ **Post-installation Configuration for IBM BAW**  
✅ **Per-host Upgrade Reporting**  

---

## How the Role Works

The `b_upgrade` role works in the following sequence:

### **1️⃣ Pre-checks (`precheck.yml`)**

- Ensures that the system is **ready for the upgrade** by checking disk space.
- **Captures the pre-upgrade state** of installed packages.

### **2️⃣ Identifying Installed Products (`check_installed.yml` and `check_baw_installed.yml`)**

- **Determines if WebSphere, Plugins, IBM HTTP Server, and IBM BAW are installed.**
- **Only upgrades installed products.**

### **3️⃣ Checking Available Upgrade Versions (`available_versions.yml` and `available_baw_versions.yml`)**

- Fetches the **latest available version** from configured repositories.

### **4️⃣ Checking if an Upgrade is Necessary (`no_upgrade_check.yml` and `no_baw_upgrade_check.yml`)**

- Compares installed versions against the target upgrade versions.
- **Exits early if no upgrade is needed.**

### **5️⃣ Backup (`backup.yml` and `backup_baw.yml`)**

- **Backs up only the directories requiring an upgrade**.

### **6️⃣ Executing the Upgrade**

- Runs the upgrade for **each installed product**:
  - **WebSphere (`upgrade_websphere.yml`)**
  - **WebSphere Plugins (`upgrade_plugins.yml`)**
  - **IBM HTTP Server (`upgrade_ibm_http.yml`)**
  - **IBM BAW (`upgrade_baw.yml`)**

### **7️⃣ Rollback if the Upgrade Fails (`rollback.yml` and `rollback_baw.yml`)**

- If any upgrade fails, the **rollback tasks restore the backup**.

### **8️⃣ Post-upgrade Validation (`post_upgrade.yml` and `post_upgrade_baw.yml`)**

- Captures the new versions after the upgrade.

### **9️⃣ IBM BAW Post-installation Configuration (`baw_post_config.yml`)**

- Runs additional **post-installation configurations for IBM BAW**.

### **🔟 Generate Per-host Upgrade Report (`report.yml`)**

- Generates **a detailed upgrade report** for each host.

### **1️⃣1️⃣ Restart Services & Cleanup (`post_actions.yml`)**

- Restarts WebSphere services if necessary.

---

## Variables

The role uses a centralized **`vars/` folder** for managing configurations:

```yaml
# vars/main.yml

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

---

## Upgrade Reporting

After execution, a **detailed upgrade report** is generated per host:

```text
Upgrade Version Report for dev1
=================================================
Date: YYYY-MM-DD

WebSphere Application Server:
-----------------------------
Installation Directory: /opt/IBM/WebSphere/AppServer
Pre-upgrade Version: 9.0.0
Target Version: 9.0.5
Post-upgrade Version: 9.0.5

IBM BAW:
--------
Installation Directory: /opt/IBM/BAW
Pre-upgrade Version: 23.0.0.2
Target Version: 24.0.0.1
Post-upgrade Version: 24.0.0.1

Overall Upgrade Status: Success
```

---

## Running the Role

To execute the upgrade process for a specific environment:

### **Run for Development Environment (`dev`)**

```sh
ansible-playbook -i inventory/dev/inventory.yml playbooks/upgrade_websphere.yml
```

### **Run for Production (`prod`)**

```sh
ansible-playbook -i inventory/prod/inventory.yml playbooks/upgrade_websphere.yml
```

---

## Why is `b_upgrade` Efficient?

✅ **Smart Execution:** Runs only required upgrade tasks.  
✅ **Automated Backup & Rollback:** Ensures system stability.  
✅ **Pre-checks & Post-validation:** Guarantees upgrade success.  
✅ **IBM BAW Post-configurations:** Automates additional steps.  
✅ **Per-host Reports:** Provides detailed logs.  

---

## Summary

The `b_upgrade` role **ensures a seamless, automated, and safe upgrade process** for IBM WebSphere and IBM BAW. It provides **selective execution, backup, rollback, validation, post-configuration, and reporting**.

🚀 **Now Ready for Deployment in Production!**

