# ✅ PisoNet Deployment Checklist

Use this checklist to ensure your PisoNet system is properly deployed.

---

## Pre-Deployment

### Hardware Preparation
- [ ] Orange Pi One or Raspberry Pi obtained
- [ ] MicroSD card (8GB+) installed with OS
- [ ] Orange Pi connected to network (WiFi or Ethernet)
- [ ] GPIO coin detector wired correctly (Pin 3)
- [ ] Relay module wired correctly (Pin 5)
- [ ] Power supply connected and stable

### Network Preparation
- [ ] Router accessible
- [ ] Static IP or DHCP reservation configured
- [ ] Hostname resolved (ping pisonet.local)
- [ ] Port 5000 accessible on network
- [ ] All client machines connected to same network

### Software Preparation
- [ ] All files downloaded/copied
- [ ] Git repository cloned or ZIP extracted
- [ ] Python 3.8+ available on all machines
- [ ] Windows clients have Python installed

---

## Server Deployment

### Step 1: Orange Pi Setup

#### Hardware Check
- [ ] Orange Pi boots successfully
- [ ] Network connection working
- [ ] Can SSH into Orange Pi
- [ ] GPIO pins accessible

**Command to verify:**
```bash
ssh root@pisonet.local
gpio readall
```

#### Software Installation

**Option A: Automated Installation**
```bash
curl -O https://raw.githubusercontent.com/yourusername/pisonet/main/server/install.sh
chmod +x install.sh
sudo ./install.sh
```
- [ ] Installation script completes without errors
- [ ] All dependencies installed

**Option B: Manual Installation**
- [ ] Python 3 installed: `python3 --version`
- [ ] pip3 installed: `pip3 --version`
- [ ] Flask installed: `pip3 install Flask Flask-CORS RPi.GPIO`
- [ ] Directories created: `/opt/pisonet`, `/var/lib/pisonet`
- [ ] Files copied to correct locations
- [ ] Service file installed: `/etc/systemd/system/pisonet.service`
- [ ] Service enabled: `sudo systemctl enable pisonet`

#### Verification
```bash
sudo systemctl status pisonet
# Should show: active (running)

curl http://localhost:5000/login
# Should return HTML login page

sudo journalctl -u pisonet -n 5
# Should show: "Server starting", etc.
```

- [ ] Server starts without errors
- [ ] Service is running
- [ ] Can access login page from browser
- [ ] No errors in systemd logs

### Step 2: Configuration

#### Server Configuration
- [ ] Hostname set: `pisonet.local` or custom
- [ ] mDNS resolves correctly
- [ ] Database location correct: `/var/lib/pisonet/pisonet.db`
- [ ] Log location correct: `/var/log/pisonet/server.log`
- [ ] GPIO pins match hardware: Pin 3 (coin), Pin 5 (relay)

#### Security
- [ ] Admin password changed from default
- [ ] Firewall configured to allow port 5000
- [ ] SSH uses non-default port (optional)
- [ ] Root password changed

**Command to change password:**
```bash
# Edit /opt/pisonet/pisonet_server.py
# Change: ADMIN_PASSWORD = 'your-new-password'
sudo systemctl restart pisonet
```

- [ ] Admin password changed
- [ ] Change reflected in login

### Step 3: Verification

#### Dashboard Access
- [ ] Open browser: `http://pisonet.local:5000`
- [ ] Login page appears
- [ ] Can login with admin credentials
- [ ] Dashboard appears with no errors

#### API Endpoints
```bash
curl http://pisonet.local:5000/api/server/info
# Should return JSON with server info
```

- [ ] `/api/server/info` returns data
- [ ] `/api/clients` returns empty list (no clients yet)
- [ ] `/api/license/status` returns license info

#### GPIO Verification (if GPIO available)
```bash
curl http://pisonet.local:5000/api/gpio/status
# Should return GPIO pin status
```

- [ ] GPIO endpoint accessible
- [ ] Pin status shows correctly
- [ ] Relay can be toggled via API

---

## Client Deployment

### Per Workstation Installation

#### Step 1: Python Setup

**Windows:**
- [ ] Python 3.8+ installed
- [ ] Python added to PATH
- [ ] Verify: `python --version`

**Verification:**
```bash
python --version
# Should show: Python 3.x.x
```

#### Step 2: Client Installation

**Option A: Automated (Easiest)**
1. [ ] Download `oneclick_client.bat`
2. [ ] Right-click → Run as Administrator
3. [ ] Follow prompts
4. [ ] Installation completes
5. [ ] Client shortcut appears on desktop

