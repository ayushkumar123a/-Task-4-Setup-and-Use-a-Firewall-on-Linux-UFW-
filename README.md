# -Task-4-Setup-and-Use-a-Firewall-on-Linux-UFW-
Configure and test basic firewall rules to allow or block traffic using **UFW** (Uncomplicated Firewall) on Kali Linux.
## 🛠 Tools Used
- UFW (Uncomplicated Firewall)
- netcat (`nc`)
- telnet
- rsyslog (for advanced logging)
- systemctl & journalctl

---

## 🧩 What Was Done

### ✅ Basic UFW Setup
```bash
sudo apt install ufw -y
sudo ufw enable
```

### 🔐 Applied Firewall Rules
```bash
sudo ufw deny 23                     # Block Telnet
sudo ufw allow 22                    # Allow SSH
sudo ufw status numbered             # View current rules
sudo ufw delete 1                    # Removed rule to restore state
```

### 🧠 Advanced Customization To Stand Out
```bash
sudo ufw limit ssh                              # Limit SSH login attempts
sudo ufw allow from 192.168.56.0/24 to any port 22  # Restrict SSH access to local subnet
sudo ufw delete allow 22                        # Remove broad SSH allow
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw logging on
```

### 🎯 Created and Allowed Custom App Profile
```bash
sudo nano /etc/ufw/applications.d/custom-http-server
# Added:
# [Custom HTTP Server]
# title=My Web Server
# description=Opens port 8080 for web
# ports=8080/tcp

sudo ufw app update
sudo ufw allow "Custom HTTP Server"
sudo ufw app info "Custom HTTP Server"
```

### 🔍 Auditing and Logging
```bash
sudo apt install rsyslog -y
sudo nano /etc/rsyslog.d/20-ufw.conf

# Inside the file:
:msg,contains,"[UFW " /var/log/ufw.log

sudo systemctl restart rsyslog
sudo ufw logging high
sudo tail -f /var/log/ufw.log         # Confirmed logging works
```

### 🔐 Outbound Restrictions (Very Advanced)
```bash
sudo ufw default deny outgoing
sudo ufw allow out 53                 # DNS
sudo ufw allow out 80                 # HTTP
sudo ufw allow out 443                # HTTPS
sudo ufw allow out 22                 # SSH
```

---

## 💥 What Makes This Stand Out

- ✅ Went beyond basic allow/deny by using:
  - Network-specific access controls
  - Rate-limiting SSH
  - Custom service profiles
  - Outbound traffic policies
  - Logging setup using `rsyslog`
- 🧠 All logs confirmed via `/var/log/ufw.log` and `journalctl`
