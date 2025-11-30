# Backup you WSL And Resport:

---

## 1- ✅ Backup wsl:

1st Shutdown you wsl:
```powerShell
wsl --shutdown
```
Now Backup:
```Power Shell:
wsl --export Ubuntu-22.04 "C:\Erpnext\ubuntu-22.04-backup.tar"
```

## 2- ✅ Create in C:\\ or D:\\ the folder where the new WSL instance will live
```PowerShell:
mkdir "C:\WSL\ERPNext-6"
```

## 3- ✅ Import (restore) the backup with the new name ERPNext-6
```Power Shell:
wsl --import ERPNext-6 "C:\WSL\ERPNext-6" "C:\Erpnext\ubuntu-22.04.tar" --version 2
```

## 4- ✅ Run your new WSL instance from Power Shell or Window Start.
```Powershell:
wsl -d ERPNext-6
```

## 5- ✅ Set default user
```Terminal
su - frappe
```
`your user`, frappe, erpnext, abc`


---

# 1️⃣ Delete WSL instance Like ERPNext-6 completely

## Start Power Shell:

```Power Shell
wsl --shutdown
```

## Delete instance 

```powershell
wsl --unregister ERPNext-6
```


# 2️⃣ Restore / reinstall ERPNext-6 from backup



## 2- Create Folder ERPNext-6 Again: or other name.
```PowerShell:
mkdir "C:\WSL\ERPNext-6"
```

## Restore / reinstall ERPNext-6 from backup
```Power Shell
wsl --import ERPNext-6 "C:\WSL\ERPNext-6" "C:\Erpnext\ubuntu-22.04.tar" --version 2
```
set your folder loccation name and path     
`1. "C:\WSL\ERPNext-6"`     
`2. "C:\Erpnext\ubuntu-22.04.tar"`    

-------









