# Project Roadmap & Future Work

This document outlines my personal roadmap for evolving this home lab. While the core infrastructure is operational, a security lab is never truly "finished." Below are the specific areas I plan to explore next, along with the technical objectives I am looking to solve.

If you are building a similar setup, I invite you to collaborate. Contributions, suggestions, and Pull Requests are welcome.

## 1. Expanding the Target Environment

To simulate a modern enterprise attack surface, I need to introduce more complexity and containerized services beyond the current Metasploitable 2 and Windows 10 setup.

### Metasploitable 3 Deployment
**Goal:** Deploy both the Windows and Linux versions of Metasploitable 3.
**Why:** Adds modern vulnerabilities (PowerShell, newer kernels) that are relevant to current pentesting engagements.
**Technical Objective:** Automate the build process using Packer and Vagrant to ensure it integrates seamlessly into the existing `VICTIM_NET`.

### Containerized Vulnerabilities
**Goal:** Integrate a Docker host into the DMZ to run "Damn Vulnerable Web App" (DVWA) and OWASP Juice Shop.
**Why:** Container security is a critical skill set. This allows practice with exploiting and securing Docker containers, APIs, and microservices.
**Technical Objective:** Configure Docker host networking to expose containers to the `ATTACKER_NET` while maintaining isolation from the core management network.

## 2. Advanced Networking & Segmentation

Moving from simple VirtualBox internal networks to a scalable, industry-standard approach.

### VLAN Implementation
**Goal:** Refactor the network to use 802.1Q VLAN tagging.
**Why:** Simulates real-world router-on-a-stick topology where multiple zones share a single physical interface.
**Technical Objective:** Configure VirtualBox to pass VLAN tags correctly to the pfSense VM and ensure the switching logic is sound.

### VPN Remote Access
**Goal:** Configure an OpenVPN server on pfSense.
**Why:** Simulates a "Work from Home" scenario for secure remote access to the `MGMT_NET`.
**Technical Objective:** Generate certificates and configure firewall rules to allow tunnel traffic while maintaining strict isolation for internal zones.

## 3. Automation & Infrastructure as Code (IaC)

Treating infrastructure as code to reduce manual configuration time.

### Ansible Provisioning
**Goal:** Write Ansible playbooks to configure the victim machines.
**Why:** Automates the installation of services (IIS, RDP) and user creation, ensuring consistency across rebuilds.
**Technical Objective:** Manage Windows hosts via WinRM and write idempotent playbooks for state management.

## 4. Blue Team Operations

Tuning the defensive stack (Snort + Wazuh) to detect sophisticated attacks.

### Custom Detection Rules
**Goal:** Develop custom Snort rules and Wazuh decoders.
**Why:** Reduces noise and focuses on detecting specific C2 (Command and Control) beaconing and post-exploitation activities.
**Technical Objective:** Test rules against real attack traffic generated from Kali and tune out false positives.

---

## Contributing

I am treating this repository as an open roadmap. If you have solved any of these objectives in your own lab, or if you have scripts that could improve this setup, please feel free to open a Pull Request.

I am particularly interested in:
*   Vagrantfiles for automated lab spin-up.
*   Custom Wazuh dashboards for threat hunting.
*   Scripts for generating realistic background traffic.
