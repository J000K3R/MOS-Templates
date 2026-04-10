---
title: 🎮 Free Games Claimer
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 10
---

# 🎮 Free Games Claimer

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/free-games-claimer.png" width="80" />

**Free Games Claimer** automatically claims free games on **Epic Games Store**,
**Amazon Prime Gaming** and **GOG** — fully automated so you never miss a free game again!

🏷️ **Category:** Games

🐳 **Image:** `ghcr.io/vogler/free-games-claimer`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/vogler/free-games-claimer](https://github.com/vogler/free-games-claimer) |
| 🐛 **Support** | [GitHub Issues](https://github.com/vogler/free-games-claimer/issues) |
| 💛 **Donate** | [github.com/sponsors/vogler](https://github.com/sponsors/vogler) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `6080` | TCP | VNC Web Interface (Browser Automation) |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/free-games-claimer/data` | `/fgc/data` | RW | Browser data & claim history |

---

## ⚙️ Environment Variables

### 🌍 Generic (all platforms)

| Variable | Default | Masked | Description |
|---|---|---|---|
| `EMAIL` | `email` | ❌ | Generic email for all platforms |
| `PASSWORD` | `xxxx` | ✅ | Generic password for all platforms |

### 🎮 Epic Games Store

| Variable | Default | Masked | Description |
|---|---|---|---|
| `EG_EMAIL` | `email` | ❌ | Epic Games account email |
| `EG_PASSWORD` | `xxxx` | ✅ | Epic Games account password |
| `EG_OTPKEY` | `key` | ❌ | 2FA OTP secret key |

### 📦 Amazon Prime Gaming

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PG_EMAIL` | `email` | ❌ | Prime Gaming account email |
| `PG_PASSWORD` | `xxxx` | ✅ | Prime Gaming account password |
| `PG_OTPKEY` | `key` | ❌ | 2FA OTP secret key |
| `PG_REDEEM` | `0` | ❌ | Auto-redeem to external stores (`0`=off, `1`=on) |

### 🛒 GOG

| Variable | Default | Masked | Description |
|---|---|---|---|
| `GOG_EMAIL` | `email` | ❌ | GOG account email |
| `GOG_PASSWORD` | `xxxx` | ✅ | GOG account password |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Free Games Claimer**
2. Fill in your account credentials for the platforms you want to use
3. For 2FA accounts, add your OTP secret key (`EG_OTPKEY` / `PG_OTPKEY`)
4. Click **Install**
5. Access the VNC web interface at `http://your-server-ip:6080` to watch it work

---

> 💡 **Tip:** You can use the generic `EMAIL` and `PASSWORD` variables if all your
> platform accounts share the same credentials — no need to fill in each platform individually!

> 🔒 **2FA Note:** For the OTP keys, you need the **secret key** from your
> authenticator app setup, not the 6-digit code. This is usually shown when you
> first enable 2FA as a QR code or text string.

> 📚 **For more information:** Visit the [Free Games Claimer GitHub](https://github.com/vogler/free-games-claimer)
> for supported platforms, notification options, and advanced configuration.
