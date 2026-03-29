# 🪙 PisoNet System Overview

## What is PisoNet?

PisoNet is a **complete, production-ready cybercafe management system** designed for small to large cybercafes. It consists of:

1. **Orange Pi Server** - Central management server with GPIO control
2. **Windows Client** - Floating timer application for each workstation
3. **Web Dashboard** - Real-time admin interface
4. **Licensing System** - Device-bound activation (online once, works offline forever)

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    PisoNet System                            │
└─────────────────────────────────────────────────────────────┘

                        NETWORK
                ┌───────────┬───────────┐
                │           │           │
        ┌───────▼────────┐  │  ┌───────▼────────┐
        │  Orange Pi     │  │  │  Windows       │
        │  Server        │  │  │  Clients       │
        │  (Port 5000)   │  │  │  (PyQt5 App)   │
        │                │  │  │                │
        │  - Flask API   │  │  │  - Timer UI    │
        │  - Database    │  │  │  - Sync Thread │
        │  - GPIO Control│  │  │  - Auto-update │
        │  - Licensing   │  │  │                │
        └────┬───────────┘  │  └────────────────┘
             │              │
             └──────────────┘
                    │
        ┌───────────┴───────────┐
        │                       │
        ▼                       ▼
    ┌──────────┐           ┌──────────┐
    │ Database │           │ GPIO     │
    │ SQLite   │           │ Control  │
    │          │           │          │
    │ Clients  │           │ Coin (3) │
    │ Sessions │           │ Relay(5) │
    │ Txns     │           └──────────┘
    └──────────┘
```

---

## Key Components

### 1. Orange Pi Server (`server/pisonet_server.py`)

**What it does:**
- Manages all clients and sessions
- Controls GPIO coin detector and relay
- Stores data in SQLite database
- Provides REST API for clients
- Hosts web dashboard for admins
- Validates licenses and enforces limits

**Hardware:**
- Orange Pi One (1GB RAM)
- GPIO Pin 3: Coin input
- GPIO Pin 5: Relay output
- Network: Ethernet or WiFi

**Ports:**
- 5000: Main API and web interface

**Data Storage:**
- `/var/lib/pisonet/pisonet.db` - Database
- `/var/lib/pisonet/license.key` - License file
- `/var/log/pisonet/server.log` - Logs

---

### 2. Windows Client (`client/pisonet_client.py`)

**What it does:**
- Displays floating timer window
- Syncs with server periodically
- Auto-registers with server
- Updates remaining time
- Allows adding time from workstation
- Minimizes to system tray

**UI:**
- Large timer display (HH:MM:SS)
- Client name label
- +1min and +30min buttons
- Status indicator (connected/error)
- Always-on-top floating window
- Customizable position and transparency

**Features:**
- Background sync thread
- Automatic registration
- Network error handling
- Configuration file support
- Logging for debugging

---

### 3. Web Dashboard (`server/templates/dashboard.html`)

**Access:** `http://pisonet.local:5000`

**Features:**
- Real-time client list
- Server statistics
- Client management (add time)
- GPIO control
- License management
- System logs viewer
- Admin login

**Dashboard Sections:**
1. **Statistics** - Total clients, active clients, uptime, server IP
2. **GPIO Status** - Coin detector status, relay control
3. **Clients List** - All connected clients with remaining time
4. **License Status** - Trial/Licensed status
5. **Logs** - Real-time system logs with filtering
6. **System Info** - Server health and metrics

---

### 4. Licensing System

**How it works:**
1. **Device Fingerprinting**
   - CPU Serial from `/proc/cpuinfo`
   - MAC Address from network interface
   - SHA256 hash of combined string

2. **Trial Mode**
   - Max 2 concurrent clients
   - 30 minutes per session
   - All features available

3. **Online Activation**
   - One-time internet connection required
   - Fingerprint sent to server
   - License generated and stored locally
   - Offline operation after activation

4. **Anti-Tamper**
   - License tied to device hardware
   - Prevents license sharing
   - Validates fingerprint on startup
   - Reactivation required if hardware changes

**License File:**
```json
{
  "fingerprint": "sha256hash...",
  "activation_key": "auto-activation",
  "activated_at": "2024-01-15T10:00:00",
  "expiry": null,
  "version": "1.0",
  "max_clients": 999
}
```

---

## Database Schema

### Clients Table
```sql
id              INTEGER PRIMARY KEY
client_id       TEXT UNIQUE          -- UUID
name            TEXT                 -- Display name
ip_address      TEXT
mac_address     TEXT
total_time      INTEGER              -- Seconds
remaining_time  INTEGER              -- Seconds
status          TEXT                 -- active/idle
last_seen       TIMESTAMP
created_at      TIMESTAMP
```

### Sessions Table
```sql
id              INTEGER PRIMARY KEY
client_id       TEXT
start_time      TIMESTAMP
end_time        TIMESTAMP
duration        INTEGER
cost            REAL
```

