# 🚀 Frappe + ERPNext v15 Installation Guide – Ubuntu 22.04 LTS

A complete step-by-step guide to install **Frappe Framework + ERPNext v15** on **Ubuntu 22.04 LTS**.  
No Docker. No scripts. Clean manual install.

---

## 📋 Pre-requisites

- Python 3.11+
- Node.js 18+
- Redis 5+
- MariaDB 10.3.x – 10.6 (10.11 will work but shows a warning)
- Yarn 1.12+
- pip 20+
- wkhtmltopdf 0.12.5 (with patched Qt)
- cron (for scheduled jobs)
- NGINX (optional – for production deployments)

---

## 🐍 Install Python 3.11 (If not already)

> Ubuntu 23.x and above comes with Python 3.11 by default.

```bash
sudo add-apt-repository ppa:deadsnakes/ppa -y
sudo apt update
sudo apt install python3.11
python3.11 --version
Install all optional dependencies:

bash
Copy
Edit
sudo apt install python3.11-full
🔧 STEP-BY-STEP INSTALLATION
✅ Step 1: Install Git
bash
Copy
Edit
sudo apt-get install git
✅ Step 2: Install Python Dev Headers
bash
Copy
Edit
sudo apt-get install python3-dev
✅ Step 3: Install pip and setuptools
bash
Copy
Edit
sudo apt-get install python3-setuptools python3-pip
✅ Step 4: Install Virtualenv
bash
Copy
Edit
sudo apt install python3.11-venv
✅ Step 5: Install MariaDB
bash
Copy
Edit
sudo apt-get install software-properties-common
sudo apt install mariadb-server
sudo systemctl status mariadb
sudo mysql_secure_installation
Follow on-screen instructions:

Press ENTER for current root password

Choose Y for unix_socket

Set root password

Choose Y for all security options

✅ Step 6: Install MySQL Development Headers
bash
Copy
Edit
sudo apt-get install libmysqlclient-dev
✅ Step 7: Configure MariaDB
bash
Copy
Edit
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
Paste the following into the file:

ini
Copy
Edit
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
Save and exit (Ctrl + X, then Y, then Enter)

Restart MariaDB:

bash
Copy
Edit
sudo service mysql restart
✅ Step 8: Install Redis
bash
Copy
Edit
sudo apt-get install redis-server
✅ Step 9: Install Node.js 18 with NVM
bash
Copy
Edit
sudo apt install curl
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 18
✅ Step 10: Install Yarn
bash
Copy
Edit
sudo apt-get install npm
sudo npm install -g yarn
✅ Step 11: Install wkhtmltopdf
bash
Copy
Edit
sudo apt-get install xvfb libfontconfig wkhtmltopdf
✅ Step 12: Install Frappe Bench CLI
bash
Copy
Edit
sudo -H pip3 install frappe-bench
bench --version
✅ Step 13: Initialize Frappe Bench
bash
Copy
Edit
bench init frappe-bench --frappe-branch version-15 --python python3.11
cd frappe-bench
✅ Step 14: Create New Site
bash
Copy
Edit
bench new-site dcode.com
Set MySQL root password and ERPNext admin password as prompted.

Add the site to hosts file:

bash
Copy
Edit
bench --site dcode.com add-to-hosts
✅ Step 15: Install ERPNext App
bash
Copy
Edit
bench get-app erpnext --branch version-15
# OR
# bench get-app https://github.com/frappe/erpnext --branch version-15

bench --site dcode.com install-app erpnext
✅ Step 16: Start Development Server
bash
Copy
Edit
bench start
Then open in browser:

arduino
Copy
Edit
http://dcode.com:8000
Login with:

Username: Administrator

Password: (from site creation step)

🎯 You're all set!
You now have a full Frappe + ERPNext v15 setup on Ubuntu 22.04 LTS ready for development and testing.

💡 Want production setup with NGINX + Supervisor?
Let me know — I’ll share the full production deployment guide as well!

javascript
Copy
Edit

> ✅ Save this as `README.md` in your GitHub repo.

Would you like a **production deployment version** too for Gunicorn + NGINX + SSL setup?








Ask ChatGPT



Tools



