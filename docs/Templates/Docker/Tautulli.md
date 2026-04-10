---
title: 📺 Tautulli
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 20
---

# 📺 Tautulli

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/tautulli.png" width="80" />

**Tautulli** is a Python-based monitoring and tracking tool for **Plex Media Server**. It provides detailed statistics, real-time activity monitoring, and notifications about your Plex library usage.

Track who is watching what, when, and from where. Get insights into your most popular content, peak usage times, and comprehensive history of all streaming activity.

🏷️ **Category:** Media

🐳 **Image:** `lscr.io/linuxserver/tautulli:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/Tautulli/Tautulli](https://github.com/Tautulli/Tautulli) |
| 🐛 **Support** | [GitHub Issues](https://github.com/Tautulli/Tautulli/issues) |
| 📖 **Docs** | [github.com/Tautulli/Tautulli/wiki](https://github.com/Tautulli/Tautulli/wiki) |
| 💛 **Website** | [tautulli.com](https://tautulli.com/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8181` | TCP | Tautulli Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/tautulli` | `/config` | RW | Tautulli configuration, database, and logs |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Tautulli**
2. Click **Install**
3. Access Tautulli at `http://YOUR_SERVER_IP:8181`
4. Complete the initial setup wizard:
   - Connect to your **Plex Server** (requires Plex Token)
   - Configure notification agents (optional)
   - Set up user monitoring preferences

---

## 🔗 Plex Server Connection

### Finding Your Plex Token:

1. Open your Plex Web UI
2. Go to **Settings** → **Network**
3. Or use this URL while logged into Plex:
   ```
   https://plex.tv/pms/servers.xml?X-Plex-Token=
   ```
4. Copy the token from the URL

### In Tautulli Setup:

1. Enter your Plex URL: `http://YOUR_PLEX_IP:32400`
2. Paste your Plex Token
3. Click "Verify" and "Save"

---

## 📊 Features Overview

### Real-Time Monitoring
- **Current Activity** - See who is streaming right now
- **Bandwidth Usage** - Monitor upload/download in real-time
- **Transcoding Status** - Track if content is being transcoded
- **Player Information** - Device, quality, location details

### Statistics & Analytics
- **Play Counts** - Most watched movies and TV shows
- **User Statistics** - Individual user viewing habits
- **Platform Stats** - Which devices are most used
- **Stream Type** - Direct play vs. transcoding statistics
- **Geolocation** - Where your users are connecting from

### Historical Data
- **Complete History** - Every stream ever recorded
- **Graphs & Charts** - Visualize trends over time
- **Export Data** - CSV export for custom analysis

---

## 🔔 Notifications

Tautulli can notify you via multiple channels:

| Agent | Use Case |
|---|---|
| **Discord** | Server notifications |
| **Telegram** | Mobile push notifications |
| **Email** | Detailed reports |
| **Slack** | Team notifications |
| **Webhook** | Custom integrations |
| **Apprise** | Universal notification library |

### Popular Notification Triggers:
- Plex server down/up
- New content added to library
- Someone starts watching
- Transcoding started/stopped
- User has watched X episodes
- Monthly/weekly statistics report

---

## 🛠️ Advanced Features

### Custom Scripts
Execute scripts on notification triggers:
- Post to social media
- Update Discord rich presence
- Trigger home automation
- Log to external databases

### Libraries Sync
- Automatically detect new Plex libraries
- Sync watch status
- Library statistics comparison

### User Management
- Group users by household
- Set user-specific notifications
- Track user play counts and duration
- Identify inactive users

---

## 📈 Popular Dashboards

After setup, explore these sections in Tautulli:

| Section | Description |
|---|---|
| **Home** | Real-time activity and quick stats |
| **Libraries** | Per-library statistics and history |
| **Users** | Individual user analytics |
| **History** | Complete streaming log |
| **Graphs** | Visual trends and insights |
| **Logs** | Tautulli and Plex logs |

---

## 🔒 Security Notes

1. **Secure your Plex Token** - It provides full access to your Plex account
2. **Use HTTPS** - Enable SSL in Tautulli settings for production
3. **Restrict access** - Don't expose Tautulli publicly without authentication
4. **Backup your database** - Tautulli stores all history in `/config`

---

> ⚠️ **Note:** Tautulli requires an active Plex Media Server. It cannot monitor Jellyfin, Emby, or other media servers.

> 💡 **Tip:** Use the **Newsletter** feature to automatically email users a weekly/monthly summary of new content added to your Plex libraries!

> 💡 **Tip:** Enable **IP Logging** to see geographic distribution of your users and detect unusual access patterns.

> 💡 **Tip:** Pair Tautulli with **Grafana** and the [Tautulli Grafana plugin](https://grafana.com/grafana/dashboards/11092) for advanced visualizations.

> 📚 **For more information:** Visit the [Tautulli Wiki](https://github.com/Tautulli/Tautulli/wiki) for advanced configuration, API usage, and custom scripts.
