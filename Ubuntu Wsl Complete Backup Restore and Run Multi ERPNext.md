1. Current Running

```powershell
wsl --list --verbose
```
2. Create insance
```powershell
powershell -ExecutionPolicy Bypass -File .\ERPNext-WSL-Auto-Create.ps1 -Action Create
```
2. Manager
```powershell
powershell -ExecutionPolicy Bypass -File .\ERPNext-WSL-Manager.ps1
```
2. PS Version
```powershell
$PSVersionTable.PSVersion
```
2. To Tools Folder
```powershell
cd "E:\ERPNext\Tools"
```
2. Quiet
```powershell
wsl --list --quiet
```
```powershell
powershell -ExecutionPolicy Bypass -File .\ERPNext-WSL-Auto-Create.ps1 -Action Manage
```
```powershell
powershell -ExecutionPolicy Bypass -File .\ERPNext-Instance-Dashboard.ps1
```
```powershell
$distros = (wsl --list --quiet)
```




