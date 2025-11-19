# 🌟 Frappe-ERPNext Version 15 Installation Guide (Ubuntu 22.04 LTS)

A complete guide to install Frappe/ERPNext v15 on Ubuntu 22.04 LTS.

---

## 📚 Pre-requisites

- **Python:** 3.11+
- **Node.js:** 18+
- **Redis:** 5+ (caching and real-time updates)
- **MariaDB:** 10.3.x / **Postgres:** 9.5.x (database apps)
- **yarn:** 1.12+ (JS dependency manager)
- **pip:** 20+ (Python dependency manager)
- **wkhtmltopdf:** 0.12.5+ (with patched Qt, for PDF generation)
- **cron:** (scheduled jobs: backups, certificate renewal)
- **NGINX:** (production proxying)

---

## 🚀 Steps to Install Python 3.11.x

> **Note:** Ubuntu 23.xx+ includes Python 3.11 by default. Skip these steps if you already have it.

1. **Add Python repo & update:**
   ```bash
   sudo add-apt-repository ppa:deadsnakes/ppa -y
   sudo apt update
   ```

2. **Install Python 3.11:**
   ```bash
   sudo apt install python3.11
   python3.11 --version
   ```

3. **Install full extras:**
   ```bash
   sudo apt install python3.11-full
   ```
---

## 🚀 Create a New User
1. **Create the user**

   ```bash
   sudo adduser frappe
   ```

   You’ll be prompted to set a password and (optionally) provide user information.

2. **Give the user sudo privileges**

   ```bash
   sudo usermod -aG sudo frappe
   ```

3. **Switch to the new user**

   ```bash
   su frappe
   ```

4. **Navigate to the user’s home directory**

   ```bash
   cd /home/frappe
   ```

---

---

## 🛠 Installation Steps

### 🟢 STEP 1: Install Git

```bash
sudo apt-get install git
```

---

### 🟢 STEP 2: Install python-dev

```bash
sudo apt-get install python3-dev
```

---

### 🟢 STEP 3: Install setuptools & pip

```bash
sudo apt-get install python3-setuptools python3-pip
```

---

### 🟢 STEP 4: Install virtualenv

```bash
sudo apt install python3.11-venv
```

---

### 🟢 STEP 5: Install MariaDB

```bash
sudo apt-get install software-properties-common
sudo apt install mariadb-server
sudo mysql_secure_installation
```
> **During `mysql_secure_installation`, follow these:**
> - Enter current password for root (press ENTER)
> - Switch to unix_socket authentication: **Y**
> - Change root password: **Y** (set a new password)
> - Remove anonymous users: **Y**
> - Disallow root login remotely: **Y**
> - Remove test database: **Y**
> - Reload privilege tables: **Y**

---

### 🟢 STEP 6: Install MySQL development files

```bash
sudo apt-get install libmysqlclient-dev
```

---

### 🟢 STEP 7: Configure MariaDB (Unicode encoding)

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

**Add at the end:**
```ini
[server]
user = mysql
pid-file = /run/mysqld/mysqld.pid
socket = /run/mysqld/mysqld.sock
basedir = /usr
datadir = /var/lib/mysql
tmpdir = /tmp
lc-messages-dir = /usr/share/mysql
bind-address = 127.0.0.1
query_cache_size = 16M
log_error = /var/log/mysql/error.log

[mysqld]
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci      

[mysql]
default-character-set = utf8mb4
```
Press `Ctrl+X`, then `Y`, then `Enter` to save and exit.

**Restart MariaDB:**
```bash
sudo service mysql restart
```

---

### 🟢 STEP 8: Install Redis

```bash
sudo apt-get install redis-server
```

---

### 🟢 STEP 9: Install Node.js 18.x (via NVM)

```bash
sudo apt install curl 
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 18
```

---

### 🟢 STEP 10: Install Yarn

```bash
sudo apt-get install npm
sudo npm install -g yarn
```

---

### 🟢 STEP 11: Install wkhtmltopdf

```bash
sudo apt-get install xvfb libfontconfig wkhtmltopdf
```

---

### 🟢 STEP 12: Install frappe-bench CLI

```bash
sudo -H pip3 install frappe-bench
bench --version
```

---

### 🟢 STEP 13: Initialize Frappe bench & install Frappe v15

```bash
bench init frappe-bench --frappe-branch version-15 --python python3.11
cd frappe-bench/
bench start
```

---

### 🟢 STEP 14: Create a site in Frappe bench

```bash
bench new-site site1.local
bench use site1.local
bench --site site1.local add-to-hosts
```

> Visit: [http://site1.local:8000](http://site1.local:8000)

---

### 🟢 STEP 15: Install ERPNext v15 in bench & site

```bash
bench get-app erpnext --branch version-15
# OR
bench get-app https://github.com/frappe/erpnext --branch version-15

bench --site site1.local install-app erpnext
bench start
```

---

### 🟢 STEP 16: Payments Module Installation

```bash
bench get-app payments
bench --site site1.local install-app payments
```
---
## Setting ERPNext for Production

### 🟢 STEP 17: Enable Scheduler

```bash
bench --site site1.local enable-scheduler
```
---
### 🟢 STEP 18: Disable maintenance mode

```bash
bench --site site1.local set-maintenance-mode off
```
---
### 🟢 STEP 19: Setup production config

```bash
sudo bench setup production frappe
```
---
### 🟢 STEP 20: Setup NGINX to apply the changes

```bash
bench setup nginx
```
---
### 🟢 STEP 21: Restart Supervisor and Launch Production Mode

```bash
sudo supervisorctl restart all
sudo bench setup production frappe
```
---
---

# Setup Multitenancy
---


### 🟢 STEP 16: Payments Module Installation

```bash
bench get-app payments
bench --site site1.local install-app payments
```
---
### 🟢 STEP 16: Payments Module Installation

```bash
bench get-app payments
bench --site site1.local install-app payments
```
---
### 🟢 STEP 16: Payments Module Installation

```bash
bench get-app payments
bench --site site1.local install-app payments
```
---
===================




---
## 🎉 **All Done!**

You now have Frappe and ERPNext v15 running on Ubuntu 22.04 LTS.  
Ready to build, customize, and scale your business apps!

