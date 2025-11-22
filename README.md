# The Ultimate Cybersecurity Home Lab

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Platform](https://img.shields.io/badge/platform-VirtualBox-orange.svg)
![Tools](https://img.shields.io/badge/tools-Kali%20%7C%20pfSense%20%7C%20Snort%20%7C%20Wazuh-red.svg)

> **Design, Build, & Defend.** A comprehensive guide to building an enterprise-grade cybersecurity lab environment at home.

---

## About This Project

This repository documents the journey of designing and implementing a segmented, secure, and monitored virtual network. Unlike simple "flat" labs, this project mimics a real-world corporate environment with distinct zones, a DMZ, and a centralized SIEM.

**Key Features:**
*   **Network Segmentation:** Isolated zones for Attackers, Victims, Management, and DMZ.
*   **Enterprise Firewall:** Powered by **pfSense** with strict firewall rules.
*   **Intrusion Detection:** **Snort** IDS/IPS integration for real-time threat detection.
*   **SIEM Integration:** **Wazuh** for centralized log analysis and threat hunting.
*   **Pedagogical Approach:** Designed to teach *why* we build things this way, not just *how*.

---

## Repository Structure

```text
/home-lab
├── PROJECT_REPORT.md       # The Full Detailed Report (Start Here!)
├── roadmap.md              # Future Work & Interactive Challenges
├── configs/                # Configuration Templates (Community Contributed)
└── images/                 # Screenshots & Diagrams
```

---

## Quick Start

1.  **Read the Report:** Start with [PROJECT_REPORT.md](PROJECT_REPORT.md) to understand the architecture and design decisions.
2.  **Explore the Challenges:** Check out [roadmap.md](roadmap.md) for ideas on how to expand the lab and test your skills.
3.  **Use the Configs:** Browse the `configs/` folder for reference configurations to help you set up your own tools.

---

## Tech Stack

*   **Hypervisor:** Oracle VirtualBox
*   **Firewall/Router:** pfSense
*   **Attacker:** Kali Linux
*   **Targets:** Metasploitable 2, Windows 10, Windows Server 2022
*   **Defensive Stack:** Snort (IDS), Wazuh (SIEM)

---

## Contributing

Contributions, issues, and feature requests are welcome! Feel free to check the [issues page](https://github.com/yourusername/home-lab/issues).

1.  Fork the Project
2.  Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3.  Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4.  Push to the Branch (`git push origin feature/AmazingFeature`)
5.  Open a Pull Request

---

## License

Distributed under the MIT License. See `LICENSE` for more information.

---

*Built by Fausto Rosado*
