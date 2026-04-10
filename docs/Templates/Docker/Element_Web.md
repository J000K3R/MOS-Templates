---
title: 💬 Element Web
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 9
---

# 💬 Element Web

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/element.png" width="80" />

**Element Web** is a feature-rich Matrix web client built on the Matrix JS SDK.
Formerly known as Vector and Riot, it provides a full-featured messaging experience
with end-to-end encryption, voice & video calls, and room management — all
self-hostable.

🏷️ **Category:** Cloud, Productivity, Network

🐳 **Image:** `vectorim/element-web:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/element-hq/element-web](https://github.com/element-hq/element-web) |
| 🐛 **Support** | [GitHub Issues](https://github.com/element-hq/element-web/issues) |
| 💛 **Pricing** | [element.io/pricing](https://element.io/de/pricing) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8075` | TCP | Element Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/element-web/config/config.json` | `/app/config.json` | RW | Element config file |

---

## ⚙️ Environment Variables

No environment variables required for this container.

---

## 📄 Configuration File

Element Web is configured via the `config.json` file. A minimal example to point
it at your own Matrix homeserver:
```json
{
  "default_server_config": {
    "m.homeserver": {
      "base_url": "https://matrix.yourdomain.com",
      "server_name": "yourdomain.com"
    }
  },
  "brand": "Element",
  "integrations_ui_url": "https://scalar.vector.im/",
  "integrations_rest_url": "https://scalar.vector.im/api",
  "bug_report_endpoint_url": "https://element.io/bugreports/submit",
  "defaultCountryCode": "DE"
}
```

Make sure the file exists at `/mnt/cache/appdata/element-web/config/config.json`
**before** starting the container!

---

## 🚀 Quick Start

1. Create the config file at `/mnt/cache/appdata/element-web/config/config.json`
2. Set `base_url` to your **Synapse** or **Dendrite** homeserver URL
3. Open the **MOS Hub** and search for **Element Web**
4. Click **Install**
5. Access Element at `http://your-server-ip:8075`

---

> ⚠️ **Note:** Element Web requires a running **Matrix homeserver** like
> **Synapse** to connect to. Without a homeserver it will show a connection error.

> 💡 **Tip:** Element Web works best behind a **reverse proxy with SSL** —
> many Matrix features like VoIP require a secure HTTPS connection to function correctly.

> 📚 **For more information:** Visit the [Element documentation](https://element.io/)
> for Matrix client features and homeserver setup guides.
