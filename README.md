# NAS-secure-recovery-sandbox
Isolated VirtualBox sandbox enviorment built to safely access a potentially infected WD MyCloud Nas and recover data

# Maleware Isolation & NAS Data Recovery Lab 
A securce, isolated sandbox enviroment built with Oracle VirtualBox to analyze a potentially infected Western Digital (WD) MyCloud EX2 Ultra NAS and safely recover legacy data (family photographs) without risking the host operating system. 

## Project Overview 
**Goal: ** Secure data recovery from a suspected maleware-infected network storage device.
**Core Technology:** Oracle VirtulaBox 7.2 + Extension Pack.
**Sandbox OS: ** Linux Ubuntu Desktop (LTS) - chosen for its inherent immunity against typical Windows-based -executable maleware.
**Network Strategy: ** Bridged Adapter configuration coupled with physical router isolation (disabling Wi-Fi) to prevent lateral movement. 

## Security Architecture & Network Design 

### 1. The Shield-VM Concept (Host Protection)
Running a dedicated Linux Virtual Machine (VM) acts as a digital cleanroom. By executing all data interaction inside the guest OS, the Windows host machine remains completely untouched by potential malware triggers. 

### 2. Network Layout: Bypassing the Host safely
The Challenge: The WD MyCloud EX2 Ultra runs on proprietary ARM architecture. Its firmware cannot be rund directly as a virtual disk inside Virtulabox. Therefore, the physical NAS msut stay connected to the local network.
The Solution (Bridged Mode): Instad of NAT or Host-Only, the VM utilizes a "Bridge Adapter". This configures the VM as completely independent machine with its own IP address from the router. 
The Security Layer: This configuration prevents "Lateral Movement" (the virus trying to hop from the NAS to the main PC via local network), as the main PC's network stack is bypassed during the data transmission. 


## Project Journal 

### Day 1: Virtualization Setup & Sandbox Prep
* Installed Oracle VirtualBox 7.2 and the corresponding Extension Pack for full hardware integration.
* Created a virtual machine hardware profile ("NAS-Testlabor") allocating 4 GB RAM and 2 vCPUs.
* Configured the virtual network interface card (NIC) to **Bridged Mode**.
* Downloaded the official Ubuntu Linux Desktop ISO installer and mounted it as a virtual optical drive.

### Day 2: Linux Sandbox Installation & Network Verification
* **OS Deployment:** Successfully installed Linux Ubuntu Desktop inside the "NAS-Testlabor" virtual machine using the newly created "ISOs" directory structure. 
* **Security Validation:** Confirmed that the "Erase disk and install" warning only affected the isolated 20 GB virtual drive, keeping the Windows host completely safe. 
* **Network Bridge Testing:**
  * Encountered a "100% packet loss" during a traditional terminal "ping" test due to the router/firewall blocking ICMP traffic.
  * **Package Management:** Discovered that "curl" was not pre-installed on the fresh OS. Successfully updated the package lists and installed the utility via the terminal.
  * **Final Verification:** Verified full network functionality and successful internet access via successfully streaming media in Firefox and running the command `curl -I https://www.google.com`, which returned a valid "HTTP/2 301" redirect status.
* **System State Frozen:** Created a VirtualBox Snapshot named "Linux installed" to freeze this clean, verified state before any interaction with the physical NAS takes place.


