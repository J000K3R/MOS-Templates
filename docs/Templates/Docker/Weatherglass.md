---
title: 🌡️ Weatherglass
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 51
---

# 🌡️ Weatherglass

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/weatherglass.png" width="80" />

**Weatherglass** is a small, self-hosted watchdog for your homelab. It probes your services on a schedule, keeps their history in a single SQLite file, draws the graphs, and tells you when something breaks — the questions you actually ask during an outage: *what is down, since when, and how often has it happened?*

One container, a few megabytes of RAM, and no agent to install on the machines it watches.

🏷️ **Category:** Monitoring

🐳 **Image:** `ghcr.io/j000k3r/weatherglass:dev`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/j000k3r/weatherglass](https://github.com/j000k3r/weatherglass) |
| 🐛 **Support** | [GitHub Issues](https://github.com/j000k3r/weatherglass/issues) |
| 📖 **Docs** | [j000k3r.github.io/weatherglass-docs](https://j000k3r.github.io/weatherglass-docs/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | Weatherglass Web Interface and dashboard |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/weatherglass` | `/data` | RW | SQLite database, its write-ahead log, and `weatherglass.yaml` |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `WEATHERGLASS_BASE_URL` | *(empty)* | ❌ | Optional. The address you reach the dashboard on. Alert messages link back to it. Type it out in full — `[IP]` and `[PORT]` are **not** substituted here |
| `WEATHERGLASS_LOG_LEVEL` | `info` | ❌ | `debug`, `info`, `warn` or `error` |
| `WEATHERGLASS_ADMIN_PASSWORD` | *(empty)* | ✅ | Optional. Skips the setup screen on a fresh database; ignored once a password has been stored |

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **Weatherglass**
2. Click **Install**
3. Access Weatherglass at `http://YOUR_SERVER_IP:8080` and choose the dashboard password

Four example sensors are created on a fresh database, so there is something on the screen to look at. They can be removed again in one click under **Settings → Monitoring → Example data**.

---

## ⚠️ Notes

**The `/data` folder has to belong to the container's user.** The image runs as UID 500 — the same uid MOS gives the appdata folders it creates, and not root and not `PUID`. A bind mount takes the ownership of the **host** folder rather than of the image, so if that folder belongs to anybody else, a first install can stop straight away with

```
weatherglass: ping database /data/weatherglass.db: unable to open database file (14)
```

Hand the folder to the container's user once and install again. MOS rewrites the `/mnt/cache` prefix to your own appdata pool, so use the path it actually created:

```bash
chown -R 500:500 /mnt/cache/appdata/weatherglass
```

A container started from this template on a folder MOS itself created needs none of that: both are 500.

**The image is public and needs no login.** `ghcr.io/j000k3r/weatherglass:dev` pulls anonymously, from the MOS machine like anywhere else — no registry account, no token, no `docker login`. There is no `latest` tag while the project is in development; `dev` is the current build, and `sha-<commit>` names one exactly. Version tags appear with the 0.1.0 release.

**Ping sensors need `NET_RAW`.** It is already in the template. Without it Weatherglass falls back to an unprivileged UDP ping and says so in the sensor result; every other sensor type keeps working either way.

**Monitoring Docker containers is opt-in.** It needs the host's Docker socket, and whoever can reach that socket can start a container that mounts the whole host as root. Add it only if you want it, and read `SECURITY.md` first.

```bash
-v /var/run/docker.sock:/var/run/docker.sock:ro
```

**There is no `PUID`, `PGID` or `TZ` here, and that is deliberate.** The image runs as UID 500 — the uid MOS uses for appdata — and a volume created from the image is already owned by it, so there is nothing to set. And everything is stored in UTC while the dashboard renders timestamps in your browser's own timezone — a `TZ` variable would change nothing, so this image does not have one.

**The container is distroless and has no shell**, so the MOS Hub console will not open. To look in on it from the host instead:

```bash
docker logs weatherglass
docker exec weatherglass /usr/local/bin/weatherglass healthcheck
```

> 💡 **Tip:** History is kept for 7 days at full resolution and 90 days hourly by default — long enough to answer "how often has this flapped this quarter". Both can be changed later in the web interface.

> 📚 **For more information:** Visit the [Weatherglass documentation](https://j000k3r.github.io/weatherglass-docs/) for the sensor reference, the alert channels, the HTTP API and the configuration file.
