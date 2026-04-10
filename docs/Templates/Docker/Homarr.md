---
title: 🏠 Homarr
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 12
---

# 🏠 Homarr

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/homarr.png" width="80" />

**Homarr** is a sleek, modern dashboard that puts all of your apps and services
at your fingertips. Control everything in one convenient location and seamlessly
integrate with your self-hosted services for at-a-glance information.

🏷️ **Category:** Productivity

🐳 **Image:** `ghcr.io/homarr-labs/homarr:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/homarr-labs/homarr](https://github.com/homarr-labs/homarr) |
| 🐛 **Support** | [GitHub Issues](https://github.com/homarr-labs/homarr/issues) |
| 💛 **Donate** | [opencollective.com/homarr](https://opencollective.com/homarr) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `7575` | TCP | Homarr Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/homarr` | `/appdata` | RW | Config, database & user data |
| `/var/run/docker.sock` | `/var/run/docker.sock` | RW | Docker Socket for container monitoring |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `SECRET_ENCRYPTION_KEY` | `` | ❌ | Encryption key for sensitive data (see below!) |

---

## 🔑 Generating the SECRET_ENCRYPTION_KEY

The `SECRET_ENCRYPTION_KEY` is **required** and must be generated before starting!
Run the following command on your server to generate a secure key:
```bash
openssl rand -hex 32
```

Copy the output and paste it as the value for `SECRET_ENCRYPTION_KEY`.

---

## 🚀 Quick Start

1. Generate your `SECRET_ENCRYPTION_KEY` using the command above
2. Open the **MOS Hub** and search for **Homarr**
3. Paste the generated key into `SECRET_ENCRYPTION_KEY`
4. Click **Install**
5. Access Homarr at `http://your-server-ip:7575`
6. Create your admin account on first launch

---

> 💡 **Tip:** Homarr can monitor and control your Docker containers directly via
> the Docker socket — add your containers as integrations in the dashboard for
> live status, quick restarts and more!

> ⚠️ **Note:** Keep your `SECRET_ENCRYPTION_KEY` safe and backed up — losing it
> means losing access to all encrypted data stored in Homarr!

> 📚 **For more information:** Visit the [Homarr documentation](https://homarr.dev/)
> for widget configuration, theming options, and integration guides.
