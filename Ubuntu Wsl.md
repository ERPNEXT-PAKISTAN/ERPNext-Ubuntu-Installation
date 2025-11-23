# 🐧 Complete Guide: Full Backup & Restore for Ubuntu WSL (with Copy Ready Steps)

![Ubuntu Logo](https://assets.ubuntu.com/v1/29985a98-ubuntu-logo32.png)

Easily backup your Ubuntu WSL to a file and restore it later or onto another PC using **official WSL commands**. This guide lists every step—just copy and paste!

---

## ✨ Table of Contents
1. [Check Current WSL Distributions](#1-check-current-wsl-distributions)
2. [Shut Down All WSL Instances (Recommended)](#2-shut-down-all-wsl-instances-recommended)
3. [Export (Backup) Your Ubuntu WSL](#3-export-backup-your-ubuntu-wsl)
4. [Restore (Import) the Backup — As New or Overwrite Original](#4-restore-import-the-backup--as-new-or-overwrite-original)
5. [Create Extra Copies (Multiple Ubuntu Instances)](#5-create-extra-copies-multiple-ubuntu-instances)
6. [Start/Use Any Instance](#6-startuse-any-instance)
7. [Tips & Official Documentation](#7-tips--official-documentation)

---

## 1. Check Current WSL Distributions

Open a **PowerShell** or **CMD** window and see all available distributions and their exact names:

```powershell
wsl --list --verbose
```
> You'll see a table. Note the name of your Ubuntu instance (e.g. `Ubuntu-22.04`).

---

## 2. Shut Down All WSL Instances (Recommended)

Cleanly shut down WSL before backup (avoids data loss or FS corruption):

```powershell
wsl --shutdown
```

---

## 3. Export (Backup) Your Ubuntu WSL

Create a tar file—this is your full backup! Replace names and paths as needed.

```powershell
wsl --export <WSL-Distro-Name> "D:\Backups\your-backup-name.tar"
```

**Example:**
```powershell
wsl --export Ubuntu-22.04 "D:\Backups\ubuntu-22.04-fullbackup.tar"
```
- `<WSL-Distro-Name>` — from step 1 (example: Ubuntu-22.04)
- The file can go anywhere (external, network, second drive, etc.)

---

## 4. Restore (Import) the Backup — As New or Overwrite Original

### 💡 **A. Restore as a New Instance** (Safest, keeps old copy)

1. Create a folder for the new Ubuntu instance's filesystem.

    ```powershell
    mkdir "D:\WSL\UbuntuRestored"
    ```

2. Import the backup:

    ```powershell
    wsl --import UbuntuRestored "D:\WSL\UbuntuRestored" "D:\Backups\ubuntu-22.04-fullbackup.tar" --version 2
    ```

    - `UbuntuRestored` — any name you want for the new distro.
    - 2nd argument = folder created above.
    - 3rd argument = path to your backup tar file.
    - `--version 2` ensures it's WSL2.

---

### ⚠️ **B. Overwrite/Replace the Original WSL** (Deletes original, then restores!)

**WARNING:** This removes your original WSL installation!

1. Export (already done—see Step 3).

2. Unregister (delete) your original Ubuntu:

    ```powershell
    wsl --unregister Ubuntu-22.04
    ```

3. Import the backup with the original name:

    ```powershell
    wsl --import Ubuntu-22.04 "D:\WSL\Ubuntu-22.04" "D:\Backups\ubuntu-22.04-fullbackup.tar" --version 2
    ```

---

## 5. Create Extra Copies (Multiple Ubuntu Instances)

You can run several Ubuntu distros based on the same backup tar.

- Create a folder for each new instance:

    ```powershell
    mkdir "E:\WSL\Ubuntu-22.04-Second"
    mkdir "E:\WSL\Ubuntu-22.04-Third"
    ```

- Import as new names:

    ```powershell
    wsl --import Ubuntu-22.04-Second "E:\WSL\Ubuntu-22.04-Second" "E:\Backups\ubuntu-22.04-fullbackup.tar" --version 2
    wsl --import Ubuntu-22.04-Third "E:\WSL\Ubuntu-22.04-Third" "E:\Backups\ubuntu-22.04-fullbackup.tar" --version 2
    ```

- List all instances anytime:

    ```powershell
    wsl --list --verbose
    ```

---

## 6. Start/Use Any Instance

Use any Ubuntu instance by name:

```powershell
wsl -d Ubuntu-22.04             # original
wsl -d UbuntuRestored           # restored/new copy
wsl -d Ubuntu-22.04-Second      # extra copy
wsl -d Ubuntu-22.04-Third       # another copy
```

You can run multiple terminals with different instances at the same time!

---

## 7. Tips & Official Documentation

- Backups include **all files, installed packages, user configs, and home directories**.
- Store backups on **external drives or cloud** for safe keeping.
- Test your backup by importing it on another PC or as a new instance before wiping the original.
- For more info, see [Microsoft Docs: WSL Export/Import](https://learn.microsoft.com/en-us/windows/wsl/use-custom-distro#exporting-and-importing-an-exported-distribution) and [WSL Official](https://learn.microsoft.com/en-us/windows/wsl/).

---

### 👍 Done! Now your Ubuntu WSL state is fully portable and restorable!
