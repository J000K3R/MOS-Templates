---
title: 🌡️ ProbeGlass
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 51
---

# 🌡️ ProbeGlass

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/probeglass.png" width="80" />

**ProbeGlass** is a small, self-hosted watchdog for your homelab. It probes your services on a schedule, keeps their history in a single SQLite file, draws the graphs, and tells you when something breaks — the questions you actually ask during an outage: *what is down, since when, and how often has it happened?*

One container, a few megabytes of RAM, and no agent to install on the machines it watches.

🏷️ **Category:** Monitoring

🐳 **Image:** `ghcr.io/j000k3r/probeglass:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/j000k3r/probeglass](https://github.com/j000k3r/probeglass) |
| 🐛 **Support** | [GitHub Issues](https://github.com/j000k3r/probeglass/issues) |
| 📖 **Docs** | [j000k3r.github.io/probeglass-docs](https://j000k3r.github.io/probeglass-docs/) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | ProbeGlass Web Interface and dashboard |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/probeglass` | `/data` | RW | SQLite database, its write-ahead log, and `probeglass.yaml` |

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID the container runs as. It starts as root, hands `/data` to this user and then becomes it, so the appdata folder never has to be prepared by hand |
| `PGID` | `500` | ❌ | Group ID for the same. See `PUID` |
| `UMASK` | `022` | ❌ | File creation mask for new files, in octal |
| `PROBEGLASS_DATA_DIR` | `/data` | ❌ | Optional. The directory the database lives in — the path the volume above is mounted at |
| `PROBEGLASS_DB` | *(empty)* | ❌ | Optional. Full path to the SQLite file; empty means `<data dir>/probeglass.db` |
| `PROBEGLASS_LISTEN` | `:8080` | ❌ | Optional. `host:port` to serve on. Has to match the port mapping |
| `PROBEGLASS_LOG_LEVEL` | `info` | ❌ | `debug`, `info`, `warn` or `error` |
| `PROBEGLASS_LOG_FORMAT` | `text` | ❌ | Optional. `text` or `json` |
| `PROBEGLASS_TRUST_PROXY` | `false` | ❌ | Optional. `true` only behind a reverse proxy you control: it makes the daemon believe `X-Forwarded-For` and `X-Forwarded-Proto` |
| `PROBEGLASS_RETENTION_RAW` | `7d` | ❌ | Optional. How long raw probes are kept. `30s`, `5m`, `2h`, `7d`, `2w`, or a number of seconds |
| `PROBEGLASS_RETENTION_HOURLY` | `90d` | ❌ | Optional. How long the hourly rollups behind the long-range graphs are kept |
| `PROBEGLASS_BASE_URL` | *(empty)* | ❌ | Optional. The address you reach the dashboard on. Alert messages link back to it. Type it out in full — `[IP]` and `[PORT]` are **not** substituted here |
| `PROBEGLASS_ADMIN_PASSWORD` | *(empty)* | ✅ | Optional. Skips the setup screen on a fresh database; ignored once a password has been stored |
| `PROBEGLASS_CONFIG` | *(empty)* | ❌ | Optional. Path to a YAML file declaring sensors and channels. If you set it, the file has to exist or the daemon refuses to start; empty means `<data dir>/probeglass.yaml` when that file is there |

Every one of them is optional: a value that is left empty is ignored and the built-in
default applies, so the container runs on defaults with nothing set at all.

---

## 🚀 Quick Start

1. Open the **MOS Hub** and search for **ProbeGlass**
2. Click **Install**
3. Access ProbeGlass at `http://YOUR_SERVER_IP:8080` and choose the dashboard password

Thirteen example sensors are created on a fresh database, at three example places, so there is something on the screen and on the map to look at — one place with everything answering, one that is a homelab's own addresses and paused, and one with an outage on it, so the map shows a pin in each of its three colours. They can be removed again in one click under **Settings → Monitoring → Example data**.

---

## ⚠️ Notes

**The `/data` folder is taken over by the container, so nothing has to be chowned by hand.** The MOS Hub creates the appdata folder itself, as root, and a bind mount keeps that ownership — a container that started as UID 500 could not write into it and could not repair it either. So this image starts as **root**, gives `/data` and everything inside it to **`PUID:PGID`** — `500:500` by default — and only then runs as that user, before the database is opened. That is how the other images on MOS do it, and it is why installing ProbeGlass on a fresh appdata folder now works without touching a shell.

