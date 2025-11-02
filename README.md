## wordpress-plugins

![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-automated-blue)
![Update Frequency](https://img.shields.io/badge/update-every_6_hours-brightgreen)
![WordPress Plugins](https://img.shields.io/badge/plugins-60,000%2B-orange)
![Go Version](https://img.shields.io/badge/Go-1.23.3-00ADD8)

A continuously updated repository that monitors and collects data from the official WordPress Plugin Directory, automatically refreshed every 6 hours using GitHub Actions.

## 🚀 Overview

This repository serves as a comprehensive monitoring system for WordPress plugins, tracking the entire ecosystem from the official WordPress Plugin Directory. It provides developers, security researchers, and WordPress administrators with up-to-date plugin information and metadata.

## ⚙️ How It Works

### Automated Monitoring Pipeline

```yaml
Schedule: Runs every 6 hours
Trigger: Also on push to main branch
Process:
1. 📊 Queries WordPress Plugin API for total plugin count
2. 🔍 Scrapes plugin metadata using wppdm
3. 💾 Stores data in structured JSON format
4. ✅ Automatically commits and pushes updates
```

### Tools Used

- **wppdm**: Custom tool for WordPress plugin data mining
- **WordPress Plugin API**: Official source for plugin information
- **GitHub Actions**: For automation and scheduling
- **jq**: For JSON processing and data extraction

## 📂 Repository Structure

```
wordpress-plugins-monitor/
├── .github/workflows/
│   └── monitor.yml           # Automation workflow
├── plugins.json              # Plugin metadata (auto-generated)
├── plugins_count.txt         # Total plugin count (auto-generated)
└── README.md
```

## 🛠️ Usage

### Accessing Plugin Data

The collected data is available in two main formats:

1. **plugins_count.txt**: Contains the total number of plugins in the WordPress directory
2. **plugins.json**: Comprehensive metadata for all available plugins

### Example Usage

```bash
# Get total plugin count
cat plugins_count.txt

# Query specific plugin information from JSON
jq '.[] | select(.slug == "woocommerce")' plugins.json

# Extract plugin names and versions
jq '.[] | {name, version, downloaded}' plugins.json

# Find recently updated plugins
jq '.[] | select(.last_updated > "2024-01-01")' plugins.json
```

### For Developers

```python
import json
import requests

# Load plugin data directly from this repository
url = "https://raw.githubusercontent.com/rix4uni/wordpress-plugins/main/plugins.json"
response = requests.get(url)
plugins_data = response.json()

# Analyze plugin ecosystem
total_plugins = len(plugins_data)
active_plugins = [p for p in plugins_data if p.get('active_installs', 0) > 0]
recent_updates = [p for p in plugins_data if p.get('last_updated', '').startswith('2024')]
```

## 🔄 Automation Details

- **Schedule**: Runs every 6 hours (0 */6 * * *)
- **Data Source**: Official WordPress Plugin API (`api.wordpress.org/plugins/info/1.2/`)
- **Tools**: 
  - `wppdm` for plugin data mining
  - `jq` for JSON processing
  - GitHub Actions for automation
- **Commit**: Automatically commits changes with IST timestamp

## 📊 Data Collection

### What We Monitor

- **Plugin Metadata**: Name, slug, version, description
- **Statistics**: Active installations, ratings, downloaded count
- **Timestamps**: Last updated, added dates
- **Compatibility**: Tested up to WordPress version
- **Author Information**: Developer details and profiles
- **Repository Links**: WordPress.org and author URLs

### Sample Data Structure

```json
{
  "name": "Example Plugin",
  "slug": "example-plugin",
  "version": "1.2.3",
  "author": "developer123",
  "author_profile": "https://profiles.wordpress.org/developer123/",
  "contributors": {},
  "requires": "5.8",
  "tested": "6.3",
  "requires_php": "7.4",
  "rating": 90,
  "ratings": {
    "1": 1,
    "2": 1,
    "3": 2,
    "4": 3,
    "5": 23
  },
  "num_ratings": 30,
  "support_threads": 5,
  "support_threads_resolved": 3,
  "active_installs": 10000,
  "last_updated": "2024-01-15 10:30:00",
  "added": "2023-05-20 14:25:00",
  "homepage": "https://wordpress.org/plugins/example-plugin/",
  "download_link": "https://downloads.wordpress.org/plugin/example-plugin.1.2.3.zip",
  "tags": {
    "ecommerce": "Ecommerce",
    "payment": "Payment"
  },
  "donate_link": "",
  "banners": {
    "low": "https://ps.w.org/example-plugin/assets/banner-772x250.jpg",
    "high": "https://ps.w.org/example-plugin/assets/banner-1544x500.jpg"
  }
}
```

## 🔗 Related Projects

- [WordPress Plugin Directory](https://wordpress.org/plugins/) - Official plugin repository
- [WordPress API](https://codex.wordpress.org/WordPress.org_API) - Official WordPress.org API documentation
- [wppdm](https://github.com/rix4uni/wppdm) - WordPress plugin data mining tool
