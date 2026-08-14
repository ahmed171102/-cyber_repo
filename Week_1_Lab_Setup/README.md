# Week 1: Lab Setup & Configuration

This documentation covers the initial lab setup based strictly on the configuration steps captured in the project screenshots.

---

## 1. VirtualBox NAT Network Configuration
A custom NAT Network was created in VirtualBox to manage local network routing and DHCP services for the virtual environment.
* **Network Name:** `NatNetwork`
* **IPv4 Prefix:** `10.0.0.0/24`
* **DHCP Server:** Enabled

![NAT Network Settings](assets/image_0a426d.png)

---

## 2. Kali Linux Static IP Configuration
Inside the Kali Linux virtual machine, the network connection was configured manually to assign a static IP address within the NAT network range.
* **Connection Name:** Wired connection 1
* **Method:** Manual
* **Address:** `10.0.0.2`
* **Netmask:** `24`
* **Gateway:** `10.0.0.1`
* **DNS Servers:** `10.0.0.1`

![Kali Static IP Configuration](assets/image_09e09e.png)

---

## 3. Network Verification (Gateway Ping Test)
To verify network connectivity and ensure the Kali machine can communicate with the virtual gateway, an ICMP ping test was executed in the terminal.
* **Command:** `ping -c 4 10.0.0.1`
* **Result:** 4 packets transmitted, 4 received, **0% packet loss**.

![Gateway Ping Test](assets/image_09dc01.png)

---

## 4. Snapshot & Backup Strategy
To secure a clean working state of the virtual machine before proceeding with further testing or security labs, a system snapshot was captured.
* **Snapshot Name:** `My fresh kali linux`

![Snapshot Manager](assets/image_09d936.png)