**Option B: Manual Installation**
```bash
pip install PyQt5==5.15.9 requests==2.31.0
mkdir %APPDATA%\PisoNet
copy pisonet_client.py %APPDATA%\PisoNet\
copy client_config.ini %APPDATA%\PisoNet\
```
- [ ] PyQt5 installed
- [ ] Requests library installed
- [ ] Directories created
- [ ] Files copied

#### Step 3: Configuration

**Edit Client Config:**
`C:\Users\YourName\AppData\Roaming\PisoNet\client_config.ini`

- [ ] `server_ip` set to correct IP (or hostname)
- [ ] `server_port` set to 5000 (or custom)
- [ ] `client_name` set to meaningful name
- [ ] `check_interval` appropriate for network
- [ ] `timer_x` and `timer_y` set to desired position
- [ ] `transparency` set for readability

**Example config:**
```ini
[Server]
server_ip = 192.168.1.100
server_port = 5000
server_hostname = pisonet.local

[Client]
client_name = Workstation-1

[UI]
timer_x = 10
timer_y = 10
```

#### Step 4: Testing

**Manual Start:**
```bash
python %APPDATA%\PisoNet\pisonet_client.py
```

- [ ] Client starts without errors
- [ ] Timer window appears
- [ ] Status shows "🟢 Active" or "⚪ Idle"
- [ ] Server connection message appears

**Check Logs:**
```bash
type %APPDATA%\PisoNet\client.log
```

- [ ] Logs show successful registration
- [ ] Logs show successful sync with server

#### Step 5: Server Verification

**On Server Dashboard:**
```
http://pisonet.local:5000/dashboard
```

- [ ] Client appears in "Connected Clients" list
- [ ] Client name matches configuration
- [ ] Client status shows correctly
- [ ] IP address shows correctly

#### Step 6: Automation

- [ ] Client starts automatically on user login
- [ ] Create shortcut in startup folder (optional)
- [ ] Test reboot: Computer restarts, client runs

---

## Network Testing

### Connectivity

#### From Client Machine
```bash
ping pisonet.local
# Should ping successfully

ping 192.168.1.100
# Replace with your server IP

curl http://pisonet.local:5000/api/server/info
# Should return JSON
```

- [ ] Can ping server by hostname
- [ ] Can ping server by IP
- [ ] Can access server API

#### Firewall Configuration

**Windows Firewall:**
- [ ] Port 5000 allowed for Python
- [ ] If firewalled, add exception

**Linux Firewall (if using ufw):**
```bash
sudo ufw allow 5000/tcp
```
- [ ] Port 5000 allowed in firewall rules

#### Network Resilience
- [ ] Clients reconnect after brief disconnection
- [ ] Server handles multiple simultaneous clients
- [ ] No connection drops during normal operation

---

## Hardware Testing

### Coin Detector (GPIO Pin 3)

```bash
gpio mode 3 in
gpio read 3
# Insert coin - should change from 0 to 1
```

- [ ] Can read coin pin status
- [ ] Coin insertion changes status
- [ ] Server logs show coin detection
- [ ] Multiple consecutive coins detected

**Server Log Check:**
```bash
sudo journalctl -u pisonet -f | grep -i coin
# Should show coin detection events
```

### Relay (GPIO Pin 5)

```bash
gpio mode 5 out
gpio write 5 1  # ON - relay should click
sleep 2
gpio write 5 0  # OFF - relay should click back
```

- [ ] Relay can be turned on
- [ ] Relay can be turned off
- [ ] Audible click sound when toggling
- [ ] Relay responds quickly

**From Dashboard:**
- [ ] Can control relay from GPIO section
- [ ] Status updates correctly
- [ ] Relay physical feedback matches status

---

## License Activation

### Trial Mode Verification
- [ ] Server starts in trial mode
- [ ] Dashboard shows "⏱️ Trial Mode"
- [ ] Max 2 clients enforced
- [ ] Max 30 minutes per session enforced

### Online Activation

**Requirements:**
- [ ] Orange Pi has internet connection
- [ ] Port 443 (HTTPS) accessible
- [ ] DNS working: `ping 8.8.8.8`

**Activation Process:**
1. [ ] Open dashboard: `http://pisonet.local:5000`
2. [ ] Go to License tab
3. [ ] Click "Activate Online"
4. [ ] Device fingerprint shown
5. [ ] Activation succeeds
6. [ ] License file created: `/var/lib/pisonet/license.key`

**Verification:**
```bash
cat /var/lib/pisonet/license.key
# Should show JSON with license info
```

- [ ] License file exists
- [ ] License file contains valid data
- [ ] Dashboard shows "✅ Licensed"

### Offline Operation
- [ ] Disconnect internet
- [ ] Server still runs normally
- [ ] License still valid
- [ ] More than 2 clients can connect (if licensed)

---

## Dashboard Verification

