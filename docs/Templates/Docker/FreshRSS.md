---
title: 📰 FreshRSS
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 27
---

# 📰 FreshRSS

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/freshrss.png" width="80" />

**FreshRSS** is a self-hosted RSS feed aggregator. It is lightweight, easy to work with, powerful, and customizable.

Perfect for keeping up with news, blogs, and websites without relying on third-party services.

🏷️ **Category:** Productivity

🐳 **Image:** `freshrss/freshrss:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [freshrss.org](https://freshrss.org/) |
| 📖 **Documentation** | [freshrss.github.io/FreshRSS](https://freshrss.github.io/FreshRSS/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/FreshRSS/FreshRSS/issues) |
| 💛 **GitHub** | [github.com/FreshRSS/FreshRSS](https://github.com/FreshRSS/FreshRSS) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | FreshRSS web interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/freshrss/data` | `/var/www/FreshRSS/data` | RW | FreshRSS data, articles, and configuration |
| `/mnt/cache/appdata/freshrss/extensions` | `/var/www/FreshRSS/extensions` | RW | Custom themes and extensions |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

### 🔄 Feed Updates

| Variable | Default | Masked | Description |
|---|---|---|---|
| `CRON_MIN` | `*/20` | ❌ | Cron schedule for automatic feed updates (default: every 20 minutes) |

---

## 🚀 Quick Start

### Step 1: Install via MOS Hub

1. Open the **MOS Hub** and search for **FreshRSS**
2. Click **Install**
3. Wait for the container to start

### Step 2: First Setup

1. Open FreshRSS at `http://YOUR_SERVER_IP:8080`
2. Follow the setup wizard:
   - **Language:** Select your preferred language
   - **Database:** Choose SQLite (default) or connect to MySQL/PostgreSQL
   - **User:** Create your admin account
   - **Configuration:** Set your preferences

### Step 3: Add Your First Feed

1. Login to FreshRSS
2. Click **+ Subscription** (top right)
3. Enter a feed URL (e.g., `https://news.ycombinator.com/rss`)
4. Click **Add**

---

## 🔧 Configuration

### Feed Update Frequency

Change how often feeds are refreshed:
```
CRON_MIN=*/20    # Every 20 minutes (default)
CRON_MIN=*/30    # Every 30 minutes
CRON_MIN=0       # Every hour at :00
CRON_MIN=0,30    # Every hour at :00 and :30
```

Then restart the container.

### Database Options

During initial setup, you can choose:
- **SQLite** - Default, easiest, no extra container needed
- **MySQL** - Better for many users or high volume
- **PostgreSQL** - Alternative to MySQL

For MySQL/PostgreSQL, you need separate database containers.

---

## 📊 Key Features

### Feed Management
- Unlimited RSS/Atom feed subscriptions
- OPML import/export
- Feed categorization with folders
- Automatic feed discovery from URLs
- Filter and search articles
- Mark as read/unread/favorite

### Reading Experience
- Multiple view modes (normal, global, reader)
- Mobile-friendly responsive design
- Keyboard shortcuts
- Dark mode support
- Customizable themes
- Full-text article view

### Sharing & Integration
- Share articles via email
- Integration with Wallabag, Shaarli, Diaspora, etc.
- Fever API support (compatible with mobile apps)
- Google Reader API support

### User Management
- Multi-user support
- Anonymous reading mode
- Admin panel for user management

---

## 📱 Mobile Apps

Use these apps with FreshRSS via the Fever or Google Reader API:

| App | Platform | API |
|---|---|---|
| **Fiery Feeds** | iOS | Fever API |
| **Unread** | iOS | Fever API |
| **Reeder** | iOS/macOS | Fever API |
| **FeedMe** | Android | Fever API |
| **NewsFlash** | Linux | Google Reader API |

Enable API in FreshRSS: **Settings** → **Profile** → **API Management**

---

## 🔌 Extensions

FreshRSS supports extensions for additional functionality:

**Popular Extensions:**
- **Custom CSS** - Add your own styles
- **Custom JS** - Add JavaScript functionality
- **Reading Time** - Show estimated reading time
- **YouTube** - Better YouTube feed handling
- **Title Wrapper** - Wrap long titles

Install extensions by placing them in the extensions directory.

---

## 🛠️ Troubleshooting

### Feeds Not Updating

1. Check `CRON_MIN` is set correctly
2. Restart the container
3. Check logs: `docker logs freshrss`
4. Verify feeds are valid (some feeds may be broken)

### Database Connection Errors

For SQLite (default), this shouldn't happen. If using MySQL/PostgreSQL:
1. Verify database container is running
2. Check credentials in FreshRSS config
3. Ensure database is accessible from FreshRSS container

### Permission Errors

Fix volume permissions:
```bash
chown -R 500:500 /mnt/cache/appdata/freshrss
docker restart freshrss
```

### Missing Favicons

FreshRSS will fetch favicons automatically. If missing:
1. Wait a few minutes after adding feed
2. Clear browser cache
3. Check that feed website is accessible

### Slow Performance

With many feeds, consider:
1. Increasing `CRON_MIN` to reduce update frequency
2. Using MySQL instead of SQLite
3. Adding more RAM to the container

---

## 🔄 Backup & Migration

### Backup Data

All data is in the volumes:
```bash
cp -r /mnt/cache/appdata/freshrss /backup/freshrss-backup-$(date +%Y%m%d)
```

### Migration to New Server

1. Stop FreshRSS container
2. Copy `/mnt/cache/appdata/freshrss` to new server
3. Install FreshRSS on new server
4. Start container - done!

---

## 💡 Tips

- Use **folders** to organize feeds by topic (News, Tech, Blogs, etc.)
- Enable **Reading View** for distraction-free reading
- Set up **filtering rules** to mark certain articles as read automatically
- Use **keyboard shortcuts** (press `?` to see them)
- **Mark as read on scroll** - automatically marks articles as you scroll
- Configure **sharing options** to easily save articles to read-later services
- Use **Fever API** for the best mobile app compatibility

---

> 💡 **Tip:** If you follow hundreds of feeds, consider using PostgreSQL or MySQL instead of SQLite for better performance.

> 💡 **Tip:** Use the "Global View" (shortcut `3`) for a quick overview of all unread articles across feeds.

> 💡 **Tip:** FreshRSS can automatically remove old articles after a set time - configure in Settings → Archiving.

> 📚 **For more information:** Visit the [FreshRSS Documentation](https://freshrss.github.io/FreshRSS/) for detailed guides on configuration, theming, and API usage.
