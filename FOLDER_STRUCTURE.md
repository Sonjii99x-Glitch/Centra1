# 📁 PisoNet Complete Folder Structure

```
pisonet/
│
├── README.md                               # Main documentation (START HERE)
├── QUICK_START.md                          # 5-minute quick start guide
├── FOLDER_STRUCTURE.md                     # This file
│
├── server/                                 # Orange Pi Server
│   ├── pisonet_server.py                  # Main Flask server application
│   ├── requirements.txt                   # Python dependencies
│   ├── install.sh                         # Automated installation script
│   ├── install_opi.sh                     # Orange Pi specific installer
│   ├── pisonet.service                    # Systemd service file
│   │
│   └── templates/                         # Web interface templates
│       ├── login.html                     # Login page
│       ├── dashboard.html                 # Main admin dashboard
│       ├── license.html                   # License management page
│       └── logs.html                      # System logs viewer
│
├── client/                                 # Windows Client Application
│   ├── pisonet_client.py                  # Main PyQt5 client application
│   ├── requirements.txt                   # Python dependencies
│   ├── oneclick_client.bat                # Windows one-click installer
│   ├── client_config.ini                  # Client configuration template
│   │
│   └── assets/                            # Images and icons (optional)
│       ├── icon.ico                       # Application icon
│       └── logo.png                       # Logo image
│
├── installer/                             # Installation & Deployment Tools
│   ├── oneclick_master.bat                # Master Windows installer menu
│   ├── oneclick_client.bat                # Client-only installer
│   ├── install_opi.sh                     # Orange Pi setup script
│   ├── oneclick_installer.py              # Universal Python installer
│   ├── install_license.bat                # License setup helper
│   │
│   └── scripts/                           # Helper scripts
│       ├── deploy_ssh.sh                  # SSH deployment script
│       ├── backup.sh                      # Backup script
│       └── restore.sh                     # Restore script
│
├── docs/                                  # Documentation
│   ├── INSTALLATION.md                    # Complete installation guide
│   ├── API.md                             # REST API documentation
│   ├── TROUBLESHOOTING.md                 # Troubleshooting guide
│   ├── CONFIGURATION.md                   # Configuration reference
│   │
│   ├── hardware/                          # Hardware documentation
│   │   ├── GPIO_WIRING.md                # GPIO pin wiring guide
│   │   ├── COIN_DETECTOR.md              # Coin detector setup
│   │   └── RELAY_WIRING.md               # Relay configuration
│   │
│   ├── images/                           # Documentation images
│   │   ├── gpio-pinout.png              # GPIO pin diagram
│   │   ├── wiring-diagram.png           # Hardware wiring diagram
│   │   └── dashboard-screenshot.png     # Dashboard preview
│   │
│   └── examples/                         # Configuration examples
│       ├── config-gaming.ini             # Gaming cafe config
│       ├── config-classroom.ini          # Classroom config
│       └── api-examples.sh               # API usage examples
│
├── tests/                                # Test files (optional)
│   ├── test_server.py                    # Server unit tests
│   ├── test_client.py                    # Client unit tests
│   ├── test_gpio.py                      # GPIO tests
│   └── test_api.sh                       # API integration tests
│
├── config/                               # Configuration templates
│   ├── pisonet_server_default.py         # Server config template
│   ├── client_config.ini                 # Client config template
│   └── nginx_reverse_proxy.conf          # Nginx config for HTTPS
│
├── scripts/                              # Utility scripts
│   ├── setup.sh                          # One-time setup script
│   ├── update.sh                         # Update script
│   ├── backup.sh                         # Backup database
│   ├── restore.sh                        # Restore database
│   ├── reset_password.sh                 # Reset admin password
│   ├── uninstall.sh                      # Uninstall script
│   │
│   └── windows/                          # Windows batch scripts
│       ├── setup.bat                     # Windows setup
│       ├── backup.bat                    # Windows backup
│       └── update.bat                    # Windows update
│
├── migrations/                           # Database migrations
│   ├── 001_initial_schema.sql            # Initial database schema
│   ├── 002_add_fields.sql                # Schema updates
│   └── migrate.py                        # Migration runner
│
├── locale/                               # i18n translations (optional)
│   ├── en/                               # English translations
│   ├── es/                               # Spanish translations
│   └── fr/                               # French translations
│
├── logs/                                 # Log directory (created at runtime)
│   ├── server.log                        # Server logs
│   └── client.log                        # Client logs
│
├── data/                                 # Data directory (created at runtime)
│   ├── pisonet.db                        # SQLite database
│   ├── license.key                       # License file
│   └── backups/                          # Automatic backups
│
├── .gitignore                            # Git ignore file
├── .env.example                          # Environment variables template
├── LICENSE                               # License file
└── VERSION                               # Version file

```