### Login & Security
- [ ] Login page loads
- [ ] Default credentials work
- [ ] Invalid credentials rejected
- [ ] Can logout and login again
- [ ] Session maintained properly

### Dashboard Features
- [ ] Client list displays
- [ ] Real-time updates working
- [ ] Client status updates
- [ ] Can add time to client
- [ ] Time appears in client timer

### Logs Section
- [ ] Logs tab accessible
- [ ] Logs display correctly
- [ ] Can filter logs
- [ ] Can download logs
- [ ] Auto-refresh working

### License Section
- [ ] License status displays
- [ ] Device fingerprint shown
- [ ] Activation button visible
- [ ] Correct trial/licensed status shown

---

## Performance & Load Testing

### Single Client
- [ ] Timer updates smoothly
- [ ] No lag in UI
- [ ] Server CPU normal (<20%)
- [ ] Memory usage stable
- [ ] Network traffic minimal

### Multiple Clients (5-10)
- [ ] All clients connect successfully
- [ ] Timer still responsive
- [ ] Dashboard still fast
- [ ] No connection drops
- [ ] Server stable

### Load Test (20+ clients)
- [ ] Can connect many clients
- [ ] Server doesn't crash
- [ ] Performance acceptable
- [ ] Database not locked
- [ ] Logs don't fill up disk

---

## Backup & Recovery

### Backup Setup
- [ ] Backup directory exists: `/var/lib/pisonet.backup/`
- [ ] Database backup script working
- [ ] Can backup to external drive
- [ ] Backups timestamped

**Create Backup:**
```bash
sudo cp -r /var/lib/pisonet /var/lib/pisonet.backup
```

- [ ] Backup completes without errors
- [ ] Backup contains all files

### Recovery Testing
- [ ] Can restore from backup
- [ ] Data intact after restore
- [ ] Clients still connect
- [ ] No data corruption

---

## Security Hardening

### Essential
- [ ] Admin password changed
- [ ] Root password changed (Orange Pi)
- [ ] SSH port changed (optional)
- [ ] Firewall restricting access

### Advanced (Optional)
- [ ] HTTPS configured with SSL
- [ ] Nginx reverse proxy installed
- [ ] Client certificates enforced
- [ ] API rate limiting enabled

---

## Documentation

### User Documentation
- [ ] README.md accessible
- [ ] QUICK_START.md provided
- [ ] INSTALLATION.md complete
- [ ] API.md documented

### Admin Documentation
- [ ] CONFIGURATION.md provided
- [ ] TROUBLESHOOTING.md available
- [ ] Hardware setup guides included
- [ ] Password change documented

### Staff Training
- [ ] Admin trained on dashboard
- [ ] Support knows common issues
- [ ] Escalation procedures documented
- [ ] Contact information shared

---

## Go-Live Checklist

### Final Verification
- [ ] All hardware working
- [ ] All software installed
- [ ] All clients registered
- [ ] All tests passing
- [ ] Backups working

### Communication
- [ ] Users notified of launch
- [ ] Support team ready
- [ ] IT team briefed
- [ ] Emergency contact info provided

### Monitoring
- [ ] Server health monitoring active
- [ ] Client connectivity monitoring
- [ ] Database integrity checked
- [ ] Error alerts configured

### First Day
- [ ] Monitor closely for issues
- [ ] Be ready to help users
- [ ] Document any problems
- [ ] Keep backups current

---

## Post-Deployment

### Week 1
- [ ] Monitor for any errors
- [ ] Fix any bugs/issues
- [ ] User feedback collected
- [ ] Performance baseline established

### Month 1
- [ ] Review logs for patterns
- [ ] Check database size
- [ ] Verify backups working
- [ ] Plan any improvements

### Ongoing
- [ ] Daily: Check service status
- [ ] Weekly: Review error logs
- [ ] Monthly: Backup verification
- [ ] Quarterly: System updates

---

## Rollback Plan

If deployment fails:

1. **Stop service:**
   ```bash
   sudo systemctl stop pisonet
   ```

2. **Restore from backup:**
   ```bash
   sudo rm -rf /var/lib/pisonet
   sudo cp -r /var/lib/pisonet.backup /var/lib/pisonet
   ```

3. **Restart service:**
   ```bash
   sudo systemctl start pisonet
   ```

- [ ] Rollback plan documented
- [ ] Backup verified
- [ ] Recovery tested
- [ ] All staff trained

---

## Sign-Off

- [ ] **Deployed By:** _______________
- [ ] **Date:** _______________
- [ ] **Verified By:** _______________
- [ ] **Go-Live Approved:** _______________

---

**PisoNet Version:** 1.0.0  
**Deployment Date:** _______________  
**Support Contact:** _______________
