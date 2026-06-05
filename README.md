# azure-vpn-infra

self-hosted network tunnel running on a linux VM in the cloud.

built because hostel internet was blocking half the internet. 
ended up learning more about cloud infrastructure and networking than i expected to as a first-year mechanical engineering student.

---

**Stack**
- Microsoft Azure (Ubuntu 22.04 LTS)
- 3x-UI panel + Xray-core
- VLESS + Reality protocol
- SSH for all remote management

**What I did**
- provisioned and configured a Linux VM on Azure
- set up network security rules at the cloud level
- installed and configured 3x-UI for managing tunnel inbounds
- ran VLESS + Reality for secure, low-detection tunneling
- debugged firewall issues, service crashes, and client config mismatches over SSH

**What I Learnt**
- cloud-level firewalls (Azure NSG) vs application-level firewalls (ufw) are different things and you need both
- systemd, service management, logs — the basics of keeping a server alive
- how tunneling protocols work conceptually
- debugging without a GUI is a skill

---

*May 2026 — ongoing, hopefully something doesn't break later on.*  
*B.E. Mechanical Engineering, SSN college of engineering*
