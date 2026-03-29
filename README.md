# 🪙 PisoNet - Complete Cybercafe System

**A complete, production-ready cybercafe management system** for Orange Pi/Raspberry Pi servers with Windows client apps and web dashboard.

---

## 📦 SYSTEM OVERVIEW

### Hardware Requirements
- **Server**: Orange Pi One (1GB RAM) or Raspberry Pi
- **Coin Detector**: Connected to GPIO Pin 3
- **Relay**: Connected to GPIO Pin 5
- **Network**: WiFi/Ethernet with DHCP

### Software Components
1. **Orange Pi Server** - Python Flask server + GPIO control
2. **Windows Client** - PyQt5 client application
3. **Web Admin Dashboard** - Real-time management interface
4. **Licensing System** - Device-bound activation

---

## 🚀 QUICK START (NON-TECH USERS)

### Step 1: Install Orange Pi Server
```bash
# SSH into your Orange Pi (default: pi@192.168.x.x)
# Run this command:
curl -O https://raw.githubusercontent.com/yourusername/pisonet/main/install_opi.sh
chmod +x install_opi.sh
sudo ./install_opi.sh
```

**OR use the all-in-one installer:**
```bash
python oneclick_installer.py
```

### Step 2: Install Windows Client
1. Download `oneclick_client.bat`
2. Right-click → Run as Administrator
3. Select your server IP
4. Done! Client starts automatically

### Step 3: Activate License
1. Open Web Admin Dashboard: `http://pisonet.local:5000`
2. Go to License → Activate Online
3. Click "Activate Now"
4. System locked to your device forever

### Step 4: Start Operating
- **Windows Client**: Starts automatically after installation
- **Web Dashboard**: Access at `http://pisonet.local:5000`
- **Default Admin Password**: `admin123` (CHANGE THIS!)

---

## 📋 FILE STRUCTURE

```
pisonet/
├── server/                          # Orange Pi Server
│   ├── pisonet_server.py           # Main Flask server
│   ├── gpio_controller.py          # GPIO hardware control
│   ├── license_manager.py          # Licensing & activation
│   ├── database_manager.py         # SQLite database
│   ├── requirements.txt            # Python dependencies
│   ├── install.sh                  # Auto-install script
│   ├── install_opi.sh              # Orange Pi installer
│   ├── pisonet.service             # Systemd service
│   └── templates/                  # Flask HTML templates
│       ├── dashboard.html          # Admin dashboard
│       ├── license.html            # License manager
│       └── logs.html               # System logs
│
├── client/                         # Windows Client
│   ├── pisonet_client.py          # Main PyQt5 app
│   ├── requirements.txt            # Python dependencies
│   ├── oneclick_client.bat         # Windows installer
│   ├── client_config.ini           # Client config
│   └── assets/                     # Images & icons
│
├── installer/                      # One-click installers
│   ├── oneclick_master.bat         # Master installer
│   ├── oneclick_client.bat         # Client installer
│   ├── install_license.bat         # License installer
│   └── oneclick_installer.py       # Python universal installer
│
├── docs/                           # Documentation
│   ├── INSTALLATION.md             # Full installation guide
│   ├── API.md                      # API documentation
│   ├── TROUBLESHOOTING.md          # Troubleshooting guide
│   └── CONFIGURATION.md            # Configuration guide
│
└── README.md                       # This file
```

---

## 🔑 DEFAULT CREDENTIALS

**Web Admin Dashboard**
- URL: `http://pisonet.local:5000`
- Username: `admin`
- Password: `admin123`

⚠️ **IMPORTANT**: Change password immediately after first login!

---

## 🎯 KEY FEATURES

✅ **Automatic Client Registration**
- Clients register themselves automatically
- No manual configuration needed
- Supports unlimited concurrent clients

✅ **Real-Time Monitoring**
- Live view of all connected clients
- Real-time timer sync
- Session management

✅ **Coin & Payment System**
- GPIO-based coin detection
- Automatic timer increment
- Relay control for session management

✅ **Device-Bound Licensing**
- One-time online activation
- CPU serial + MAC address hashing
- Anti-tamper validation
- Offline operation after activation

✅ **Trial Mode**
- Max 2 simultaneous clients
- 30-minute runtime per session
- Auto-shutdown after timeout

