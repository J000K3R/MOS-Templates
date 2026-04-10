---
title: 🎮 AMP (Application Management Panel)
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 1
---

# 🎮 AMP (Application Management Panel)

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/amp.png" width="80" />

**AMP** (Application Management Panel) by CubeCoders is a professional game server management panel that allows you to easily create and manage game servers from a modern web interface.

⚠️ **Important:** AMP is **commercial software** and requires a **paid license** from CubeCoders. A free trial is available, but ongoing use requires purchasing a license.

🏷️ **Category:** Games

🐳 **Image:** `cubecoders/ampbase:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [cubecoders.com/AMP](https://cubecoders.com/AMP) |
| 📖 **Docs** | [cubecoders.com/AMP/Docs](https://cubecoders.com/AMP/Docs) |
| 💬 **Support** | [discourse.cubecoders.com](https://discourse.cubecoders.com/) |
| 🛒 **Get License** | [cubecoders.com/AMP#Buy](https://cubecoders.com/AMP#Buy) |

---

## ⚠️ License Requirements

**AMP is not free software.** You must purchase a license from CubeCoders:

- **Network Edition** (~£7.50/month or ~£75/year) - Unlimited instances
- **Professional Edition** (~£15/month) - Additional enterprise features
- **Free Trial** - 30-day trial available

Get your license at: [cubecoders.com/AMP#Buy](https://cubecoders.com/AMP#Buy)

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | AMP main web interface |
| `25565-25580` | TCP/UDP | Default game server ports (adjust based on needs) |

**Note:** You may need to add additional port ranges in MOS Hub depending on which games you run.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/amp` | `/home/amp/.ampdata` | RW | AMP configuration and game server files |

---

## ⚙️ Environment Variables

### 🔑 License & Setup (Required)

| Variable | Default | Masked | Description |
|---|---|---|---|
| `AMPLICENCE` | `` | ✅ | Your AMP license key from CubeCoders |
| `AMPACCEPTLICENSE` | `Yes` | ❌ | Must be 'Yes' to accept license terms |
| `AMPUSER` | `admin` | ❌ | Default admin username |
| `AMPPASSWORD` | `` | ✅ | Default admin password (min 8 chars) |

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone |

---

## 🚀 Quick Start

### Step 1: Obtain License

1. Visit [cubecoders.com/AMP#Buy](https://cubecoders.com/AMP#Buy)
2. Purchase a Network Edition or Professional license
3. You will receive a license key via email

### Step 2: Install via MOS Hub

1. Open the **MOS Hub** and search for **AMP**
2. Enter your `AMPLICENCE` key
3. Set a strong `AMPPASSWORD` (minimum 8 characters)
4. Ensure `AMPACCEPTLICENSE` is set to `Yes`
5. Click **Install**
6. Access AMP at `http://YOUR_SERVER_IP:8080`
7. Login with username `admin` and your password

### Step 3: Activate License

1. In AMP, go to **Configuration** → **Licensing**
2. Enter your license key if not already activated
3. Click **Activate**

---

## 🎮 Supported Games

AMP supports 50+ game servers including:

### Popular Games:
| Game | Description |
|---|---|
| **Minecraft** | Java & Bedrock editions |
| **Counter-Strike 2** | Valve's latest shooter |
| **Rust** | Survival multiplayer |
| **Terraria** | 2D sandbox adventure |
| **Factorio** | Automation strategy |
| **Valheim** | Viking survival |
| **Project Zomboid** | Zombie survival |
| **ARK: Survival Evolved** | Dinosaur survival |
| **Conan Exiles** | Open-world survival |
| **7 Days to Die** | Zombie survival |

### Voice Servers:
- **TeamSpeak 3**
- **Mumble**
- **TeaSpeak**

### And many more...

See full list: [cubecoders.com/AMP/SupportedApplications](https://cubecoders.com/AMP/SupportedApplications)

---

## ✨ Key Features

### Game Server Management
- **One-Click Install** - Download and configure servers automatically
- **Web Console** - View logs and send commands from browser
- **File Manager** - Built-in FTP-like file management
- **Mod Support** - Install mods for supported games
- **Backup & Restore** - Automatic backups of server data
- **Update Management** - Keep servers up to date automatically

### Multi-Instance
- **Unlimited Instances** (with Network/Pro license)
- **Per-Instance Configuration** - Each server independent
- **Resource Monitoring** - CPU, RAM usage per instance
- **Auto-Restart** - Restart crashed servers automatically

### User Management
- **Multi-User Support** - Delegate server management
- **Role-Based Access** - Control permissions per user
- **ADS (ADS Controller)** - Manage multiple AMP instances

---

## 🔧 Additional Port Configuration

Depending on your games, you may need additional ports:

### Example: Adding Minecraft Server Ports
In MOS Hub, edit the container and add:
- `25565:25565/tcp` (Default Minecraft)
- `25565:25565/udp` (Default Minecraft)
- `25566:25566/tcp` (Second Minecraft server)
- etc.

### Common Game Ports:
| Game | Default Port |
|---|---|
| Minecraft Java | 25565 |
| Minecraft Bedrock | 19132 (UDP) |
| Rust | 28015 |
| CS2 | 27015 |
| Terraria | 7777 |
| Valheim | 2456-2458 |

---

## 🛡️ Security Best Practices

1. **Strong Admin Password** - Set during first setup
2. **Firewall Rules** - Only open required game ports
3. **Regular Backups** - AMP has built-in backup features
4. **Keep Updated** - Regularly update AMP and game servers
5. **Use ADS Controller** - For managing multiple AMP instances securely

---

## 🔗 Comparison: AMP vs Other Game Panels

| Feature | AMP | Pterodactyl | LinuxGSM |
|---------|-----|-------------|----------|
| **License** | Commercial | Open Source | Open Source |
| **Ease of Use** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Game Support** | 50+ | Many | Many |
| **Web Interface** | Modern | Modern | CLI-based |
| **Mod Support** | Built-in | Via eggs | Manual |
| **Price** | £7.50/mo | Free | Free |
| **Best For** | Users wanting ease | Tech-savvy users | Linux experts |

---

## 📝 Troubleshooting

### License Not Activating
- Verify your license key is correct
- Check internet connectivity from container
- Ensure firewall allows outbound connections

### Game Servers Not Starting
- Check available ports don't conflict
- Verify sufficient RAM and CPU allocation
- Review logs in AMP console

### Privileged Mode Required
AMP requires `--privileged` for:
- Some game servers needing system access
- Proper process management
- Network configuration

---

> ⚠️ **Important:** This is **commercial software**. You must purchase a license from CubeCoders to use AMP beyond the trial period.

> 💡 **Tip:** Start with one game server to test, then scale up to multiple instances once familiar with AMP's interface.

> 💡 **Tip:** Use the **ADS (Application Deployment Service)** feature to create a controller instance that manages child AMP instances across multiple servers.

> 📚 **For more information:** Visit the [AMP Documentation](https://cubecoders.com/AMP/Docs) for detailed setup guides, game-specific configuration, and troubleshooting.
