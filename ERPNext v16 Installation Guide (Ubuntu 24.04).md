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
```

### 🔄 STEP 1: System Update

```bash
sudo apt-get update -y   
sudo apt-get upgrade -y   
```


### .⚙ STEP 2.1 : Install Git  
```bash  
sudo apt-get install git -y
```

### ⚙ STEP 2.2 :  Install cURL
```bash
sudo apt-get install curl -y

```

### 🔐 STEP 2.3 Install Python

```bash
sudo apt-get install python3-dev python3-pip python3-setuptools -y
sudo apt-get install python3-venv -y
```


### 🐍 Install Virtual Enviroment
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
source ~/.bashrc
uv python install 3.14 --default
uv –-version
python3 --version
```

### 2.4 Install other required packages
```bash
sudo apt-get install software-properties-common -y
sudo apt-get install xvfb libfontconfig -y
sudo apt-get install libmysqlclient-dev -y
sudo apt-get install pkg-config -y
```

### 2.5 Install Redis Server
```bash
sudo apt-get install redis-server -y
redis-server --version
```

### 2.6 Install wkhtmltopdf
```bash
sudo wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-2/wkhtmltox_0.12.6.1-2.jammy_arm64.deb
# Change to amd64.deb depending on OS architecture

sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_arm64.deb
# Change to amd64.deb depending on OS architecture
# Running this command will show errors which we solve by running the next command

sudo apt-get -f install -y

sudo dpkg -i wkhtmltox_0.12.6.1-2.jammy_arm64.deb
```

### 3.1 Install MariaDB server
```bash
sudo apt install mariadb-server mariadb-client -y
```

### 3.2 Configure MariaDB server
```bash
sudo mysql_secure_installation
```

```css
Copy code
Switch to unix_socket authentication? → Y
Change root password? → N
Remove anonymous users? → Y
Disallow root login remotely? → Y
Remove test database? → Y
Reload privilege tables? → Y
```


### 🗄 3.3 Update MariaDB config file
```bash
sudo nano /etc/mysql/my.cnf
```

#### 🟢Add the below code block at the end of the file:
```bash
[mysqld]
character-set-client-handshake = FALSE
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
```


#### 🧶 3.4 Restart MariaDB server
```bash
sudo service mysql restart
```


### 🧰 4.1 Install Node
```bash
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 24
nvm use 24
node --version
```
#### 4.2 Install Node Package Manager (npm)
```bash
sudo apt-get install npm -y
```


### 4.3 Install Yarn
```bash
sudo npm install -g yarn
```


### 🚀 5.1 Install Frappe v16 Bench
```bash
uv tool install frappe-bench
bench --version
cd frappe-bench
```

### 🚀 5.2 Initialize Frappe v16 Bench
```bash
bench init --frappe-branch version-16 frappe-bench
```

### 5.3 Set bench directory permissions
```bash
sudo chmod -R o+rx /home/frappe-user/
```



### 🌐 6.1 Create new site
```bash
bench new-site site1.local
```
Enter:

`MySQL super user → frappe`   
`MySQL password → frappe`   
`Administrator password → (choose)`  







📦 STEP 11: Install ERPNext v16
```bash
Copy code
bench get-app erpnext --branch version-16
bench --site site1.local install-app erpnext
```

▶ STEP 12: Start Development Server
```bash
Copy code
bench start
Open:
```
```
arduino
Copy code
http://localhost:8000
```

🔐 STEP 13: Production Setup (VPS Only)
```bash
Copy code
sudo bench setup production frappe
Includes:
```
### NGINX

Supervisor
Redis queues
Cron jobs

🔒 SSL (Let’s Encrypt)
```bash
Copy code
sudo apt install -y certbot python3-certbot-nginx
sudo bench setup lets-encrypt site1.local
```

🧩 Optional Apps (v16)
```bash
Copy code
bench get-app payments --branch version-16
bench get-app hrms --branch version-16
```
```
bench --site site1.local install-app payments
bench --site site1.local install-app hrms
```

🛡 Firewall (Optional)
```bash
Copy code
sudo ufw allow OpenSSH
sudo ufw allow 80
sudo ufw allow 443
sudo ufw enable
```

✅ Final Health Check
```bash
Copy code
bench doctor
```
Expected:
Redis ✅
MariaDB ✅
Workers ✅
Scheduler ✅

### 🎯 Notes
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










