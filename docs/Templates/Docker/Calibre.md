---
title: 📚 Calibre
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 6
---

# 📚 Calibre

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/calibre.png" width="80" />

**Calibre** is a powerful and easy to use e-book manager. It's the one-stop solution to all your e-book needs. Calibre can organize your e-book library, convert e-books between formats, sync with e-book readers, and even download news and magazines.

This container provides the full Calibre desktop application accessible via your web browser through noVNC, plus the Calibre content server for sharing your library.

🏷️ **Category:** Media

🐳 **Image:** `lscr.io/linuxserver/calibre:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [calibre-ebook.com](https://calibre-ebook.com/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/linuxserver/docker-calibre/issues) |
| 📖 **Docs** | [manual.calibre-ebook.com](https://manual.calibre-ebook.com/) |
| 💛 **Donate** | [calibre-ebook.com/donate](https://calibre-ebook.com/donate) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | Calibre desktop GUI via noVNC web interface |
| `8081` | TCP | Calibre content server (eBook access API) |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/calibre` | `/config` | RW | Calibre configuration, database, and settings |
| `/mnt/cache/media/books` | `/books` | RW | Your eBook library storage |
| `/mnt/cache/media/books-import` | `/import` | RW | Optional import folder for new eBooks |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |
| `PASSWORD` | `` | ✅ | Optional password for GUI access |
| `CLI_ARGS` | `` | ❌ | Optional arguments to pass to Calibre |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Calibre**
2. Create your library folder:
   ```bash
   mkdir -p /mnt/cache/media/books
   ```
3. Ensure `PUID`/`PGID` can read/write your book files
4. Click **Install**
5. Access Calibre GUI at `http://YOUR_SERVER_IP:8080`
6. On first launch, complete the Calibre setup wizard:
   - Choose your library location: `/books`
   - Select your e-book reader device (optional)

---

## 🖥️ Using the Calibre GUI

The container runs the full Calibre desktop application via **noVNC**, so you get all features:

### Library Management
- **Add Books** - Import eBooks from `/import` folder or upload directly
- **Edit Metadata** - Titles, authors, covers, tags, ratings
- **Organize** - Collections, custom columns, search & filter
- **Convert** - Between ePub, MOBI, AZW3, PDF, and 20+ formats

### Content Server (Port 8081)
The built-in content server at port `8081` allows:
- **OPDS Catalog** - Access from e-book apps
- **Web Access** - Browse and download via browser
- **API Access** - For integrations like Calibre-Web

Enable it in Calibre: **Connect/Share** → **Start Content Server**

---

## 📖 Supported eBook Formats

### Input Formats:
- **ePub**, **MOBI**, **AZW3**, **PDF**
- **CBR**, **CBZ** (Comics)
- **TXT**, **HTML**, **RTF**
- **DOCX**, **ODT**
- And many more...

### Output Formats:
- **ePub** (universal standard)
- **MOBI/AZW3** (Kindle)
- **PDF** (preserve formatting)
- **TXT** (plain text)

---

## 🔧 Common Tasks

### Import eBooks:

**Via Import Folder:**
1. Place eBooks in `/mnt/cache/media/books-import/`
2. In Calibre: **Add Books** → select `/import` folder
3. Books are copied to library with metadata

**Via Upload:**
1. Use Calibre GUI: **Add Books** → drag & drop files
2. Or use the content server upload feature

### Convert eBooks:

1. Select book(s) in library
2. Click **Convert Books**
3. Choose output format (ePub, MOBI, etc.)
4. Customize settings (optional)
5. Click **OK**

### Send to Device:

1. Connect Kindle/e-reader to PC with USB
2. Or configure **Email to Kindle**
3. Right-click book → **Send to Device**

---

## 🌐 Calibre Content Server

The content server runs on port `8081` and provides:

| Feature | URL |
|---|---|
| **Web Interface** | `http://YOUR_SERVER_IP:8081` |
| **OPDS Feed** | `http://YOUR_SERVER_IP:8081/opds` |
| **Mobile App** | Use any OPDS-compatible reader |

### Supported OPDS Apps:
- **KyBook** (iOS)
- **Aldiko** (Android)
- **Moon+ Reader** (Android)
- **FBReader** (Cross-platform)

---

## 🔗 Integration with Calibre-Web

Calibre-Web provides a better web interface for browsing/reading. Use this container as the backend:

1. Install **Calibre-Web** (separate template)
2. Point Calibre-Web to Calibre's `/books` folder
3. Calibre-Web provides modern UI while Calibre handles management

---

## 🔒 Security & Best Practices

1. **Set a Password** - Use `PASSWORD` environment variable to protect GUI
2. **Backup Library** - Your `/books` folder contains all eBooks
3. **Backup Database** - `metadata.db` in your library folder
4. **Regular Exports** - Export library list periodically

---

> ⚠️ **Note:** The GUI runs via noVNC which can be resource-intensive. Consider using Calibre-Web for day-to-day browsing if performance is an issue.

> 💡 **Tip:** Use the **"Fetch News"** feature to automatically download newspapers and magazines as eBooks!

> 💡 **Tip:** Install the **Goodreads** or **Amazon** metadata download plugins for automatic book information.

> 💡 **Tip:** The content server (port 8081) is much lighter than the full GUI for simple eBook access.

> 📚 **For more information:** Visit the [Calibre Manual](https://manual.calibre-ebook.com/) for detailed guides on conversion, plugins, and advanced features.
