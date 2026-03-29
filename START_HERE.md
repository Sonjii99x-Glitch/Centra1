# 🪙 PisoNet - START HERE

**Welcome to PisoNet - The Complete Cybercafe Management System!**

This document guides you through what you have and how to use it.

---

## 📚 WHAT YOU HAVE

You have received a **COMPLETE, PRODUCTION-READY** cybercafe management system including:

✅ **Orange Pi Server** - Central control system with GPIO interface  
✅ **Windows Client** - Floating timer app for workstations  
✅ **Web Dashboard** - Real-time admin interface  
✅ **Licensing System** - Device-bound activation  
✅ **Complete Documentation** - 5,900 lines of guides  
✅ **One-Click Installers** - Easy deployment  
✅ **Sample Configurations** - Ready to customize  

---

## 🚀 QUICK START (5 MINUTES)

### For Non-Tech Users:

**Step 1: Orange Pi Server**
1. Open Command Prompt on your computer
2. Connect to Orange Pi: `ssh root@pisonet.local`
3. Run this command:
   ```bash
   curl -O https://raw.githubusercontent.com/yourusername/pisonet/main/server/install.sh && chmod +x install.sh && sudo ./install.sh
   ```
4. Wait 5 minutes for installation

**Step 2: Windows Clients (Per Machine)**
1. Find and right-click `oneclick_client.bat`
2. Select "Run as Administrator"
3. Click Next on all screens
4. Done! Timer appears automatically

**Step 3: Activate License**
1. Open browser: `http://pisonet.local:5000`
2. Login: admin / admin123
3. Click License → Activate Online
4. Done!

**That's it! Your cybercafe is ready.**

---

## 📖 DOCUMENTATION ROADMAP

Read these in order:

### 1️⃣ **README.md** (15 min read)
   - Overview of what PisoNet is
   - Features and capabilities
   - Default credentials
   - Basic troubleshooting

### 2️⃣ **QUICK_START.md** (5 min read)
   - Step-by-step 5-minute setup
   - File locations
   - If something goes wrong

### 3️⃣ **SYSTEM_OVERVIEW.md** (10 min read)
   - How the system works
   - Architecture and components
   - Technology used
   - Performance specs

### 4️⃣ **docs/INSTALLATION.md** (20 min read)
   - Detailed installation for each component
   - Network setup
   - Hardware setup
   - Troubleshooting

### 5️⃣ **docs/CONFIGURATION.md** (15 min read)
   - How to customize settings
   - Server configuration
   - Client configuration
   - Examples for different setups

### 6️⃣ **docs/TROUBLESHOOTING.md** (As needed)
   - Common problems and solutions
   - Diagnostic commands
   - Where to find logs
   - When to contact support

### 7️⃣ **docs/API.md** (For developers)
   - REST API endpoints
   - Example API calls
   - Database schema
   - Integration guide

---

## 📁 WHERE TO FIND EVERYTHING

### **Server Files** (Orange Pi)
```
server/
├── pisonet_server.py    ← Main application
├── requirements.txt     ← Dependencies
├── install.sh          ← Installation script
├── install_opi.sh      ← Orange Pi setup
├── pisonet.service     ← Service file
└── templates/          ← Web pages
```

### **Client Files** (Windows)
```
client/
├── pisonet_client.py     ← Client application
├── requirements.txt      ← Dependencies
├── oneclick_client.bat   ← Windows installer
└── client_config.ini     ← Configuration template
```

### **Installers**
```
installer/
├── oneclick_master.bat      ← Master menu
├── oneclick_client.bat      ← Client installer
├── oneclick_installer.py    ← Python installer
└── install_opi.sh          ← Server installer
```

### **Documentation**
```
docs/
├── INSTALLATION.md      ← Complete install guide
├── CONFIGURATION.md     ← Settings reference
├── TROUBLESHOOTING.md   ← Problem solving
├── API.md              ← Developer reference
└── hardware/           ← Hardware guides
```

### **Main Documents**
```
├── README.md               ← What is PisoNet?
├── QUICK_START.md         ← 5-minute setup
├── START_HERE.md          ← This file
├── SYSTEM_OVERVIEW.md     ← How it works
├── FOLDER_STRUCTURE.md    ← File layout
├── DEPLOYMENT_CHECKLIST.md ← Go-live guide
└── DELIVERABLES.md        ← Complete inventory
```

---

## ✅ INSTALLATION CHECKLIST

