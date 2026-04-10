---
title: 📈 Beszel Hub
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 2
---

# 📈 Beszel Hub

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/beszel.png" width="80" />

**Beszel Hub** is a simple, lightweight server monitoring hub with a clean web UI.
Monitor real-time CPU, memory, disk and network usage across all your systems —
including Docker container stats — from a single dashboard.

🏷️ **Category:** Monitoring

🐳 **Image:** `henrygd/beszel:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/henrygd/beszel](https://github.com/henrygd/beszel) |
| 🐛 **Support** | [GitHub Issues](https://github.com/henrygd/beszel/issues) |
| 💛 **Donate** | [buymeacoffee.com/henrygd](https://www.buymeacoffee.com/henrygd) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8090` | TCP | Beszel Hub Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/beszel/data/` | `/beszel_data` | RW | Database, config & monitoring data |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `APP_URL` | `http://[YOUR-IP]:8090` | ❌ | Public URL of the Beszel Hub |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Beszel Hub**
2. Set `APP_URL` to your server IP, e.g. `http://192.168.1.100:8090`
3. Click **Install**
4. Access the Hub at `http://your-server-ip:8090`
5. Create your admin account on first launch
6. Add your first system via **Add System** and copy the Public Key & Token
7. Deploy the **[Beszel Agent](Beszel-Agent.html)** on each server you want to monitor

---

> 💡 **Tip:** Deploy the **Beszel Hub** once and then install a
> **[Beszel Agent](Beszel-Agent.html)** on every server you want to monitor —
> all metrics flow back to this central dashboard!

> 📚 **For more information:** Visit the [Beszel documentation](https://beszel.dev/)
> for additional configuration options and alert settings.
