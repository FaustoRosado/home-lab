# The Ultimate Cybersecurity Home Lab: Design, Build, & Defend

**Author:** Fausto Rosado
**Date:** 05/31/2025

---

## 📖 Abstract
This paper details the experience of designing and building an advanced cybersecurity home lab. Using a virtual setup, I created a segmented network mimicking a real corporate environment. The core components include a **pfSense firewall**, **Snort IDS/IPS**, and **Wazuh SIEM**. The lab features distinct zones:
- **Attacker Zone** (Kali Linux)
- **Victim Zone** (Metasploitable2, Windows 10)
- **DMZ** (Windows Server 2022)
- **Management Zone** (Wazuh)

This report covers tool selection, configuration, and the pedagogical value of building such a lab.

---

## 1. My First Steps
The world of cybersecurity changes so fast, and it feels like there's always something new to learn. Just knowing the theories isn't enough; you really need to get your hands dirty to understand how things work. My goal was to create a space where I could safely experiment and learn by doing.

For this project, I set up a virtual network using **VirtualBox**. The heart of my lab is a **pfSense firewall**, which controls all the traffic. I also integrated **Snort** to act as an intrusion detection and prevention system (IDS/IPS), and a **Wazuh SIEM** (Security Information and Event Management) system to collect and analyze logs from all over my lab.

To make it feel more like a real network, I divided my lab into different zones. I have an **Attacker zone**, where I can use tools to see how attacks work. There's a **Victim zone**, with machines that have known weaknesses, perfect for practicing on. I also set up a **Services/DMZ zone**, which is like a more exposed part of a network, and a **Management zone** for my security tools. This setup lets me try out different attack and defense scenarios in a controlled way.

> [!NOTE]
> **Why Zones Matter:**
> In the real world, "flat" networks are a security nightmare. If an attacker compromises one machine, they can reach everything. Segmentation (zones) limits the "blast radius" of an attack.

---

## 2. Designing My Lab: The Blueprint
When I started thinking about my lab, I wanted it to be flexible enough to try many different things, realistic enough to teach me about real-world setups, and good for practicing both attacking and defending. I decided that creating different network segments, or zones, was the way to go.

### Network Topology
My lab's network topology is built around a segmented design, with the pfSense firewall acting as the central nervous system.

![Network Topology](images/img-000.png)
*Figure 1: Lab Network Topology*

Each distinct zone is essentially its own isolated network, and pfSense sits in the middle, controlling how, or if, these zones can communicate with each other and with the outside world.

### Virtualization Platform and Network Structure
I chose **Oracle VirtualBox** because it's free, widely used, and has robust networking features like "Internal Networks" and "NAT Networks."

- **Internal Networks:** Used for isolation. Each zone (Attacker, Victim, DMZ, Management) is on its own Internal Network.
- **NAT Network:** Used for WAN. Connects pfSense to the internet via my host machine.

#### The Zones
1.  **Attacker Zone (ATTACKER_NET):**
    -   **Subnet:** `192.168.10.0/24`
    -   **Host:** Kali Linux (`192.168.10.10`)
2.  **Victim Zone (VICTIM_NET):**
    -   **Subnet:** `192.168.20.0/24`
    -   **Hosts:** Metasploitable2 (`.20`), Windows 10 (`.30`)
3.  **Services/DMZ Zone (DMZ_NET):**
    -   **Subnet:** `172.16.10.0/24`
    -   **Host:** Windows Server 2022 (`.10`)
    -   *Concept:* A Demilitarized Zone (DMZ) protects the internal network from untrusted traffic while hosting public-facing services.
4.  **Management Zone (MGMT_NET):**
    -   **Subnet:** `10.10.10.0/24`
    -   **Host:** Wazuh SIEM (`.5`)

---

## 3. The Building Blocks: Core Tools and Machines

### pfSense: In the Middle
pfSense is the central point for controlling all network traffic. It handles firewalling, routing, NAT, and DHCP. I also integrated Snort for IDS/IPS capabilities.

### Setting Up the Virtual Machines

