---
title: 🔍 Beszel Agent
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 3
---

# 🔍 Beszel Agent

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/beszel.png" width="80" />

**Beszel Agent** runs on each server you want to monitor. It collects system
metrics (CPU, memory, disk, network) and Docker container stats, then reports
them back to the **Beszel Hub**.

🏷️ **Category:** Monitoring

🐳 **Image:** `henrygd/beszel-agent:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/henrygd/beszel](https://github.com/henrygd/beszel) |
| 🐛 **Support** | [GitHub Issues](https://github.com/henrygd/beszel/issues) |
| 💛 **Donate** | [buymeacoffee.com/henrygd](https://www.buymeacoffee.com/henrygd) |

---

## 🌐 Ports

No ports exposed — the Agent connects **outbound** to the Beszel Hub only.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/beszel/agent/` | `/var/lib/beszel-agent` | RW | Agent state & config |
| `/var/run/docker.sock` | `/var/run/docker.sock` | RO | Docker Socket for container monitoring |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `HUB_URL` | `http://[YOUR-HUB-IP]:8090` | ❌ | URL of your Beszel Hub |
| `LISTEN` | `45876` | ❌ | Port the agent listens on |
| `KEY` | `` | ❌ | Public SSH key from the Beszel Hub |
| `TOKEN` | `` | ✅ | Auth token from the Beszel Hub |

---

## 🚀 Quick Start

1. Make sure **[Beszel Hub](Beszel-Hub.html)** is already running!
2. In the Hub UI click **Add System**
3. Copy the **Public Key** and **Token** from the dialog
4. Open the **MOS Hub** and search for **Beszel Agent**
5. Set `HUB_URL` to your Hub's IP, e.g. `http://192.168.1.100:8090`
6. Paste the **Public Key** into `KEY`
7. Paste the **Token** into `TOKEN`
8. Make sure `LISTEN` port matches what you entered in the Hub UI (default `45876`)
9. Click **Install**

---

> ⚠️ **Note:** The Agent uses **host networking** — it needs full network access
> to accurately report network interface statistics for the host system.

> 💡 **Tip:** Install one Agent per server you want to monitor — all agents report
> back to the same **[Beszel Hub](Beszel-Hub.html)** instance!

> ⚠️ **Port Conflict:** Make sure port `45876` is not already in use on the host
> before starting the Agent. You can check with:
> ```bash
> ss -tlnp | grep 45876
> ```
> If the port is taken, change `LISTEN` to a free port and update the port in the
> Beszel Hub UI accordingly!
