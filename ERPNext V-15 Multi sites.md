diff --git a//home/frappe/ERPNext-v15-Ubuntu-22.04-Complete-Guide.md b//home/frappe/ERPNext-v15-Ubuntu-22.04-Complete-Guide.md
new file mode 100644
--- /dev/null
+++ b//home/frappe/ERPNext-v15-Ubuntu-22.04-Complete-Guide.md
@@ -0,0 +1,419 @@
+# 🌟 Frappe-ERPNext Version 15 Installation Guide (Ubuntu 22.04 LTS)
+
+A complete guide to install Frappe/ERPNext v15 on Ubuntu 22.04 LTS.
+
+---
+
+## 📚 Pre-requisites
+
+- **Python:** 3.11+
+- **Node.js:** 18+
+- **Redis:** 5+ (caching and real-time updates)
+- **MariaDB:** 10.6+ (recommended on Ubuntu 22.04)
+- **yarn:** 1.22+ (JS dependency manager)
+- **pip:** 20+ (Python dependency manager)
+- **wkhtmltopdf:** 0.12.5+ (with patched Qt, for PDF generation)
+- **cron:** scheduled jobs (backups, scheduler)
+- **NGINX:** production reverse proxy
+
+---
+
+## 🚀 Steps to Install Python 3.11.x
+
+> **Note:** Ubuntu 23.xx+ includes Python 3.11 by default. Skip these steps if you already have it.
+
+1. **Add Python repo & update:**
+   ```bash
+   sudo add-apt-repository ppa:deadsnakes/ppa -y
+   sudo apt update
+   ```
+
+2. **Install Python 3.11:**
+   ```bash
+   sudo apt install -y python3.11 python3.11-dev python3.11-venv python3-distutils
+   python3.11 --version
+   ```
+
+---
+
+## 🚀 Create a New User
+
+1. **Create the user**
+   ```bash
+   sudo adduser frappe
+   ```
+
+2. **Give the user sudo privileges**
+   ```bash
+   sudo usermod -aG sudo frappe
+   ```
+
+3. **Switch to the new user**
+   ```bash
+   su - frappe
+   ```
+
+4. **Navigate to home directory**
+   ```bash
+   cd /home/frappe
+   ```
+
+---
+
+## 🛠 Installation Steps
+
+### 🟢 STEP 1: Install Base Packages
+
+```bash
+sudo apt-get update
+sudo apt-get install -y \
+  git curl wget vim cron nginx software-properties-common \
+  python3-dev python3-pip python3-setuptools python3-venv \
+  mariadb-server mariadb-client libmysqlclient-dev \
+  redis-server \
+  xvfb libfontconfig wkhtmltopdf
+```
+
+---
+
+### 🟢 STEP 2: Configure MariaDB (Unicode)
+
+Create Frappe MariaDB config:
+
+```bash
+sudo tee /etc/mysql/mariadb.conf.d/99-frappe.cnf > /dev/null <<'EOF'
+[mysqld]
+character-set-client-handshake = FALSE
+character-set-server = utf8mb4
+collation-server = utf8mb4_unicode_ci
+innodb-file-per-table = 1
+EOF
+```
+
+Restart MariaDB:
+
+```bash
+sudo systemctl restart mariadb
+sudo systemctl enable mariadb
+```
+
+Create DB user for setup:
+
+```bash
+sudo mariadb -e "CREATE USER IF NOT EXISTS 'frappe'@'localhost' IDENTIFIED BY 'YOUR_DB_PASSWORD';"
+sudo mariadb -e "GRANT ALL PRIVILEGES ON *.* TO 'frappe'@'localhost' WITH GRANT OPTION;"
+sudo mariadb -e "FLUSH PRIVILEGES;"
+```
+
+---
+
+### 🟢 STEP 3: Install Node.js 18.x (via NVM)
+
+```bash
+curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
+export NVM_DIR="$HOME/.nvm"
+[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
+nvm install 18
+nvm alias default 18
+node -v
+```
+
+---
+
+### 🟢 STEP 4: Install Yarn
+
+```bash
+npm install -g yarn
+yarn -v
+```
+
+---
+
+### 🟢 STEP 5: Install frappe-bench CLI
+
+```bash
+python3 -m pip install --user frappe-bench
+echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
+export PATH="$HOME/.local/bin:$PATH"
+bench --version
+```
+
+---
+
+### 🟢 STEP 6: Initialize Frappe Bench v15
+
+```bash
+bench init frappe-bench --frappe-branch version-15 --python python3.11
+cd frappe-bench/
+```
+
+---
+
+### 🟢 STEP 7: Change User Directory Permissions (Important for assets)
+
+```bash
+chmod o+rx /home/frappe
+```
+
+---
+
+### 🟢 STEP 8: Create First Site
+
+```bash
+bench new-site site1.local \
+  --db-root-username frappe \
+  --db-root-password YOUR_DB_PASSWORD \
+  --admin-password YOUR_ADMIN_PASSWORD
+
+bench use site1.local
+```
+
+Optional (local machine hostname mapping):
+
+```bash
+bench --site site1.local add-to-hosts
+```
+
+---
+
+### 🟢 STEP 9: Install ERPNext v15 + Payments
+
+```bash
+bench get-app erpnext --branch version-15
+bench --site site1.local install-app erpnext
+
+bench get-app payments --branch version-15
+bench --site site1.local install-app payments
+```
+
+---
+
+# Setting ERPNext for Production
+
+### 🟢 STEP 10: Enable Scheduler + Disable Maintenance
+
+```bash
+bench --site site1.local enable-scheduler
+bench --site site1.local set-maintenance-mode off
+```
+
+---
+
+### 🟢 STEP 11: Setup Production Config
+
+Install bench for root user (needed by some production commands):
+
+```bash
+sudo -H pip3 install frappe-bench
+```
+
+Run production setup:
+
+```bash
+sudo bench setup production frappe
+```
+
+---
+
+### 🟢 STEP 12: Ensure Supervisor Programs Are Loaded
+
+```bash
+sudo ln -sf /home/frappe/frappe-bench/config/supervisor.conf /etc/supervisor/conf.d/frappe-bench.conf
+sudo supervisorctl reread
+sudo supervisorctl update
+sudo supervisorctl restart all
+sudo supervisorctl status
+```
+
+---
+
+# Setup Multitenancy → Multiple Sites
+
+Creating multiple sites inside the same Frappe/ERPNext bench.
+
+### ✔️ One bench → Many Sites
+
+Example:
+
+- `site1.local`
+- `site2.local`
+- `site3.local`
+
+### 🎯 Separate Tenant
+
+Each site has:
+
+- Separate database
+- Separate users
+- Separate settings
+- Independent operations
+
+### 🧩 Two ways to run multiple sites
+
+- **Port-based multitenancy:** each site on different port
+- **DNS-based multitenancy:** each site on different hostname/domain
+
+---
+
+## A) DNS-based Multitenancy (same port, different hostname)
+
+### 🚀 STEP A1: Create Additional Sites
+
+```bash
+bench new-site site2.local \
+  --db-root-username frappe \
+  --db-root-password YOUR_DB_PASSWORD \
+  --admin-password YOUR_ADMIN_PASSWORD
+
+bench --site site2.local install-app erpnext
+bench --site site2.local install-app payments
+bench --site site2.local enable-scheduler
+bench --site site2.local set-maintenance-mode off
+
+bench new-site site3.local \
+  --db-root-username frappe \
+  --db-root-password YOUR_DB_PASSWORD \
+  --admin-password YOUR_ADMIN_PASSWORD
+
+bench --site site3.local install-app erpnext
+bench --site site3.local install-app payments
+bench --site site3.local enable-scheduler
+bench --site site3.local set-maintenance-mode off
+```
+
+---
+
+### 🚀 STEP A2: Turn on DNS multitenancy
+
+```bash
+bench config dns_multitenant on
+bench setup nginx
+sudo nginx -t
+sudo systemctl reload nginx
+sudo supervisorctl restart all
+```
+
+---
+
+### 🚀 STEP A3: Access URLs
+
+```text
+http://site1.local
+http://site2.local
+http://site3.local
+```
+
+> On network clients, map DNS/hosts to server IP.  
+> Wrong format example (do not use): `http://172.169.12.28.site3.local`
+
+---
+
+## B) Port-based Multitenancy (same IP, different ports)
+
+This mode avoids hostname mapping and is best for mobile/LAN quick access.
+
+### 🚀 STEP B1: Disable DNS mode
+
+```bash
+bench config dns_multitenant off
+```
+
+---
+
+### 🚀 STEP B2: Assign Ports Per Site
+
+```bash
+bench set-nginx-port site1.local 8081
+bench set-nginx-port site2.local 8082
+bench set-nginx-port site3.local 8083
+```
+
+---
+
+### 🚀 STEP B3: Reload Nginx and Restart Processes
+
+```bash
+sudo nginx -t
+sudo systemctl reload nginx
+sudo supervisorctl restart all
+```
+
+---
+
+### 🚀 STEP B4: Open Firewall Ports (if UFW enabled)
+
+```bash
+sudo ufw allow 8081/tcp
+sudo ufw allow 8082/tcp
+sudo ufw allow 8083/tcp
+sudo ufw status
+```
+
+---
+
+### 🚀 STEP B5: Access URLs
+
+```text
+http://172.169.12.28:8081  -> site1.local
+http://172.169.12.28:8082  -> site2.local
+http://172.169.12.28:8083  -> site3.local
+```
+
+---
+
+## ✅ Verification Commands
+
+```bash
+bench --site site1.local list-apps
+bench --site site2.local list-apps
+bench --site site3.local list-apps
+
+sudo supervisorctl status
+sudo systemctl status nginx --no-pager
+```
+
+---
+
+## 🧯 Common Fixes
+
+### CSS/JS not loading
+
+```bash
+chmod o+rx /home/frappe
+sudo systemctl reload nginx
+```
+
+### NGINX error: unknown log format "main"
+
+In `/home/frappe/frappe-bench/config/nginx.conf`, use:
+
+```nginx
+access_log /var/log/nginx/access.log;
+```
+
+Then:
+
+```bash
+sudo nginx -t
+sudo systemctl reload nginx
+```
+
+### 502 Bad Gateway
+
+```bash
+sudo supervisorctl status
+sudo supervisorctl restart all
+```
+
+---
+
+## 🎉 All Done!
+
+You now have Frappe and ERPNext v15 running on Ubuntu 22.04 LTS with:
+
+- Single-site production setup
+- DNS-based multitenancy
+- Port-based multitenancy
+
+Ready to build, customize, and scale your business apps.
+
