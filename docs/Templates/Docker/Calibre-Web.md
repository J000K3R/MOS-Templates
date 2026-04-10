---
title: 📖 Calibre-Web
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 7
---

# 📖 Calibre-Web

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/calibre-web.png" width="80" />

**Calibre-Web** is a web app providing a clean interface for browsing, reading and downloading eBooks using an existing Calibre database. It offers a modern, responsive web interface that's much more user-friendly than Calibre's built-in content server.

It works alongside **Calibre** - use Calibre for management/conversion and Calibre-Web for browsing and reading.

🏷️ **Category:** Media

🐳 **Image:** `lscr.io/linuxserver/calibre-web:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/janeczku/calibre-web](https://github.com/janeczku/calibre-web) |
| 🐛 **Support** | [GitHub Issues](https://github.com/janeczku/calibre-web/issues) |
| 💛 **Wiki** | [github.com/janeczku/calibre-web/wiki](https://github.com/janeczku/calibre-web/wiki) |
| 📦 **LinuxServer** | [linuxserver/docker-calibre-web](https://github.com/linuxserver/docker-calibre-web) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8083` | TCP | Calibre-Web interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/calibre-web` | `/config` | RW | Calibre-Web configuration |
| `/mnt/cache/media/books` | `/books` | RO | Calibre library (needs `metadata.db`) |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone |
| `DOCKER_MODS` | `` | ❌ | Enable Calibre tools: `linuxserver/mods:universal-calibre` |

---

## 🚀 Quick Start

### Prerequisites

You need a **Calibre library** first. Either:
1. Install the **Calibre** container and create a library
2. Or use an existing Calibre library from another system

### Installation

1. Ensure your Calibre library exists at `/mnt/cache/media/books` with `metadata.db`
2. Open the **MOS Hub** and search for **Calibre-Web**
3. Click **Install**
4. Access Calibre-Web at `http://YOUR_SERVER_IP:8083`
5. On first launch, configure:
   - **Library Configuration**: Set path to `/books`
   - **Admin Login**: Create admin account
   - **Finish Setup**

---

## ✨ Features

### Modern Web Interface
- **Responsive Design** - Works on desktop, tablet, mobile
- **Dark Mode** - Easy on the eyes
- **Cover Grid** - Beautiful book cover display
- **Advanced Search** - Filter by author, series, tags, rating

### Reading Features
- **In-Browser Reader** - Read eBooks directly
- **Multiple Formats** - ePub, PDF, CBZ, TXT supported
- **Progress Sync** - Track reading progress
- **Bookmarks** - Save your place

### User Management
- **Multi-User** - Individual accounts for family
- **Permissions** - Control access per library
- **Public Shelves** - Share curated collections
- **Anonymous Browsing** - Optional public access

### Integration
- **Send to Kindle** - Email books directly
- **Download** - Any format you need
- **OPDS Catalog** - Use with e-reader apps
- **Kobo Sync** - Direct integration with Kobo devices

---

## 📱 Mobile & eReader Access

### Web Reader
Visit `http://YOUR_SERVER_IP:8083` on any device with a browser for the full experience.

### OPDS Feed
For e-reader apps that support OPDS:
```
http://YOUR_SERVER_IP:8083/opds
```

**Supported Apps:**
- **KyBook** (iOS)
- **Moon+ Reader** (Android)
- **Aldiko** (Android)
- **FBReader** (Multi-platform)

### Kobo Integration
Calibre-Web can sync directly with Kobo e-readers:
1. Enable Kobo sync in settings
2. Configure your Kobo with the Calibre-Web URL
3. Browse and download wirelessly

---

## 🔧 Advanced Configuration

### Enable eBook Conversion

To convert books within Calibre-Web (instead of just downloading existing formats):

1. Set environment variable:
   ```
   DOCKER_MODS=linuxserver/mods:universal-calibre
   ```
2. Restart the container
3. Calibre-Web can now convert between formats

### Enable Uploads

Allow users to upload new books:
1. Admin → Basic Configuration → Feature Configuration
2. Enable "Enable Uploads"
3. Set upload folder permissions

### Send to Kindle

Configure email settings to send books to Kindle devices:
1. Admin → Basic Configuration → Mail Server Settings
2. Enter your SMTP details
3. Users can now "Send to Kindle"

---

## 🔗 Calibre vs Calibre-Web

| Feature | Calibre | Calibre-Web |
|---------|---------|-------------|
| **Purpose** | Library Management | Web Browsing/Reading |
| **GUI** | Desktop (noVNC) | Modern Web UI |
| **Best For** | Adding/Editing Books | Browsing/Reading |
| **Performance** | Heavier | Lightweight |
| **Mobile** | Not optimized | Fully responsive |

**Recommended Setup:**
- Use **Calibre** (port 8080) for library management and conversion
- Use **Calibre-Web** (port 8083) for daily browsing and reading
- Both share the same `/books` library

---

## 🔒 Security Notes

1. **Public Access** - Disable anonymous browsing if not needed
2. **User Permissions** - Set appropriate permissions per user
3. **HTTPS** - Use reverse proxy with SSL for external access
4. **Library Path** - Ensure `/books` is not writable if only reading

---

## 📊 Comparison with Other eBook Servers

| Feature | Calibre-Web | Audiobookshelf | Kavita |
|---------|-------------|----------------|--------|
| **eBooks** | ✅ Excellent | ❌ No | ✅ Good |
| **Audiobooks** | ❌ No | ✅ Excellent | ⚠️ Limited |
| **Comics/Manga** | ⚠️ Basic | ❌ No | ✅ Excellent |
| **Mobile App** | Web only | ✅ Native | ✅ Native |
| **Best For** | eBook Libraries | Audiobooks | Mixed Content |

---

> ⚠️ **Note:** Calibre-Web requires an existing Calibre library with `metadata.db`. It cannot create libraries from scratch.

> 💡 **Tip:** For the best experience, install both Calibre and Calibre-Web containers sharing the same `/books` folder.

> 💡 **Tip:** Enable **"Show Books with Coordinates"** in settings to see books on a world map based on their geographic metadata!

> 💡 **Tip:** Use the **"Read in Browser"** feature for instant reading without downloading - perfect for quick browsing.

> 📚 **For more information:** Visit the [Calibre-Web Wiki](https://github.com/janeczku/calibre-web/wiki) for LDAP integration, reverse proxy setup, and troubleshooting.
