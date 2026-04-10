---
title: 📚 BookStack
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 4
---

# 📚 BookStack

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/bookstack.png" width="80" />

**BookStack** is a platform for storing and organising information and documentation.
It provides a pleasant and simple out-of-the-box experience with an intuitive interface
— only basic word-processing skills are required to get started.

🏷️ **Category:** Productivity

🐳 **Image:** `lscr.io/linuxserver/bookstack:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/BookStackApp/BookStack](https://github.com/BookStackApp/BookStack) |
| 🐛 **Support** | [GitHub Issues](https://github.com/BookStackApp/BookStack/issues) |
| 💛 **Donate** | [github.com/sponsors/ssddanbrown](https://github.com/sponsors/ssddanbrown) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `6875` | TCP | BookStack Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/bookstack` | `/config` | RW | App Data & Config |

---

## ⚙️ Environment Variables

### 🌍 General

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TZ` | `Europe/Berlin` | ❌ | Timezone |
| `APP_URL` | `http://192.168.1.1:6875` | ❌ | Full URL to your BookStack instance |
| `APP_KEY` | `` | ❌ | Session encryption key (see below!) |
| `QUEUE_CONNECTION` | `database` | ❌ | Async action handling |

### 🗄️ Database

| Variable | Default | Masked | Description |
|---|---|---|---|
| `DB_HOST` | `mariadb` | ❌ | Database Host |
| `DB_PORT` | `3306` | ❌ | Database Port |
| `DB_USERNAME` | `bookstack` | ❌ | Database Username |
| `DB_PASSWORD` | `bookstack` | ❌ | Database Password |
| `DB_DATABASE` | `bookstack` | ❌ | Database Name |

---

## 🔑 Generating the APP_KEY

The `APP_KEY` is **required** and must be generated before starting the container!
Run the following command to generate it:
```bash
docker run -it --rm --entrypoint /bin/bash \
  lscr.io/linuxserver/bookstack:latest appkey
```

Copy the output and paste it as the value for `APP_KEY`.

---

## 🚀 Quick Start

1. Set up a **MariaDB** database first (required!)
2. Generate your `APP_KEY` using the command above
3. Open the **MOS Hub** and search for **BookStack**
4. Fill in all environment variables
5. Click **Install**
6. Access BookStack at `http://your-server-ip:6875`
7. Default login: **`admin@admin.com`** / **`password`**

---

> ⚠️ **Note:** BookStack requires a running **MariaDB** instance.
> Make sure the database is up and the credentials match before starting BookStack.

> 💡 **Tip:** Change the default admin password immediately after your first login!

> 📚 **For more information:** Visit the [BookStack documentation](https://www.bookstackapp.com/docs/)
> for detailed setup guides, user management, and customization options.
