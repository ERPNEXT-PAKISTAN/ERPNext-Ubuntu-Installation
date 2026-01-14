# 🌟 Frappe-ERPNext Version 16 Installation Guide (Ubuntu 22.04 LTS)

A complete guide to install Frappe/ERPNext v16 on Ubuntu 22.04 LTS.

---

## 📚 Pre-requisites

- **Python:** 3.14+  
- **Node.js:** 24+  
- **Redis:** 5+ (caching and real-time updates)  
- **MariaDB:** 10.6.x (recommended)  
- **yarn:** 1.22+ (JS dependency manager)  
- **pip:** 20+ (Python dependency manager)  
- **wkhtmltopdf:** 0.12.5+ (with patched Qt, for PDF generation)  
- **cron:** (scheduled jobs: backups, certificate renewal)  
- **NGINX:** (production proxying)  

  (❌-Not Install it)
---

## 🚀 Steps to Install Python 3.14.x

> **Note:** Ubuntu 22.04 does not include Python 3.14 by default. We’ll use **uv** to install it.


### 0- 🚀 Create a New Use `frappe`

```
sudo adduser frappe
```
`set Password for user`
```
sudo usermod -aG sudo frappe
su frappe
```
```
cd /home/frappe
```

🟢🟢 STEP : Change User Directory Permissions:
```
chmod -R o+rx /home/frappe
```




1 - **Install uv (Python manager):**
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc
uv --version
```

2 - Install Python 3.14:
```
uv python install 3.14 --default
python3.14 --version
```
---
```
sudo apt clean
```
```
sudo apt update
```
```
sudo apt upgrade -y
```

```
❌ sudo apt update --fix-missing
```


---





## 🛠 Installation Step


🟢 STEP 1: Install Git
```
sudo apt-get install git
```


🟢 STEP 5: Install MariaDB
```
sudo apt install mariadb-server mariadb-client
sudo mysql_secure_installation
```
`
During mysql_secure_installation, follow these:
- Switch to unix_socket authentication: Y
- Change root password: N (keep default)
- Remove anonymous users: Y
- Disallow root login remotely: Y
- Remove test database: Y
- Reload privilege tables: Y
`

🟢 STEP 6: Install MySQL development files
```
sudo apt-get install libmysqlclient-dev
```


🟢 STEP 7: Configure MariaDB (Unicode encoding)
```
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```

Add at the end:
[mysqld]
```
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci
innodb-file-format=barracuda
innodb-file-per-table=1
innodb-large-prefix=1
```
[mysql]
```
default-character-set = utf8mb4
```

Restart MariaDB:
```
sudo service mariadb restart
```
```
sudo service mysql restart
```

---
Create database user for Frappe
```
sudo mariadb
```
```SQL
CREATE USER 'frappe'@'localhost' IDENTIFIED BY 'frappe';
GRANT ALL PRIVILEGES ON *.* TO 'frappe'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;
EXIT;
```
---



🟢 STEP 8: Install Redis
```
sudo apt install -y redis-server
sudo apt-get install redis-server
sudo systemctl enable --now redis-server
```


### 🟢  Install pkg-config
```
sudo apt install -y pkg-config build-essential libffi-dev libssl-dev
```
```
pkg-config --version
which pkg-config
```

### If pkg-config still isn’t found for any reason, install the alternative provider:
```
sudo apt install -y pkgconf
pkg-config --version
```



🟢 STEP 9: Install Node.js 24.x (via NVM)
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
source ~/.bashrc
nvm install 24
nvm use 24
node -v
```


🟢 STEP 10: Install Yarn
```
corepack enable
corepack prepare yarn@1.22.22 --activate
yarn -v
```


🟢 STEP 11: Install wkhtmltopdf
```
sudo apt-get install xvfb libfontconfig wkhtmltopdf
```


🟢 STEP 12: Install frappe-bench CLI
```
uv tool install frappe-bench
export PATH="$HOME/.local/bin:$PATH"
bench --version
```


🟢 STEP 13: Initialize Frappe bench & install Frappe v16
```
bench init frappe-bench --frappe-branch version-16 --python python3.14
cd frappe-bench/
```


🟢 STEP 14: Change User Directory Permissions
```
chmod -R o+rx /home/frappe
```


🟢 STEP 15: Create a site in Frappe bench
```
bench new-site site1.local
bench use site1.local
bench --site site1.local add-to-hosts
```