---

## File Descriptions

### Root Level

| File | Purpose |
|------|---------|
| `README.md` | Main documentation, installation overview |
| `QUICK_START.md` | 5-minute setup guide for non-tech users |
| `FOLDER_STRUCTURE.md` | This file, describes all folders |

### `server/` Directory

**Main Application:**
- `pisonet_server.py` - Main Flask server (2000+ lines)
  - GPIO handling
  - Database management
  - License validation
  - REST API endpoints
  - Web routing

**Configuration:**
- `requirements.txt` - Python package dependencies

**Installation:**
- `install.sh` - Generic installation script
- `install_opi.sh` - Orange Pi specific setup
- `pisonet.service` - Systemd service definition

**Web Interface:**
- `templates/login.html` - Login page (styled, responsive)
- `templates/dashboard.html` - Admin dashboard (live updates)
- `templates/license.html` - License manager
- `templates/logs.html` - Log viewer with filtering

### `client/` Directory

**Main Application:**
- `pisonet_client.py` - PyQt5 client application
  - Floating timer window
  - Background thread for server sync
  - System tray integration
  - Configuration management

**Configuration:**
- `requirements.txt` - Python dependencies (PyQt5, requests)
- `client_config.ini` - Client configuration file

**Installation:**
- `oneclick_client.bat` - Windows installer (one-click)

### `installer/` Directory

**Windows Installers:**
- `oneclick_master.bat` - Master menu installer
- `oneclick_client.bat` - Client-only installer
- `install_license.bat` - License setup helper
- `oneclick_installer.py` - Universal Python installer

**Server Setup:**
- `install_opi.sh` - Orange Pi installation script

**Helper Scripts:**
- `scripts/deploy_ssh.sh` - SSH-based deployment
- `scripts/backup.sh` - Database backup
- `scripts/restore.sh` - Backup restoration

### `docs/` Directory

**Main Documentation:**
- `INSTALLATION.md` - Complete install guide with troubleshooting
- `API.md` - REST API reference
- `TROUBLESHOOTING.md` - Common issues and solutions
- `CONFIGURATION.md` - Configuration reference

**Hardware Guides:**
- `hardware/GPIO_WIRING.md` - GPIO pinout and wiring
- `hardware/COIN_DETECTOR.md` - Coin sensor setup
- `hardware/RELAY_WIRING.md` - Relay configuration

**Examples:**
- `examples/config-gaming.ini` - Gaming cafe configuration
- `examples/config-classroom.ini` - Classroom setup
- `examples/api-examples.sh` - API usage examples

### `scripts/` Directory

**Setup & Maintenance:**
- `setup.sh` - Initial setup script (Linux)
- `setup.bat` - Initial setup script (Windows)
- `update.sh` - Update script (Linux)
- `backup.sh` - Database backup (Linux)
- `restore.sh` - Restore from backup (Linux)

**Utilities:**
- `reset_password.sh` - Reset admin password
- `uninstall.sh` - Clean uninstall

### `config/` Directory

**Templates:**
- `pisonet_server_default.py` - Server config template
- `client_config.ini` - Client config template
- `nginx_reverse_proxy.conf` - HTTPS setup with nginx

### `migrations/` Directory

**Database Schema:**
- `001_initial_schema.sql` - Initial database structure
- `002_add_fields.sql` - Schema updates
- `migrate.py` - Migration runner

---

## Runtime Directories

