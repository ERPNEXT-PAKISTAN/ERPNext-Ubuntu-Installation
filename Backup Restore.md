<p align="center">
  <img width="120" alt="ERPNext Logo" src="https://raw.githubusercontent.com/frappe/design/master/logos/logo-2019/erpnext-logo.png" />
</p>

# 🗄️ How to Restore ERPNext Backup

Restoring an ERPNext backup lets you recover your site to a previous state, including your database and file attachments. This is useful after migrations, server failures, or when moving sites between servers.

---

## 🛠️ Step-by-step Guide

### 1. **Prepare Your Backup Files**
Make sure you have:
- **Database backup file** (e.g., `database.sql.gz`)
- **Private files backup** (e.g., `private-files.tar`)
- **Public files backup** (e.g., `public-files.tar`)

### 2. **Run the Restore Command**

Use the following command to restore your site.  
**Copy and paste the command below, replacing the file names and site name as needed:**

```bash
bench --site site1.local --force restore [database_file] --with-private-files [private_file] --with-public-files [public_file]
```
- Replace `site1.local` with your ERPNext site name.
- Replace `[database_file]`, `[private_file]`, and `[public_file]` with your actual backup file paths.

**Example:**
```bash
bench --site site1.local --force restore database.sql.gz --with-private-files private-files.tar --with-public-files public-files.tar
```

---

### 💡 **What This Command Does**
- Restores your ERPNext database from the backup file.
- Restores private and public file attachments.
- The `--force` flag overwrites the current site data.

---

### ⚠️ **Important Tips**
- **Backup current data** before restoring, as this process will overwrite the existing site.
- Make sure your files are accessible to the user running the `bench` command.
- After restore, check that your site is working correctly by logging in.

---

<p align="center">
  <img width="40" alt="WhatsApp" src="https://cdn-icons-png.flaticon.com/512/124/124034.png" />
  <strong>Need help? WhatsApp: <a href="https://wa.me/923009808900">+92 300 9808900</a></strong>
</p>





bench --site site1.local --force restore [database_file] --with-private-files [private_file] --with-public-files [public_file]