✅ **Floating UI Client**
- Minimalist timer display
- Always-on-top window
- System tray integration
- Minimal CPU usage

---

## 🛠️ CONFIGURATION

### Server Configuration (`server/pisonet_server.py`)
```python
# Network
SERVER_IP = "0.0.0.0"
SERVER_PORT = 5000
HOSTNAME = "pisonet.local"

# GPIO
COIN_INPUT_PIN = 3
RELAY_OUTPUT_PIN = 5

# Trial Settings (if no license)
TRIAL_MAX_CLIENTS = 2
TRIAL_MAX_MINUTES = 30

# Database
DB_PATH = "/var/lib/pisonet/pisonet.db"
```

### Client Configuration (`client/client_config.ini`)
```ini
[Server]
server_ip = 192.168.1.100
server_port = 5000

[Client]
client_name = Workstation-1
check_interval = 1

[UI]
timer_x = 10
timer_y = 10
transparency = 0.9
```

---

## 📡 NETWORK SETUP

### Server Discovery
The server broadcasts as `pisonet.local` on your network.

**To find your server IP:**
```bash
# On Windows (CMD)
ping pisonet.local

# On Mac/Linux (Terminal)
ping pisonet.local
```

### Port Requirements
- **5000**: Web Dashboard & API
- **5001**: Client-Server Communication
- **GPIO Pins 3, 5**: Hardware control

---

## 🔐 LICENSING & ACTIVATION

### How Licensing Works
1. **Device Fingerprint**: CPU serial + MAC address hashed
2. **Online Activation**: One-time internet connection required
3. **Offline Operation**: Works without internet after activation
4. **Anti-Tamper**: Validates device hasn't changed

### License File Location
- **Windows**: `C:\ProgramData\PisoNet\license.key`
- **Linux**: `/var/lib/pisonet/license.key`

### Activate Online
1. Open `http://pisonet.local:5000`
2. Login with admin credentials
3. Go to License → Activate
4. Click "Activate Online"
5. License stored permanently

---

## ⚙️ TRIAL MODE

**Trial limitations (until activated):**
- Maximum 2 clients simultaneously
- 30 minutes per session
- Full features unlocked
- Auto-shutdown after 30 minutes

To activate and remove limitations, see **Licensing & Activation** above.

---

## 🐛 TROUBLESHOOTING

### Server won't start
```bash
# Check service status
sudo systemctl status pisonet

# View logs
sudo journalctl -u pisonet -f

# Manual restart
sudo systemctl restart pisonet
```

### Client can't find server
1. Ping the server: `ping pisonet.local`
2. Check firewall settings
3. Ensure server IP is correct in config

### GPIO not detecting coins
1. Check coin sensor wiring (Pin 3)
2. Test GPIO: `gpio read 3`
3. Check relay output (Pin 5): `gpio read 5`

### License activation fails
1. Check internet connection
2. Firewall may be blocking activation
3. Check server logs: `sudo journalctl -u pisonet -f`

See **TROUBLESHOOTING.md** for more help.

---

## 📞 TECHNICAL SUPPORT

**For issues, check these in order:**
1. `docs/TROUBLESHOOTING.md` - Common problems
2. Server logs: `sudo journalctl -u pisonet -f`
3. Client logs: `C:\Users\YourName\AppData\Local\PisoNet\client.log`

---

## 🔄 UPDATES

To update to the latest version:

**On Orange Pi:**
```bash
cd ~/pisonet
git pull origin main
sudo systemctl restart pisonet
```

**On Windows:**
Download latest `oneclick_client.bat` and run it.

---

## 📝 LICENSE

This software is proprietary. For commercial use, contact licensing.

---

## ✨ FEATURES CHECKLIST

- [x] Orange Pi server with GPIO control
- [x] Windows PyQt5 client
- [x] Web admin dashboard
- [x] Device-bound licensing
- [x] Coin detection & relay control
- [x] Real-time sync system
- [x] Floating timer UI
- [x] Auto-registration system
- [x] Trial mode (2 clients, 30 min)
- [x] Anti-tamper validation
- [x] One-click installers
- [x] Systemd integration
- [x] Complete documentation

---

**Built with ❤️ for cybercafes worldwide**