**Want a different uid? Set `PUID` and `PGID` in the template.** They are variables in this template's Environment section and take effect on the next start; the daemon logs the `uid` and `gid` it ended up as, so `docker logs probeglass` is the proof. `PUID=0` keeps the container as root, which works and is not recommended. The three only apply while the container still starts as root — with `--user` in **Extra Parameters** the process is not root, cannot claim the folder and says so in one log line instead of silently ignoring them.

If a start still fails on the directory, the log now says which of the two it is:

```
probeglass: ping database /data/probeglass.db: unable to open database file (14); /data is
not writable by uid 500, the user this process runs as. In a container the daemon starts as
root, takes the data directory over and then drops to PUID:PGID, so this means it did not start
as root — drop --user, or set PUID and PGID to whoever owns the folder, or hand it over
yourself: chown -R 500:500 <host folder>
```

The other message, `not writable even as root`, means the mount itself is not taking writes — a read-only mount or a full disk — and no `chown` will change that.

MOS rewrites the `/mnt/cache` prefix to your own appdata pool, so if you are working from the host, use the path it actually created:

```bash
chown -R 500:500 /mnt/cache/appdata/probeglass
```

**What the root moment means.** It lasts a few milliseconds, before any file is opened and before the port is bound, and it is permanent once it is over: a privileged `setuid` cannot be undone, and Go applies it to every thread. A container running from this template never touches the appdata folder by hand again after that first start. The full account is in [SECURITY.md](https://github.com/j000k3r/probeglass/blob/main/SECURITY.md) and in the [security concept](https://j000k3r.github.io/probeglass-docs/concepts/security#privileges).

`doctor` is worth knowing when something is off, and it has to be run as the daemon's user to be meaningful — `docker exec` starts a new process as root:

```bash
docker exec -u 500:500 probeglass /usr/local/bin/probeglass doctor
```

**The image is public and needs no login.** `ghcr.io/j000k3r/probeglass:latest` pulls anonymously, from the MOS machine like anywhere else — no registry account, no token, no `docker login`. `latest` is the newest release and only ever a release: a tagged pre-release is excluded from it, so no candidate build reaches the Hub. It is also the tag this collection uses almost everywhere else — 45 of its 56 container templates — so an installed app and this template agree on what they are running.

Pin an exact version instead if you would rather it never moved under you. `0.1.0` is the version as released, `0.1` follows its patch releases, and `sha-<commit>` names one build exactly. All of them are on the same public package page, `ghcr.io/j000k3r/probeglass`. Development builds of whatever is in the tree today live under `dev` and are not what this template installs.

**Ping sensors need `NET_RAW`.** It is already in the template. Without it ProbeGlass falls back to an unprivileged UDP ping and says so in the sensor result; every other sensor type keeps working either way.

**Monitoring Docker containers is opt-in.** It needs the host's Docker socket, and whoever can reach that socket can start a container that mounts the whole host as root. Add it only if you want it, and read `SECURITY.md` first.

```bash
-v /var/run/docker.sock:/var/run/docker.sock:ro
```

**There is no `TZ` here, and that is deliberate.** Everything is stored in UTC while the dashboard renders timestamps in your browser's own timezone, so a `TZ` variable would change nothing and this image does not have one.

**`PUID` and `PGID` exist and do something**, unlike a `TZ` that would not: they decide which user the container becomes after it has taken `/data` over. `500:500` is the default and the right answer almost always — it is the uid the other templates on MOS run their containers as, so the files ProbeGlass writes look like every other file in appdata. Change it only when the volume points somewhere that something else owns.

**The container is distroless and has no shell**, so the MOS Hub console will not open. To look in on it from the host instead:

```bash
docker logs probeglass
docker exec probeglass /usr/local/bin/probeglass healthcheck
```

> 💡 **Tip:** History is kept for 7 days at full resolution and 90 days hourly by default — long enough to answer "how often has this flapped this quarter". Both can be changed later in the web interface.

> 📚 **For more information:** Visit the [ProbeGlass documentation](https://j000k3r.github.io/probeglass-docs/) for the sensor reference, the alert channels, the HTTP API and the configuration file.
