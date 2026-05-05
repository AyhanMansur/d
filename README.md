# 🛡️ AyhanxGurd-Fredom iran Advanced Multi-Hop Privacy Bridge

> **A highly resilient, traffic-obfuscated privacy infrastructure combining WireGuard, Custom Python Proxies, and Cloudflare DoH.**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python)](https://www.python.org/)
[![WireGuard](https://img.shields.io/badge/Protocol-WireGuard-green?logo=wireguard)](https://www.wireguard.com/)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen)]()

---

## 🚀 Overview

`AyhanxGurd-Fredom iran` is not just a VPN; it is a **multi-layered privacy architecture** designed to bypass advanced DPI (Deep Packet Inspection) and network censorship. By combining **traffic normalization**, **multi-hop bridging**, and **DNS-over-HTTPS (DoH)** leakage prevention, this project ensures that your digital footprint remains invisible to local ISPs and national firewalls.

### 🏗️ Architecture

```mermaid
graph LR
    subgraph "Client Side (Iran)"
        A[Legacy Linux Laptop] -->|Normalized TLS Traffic| B[Python Proxy Server]
        B -->|Obsfucated Stream| C[WireGuard Tunnel]
    end

    subgraph "Bridge Layer"
        C -->|Encrypted UDP| D[VPS Iran]
        D -->|WireGuard Hop| E[VPS Turkey]
    end

    subgraph "Exit & DNS Layer"
        E -->|WireGuard Hop| F[VPS Germany]
        F -->|Clean IP| G[Global Internet]
        F -->|Secure DNS Query| H[Cloudflare DoH]
    end

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style B fill:#bbf,stroke:#333,stroke-width:2px
    style F fill:#bfb,stroke:#333,stroke-width:2px
    style H fill:#ff9,stroke:#333,stroke-width:2px