### Transactions Table
```sql
id              INTEGER PRIMARY KEY
client_id       TEXT
amount          REAL
type            TEXT                 -- add-time, payment, etc
timestamp       TIMESTAMP
```

---

## REST API Endpoints

### Authentication
- `POST /login` - Admin login
- `GET /logout` - Logout

### Clients
- `POST /api/register` - Client registration
- `GET /api/clients` - List all clients
- `GET /api/client/<id>` - Get client details
- `POST /api/client/<id>/add-time` - Add time to client

### GPIO
- `GET /api/gpio/status` - Get GPIO status
- `POST /api/gpio/relay` - Control relay

### License
- `GET /api/license/status` - License status
- `POST /api/license/activate` - Activate license

### Server
- `GET /api/server/info` - Server information

---

## Installation Methods

### Server
1. **Automated:** Run `install.sh` on Orange Pi
2. **SSH Deployment:** Use SSH installer
3. **Manual:** Follow step-by-step guide

### Client
1. **One-Click:** Run `oneclick_client.bat`
2. **Universal Installer:** Run `oneclick_installer.py`
3. **Manual:** Install dependencies, copy files

### License
1. **Online:** Dashboard → License → Activate
2. **Offline:** Manual activation with support

---

## Network Topology

### Local Network (Recommended)

```
┌────────────────────────────────────┐
│        Local Network               │
│      192.168.1.0/24                │
│                                    │
│  Orange Pi ────┬── Clients 1-5    │
│  192.168.1.100 │                  │
│                │── Clients 6-10   │
│                └── Admin PC       │
│                                    │
└────────────────────────────────────┘
```

### Remote Network (Advanced)

```
┌─ Cybercafe Network
│  192.168.1.0/24
│
│  Orange Pi ──────────────────┐
│  (Port 5000 on Router)       │
│                              │
└──────────────────────────────┼──────────────┐
                               │              │
                         Internet/VPN        │
                               │              │
                         ┌──────▼────────┐    │
                         │ Admin Remote  │    │
                         │ 203.0.113.x   │    │
                         └───────────────┘    │
                                              │
                         Clients on          │
                         Same Network ───────┘
```

---

## File Transfer Methods

### Server Files to Orange Pi

**Option 1: Git Clone**
```bash
git clone https://github.com/yourusername/pisonet.git
cd pisonet
sudo cp -r server /opt/pisonet
```

**Option 2: SCP (Secure Copy)**
```bash
scp -r server/ root@pisonet.local:/opt/pisonet/
```

**Option 3: USB Drive**
1. Copy files to USB
2. Plug into Orange Pi
3. Mount and copy to `/opt/pisonet/`

### Client Files to Windows

**Option 1: Download BAT**
1. Right-click `oneclick_client.bat`
2. Run as Administrator

**Option 2: Manual Copy**
1. Copy `pisonet_client.py` to AppData
2. Copy `requirements.txt`
3. Run installer

**Option 3: Network Share**
1. Share folder from Orange Pi
2. Map drive on client
3. Run installer from share

---

## Security Model

### Authentication
- Admin login with username/password
- Session-based authentication
- Secure cookies

### Authorization
- Admin-only dashboard access
- Public API endpoints (client registration)
- Protected endpoints (admin functions)

### Device Binding
- License tied to CPU + MAC address
- Prevents license sharing
- Anti-tamper validation

### Network Security
- DHCP/Static IP both supported
- Firewall-friendly (single port)
- Optional HTTPS with reverse proxy

---

## Performance Specifications

### Server Capacity
- **Concurrent Clients:** 2 (Trial) → 999+ (Licensed)
- **Clients per Server:** 100+ (with optimization)
- **Concurrent Connections:** Limited by Orange Pi RAM (~50-100)
- **Database Entries:** 10,000+ sessions before maintenance

### Client Performance
- **Memory Usage:** ~50 MB per client
- **Network Bandwidth:** <1 MB/hour per client
- **CPU Usage:** <1% when idle
- **Update Frequency:** Configurable 0.5-10 seconds

### Database Performance
- **Query Speed:** <100ms average
- **Sync Interval:** 1 second (configurable)
- **Log Retention:** Last 100 entries (configurable)
- **Backup Size:** ~1 MB per 1000 transactions

---

## Deployment Options

### Single Server (Small Cafe)
- 1 Orange Pi server
- 5-20 Windows clients
- All on same LAN
- Simple setup

### Multi-Server (Medium Cafe)
- Multiple Orange Pi servers
- Clients connect to nearest server
- Manual load balancing
- More complex setup

### Enterprise (Large Cafe)
- Dedicated Linux server
- Database cluster (advanced)
- Multiple reverse proxies
- Backup systems
- Advanced monitoring

---

## Backup & Disaster Recovery

