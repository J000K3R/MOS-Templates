---
title: 📝 Bazarr
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 4
---

# 📝 Bazarr

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/bazarr.png" width="80" />

**Bazarr** is a companion application to **Sonarr** and **Radarr** that manages and downloads subtitles based on your requirements. It automatically searches for missing subtitles and upgrades existing ones to better quality versions.

It works with most popular subtitle providers including OpenSubtitles, Subscene, and many more.

🏷️ **Category:** Media

🐳 **Image:** `lscr.io/linuxserver/bazarr:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/morpheus65535/bazarr](https://github.com/morpheus65535/bazarr) |
| 🐛 **Support** | [GitHub Issues](https://github.com/morpheus65535/bazarr/issues) |
| 💛 **Donate** | [Buy Me A Coffee](https://www.buymeacoffee.com/morpheus65535) |
| 📖 **Wiki** | [wiki.bazarr.media](https://wiki.bazarr.media/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `6767` | TCP | Bazarr Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/bazarr` | `/config` | RW | Configuration files and database for Bazarr. |
| `/mnt/cache/media/movies` | `/movies` | RW | Location of your movie library (must match Radarr path). |
| `/mnt/cache/media/tv` | `/tv` | RW | Location of your TV show library (must match Sonarr path). |

---

## ⚙️ Environment Variables

### 👤 User/Group Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions. |
| `PGID` | `500` | ❌ | Group ID for file permissions. |
| `UMASK` | `022` | ❌ | File creation mask for new files. |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container. |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Bazarr**
2. Adjust the volume paths to match your library structure (must be the same as Sonarr/Radarr)
3. Ensure `PUID`/`PGID` match your other *arr applications
4. Click **Install**
5. Open the WebUI at `http://YOUR_SERVER_IP:6767`
6. Complete the initial setup:
   - Add your subtitle providers (Settings → Providers)
   - Configure Sonarr and Radarr connections (Settings → Sonarr / Radarr)
   - Set your language preferences (Settings → Languages)
   - Configure automatic search intervals

---

## 🔗 Integration with Sonarr & Radarr

Bazarr needs to connect to your **Sonarr** and **Radarr** instances to know which media files need subtitles:

### Sonarr Connection:
1. In Bazarr: Go to **Settings → Sonarr**
2. Enable Sonarr integration
3. Enter your Sonarr URL: `http://YOUR_SERVER_IP:8989`
4. Enter your Sonarr API key (found in Sonarr: Settings → General)
5. Test and Save

### Radarr Connection:
1. In Bazarr: Go to **Settings → Radarr**
2. Enable Radarr integration
3. Enter your Radarr URL: `http://YOUR_SERVER_IP:7878`
4. Enter your Radarr API key (found in Radarr: Settings → General)
5. Test and Save

---

## 🌍 Supported Subtitle Providers

Bazarr supports many subtitle providers:
- **OpenSubtitles** (free and VIP)
- **Subscene**
- **Addic7ed**
- **Podnapisi**
- **Supersubtitles**
- And many more!

---

## 📋 Path Mapping Requirements

**Important:** The paths configured in Bazarr must match the paths used by Sonarr and Radarr exactly:

| Application | Container Path | Must Match |
|---|---|---|
| Sonarr | `/tv` | Bazarr `/tv` |
| Radarr | `/movies` | Bazarr `/movies` |

If your paths differ between containers, use the **Path Mappings** feature in Bazarr (Settings → Sonarr/Radarr → Path Mappings).

---

> ⚠️ **Note:** Ensure your media folders have the same `PUID`/`PGID` permissions across all *arr applications, otherwise Bazarr may not be able to write subtitle files.

> 💡 **Tip:** Enable **Automatic Subtitle Search** in Settings → Scheduler to have Bazarr continuously look for better subtitles for your existing media.

> 💡 **Tip:** Use the **Wanted** tab to see all media missing subtitles and manually trigger searches.

> 📚 **For more information:** Visit the [Bazarr Wiki](https://wiki.bazarr.media/) for detailed setup guides, provider configuration, and troubleshooting.
