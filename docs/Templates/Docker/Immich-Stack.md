---
title: 📸 Immich-Stack
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 51
---

# 📸 Immich-Stack

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/immich-stack.png" width="80" />

**Immich-Stack** is a tool that automatically groups similar photos into stacks within the [Immich](https://github.com/immich-app/immich) photo management system. It detects burst sequences, groups similar images by filename and date, finds duplicates, and cleans up trash inconsistencies.

Runs in **cron mode** (recurring) or **one-shot mode** (single run then exits). No web interface — it communicates with Immich via its API.

🏷️ **Category:** Media

🐳 **Image:** `ghcr.io/majorfi/immich-stack:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [github.com/Majorfi/immich-stack](https://github.com/Majorfi/immich-stack) |
| 📖 **Documentation** | [Immich-Stack Wiki](https://github.com/Majorfi/immich-stack/wiki) |
| 🐛 **Support** | [GitHub Issues](https://github.com/Majorfi/immich-stack/issues) |
| 💛 **Donate** | [GitHub Sponsors](https://github.com/sponsors/Majorfi) |

---

## 🌐 Ports

No ports exposed — Immich-Stack is a CLI tool that communicates with Immich via API.

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/immich-stack/logs` | `/app/logs` | RW | Optional log file storage (only used when `LOG_FILE` is set) |

---

## ⚙️ Environment Variables

### 🔑 Required

| Variable | Default | Masked | Description |
|---|---|---|---|
| `API_KEY` | — | ✅ | Immich API key. Generate in Immich under **Settings → API Keys**. Multiple keys can be comma-separated for multi-user stacking. |
| `API_URL` | `http://immich-server:2283/api` | ❌ | URL to the Immich server API endpoint. |

### ⏱️ Execution

| Variable | Default | Masked | Description |
|---|---|---|---|
| `RUN_MODE` | `cron` | ❌ | Execution mode: `cron` (recurring) or `once` (single run then exits). |
| `CRON_INTERVAL` | `86400` | ❌ | Interval in seconds between runs when `RUN_MODE=cron`. Default: 86400 (24h). |
| `DRY_RUN` | `false` | ❌ | If `true`, simulates stacking without making changes. Good for testing. |
| `REPLACE_STACKS` | `false` | ❌ | If `true`, deletes existing stacks and recreates them. |
| `RESET_STACKS` | `false` | ❌ | If `true`, resets all stacks. Requires `CONFIRM_RESET_STACK` to be set. |
| `CONFIRM_RESET_STACK` | — | ❌ | Must be set to `I acknowledge all my current stacks will be deleted and new one will be created` to confirm reset. |

### 🖼️ Stacking Behavior

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PARENT_FILENAME_PROMOTE` | `edit` | ❌ | Filename pattern to promote as stack parent. |
| `PARENT_EXT_PROMOTE` | `.jpg,.dng` | ❌ | Comma-separated file extensions to prioritize as stack parent. |
| `WITH_ARCHIVED` | `false` | ❌ | If `true`, includes archived assets in stacking. |
| `WITH_DELETED` | `false` | ❌ | If `true`, includes deleted/trashed assets in stacking. |
| `CRITERIA` | — | ❌ | Custom stacking criteria (optional). |

### 📝 Logging

| Variable | Default | Masked | Description |
|---|---|---|---|
| `LOG_LEVEL` | `info` | ❌ | Logging verbosity: `trace`, `debug`, `info`, `warn`, `error`. |
| `LOG_FORMAT` | `text` | ❌ | Log output format: `text` (human-readable) or `json` (structured). |
| `LOG_FILE` | — | ❌ | Optional path for persistent file logging. Set to `/app/logs/immich-stack.log` to enable. |

### 🌍 General

| Variable | Default | Masked | Description |
|---|---|---|---|
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container. |

---

## 🚀 Quick Start

### Step 1: Generate an Immich API Key

1. Open **Immich** in your browser
2. Go to **Settings → API Keys**
3. Click **New API Key**
4. Give it a name (e.g., "Immich-Stack")
5. Copy the generated key

### Step 2: Install via MOS Hub

1. Open the **MOS Hub** and search for **Immich-Stack**
2. Set the following required variables:
   - **API_KEY**: Paste your Immich API key
   - **API_URL**: Your Immich server API URL (e.g., `http://immich-server:2283/api`)
3. Optionally adjust:
   - **RUN_MODE**: `cron` for recurring, `once` for single run
   - **CRON_INTERVAL**: How often to run (in seconds)
   - **DRY_RUN**: Set to `true` first to test without changes
4. Click **Install**
5. Wait for the container to start

### Step 3: Verify

1. Check container logs:
   ```
   docker logs -f immich_stack
   ```
2. If `DRY_RUN=true`, review what would be stacked
3. Set `DRY_RUN=false` and restart to apply stacking

---

## 📊 Key Features

### Automatic Photo Stacking
- Groups similar photos by filename and date
- Detects burst sequences (Sony DSCPDC, Canon IMG patterns, etc.)
- Lets you pick the stack parent by extension, filename pattern, regex, or size

### Duplicate Detection
- Scans library and reports duplicates by filename and timestamp
- Helps identify redundant assets

### Trash Cleanup
- Moves stack members of trashed assets to trash
- Optional `--trash-orphaned-raws` flag removes RAW files without a developed companion (JPG, HEIC, etc.)

### Multi-User Support
- Process multiple users in one run (comma-separated API keys)

### Flexible Execution
- **Cron mode**: Runs at configurable intervals
- **One-shot mode**: Single run then exits
- **Dry-run mode**: Preview without making changes
- **Stack replacement**: Delete and recreate existing stacks

---

## 💡 Tips

- **Start with `DRY_RUN=true`** to see what would be stacked before making changes
- **Use `LOG_FILE=/app/logs/immich-stack.log`** for persistent logs across restarts
- **Multiple users**: Comma-separate API keys in `API_KEY` to stack for all users at once
- **Burst photos**: The tool automatically detects burst sequences from camera naming patterns
- **RAW files**: Use `PARENT_EXT_PROMOTE` to prioritize which file type becomes the stack cover image
- **Network**: If Immich runs in a different Docker network, make sure both containers can communicate (same network or `API_URL` points to the correct address)

---

> ⚠️ **Important:** Immich-Stack needs network access to your Immich server. If they're in different Docker networks, either join the same network or use the Immich server's IP address in `API_URL`.

> 💡 **Tip:** Run with `DRY_RUN=true` first to verify the stacking behavior matches your expectations before applying changes.

> 📚 **For more information:** Visit the [Immich-Stack Wiki](https://github.com/Majorfi/immich-stack/wiki) for detailed documentation on commands, options, and advanced usage.
