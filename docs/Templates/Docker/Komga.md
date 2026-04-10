---
title: 💥 Komga
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 14
---

# 💥 Komga

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/komga.png" width="80" />

**Komga** is a free and open source comics/mangas/media server. It allows you to manage and read your local comics, manga, and books from a modern web interface with support for multiple users, OPDS feeds, and integration with mobile apps.

Designed specifically for image-based media with features like infinite scroll, double-page spread support, and web reader optimized for comics and manga.

🏷️ **Category:** Media

🐳 **Image:** `gotson/komga:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/gotson/komga](https://github.com/gotson/komga) |
| 🐛 **Support** | [GitHub Issues](https://github.com/gotson/komga/issues) |
| 📖 **Docs** | [komga.org](https://komga.org/) |
| 💛 **Donate** | [GitHub Sponsors](https://github.com/sponsors/gotson) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `25600` | TCP | Komga web interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/komga` | `/config` | RW | Komga configuration and database |
| `/mnt/cache/media/comics` | `/data/comics` | RO | Comics and manga library |
| `/mnt/cache/media/books` | `/data/books` | RO | Optional: Additional books folder |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

---

## 🚀 Quick Start

### Step 1: Prepare Your Library

Create your comics/manga folder structure:

```bash
mkdir -p /mnt/cache/media/comics
mkdir -p /mnt/cache/media/comics/Marvel
mkdir -p /mnt/cache/media/comics/DC
mkdir -p /mnt/cache/media/comics/Manga/One Piece
mkdir -p /mnt/cache/media/comics/Manga/Attack on Titan
```

### Step 2: Install via MOS Hub

1. Open the **MOS Hub** and search for **Komga**
2. Ensure `PUID`/`PGID` can read your media files
3. Click **Install**
4. Access Komga at `http://YOUR_SERVER_IP:25600`
5. Create your admin account on first launch
6. Add your libraries (Comics/Manga)

---

## 📁 Library Organization

Komga supports various folder structures:

### Recommended Structure:
```
/comics/
├── Publisher/
│   ├── Series Name/
│   │   ├── Series Name #01.cbz
│   │   ├── Series Name #02.cbz
│   │   └── Specials/
│   └── Another Series/
└── Manga/
    ├── One Piece/
    │   ├── Vol.01 Ch.001.cbz
    │   ├── Vol.01 Ch.002.cbz
    │   └── ...
    └── Naruto/
```

### Supported File Formats:
- **CBZ** (Comic Book ZIP) - Recommended
- **CBR** (Comic Book RAR)
- **PDF** - Books and comics
- **EPUB** - Limited support
- **Folder of images** - JPG, PNG, WEBP

---

## 📚 Library Types

### Comics Library
- American/European comics
- Graphic novels
- Issues and volumes
- Metadata from ComicVine

### Manga Library
- Japanese manga
- Chapter-based organization
- Right-to-left reading
- Metadata from various sources

### Book Library
- Regular books
- PDF documents
- EPUB support (basic)

---

## 🖥️ Web Reader Features

### Reading Modes
- **Paged** - Single page view
- **Double Page** - Spread view for comics
- **Continuous (Webtoon)** - Infinite scroll for manga
- **Right-to-Left** - For manga reading

### Reader Controls
- **Zoom** - Fit width, fit height, original size
- **Fullscreen** - Immersive reading
- **Page Navigation** - Keyboard shortcuts, thumbnails
- **Reading Progress** - Auto-saved per user

### Mobile Support
- **Touch gestures** - Swipe to navigate
- **Pinch to zoom** - On mobile browsers
- **Responsive UI** - Works on all screen sizes

---

## 📱 Mobile Apps & Clients

