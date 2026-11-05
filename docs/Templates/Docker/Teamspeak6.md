---
title: 🎙️ TeamSpeak 6 Server
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 52
---

# 🎙️ TeamSpeak 6 Server

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/teamspeak6.png" width="80" />

**TeamSpeak 6 Server** is the next generation of the TeamSpeak voice server.
It keeps the low-latency voice model of TS3 while adding a modern architecture,
a YAML-based configuration and full amd64/arm64 support.

🏷️ **Category:** Games

🐳 **Image:** `teamspeaksystems/teamspeak6-server:latest`

---

## ⚠️ Beta Notice

This is a **beta release** (`6.0.0-beta`). The server ships with a **32-slot beta
license** that is renewed every two months. Larger licenses are not available for
TS6 yet. Do not expect stability guarantees from a production standpoint.

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [teamspeak.com](https://www.teamspeak.com/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/teamspeak/teamspeak6-server/issues) |
| 📖 **Repository** | [teamspeak/teamspeak6-server](https://github.com/teamspeak/teamspeak6-server) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `9987` | UDP | Voice — main client port |
| `30033` | TCP | File transfer (icons, avatars, channel files) |
| `10080` | TCP | WebQuery (HTTP) for automation |
| `10022` | TCP | SSH ServerQuery (encrypted) |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/teamspeak6` | `/var/tsserver` | RW | Database, files, logs & license |

> ⚠️ **Note:** The data path is `/var/tsserver`, **not** `/teamspeak` like the
> TeamSpeak 3 image. Using the TS3 path silently loses all data on restart.

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TSSERVER_LICENSE_ACCEPTED` | `accept` | ❌ | Accept the server license (also accepts `1`) — required, no start without it |
| `TSSERVER_LOG_TIMEZONE` | `local` | ❌ | Log timestamps: `utc` or `local` — **only** these two values are valid |
| `TZ` | `Europe/Vienna` | ❌ | Container timezone, applied when log timezone is `local` |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **TeamSpeak 6**
2. Leave `TSSERVER_LICENSE_ACCEPTED` at `accept`
3. Click **Install**
4. Open the container log — the **ServerAdmin privilege key** is printed there on
   first run. Copy it before creating anything else
5. Connect with a TS3 client (3.6.0+) or the TS6 client and enter the key
6. Forward UDP `9987` and TCP `30033` in your router if the server should be
   reachable from the internet

---

## 🔄 Migration from TeamSpeak 3

**There is no migration path.** TeamSpeak states explicitly that TS3 server
licenses are not compatible with TS6, and an existing `ts3server.sqlitedb`
cannot be imported.

Moving from TS3 means rebuilding channels, groups, permissions and client
registrations by hand. Only the voice port can stay the same.

---

## 💡 Notes

> 💡 **Tip:** TS3 clients from version 3.6.0 upwards can connect to a TS6 server,
> so your users do not need a new client.

> 🔒 **Security:** Keep `10022` and `10080` closed to the internet unless you
> actually use ServerQuery. They are only needed for automation, and both are
> credential-based — expose them via VPN or not at all.

> 🖥️ **ARM support:** Unlike the TeamSpeak 3 image, TS6 ships multi-arch builds
> for `amd64` **and** `arm64`, so it also runs on a Raspberry Pi.

> 📚 **For more information:** See the [CONFIG.md](https://github.com/teamspeak/teamspeak6-server/blob/main/CONFIG.md)
> for every environment variable, CLI flag and `tsserver.yaml` option.
