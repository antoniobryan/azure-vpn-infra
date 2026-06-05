# Self-Hosted VPN / Network Tunnel Infrastructure on Azure

A personal infrastructure project where I deployed and configured a self-hosted network tunneling setup on a Linux VM hosted on Microsoft Azure.

Built as a first-year mechanical engineering student after getting tired of restricted hostel internet. Ended up learning way more about cloud infrastructure, Linux server management, and networking than I expected.

---

## What I Built

A cloud-hosted tunneling server running on Azure, configured with:

- **3x-UI** panel for managing inbound/outbound connections
- **VLESS + Reality** protocol via Xray-core for secure, low-detection tunneling
- **SSH-based remote administration** for all configuration and management
- Full cloud VM setup on Microsoft Azure (Ubuntu 22.04 LTS)

The server runs 24/7 on a Standard Azure VM with a static public IP, and connects to clients across multiple devices.

---

## Stack

| Layer | Tool |
|---|---|
| Cloud Provider | Microsoft Azure |
| OS | Ubuntu 22.04 LTS |
| Panel | 3x-UI |
| Core Engine | Xray-core |
| Protocol | VLESS + Reality |
| Remote Access | SSH |
| Shell | Bash / systemd |

---

## How It Works

1. **Azure VM provisioned** with Ubuntu, public IP assigned, SSH keys configured
2. **Inbound ports opened** via Azure Network Security Group rules (custom port, not default)
3. **3x-UI installed** via bash script — provides a web-based management panel for Xray
4. **VLESS + Reality inbound** configured — Reality mimics legitimate TLS traffic, making the connection harder to detect and block
5. **Client-side config** exported from 3x-UI and imported into compatible clients
6. **SSH used for all server-side management** — checking service status, logs, restarting services, editing configs

---

## Setup Overview

> Not a step-by-step tutorial.

### 1. Creating the VM

- Create a new VM on Azure portal (Ubuntu 22.04, Standard_B1s or similar)
- Generate SSH key pair during setup
- Assign a static public IP

### 2. Configure Network Security Group

- Allow SSH (port 22) — restrict to your IP if possible
- Allow your chosen tunnel port (custom, not 443 or 80 to avoid conflicts)
- Block everything else by default

### 3. SSH Into the Server

```bash
ssh azureuser@your-public-ip
```

### 4. Install 3x-UI

```bash
bash <(curl -Ls https://raw.githubusercontent.com/mhsanaei/3x-ui/master/install.sh)
```

Access the panel via browser at `http://your-ip:panel-port`

### 5. Configure Inbound in 3x-UI

- Protocol: VLESS
- Security: Reality
- Generate Reality keypair inside the panel
- Set a destination domain (e.g. a legitimate HTTPS site for fingerprinting)
- Save and note the config

### 6. Export and Use Client Config

- Copy the VLESS + Reality link from 3x-UI
- Import into a compatible client (e.g. v2rayN on PC, v2rayNG on Android, etc.)

---

## What Broke (and How I Fixed It)

**Azure firewall blocking the tunnel port**
→ Fixed by adding an explicit inbound rule in the Azure NSG. 
  Took an embarrassingly long time to realize the issue wasn't the server, it was the cloud-level firewall sitting above it.

**3x-UI panel inaccessible after reboot**
→ Service wasn't set to start on boot. 
  Fixed with `systemctl enable x-ui`.

**Reality config not connecting on client**
→ Destination domain mismatch. The fingerprint domain in the config needs to match what the client expects. Regenerated keys and matched settings.

---

## What I Learnt

- How cloud VMs actually work — networking & firewalls at the infrastructure level
- Linux server basics: systemd, service management, SSH, user permissions, log checking
- The difference between application-layer firewalls (ufw) and cloud-level security groups (Azure NSG) — and why you need to configure both
- How tunneling protocols work (VLESS, Reality, TLS fingerprinting)
- Debugging without a GUI — just SSH, logs, and `systemctl status`

---

## Skills Involved

`Linux` `Microsoft Azure` `SSH` `Networking` `Cloud Computing` `System Administration` `Virtual Machines`

---

## Notes

- This was a curiosity-driven, personal learning project.
- VM runs 24/7 as of May 2026 — still experimenting and adding to it.
- Built during my first year of B.E. Mechanical Engineering @ SSN College of Engineering.

