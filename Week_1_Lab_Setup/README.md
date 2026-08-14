# Local Cybersecurity Penetration Testing Lab (Week 1)

## 📹 Video Demonstration
*(Include a short 30–60 second video clip or GIF here demonstrating your lab in action, such as executing a successful ping sweep across the target VMs.)*
`![Lab Demo Video](assets/lab-demo.gif)`

---

## ✅ Project Overview and Objectives
As a computer engineering graduate aspiring to specialize in networking and cybersecurity, I built this locally hosted penetration testing lab to gain practical, hands-on experience. 

The primary objectives of this project are:
* **Safe Sandboxing:** To create an isolated environment where network vulnerabilities can be exploited, and malware can be analyzed without posing any risk to the physical host machine or the broader local area network (LAN).
* **Network Infrastructure Mastery:** To manually configure static routing, subnets, and gateway communication across diverse operating systems.
* **Security Tool Familiarization:** To establish a reliable base of operations (Kali Linux) for practicing penetration testing methodologies on legacy and modern systems.

---

## ✅ Configuration Tables (Real Values)

### Host Machine Specifications
To successfully run multiple VMs simultaneously, strict resource allocation was required on the host system:
| Component | Specification |
| :--- | :--- |
| **Machine** | HP Pavilion |
| **GPU** | NVIDIA MX150 (4GB VRAM) |
| **Memory** | 8 GB RAM |
| **Hypervisor** | Oracle VirtualBox 7.x |

### Virtual Network Configuration
All machines are isolated on a custom VirtualBox switch to prevent accidental external exposure.
| Setting | Value |
| :--- | :--- |
| **Network Type** | NAT Network (`NatNetwork`) |
| **Subnet / CIDR** | `10.0.0.0/24` |
| **Default Gateway** | `10.0.0.1` |
| **DHCP Server** | Enabled |
| **Promiscuous Mode** | Allow All |

### Target & Attacker IP Allocations
| Virtual Machine | Role | IP Address |
| :--- | :--- | :--- |
| **Kali Linux 2026.1** | Attacker Workstation | `10.0.0.2` |
| **Windows 7** | Legacy Target | `10.0.0.7` |
| **Android 9.x** | Mobile Target | `10.0.0.9` |
| **Windows 10** | Client Target | `10.0.0.10` |
| **Windows Server 2016** | Infrastructure Target | `10.0.0.11` |
| **Windows 11** | Modern Target | `10.0.0.16` |

---

## ✅ Step-by-Step Setup

### Step 1: Configuring the Virtual Infrastructure
* **What I did:** Installed Oracle VirtualBox and created a new NAT Network (`NatNetwork`) with a `10.0.0.0/24` address space.
* **Why I did it:** To provide a virtual router that allows guest VMs to talk to each other while securely sharing the host's outbound internet connection.
* **Screenshot:**
  ![NAT Network Settings](assets/image_0a426d.png)

### Step 2: Deploying the Attacker Machine (Kali Linux)
* **What I did:** Imported the pre-compiled Kali Linux VM image, attached its network adapter to the new `NatNetwork`, and manually assigned the static IP `10.0.0.2`.
* **Why I did it:** Kali Linux serves as the central command post. Setting a static IP ensures that target machines and reverse shells always know exactly where to send traffic back to.
* **Screenshot:**
  ![Kali Network Settings](assets/image_09e09e.png)

### Step 3: Provisioning Target Environments
* **What I did:** Spun up various Windows and Android VMs, attached their adapters to `NatNetwork`, and assigned their respective static IPs according to the configuration table.
* **Why I did it:** Real-world networks consist of mixed environments. Having legacy systems (Win 7), servers (Server 2016), and modern clients (Win 10/11) allows for testing a wide array of specific exploits.
* **Screenshot:**
  *(Placeholder for Target VMs Configuration Screenshot)*

---

## ✅ Verification Commands and Results

**1. Gateway Connectivity Test:**
* **Command:** `ping -c 4 10.0.0.1`
* **Result:** 4 packets transmitted, 4 received, 0% packet loss.
* **Screenshot:** 
  ![Ping Gateway](assets/image_09dc01.png)

**2. Interface Configuration Check:**
* **Command:** `ip a show eth0`
* **Result:** Interface displayed `inet 10.0.0.2/24`.
* **Screenshot:** *(Placeholder for ip a output)*

**3. Inter-VM Connectivity (Cross-Talk):**
* **Command:** `ping -c 4 10.0.0.10` (Pinging Windows 10)
* **Result:** 4 packets transmitted, 4 received, 0% packet loss.
* **Screenshot:** *(Placeholder for pinging Windows 10)*

---

## ✅ Problems Faced and How They Were Solved

### 1. Hardware Virtualization Blocked (VT-x)
* **The Problem:** VirtualBox crashed on launch with the error `VERR_VMX_MSR_ALL_VMX_DISABLED`.
* **The Solution:** The host laptop's BIOS had virtualization disabled by default. I rebooted the machine, accessed the BIOS settings using `F10`, navigated to the System Configuration tab, and enabled Intel Virtualization Technology.

### 2. Kali Linux Display Scaling
* **The Problem:** The Kali Linux graphical interface booted into a low-resolution, heavily pixelated state that was difficult to navigate.
* **The Solution:** I used the VirtualBox Host key (`Right Ctrl + C`) to disable Scaled Mode, enabled "Auto-resize Guest Display" from the View menu, and ensured the `virtualbox-guest-x11` additions were fully updated via the terminal.

### 3. Kali Network Connectivity Drops
* **The Problem:** NetworkManager in newer versions of Kali Linux sometimes drops static IP connections due to duplicate address timeouts.
* **The Solution:** Ran `sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0` to bypass the Duplicate Address Detection check, permanently stabilizing the connection.

---

## ✅ Snapshot & Backup Strategy
* Once a VM was configured and passed the ping tests, it was powered down, and a baseline snapshot (`My fresh kali linux`) was created. 
* VirtualBox allows me to instantly roll back any machine to this state in seconds after detonating malware or breaking a system.
* **Screenshot:**
  ![Snapshot Manager](assets/image_09d936.png)

---

## ✅ What I Learned
Through this project, I gained practical knowledge in:
* **Hypervisor Networking:** Understanding the fundamental differences between Bridged, Host-Only, NAT, and custom NAT Networks.
* **Resource Optimization:** Successfully running a multi-machine testing environment on an 8GB RAM host by carefully balancing memory allocation across guest OSs.
* **System Troubleshooting:** Diagnosing low-level hypervisor errors (VT-x) and resolving Linux GUI scaling and driver issues.
* **Network Infrastructure:** Subnetting (`/24`), assigning static IP addressing schemes, and utilizing ICMP requests for network validation.
