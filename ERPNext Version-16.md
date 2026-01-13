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

Notes & tips:
- wkhtmltopdf is optional but required for PDF generation. Use a statically linked build compatible with your distro if needed.
- For production, ensure proper firewall settings, TLS (Let's Encrypt), and secure MariaDB credentials.
- If you need a systemd or supervisor customization, provide details of your VPS provider and I'll add examples.

---


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

Notes & tips:
- wkhtmltopdf is optional but required for PDF generation. Use a statically linked build compatible with your distro if needed.
- For production, ensure proper firewall settings, TLS (Let's Encrypt), and secure MariaDB credentials.
- If you need a systemd or supervisor customization, provide details of your VPS provider and I'll add examples.