`Visit: http://site1.local:8000`


🟢 STEP 16: Install ERPNext v16 in bench & site
```
bench get-app erpnext --branch version-16
bench --site site1.local install-app erpnext
bench start
```


🟢 STEP 17: Payments Module Installation
```
bench get-app payments
bench --site site1.local install-app payments
```

## install honcho
```
uv tool install honcho
export PATH="$HOME/.local/bin:$PATH"
honcho --version
```

# Bench Start
Setting ERPNext for Production
```
cd ~/frappe-bench
```


✅ STEP 1: Install required production packages
```
sudo apt update
sudo apt install -y nginx supervisor ansible
sudo systemctl enable --now nginx supervisor
```

#### Verify:
```
sudo systemctl status supervisor --no-pager | head -n 20
sudo systemctl status nginx --no-pager | head -n 20
```

✅ STEP 2: Ensure bench is available in sudo environment (recommended)
```
sudo env "PATH=$PATH" bench --version
```
```
sudo ln -sf /home/frappe/.local/bin/bench /usr/local/bin/bench
```

✅ 3 Run production setup (ONE TIME)
```
sudo env "PATH=$PATH" bench setup production frappe
```
This generates:
`supervisor config`
`nginx templates`
`system configs for queues/workers/scheduler`

✅ 4) Link supervisor config (required for auto-start)
```
sudo ln -sf /home/frappe/frappe-bench/config/supervisor.conf /etc/supervisor/conf.d/frappe-bench.conf
sudo supervisorctl reread
sudo supervisorctl update
```
    ✅ Start processes:
```
sudo supervisorctl restart all
sudo supervisorctl status
```

 ✅ 5) Setup Nginx (serve without port 8000)
```
sudo env "PATH=$PATH" bench setup nginx
sudo systemctl reload nginx
```
    ✅ Check nginx config loads:
```
sudo nginx -t
```


 ✅ 6) Enable scheduler + disable maintenance mode (site-level)
      Replace ziafoods.ksa with your site name.
```
bench --site ziafoods.ksa enable-scheduler
bench --site ziafoods.ksa set-maintenance-mode off
```
Recommended:
```
bench --site ziafoods.ksa migrate
bench --site ziafoods.ksa clear-cache
bench --site ziafoods.ksa clear-website-cache
```

7) Restart production services
```
sudo supervisorctl restart all
sudo systemctl reload nginx
```
8) Final health checks
```
sudo supervisorctl status
bench doctor
```

9) Reboot test (auto-start confirmation)
```
sudo reboot
```

After reboot:
```
sudo supervisorctl status
sudo systemctl status nginx --no-pager | head -n 20
```
✅ If supervisor shows all processes RUNNING, production auto-start is confirmed.




## 🔒 Optional: SSL (Let’s Encrypt)

Prerequisites
  Domain A record points to server IP
  Nginx is working for the domain
Install certbot:
```
sudo apt install -y certbot python3-certbot-nginx
```

Enable SSL:
```
sudo env "PATH=$PATH" bench setup lets-encrypt ziafoods.ksa
```

Test renewal:
```
sudo certbot renew --dry-run
```







---





# Setup Multitenancy → Multiple Sites
✔️ One bench → Many Sites
Example:
- site1.local
- site2.local
- site3.local

DNS-based multitenancy
🚀 STEP 1: Turn on DNS multitenancy
bench config dns_multitenant on


🚀 STEP 2: Create a new site
bench new-site site2.local


🚀 STEP 3: Update the Nginx configuration
bench setup nginx


🚀 STEP 4: Reload Nginx
sudo service nginx reload



Port-based multitenancy
1️⃣ Disable DNS Multitenancy
bench config dns_multitenant off


2️⃣ Create a New Site
bench new-site site2.local


3️⃣ Set Up Nginx
bench setup nginx


4️⃣ Reload Nginx
sudo systemctl reload nginx



🎉 All Done!
You now have Frappe and ERPNext v16 running on Ubuntu 22.04 LTS.
Ready to build, customize, and scale your business apps!

---

✨ This is now fully aligned with **ERPNext v16 requirements** (Python 3.14, Node.js 24, Bench 5.28+).  

Would you like me to also add a **“Quick Verification” section** at the end (Python, Node, Yarn, Bench versions) so you can confirm everything is installed correctly before production setup?









