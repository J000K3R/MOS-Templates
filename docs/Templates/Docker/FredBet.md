---
title: ⚽ FredBet
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 31
---

# ⚽ FredBet

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/fredbet.png" width="80" />

**FredBet** is a simple football betting application using Spring Boot,
Thymeleaf and Bootstrap. Well prepared for betting with friends — responsive
design for mobile devices, multi-language support, and rich features for
managing your betting game.

🏷️ **Category:** Productivity, Entertainment

🐳 **Image:** `fred4jupiter/fredbet:4.7.1`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/fred4jupiter/fredbet](https://github.com/fred4jupiter/fredbet) |
| 🐛 **Support** | [GitHub Issues](https://github.com/fred4jupiter/fredbet/issues) |
| 📖 **Docs** | [README](https://github.com/fred4jupiter/fredbet#readme) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | FredBet Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/fredbet` | `/home/appuser` | RW | H2 database, uploaded images & app data |

---

## ⚙️ Environment Variables

### 🗄️ Database

| Variable | Default | Masked | Description |
|---|---|---|---|
| `SPRING_DATASOURCE_URL` | `jdbc:h2:file:/home/appuser/fredbetdb...` | ❌ | JDBC datasource URL. Default uses H2 file-based DB |
| `SPRING_DATASOURCE_DRIVER_CLASS_NAME` | `org.h2.Driver` | ❌ | DB driver: `org.h2.Driver`, `org.postgresql.Driver`, `com.mysql.jdbc.Driver`, `org.mariadb.jdbc.Driver` |
| `SPRING_DATASOURCE_USERNAME` | `sa` | ❌ | Database username |
| `SPRING_DATASOURCE_PASSWORD` | *(empty)* | ✅ | Database password. Leave empty for H2 default |

### 👤 Admin & App

| Variable | Default | Masked | Description |
|---|---|---|---|
| `FREDBET_ADMIN_USERNAME` | `admin` | ❌ | Initial admin username (created on first boot) |
| `FREDBET_ADMIN_PASSWORD` | `admin` | ✅ | Initial admin password — **change immediately!** |
| `FREDBET_DEFAULT_LANGUAGE` | `de` | ❌ | UI language: `de`, `en`, `pl`, `ca`, `es`, `sv` |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

---

## 🚀 Quick Start

### Step 1: Install via MOS Hub

1. Open the **MOS Hub** and search for **FredBet**
2. Click **Install**

### Step 2: First Login

1. Access FredBet at `http://your-server-ip:8080`
2. Log in with `admin` / `admin`
3. **⚠️ Change the admin password immediately!** Go to **Profile → Change Password**

### Step 3: Import Matches

1. Go to **Administration → Match Import**
2. Upload the **World Cup 2026** Excel template (included)
3. Share the URL with your colleagues and start betting! 🎯

---

## 🏆 Features

| Feature | Description |
|---|---|
| **Match Betting** | Bet on football championship matches |
| **Extra Bets** | Bet on 1st, 2nd and 3rd place winners |
| **Joker** | Use a joker to double your points on a match |
| **Image Gallery** | Upload and share images |
| **User Profile Images** | Avatar support for each user |
| **Ranking** | Points statistics with adult/child filter |
| **Excel Import/Export** | Import matches, export bets and statistics |
| **PDF Export** | Export user ranking as PDF |
| **Multi-Language** | German, English, Polish, Catalan, Spanish, Swedish |
| **Self Registration** | Users can register themselves (can be disabled) |
| **Group Standings** | Live group standings view |
| **Football API** | Optional football-data.org integration for auto match import & live scores |
| **UI Themes** | Changeable design themes |
| **Rich Text Editor** | For rules, prices and misc pages |

---

## 🗄️ Database Options

The template uses **H2 file-based** database by default — simple and no
extra container needed. For production with many users, consider switching
to PostgreSQL, MariaDB or MySQL.

### Switching to PostgreSQL

1. Deploy a PostgreSQL container (e.g. via MOS Hub)
2. Update the environment variables:

| Variable | Value |
|---|---|
| `SPRING_DATASOURCE_DRIVER_CLASS_NAME` | `org.postgresql.Driver` |
| `SPRING_DATASOURCE_URL` | `jdbc:postgresql://postgres:5432/fredbetdb` |
| `SPRING_DATASOURCE_USERNAME` | your DB user |
| `SPRING_DATASOURCE_PASSWORD` | your DB password |

3. Make sure both containers are on the same Docker network

---

## 📊 Match Import

FredBet includes ready-to-use Excel templates:

- **World Cup 2026** — pre-configured match schedule
- **Club World Cup**

Go to **Administration → Match Import** and upload the template.

---

## 🩺 Health Check

`GET /actuator/health` returns `{"status":"UP"}` — no authentication required.

---

> 💡 **Tip:** The H2 database is fine for small groups (up to ~50 users).
> For larger deployments, switch to PostgreSQL for better reliability and
> concurrent access.

> ⚠️ **Important:** Change the default admin password (`admin/admin`)
> immediately after first login!

> 📚 **For more information:** Visit the
> [FredBet documentation](https://github.com/fred4jupiter/fredbet) and
> [application.yml](https://github.com/fred4jupiter/fredbet/blob/master/src/main/resources/application.yml)
> for all configurable properties.
