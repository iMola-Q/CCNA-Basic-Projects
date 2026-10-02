# CCNA-Basic-Projects
A collection of practical CCNA network topology designs, VLSM subnetting implementations, VLAN configurations, and Cisco Packet Tracer labs.




# CCNA Network Subnetting & Configuration Project (Cisco Packet Tracer)

## 📌 Overview
This project presents a complete implementation of a CCNA-level enterprise network design based on **VLSM (Variable Length Subnet Masking)**. The target network connects **35 hosts** across **4 distinct departments/subnets** derived from the `9.9.9.0/24` block, resolving router port limitations via VLAN segmentation and multi-switched structures.

---

## 📐 Network Requirements & Subnetting Scheme

**Base Network:** `9.9.9.0/24`

| Department / Subnet | Host Count | Block Size | Subnet Mask | Subnet ID | First Usable IP | Last Usable IP | Broadcast IP |
| :--- | :---: | :---: | :--- | :--- | :--- | :--- | :--- |
| **HR** | 20 | 32 | `255.255.255.224` (/27) | `9.9.9.0` | `9.9.9.1` | `9.9.9.30` | `9.9.9.31` |
| **IT** | 12 | 32 | `255.255.255.224` (/27) | `9.9.9.32` | `9.9.9.33` | `9.9.9.62` | `9.9.9.63` |
| **Accounting (Acc)** | 3 | 16 | `255.255.255.240` (/28) | `9.9.9.64` | `9.9.9.65` | `9.9.9.78` | `9.9.9.79` |
| **Services (S)** | 3 | 16 | `255.255.255.240` (/28) | `9.9.9.80` | `9.9.9.81` | `9.9.9.94` | `9.9.9.95` |

---

## 🛠️ Key Network Configurations

### 1. Switch VLAN Configuration
To bypass hardware port limitations on the central router, VLAN assignment was used across switches:

#### **HR Switch (VLAN 10):**
```syntax
Switch> enable
Switch# configure terminal
Switch(config)# vlan 10
Switch(config-vlan)# name HR
Switch(config-vlan)# exit
Switch(config)# interface range FastEthernet 0/1 - 20
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# exit
