---
title: ⛏️ Minecraft Server
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 15
---

# ⛏️ Minecraft Server

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/minecraft.png" width="80" />

**Minecraft Server** automatically downloads and runs the latest stable Minecraft
server version at startup. Supports multiple server types including Vanilla, Paper,
Forge, Fabric and many more.

🏷️ **Category:** Games

🐳 **Image:** `itzg/minecraft-server:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/itzg/docker-minecraft-server](https://github.com/itzg/docker-minecraft-server) |
| 🐛 **Support** | [GitHub Issues](https://github.com/itzg/docker-minecraft-server/issues) |
| 💛 **Donate** | [github.com/sponsors/itzg](https://github.com/sponsors/itzg) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `25565` | TCP | Minecraft Server (player connections) |
| `25575` | TCP | RCON (remote administration) |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/minecraft-server` | `/data` | RW | World data, config & plugins |

---

## ⚙️ Environment Variables

### 🎮 Server

| Variable | Default | Masked | Description |
|---|---|---|---|
| `EULA` | `TRUE` | ❌ | Accept Minecraft EULA (must be TRUE!) |
| `TYPE` | `VANILLA` | ❌ | Server type (see below) |
| `MEMORY` | `12G` | ❌ | Max memory allocation (e.g. `2G`, `4G`, `12G`) |

### 🔧 RCON

| Variable | Default | Masked | Description |
|---|---|---|---|
| `ENABLE_RCON` | `true` | ❌ | Enable RCON remote console |
| `RCON_PASSWORD` | `` | ✅ | RCON password |
| `RCON_PORT` | `25575` | ❌ | RCON port |

---

## 🖥️ Server Types

The `TYPE` variable supports many different server types:

| Type | Description |
|---|---|
| `VANILLA` | Official Mojang server |
| `PAPER` | High-performance fork with plugin support |
| `FORGE` | Mod support via Forge |
| `FABRIC` | Lightweight mod support via Fabric |
| `SPIGOT` | Plugin support, CraftBukkit fork |
| `PURPUR` | Paper fork with extra features |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Minecraft Server**
2. Set `EULA` to `TRUE` (required!)
3. Choose your server `TYPE`
4. Adjust `MEMORY` based on your available RAM
5. Set a secure `RCON_PASSWORD`
6. Click **Install**
7. Connect to your server at `your-server-ip:25565`

---

> 💡 **Tip:** For best performance with many players, use `TYPE=PAPER` instead
> of `VANILLA` — Paper includes many optimizations and supports plugins like
> EssentialsX, WorldEdit and more!

> ⚠️ **Note:** Make sure to open port `25565` in your firewall or router if you
> want players outside your local network to connect!
