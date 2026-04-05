# 📱 Android Optimization & ADB Toolkit (Realme X7 Pro)

## 📌 Overview

This document captures the complete setup and optimization performed on a Realme device using ADB and Termux.

Goals:

* Remove bloatware
* Improve performance & battery
* Enable advanced control via ADB
* Convert device into a mini Linux server

---

# 🔧 ADB Setup

## Enable Developer Options

* Settings → About Phone → Tap **Build Number** 7 times

## Enable USB Debugging

* Developer Options → USB Debugging → ON
* Allow connection when prompted

## Verify Connection

```bash
adb devices
```

---

# 📦 Install APK via ADB

```bash
adb install app.apk
```

Reinstall:

```bash
adb install -r app.apk
```

Grant permissions:

```bash
adb install -g app.apk
```

---

# 🧹 Debloat (Remove Bloatware)

## List Packages

```bash
adb shell pm list packages
```

Filter:

```bash
adb shell pm list packages | grep -i realme
adb shell pm list packages | grep -i oplus
adb shell pm list packages | grep -i coloros
```

## Remove App

```bash
adb shell pm uninstall --user 0 <package>
```

## Disable App (safer)

```bash
adb shell pm disable-user --user 0 <package>
```

## Restore App

```bash
adb shell cmd package install-existing <package>
```

---

# 🚀 Debloat Script

```bash
#!/bin/bash

echo "Starting Realme Debloat..."

adb shell pm uninstall --user 0 com.heytap.themestore
adb shell pm uninstall --user 0 com.heytap.usercenter
adb shell pm uninstall --user 0 com.oppo.quicksearchbox
adb shell pm uninstall --user 0 com.coloros.video
adb shell pm uninstall --user 0 com.coloros.soundrecorder
adb shell pm uninstall --user 0 com.oplus.games
adb shell pm uninstall --user 0 com.oplus.cast
adb shell pm uninstall --user 0 com.oplus.screenrecorder

adb shell pm disable-user --user 0 com.oplus.statistics.rom

echo "Debloat completed!"
```

Run:

```bash
chmod +x debloat_realme.sh
./debloat_realme.sh
```

---

# 🔋 Performance & Battery Optimization

## Disable Tracking

```bash
adb shell pm disable-user --user 0 com.oplus.statistics.rom
adb shell pm disable-user --user 0 com.heytap.mcs
```

## Optional (Advanced)

```bash
adb shell pm disable-user --user 0 com.oplus.deepthinker
adb shell pm disable-user --user 0 com.oplus.athena
```

## Clear Cache

```bash
adb shell pm trim-caches 999999999999999999
```

## Reduce Animations

* Developer Options:

  * Window animation: 0.5x
  * Transition animation: 0.5x
  * Animator duration: 0.5x

---

# 🖥️ Termux Setup (Mini Linux Server)

## Install Packages

```bash
pkg update && pkg upgrade -y
pkg install openssh git curl wget nano htop -y
```

---

## 🔐 SSH Server

```bash
passwd
sshd
```

Find IP:

```bash
ip a
```

Connect from PC:

```bash
ssh <user>@<phone_ip> -p 8022
```

---

## 🌐 Web Server (NGINX)

```bash
pkg install nginx -y
nginx
```

Access:

```
http://localhost:8080
```

---

## 🐍 Python Server

```bash
pkg install python -y
python -m http.server 8000
```

---

## 🗄️ Database (Optional)

```bash
pkg install mariadb -y
```

---

## 🔁 Auto Start Services

Create script:

```bash
nano start.sh
```

```bash
sshd
nginx
```

Run:

```bash
bash start.sh
```

---

## ⚡ Keep Services Running

```bash
pkg install termux-services -y
sv-enable sshd
```

---

# 🔥 Use Cases

* Remote Linux server
* File transfer via SSH/SFTP
* Host websites locally
* Run automation scripts
* Development environment

---

# ⚠️ Important Notes

* Do NOT remove critical system apps:

  * com.android.systemui
  * com.android.settings
  * com.google.android.gms

* Always debloat in small steps

---

# 🧠 Future Enhancements

* Install Shizuku for advanced control
* Setup Docker (advanced)
* Use custom launcher for clean UI
* Explore GSI ROM (experimental)

---

# ✅ Status

✔ ADB configured
✔ Bloatware removed
✔ Performance optimized
✔ Termux server running

---

# 🚀 Author

Optimized by: Alok B
Device: Realme X7 Pro
Purpose: Performance + Dev Environment
