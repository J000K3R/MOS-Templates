---
title: 🌩️ Cloudflared
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 6
---

# 🌩️ Cloudflared

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/cloudflare.png" width="80" />

**Cloudflared** is the command-line client for **Cloudflare Tunnel** — a tunneling
daemon that proxies traffic from the Cloudflare network to your origins without
requiring you to open any ports on your firewall. Your origin server can remain
completely closed to the public internet.

🏷️ **Category:** Web

🐳 **Image:** `cloudflare/cloudflared:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/cloudflare/cloudflared](https://github.com/cloudflare/cloudflared) |
| 🐛 **Support** | [GitHub Issues](https://github.com/cloudflare/cloudflared/issues) |
| 🌐 **Cloudflare** | [cloudflare.com](https://www.cloudflare.com/) |

---

## 🌐 Ports

No ports exposed — Cloudflared connects **outbound** to Cloudflare's network only.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/cloudflared` | `/home/nonroot/.cloudflared/` | RW | Tunnel config & credentials |

---

## ⚙️ Environment Variables

No environment variables required for this container.

---

## 🚀 Quick Start

1. Log in to the [Cloudflare Zero Trust Dashboard](https://one.dash.cloudflare.com/)
2. Go to **Networks** → **Tunnels** → **Create a Tunnel**
3. Copy your **Tunnel Token** (the `XXXXX` part)
4. Open the **MOS Hub** and search for **Cloudflared**
5. Replace `XXXXX` in the post parameters with your Tunnel Token
6. Click **Install**

---

> 🔒 **How it works:** Cloudflared creates an outbound-only connection to
> Cloudflare's edge network. No inbound ports need to be opened on your firewall,
> making it one of the safest ways to expose self-hosted services to the internet.

> 💡 **Tip:** You can expose multiple services through a single Cloudflared tunnel
> using **Public Hostnames** in the Cloudflare Zero Trust Dashboard — no need to
> run multiple containers!