### Before You Start
- [ ] Orange Pi One or Raspberry Pi ready
- [ ] MicroSD card with OS installed
- [ ] Connected to network (WiFi or Ethernet)
- [ ] Windows PC(s) for clients
- [ ] Python 3.8+ installed on Windows
- [ ] Internet connection for license activation

### Server Installation (30 minutes)
- [ ] SSH into Orange Pi: `ssh root@pisonet.local`
- [ ] Run install script (see QUICK_START.md)
- [ ] Verify: `sudo systemctl status pisonet`
- [ ] Access: Open browser to `http://pisonet.local:5000`
- [ ] Login with: admin / admin123

### Client Installation (5 minutes per machine)
- [ ] Run `oneclick_client.bat` as Administrator
- [ ] Edit configuration if needed
- [ ] Verify timer appears on screen
- [ ] Check that client shows in dashboard

### Hardware Setup (Optional but recommended)
- [ ] Wire coin detector to GPIO pin 3
- [ ] Wire relay to GPIO pin 5
- [ ] Test coin detection
- [ ] Test relay activation

### License Activation (1 minute)
- [ ] Open `http://pisonet.local:5000/license`
- [ ] Click "Activate Online"
- [ ] Confirm activation
- [ ] Verify unlimited features

---

## 🎯 WHAT TO DO NEXT

### **Immediately After Setup:**
1. Change admin password (for security)
2. Configure client settings
3. Test coin detector and relay
4. Create database backup

### **First Week:**
1. Adjust timer positions on each workstation
2. Test all clients connecting simultaneously
3. Verify coin detection works
4. Monitor logs for any errors

### **Ongoing:**
1. Monitor system daily
2. Back up database weekly
3. Check logs weekly
4. Update system monthly

---

## 🔧 COMMON TASKS

### Find Server IP Address
```bash
ping pisonet.local
```
or
```
Advanced IP Scanner (Windows) → Look for "pisonet"
```

### Change Admin Password
1. SSH to Orange Pi: `ssh root@pisonet.local`
2. Edit `/opt/pisonet/pisonet_server.py`
3. Find: `ADMIN_PASSWORD = 'admin123'`
4. Change to: `ADMIN_PASSWORD = 'your-new-password'`
5. Restart: `sudo systemctl restart pisonet`

### View Server Logs
```bash
sudo journalctl -u pisonet -f
```

### Backup Database
```bash
sudo cp /var/lib/pisonet/pisonet.db /home/pi/pisonet.db.backup
```

### Add Time to Client
1. Open dashboard: `http://pisonet.local:5000`
2. Find client in list
3. Click [+1min] or [+30min]
4. Done!

### Control Relay from Dashboard
1. Open dashboard
2. Look for "Relay Control" section
3. Click "Turn On" or "Turn Off"
4. Relay will activate/deactivate

---

## 🚨 IF SOMETHING GOES WRONG

### Server Won't Start
```bash
sudo systemctl status pisonet
sudo journalctl -u pisonet -n 20
```
→ See docs/TROUBLESHOOTING.md

### Client Won't Connect
```bash
ping pisonet.local
curl http://pisonet.local:5000/api/server/info
```
→ Check client_config.ini IP address

### Coin Detector Not Working
```bash
gpio readall
gpio read 3
```
→ Check wiring, see docs/hardware/

### Help!
1. Check `docs/TROUBLESHOOTING.md`
2. Search for your error in logs
3. Run diagnostic commands
4. Contact support with logs

---

## 📊 SYSTEM REQUIREMENTS

### Minimum Hardware
- **Server:** Orange Pi One (1GB RAM)
- **Network:** 100 Mbps Ethernet or WiFi
- **Client:** Windows 7 or later

### Recommended Hardware
- **Server:** Raspberry Pi 4 (2GB+ RAM)
- **Network:** 1 Gbps Ethernet
- **Client:** Windows 10 or later

### Network Requirements
- Router with DHCP
- mDNS support (usually default)
- Port 5000 accessible

---

## 🔐 SECURITY REMINDER

### IMPORTANT: Change These Immediately
- [ ] Admin password (default: admin123)
- [ ] Orange Pi root password
- [ ] Any SSH keys

### Best Practices
- Keep system updated: `sudo apt-get update && sudo apt-get upgrade`
- Monitor logs regularly
- Back up database daily
- Restrict network access to port 5000

---

## 📞 GETTING HELP

