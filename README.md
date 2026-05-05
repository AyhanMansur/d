# 🛡️ Ayhanx-Fredom Advanced Multi-Hop Privacy Bridge

> **A highly resilient, traffic-obfuscated privacy infrastructure combining WireGuard, Custom Python Proxies, and Cloudflare DoH.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python)](https://www.python.org/)
[![WireGuard](https://img.shields.io/badge/Protocol-WireGuard-green?logo=wireguard)](https://www.wireguard.com/)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen)]()

---

## 🚀 Overview

`AyhanxGurd-Fredom iran` is not just a VPN; it is a **multi-layered privacy architecture** designed to bypass advanced DPI (Deep Packet Inspection) and network censorship. By combining **traffic normalization**, **multi-hop bridging**, and **DNS-over-HTTPS (DoH)** leakage prevention, this project ensures that your digital footprint remains invisible to local ISPs and national firewalls.
Here is a comprehensive, deep-dive `README.md` for your project. This document is designed to be professional, technically rigorous, and educational, explaining the "why" and "how" behind every architectural decision.

You can copy and paste the content below directly into a `README.md` file in your GitHub repository.

***

# AyhanX-fredom: Advanced Multi-Hop Obfuscated Proxy Infrastructure

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![Sing-Box](https://img.shields.io/badge/Sing--Box-v1.8+-green.svg)](https://github.com/SagerNet/sing-box)
[![WireGuard](https://img.shields.io/badge/WireGuard-v1.0+-blue.svg)](https://www.wireguard.com/)
[![Ubuntu](https://img.shields.io/badge/OS-Ubuntu%2022.04+-informational.svg)](https://ubuntu.com/)

## 📖 Table of Contents
## ⚡ Quick Links
- 🚀 **[Jump to Installation](#-step-by-step-installation-guide)**
- 🔑 **[Key Generation Guide](#phase-3-reality-key-generation)**
- 🛡️ **[Security Best Practices](#-security-analysis)**
- 🐛 **[Need Help?](#-troubleshooting--maintenance)**

---

## 🌍 Project Overview

**MHR-Chain** is a resilient, multi-layered network infrastructure designed to bypass advanced state-level internet censorship, Deep Packet Inspection (DPI), and selective blackouts. Unlike standard VPN solutions that rely on single-point-of-failure connections, MHR-Chain utilizes a **three-node architecture** combining **Traffic Obfuscation** and **Secure Tunneling**.

This project does not just "hide" traffic; it makes the traffic indistinguishable from legitimate, everyday web browsing, rendering it invisible to sophisticated censorship systems like those used in restrictive internet environments.

### Key Features:
- **State-Grade Obfuscation:** Uses `VLESS + Reality` protocol to mimic standard HTTPS traffic.
- **Multi-Hop Resilience:** Routes traffic through Iran → Turkey → Germany to mitigate localized outages.
- **High Performance:** Utilizes `WireGuard` for low-latency internal routing and `Sing-Box` for efficient packet handling.
- **Zero-Cost Entry (Optional):** Can be adapted to use free cloud resources, though this guide focuses on dedicated VPS for stability.
- **DNS Leak Protection:** Configured to force all DNS queries through the secure tunnel.

---

## 🏗️ Architecture & Design Philosophy

The infrastructure is built on two distinct layers, each serving a specific purpose in the chain of anonymity and connectivity.

### 1. Layer 1: The Obfuscation Bridge (Iran → Turkey)
**Protocol:** `VLESS` with `Reality` transport.

The primary challenge in highly censored networks is **DPI (Deep Packet Inspection)**. Traditional protocols like OpenVPN, Shadowsocks, or even raw WireGuard have distinct "fingerprints" (packet sizes, handshake patterns, TLS signatures) that allow firewalls to identify and block them.

**How Reality solves this:**
- **SNI Hiding:** The `Server Name Indication` (SNI) in the TLS handshake is hidden. The client claims to connect to a legitimate public server (e.g., `www.google.com`).
- **Certificate Spoofing:** The server presents a valid certificate for that public domain. The firewall sees a valid HTTPS connection to Google.
- **Handshake Verification:** The `Reality` protocol uses a public/private key pair to verify that the client is authorized without revealing the actual server IP to the firewall.
- **Result:** To the censor, this traffic looks exactly like a user visiting Google.com. It cannot be blocked without blocking Google.com for everyone.

### 2. Layer 2: The Secure Tunnel (Turkey → Germany)
**Protocol:** `WireGuard`.

Once traffic escapes the censored network (Iran) and reaches a neutral jurisdiction (Turkey), the need for heavy obfuscation decreases. However, we still need speed and privacy.

**Why WireGuard?**
- **Speed:** It is significantly faster than IPsec or OpenVPN due to its minimalistic kernel-space implementation.
- **Privacy:** It uses modern cryptographic primitives (Noise Protocol Framework).
- **Stability:** It handles NAT traversal and roaming better than most protocols.
- **Internal Routing:** It creates a private LAN between the Turkey and Germany nodes, ensuring that traffic exiting to the global internet comes from a clean, unrestricted IP address.

### Visual Flow
```mermaid
┌─────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  VPS Iran   │────▶│ VPS Turkey   │────▶│ VPS Germany  │────▶│Global Internet│
│  (Client)   │     │ (Obfuscator) │     │  (Exit Node) │     │              │
└─────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
       │                   │                      │
       │  VLESS+Reality    │  WireGuard           │  Clean IP
       │  (HTTPS Mimic)    │  (Encrypted Tunnel)  │
       └───────────────────┴──────────────────────┘
```

---

## 🛡️ Security Analysis

| Component | Security Mechanism | Threat Mitigation |
| :--- | :--- | :--- |
| **Sing-Box (Iran)** | `obfs` (obfuscation) & TLS 1.3 | Prevents DPI from identifying the protocol. Blocks Man-in-the-Middle (MitM) attacks via certificate pinning. |
| **Reality Protocol** | Public Key Cryptography | Prevents server spoofing. Ensures only authorized clients can connect. |
| **WireGuard** | ChaCha20-Poly1305 Encryption | Provides strong confidentiality and integrity. Resists brute-force attacks. |
| **Firewall (UFW)** | Port Restriction | Only opens necessary ports (443 for HTTPS, 51820 for WG). Reduces attack surface. |

> **Note:** Security is only as strong as the keys. **Never share your `private.key` or `UUID` publicly.** If compromised, regenerate keys immediately.

---

## 📦 Prerequisites

Before beginning, ensure you have the following:

1.  **Three VPS Servers:**
    -   **VPS 1 (Iran):** Ideally located in Iran to bypass local routing restrictions. OS: Ubuntu 22.04 LTS.
    -   **VPS 2 (Turkey):** A neutral location with good connectivity to Iran. OS: Ubuntu 22.04 LTS.
    -   **VPS 3 (Germany):** A location with unrestricted internet access. OS: Ubuntu 22.04 LTS.
2.  **Root Access:** SSH root access to all three servers.
3.  **Basic Knowledge:** Familiarity with Linux command line, `nano` editor, and SSH.

---

## 🛠️ Step-by-Step Installation Guide

### Phase 1: System Preparation

We start by updating the system packages and configuring the firewall on **all three servers**. This ensures a clean, secure baseline.

**Execute on ALL servers (Iran, Turkey, Germany):**

```bash
# Update package lists and upgrade existing packages
sudo apt update && sudo apt upgrade -y

# Install essential utilities
# curl: for downloading files
# wget: for downloading files
# git: for version control
# nano: for editing configuration files
# ufw: Uncomplicated Firewall for security
sudo apt install curl wget git nano ufw -y

# Configure UFW Firewall
# Allow SSH so we don't lock ourselves out
sudo ufw allow OpenSSH
# Allow TCP 443 for Sing-Box (HTTPS)
sudo ufw allow 443/tcp
# Allow UDP 51820 for WireGuard
sudo ufw allow 51820/udp
# Enable the firewall
sudo ufw enable

# Verify firewall status
sudo ufw status verbose
```

---

### Phase 2: Sing-Box Installation (Layer 1)

**Sing-Box** is a universal proxy platform. It supports multiple protocols including VLESS, Reality, and XHTTP. We will install the latest stable version.

**Execute on VPS Iran and VPS Turkey:**

```bash
# Define the latest version dynamically
SINGBOX_VERSION=$(curl -s https://api.github.com/repos/SagerNet/sing-box/releases/latest | grep '"tag_name"' | sed -E 's/.*"v(.*?)".*/\1/')

# Download the binary
curl -L "https://github.com/SagerNet/sing-box/releases/download/v${SINGBOX_VERSION}/sing-box-${SINGBOX_VERSION}-linux-amd64.tar.gz" -o sing-box.tar.gz

# Extract the archive
tar -xzf sing-box.tar.gz

# Move the binary to the system path
sudo mv sing-box-${SINGBOX_VERSION}-linux-amd64/sing-box /usr/local/bin/

# Make it executable
sudo chmod +x /usr/local/bin/sing-box

# Clean up temporary files
rm sing-box.tar.gz
rm -rf sing-box-${SINGBOX_VERSION}-linux-amd64

# Verify installation
sing-box version
```

---

### Phase 3: Reality Key Generation

The `Reality` protocol requires a pair of cryptographic keys to verify the client's identity without exposing the server's true nature.

**Execute ONLY on VPS Turkey:**

```bash
# Generate a UUID (Universally Unique Identifier) for the user
# This acts as the username
sing-box generate uuid

# Generate Reality Key Pair
# This creates a public/private key pair for the TLS handshake
sing-box generate reality-keypair
```

**⚠️ IMPORTANT:**
Copy the output of these commands. You will need:
1.  The **UUID** (for both server and client configs).
2.  The **Private Key** (for the Turkey server config).
3.  The **Public Key** (for the Iran client config).
4.  A **Short ID**: You can generate a random 8-character string using `openssl rand -hex 4`. Example: `a1b2c3d4`.

---

### Phase 4: Server Configuration (Turkey)

Now we configure the Turkey VPS to act as the **Obfuscation Server**. It will listen for VLESS Reality connections on port 443.

1.  Create the configuration file:
    ```bash
    sudo nano /etc/sing-box/config.json
    ```

2.  Paste the following configuration. **Replace the placeholders** with your generated keys.

    ```json
    {
      "log": {
        "level": "warn",
        "timestamp": true
      },
      "inbounds": [
        {
          "type": "vless",
          "tag": "vless-in",
          "listen": "::",
          "listen_port": 443,
          "users": [
            {
              "uuid": "YOUR_GENERATED_UUID_HERE",
              "flow": ""
            }
          ],
          "tls": {
            "enabled": true,
            "server_name": "www.google.com",
            "reality": {
              "enabled": true,
              "handshake": {
                "server": "www.google.com",
                "server_port": 443
              },
              "private_key": "YOUR_PRIVATE_KEY_HERE",
              "short_id": [
                "YOUR_SHORT_ID_HERE"
              ]
            }
          }
        }
      ],
      "outbounds": [
        {
          "type": "direct",
          "tag": "direct"
        }
      ]
    }
    ```

    **Explanation of Fields:**
    -   `listen_port`: 443 is standard HTTPS. Censors rarely block all HTTPS traffic.
    -   `server_name`: The domain name that will appear in the SNI. We use `www.google.com` to blend in.
    -   `handshake`: Specifies which server to connect to during the TLS handshake to get a valid certificate.
    -   `private_key`: The key generated in Phase 3.

3.  Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).

4.  Start the service manually to test:
    ```bash
    sudo sing-box run -c /etc/sing-box/config.json &
    ```
    *Note: The `&` runs it in the background. For production, we will create a systemd service later.*

---

### Phase 5: Client Configuration (Iran)

Now we configure the Iran VPS to connect to the Turkey server. It will act as a **SOCKS5 Proxy** on localhost, allowing other applications to route traffic through it.

1.  Create the configuration file:
    ```bash
    sudo nano /etc/sing-box/config.json
    ```

2.  Paste the following configuration. **Replace `YOUR_SERVER_IP` with the public IP of the Turkey VPS.**

    ```json
    {
      "log": {
        "level": "warn",
        "timestamp": true
      },
      "inbounds": [
        {
          "type": "mixed",
          "tag": "mixed-in",
          "listen": "127.0.0.1",
          "listen_port": 1080
        }
      ],
      "outbounds": [
        {
          "type": "vless",
          "tag": "vless-out",
          "server": "YOUR_SERVER_IP_HERE",
          "server_port": 443,
          "uuid": "YOUR_GENERATED_UUID_HERE",
          "flow": "",
          "tls": {
            "enabled": true,
            "server_name": "www.google.com",
            "reality": {
              "enabled": true,
              "public_key": "YOUR_PUBLIC_KEY_HERE",
              "short_id": "YOUR_SHORT_ID_HERE"
            }
          }
        },
        {
          "type": "direct",
          "tag": "direct"
        }
      ],
      "route": {
        "final": "vless-out"
      }
    }
    ```

    **Explanation of Fields:**
    -   `inbounds`: Listens on `127.0.0.1:1080`. This is a standard SOCKS5 port.
    -   `outbounds`: Points to the Turkey VPS IP.
    -   `public_key`: The public key generated in Phase 3. This verifies the server's identity.
    -   `route`: Forces all traffic to go through the `vless-out` outbound.

3.  Save and exit.

4.  Start the service:
    ```bash
    sudo sing-box run -c /etc/sing-box/config.json &
    ```

5.  **Test the Connection:**
    Run this on the **Iran VPS**:
    ```bash
    curl -x socks5h://127.0.0.1:1080 ifconfig.me
    ```
    If successful, you will see the IP address of the **Turkey** or **Germany** VPS. If you see the Iran IP, the connection failed. Check logs with `journalctl -u sing-box` (if using systemd) or check the terminal output.

---

### Phase 6: WireGuard Setup (Layer 2)

Now we connect Turkey to Germany using WireGuard for high-speed internal routing.

#### Step 6.1: Install WireGuard

**Execute on VPS Turkey and VPS Germany:**

```bash
sudo apt install wireguard -y
```

#### Step 6.2: Generate Keys

**Execute on VPS Turkey and VPS Germany:**

```bash
cd /etc/wireguard
wg genkey | tee private.key | wg pubkey > public.key
```

Copy the `public.key` content from **Germany** to a safe place. You will need it for the Turkey config.

#### Step 6.3: Configure Turkey (Client to Germany)

Create the config file:
```bash
sudo nano /etc/wireguard/wg0.conf
```

Paste the following (Replace placeholders):

```ini
[Interface]
Address = 10.0.0.2/24
PrivateKey = YOUR_PRIVATE_KEY_FROM_TURKEY
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT

[Peer]
PublicKey = YOUR_PUBLIC_KEY_FROM_GERMANY
Endpoint = YOUR_GERMANY_VPS_IP:51820
AllowedIPs = 0.0.0.0/0
PersistentKeepalive = 25
```

**Explanation:**
-   `PersistentKeepalive = 25`: Sends a packet every 25 seconds to keep the NAT mapping alive. Crucial for stability.
-   `AllowedIPs = 0.0.0.0/0`: Routes all traffic through this tunnel.

#### Step 6.4: Configure Germany (Server from Turkey)

Create the config file:
```bash
sudo nano /etc/wireguard/wg0.conf
```

Paste the following:

```ini
[Interface]
Address = 10.0.0.3/24
PrivateKey = YOUR_PRIVATE_KEY_FROM_GERMANY
PostUp = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT

[Peer]
PublicKey = YOUR_PUBLIC_KEY_FROM_TURKEY
AllowedIPs = 10.0.0.2/32
```

**Explanation:**
-   `AllowedIPs = 10.0.0.2/32`: Only accepts connections from the Turkey node's IP.

#### Step 6.5: Start WireGuard

**Execute on VPS Turkey and VPS Germany:**

```bash
sudo systemctl enable wg-quick@wg0
sudo systemctl start wg-quick@wg0
sudo systemctl status wg-quick@wg0
```

#### Step 6.6: Test WireGuard

**From Turkey VPS:**
```bash
ping 10.0.0.3
```
If you get replies, the tunnel is up.

---

### Phase 7: Final Integration & Automation

To make these services survive reboots, we should create systemd services. However, for the purpose of this guide, running them in the background (`&`) is sufficient for testing.

To make it persistent, you can create a service file:

```bash
sudo nano /etc/systemd/system/sing-box.service
```

Paste:
```ini
[Unit]
Description=sing-box proxy service
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/sing-box run -c /etc/sing-box/config.json
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Then:
```bash
sudo systemctl daemon-reload
sudo systemctl enable sing-box
sudo systemctl start sing-box
```

---

## 🐛 Troubleshooting & Maintenance

### 1. Connection Refused (Iran to Turkey)
-   **Check IP:** Ensure `YOUR_SERVER_IP_HERE` in the Iran config is the *public* IP of the Turkey VPS, not the internal WireGuard IP.
-   **Check Firewall:** Ensure port 443 is open on the Turkey VPS (`sudo ufw allow 443/tcp`).
-   **Check Logs:** Run `sudo sing-box run -c /etc/sing-box/config.json` (without `&`) to see real-time errors.

### 2. DNS Leaks
-   Ensure your client (e.g., v2rayN on your local machine) is set to use `127.0.0.1:1080` as the proxy.
-   Enable "Remote DNS" or "DNS over HTTPS" in your client settings to ensure DNS queries also go through the tunnel.

### 3. Slow Speeds
-   **Check MTU:** WireGuard often requires MTU adjustment. Try setting MTU to `1420` on the WireGuard interface.
-   **Geographic Distance:** Turkey to Germany is usually fast. If it's slow, try changing the endpoint in the WireGuard config to a closer server if available.

### 4. IP Blacklisting
-   If the Turkey VPS IP gets blacklisted by the censor:
    1.  Migrate the Sing-Box configuration to a new VPS in a different location (e.g., Netherlands).
    2.  Update the `server` IP in the Iran config.
    3.  The Reality keys remain valid, so no re-generation is needed.

---

## ⚖️ Disclaimer

This software is provided for **educational, research, and testing purposes only**.

-   **Legal Compliance:** You are responsible for complying with all local, national, and international laws and regulations. Using this software to bypass government censorship may be illegal in your jurisdiction.
-   **No Warranty:** This software is provided "AS IS", without warranty of any kind, express or implied.
-   **Liability:** The developers are not responsible for any damages, data loss, or legal consequences resulting from the use of this project.

**Use at your own risk.**
