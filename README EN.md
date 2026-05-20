This project presents the implementation of a secure network architecture and a mini virtualized Security Operations Center (SOC) under the **Proxmox VE** hypervisor. The objective is to segment network traffic using a virtual firewall (**pfSense**), to host exposed and internal services, and to implement a real-time monitoring solution (**Netdata**) as well as active protection (**Fail2ban**) against simulated attacks from an external machine (**Kali Linux**).
# SOC Architecture & Network Segmentation on Proxmox VE

This project presents the implementation of a secure network architecture and a virtualized mini Security Operations Center (SOC) hosted under the **Proxmox VE** hypervisor. The primary objective is to segment network traffic using a virtual firewall (**pfSense**), host both exposed and internal services, and deploy a real-time monitoring solution (**Netdata**) alongside active defense mechanisms (**Fail2ban**) to counter simulated attacks launched from an external machine (**Kali Linux**).

---

## 🔍 Architecture Overview

The entire infrastructure is virtualized on a **Proxmox VE** hypervisor (Host IP: `192.168.6.136`). Core routing and security policies are managed by a **pfSense** virtual appliance, which interconnects four distinct zones (WAN, LAN, DMZ, SOC).

### Conceptual Framework

* **External Attacker** (VMware / Kali Linux) → Targets the **WAN** interface of the firewall (pfSense).
* **pfSense** → Inspects, filters, and distributes traffic across the **LAN**, **DMZ**, and **SOC** segments.
* **DMZ Server** → Hosts a monitored Apache web server.
* **SOC Server** → Centralizes performance metrics and oversees operational activity.
* **LAN Client** → Represents a standard internal user workstation for legitimate access.

---

## 🌐 IP Addressing Plan & Segmentation

Network segmentation is enforced via dedicated interfaces provisioned on the pfSense firewall:

| Network Zone | Subnet / IP CIDR | pfSense Interface IP | Role / Description |
| --- | --- | --- | --- |
| **WAN** | `192.168.6.0/24` | `192.168.6.138` | Internet Access / Exposed external interface |
| **LAN** | `192.168.10.0/24` | `192.168.10.1` | Internal trusted corporate user network |
| **DMZ** | `192.168.20.0/24` | `192.168.20.1` | Demilitarized Zone (Publicly accessible services) |
| **SOC** | `192.168.30.0/24` | `192.168.30.1` | SOC Network / Dedicated monitoring & supervision |

---

## 🛠️ Component Roles & Services

### 1. Firewall / Router: pfSense

* **Core Functions:** Stateful firewalling, inter-VLAN routing, NAT (Network Address Translation), and network isolation.
* **Additional Security:** Capability to integrate an IDS/IPS engine such as *Suricata* (optional) for gateway-level intrusion detection.

### 2. Client Node (LAN)

* **Component:** Linux Debian workstation (VM or LXC container).
* **IP Address:** `192.168.10.10`
* **Role:** Acts as a legitimate internal user executing standard corporate operations.

### 3. DMZ Server (LXC Container)

* **IP Address:** `192.168.20.10`
* **Enabled Services:**
* **Apache (Web Server):** A HTTP daemon listening on port 80 to host the public-facing web application.
* **Netdata Agent:** Collects local system performance metrics in real time and streams telemetry data to the SOC collector.



### 4. SOC Server (LXC Container)

* **IP Address:** `192.168.30.10`
* **Enabled Services:**
* **Netdata Stream Receiver & Dashboard:** Centralizes metrics ingested from the DMZ agent into a consolidated web dashboard.
* **Fail2ban (Detection/Prevention):** Parses system and application logs to identify malicious behavior and dynamically apply automated IP bans.



### 5. Attacker Node (VMware)

* **Component:** Kali Linux operating system.
* **IP Address:** `192.168.6.128`
* **Role:** External offensive machine dedicated to penetration testing and simulating cyber attacks directed at the WAN perimeter.

---

## 🔀 Network Flows and Communications

The infrastructure relies on four primary categories of network traffic, strictly regulated by the pfSense firewall:

1. **Internet Egress:** Outbound traffic required for system updates and legitimate web browsing.
2. **Internal Communication (Legitimate):** Administrative or operational traffic allowed between trusted subnets.
3. **Attack Traffic (Kali ➔ WAN):** Simulated offensive vectors targeting exposed infrastructure through the firewall's external interface.
4. **Netdata Stream (DMZ Agent ➔ SOC Stream Receiver):** Continuous telemetry traffic routing performance metrics from the DMZ to the isolated SOC network.

---

## 🔒 Security Scenarios & Attack Simulations

### Scenario A: Real-Time Telemetry & Monitoring

The **Netdata Agent** in the DMZ continuously pushes system metrics to the **SOC Master**. In the event of a sudden performance bottleneck or network anomaly on the Apache server, the SOC analyst can immediately isolate the root cause on the centralized dashboard.

### Scenario B: Active Intrusion Prevention

1. The **Kali Linux** node initiates an offensive operation (e.g., an SSH brute-force attack or an aggressive web directory scan against the Apache server).
2. **Fail2ban** actively analyzes log streams to detect anomalous authentication failures or unauthorized requests.
3. **Fail2ban** dynamically triggers iptables/firewall rules to drop incoming traffic from the attacker's IP address, mitigating the threat autonomously.

---

## 📌 Configuration Notes & Best Practices

* **Strict Isolation:** Inter-subnet traffic rules follow an explicit deny-all policy by default.
* **Least Privilege:** Access is granted only to ports strictly required for applications (e.g., TCP Port 80 for Apache HTTP traffic).
* **Out-of-Band Monitoring:** Utilizing a dedicated telemetry stream ensures that the monitoring control plane is never exposed directly to the public Internet or the untrusted DMZ.