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


Switch> enable
Switch# configure terminal
Switch(config)# vlan 20
Switch(config-vlan)# name IT
Switch(config-vlan)# exit
Switch(config)# interface range FastEthernet 0/1 - 12
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 20
Switch(config-if-range)# exit

Switch(config)# vlan 40
Switch(config-vlan)# name Services
Switch(config-vlan)# exit
Switch(config)# interface range FastEthernet 0/13 - 15
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 40
Switch(config-if-range)# exit



Switch> enable
Switch# configure terminal
Switch(config)# vlan 30
Switch(config-vlan)# name Accounting
Switch(config-vlan)# exit
Switch(config)# interface range FastEthernet 0/1 - 3
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 30
Switch(config-if-range)# exit


Router> enable
Router# configure terminal

! HR Gateway
Router(config)# interface GigabitEthernet0/0
Router(config-if)# ip address 9.9.9.1 255.255.255.224
Router(config-if)# no shutdown
Router(config-if)# exit

! IT Gateway
Router(config)# interface GigabitEthernet0/1
Router(config-if)# ip address 9.9.9.33 255.255.255.224
Router(config-if)# no shutdown
Router(config-if)# exit

! Accounting Gateway
Router(config)# interface GigabitEthernet0/2
Router(config-if)# ip address 9.9.9.65 255.255.255.240
Router(config-if)# no shutdown
Router(config-if)# exit


! HR Pool
Router(config)# ip dhcp pool HR_POOL
Router(dhcp-config)# network 9.9.9.0 255.255.255.224
Router(dhcp-config)# default-router 9.9.9.1
Router(dhcp-config)# exit

! IT Pool
Router(config)# ip dhcp pool IT_POOL
Router(dhcp-config)# network 9.9.9.32 255.255.255.224
Router(dhcp-config)# default-router 9.9.9.33
Router(dhcp-config)# exit

! Accounting Pool
Router(config)# ip dhcp pool ACC_POOL
Router(dhcp-config)# network 9.9.9.64 255.255.255.240
Router(dhcp-config)# default-router 9.9.9.65
Router(dhcp-config)# exit

! Services Pool
Router(config)# ip dhcp pool S_POOL
Router(dhcp-config)# network 9.9.9.80 255.255.255.240
Router(dhcp-config)# default-router 9.9.9.33
Router(dhcp-config)# exit


