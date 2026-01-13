Below is the same style, same structure, copy-paste–ready .md file for
Frappe + ERPNext v16 on Ubuntu 25.xx (future-proofed, minimal, uv-based).

This is NOT experimental — it simply assumes:
newer Ubuntu base
newer MariaDB
same hard v16 requirements (Python 3.14 + Node 24)
You can save this as:
frappe_erpnext_v16_ubuntu_25.md

# ✅ Frappe + ERPNext v16 Installation Guide
Ubuntu 25.xx (Clean, Future-Proof, v16 Compatible)

---

## 📌 Pre-requisites (v16 – FINAL)

| Component | Required Version |
|---|---:|
| Ubuntu | 25.xx |
| Python | 3.14.x (MANDATORY) |
| Node.js | 24.x (MANDATORY) |
| MariaDB | 10.11+ (default OK) |
| Redis | 6+ |
| Yarn | 1.22.x |
| Bench | via `uv` |
| wkhtmltopdf | Optional (PDFs only) |
| NGINX | Production only |
| cron | Required |

---

## 👤 STEP 0: Create Dedicated User (MANDATORY)
```bash
sudo adduser frappe
sudo usermod -aG sudo frappe
su - frappe
```

🔄 STEP 1: System Update
```
sudo apt update && sudo apt upgrade -y
sudo reboot
```

Login again as frappe.

⚙ STEP 2: Install System Dependencies (ONLY REQUIRED)
```
sudo apt install -y \
  git curl build-essential pkg-config \
  mariadb-server mariadb-client redis-server \
  libffi-dev libssl-dev \
  libjpeg-dev zlib1g-dev liblcms2-dev \
  libtiff-dev libwebp-dev \
  libmysqlclient-dev
```

Enable services:
```
sudo systemctl enable --now mariadb redis-server
```

Verify:
```
redis-server --version
mariadb --version
```


🔐 STEP 3: Secure MariaDB
```
sudo mysql_secure_installation
```

Recommended answers:
`
Switch to unix_socket authentication? → Y
Change root password? → N
Remove anonymous users? → Y
Disallow root login remotely? → Y
Remove test database? → Y
Reload privilege tables? → Y
`


🗄 STEP 4: Create Database User for Frappe (REQUIRED)
```
sudo mariadb
```
```
CREATE USER 'frappe'@'localhost' IDENTIFIED BY 'frappe';
GRANT ALL PRIVILEGES ON *.* TO 'frappe'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```


🐍 STEP 5: Install uv + Python 3.14 (MANDATORY)
```
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc
```
```
uv python install 3.14 --default
python3.14 --version
```

⚠️ Do NOT install:

python3-dev
python3-venv
virtualenv


### uv manages all Python environments.

🟢 STEP 6: Install Node.js 24 (MANDATORY)
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
```
```
nvm install 24
nvm use 24
nvm alias default 24
node -v
```


🧶 STEP 7: Install Yarn (Classic)
```
corepack enable
corepack prepare yarn@1.22.22 --activate
yarn -v
```


🧰 STEP 8: Install Bench + Process Manager
```
uv tool install frappe-bench
uv tool install honcho
export PATH="$HOME/.local/bin:$PATH"
```
```
bench --version
honcho --version
```


🚀 STEP 9: Initialize Frappe v16 Bench
```
bench init frappe-bench --frappe-branch version-16 --python python3.14
cd frappe-bench
```

🌐 STEP 10: Create a Site
```
bench new-site site1.local
```

Enter:
#### MySQL super user → frappe
#### MySQL password → frappe
#### Administrator password → (choose)


📦 STEP 11: Install ERPNext v16
```
bench get-app erpnext --branch version-16
bench --site site1.local install-app erpnext
```

▶ STEP 12: Start Development Server
```
bench start
```


Open:
```
http://localhost:8000
```

🔐 STEP 13: Production Setup (VPS Only)
```
sudo bench setup production frappe
```

Includes:

### NGINX
Supervisor
Redis queues
Cron jobs

🔒 SSL (Let’s Encrypt)
```
sudo apt install -y certbot python3-certbot-nginx
sudo bench setup lets-encrypt site1.local
```


🧩 Optional Apps (v16)
```
bench get-app payments --branch version-16
bench get-app hrms --branch version-16
```
```
bench --site site1.local install-app payments
bench --site site1.local install-app hrms
```

🛡 Firewall (Optional)
```
sudo ufw allow OpenSSH
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

✅ Final Health Check
```
bench doctor
```

### Expected:
Redis ✅
MariaDB ✅
Workers ✅
Scheduler ✅

### 🎯 Notes

wkhtmltopdf is OPTIONAL (install only if PDFs are required)

Do NOT use pip, pipx, or system Python
Python 3.14 + Node 24 are non-negotiable for v16
uv is the long-term supported toolchain


---

### ✅ Final confirmation
- ❌ No legacy steps
- ❌ No pip / pipx
- ❌ No python3-dev / venv
- ✅ Same structure as Ubuntu 24 guide
- ✅ Ready for Ubuntu 25+

If you want next, I can:
- generate **Ubuntu 22 / 24 / 25 comparison table**
- add **WSL-specific notes**
- produce **production-only minimal docs**
- export all three guides into **one combined README**

Just say the word 👍
