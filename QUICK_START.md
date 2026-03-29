# 🚀 PisoNet Quick Start Guide

**Get your cybercafe running in 5 minutes!**

---

## Step 1: Orange Pi Server (5 minutes)

### For Beginners (Use SSH)

1. **Connect Orange Pi to network** (WiFi or Ethernet)

2. **Find its IP address:**
   - Plug monitor into Orange Pi
   - Look for IP in terminal, or
   - Use IP scanner on Windows

3. **Open Command Prompt (Windows):**
   ```bash
   ssh root@192.168.x.x
   # Password: 1234
   ```

4. **Copy-paste this ONE command:**
   ```bash
   curl -O https://raw.githubusercontent.com/yourusername/pisonet/main/server/install.sh && chmod +x install.sh && sudo ./install.sh
   ```

5. **Wait 5 minutes** for installation to complete

6. **Server is ready!** Access at: `http://pisonet.local:5000`

---

## Step 2: Windows Clients (2 minutes per workstation)

### For Each Computer:

1. **Right-click** `oneclick_client.bat`
2. **Select** "Run as Administrator"
3. **Follow prompts** (just click Next)
4. **Restart computer**

**That's it!** Timer window appears automatically.

---

## Step 3: Activate License (1 minute)

1. **Open browser:** `http://pisonet.local:5000`
2. **Login:** admin / admin123
3. **Go to:** License tab
4. **Click:** "Activate Online"
5. **Done!** System now has unlimited clients

---

## Accessing Your System

| What | Where |
|------|-------|
| **Admin Dashboard** | http://pisonet.local:5000 |
| **Clients List** | Dashboard → see connected workstations |
| **Server Logs** | Dashboard → Logs tab |
| **License Status** | Dashboard → License tab |
| **Client Config** | C:\Users\You\AppData\Roaming\PisoNet |

---

## Default Credentials

```
Username: admin
Password: admin123

⚠️ CHANGE THIS IMMEDIATELY!
```

---

## First-Time Setup Checklist

- [ ] Orange Pi running with internet
- [ ] Clients installed on workstations
- [ ] Dashboard accessible at pisonet.local:5000
- [ ] All clients appear in dashboard
- [ ] License activated (or using trial)
- [ ] Admin password changed
- [ ] Coin detector wired (GPIO pin 3)
- [ ] Relay wired (GPIO pin 5)
- [ ] Test coin: coin inserted → relay clicks

---

## If Something Goes Wrong

### "Can't find server"
```bash
ping pisonet.local
# If this works, check server_ip in client config
# If not, use IP instead: 192.168.1.100
```

### "Server won't start"
```bash
ssh root@192.168.x.x
sudo systemctl status pisonet
# Check error message
```

### "License won't activate"
- Check internet on Orange Pi: `ping 8.8.8.8`
- Check firewall allows HTTPS
- Try again in a minute

### Still stuck?
→ See `docs/TROUBLESHOOTING.md`

---

## Next Steps

### Configuration
- Change admin password
- Set server hostname in client config
- Adjust timer position and size

### Customization
- Edit client names in config
- Set different refresh intervals
- Configure GPIO pins if using different ones

### Advanced
- Setup HTTPS with reverse proxy
- Configure backup schedule
- Monitor system logs

→ See `docs/CONFIGURATION.md` for details

---

## File Locations

### On Orange Pi
```
/opt/pisonet/                - Main server
/var/lib/pisonet/            - Database & license
/var/log/pisonet/            - Logs
/etc/systemd/system/pisonet.service - Service file
```

### On Windows Client
```
C:\Users\You\AppData\Roaming\PisoNet\
  ├── client_config.ini      - Configuration
  ├── client.log             - Client logs
  └── pisonet_client.py      - Client script
```

---

## Support

