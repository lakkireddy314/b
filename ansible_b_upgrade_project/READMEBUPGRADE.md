# Comprehensive Guide for b_upgrade Role

## Overview
The `b_upgrade` role is designed to **automate the upgrade process for IBM WebSphere Application Server, WebSphere Plugins, and IBM HTTP Server** while ensuring **backup and rollback capabilities** in case of failures.

## Features
✅ **Pre-checks before upgrade execution**  
✅ **Backup only when required**  
✅ **Rollback mechanism in case of failures**  
✅ **Upgrade execution based on available target versions**  
✅ **Post-upgrade validation and reporting**  
✅ **Restart WebSphere services after upgrade**  

## Directory Structure
```
roles/
└── b_upgrade/
    ├── defaults/
    │   └── main.yml
    ├── tasks/
    │   ├── main.yml
    │   ├── precheck.yml
    │   ├── pre_upgrade.yml
    │   ├── check_installed.yml
    │   ├── available_versions.yml
    │   ├── no_upgrade_check.yml
    │   ├── backup.yml
    │   ├── upgrade_websphere.yml
    │   ├── upgrade_plugins.yml
    │   ├── upgrade_ibm_http.yml
    │   ├── rollback.yml
    │   ├── post_upgrade.yml
    │   ├── report.yml
    │   ├── post_actions.yml
    ├── templates/
    │   └── upgrade_version_report.j2
```

## Execution Flow

1️⃣ **Pre-checks** (`precheck.yml`) - Ensure system readiness.  
2️⃣ **Pre-upgrade state capture** (`pre_upgrade.yml`) - Identify installed products.  
3️⃣ **Identify target upgrade versions** (`available_versions.yml`) - Fetch available packages.  
4️⃣ **Check if upgrade is required** (`no_upgrade_check.yml`) - Skip if already updated.  
5️⃣ **Backup directories if required** (`backup.yml`) - Create safety backups.  
6️⃣ **Perform upgrade** (`upgrade_websphere.yml`, `upgrade_plugins.yml`, `upgrade_ibm_http.yml`)  
7️⃣ **Rollback in case of failures** (`rollback.yml`) - Restore backup if upgrade fails.  
8️⃣ **Validate and report** (`post_upgrade.yml`, `report.yml`) - Capture results.  
9️⃣ **Restart WebSphere services** (`post_actions.yml`) - Ensure services run post-upgrade.  

## Running the Role
To execute the upgrade, use the following command based on the environment:

```sh
ansible-playbook -i inventory/dev/inventory.yml playbooks/upgrade_websphere.yml
```

## Reporting
After execution, a report (`upgrade_version_report.txt`) is generated showing:

```
Upgrade Version Report
======================
Date: YYYY-MM-DD

WebSphere Application Server:
-----------------------------
Installation Directory: /opt/dev/websphere
Pre-upgrade Version: 9.0.0
Target Version: 9.0.5
Post-upgrade Version: 9.0.5
```

This role ensures **WebSphere upgrades are automated, resilient, and safe**.

