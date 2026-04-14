---
title: 🍽️ Mealie
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 26
---

# 🍽️ Mealie

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/mealie.png" width="80" />

**Mealie** is a self-hosted recipe manager and meal planner with a beautiful web interface. Import recipes from URLs, organize your collection, plan meals, and generate shopping lists.

Perfect for food enthusiasts who want to organize their recipes, plan weekly meals, and streamline grocery shopping.

🏷️ **Category:** Productivity

🐳 **Image:** `ghcr.io/mealie-recipes/mealie:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [mealie.io](https://mealie.io/) |
| 📖 **Documentation** | [docs.mealie.io](https://docs.mealie.io/) |
| 🐛 **Support** | [GitHub Issues](https://github.com/mealie-recipes/mealie/issues) |
| 💛 **GitHub** | [github.com/mealie-recipes/mealie](https://github.com/mealie-recipes/mealie) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `9000` | TCP | Mealie web interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/mealie` | `/app/data` | RW | Application data, database, uploaded images |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

### 🌐 Application Settings

| Variable | Default | Masked | Description |
|---|---|---|---|
| `BASE_URL` | `http://localhost:9000` | ❌ | The URL where Mealie is accessed. Update for reverse proxy. |
| `ALLOW_SIGNUP` | `true` | ❌ | Allow new user registrations |
| `DEFAULT_GROUP` | `Home` | ❌ | Default group name for new users |
| `DEFAULT_EMAIL` | `admin@example.com` | ❌ | Default admin email for initial setup |
| `DEFAULT_PASSWORD` | `MyPassword123` | ✅ | Default admin password (change after first login!) |
| `MAX_WORKERS` | `1` | ❌ | Number of background workers for recipe imports |

---

## 🚀 Quick Start

### Step 1: Install via MOS Hub

1. Open the **MOS Hub** and search for **Mealie**
2. Review and optionally customize:
   - `DEFAULT_EMAIL` - Set your admin email
   - `DEFAULT_PASSWORD` - Change the default password
   - `BASE_URL` - Update if using reverse proxy (e.g., `https://recipes.yourdomain.com`)
3. Click **Install**
4. Wait for the container to start (first startup creates the database)

### Step 2: First Login

1. Access Mealie at `http://YOUR_SERVER_IP:9000`
2. Login with the default credentials:
   - **Email:** `admin@example.com` (or your custom `DEFAULT_EMAIL`)
   - **Password:** `MyPassword123` (or your custom `DEFAULT_PASSWORD`)
3. **Immediately change the password** in User Settings

### Step 3: Import Your First Recipe

1. Click **Create Recipe**
2. Select **Import from URL**
3. Paste a recipe URL (e.g., from AllRecipes, Food Network, etc.)
4. Mealie will automatically scrape and format the recipe

---

## 🔧 Configuration

### Disable Public Signup

After creating your account, disable new registrations:
```
ALLOW_SIGNUP=false
```
Then redeploy the container.

### Reverse Proxy Setup

Update `BASE_URL` to your external URL:
```
BASE_URL=https://recipes.yourdomain.com
```

### Multi-User Setup

- Keep `ALLOW_SIGNUP=true` to let family members create accounts
- Each user has private recipes by default
- Share recipes by adding them to groups
- Groups allow collaborative recipe collections

---

## 📊 Key Features

### Recipe Management
- Import recipes from URLs (auto-scraping)
- Manual recipe entry with rich text editor
- Upload recipe images
- Organize with tags and categories
- Full-text search
- Recipe scaling (adjust servings)
- Print-friendly layouts

### Meal Planning
- Weekly meal planner with drag-and-drop
- Calendar view for meal schedule
- Reuse previous meal plans
- Shopping list generation from meal plans

### Shopping Lists
- Auto-generate from meal plans
- Manual item addition
- Mark items as purchased
- Export to various formats

### User Features
- User registration and management
- Recipe ratings and reviews
- Recipe comments
- Private vs shared recipes
- API access for integrations

---

## 🔌 API & Integrations

Mealie provides a REST API for integrations:
- **API Docs:** `http://YOUR_SERVER_IP:9000/docs`
- **API Key:** Generate in User Settings

### Home Assistant Integration
Use the Mealie integration to:
- Display daily meal plans
- Add recipes to shopping list
- Search recipes by voice

### Mobile Apps
While there's no official mobile app, the web interface is fully responsive and works great on mobile browsers. Some users create "Add to Home Screen" shortcuts for a native app feel.

---

## 🛠️ Troubleshooting

### Recipe Import Fails

Some websites block scraping. Try:
1. Manual entry instead of URL import
2. Use the browser bookmarklet (in Mealie settings)
3. Check if the site has structured recipe data

### High Memory Usage

Reduce workers or restart container:
```
MAX_WORKERS=1
```

### Database Locked Errors

Usually resolves after container restart. For persistent issues:
```bash
docker exec -it mealie rm -f /app/data/mealie.db-shm /app/data/mealie.db-wal
docker restart mealie
```

### Slow Recipe Imports

Increase workers if you have CPU resources:
```
MAX_WORKERS=2
```

### Images Not Loading

1. Check volume permissions: `chown -R 500:500 /mnt/cache/appdata/mealie`
2. Verify PUID/PGID match your system
3. Restart container

---

## 💡 Tips

- Use the **recipe scraper** for most cooking websites - it extracts ingredients, instructions, and images automatically
- Create **recipe collections** for holidays, seasons, or meal types
- Set up **household groups** to share recipes with family
- Use **meal planning** to reduce food waste and save money
- **Tag recipes** for dietary restrictions (vegan, gluten-free, etc.)
- The **API** allows integrations with smart home systems

---

> 💡 **Tip:** After initial setup, immediately change the default password and consider disabling `ALLOW_SIGNUP` if you don't want public registrations.

> 💡 **Tip:** Use the browser extension bookmarklet for easy recipe import while browsing cooking websites.

> 📚 **For more information:** Visit the [Mealie Documentation](https://docs.mealie.io/) for detailed guides on meal planning, user management, and API usage.
