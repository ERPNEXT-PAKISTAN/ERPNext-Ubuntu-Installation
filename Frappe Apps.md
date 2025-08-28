## 📦 Additional App Installations for Frappe/ERPNext

Easily extend your site with these apps using the commands below.  
Just copy each code block to your terminal!

---

### ✨ Insights Installation

**Latest Version:**
```bash
bench get-app insights
OR
bench get-app insights --branch version-3
bench --site site1.local install-app insights
```

---

### 🖨️ Print Designer Installation

```bash
bench get-app print_designer
bench --site site1.local install-app print_designer
```

---

### 🛠️ Studio Installation

**Standard:**
```bash
bench get-app studio
bench --site site1.local install-app studio
```

**Or, install with new site:**
```bash
bench get-app studio
bench new-site studio.localhost --install-app studio
bench browse studio.localhost --user Administrator
```

---

### 🛒 Webshop Installation

```bash
bench get-app webshop
bench --site site1.local install-app webshop
```

---

### 🏗️ Builder Installation

```bash
bench get-app builder
bench --site site1.local install-app builder
```

---

### 👥 HRMS Installation

```bash
bench get-app hrms --branch version-15
bench --site site1.local install-app hrms
```
---

### 👥 Loan/Lending Installation

```bash
bench get-app lending
bench --site site1.local install-app lending
```
---

### 💳 Payments Module Installation

```bash
bench get-app payments
bench --site site1.local install-app payments
```

---

🛠 ### 💬 Chat Installation

```bash
bench get-app chat
bench --site site1.local install-app chat
```

---
🛠 ### IF ERROR
```bash
bench update --reset
```
 
🛠 ### Disable maintenance mode
```bash
bench --site site1.local set-maintenance-mode off
```
---
🛠 ### Enable or Disable Developer Mode
```bash
bench set-config developer_mode 1
```
```
bench set-config developer_mode 0
```
---
🛠 ### Enable or Disable Server Script:
```bash
bench set-config -g server_script_enabled 1
```
```
bench set-config -g server_script_enabled 0
```

---
###🛠 Enable Scheduler:
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

### Cd frappe-bench/sites
```bash
```
cat common_site_config.json
```

### Bench Restart.
```
sudo supervisorctl restart all
```
