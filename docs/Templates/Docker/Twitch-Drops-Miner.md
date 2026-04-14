---
title: 🎮 Twitch Drops Miner
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 25
---

# 🎮 Twitch Drops Miner

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/twitch-drops-miner.png" width="80" />

**Twitch Drops Miner** is an application that automatically watches Twitch streams to earn Twitch Drops. It intelligently selects streams with active drops and watches them on your behalf to claim rewards.

⚠️ **Important:** This is an **unofficial Docker container** for automatically mining Twitch drops with a web-based GUI interface. This tool automates Twitch viewing. Ensure you comply with Twitch's Terms of Service and use at your own risk.

🚫 **Do NOT report Docker issues to the original [DevilXD/TwitchDropsMiner](https://github.com/DevilXD/TwitchDropsMiner) project!**

🏷️ **Category:** Games

🐳 **Image:** `dungfu/twitch-drops-miner:latest`

---

## 🔗 Links

| | |
|---|---|
| 📦 **Project** | [github.com/DevilXD/TwitchDropsMiner](https://github.com/DevilXD/TwitchDropsMiner) |
| 🐛 **Support** | [GitHub Issues](https://github.com/DevilXD/TwitchDropsMiner/issues) |
| 💛 **Original Author** | [DevilXD](https://github.com/DevilXD) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `9222` | TCP | Web interface for configuration and monitoring (if GUI enabled) |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/twitch-drops-miner` | `/app/data` | RW | Configuration, cookies, cache, and login session |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

### 🔑 Twitch Account (Optional)

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TWITCH_USERNAME` | `` | ❌ | Your Twitch username (optional) |
| `TWITCH_PASSWORD` | `` | ✅ | Your Twitch password (optional) |
| `HEADLESS` | `true` | ❌ | Run browser in headless mode |
| `AUTO_START` | `true` | ❌ | Start mining automatically |

---

## 🚀 Quick Start

### Step 1: Install via MOS Hub

1. Open the **MOS Hub** and search for **Twitch-Drops-Miner**
2. Click **Install**

### Step 2: Configure Twitch Login

**Option A - Web UI (Recommended):**
1. Access the web interface at `http://YOUR_SERVER_IP:9222`
2. Login with your Twitch credentials
3. Authorize the application

**Option B - Environment Variables:**
1. Set `TWITCH_USERNAME` and `TWITCH_PASSWORD` before starting
2. The miner will auto-login on startup

### Step 3: Configure Drop Campaigns

1. Go to the web interface
2. Select which games/campaigns to farm
3. Set priority order for drops
4. Start mining

---

## 🎮 How It Works

### Automatic Stream Selection
The miner automatically:
1. Checks for active Twitch Drop campaigns
2. Finds streamers playing the selected games
3. Prioritizes streams with the most viewers
4. Watches the stream to earn drops
5. Claims drops automatically when ready
6. Switches to next campaign when complete

### Drop Priority
You can prioritize which drops to farm first:
- **High Priority** - Farm these first
- **Low Priority** - Farm when nothing else available
- **Exclude** - Skip these drops

---

## 📊 Features

### Automatic Farming
- ✅ Finds streams with active drops automatically
- ✅ Switches between streamers efficiently
- ✅ Claims drops when they become available
- ✅ Handles multiple campaigns simultaneously
- ✅ Low resource usage in headless mode

### Smart Behavior
- **Viewer Count Priority** - Joins streams with more viewers (better drop rates)
- **Auto-Switch** - Moves to better campaigns when available
- **Progress Tracking** - Shows estimated time until drop
- **Notification Support** - Can send notifications when drops claimed

### Web Interface
- **Live Status** - See current stream and progress
- **Campaign List** - View all available drops
- **Settings** - Configure behavior and priorities
- **Logs** - View detailed operation logs

---

## ⚠️ Important Warnings

### Terms of Service Compliance
> **Use at your own risk.** This tool automates Twitch viewing which may violate Twitch's Terms of Service:
> - Twitch ToS prohibits "using or promoting third-party software for drops"
> - Your account could be suspended or banned
> - Only use on alternate accounts, never your main account

### Best Practices
1. **Use Alt Account** - Never use your main Twitch account
2. **Headless Mode** - Keep enabled for lower resource usage
3. **Check Logs** - Monitor for errors or rate limiting
4. **Limit Campaigns** - Don't farm everything at once
5. **Be Patient** - Drops take time, let it run

---

## 🔒 Security Considerations

### Account Safety
- **Password Storage** - Credentials stored in container, not encrypted
- **Session Persistence** - Login session saved between restarts
- **Rate Limiting** - Tool includes delays to avoid detection

### Network Access
- **Port 9222** - Web UI accessible on your network
- **Internal Only** - Don't expose to internet without auth
- **VPN Recommended** - Consider VPN for additional privacy

---

## 🔧 Troubleshooting

### Login Issues
1. Check credentials in web UI
2. Clear cookies/cache in `/mnt/cache/appdata/twitch-drops-miner`
3. Enable 2FA may require manual login first

### No Drops Farming
1. Verify active drop campaigns exist
2. Check selected games have drops enabled
3. Ensure streamers are playing the correct games
4. Check logs for "no eligible streams" messages

### High Resource Usage
1. Ensure `HEADLESS=true` is set
2. Reduce concurrent campaigns
3. Check if browser process is stuck

### Rate Limited
If you see rate limit errors:
1. Stop the miner for a few hours
2. Reduce check frequency in settings
3. Consider using a VPN

---

## 🌐 Alternative: Drop Farm Services

Instead of self-hosting, consider:
- **Twitch's Official Mobile App** - Background audio while doing other things
- **Watch on TV** - Cast to TV and do other activities

These are safer but require more manual effort.

---

> ⚠️ **DISCLAIMER:** This tool is for educational purposes. The authors are not responsible for any account bans or Terms of Service violations. Twitch actively monitors for automation tools.

> 💡 **Tip:** Check the [GitHub Issues](https://github.com/DevilXD/TwitchDropsMiner/issues) regularly for updates on Twitch detection and countermeasures.

> 💡 **Tip:** Use a dedicated Twitch account with no payment methods attached to minimize risk.

> 💡 **Tip:** The miner works best when left running 24/7 on a server. Drops often require 2-4 hours of watch time.

> 📚 **For more information:** Visit the [TwitchDropsMiner GitHub](https://github.com/DevilXD/TwitchDropsMiner) for detailed configuration, troubleshooting, and updates.