| Problem | Solution |
|---------|----------|
| Client won't connect | Check firewall, ping server |
| Coin not detected | Check GPIO pin 3 wiring |
| Relay won't click | Check GPIO pin 5, power, wiring |
| Dashboard slow | Reduce refresh rate in config |
| License fails | Check internet, disable VPN/proxy |

→ Full guide: `docs/TROUBLESHOOTING.md`

---

## Command Reference

### Server Commands

```bash
# Start/Stop/Status
sudo systemctl start pisonet
sudo systemctl stop pisonet
sudo systemctl status pisonet

# View Logs
sudo journalctl -u pisonet -f
sudo journalctl -u pisonet -n 100

# Restart
sudo systemctl restart pisonet

# Reset Password
sudo nano /opt/pisonet/pisonet_server.py
# Edit: ADMIN_PASSWORD = 'newpass'
sudo systemctl restart pisonet
```

### Client Commands

```bash
# Start Client
python %APPDATA%\PisoNet\pisonet_client.py

# View Logs
type %APPDATA%\PisoNet\client.log

# Reinstall
pip install --force-reinstall PyQt5 requests
```

### GPIO Test

```bash
# Check GPIO
gpio readall

# Test Coin Input (Pin 3)
gpio mode 3 in
gpio read 3

# Test Relay (Pin 5)
gpio mode 5 out
gpio write 5 1
sleep 2
gpio write 5 0
```

---

## API Quick Test

```bash
# Get server info
curl http://pisonet.local:5000/api/server/info

# List all clients
curl http://pisonet.local:5000/api/clients

# Add 1 hour to client
curl -X POST http://pisonet.local:5000/api/client/CLIENT-ID/add-time \
  -H "Content-Type: application/json" \
  -d '{"minutes": 60}'

# Activate relay
curl -X POST http://pisonet.local:5000/api/gpio/relay \
  -H "Content-Type: application/json" \
  -d '{"action": "on"}'
```

---

## Performance Tips

**For Large Networks (50+ clients):**
1. Increase client sync interval: `check_interval = 5`
2. Reduce dashboard refresh: Change 2000 to 5000ms
3. Increase log retention: Keep only 50 lines
4. Use static IP instead of mDNS

**For Slow Networks:**
1. Use IP address instead of hostname
2. Increase timeouts in config
3. Reduce check frequency

**For High Traffic:**
1. Add more RAM to Orange Pi
2. Use external SSD for database
3. Run multiple servers if >100 clients

---

## Maintenance

### Weekly
- Check logs for errors
- Verify clients connecting
- Test coin and relay

### Monthly
- Backup database: `/var/lib/pisonet/pisonet.db`
- Review transactions
- Check disk space: `df -h`

### Quarterly
- Update system: `sudo apt-get update && sudo apt-get upgrade`
- Review security settings
- Test disaster recovery

---

## FAQ

**Q: Can I run multiple servers?**
A: Yes! Each needs different hostname/port

**Q: How many clients supported?**
A: Trial: 2, Licensed: 999+

**Q: Can clients work offline?**
A: No, they need connection to server (which works offline)

**Q: How do I back up licenses?**
A: Copy `/var/lib/pisonet/license.key` to safe location

**Q: Can I monitor from my phone?**
A: Yes, open http://pisonet.local:5000 in mobile browser

---

## Common Mistakes

❌ **Don't:** Run server on Windows (use WSL2 or VM)
✅ **Do:** Run on Orange Pi or Raspberry Pi

❌ **Don't:** Skip changing admin password
✅ **Do:** Change immediately after first login

❌ **Don't:** Forget to wire GPIO pins
✅ **Do:** Test coin detector and relay before opening

❌ **Don't:** Ignore firewall warnings
✅ **Do:** Allow port 5000 through firewall

❌ **Don't:** Move license file
✅ **Do:** Keep it in `/var/lib/pisonet/`

---

**🎉 Your PisoNet is ready! Welcome to modern cybercafe management!**

Questions? See `README.md` or `docs/` folder.
