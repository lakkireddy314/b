# b_upgrade Role - Detailed Guide

## Overview

The `b_upgrade` role is designed to **automate the upgrade process for IBM WebSphere Application Server, WebSphere Plugins, IBM HTTP Server, and IBM BAW (Business Automation Workflow)** while ensuring **safe execution with backup and rollback mechanisms**.

This role follows **best practices** to ensure that:
- Only **necessary upgrades occur** (skips already upgraded systems).
- **Automatic rollback is triggered** in case of upgrade failures.
- **Backup is taken before the upgrade** to prevent data loss.
- **Post-upgrade verification and IBM BAW post-configuration tasks** are executed.

---

## Features of the `b_upgrade` Role

✅ **Automated WebSphere, Plugins, IBM HTTP Server, and IBM BAW Upgrades**  
✅ **Selective Execution** – Only upgrades installed components  
✅ **Automatic Backup Before Upgrade**  
✅ **Rollback if Upgrade Fails**  
✅ **Pre-upgrade Checks & Post-upgrade Validation**  
✅ **Post-installation Configuration for IBM BAW**  
✅ **Per-host Upgrade Reporting**  

---

## **Step-by-Step Explanation of the Upgrade Process**

### **1️⃣ Pre-checks (`precheck.yml`)**  
- Ensures that the system is **ready for the upgrade** by checking **available disk space and system health**.
- **Captures the pre-upgrade state** of installed packages.

### **2️⃣ Identifying Installed Products (`check_installed.yml` and `check_baw_installed.yml`)**  
- **Determines if WebSphere, Plugins, IBM HTTP Server, and IBM BAW are installed.**
- **Skips the upgrade for uninstalled products.**

### **3️⃣ Checking Available Upgrade Versions (`available_versions.yml` and `available_baw_versions.yml`)**  
- Fetches the **latest available version** from the configured repositories.
- Compares with the **installed version**.

### **4️⃣ Checking if an Upgrade is Necessary (`no_upgrade_check.yml` and `no_baw_upgrade_check.yml`)**  
- If the **installed version matches the target version**, the role **skips the upgrade**.
- Prevents unnecessary executions.

### **5️⃣ Backup (`backup.yml` and `backup_baw.yml`)**  
- **Creates a backup before upgrading** to ensure recovery in case of failure.
- Backups only the directories of the **installed products**.

### **6️⃣ Executing the Upgrade**  

For each installed product, the upgrade is performed separately:

- **WebSphere Upgrade (`upgrade_websphere.yml`)**  
  - Runs the WebSphere upgrade only if installed.
  - Uses **IBM Installation Manager (imcl) commands** for execution.

- **WebSphere Plugins Upgrade (`upgrade_plugins.yml`)**  
  - Runs the Plugins upgrade.
  - Ensures compatibility with the WebSphere version.

- **IBM HTTP Server Upgrade (`upgrade_ibm_http.yml`)**  
  - Upgrades the IBM HTTP Server if installed.
  - Uses official IBM repositories.

- **IBM BAW Upgrade (`upgrade_baw.yml`)**  
  - Upgrades IBM BAW to **version 24.0.0.1**.
  - Runs only if BAW is installed.

### **7️⃣ Rollback if the Upgrade Fails (`rollback.yml` and `rollback_baw.yml`)**  
- If **any upgrade task fails**, the **role automatically restores from the backup**.
- Prevents **partial upgrades** or **system inconsistencies**.

### **8️⃣ Post-upgrade Validation (`post_upgrade.yml` and `post_upgrade_baw.yml`)**  
- Verifies **newly installed versions**.
- Confirms if the upgrade was **successful**.

### **9️⃣ IBM BAW Post-installation Configuration (`baw_post_config.yml`)**  
- Runs additional **post-installation configurations for IBM BAW**.
- Ensures BAW services are **properly configured after the upgrade**.

### **🔟 Generate Per-host Upgrade Report (`report.yml`)**  
- Creates a **detailed per-host upgrade report** including:
  - **Hostname**
  - **Installation directories**
  - **Pre-upgrade, target, and post-upgrade versions**
  - **Success or failure status**

### **1️⃣1️⃣ Restart Services & Cleanup (`post_actions.yml`)**  
- **Restarts WebSphere services** if necessary.
- Cleans up temporary files.

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
