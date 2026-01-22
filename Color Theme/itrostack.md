## <img width="100" height="100" alt="image" src="https://github.com/user-attachments/assets/b468b520-d45f-4dca-8a1b-f0dac508dbfa" />   Change Frappe/ERPNext UI Color


### ✨ Color Theme Installation

```bash
cd ~/frappe-bench
```

### 🛠️ Get (download) the app into the bench

```bash
bench get-app https://github.com/itrostack/material_theme.git
```

### 🚀 Install the app on your site

```bash
bench --site site1.local install-app material_theme
```
If your site name is different, replace `site1.local` with it.


### 📦 Build assets (recommended for themes)

```bash
bench build --app material_theme
```

### 🔄 Restart Bench
```bash
bench restart
```
if   

### 🟡 Restart Processes
```bash
sudo supervisorctl restart all
sudo systemctl reload nginx
```

