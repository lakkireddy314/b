# Ansible b_upgrade Project

This project contains an Ansible role (`b_upgrade`) designed to **upgrade IBM WebSphere Application Server, WebSphere Plugins, and IBM HTTP Server** across different environments.

---

## **1. Directory Structure**

```
ansible_b_upgrade_project/
├── roles/
│   └── b_upgrade/
│       ├── defaults/main.yml
│       ├── tasks/
│       │   ├── main.yml
│       │   ├── precheck.yml
│       │   ├── pre_upgrade.yml
│       │   ├── check_installed.yml
│       │   ├── available_versions.yml
│       │   ├── no_upgrade_check.yml
│       │   ├── backup.yml
│       │   ├── upgrade_websphere.yml
│       │   ├── upgrade_plugins.yml
│       │   ├── upgrade_ibm_http.yml
│       │   ├── rollback.yml
│       │   ├── post_upgrade.yml
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

---

## **2. Inventory Setup**
The inventory is divided into **four environments** (`dev`, `intg`, `perf`, and `prod`). 

Each environment has its own `inventory.yml` file.

### **Inventory: `inventory/dev/inventory.yml` (Dev + Unit as Sub-Children)**
```yaml
all:
  children:
    BAW:
      children:
        dev:
          hosts:
            dev1: { ansible_host: 192.168.1.11 }
            dev2: { ansible_host: 192.168.1.12 }
            dev3: { ansible_host: 192.168.1.13 }
            dev4: { ansible_host: 192.168.1.14 }
          children:
            unit:
              hosts:
                unit1: { ansible_host: 192.168.2.11 }
                unit2: { ansible_host: 192.168.2.12 }
                unit3: { ansible_host: 192.168.2.13 }
```

### **Inventory: `inventory/intg/inventory.yml`**
```yaml
all:
  children:
    BAW:
      children:
        intg:
          hosts:
            intg1: { ansible_host: 192.168.3.11 }
            intg2: { ansible_host: 192.168.3.12 }
            intg3: { ansible_host: 192.168.3.13 }
```

### **Inventory: `inventory/perf/inventory.yml`**
```yaml
all:
  children:
    BAW:
      children:
        perf:
          hosts:
            perf1: { ansible_host: 192.168.4.11 }
            perf2: { ansible_host: 192.168.4.12 }
            perf3: { ansible_host: 192.168.4.13 }
```

### **Inventory: `inventory/prod/inventory.yml`**
```yaml
all:
  children:
    BAW:
      children:
        prod:
          hosts:
            prod1: { ansible_host: 192.168.5.11 }
            prod2: { ansible_host: 192.168.5.12 }
            prod3: { ansible_host: 192.168.5.13 }
            prod4: { ansible_host: 192.168.5.14 }
```

---

## **3. Running the Playbook for a Specific Environment**
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

---

## **4. Features of the `b_upgrade` Role**
✅ **Modular and Structured** role  
✅ **Supports multiple environments**  
✅ **Automatic rollback if upgrade fails**  
✅ **Prevents unnecessary upgrades**  
✅ **Detailed reporting for tracking**  

---

## **5. Role Execution Breakdown**
Each task in `b_upgrade` is executed in **sequence**:

1️⃣ **Pre-checks (`precheck.yml`)**  
2️⃣ **Capture Pre-upgrade Versions (`pre_upgrade.yml`)**  
3️⃣ **Check Installed Software (`check_installed.yml`)**  
4️⃣ **Fetch Available Versions (`available_versions.yml`)**  
5️⃣ **Skip Upgrade if Already Updated (`no_upgrade_check.yml`)**  
6️⃣ **Backup Directories (`backup.yml`)**  
7️⃣ **Execute Upgrade (`upgrade_websphere.yml`, `upgrade_plugins.yml`, `upgrade_ibm_http.yml`)**  
8️⃣ **Rollback if Upgrade Fails (`rollback.yml`)**  
9️⃣ **Capture Post-upgrade Versions (`post_upgrade.yml`)**  
🔟 **Generate Upgrade Report (`report.yml`)**  

---

## **6. Reporting**
A report is generated at `upgrade_version_report.txt` after execution. It includes:

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

---

## **7. Summary**
This project is structured for **WebSphere upgrades** with:

✔️ **Multi-environment support (Dev, Intg, Perf, Prod)**  
✔️ **Rollback mechanism in case of failure**  
✔️ **Comprehensive upgrade reporting**  

🚀 **Easily extendable for future environments!**  

---
