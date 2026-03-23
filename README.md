# 🔐 Bypassing SSL Pinning in Flutter Applications using Frida & Burp Suite

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Android-green?style=for-the-badge">
  <img src="https://img.shields.io/badge/Tool-Frida-blue?style=for-the-badge">
  <img src="https://img.shields.io/badge/Proxy-BurpSuite-orange?style=for-the-badge">
  <img src="https://img.shields.io/badge/Focus-Mobile%20Pentesting-red?style=for-the-badge">
</p>

---

## 📌 Overview

This repository demonstrates a complete workflow for:

* Rooting an Android device
* Configuring interception using Burp Suite
* Installing and running Frida
* Bypassing SSL pinning in mobile applications
* Intercepting and analyzing HTTPS traffic

---

## 🧰 Tools Used

* 🛠️ Frida
* 🌐 Burp Suite
* 📱 Android Device (Rooted with Magisk)
* 🔌 ADB & Fastboot

---

# ⚙️ Setup Guide

---

## 🔓 1. Root Android Device (Magisk)
Root access is required in this setup to enable Frida instrumentation and full control over the application runtime.

The device was rooted using Magisk.
### 🔧 Steps (High-Level)
* Enable Developer Options
* Enable USB Debugging & OEM Unlock
* Unlock bootloader (this wipes data)
* Extract `boot.img` from firmware
* Patch using Magisk
* Flash patched image:

```bash
fastboot flash boot magisk_patched.img
fastboot reboot
```

### ✅ Verify Root

#### 🔹 Using ADB
```bash
adb shell
su
```
#### 🔹 Using Root Checker Application
<img src="Root Checker Application.jpeg" width="250">

---

## 🧪 2. Install Frida (Kali Linux)

```bash
pip install frida-tools
frida --version
```

---

## 📲 3. Install Frida Server on Android
🔹 Download Frida Server

Download from:
https://github.com/frida/frida/releases

(Select version matching your device architecture — e.g., arm64)

🔹 Extract & Prepare

```bash
xz -d frida-server-*.xz
mv frida-server-* frida-server
```
🔹 Push to Device
```bash
adb push frida-server /data/local/tmp/
```
🔹 Set Permissions
```bash
adb shell
su
chmod 755 /data/local/tmp/frida-server
```

---

## 🚀 4. Start Frida Server

```bash
/data/local/tmp/frida-server &
```

✅ Verify

```bash
frida-ps -U
```

---

## 🌐 5. Configure Burp Suite

* Proxy Listener:

  * IP → Your local machine IP
  * Port → 8080
> Note: Enable "Support invisible proxying"
<img src="Proxy Listener.png" width="250">

---

## 🔑 6. Install Burp Certificate

* Export as `.der`
* Install on Android:

```
Settings → Security → Install CA Certificate
```

✔ Confirm: **PortSwigger Certificate installed**

---

## 📡 7. Configure Proxy

### Option 1 — WiFi

* Host → Kali IP
* Port → 8080

### Option 2 — ProxyDroid

* Enable Global Proxy

---


# 🔥 SSL Pinning Bypass (Frida)

## 📜 Script

The Frida script used for bypassing SSL pinning is included in this repository:

👉 `script.js`
> Note: Replace PROXY_IP with Your local machine IP "
---

## ▶️ Run Script

### Spawn (Recommended)

```bash
frida -U -f com.target.app -l script.js
```
---

## 📊 Result

After applying the Frida bypass, HTTPS traffic from the target application is successfully intercepted in Burp Suite.

Requests and responses can now be inspected and modified during testing.