#### The Attacker Workstation: Kali Linux
My main Attacker VM is Kali Linux, placed on the ATTACKER_NET with a static IP.

![Kali Network Config](images/img-001.png)
*Figure 2: Kali Linux Network Configuration Utility*

The IP configuration was further verified using the terminal:

![Kali ifconfig](images/img-002.png)
*Figure 3: Verifying IP with ifconfig*

#### The Target Gallery: Metasploitable2 and Windows 10
Metasploitable2 sits on the VICTIM_NET. Configuring its static IP involved editing the network interface file.

![Metasploitable Interface Config](images/img-003.png)
*Figure 4: Editing network interfaces*

I added the static IP configuration details for the `eth0` interface:

![Metasploitable Static IP](images/img-004.png)
*Figure 5: Static IP details*

After restarting networking, I verified the configuration:

![Metasploitable ifconfig](images/img-005.png)
*Figure 6: Verifying Metasploitable IP*

The Windows 10 VM in the VICTIM_NET was also verified:

![Windows 10 ipconfig](images/img-006.png)
*Figure 7: Windows 10 ipconfig*

#### The Simulated Service Host: Windows Server 2022
In the DMZ_NET, the Windows Server 2022 VM simulates a corporate server.

![Windows Server ipconfig](images/img-007.png)
*Figure 8: Windows Server 2022 ipconfig*

#### The Security Monitoring Hub: Wazuh Server
The Wazuh Server resides in the MGMT_NET. Configuring its static IP involved editing the systemd network configuration file.

![Wazuh Network Config](images/img-008.png)
*Figure 9: Wazuh systemd configuration*

After setup, I accessed the Wazuh dashboard:

![Wazuh Dashboard](images/img-009.png)
*Figure 10: Wazuh Dashboard Login*

---

## 4. Watching Out for Trouble: Security Monitoring Tools

### Snort: My Network Watchdog (IDS/IPS)
Snort uses signature-based detection to identify malicious traffic. I integrated it directly into pfSense.

![Snort Installation](images/img-010.png)
*Figure 11: Snort Package Installation in pfSense*

I configured it to monitor the ATTACKER_NET and DMZ_NET interfaces:

![Snort Interface Settings](images/img-011.png)
*Figure 12: Snort Interface Assignment*

### Wazuh: Security Command Center (SIEM)
Wazuh collects logs, analyzes them, and finds threats. Agents on the VMs send logs to the Wazuh manager, which also receives syslog data from pfSense (including Snort alerts).

> [!TIP]
> **Interactive Challenge: Generate Noise**
> Try running a simple Nmap scan from Kali against your Metasploitable machine. Watch the Snort alerts appear in pfSense, and then see them flow into your Wazuh dashboard!

---

## 5. Exploring Kali Linux: My Attacker's Toolkit
Kali Linux is the industry standard for penetration testing. Here are some tools I explored:

-   **Nmap:** For network discovery and port scanning.
-   **Metasploit:** For exploiting vulnerabilities.
-   **Wireshark:** For deep packet analysis.
-   **Burp Suite:** For web application security testing.
-   **John the Ripper / Hashcat:** For password cracking.
-   **Aircrack-ng:** For Wi-Fi security (though harder to practice in a purely virtual lab).
-   **SQLMap:** For automating SQL injection attacks.

---

## 6. Building the Lab: How It All Came Together

### Initial Virtual Environment Configuration
I started by creating the `WAN_NAT_Network` and the four internal networks in VirtualBox.

### pfSense Firewall Deployment and Configuration
I created a new VM for pfSense with **five network adapters**:

![pfSense VM](images/img-012.png)
*Figure 13: pfSense VM in VirtualBox*

![pfSense Adapters](images/img-013.png)
*Figure 14: VirtualBox Network Adapter Configuration*

During setup, I mapped the interfaces (`vtnet0`, `vtnet1`, etc.) to my zones.
Logging into the pfSense WebGUI for the first time:

![pfSense Login](images/img-014.png)
*Figure 15: pfSense WebGUI Login*

