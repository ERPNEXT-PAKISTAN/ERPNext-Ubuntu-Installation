# ✅ Frappe + ERPNext v15 Installation Guide (Ubuntu 24.04 LTS)

---

## 📌 Pre-requisites

| Component    | Required Version                         |
|--------------|-----------------------------------------|
| Python       | 3.11+ (Ubuntu 24.04 comes with 3.12)    |
| Node.js      | 18+                                     |
| MariaDB      | 10.3.x – 10.6 (10.11 will work with warning) |
| Redis        | 5+                                      |
| Yarn         | 1.12+                                   |
| pip          | 20+                                     |
| wkhtmltopdf  | 0.12.5 (with patched Qt)                |
| NGINX        | For production (optional for dev)        |
| cron         | Required for background jobs             |

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


## ⚙ STEP 1: Install Git

```bash
sudo apt-get install git
```

---

## ⚙ STEP 2: Install Python Dev Headers

```bash
sudo apt-get install python3-dev
```

---

## ⚙ STEP 3: Install pip and setuptools

```bash
sudo apt-get install python3-setuptools python3-pip
```

---

## ⚙ STEP 4: Install Virtualenv

```bash
sudo apt install python3.12-venv
```

---

## ⚙ STEP 5: Install MariaDB Server

```bash
sudo apt-get install software-properties-common
sudo apt install mariadb-server
sudo systemctl status mariadb
sudo mysql_secure_installation
```

**During `mysql_secure_installation`:**
- Enter current password: press ENTER
- Switch to unix_socket: Y
- Change root password: Y
- Remove anonymous users: Y
- Disallow root login remotely: N
- Remove test database: Y
- Reload privilege tables: Y

---

## ⚙ STEP 6: Install MySQL Development Headers

```bash
sudo apt-get install libmysqlclient-dev
```

---

## ⚙ STEP 7: Configure MariaDB

Edit config file:

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Paste this at the end of the file:

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

Save and exit, then:

```bash
sudo service mysql restart
```

---

## ⚙ STEP 8: Install Redis

```bash
sudo apt-get install redis-server
```

---

## ⚙ STEP 9: Install Node.js 18 via NVM

```bash
sudo apt install curl
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 18
```

---

## ⚙ STEP 10: Install Yarn

```bash
sudo apt-get install npm
sudo npm install -g yarn
```

---

## ⚙ STEP 11: Install wkhtmltopdf (for PDF reports)

```bash
sudo apt-get install xvfb libfontconfig wkhtmltopdf
```

---

## ⚙ STEP 12: Install Frappe Bench CLI

```bash
sudo -H pip3 install frappe-bench --break-system-packages
bench --version
```

---

## ⚙ STEP 13: Initialize Frappe Bench (Frappe v15)

```bash
bench init frappe-bench --frappe-branch version-15
cd frappe-bench
```

---

## ⚙ STEP 14: Create Your Site

```bash
bench new-site site1.local
bench use site1.local   
```

Set MySQL root password  
Choose Administrator password

Then:

```bash
bench --site site1.local add-to-hosts
```

---

## ⚙ STEP 15: Install ERPNext App (v15)

```bash
bench get-app erpnext --branch version-15
# OR if you prefer full URL:
# bench get-app https://github.com/frappe/erpnext --branch version-15

bench --site site1.local install-app erpnext
```

---

## ⚙ STEP 16: Start Development Server

```bash
bench start
```

Then open your browser:

```arduino
http://site1.local:8000

```

Login using:  
**User:** Administrator  
**Password:** admin