### Official Komga Apps:
| Platform | App | Features |
|---|---|---|
| **Android** | [Komga App](https://github.com/gotson/komga/releases) | Native app, offline reading, sync |
| **iOS** | Web interface | Full browser support |

### Third-Party Apps:
| App | Platform | Note |
|---|---|---|
| **Tachiyomi** | Android | Manga reader with Komga extension |
| **Aidoku** | iOS | Manga reader with Komga support |
| **Paperback** | iOS | Reader with Komga integration |

### OPDS Support:
Use any OPDS-compatible reader:
```
http://YOUR_SERVER_IP:25600/opds/v1.2/catalog
```

---

## 🔄 Metadata & Scanning

### Automatic Metadata:
Komga can fetch metadata from:
- **ComicVine** - Comics and graphic novels
- **AniList** - Manga series info
- **Google Books** - General books
- **Local metadata** - ComicInfo.xml inside CBZ files

### Scanning Libraries:
- **Auto-scan** - Watches folders for changes
- **Manual scan** - Trigger from web interface
- **Scheduled scan** - Periodic checks

### ComicInfo.xml:
Embed metadata directly in CBZ files:
```xml
<ComicInfo>
  <Series>Amazing Spider-Man</Series>
  <Number>1</Number>
  <Year>1963</Year>
  <Writer>Stan Lee</Writer>
  <Penciller>Steve Ditko</Penciller>
</ComicInfo>
```

---

## 👥 Multi-User Features

### User Management:
- **Individual accounts** - Separate reading progress
- **Age restrictions** - Content filtering per user
- **Library access** - Control which libraries users see
- **Sharing** - Share specific series or books

### Reading Lists:
- **Personal** - Each user's custom lists
- **Shared** - Curated lists for all users
- **Read Later** - Bookmark interesting books

---

## 🔗 Integration with *arr Apps

Komga works well with:
- **Readarr** - Automated book/comic downloading
- **Mylar3** - Comic book automation
- **Calibre** - eBook management (for books)

---

## 🔒 Security & Best Practices

1. **Library Permissions** - Set read-only if not adding new content
2. **User Accounts** - Create separate accounts for family
3. **Age Restrictions** - Filter mature content for younger users
4. **HTTPS** - Use reverse proxy for external access
5. **Backups** - Regular backup of `/config` folder

---

## 📊 Comparison: Komga vs Alternatives

| Feature | Komga | Kavita | Ubooquity | Calibre-Web |
|---------|-------|--------|-----------|-------------|
| **Comics** | ✅ Excellent | ✅ Good | ⚠️ Basic | ⚠️ Basic |
| **Manga** | ✅ Excellent | ✅ Good | ⚠️ Basic | ❌ No |
| **Web Reader** | ✅ Modern | ✅ Modern | ⚠️ Dated | ✅ Modern |
| **Mobile Apps** | ✅ Yes | ✅ Yes | ❌ No | ⚠️ Web only |
| **Metadata** | ✅ Excellent | ✅ Good | ⚠️ Limited | ✅ Good |
| **Best For** | Comics/Manga | Mixed | Simple | eBooks |

---

## 🛠️ Advanced Configuration

### Reverse Proxy (Nginx):
```nginx
location / {
    proxy_pass http://komga:25600;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}
```

### Tachiyomi Extension Setup:
1. Install Tachiyomi on Android
2. Go to **Browse** → **Extensions**
3. Install **Komga** extension
4. Add your server: `http://YOUR_SERVER_IP:25600`
5. Login with credentials
6. Browse and read!

---

> ⚠️ **Note:** Komga is optimized for image-based media (CBZ, CBR, PDF). For text-based eBooks (EPUB), consider Calibre-Web instead.

> 💡 **Tip:** Use **CBZ** format instead of CBR - it's faster to scan and more widely supported.

> 💡 **Tip:** Embed **ComicInfo.xml** in your CBZ files for perfect metadata without online lookups.

> 💡 **Tip:** Enable **"Analyze books"** in library settings to extract thumbnails and page counts for better browsing.

> 📚 **For more information:** Visit the [Komga Documentation](https://komga.org/) for detailed guides, API reference, and advanced configuration.
