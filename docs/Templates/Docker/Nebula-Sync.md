---
title: 🔄 Nebula Sync
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 34
---

# 🔄 Nebula Sync

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/nebula-sync.png" width="80" />

**Nebula Sync** keeps two or more **Pi-hole v6** instances identical. One Pi-hole is the **primary**, all others are **replicas**: on a schedule they take over block lists, allow/deny lists, groups, clients and (optionally) DNS settings from the primary – using the official Pi-hole v6 API.

It replaces the old *Gravity Sync*, which was built for Pi-hole v5 and copies the database files directly. With Pi-hole v6 that can leave a broken `gravity.db` behind, and then the Pi-hole silently blocks nothing.

🏷️ **Category:** Network, Tools

🐳 **Image:** `ghcr.io/lovelaze/nebula-sync:latest`

⚠️ **Requires:** Pi-hole **v6** on all instances · an **app password** per Pi-hole · `webserver.api.app_sudo = true` on every Pi-hole

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/lovelaze/nebula-sync](https://github.com/lovelaze/nebula-sync) |
| 🐛 **Support** | [GitHub Issues](https://github.com/lovelaze/nebula-sync/issues) |

---

## 🌐 Network

| Setting | Value |
|---|---|
| Network mode | **bridge** |
| Ports | none – the container only makes outgoing connections to the Pi-hole APIs |

---

## 💾 Volumes

None. Nebula Sync is stateless.

---

## ⚙️ Environment Variables

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PRIMARY` | `http://192.168.1.2\|APP_PASSWORD` | ✅ | Primary Pi-hole: `http://IP\|app-password` |
| `REPLICAS` | `http://192.168.1.3\|APP_PASSWORD` | ✅ | Replica(s), several separated by a comma |
| `CRON` | `*/15 * * * *` | ❌ | Sync schedule |
| `TZ` | `Europe/Vienna` | ❌ | Timezone |
| `RUN_GRAVITY` | `true` | ❌ | Rebuild gravity on the replicas after a sync |
| `FULL_SYNC` | `false` | ❌ | `true` = full Teleporter sync (whole config incl. webserver/TLS); `false` = only the parts below |
| `SYNC_CONFIG_DNS` | `true` | ❌ | Upstreams, local DNS records, CNAMEs |
| `SYNC_CONFIG_DHCP` / `NTP` / `RESOLVER` / `DATABASE` / `MISC` / `DEBUG` | `false` | ❌ | Host-specific settings – usually keep them per Pi-hole |
| `SYNC_GRAVITY_GROUP` / `AD_LIST` / `AD_LIST_BY_GROUP` | `true` | ❌ | Groups and block lists |
| `SYNC_GRAVITY_DOMAIN_LIST` / `DOMAIN_LIST_BY_GROUP` | `true` | ❌ | Allow/deny lists (exact + regex) |
| `SYNC_GRAVITY_CLIENT` / `CLIENT_BY_GROUP` | `true` | ❌ | Clients and their groups |
| `SYNC_GRAVITY_DHCP_LEASES` | `false` | ❌ | DHCP leases |

---

## 🚀 Setup

1. On **every** Pi-hole: *Settings → Web interface / API → Expert → Configure app password* → copy it → *Enable new app password*.
   Note: a Pi-hole has only **one** app password. Other tools that use it (e.g. the Home Assistant Pi-hole integration) need the new one, too.
2. On **every** Pi-hole allow the app password to change settings:
   ```bash
   pihole-FTL --config webserver.api.app_sudo true
   ```
3. Install the template and fill in `PRIMARY` and `REPLICAS`, e.g. `http://192.168.1.2|<app password of Pi-hole 1>`.
4. Check the log:
   ```bash
   docker logs Nebula-Sync
   ```
   A successful run ends with `Sync completed`.

From now on, make changes **only on the primary** – the replicas are overwritten on the next run.

---

## 🛠️ Troubleshooting

| Problem | Fix |
|---|---|
| `401` / `unauthorized` | Wrong app password, or a new one was generated after setup |
| `403` / `forbidden` when writing | `webserver.api.app_sudo` is not `true` on that Pi-hole |
| Replica settings keep changing back | That's expected – edit the primary instead |
| Migrating from Gravity Sync | Disable it first on **all** Pi-holes: `systemctl disable --now gravity-sync.timer` – never run both |

---

## 💡 Best Practices

- Use **app passwords**, not the web login password
- Keep `FULL_SYNC=false` unless the Pi-holes are truly identical (TLS certificates and webserver settings are host-specific)
- Monitor the replicas (e.g. Home Assistant Pi-hole integration: alert when *domains being blocked* drops)
