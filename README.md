# Cybersecurity Laboratory Environment Setup

### *Oracle VirtualBox & Kali Linux Security Lab*

[![VirtualBox](https://img.shields.io/badge/VirtualBox-7.0+-183A61?logo=virtualbox&logoColor=white)](https://www.virtualbox.org/)
[![Kali Linux](https://img.shields.io/badge/Kali-Linux-557C94?logo=kalilinux&logoColor=white)](https://www.kali.org/)
[![Networking](https://img.shields.io/badge/Networking-NAT_Network-0073e6?logo=cisco&logoColor=white)](https://www.virtualbox.org/manual/ch06.html)
[![License](https://img.shields.io/badge/License-Educational-brightgreen)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Completed-success)](https://github.com/pradheepa73/Cybersecurity-Lab-Setup)

### 📂 [GitHub Repository](https://github.com/pradheepa73/Cybersecurity-Lab-Setup)
## Executive Summary

This document outlines the setup and configuration of an isolated cybersecurity laboratory environment utilizing **Oracle VirtualBox** as the virtualization platform and **Kali Linux** as the security workstation. The laboratory provides a controlled, isolated network environment designed for security training and penetration testing exercises.

The implementation establishes a dedicated VirtualBox NAT Network with static IP addressing, ensuring network isolation while maintaining Internet connectivity for tool updates.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Objectives](#objectives)
- [Tools & Technologies](#tools--technologies)
- [Network Architecture](#network-architecture)
- [Implementation Steps](#implementation-steps)
- [Verification & Testing](#verification--testing)
- [Project Structure](#project-structure)
- [Operational Commands](#operational-commands)
- [Use Cases](#use-cases)
- [Troubleshooting Guide](#troubleshooting-guide)
- [Conclusion](#conclusion)
- [Author Information](#author-information)
- [License](#license)

---

## Project Overview

The Cybersecurity Laboratory Environment is a purpose-built virtual infrastructure designed to facilitate secure security testing and skill development. By leveraging Oracle VirtualBox's NAT Networking capabilities, this implementation creates an isolated subnet where security professionals and students can safely conduct penetration testing and vulnerability assessments.

**Key Benefits:**
- 🔒 Network isolation from host systems
- 🌐 Internet access for tool updates
- 📊 Static IP addressing for consistent access
- 🛡️ Safe environment for security testing

---

## Objectives

The following objectives were achieved through this implementation:

| # | Objective | Status |
|---|-----------|--------|
| 1 | Deploy Kali Linux as a dedicated cybersecurity workstation | ✅ |
| 2 | Create and configure a dedicated VirtualBox NAT Network | ✅ |
| 3 | Assign static IP addressing to the Kali Linux interface | ✅ |
| 4 | Configure and verify default gateway settings | ✅ |
| 5 | Validate gateway connectivity through ICMP testing | ✅ |
| 6 | Confirm Internet connectivity for external access | ✅ |
| 7 | Document all configuration parameters and procedures | ✅ |

---

## Tools & Technologies

| Component | Technology | Purpose |
|-----------|------------|---------|
| **Hypervisor** | Oracle VirtualBox | Virtual machine management and network virtualization |
| **Guest OS** | Kali Linux | Penetration testing and security auditing platform |
| **Shell Environment** | Bash / Linux Terminal | Command-line configuration and system administration |
| **Network Type** | VirtualBox NAT Network | Isolated subnet with NAT gateway functionality |

---

## Network Architecture

### Network Topology

```
┌─────────────────────────────────────────────────────────────┐
│                    Host System                              │
│  ┌─────────────────────────────────────────────────────┐   │
│  │            Oracle VirtualBox                        │   │
│  │  ┌─────────────────────────────────────────────┐   │   │
│  │  │         CyberLab-NAT Network                │   │   │
│  │  │         10.0.0.0/24                        │   │   │
│  │  │  ┌─────────────────────────────────┐       │   │   │
│  │  │  │  Gateway: 10.0.0.1             │       │   │   │
│  │  │  │  (VirtualBox NAT Router)        │       │   │   │
│  │  │  └──────────────┬──────────────────┘       │   │   │
│  │  │                 │                          │   │   │
│  │  │  ┌──────────────▼──────────────────┐       │   │   │
│  │  │  │  Kali Linux VM                 │       │   │   │
│  │  │  │  IP: 10.0.0.2/24              │       │   │   │
│  │  │  │  Gateway: 10.0.0.1             │       │   │   │
│  │  │  │  Interface: eth0               │       │   │   │
│  │  │  └─────────────────────────────────┘       │   │   │
│  │  └─────────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### Network Configuration Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| **NAT Network Name** | `CyberLab-NAT` | VirtualBox NAT Network identifier |
| **Network Address** | `10.0.0.0/24` | IPv4 subnet |
| **Gateway Address** | `10.0.0.1` | VirtualBox NAT gateway |
| **Kali Linux IP** | `10.0.0.2` | Static IP assigned to Kali workstation |
| **Network Interface** | `eth0` | Primary network interface |
| **DHCP Status** | Disabled | Static configuration used |
| **Netmask** | `255.255.255.0` | Standard Class C subnet mask |

---

## Implementation Steps

### 1. VirtualBox Network Adapter Configuration

The Kali Linux virtual machine's network adapter was configured to utilize the dedicated `CyberLab-NAT` network:

1. Open VirtualBox Manager
2. Select the Kali Linux VM
3. Navigate to **Settings** → **Network**
4. Set **Adapter 1** to **NAT Network**
5. Select `CyberLab-NAT` from the dropdown
6. Confirm configuration

**Screenshot:**
<img width="1919" height="1131" alt="01_VirtualBox_Network_Adapter" src="https://github.com/user-attachments/assets/37e17b97-c822-43b2-a2f3-0c14dc36470c" />

---

### 2. Static IP Address Assignment

The Kali Linux network interface was configured with a static IP address:

```bash
# Assign static IP address to eth0 interface
sudo ip addr add 10.0.0.2/24 dev eth0

# Activate the network interface
sudo ip link set eth0 up
```

**Verification:**
```bash
# Confirm IP assignment
ip addr show eth0
```

**Result:** IP address `10.0.0.2/24` confirmed on interface `eth0`

**Screenshot:** 
<img width="1920" height="1040" alt="02_Kali_IP_Address" src="https://github.com/user-attachments/assets/c74faa5e-4b41-481a-875a-a1732621b413" />

---

### 3. Default Gateway Configuration

The default gateway was configured to route traffic through the VirtualBox NAT gateway:

```bash
# Add default route via gateway
sudo ip route add default via 10.0.0.1
```

**Verification:**
```bash
# Display routing table
ip route show
```

**Result:** Default route via `10.0.0.1` confirmed

**Screenshot:** 
<img width="1920" height="1040" alt="03_Default_Gateway" src="https://github.com/user-attachments/assets/0455f27c-0342-472c-8811-05aaf38db9a4" />


---

### 4. Gateway Connectivity Validation

ICMP echo requests were used to validate connectivity to the configured gateway:

```bash
# Send 4 ICMP echo requests to gateway
ping -c 4 10.0.0.1
```

**Result:** Gateway responded successfully

**Screenshot:** 
<img width="1920" height="1040" alt="04_Ping_Gateway" src="https://github.com/user-attachments/assets/b0b45433-495a-47d3-af9d-59396243e10e" />


---

### 5. Internet Connectivity Validation

External network connectivity was verified:

```bash
# Test Internet connectivity
ping -c 4 8.8.8.8
```

**Result:** Internet connectivity confirmed

**Screenshot:** 
<img width="1920" height="1040" alt="05_Ping_Internet" src="https://github.com/user-attachments/assets/7fbea553-477e-40a8-9b6c-ded3510897a4" />


---

### 6. VirtualBox NAT Network Configuration

A dedicated VirtualBox NAT Network named `CyberLab-NAT` was created with the following configuration:

**Method:** VirtualBox Manager GUI

**Configuration:**
- **Network Name:** CyberLab-NAT
- **Network CIDR:** 10.0.0.0/24
- **DHCP Server:** Disabled

**Screenshot:** 
<img width="1913" height="1128" alt="06_NAT_Network" src="https://github.com/user-attachments/assets/060baf92-aea6-4ec6-832a-d1b8ecb64a98" />


---

## Verification & Testing

### Comprehensive Validation Summary

| Validation Point | Status | Command | Result |
|------------------|--------|---------|--------|
| Network Interface | ✅ Passed | `ip addr show eth0` | IP 10.0.0.2/24 confirmed |
| Static IP Assignment | ✅ Passed | `ip addr show eth0` | IP configuration verified |
| Default Gateway | ✅ Passed | `ip route show` | Route via 10.0.0.1 confirmed |
| Gateway Connectivity | ✅ Passed | `ping -c 4 10.0.0.1` | Gateway responded |
| Internet Connectivity | ✅ Passed | `ping -c 4 8.8.8.8` | Internet access confirmed |
| NAT Network Configuration | ✅ Passed | VirtualBox Network Manager | CyberLab-NAT active |

---

## Project Structure

```
Cybersecurity-Lab-Setup/
│
├── README.md                           # Project documentation
│
└── Screenshots/                        # Configuration evidence
    ├── 01_VirtualBox_Network_Adapter.png
    ├── 02_Kali_IP_Address.png
    ├── 03_Default_Gateway.png
    ├── 04_Ping_Gateway.png
    ├── 05_Ping_Internet.png
    └── 06_NAT_Network.png
```

---

## Operational Commands

### Quick Reference

| Task | Command |
|------|---------|
| **View IP Configuration** | `ip addr show eth0` |
| **View Routing Table** | `ip route show` |
| **Test Gateway** | `ping -c 4 10.0.0.1` |
| **Test Internet** | `ping -c 4 8.8.8.8` |
| **View Interface Status** | `ip link show eth0` |

---

## Use Cases

This cybersecurity laboratory environment supports the following activities:

### Security Testing
- 🔍 Network reconnaissance and enumeration
- 🛡️ Vulnerability assessment and scanning
- 💻 Penetration testing exercises
- 📡 Traffic analysis and packet inspection

### Educational Applications
- 🎓 Security awareness training
- 🏫 Academic coursework in cybersecurity
- 🎯 Capture The Flag (CTF) challenge preparation
- 📚 Self-paced security skill development

### Research Activities
- 🔬 Security tool evaluation
- 📊 Attack vector analysis
- 🛠️ Custom security tool development

---

## Troubleshooting Guide

| Issue | Symptom | Solution |
|-------|---------|----------|
| **No Internet Connectivity** | `ping 8.8.8.8` fails | Verify gateway: `ping 10.0.0.1` |
| **DNS Resolution Failure** | Domain names unresolved | Configure DNS: `echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf` |
| **Interface Not Active** | `ip addr` shows interface down | Activate: `sudo ip link set eth0 up` |
| **NAT Network Missing** | "NAT Network not found" | Restart VirtualBox services or recreate network |
| **Route Not Persisting** | Gateway lost after reboot | Re-add route: `sudo ip route add default via 10.0.0.1` |

---

## Security Considerations

> ⚠️ **IMPORTANT:** This laboratory is designed for controlled environments only.

| Consideration | Description |
|---------------|-------------|
| **Network Isolation** | NAT Network isolates VMs from host network |
| **No DHCP** | Static configuration reduces attack surface |
| **Ethical Use** | Testing only on systems you own or have explicit permission to test |

---

## Conclusion

The Cybersecurity Laboratory Environment was successfully established using Oracle VirtualBox and Kali Linux. All configuration objectives were met:

✅ **Network Infrastructure:** Dedicated CyberLab-NAT network created with proper subnet addressing

✅ **System Configuration:** Kali Linux configured with static IP addressing and default gateway

✅ **Connectivity Validation:** Gateway and Internet connectivity confirmed through ICMP testing

✅ **Documentation:** Complete configuration parameters and procedures documented

This laboratory environment provides a secure, isolated, and controlled platform for cybersecurity training, security testing, and research activities.

---

## 👤 Author

<div align="center">

**Pradheepa M**  
*Cybersecurity Enthusiast*

[![GitHub](https://img.shields.io/badge/GitHub-pradheepa73-181717?logo=github&logoColor=white)](https://github.com/pradheepa73)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Pradheepa-0A66C2?logo=linkedin&logoColor=white)](https://www.linkedin.com/in/pradheepa-m-051728372)

</div>

## License

**Educational Use Only**

This project and its documentation are intended solely for educational and training purposes. All security testing and penetration testing activities must be performed exclusively in authorized environments with explicit permission from system owners.

Unauthorized access to computer systems, networks, or data is prohibited and may violate applicable laws and regulations. Users are responsible for ensuring their activities comply with all relevant laws, regulations, and organizational policies.

---

<div align="center">

### 🛡️ *Stay Secure. Stay Aware.*

### Made with ❤️ by Pradheepa M

</div>

