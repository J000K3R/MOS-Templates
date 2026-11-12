---
title: 🎵 Music Assistant
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 34
---

# 🎵 Music Assistant

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/music-assistant.png" width="80" />

**Music Assistant** is a music library manager and multi-room player. It combines your streaming services (Spotify, Apple Music, Tidal, Deezer, YouTube Music …), internet radio and local files into one library, and plays them on almost any speaker: **Sonos**, AirPlay, Chromecast, Squeezebox, DLNA, Snapcast and **Sendspin** (e.g. TECHO5 voice satellites).

Together with **Home Assistant** it gives you voice control like *"play Linkin Park in the bathroom"* and a media player entity for every speaker.

🏷️ **Category:** Media, Home Automation

🐳 **Image:** `ghcr.io/music-assistant/server:latest`

⚠️ **Requires:** Host networking (mDNS/SSDP discovery of speakers) · Same (V)LAN as your speakers

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/music-assistant/server](https://github.com/music-assistant/server) |
| 📖 **Documentation** | [music-assistant.io](https://www.music-assistant.io/) |
| 🏠 **Home Assistant** | [home-assistant.io/integrations/music_assistant](https://www.home-assistant.io/integrations/music_assistant/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/music-assistant/support/issues) |

---

## 🌐 Network

| Setting | Value |
|---|---|
| Network mode | **host** (required – speakers are discovered via mDNS/SSDP/multicast) |
| Port | `8095` TCP – web interface and API (Home Assistant connects here) |
| Port | `8097` TCP – audio streams to the players |

There is no port mapping because the container uses the host network stack.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/music-assistant` | `/data` | RW | Settings, library database and cache. **Back this up.** |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TZ` | `Europe/Vienna` | ❌ | Timezone |
| `LOG_LEVEL` | `info` | ❌ | `critical`, `error`, `warning`, `info`, `debug` |

---

## 🔐 Extra Parameters

`--cap-add=SYS_ADMIN --cap-add=DAC_READ_SEARCH --security-opt apparmor:unconfined` – needed only if Music Assistant should mount **SMB/NFS shares** for local music files. Without network shares you can remove them.

---

## 🚀 Setup

1. Install the template and open `http://YOUR_SERVER_IP:8095`.
2. Create the admin account, then add **music providers** (e.g. Spotify – needs Premium, Apple Music, Radio Browser, a local folder).
3. Add **player providers**: Sonos, AirPlay, Chromecast, Sendspin … – most are found automatically.
4. In Home Assistant: **Settings → Devices & services → Add Integration → Music Assistant** → URL `http://YOUR_SERVER_IP:8095`.
5. For voice control, expose the Music Assistant players to Assist; an LLM conversation agent can then use *"play … in …"*.

---

## 🛠️ Troubleshooting

| Problem | Fix |
|---|---|
| Speakers are not found | Host networking? Same VLAN? mDNS not blocked by the firewall? |
| Sonos plays but stutters | Check port `8097` is reachable from the speakers |
| Home Assistant can't connect | URL must point to port `8095`, no trailing path |
| Local share can't be mounted | Keep the extra parameters (SYS_ADMIN, DAC_READ_SEARCH, apparmor:unconfined) |

---

## 💡 Best Practices

- Run it on the same host as Home Assistant for the lowest latency
- Use the **Radio Browser** provider for internet radio, or add your own stream URLs
- Back up `/data` – it holds your providers' logins and your library
