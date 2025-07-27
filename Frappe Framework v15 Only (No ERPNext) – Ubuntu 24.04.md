# Frapppe-Framework

# ✅ Complete Installation – Frappe Framework v15 Only (No ERPNext) – Ubuntu 24.04

---

## 🔹 Step 1: Update system

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🔹 Step 2: Install Git

```bash
sudo apt install git -y
```

---

## 🔹 Step 3: Install Python Dev & Tools

```bash
sudo apt install python3-dev python3-setuptools python3-pip -y
```

---

## 🔹 Step 4: Install Python Virtual Environment

```bash
sudo apt install python3.12-venv -y
```

---

## 🔹 Step 5: Install and Secure MariaDB

```bash
sudo apt install mariadb-server -y
sudo systemctl enable mariadb
sudo systemctl start mariadb
sudo mysql_secure_installation
```
Follow prompts:
- Switch to unix_socket → Y
- Set root password → Y
- Remove anonymous users → Y
- Disallow remote root login → Y
- Remove test database → Y
- Reload privileges → Y

---

## 🔹 Step 6: Install MariaDB Dev Headers

```bash
sudo apt install libmysqlclient-dev -y
```

---

## 🔹 Step 7: Configure MariaDB for Frappe

```bash
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
Add (or update) the following:

```ini
[server]
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysqld]
innodb-file-format = barracuda
innodb-file-per-table = 1
innodb-large-prefix = 1
character-set-server = utf8mb4
collation-server = utf8mb4_unicode_ci

[mysql]
default-character-set = utf8mb4
```

Save and exit (`Ctrl + X`, then `Y`, then `Enter`), then:

```bash
sudo service mysql restart
```

---

## 🔹 Step 8: Install Redis Server

```bash
sudo apt install redis-server -y
```

---

## 🔹 Step 9: Install Node.js (v18) and NPM

```bash
sudo apt install curl -y
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.profile
nvm install 18
```

---

## 🔹 Step 10: Install Yarn

```bash
sudo apt install npm -y
sudo npm install -g yarn
```

---

## 🔹 Step 11: Install wkhtmltopdf (PDF generation)

```bash
sudo apt install xvfb libfontconfig wkhtmltopdf -y
```

---

## 🔹 Step 12: Install Frappe Bench CLI

```bash
sudo -H pip3 install frappe-bench --break-system-packages
bench --version
```

---

## 🔹 Step 13: Create Bench (Frappe Only)

```bash
bench init frappe-bench --frappe-branch version-15
cd frappe-bench
```

---

## 🔹 Step 14: Create Site (No ERPNext)

```bash
bench new-site site1.local
```
Follow prompts to:
- Enter MySQL root password
- Set Administrator password

Then:

```bash
bench --site site1.local add-to-hosts
```

---

## 🔹 Step 15: Create and Install Your Custom App

```bash
bench new-app invoicing
```
Answer prompts, then install:

```bash
bench --site site1.local install-app invoicing
```

---

## 🔹 Step 16: Start Frappe Server

```bash
bench start
```

Visit in browser:

```arduino
http://site1..local:8000
```

Login:  
**Username:** Administrator  
**Password:** admin

---

## ✅ DONE

You now have:
- Frappe Framework v15 (no ERPNext)
- A site (`site1.local`)
- A custom app (`invoicing`)

Ready to create Doctypes (forms), use REST API, build custom apps

---

### 🚀 Next Steps

Would you like the next steps to:
- Create your first Doctype
- Create a public form
- Use the REST API

Let me know and I’ll guide you!


