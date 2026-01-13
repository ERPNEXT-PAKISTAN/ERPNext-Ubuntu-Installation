# ✅ Frappe + ERPNext v16 Installation Guide
Ubuntu 24.04 LTS (Verified & Fixed)

---

## 📌 Pre-requisites (UPDATED for v16)

| Component | Required Version |
|---|---:|
| Ubuntu | 24.04 LTS |
| Python | 3.14.x (REQUIRED) |
| Node.js | 24.x (REQUIRED) |
| MariaDB | 10.6 – 10.11 (10.11 default OK) |
| Redis | 6+ |
| Yarn | 1.22+ |
| pip | auto (inside venv) |
| wkhtmltopdf | optional (PDFs) |
| NGINX | required for production |
| cron | required |

---

## 🚀 STEP 0: Create a Dedicated User (MANDATORY)
```bash
sudo adduser frappe
sudo usermod -aG sudo frappe
su - frappe
```

---

## ⚙ STEP 1: System Update
```bash
sudo apt update && sudo apt upgrade -y
sudo reboot
```
Login again as `frappe`.

---

## ⚙ STEP 2: Install System Dependencies
```bash
sudo apt install -y \
  git curl build-essential pkg-config \
  python3-dev python3-pip python3-venv \
  redis-server mariadb-server \
  libffi-dev libssl-dev \
  libjpeg-dev zlib1g-dev \
  liblcms2-dev libtiff-dev libwebp-dev \
  libharfbuzz-dev libfribidi-dev \
  libmysqlclient-dev \
  xvfb libfontconfig wkhtmltopdf
```

Enable Redis & MariaDB:
```bash
sudo systemctl enable --now redis-server mariadb
```

Verify Redis:
```bash
redis-cli ping   # should return PONG
```

---

## ⚙ STEP 3: Secure MariaDB
```bash
sudo mysql_secure_installation
```

Recommended answers:
```
Switch to unix_socket authentication? Y
Change root password? Y
Remove anonymous users? Y
Disallow root login remotely? N
Remove test database? Y
Reload privilege tables? Y
```

---

## ⚙ STEP 4: Configure MariaDB (REQUIRED)
Edit the server config:
```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Add under `[mysqld]`:
```ini
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
innodb-file-per-table = 1
innodb-file-format = barracuda
```

Restart MariaDB:
```bash
sudo systemctl restart mariadb
```

---

## ⚙ STEP 5: Install Node.js 24 (via NVM)
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc

nvm install 24
nvm use 24
nvm alias default 24

node -v   # must be v24.x
```

---

## ⚙ STEP 6: Install Yarn
```bash
npm install -g yarn
yarn -v
```

---

## ⚙ STEP 7: Install uv and Python 3.14 (REQUIRED)
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc

uv python install 3.14
uv python find 3.14
```

You should see a path like:
```
~/.local/share/uv/python/.../python3.14
```

---

## ⚙ STEP 8: Install Frappe Bench (Recommended via pipx)
```bash
sudo apt install -y pipx
pipx ensurepath
source ~/.bashrc

pipx install frappe-bench
bench --version
```

---

## ⚙ STEP 9: Initialize Bench (Frappe v16)
```bash
PY314="$(uv python find 3.14)"
bench init frappe-bench --frappe-branch version-16 --python "$PY314"
cd frappe-bench
```

This will:
- create its own venv
- use Python 3.14
- install Frappe v16 correctly

---

## ⚙ STEP 10: Create a Site
```bash
bench new-site site1.local
```

When asked:
- Enter mysql super user [root]: (press ENTER)
- MySQL root password: (enter MariaDB root password)
- Administrator password: (set ERPNext admin password)

Then:
```bash
bench --site site1.local add-to-hosts
bench use site1.local
```

---

## ⚙ STEP 11: Install ERPNext v16
```bash
bench get-app erpnext --branch version-16
bench --site site1.local install-app erpnext
```

---

## ⚙ STEP 12 (Optional): Install HRMS & Payments (v16)
```bash
bench get-app payments --branch version-16
bench get-app hrms --branch version-16

