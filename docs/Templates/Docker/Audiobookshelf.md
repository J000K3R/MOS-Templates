---
title: 🎧 Audiobookshelf
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 5
---

# 🎧 Audiobookshelf

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/audiobookshelf.png" width="80" />

**Audiobookshelf** is a self-hosted audiobook and podcast server with an elegant web player and mobile apps. It features multi-user support, progress syncing across devices, offline listening, and automatic metadata fetching from multiple sources.

Perfect for managing your audiobook collection and podcast subscriptions in one place, with full control over your data.

🏷️ **Category:** Media

🐳 **Image:** `ghcr.io/advplyr/audiobookshelf:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/advplyr/audiobookshelf](https://github.com/advplyr/audiobookshelf) |
| 🐛 **Support** | [GitHub Issues](https://github.com/advplyr/audiobookshelf/issues) |
| 📖 **Docs** | [www.audiobookshelf.org/docs](https://www.audiobookshelf.org/docs) |
| 📱 **Mobile Apps** | [App Store](https://apps.apple.com/us/app/audiobookshelf/id1558272859) / [Play Store](https://play.google.com/store/apps/details?id=com.audiobookshelf.app) |
| 💛 **Sponsor** | [GitHub Sponsors](https://github.com/sponsors/advplyr) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `13378` | TCP | Audiobookshelf Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/media/audiobooks` | `/audiobooks` | RO | Your audiobook library folder |
| `/mnt/cache/media/podcasts` | `/podcasts` | RO | Your podcast library folder |
| `/mnt/cache/appdata/audiobookshelf/config` | `/config` | RW | Configuration and database |
| `/mnt/cache/appdata/audiobookshelf/metadata` | `/metadata` | RW | Cached metadata and cover images |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |
| `DEBUG` | `false` | ❌ | Enable debug logging |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Audiobookshelf**
2. Create your library folders if they don't exist:
   ```bash
   mkdir -p /mnt/cache/media/audiobooks
   mkdir -p /mnt/cache/media/podcasts
   ```
3. Ensure `PUID`/`PGID` can read your media files
4. Click **Install**
5. Access Audiobookshelf at `http://YOUR_SERVER_IP:13378`
6. Create your root user account
7. Add your libraries (Audiobooks and/or Podcasts)

---

## 📚 Library Setup

### Audiobook Library

1. Click **Create Library**
2. Select **Audiobooks**
3. Set folder path to `/audiobooks`
4. Configure metadata providers (optional)
5. Start library scan

### Podcast Library

1. Click **Create Library**
2. Select **Podcasts**
3. Set folder path to `/podcasts`
4. Subscribe to podcasts via URL or search

---

## 📱 Mobile Apps

Audiobookshelf has official mobile apps for seamless listening:

| Platform | Download | Features |
|---|---|---|
| **iOS** | [App Store](https://apps.apple.com/us/app/audiobookshelf/id1558272859) | Download for offline, CarPlay, sleep timer |
| **Android** | [Play Store](https://play.google.com/store/apps/details?id=com.audiobookshelf.app) | Download for offline, Android Auto, sleep timer |

### Mobile App Setup:
1. Open the app
2. Tap **Add Server**
3. Enter your server URL: `http://YOUR_SERVER_IP:13378`
4. Login with your credentials
5. Start listening!

---

## 🌟 Key Features

### Audio Playback
- **Progress Syncing** - Seamless across all devices
- **Offline Mode** - Download books/podcasts for offline listening
- **Sleep Timer** - Automatically stop playback
- **Chapter Support** - Jump between chapters easily
- **Playback Speed** - Adjust from 0.5x to 3x
- **AirPlay & Chromecast** - Stream to your devices

### Library Management
- **Automatic Metadata** - Fetches from Audible, Google, iTunes, OpenLibrary
- **Cover Art** - Auto-downloads cover images
- **Multiple Libraries** - Separate audiobooks and podcasts
- **Collections & Playlists** - Organize your content
- **Search & Filter** - Find content quickly

### Multi-User
- **User Accounts** - Individual libraries and progress
- **Permissions** - Control access per library
- **Listening Stats** - Track time and books per user

### Podcasts
- **RSS Subscriptions** - Subscribe to any podcast feed
- **Auto-Download** - Fetch new episodes automatically
- **Episode Management** - Archive listened episodes

---

## 📁 File Organization

Audiobookshelf supports various folder structures:

### Recommended Structure:
```
/audiobooks/
├── Author Name/
│   ├── Book Title/
│   │   ├── Book Title.mp3 (or m4b, flac, etc.)
│   │   └── cover.jpg (optional)
│   └── Another Book/
└── Another Author/
```

### Supported Formats:
- **Audio:** MP3, M4A, M4B, FLAC, OPUS, OGG, WAV
- **Containers:** Single files or folder-based audiobooks

---

## 🔗 Metadata Providers

Audiobookshelf can fetch metadata from:

| Provider | Type |
|---|---|
| **Audible** | Audiobooks (primary source) |
| **Google Books** | Books |
| **iTunes** | Audiobooks |
| **OpenLibrary** | Books |
| **LibraryThing** | Books |

---

## 🔒 Security & Best Practices

1. **Use HTTPS** - Enable SSL via reverse proxy for production
2. **Strong Root Password** - Set on first setup
3. **User Permissions** - Create separate accounts for family members
4. **Backup Config** - Regular backups of `/mnt/cache/appdata/audiobookshelf/config`
5. **Metadata Caching** - Metadata folder grows with library size

---

## 🛠️ Advanced Configuration

### Reverse Proxy Setup

For HTTPS access, use Nginx Proxy Manager or Traefik:

```nginx
location / {
    proxy_pass http://audiobookshelf:80;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

### Backup Your Library

```bash
# Backup config and database
tar -czf audiobookshelf-backup.tar.gz /mnt/cache/appdata/audiobookshelf/

# Backup is separate (your actual audio files)
rsync -av /mnt/cache/media/audiobooks/ /backup/audiobooks/
```

---

> ⚠️ **Note:** Audiobookshelf container runs as non-root internally. Ensure your media files are readable by PUID/PGID 500.

> 💡 **Tip:** Use **M4B** format for audiobooks - it's a single file with embedded chapters and metadata, perfect for seamless listening.

> 💡 **Tip:** Enable **Auto-Scan** in library settings to automatically detect new books added to your folders.

> 💡 **Tip:** The mobile app supports **Android Auto** and **CarPlay** - perfect for listening during your commute!

> 📚 **For more information:** Visit the [Audiobookshelf Documentation](https://www.audiobookshelf.org/docs) for advanced configuration, API usage, and troubleshooting.