### What to Backup
- `/var/lib/pisonet/pisonet.db` - Database
- `/var/lib/pisonet/license.key` - License
- `/opt/pisonet/` - Server files (optional)
- Client configs (Windows)

### Backup Methods
- **Automated:** Daily via cron job
- **Manual:** Use `backup.sh` script
- **Cloud:** Upload to cloud storage
- **External Drive:** Copy to USB/HDD

### Recovery Process
1. Restore database from backup
2. Restart service
3. Verify data integrity
4. Re-activate clients

---

## Troubleshooting Tree

### Server Down
```
├─ Is service running?
│  └─ systemctl status pisonet
├─ Check logs
│  └─ journalctl -u pisonet -f
├─ Network accessible?
│  └─ ping pisonet.local
└─ Port in use?
   └─ lsof -i :5000
```

### Client Won't Connect
```
├─ Is server running?
│  └─ curl pisonet.local:5000
├─ Correct IP configured?
│  └─ Edit client_config.ini
├─ Network accessible?
│  └─ ping pisonet.local
└─ Firewall blocking?
   └─ Allow port 5000
```

### GPIO Not Working
```
├─ GPIO accessible?
│  └─ gpio readall
├─ Correct pin?
│  └─ Pin 3 (coin), Pin 5 (relay)
├─ Wiring correct?
│  └─ Check circuit
└─ Hardware working?
   └─ Test independently
```

---

## Scaling Considerations

### Vertical Scaling (Single Server)
- Upgrade Orange Pi to Pi 4 (2GB RAM)
- Use external SSD for database
- Optimize database queries
- Increase sync interval

### Horizontal Scaling (Multiple Servers)
- Run multiple servers
- Each handles 50-100 clients
- Manual client assignment
- Central backup server

### Cloud Deployment
- Run on virtual server (AWS, Linode, etc.)
- Higher bandwidth than local
- Better uptime SLA
- Monitoring and alerts

---

## Monitoring & Maintenance

### Daily Checks
- Service running: `systemctl status pisonet`
- Disk space: `df -h`
- Error logs: `journalctl -u pisonet -n 20`

### Weekly Tasks
- Check client connectivity
- Review transaction logs
- Verify GPIO operation
- Test backup restoration

### Monthly Maintenance
- Database optimization
- Log file rotation
- Performance review
- Security updates

### Quarterly Reviews
- System upgrades
- Capacity planning
- Hardware inspection
- License renewal check

---

## Feature Comparison

| Feature | Trial | Licensed |
|---------|-------|----------|
| Max Clients | 2 | Unlimited |
| Max Session | 30 min | Unlimited |
| Time Increments | Any | Any |
| Coin Detection | Yes | Yes |
| Relay Control | Yes | Yes |
| Dashboard | Yes | Yes |
| Offline Mode | After activation | Yes |
| License Cost | Free | One-time |
| Support | Community | Priority |

---

## Technology Stack

### Server
- **Language:** Python 3.8+
- **Web Framework:** Flask 3.0
- **Database:** SQLite 3
- **GPIO:** RPi.GPIO
- **API:** REST/JSON

### Client
- **Language:** Python 3.8+
- **GUI Framework:** PyQt5 5.15
- **Network:** Requests library
- **OS:** Windows 7+

### Dashboard
- **Frontend:** HTML5, CSS3, JavaScript
- **Styling:** Inline CSS
- **Updates:** Fetch API
- **Browser:** Any modern browser

### Infrastructure
- **Server OS:** Armbian (Orange Pi) or Raspberry Pi OS
- **Network:** Ethernet or WiFi (DHCP/Static)
- **Time Sync:** NTP

---

## Code Statistics

| File | Lines | Purpose |
|------|-------|---------|
| pisonet_server.py | 800+ | Main server |
| pisonet_client.py | 500+ | Client app |
| dashboard.html | 600+ | Admin interface |
| install.sh | 100+ | Installation |

**Total:** ~2000 lines of production code

---

## Future Enhancements

### Planned Features
- Multi-language support
- Mobile admin app
- Advanced reporting
- Payment integration
- SMS notifications

### Possible Add-ons
- Printer support
- CCTV integration
- Backup systems
- Load balancing
- Cluster support

---

## Support & Resources

### Documentation
- `README.md` - Start here
- `QUICK_START.md` - 5-minute setup
- `docs/INSTALLATION.md` - Complete guide
- `docs/TROUBLESHOOTING.md` - Problem solving
- `docs/API.md` - Developer reference

### Getting Help
1. Check documentation first
2. Review logs: `sudo journalctl -u pisonet -f`
3. Test connectivity: `ping pisonet.local`
4. Check GPIO: `gpio readall`
5. Contact support with logs

### Community
- GitHub Issues for bug reports
- GitHub Discussions for questions
- Email support for commercial licenses

---

**PisoNet v1.0.0**  
**Complete Cybercafe Management System**  
**Production Ready • Licensed • Supported**
