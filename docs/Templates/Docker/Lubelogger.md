---
title: 🚗 Lubelogger
layout: default
parent: 🐳 Docker
grand_parent: 🗂️ Templates
nav_order: 32
---

# 🚗 Lubelogger

<img src="https://raw.githubusercontent.com/J000K3R/MOS-Templates/main/icons/lubelogger.png" width="80" />

**Lubelogger** is a self-hosted vehicle maintenance and fuel mileage tracker. Log repairs, maintenance, fuel purchases, and expenses for multiple vehicles. Generate reports, track costs, and keep your vehicle history organized with a clean web interface.

Perfect for car enthusiasts, fleet owners, and anyone who wants to keep detailed records of their vehicles.

🏷️ **Category:** Productivity

🐳 **Image:** `ghcr.io/hargata/lubelogger:latest`

---

## 🔗 Links

| | |
|---|---|
| 🌐 **Website** | [github.com/hargata/lubelogger](https://github.com/hargata/lubelogger) |
| 📖 **Documentation** | [Lubelogger Wiki](https://github.com/hargata/lubelogger/wiki) |
| 🐛 **Support** | [GitHub Issues](https://github.com/hargata/lubelogger/issues) |
| 💛 **GitHub** | [github.com/hargata/lubelogger](https://github.com/hargata/lubelogger) |

---

## 🌐 Ports

| Port | Protocol | Description |
|---|---|---|
| `8080` | TCP | Lubelogger web interface |

---

## 💾 Volumes

| Host Path | Container Path | Mode | Description |
|---|---|---|---|
| `/mnt/cache/appdata/lubelogger` | `/App/config` | RW | Configuration, database, and uploaded attachments |

---

## ⚙️ Environment Variables

### 👤 Permissions

| Variable | Default | Masked | Description |
|---|---|---|---|
| `PUID` | `500` | ❌ | User ID for file permissions |
| `PGID` | `500` | ❌ | Group ID for file permissions |
| `TZ` | `Europe/Vienna` | ❌ | Timezone for the container |

### 🔐 Authentication

| Variable | Default | Masked | Description |
|---|---|---|---|
| `LUBELOGGER_AUTHENTICATION` | `true` | ❌ | Enable login authentication |
| `LUBELOGGER_USERNAME` | `admin` | ❌ | Default admin username |
| `LUBELOGGER_PASSWORD` | `MyPassword123` | ✅ | Default admin password (change after first login!) |

### 📧 Email (Optional)

| Variable | Default | Masked | Description |
|---|---|---|---|
| `LUBELOGGER_SMTP_HOST` | _(empty)_ | ❌ | SMTP server host |
| `LUBELOGGER_SMTP_PORT` | `587` | ❌ | SMTP server port |
| `LUBELOGGER_SMTP_USERNAME` | _(empty)_ | ❌ | SMTP username |
| `LUBELOGGER_SMTP_PASSWORD` | _(empty)_ | ✅ | SMTP password |

### 📝 Logging

| Variable | Default | Masked | Description |
|---|---|---|---|
| `LUBELOGGER_LOG_LEVEL` | `Information` | ❌ | Logging level (Verbose, Debug, Information, Warning, Error) |

---

## 🚀 Quick Start

### Step 1: Install via MOS Hub

1. Open the **MOS Hub** and search for **Lubelogger**
2. Review and optionally customize:
   - `LUBELOGGER_USERNAME` - Set your admin username
   - `LUBELOGGER_PASSWORD` - Change the default password
   - `TZ` - Set your timezone
3. Click **Install**
4. Wait for the container to start

### Step 2: First Login

1. Access Lubelogger at `http://YOUR_SERVER_IP:8080`
2. Login with the default credentials:
   - **Username:** `admin` (or your custom `LUBELOGGER_USERNAME`)
   - **Password:** `MyPassword123` (or your custom `LUBELOGGER_PASSWORD`)
3. **Immediately change the password** in Settings

### Step 3: Add Your First Vehicle

1. Click **Add Vehicle**
2. Enter vehicle details:
   - **Description:** e.g., "2020 Toyota Corolla"
   - **Make, Model, Year**
   - **License Plate** (optional)
   - **VIN** (optional)
3. Click **Save**

### Step 4: Log Your First Fuel Record

1. Select your vehicle
2. Go to the **Fuel** tab
3. Click **Add Record**
4. Enter:
   - **Date** of the fuel purchase
   - **Odometer** reading
   - **Fuel Amount** (liters/gallons)
   - **Cost** per unit
5. Click **Save** — Lubelogger automatically calculates MPG/L per 100km

---

## 🔧 Configuration

### Disable Authentication (Not Recommended)

For local-only or testing environments:
```
LUBELOGGER_AUTHENTICATION=false
```
Then redeploy the container.

### Enable Email Notifications

Configure SMTP for reminders and reports:
```
LUBELOGGER_SMTP_HOST=smtp.yourprovider.com
LUBELOGGER_SMTP_PORT=587
LUBELOGGER_SMTP_USERNAME=your@email.com
LUBELOGGER_SMTP_PASSWORD=yourpassword
```

### Reverse Proxy Setup

Lubelogger works behind reverse proxies (SWAG, Traefik, Caddy, etc.). Point your proxy to port `8080`.

---

## 📊 Key Features

### Vehicle Management
- Track multiple vehicles
- Store vehicle metadata (VIN, plate, make/model/year)
- Attach documents (insurance, registration, receipts)
- Custom vehicle notes

### Fuel Tracking
- Log fuel purchases with date, odometer, and cost
- Automatic MPG / L per 100km calculation
- Fuel cost trends over time
- Fuel economy reports

### Maintenance Records
- Log repairs and scheduled maintenance
- Track service providers and costs
- Set recurring reminders (oil changes, inspections, etc.)
- Attach receipts and invoices

### Expense Tracking
- Record all vehicle-related expenses
- Categorize costs (fuel, maintenance, insurance, etc.)
- Total cost of ownership reports
- Per-mile / per-km cost analysis

### Reports & Analytics
- Generate printable reports
- Export data to CSV
- Cost breakdowns by category
- Fuel economy trends

---

## 🛠️ Troubleshooting

### Cannot Login

1. Verify `LUBELOGGER_AUTHENTICATION` is `true`
2. Check `LUBELOGGER_USERNAME` and `LUBELOGGER_PASSWORD` values
3. Restart the container

### Database Errors

1. Check volume permissions: `chown -R 500:500 /mnt/cache/appdata/lubelogger`
2. Verify PUID/PGID match your system
3. Restart container

### Email Notifications Not Working

1. Verify SMTP credentials are correct
2. Check your SMTP provider allows app access
3. Test with a simple SMTP service (e.g., Gmail with app password)
4. Check container logs for SMTP errors

### Attachments Not Uploading

1. Check volume permissions on `/App/config`
2. Verify sufficient disk space
3. Check `LUBELOGGER_LOG_LEVEL=Debug` for detailed errors

---

## 💡 Tips

- **Add photos** of receipts and invoices directly to records for a complete paper trail
- **Set reminders** for recurring maintenance (oil changes, tire rotations, inspections)
- **Use the reports** feature at tax time to calculate vehicle expenses
- **Track multiple vehicles** — great for families or small fleets
- **Export your data** regularly as a backup (CSV export available)
- **Use custom notes** to record warranty info, part numbers, or mechanic details

---

> 💡 **Tip:** After initial setup, immediately change the default password to secure your vehicle data.

> 💡 **Tip:** Set up SMTP email notifications to receive maintenance reminders — never miss an oil change or inspection deadline again.

> 📚 **For more information:** Visit the [Lubelogger Wiki](https://github.com/hargata/lubelogger/wiki) for detailed guides on features, configuration, and usage.