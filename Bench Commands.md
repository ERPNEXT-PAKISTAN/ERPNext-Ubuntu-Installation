## 📦 Additional Bench Command for Frappe/ERPNext


---

### ✨ enable-scheduler

```bash
bench --site site1.local enable-scheduler
bench --site site1.local scheduler enable
```
```
"pause_scheduler": 1,
"maintenance_mode": 1
```
---

### 🛠️ That’s why bench doctor keeps showing

```bash
Scheduler paused for site1.local
frappe.conf.pause_scheduler is SET
```
---

### 🛒 Webshop Installation

```bash
bench get-app webshop
bench --site site1.local install-app webshop
```

---

### 🏗️ Open your site config file:

```bash
cd ~/frappe-bench/sites
nano common_site_config.json
```
```
"pause_scheduler": 0,
"maintenance_mode": 0
```
---

### 👥 Restart bench:

```bash
cd ~/frappe-bench
bench restart
```
---

### 👥 Check

```bash
bench doctor
```
---

---
### ❌ ⚠️ 🛑 IF ERROR
```bash
bench update --reset
```
 
### 🚫 Disable maintenance mode
```bash
bench --site site1.local set-maintenance-mode off
```
---
### 👨‍💻 🧑‍💻 ⚙️ Enable or Disable Developer Mode
```bash
bench set-config developer_mode 1
```
```
bench set-config developer_mode 0
```
---
### 📜 ⚡ 🔄 Enable or Disable Server Script:
```bash
bench set-config -g server_script_enabled 1
```
```
bench set-config -g server_script_enabled 0
```

---
### ⏰ 📅 ▶️ Enable Scheduler:
```bash
bench --site site1.local enable-scheduler
```
```
bench --site site1.local scheduler enable
```
```
bench --site site1.local scheduler resume
```
```
bench doctor
```
---
### 📂 🗂️ 💻 Cd frappe-bench/sites
```bash
cat common_site_config.json
```
---
### 🔄 ♻️ 🖥️ Bench Restart.
```bash
sudo supervisorctl restart all
```
```
sudo systemctl restart supervisor
```
```
sudo systemctl restart frappe-bench
```