The console confirmed the successful login:

![pfSense Console Login](images/img-015.png)
*Figure 16: Console Login*

The setup wizard guided me through the configuration:

![pfSense Wizard Hostname](images/img-016.png)
*Figure 17: Setting Hostname*

![pfSense Wizard WAN](images/img-017.png)
*Figure 18: WAN Configuration*

![pfSense Wizard LAN](images/img-018.png)
*Figure 19: LAN (Attacker) Configuration*

I then configured the OPT interfaces in `Interfaces > Assignments`:

![pfSense Interface Assignments](images/img-019.png)
*Figure 20: Assigning OPT Interfaces*

Configuring static IPs for each zone:

![pfSense Static IPs](images/img-020.png)
*Figure 21: Static IP Configuration*

![pfSense Static IPs 2](images/img-021.png)
*Figure 22: More Static IPs*

![pfSense Static IPs 3](images/img-022.png)
*Figure 23: Final Static IPs*

And setting up DHCP servers:

![pfSense DHCP](images/img-023.png)
*Figure 24: DHCP Configuration*

The final console overview:

![pfSense Console Final](images/img-024.png)
*Figure 25: pfSense Console with all Interfaces*

### Firewall Policy Implementation
I implemented a "default deny" posture.
For the **LAN (ATTACKER_NET)**, I allowed access to Victim and DMZ zones:

![LAN Rules](images/img-025.png)
*Figure 26: LAN Firewall Rules*

For **VICTIM_NET**, rules were more restrictive:

![Victim Rules](images/img-026.png)
*Figure 27: Victim Zone Rules*

For **DMZ_NET**, allowing outbound access but blocking internal access:

![DMZ Rules](images/img-027.png)
*Figure 28: DMZ Firewall Rules*

For **MGMT_NET**, permitting necessary management traffic:

![Management Rules](images/img-028.png)
*Figure 29: Management Zone Rules*

---

## 7. Troubleshooting Connectivity
Building this lab wasn't without challenges. Pings failed, rules blocked traffic, and logs were my best friend.

![Troubleshooting Diagram](images/img-029.png)
*Figure 30: Troubleshooting Flow*

> [!IMPORTANT]
> **Troubleshooting Checklist:**
> 1.  **VM IP Config:** Is the gateway correct?
> 2.  **VirtualBox Settings:** Is the "Cable Connected" box checked?
> 3.  **pfSense Rules:** Remember, **ICMP** (Ping) is a specific protocol. Allowing TCP port 80 won't allow pings!
> 4.  **Rule Order:** First match wins. Put specific rules *above* general block rules.

![pfSense Logs](images/img-030.png)
*Figure 31: Checking pfSense Logs*

![Ping Test](images/img-031.png)
*Figure 32: Systematic Ping Testing*

![Apply Changes](images/img-032.png)
*Figure 33: Don't forget to Apply Changes!*

---

## 8. Why This Lab Matters
This lab is more than a project; it's a career foundation.
-   **Practical Skills:** I built this from scratch.
-   **Architecture:** I understand *why* segmentation matters.
-   **Dual Perspectives:** I can attack *and* defend.
-   **Troubleshooting:** I learned to solve real problems.

![Final Lab Overview](images/img-033.png)
*Figure 34: The Complete Lab Environment*

---

## 9. Future Work & Recommendations
This lab is a living project. Here is how I plan to expand it (and how you can too!):

### Best Practices
-   **Regular Updates:** Patch your VMs.
-   **Snapshots:** Always take a snapshot before a major change.
-   **Documentation:** Keep your network diagram up to date.

### Lab Expansion Ideas
-   **GitHub Repository:** For version control of configs and scripts.
-   **Vulnerable Web Apps:** Deploy DVWA or OWASP WebGoat in the DMZ.
-   **Security Onion:** Try a different monitoring stack.

> [!TIP]
> **Try This:**
> Clone this repo and try to replicate the setup. Can you get the Wazuh agent on the Windows Server to report back to the Manager in the MGMT zone?

---

