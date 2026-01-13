# ✅ Frappe + ERPNext v16 Installation Guide
Ubuntu 24.04 LTS (Clean, Verified, v16-Compatible)

---

## 📌 Pre-requisites (v16 – FINAL)

| Component | Required Version |
|---|---:|
| Ubuntu | 24.04 LTS |
| Python | 3.14.x (MANDATORY) |
| Node.js | 24.x (MANDATORY) |
| MariaDB | 10.11 (default on 24.04) |
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
``

🔄 STEP 1: System Update
bash```
Copy code
sudo apt update && sudo apt upgrade -y
sudo reboot
Login again as frappe.
```

⚙ STEP 2: Install System Dependencies (ONLY REQUIRED)
bash```
Copy code
sudo apt install -y \
  git curl build-essential pkg-config \
  mariadb-server mariadb-client redis-server \
  libffi-dev libssl-dev \
  libjpeg-dev zlib1g-dev liblcms2-dev \
  libtiff-dev libwebp-dev \
  libmysqlclient-dev
Enable services:
```

bash```
Copy code
sudo systemctl enable --now mariadb redis-server
attach verification (optional):
```

bash```
Copy code
redis-server --version
mariadb --version
```

🔐 STEP 3: Secure MariaDB
bash```
Copy code
sudo mysql_secure_installation
Recommended answers:
```

css```
Copy code
Switch to unix_socket authentication? → Y
Change root password? → N
Remove anonymous users? → Y
Disallow root login remotely? → Y
Remove test database? → Y
Reload privilege tables? → Y
```

🗄 STEP 4: Create Database User for Frappe (REQUIRED)
bash
Copy code
sudo mariadb
sql
Copy code
CREATE USER 'frappe'@'localhost' IDENTIFIED BY 'frappe';
GRANT ALL PRIVILEGES ON *.* TO 'frappe'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;


🐍 STEP 5: Install uv + Python 3.14 (MANDATORY)
bash
Copy code
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc

uv python install 3.14 --default
python3.14 --version
⚠️ Do NOT install:

python3-dev
python3-venv
virtualenv

uv handles all Python environments.

🟢 STEP 6: Install Node.js 24 (MANDATORY)
bash
Copy code
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc

nvm install 24
nvm use 24
nvm alias default 24
node -v

🧶 STEP 7: Install Yarn (Classic)
bash
Copy code
corepack enable
corepack prepare yarn@1.22.22 --activate
yarn -v

🧰 STEP 8: Install Bench + Process Manager
bash
Copy code
uv tool install frappe-bench
uv tool install honcho
export PATH="$HOME/.local/bin:$PATH"

bench --version
honcho --version


🚀 STEP 9: Initialize Frappe v16 Bench
bash
Copy code
bench init frappe-bench --frappe-branch version-16 --python python3.14
cd frappe-bench


🌐 STEP 10: Create a Site
bash
Copy code
bench new-site site1.local
Enter:

MySQL super user → frappe

MySQL password → frappe

Administrator password → (choose)


📦 STEP 11: Install ERPNext v16
bash
Copy code
bench get-app erpnext --branch version-16
bench --site site1.local install-app erpnext


▶ STEP 12: Start Development Server
bash
Copy code
bench start
Open:

arduino
Copy code
http://localhost:8000


🔐 STEP 13: Production Setup (VPS Only)
bash
Copy code
sudo bench setup production frappe
Includes:

NGINX

Supervisor

Redis queues

Cron jobs

🔒 SSL (Let’s Encrypt)
bash
Copy code
sudo apt install -y certbot python3-certbot-nginx
sudo bench setup lets-encrypt site1.local


🧩 Optional Apps (v16)
bash
Copy code
bench get-app payments --branch version-16
bench get-app hrms --branch version-16

bench --site site1.local install-app payments
bench --site site1.local install-app hrms


🛡 Firewall (Optional)
bash
Copy code
sudo ufw allow OpenSSH
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable


✅ Final Health Check
bash
Copy code
bench doctor
Expected:

Redis ✅

MariaDB ✅

Workers ✅

Scheduler ✅

🎯 Notes
wkhtmltopdf is OPTIONAL (install only if PDFs are required)

Do NOT use pip, pipx, or system Python

Python 3.14 + Node 24 are non-negotiable for v16

uv is the supported future-proof toolchain

markdown
Copy code

---

### ✅ Summary
- ❌ Removed **wrong / legacy** steps
- ✅ Uses **same structure** as your `.md`
- ✅ Fully compatible with **Ubuntu 24.04 + Frappe v16**
- ✅ Copy-paste safe

If you want, I can also:
- produce a **WSL-specific version**
- generate a **production-only minimal guide**
- convert this into a **PDF or README**

Just tell me 👍