bench --site site1.local install-app payments
bench --site site1.local install-app hrms
```

---

## ⚙ STEP 13: Start Development Server
```bash
bench start
```

Open browser:
```
http://YOUR_VPS_IP:8000
```

Login:
- User: Administrator
- Password: (the one you set)

---

## 🚀 STEP 14: Production Setup (Recommended for VPS)
```bash
sudo bench setup production frappe
```

This configures:
- NGINX
- Supervisor
- Redis queues
- Cron jobs

---

## ✅ FINAL COMPATIBILITY SUMMARY

| Item | Status |
|---|---:|
| Ubuntu 24.04 | ✅ |
| Frappe v16 | ✅ |
| ERPNext v16 | ✅ |
| Python 3.14 | ✅ REQUIRED |
| Node.js 24 | ✅ REQUIRED |
| MariaDB 10.11 | ✅ |
| Redis | ✅ |

---

## 🔐 SSL (Let’s Encrypt) for ERPNext v16

Prerequisites
- Domain name pointed to your VPS IP (example: erp.example.com)
- ERPNext already in production mode

STEP 1: Setup production (if not done)
```bash
cd ~/frappe-bench
sudo bench setup production frappe
```

STEP 2: Install Certbot
```bash
sudo apt install -y certbot python3-certbot-nginx
```

STEP 3: Enable SSL for your site
```bash
sudo bench setup lets-encrypt site1.local
```

When prompted:
- Email → your email
- Agree → Y
- Redirect HTTP → Y

✅ This will:
- Issue SSL
- Auto-renew
- Update NGINX config

STEP 4: Verify auto-renew
```bash
sudo certbot renew --dry-run
```

STEP 5: Open in browser
https://erp.example.com

---

## ⚡ Performance Tuning (Contabo VPS Optimized)

These settings are safe defaults for:
- 4–8 GB RAM VPS
- ERPNext v16 production workloads

1️⃣ Enable Swap (VERY IMPORTANT on Contabo)
Create 4GB swap:
```bash
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```

Persist:
```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

Verify:
```bash
free -h
```

2️⃣ Tune Linux kernel (OOM prevention)
```bash
sudo nano /etc/sysctl.conf
```
Add:
```
vm.swappiness=10
vm.vfs_cache_pressure=50
```
Apply:
```bash
sudo sysctl -p
```

3️⃣ MariaDB Performance Tuning
Edit:
```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
Add (adjust based on RAM):

For 4 GB RAM
```
innodb_buffer_pool_size = 1G
innodb_log_file_size = 256M
max_connections = 200
```

For 8 GB RAM
```
innodb_buffer_pool_size = 2G
innodb_log_file_size = 512M
max_connections = 300
```

Restart:
```bash
sudo systemctl restart mariadb
```

4️⃣ Supervisor Workers (Critical for ERPNext)
Edit:
```bash
nano ~/frappe-bench/config/supervisor.conf
```
Set workers (example for 4–8 GB):
```
numprocs=2
```
Then:
```bash
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl restart all
```

5️⃣ Disable dev tools in production
```bash
bench set-config developer_mode 0
bench clear-cache
bench restart
```

---

## 🧩 Multi-Site Setup (ERPNext v16)

ERPNext supports multiple domains / companies / clients on one server.

STEP 1: Create another site
```bash
cd ~/frappe-bench
bench new-site site2.example.com
```

STEP 2: Install ERPNext on new site
```bash
bench --site site2.example.com install-app erpnext
```

(Optional apps)
```bash
bench --site site2.example.com install-app hrms
bench --site site2.example.com install-app payments
```

STEP 3: Assign domain
```bash
bench setup add-domain site2.example.com
```

STEP 4: Enable SSL for new site
```bash
sudo bench setup lets-encrypt site2.example.com
```

STEP 5: List all sites
```bash
bench list-sites
```

STEP 6: Switch between sites
```bash
bench use site1.local
bench use site2.example.com
```

---

## 🛡 Recommended Security Hardening (Optional but Smart)
```bash
sudo ufw allow OpenSSH
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

Disable MariaDB remote access (already default):
```bash
sudo ss -lntp | grep 3306
```

---

## ✅ FINAL STATUS CHECK
```bash
bench doctor
```

You should see:
- Redis ✅
- MariaDB ✅
- Workers ✅
- Scheduler ✅
- NGINX ✅

---

## 🎯 Recommended Contabo VPS Sizes

| VPS | Recommended |
|---|---:|
| 4 GB RAM | Small business |
| 8 GB RAM | 10–30 users |
| 16 GB RAM | Heavy HRMS / Accounting |

---

## Notes & tips
- wkhtmltopdf is optional but required for PDF generation. Use a statically linked build compatible with your distro if needed.
- For production, ensure proper firewall settings, TLS (Let's Encrypt), and secure MariaDB credentials.
- If you need systemd or supervisor customization, provide details of your VPS provider and I'll add examples.