### Documentation
1. **Quick questions?** → README.md
2. **Installation help?** → docs/INSTALLATION.md
3. **Setup problems?** → docs/TROUBLESHOOTING.md
4. **Configuration?** → docs/CONFIGURATION.md
5. **Hardware?** → docs/hardware/*
6. **Development?** → docs/API.md

### Diagnostic Tools
- Logs: `sudo journalctl -u pisonet -f`
- Network: `ping pisonet.local`
- API: `curl http://pisonet.local:5000/api/server/info`
- GPIO: `gpio readall`

### Contact Support
Provide:
- Server logs (last 100 lines)
- Client logs (if applicable)
- What you were doing
- What went wrong
- Steps to reproduce

---

## 🎉 SUCCESS INDICATORS

You'll know everything is working when:

✅ Server accessible at `http://pisonet.local:5000`  
✅ Can login with admin credentials  
✅ Dashboard shows "0 clients" initially  
✅ Clients appear in dashboard when they connect  
✅ Timer updates in real-time on clients  
✅ Can add time from dashboard  
✅ Coin detector shows status in GPIO section  
✅ Can control relay from GPIO section  
✅ License shows "Licensed" or "Trial Mode"  

---

## 📈 NEXT STEPS

### For Owners/Managers
1. Learn the dashboard (30 minutes)
2. Set up user accounts if needed
3. Configure pricing/time increments
4. Train staff on managing

### For IT/Technical
1. Review SYSTEM_OVERVIEW.md
2. Study docs/CONFIGURATION.md
3. Read docs/API.md
4. Plan backup strategy

### For Users/Customers
1. Just use the timer
2. Click buttons to add time
3. Report issues to staff

---

## 🎓 TRAINING RESOURCES

### For Admins
- Dashboard tour: 30 minutes
- Licensing guide: 15 minutes
- Troubleshooting: 1 hour

### For IT Staff
- System overview: 1 hour
- Installation procedure: 2 hours
- Backup/Recovery: 1 hour
- Monitoring: 30 minutes

### For Support
- Troubleshooting guide: 2 hours
- Common issues: 30 minutes
- Escalation procedures: 30 minutes

---

## 💡 PRO TIPS

1. **Client Position:** Adjust timer_x and timer_y in config for each workstation
2. **Backup:** Create daily backup of `/var/lib/pisonet/pisonet.db`
3. **Monitoring:** Check logs daily for errors
4. **Performance:** Reduce check_interval on slow networks
5. **Security:** Change admin password immediately
6. **Updates:** Check for system updates monthly
7. **Testing:** Test new configurations on one client first
8. **Documentation:** Keep records of changes made

---

## 🔄 TYPICAL WORKFLOW

### Daily
1. Turn on Orange Pi (boots automatically)
2. Clients connect automatically
3. Monitor dashboard throughout day
4. Add time as customers request

### Weekly
1. Check server logs for errors
2. Verify all systems operational
3. Review transactions
4. Back up database

### Monthly
1. Review performance statistics
2. Plan any upgrades
3. Check for system updates
4. Audit customer accounts

### Quarterly
1. Major updates
2. Security review
3. Capacity planning
4. License renewal

---

## ✨ YOU'RE ALL SET!

You have **EVERYTHING** needed for a complete, professional cybercafe system.

1. **Read:** START_HERE.md (you're here!) ✓
2. **Install:** Follow QUICK_START.md
3. **Configure:** Reference docs/CONFIGURATION.md
4. **Operate:** Use web dashboard daily
5. **Support:** Reference docs/TROUBLESHOOTING.md as needed

**Questions?** Check the appropriate documentation file above.

---

## 📚 QUICK REFERENCE

| Need | File | Read Time |
|------|------|-----------|
| Overview | README.md | 15 min |
| Quick Setup | QUICK_START.md | 5 min |
| How It Works | SYSTEM_OVERVIEW.md | 10 min |
| Installation | docs/INSTALLATION.md | 20 min |
| Settings | docs/CONFIGURATION.md | 15 min |
| Problems | docs/TROUBLESHOOTING.md | As needed |
| API/Dev | docs/API.md | 20 min |
| Go Live | DEPLOYMENT_CHECKLIST.md | 30 min |

---

**Welcome to PisoNet!**

**🎉 Your cybercafe management system is ready to go!**

**Questions? Start with README.md or docs/TROUBLESHOOTING.md**

---

*PisoNet v1.0.0 - Complete Cybercafe Management System*  
*Production Ready • Fully Documented • One-Click Installers*  
*Ready for immediate deployment!*
