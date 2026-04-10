---
title: 📝 Collabora CODE
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 5
---

# 📝 Collabora CODE

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/code.png" width="80" />

**Collabora CODE** is a powerful online office suite that lets you view and edit
text documents, spreadsheets, presentations and more — with full collaborative
editing support. Designed to integrate seamlessly with self-hosted cloud solutions
like Nextcloud.

🏷️ **Category:** Productivity

🐳 **Image:** `collabora/code:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/CollaboraOnline/online](https://github.com/CollaboraOnline/online) |
| 🐛 **Support** | [GitHub Issues](https://github.com/CollaboraOnline/online/issues) |
| 💛 **Donate** | [collaboraonline.com/subscriptions](https://www.collaboraonline.com/de/subscriptions/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `9980` | TCP | Collabora Web Interface & API |

---

## 💾 Volumes

No volumes required for this container.

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `aliasgroup1` | `https://cloud.mydomain.tld:443` | ❌ | Allowed domains for connecting cloud services |
| `username` | `admin` | ❌ | Admin username for web interface |
| `password` | `password` | ✅ | Admin password for web interface |
| `dictionaries` | `en_GB en_US de_DE` | ❌ | Spell check dictionaries |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Collabora CODE**
2. Set `aliasgroup1` to your Nextcloud domain, e.g.:
```
   https://cloud.yourdomain.com:443
```
3. Set a secure `password` for the admin interface
4. Click **Install**
5. Open your Nextcloud and install the **Collabora Online** app
6. Point Nextcloud to `https://your-server-ip:9980`

---

> ⚠️ **Note:** Collabora CODE is designed to run **behind a reverse proxy** like
> SWAG or Nginx. Direct access without SSL may not work with Nextcloud.

> 💡 **Tip:** Collabora CODE works best together with **Nextcloud** for a complete
> self-hosted office experience. Make sure to add your Nextcloud domain to `aliasgroup1`!

> 📚 **For more information:** Visit the [Collabora Online documentation](https://www.collaboraonline.com/code/)
> for integration guides and configuration options.
