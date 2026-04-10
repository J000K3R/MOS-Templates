---
title: 🦉 KitchenOwl
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 13
---

# 🦉 KitchenOwl

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/kitchenowl.png" width="80" />

**KitchenOwl** is a smart self-hosted grocery list and recipe manager. Easily add
items to your shopping list, create recipes, get cooking suggestions and track
your expenses — all in one place.

🏷️ **Category:** Productivity

🐳 **Image:** `tombursch/kitchenowl`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/TomBursch/kitchenowl](https://github.com/TomBursch/kitchenowl) |
| 🐛 **Support** | [GitHub Issues](https://github.com/TomBursch/kitchenowl/issues) |
| 💛 **Donate** | [github.com/sponsors/TomBursch](https://github.com/sponsors/TomBursch) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8123` | TCP | KitchenOwl Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/kitchenowl` | `/data` | RW | Database & app data |

---

## ⚙️ Environment Variables

### 🔑 Security

| Variable | Default | Masked | Description |
|---|---|---|---|
| `JWT_SECRET_KEY` | `` | ✅ | Secret key for JWT tokens (change this!) |
| `OPEN_REGISTRATION` | `false` | ❌ | Allow new users to register without invite |

### 🌍 General

| Variable | Default | Masked | Description |
|---|---|---|---|
| `FRONT_URL` | `https://kitchenowl.domain.com` | ❌ | Public URL for email links |

### 📧 Email (optional)

| Variable | Default | Masked | Description |
|---|---|---|---|
| `SMTP_HOST` | `mail.domain.com` | ❌ | SMTP server hostname |
| `SMTP_PORT` | `465` | ❌ | SMTP port (465=SSL, 587=TLS) |
| `SMTP_USER` | `mail@domain.com` | ❌ | SMTP username |
| `SMTP_PASS` | `` | ✅ | SMTP password |
| `SMTP_FROM` | `mail@domain.com` | ❌ | Sender email address |

---

## 🔑 Generating the JWT_SECRET_KEY

The `JWT_SECRET_KEY` is **required** — use a long random string. Generate one with:
```bash
openssl rand -hex 32
```

---

## 🚀 Quick Start

1. Generate your `JWT_SECRET_KEY` using the command above
2. Open the **MOS Hub** and search for **KitchenOwl**
3. Fill in `JWT_SECRET_KEY` and `FRONT_URL`
4. Optionally configure SMTP for email notifications
5. Click **Install**
6. Access KitchenOwl at `http://your-server-ip:8123`
7. Create your admin account on first launch

---

> 💡 **Tip:** KitchenOwl also has a **mobile app** for Android and iOS — just point
> it to your `FRONT_URL` to sync your shopping lists on the go!

> ⚠️ **Note:** Set `OPEN_REGISTRATION` to `false` after creating your account to
> prevent unauthorized users from signing up!

> 📚 **For more information:** Visit the [KitchenOwl documentation](https://kitchenowl.org/)
> for API documentation, mobile app setup, and advanced features.
