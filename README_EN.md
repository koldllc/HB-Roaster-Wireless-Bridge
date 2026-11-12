# HB Roaster Wireless Bridge ☕️

> **Wirelessly connect HB Coffee Roasters to Artisan Roasting Scope over Wi-Fi (macOS Native Menu Bar App)**  
> Comprehensive support for HB Model S series (M2S / M6S / L2S / L6S) and HB Standard series.

[English (Current)](README_EN.md) • [中文说明 (README.md)](README.md) • [Windows Setup Guide](docs/WINDOWS_SETUP.md) • [Releases](https://github.com/koldllc/HB-Roaster-Wireless-Bridge/releases)

---

Eliminate physical USB serial cables! 100% compatible with your existing Artisan curves, colors, 4-channel configuration, events, and alarms for an effortless, instant wireless roasting experience.

## 🌟 Supported Platforms & Apps

- 🍎 **macOS Native App** (`HB Roaster Wireless Bridge.app`): Swift & SwiftUI native menu bar application.
- 🪟 **Windows Native App** (`HB-Roaster-Wireless-Bridge.exe`): Standalone Windows executable with dark dashboard, system tray residency, and zero-driver WebSocket connection. See [Windows Setup Guide](docs/WINDOWS_SETUP.md).

## 🌟 Key Features

- ☕️ **Minimalist Menu Bar / Tray App**: Sits discreetly in your macOS top menu bar or Windows system tray with live `BT / ET` temperature readouts.
- 🖥️ **Modern Control Panel**: Displays all 4 temperature channels (BT, ET, Inlet, Exhaust), Artisan polling state, and network status at a glance.
- 🌐 **Bilingual & Localization Support**: Native English and Simplified Chinese support with automatic OS language detection and manual switching.
- ⚡ **Dual Artisan Connect Modes**:
  - **Zero-Driver WebSocket Mode (Recommended)**: Connect Artisan directly to `ws://127.0.0.1:8088/ws` without any virtual COM port driver!
  - **Virtual Serial Port Mode**: Emulates Arduino TC4 (`126 ARC_BTET` & `127 ARC_METIT`) via PTY (macOS) or com0com (Windows).
- 🛡️ **Machine Control & Safety Confirmations**: Control roaster power, heat, fan, and agitation with confirmation guards to prevent bean scorching.
- 🔄 **Auto-Discovery & Reconnect**: Built-in UDP 17888 network broadcast discovery with direct physical network interface binding (VPN/proxy-immune) and candidate IP memory.
- 🚀 **Launch at Login**: Automatic background startup on Windows & macOS.
- 🪶 **Native & Lightweight**: Standalone execution without requiring pre-installed Python.

---

## 📥 Download & Installation

### Requirements
- **OS**: macOS 13.0 (Ventura) or newer
- **Architecture**: Apple Silicon (M1/M2/M3/M4) & Intel Mac

### Installation Steps
1. Download the latest `HB-Roaster-Wireless-Bridge-macos.zip` from [**Releases**](https://github.com/koldllc/HB-Roaster-Wireless-Bridge/releases).
2. Unzip and drag **`HB Roaster Wireless Bridge.app`** into your **`/Applications`** folder.
3. Double-click to launch the app.

> **💡 macOS Security Notice (First Launch)**:  
> If macOS displays *"cannot be opened because the developer cannot be verified"*:  
> 1. Go to **System Settings** -> **Privacy & Security**.  
> 2. Scroll down to the security section and click **"Open Anyway"**.  
> *(Alternatively, right-click `HB Roaster Wireless Bridge.app` in `/Applications` and select **"Open"**)*.

---

## 📐 Architecture

```
+-------------------+                 +--------------------------------+                 +-------------------+
|   HB Coffee       |   Wi-Fi (TCP)   | HB Roaster Wireless Bridge.app |   Virtual PTY   |   Artisan Roaster |
|   Roaster (ESP)   | <=============> |  (macOS Native Menu Bar App)   | <=============> |   Scope Software  |
|  Port: 16888/TCP  |   (1Hz 4-Ch)    |   Auto-discovery & Reconnect   | /tmp/hb_roaster |   Arduino TC4     |
+-------------------+                 +--------------------------------+                 +-------------------+
```

---

## ⚡ 3-Minute Quickstart

### Step 1: Power on Roaster
1. Switch on mechanical power on the HB coffee roaster.
2. Confirm the Wi-Fi icon on the roaster screen changes from blinking to **solid** (indicating Wi-Fi connection).
3. Ensure your Mac and roaster are connected to the same local Wi-Fi router.

### Step 2: Launch App
1. Open **HB Roaster Wireless Bridge.app**.
2. A flame icon ☕️ appears in your macOS menu bar. Click to expand the popover:
   - Status will show `Online (192.168.x.x)`.
   - All 4 temperature channels begin updating in real-time.
   - *(Optional: Click the ⚙️ gear icon, enable "Launch bridge automatically at login" and choose your preferred Language)*.

### Step 3: Configure Artisan (One-time setup)
1. Open **Artisan**.
2. Go to **Config** -> **Device**: Select **HB Model S** (or **HB Standard**).
3. Go to **Config** -> **Ports**:
   - **Comm Port**: Enter `/tmp/hb_roaster`
   - **Baud Rate**: `115200`
   - **Parameters**: `8` data bits, `None (N)` parity, `1` stop bit
   - Click **OK**.
4. Click Artisan's top-left **ON** button:
   - All 4 curves will immediately start plotting in real-time!
   - The Artisan status indicator in the app popover turns green: **"Artisan Sampling (Active)"**.
5. Click **START** to record your roast profile.

---

## ⚙️ Channel Mapping

| Channel | Probe Location | Artisan Driver | Artisan Curve Name | Suggested Color | Description |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Ch 3** | **Bean Temp (BT)** | `ARC_BTET` | **BT** | Green (`#00D084`) | Temperature inside drum contacting beans; used for RoR calculation |
| **Ch 4** | **Env / Drum Temp (ET)** | `ARC_BTET` | **ET** | Red (`#FF4757`) | Environmental air temperature inside roasting chamber |
| **Ch 1** | **Exhaust Temp (MET)** | `+ARC MET/IT` | **Exhaust** | Sky Blue (`#38BDF8`) | Exhaust flue and chaff collector air temperature |
| **Ch 2** | **Inlet Temp (IT)** | `+ARC MET/IT` | **Inlet** | Warm Ochre (`#FBBF24`) | Hot air temperature entering roasting drum |

---

## 💻 Optional Python CLI Mode

In addition to the native macOS App, the repository also includes a Python CLI implementation for headless or command-line environments:

```bash
# Run CLI in foreground
python3 main.py --ip 192.168.88.88

# Or manage as a macOS launchd user service
./service.sh install
```

---

## 🔍 Diagnostics

If Artisan is not showing readings or cannot find the machine:
```bash
python3 diagnose.py
```
Or directly click the **⚙️ Gear icon -> Run Quick Test** inside the macOS Menu Bar popover.

---

## 📄 License

Free to use for all HB Roaster coffee roaster users.
