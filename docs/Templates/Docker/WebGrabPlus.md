---
title: 📺 WebGrabPlus
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 48
---

# 📺 WebGrabPlus

WebGrab+Plus is a multi-site incremental XMLTV EPG grabber. It collects TV program guide data from selected TV guide websites for your favourite channels.

## Features

- 📺 Multi-site EPG grabbing from 100+ TV guide websites worldwide
- 🔄 Incremental updates (only fetches new/changed data)
- 🖼️ Series images and artwork via MDB Postprocessor
- ⏰ Configurable cron schedule for automatic updates
- 📡 XMLTV output compatible with Tvheadend, Emby, Plex, and more

## ⚠️ Important: MAC Address & Hostname

**You MUST replace the `XX:XX:XX:XX:XX:XX` placeholder in the Extra Parameters before deploying!**

WebGrab+Plus uses a free license system that binds the license to the container's **hostname** and **MAC address**. Without fixed values, the license becomes invalid every time the container is recreated.

### How to generate a MAC address

**Option 1: Use a Locally Administered Address (recommended)**

A Locally Administered Address (LAA) ensures your MAC won't conflict with any real hardware on your network. The second-least-significant bit of the first byte must be `1` (the `2` in hex). Simply use a first byte of `02`, `06`, `0A`, or `0E`:

| Example | Explanation |
|---------|-------------|
| `02:42:AC:11:00:05` | Docker's own range – safe choice |
| `06:00:00:00:00:01` | LAA – simple and unique per container |
| `0A:FF:12:34:56:78` | LAA – pick any hex values you like |

**Quick generator (run on your MOS server):**
```bash
printf '02:%02X:%02X:%02X:%02X:%02X\n' $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256)) $((RANDOM%256))
```

**Option 2: Use your server's physical NIC MAC**

You can use the MAC address of your server's network interface (e.g. `eth0`, `end0`). This works but is **not recommended** because:
- If you ever run the container on a different host, the MAC changes
- Multiple containers sharing the same MAC can cause network issues

Find your NIC MAC:
```bash
cat /sys/class/net/eth0/address
```

### Changing in MOS Hub

1. Deploy the template
2. Before starting, go to **Extra Parameters** in the container settings
3. Replace `XX:XX:XX:XX:XX:XX` with your chosen MAC address
4. Save and start the container

## Configuration

After starting the container, you need to configure WebGrab+Plus:

1. Edit `/config/WebGrab++.config` to add your channels
2. Copy the required `.ini` and `.channels.xml` files from `/config/siteini.pack/` to `/config/siteini.user/`
3. Set up the cron schedule in `/config/wg-cron`

### Available Sites for Austria/Germany

| Site | Channels |
|------|----------|
| `tv.orf.at` | ORF 1, ORF 2, ORF III, ORF Sport+ |
| `sky.at` | All Sky channels (Sport, Cinema, Krimi, Replay, Documentaries…) |
| `m.tele.at` | ATV, Puls 4, ServusTV, ProSieben, RTL, SAT.1, and more |
| `tv.magenta.at` | MagentaTV channels |
| `m.tvtoday.de` | German channels (ARD, ZDF, RTL, ProSieben, etc.) |
| `sky.de` | Sky Germany channels |

## Integration with Tvheadend

Point TVH's **"WebGrab+Plus XML file grabber"** module to the `guide.xml` output in the Data path, then click **"Re-run Internal EPG Grabbers"**.

## Important Notes

- 🔄 First run may take 30+ minutes depending on the number of channels
- ⏰ Subsequent incremental runs are much faster (typically 1-5 minutes)
- 📁 The `siteini.pack/` folder contains grabber definitions for 100+ countries

## More Information

- [WebGrab+Plus Documentation](https://webgrabplus.com/documentation)
- [Available Sites List](https://webgrabplus.com/epg-channels)
- [LinuxServer.io Docker Docs](https://docs.linuxserver.io/images/docker-webgrabplus/)
