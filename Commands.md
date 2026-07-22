---

# Installation

## Clone Repository

```bash
git clone https://github.com/leeyuseung1377/Active-Deception.git
cd Active-Deception
```

---

## Install Dependencies

### Ubuntu

```bash
sudo apt update
sudo apt install -y nginx suricata sqlite3 python3 python3-pip
```

---

## Verify Installation

```bash
nginx -v
```

```bash
suricata --build-info
```

```bash
python3 --version
```

```bash
sqlite3 --version
```

---

# Directory Structure

```text
/opt/deception/
├── controller/
│   ├── controller.py
│   ├── sync_nginx_risky_ips.py
│   └── risky_ips.json
│
├── database/
│   └── deception.db
│
├── logs/
│
└── state/
```

---

# Run Controller

```bash
sudo python3 /opt/deception/controller/controller.py
```

---

# Synchronize Nginx

```bash
sudo python3 /opt/deception/controller/sync_nginx_risky_ips.py
```

---

# Start Nginx

```bash
sudo systemctl start nginx
```

---

# Reload Nginx

```bash
sudo systemctl reload nginx
```

---

# Test Nginx Configuration

```bash
sudo nginx -t
```

---

# Start Suricata

```bash
sudo systemctl start suricata
```

---

# Restart Suricata

```bash
sudo systemctl restart suricata
```

---

# Check Suricata Status

```bash
sudo systemctl status suricata
```

---

# Check Nginx Status

```bash
sudo systemctl status nginx
```

---

# Monitor Suricata Alerts

```bash
sudo tail -f /var/log/suricata/fast.log
```

---

# Monitor eve.json

```bash
sudo tail -f /var/log/suricata/eve.json
```

---

# View Controller Logs

```bash
sudo tail -f /opt/deception/logs/controller.log
```

---

# View Nginx Sync Logs

```bash
sudo tail -f /opt/deception/logs/nginx_sync.log
```

---

# View Database Tables

```bash
sqlite3 /opt/deception/database/deception.db ".tables"
```

---

# View Risk Scores

```bash
sqlite3 -header -column /opt/deception/database/deception.db "SELECT * FROM risk_scores;"
```

---

# View Alert Events

```bash
sqlite3 -header -column /opt/deception/database/deception.db "SELECT * FROM alert_events ORDER BY id DESC LIMIT 20;"
```

---

# Test Real Web Server

```bash
curl http://192.168.85.150
```

---

# Test Honeypot

```bash
curl http://192.168.85.160
```

---

# Test Deception Gateway

```bash
curl http://192.168.85.100
```

---

# Verify Risky IP List

```bash
cat /opt/deception/controller/risky_ips.json
```

---

# Verify Nginx Mapping

```bash
cat /etc/nginx/deception/risky_ips.map
```
