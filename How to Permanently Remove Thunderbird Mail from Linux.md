# 🧹 How to Permanently Remove Thunderbird Mail from Linux

This guide explains how to **completely uninstall Mozilla Thunderbird** (Mail client) and **remove all configuration, cache, and mail data** from your Linux system — including **Snap**, **APT**, and **DNF** installs.

---

## 🧾 Step 1: Identify Installation Type

Run the following to check if Thunderbird is installed via Snap or APT/DNF:

```bash
snap list | grep thunderbird
which thunderbird
````

---

## 🧨 Step 2: Remove Thunderbird (APT / DNF / Pacman)

### Ubuntu / Debian / Linux Mint

```bash
sudo apt purge thunderbird -y
sudo apt autoremove --purge -y
```

### Fedora / RHEL / Rocky / AlmaLinux

```bash
sudo dnf remove thunderbird -y
# or for older systems
sudo yum remove thunderbird -y
```

### Arch / Manjaro

```bash
sudo pacman -Rns thunderbird
```

---

## 🔥 Step 3: Remove Thunderbird Snap (if installed)

If Thunderbird is installed via Snap, run:

```bash
sudo snap remove thunderbird
```

Then clean up any remaining Snap data:

```bash
rm -rf ~/snap/thunderbird
sudo rm -rf /var/snap/thunderbird
sudo rm -rf /var/lib/snapd/snaps/thunderbird_*.snap
```

---

## 🧹 Step 4: Remove Configuration and Mail Data

Delete all local and cached Thunderbird data:

```bash
rm -rf ~/.thunderbird
rm -rf ~/.mozilla/thunderbird
rm -rf ~/.cache/thunderbird
sudo rm -rf /etc/thunderbird
```

These folders contain:

* Email accounts
* Messages
* Preferences
* Address books
* Cache files

---

## 🔍 Step 5: Verify Complete Removal

Check if Thunderbird is still installed:

```bash
which thunderbird
snap list | grep thunderbird
```

If both return no output, Thunderbird is fully removed.

---

## 🚫 Optional: Prevent Reinstallation

### Block APT version from reinstalling:

```bash
sudo apt-mark hold thunderbird
```

### Block Snap version from reinstalling:

```bash
sudo snap disable thunderbird
sudo snap remove thunderbird --purge
```

---

## ✅ Summary

After completing these steps, Thunderbird Mail is **permanently deleted** from your Linux system:

* ❌ Application binaries removed
* ❌ Snap and APT packages uninstalled
* ❌ User profiles, emails, and caches erased
* ❌ No auto reinstallation or updates

---

**Optional (Advanced):**
To securely erase all Thunderbird data beyond recovery, you can use:

```bash
shred -u ~/.thunderbird/*
```

---

**Author:** System Maintenance Notes
**Last Updated:** November 2025
