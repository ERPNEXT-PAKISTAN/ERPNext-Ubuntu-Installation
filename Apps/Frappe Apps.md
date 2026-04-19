## 📦 Additional App Installations for Frappe/ERPNext

Easily extend your site with these apps using the commands below.  
Just copy each code block to your terminal!

---


### ✨ Insights Installation

**Latest Version:**
```bash
bench get-app insights --branch version-3
bench --site site1.local install-app insights
```
---

### <img width="25" height="50" alt="image" src="https://github.com/user-attachments/assets/4c369cb2-411f-46e8-bcef-f7ff66c41592" /> Frappe CRM

```bash
bench get-app crm
bench --site site1.local install-app crm
bench browse site1.local --user Administrator
```



---

### 🖨️ Print Designer Installation

```bash
bench get-app print_designer
bench --site site1.local install-app print_designer
```
---

### 💬 Insall Raven
<img width="213" height="68" alt="image" src="https://github.com/user-attachments/assets/2cd05aab-6e86-4f50-8bb5-4fb71b1fd52d" />

### Step-1: Download the Raven app.
```
bench get-app https://github.com/The-Commit-Company/raven
```

 ### Step-2:   Install the app on the site

```
bench --site site1.local install-app raven
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
### Third Party Backup

```
bench get-app https://github.com/frappe/offsite_backups --branch develop
bench install-app offsite_backups
```

---

### 💬 Chat Installation

```bash
bench get-app chat
bench --site site1.local install-app chat
```

---
### ❌ ⚠️ 🛑 IF ERROR
```bash
bench update --reset
```
 
