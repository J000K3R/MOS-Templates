---
title: 🎞️ Unmanic
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 18
---

# 🎞️ Unmanic

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/unmanic.png" width="80" />

**Unmanic** is a simple tool for optimising your file library. Convert your files
into a single uniform format, manage file movements based on timestamps, or execute
custom commands against files based on their size — fully automated.

🏷️ **Category:** Media

🐳 **Image:** `josh5/unmanic:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/Unmanic/unmanic](https://github.com/Unmanic/unmanic) |
| 🐛 **Support** | [GitHub Issues](https://github.com/Unmanic/unmanic/issues) |
| 💛 **Donate** | [github.com/sponsors/Josh5](https://github.com/sponsors/Josh5) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8888` | TCP | Unmanic Web Interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/unmanic/` | `/config` | RW | Config & database |
| `/mnt/media/` | `/library` | RW | Media library to process |
| `/mnt/cache/temp` | `/tmp/unmanic` | RW | Temp working directory |

---

## ⚙️ Environment Variables

No environment variables required for this container.

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Unmanic**
2. Click **Install**
3. Access Unmanic at `http://your-server-ip:8888`
4. Configure your library path and conversion settings in the web UI
5. Add plugins for the formats you want to convert to

---

## 🔌 Popular Plugins

Unmanic has a rich plugin library — here are some popular ones:

| Plugin | Description |
|---|---|
| **Video Encoder - HEVC** | Convert videos to H.265/HEVC for smaller file sizes |
| **Video Encoder - AV1** | Convert to AV1 for even better compression |
| **Audio Encoder - AAC** | Normalize audio tracks to AAC |
| **Remove Subtitle Streams** | Strip unwanted subtitle tracks |
| **File size comparator** | Only keep converted file if it's smaller |

---

> 💡 **Tip:** Use the **File size comparator** plugin to make sure Unmanic only
> replaces your original files when the converted version is actually smaller —
> preventing accidental quality loss!

> ⚠️ **Note:** Make sure your `/mnt/cache/temp` directory has enough free space —
> Unmanic needs room to store files during conversion before moving them to the library.

> 📚 **For more information:** Visit the [Unmanic documentation](https://docs.unmanic.app/)
> for plugin development, library optimization, and automation workflows.