These directories are created when the system runs:

### Orange Pi Server

```
/opt/pisonet/                    # Server installation
├── pisonet_server.py           # Copied from server/
├── requirements.txt
└── templates/
    ├── login.html
    ├── dashboard.html
    ├── license.html
    └── logs.html

/var/lib/pisonet/               # Data directory
├── pisonet.db                  # SQLite database
├── license.key                 # License file
└── backups/
    ├── pisonet-2024-01-15.db

/var/log/pisonet/               # Log directory
├── server.log                  # Main log file
└── pisonet.log                 # Systemd output
```

### Windows Client

```
C:\Users\Username\AppData\Roaming\PisoNet\
├── client_config.ini           # Client configuration
├── client.log                  # Client log file
└── pisonet_client.py          # Copied from client/

C:\ProgramData\PisoNet\
├── license.key                 # License file

C:\Program Files\PisoNet\
├── pisonet_client.exe          # Compiled executable (if packaged)
```

---

## File Sizes (Approximate)

| File | Size | Description |
|------|------|-------------|
| `pisonet_server.py` | 15 KB | Main server |
| `pisonet_client.py` | 12 KB | Client application |
| `dashboard.html` | 20 KB | Admin interface |
| `pisonet.db` | Grows | SQLite database |
| `server.log` | Grows | Log file |

**Total uncompressed:** ~200 KB
**Compressed (zip):** ~50 KB

---

## Essential Files Checklist

### For Server Installation
- ✅ `server/pisonet_server.py` - Main app
- ✅ `server/templates/*.html` - Web pages
- ✅ `server/requirements.txt` - Dependencies
- ✅ `server/pisonet.service` - Service file

### For Client Installation
- ✅ `client/pisonet_client.py` - Client app
- ✅ `client/requirements.txt` - Dependencies
- ✅ `client/oneclick_client.bat` - Installer

### For Documentation
- ✅ `README.md` - Main guide
- ✅ `QUICK_START.md` - Quick setup
- ✅ `docs/INSTALLATION.md` - Install guide
- ✅ `docs/TROUBLESHOOTING.md` - Help

### For Deployment
- ✅ `installer/oneclick_master.bat` - Master installer
- ✅ `installer/oneclick_installer.py` - Python installer
- ✅ `server/install.sh` - Server install
- ✅ `server/install_opi.sh` - Orange Pi install

---

## Optional Files

These are not essential but enhance functionality:

- `tests/` - Unit tests
- `docs/hardware/` - Hardware guides
- `docs/images/` - Diagrams
- `config/nginx_reverse_proxy.conf` - HTTPS setup
- `migrations/` - Schema updates
- `locale/` - Language translations
- `LICENSE` - License file
- `.gitignore` - Git configuration

---

## Deployment Package

**For distribution, include only:**

```
PisoNet-1.0.0/
├── README.md
├── QUICK_START.md
├── server/
│   ├── pisonet_server.py
│   ├── templates/
│   ├── requirements.txt
│   ├── install.sh
│   └── pisonet.service
├── client/
│   ├── pisonet_client.py
│   ├── requirements.txt
│   └── oneclick_client.bat
├── installer/
│   ├── oneclick_master.bat
│   ├── oneclick_installer.py
│   └── install_opi.sh
├── docs/
│   ├── INSTALLATION.md
│   ├── API.md
│   ├── TROUBLESHOOTING.md
│   └── CONFIGURATION.md
└── LICENSE
```

**Total size:** ~150 KB (source)

---

## Git Repository Structure

If hosting on GitHub:

```
GitHub: pisonet/pisonet
├── main branch - Stable releases
├── develop branch - Development
├── hotfix/* branches - Emergency fixes
└── releases/ - Release tags
```

---

## Backup Strategy

**What to back up:**
- Database: `/var/lib/pisonet/pisonet.db`
- License: `/var/lib/pisonet/license.key`
- Config: Client's `client_config.ini`

**Don't back up:**
- Logs (can be regenerated)
- Temporary files
- `.pyc` compiled files

---

**Last Updated:** 2024-01-15
**PisoNet Version:** 1.0.0
