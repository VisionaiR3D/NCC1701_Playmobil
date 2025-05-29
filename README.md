# **Playmobil Enterprise BLE Controller**

A replacement for the official Android **Playmobil Star Trek Enterprise AR** app—which has been crashing on launch—and an integration guide for Home Assistant. Use your Android phone or an ESP32 to control the U.S.S. Enterprise bridge via BLE with one-tap buttons or Home Assistant switches.

Download the .apk file from the releases page and install it on your Android device to control your Playmobil Enterprise. 

---

## **📖 Background**

The original Android Playmobil AR app (com.playmobil.ar.startrekenterprise) was designed to showcase the iconic U.S.S. Enterprise model in augmented reality and wirelessly trigger sounds, lights, and effects on the Playmobil bridge. Unfortunately, the app has become unstable and crashes on startup.

This repository provides two replacement solutions:

1. **App Inventor Android App**: A simple MIT App Inventor project with big buttons and a custom background to send five‑byte command packets over BLE, to control your enterprise.  
2. **Home Assistant Integration**: Use an ESP32 flashed with ESPHome (`ble.yaml`) to expose each command as a momentary switch in Home Assistant.

---

## **📁 Repository Contents**

* `NCC_1701_Playmobil.aia`  
  MIT App Inventor project archive. Import into the web editor.  
* `background.psd`  
  Photoshop source for the app’s background. Edit and export as a PNG.  
* `ble.yaml`  
  ESPHome configuration for ESP32 to connect to the Playmobil bridge and expose Home Assistant switches.  
* `README.md`  
  This document.  
* `LICENSE`  
  MIT License.

---

## **🚀 Features**

### **App Inventor App**

* **Crash‑proof App**: No AR engine and no —just a simple button interface.  
* **Custom Background**: Use the included PSD for branding or theming.  
* **BLE Commands**: Leverages App Inventor’s BLE component to send raw 5‑byte packets.


### **Home Assistant Integration**

* **ESP32 as BLE Client**: Automatically scans and connects to the bridge.  
* **Momentary Switches**: Each command appears as a switch in HA, firing the packet and resetting instantly.  
* **Out‑of‑the‑box YAML**: Drop `ble.yaml` into your ESPHome projects.

---

## **🔧 Prerequisites**

### **App Inventor App**

* Android device (OS 5.0+) with BLE support.  
* MIT App Inventor account.

### **Home Assistant Integration**

* ESP32 development board (e.g. NodeMCU-32S).  
* Home Assistant with ESPHome addon or standalone ESPHome tool.  
* Wi‑Fi network credentials.

---

## **📥 Installation & Setup**

### **1\. App Inventor Replacement App**

1. Clone or download this repo.  
2. Open MIT App Inventor.  
3. **Projects → Import project (.aia)** → select `NCC_1701_Playmobil.aia`.  
4. **Media → Upload File** → add your exported PNG from `background.psd`.  
5. Connect via AI2 Companion or **Build** the APK:  
   * **Connect → AI2 Companion**, scan QR code.  
   * Or **Build → App (save .apk)** and sideload on your device.

### **2\. Home Assistant & ESPHome**

1. Install the ESPHome addon in Home Assistant or set up ESPHome CLI.  
2. Copy `ble.yaml` into your ESPHome configuration folder.

Update your Wi‑Fi and `!secret` entries:  
wifi:  
  ssid: \!secret wifi\_ssid  
  password: \!secret wifi\_password  
ota:

3.   password: \!secret esphome\_ota\_password  
4. (Optional) Pin by MAC instead of UUID by replacing `service_uuid` with `mac_address` under `ble_client`.  
5. Compile & upload to your ESP32:  
   * In HA: ESPHome → \+ → Upload.  
   * CLI: `esphome run ble.yaml`.

Once online, Home Assistant will discover a device named **NCC1701** with a series of momentary switches.

---

## **📱 Usage**

### **App Inventor App**

1. Launch the app on your Android device.  
2. it will automatically **SCAN BLE DEVICES**, then connect to your **“U.S.S. Enterprise”**.  
3. Tap any command button to trigger the effect on your Playmobil starship.

### **Home Assistant**

1. Go to your HA dashboard.  
2. Locate the **NCC1701** integrations or devices section.  
3. Each command (Warp Drive, Red Alert, etc.) appears as a switch.  
4. Toggle a switch to send that command and watch it automatically reset.

---

## **🛠️ Command & communication Reference**

Name: Pm_USSE_5D2EF

Service UUID: bc2f4cc6-aaef-4351-9034-d66268e328f0

Write Characteristic UUID: 06d1e5e7-79ad-4a71-8faa-373789f7d93c


Hex Payload:

1 Engage warp drive (Light & Audio)
0xAA 0x07 0x01 0x00 0xFF

2 Photon Torpedo (Light & Audio)
0xAA 0x07 0x03 0x00 0xFF

3 Red Alert (Light & Audio)
0xAA 0x07 0x02 0x00 0xFF

4 Toggle Power
0xAA 0x07 0x04 0x00 0xFF

Power on/off
5 Run Astrogator (Audio)
0xAA 0x08 0x03 0xC8 0xFF

6 PhotonTorpedo Fire (Audio) 
0xAA 0x08 0x05 0xC8 0xFF

7 Bridge Ambient (Audio)
0xAA 0x08 0x01 0xC8 0xFF

8 Boatswain’s whistle (Audio)
0xAA 0x08 0x02 0xC8 0xFF

9 Red Alert (Audio)
0xAA 0x08 0x07 0xC8 0xFF

10 Dilithium Core Removed (Audio)
0xAA 0x08 0x04 0xC8 0xFF

11 This Is Captain James Kirk (Audio)
0xAA 0x08 0x06 0xC8 0xFF

12 Enter Warp Drive (Audio)
0xAA 0x08 0x0A 0xC8 0xFF

13 Live Long and Prosper (Audio)
0xAA 0x08 0x08 0xC8 0xFF

14 Dilithium Core Restored (Audio)
0xAA 0x08 0x09 0xC8 0xFF

15 Dematerialization (Audio)
0xAA 0x08 0x0B 0xC8 0xFF

16 Alert (Lights)
0xAA 0x04 0x02 0xC8 0xFF

17 Console Lights (Lights)
0xAA 0x04 0x01 0xC8 0xFF

18 Photon Torpedo Full (Lights)
0xAA 0x04 0x03 0xC8 0xFF

19 Dilithium Core (Lights)
0xAA 0x05 0x02 0xC8 0xFF

20 Photon Torpedo Twice (Lights)
0xAA 0x05 0x01 0xC8 0xFF

21 Warp Nacelles (Lights)
0xAA 0x04 0x07 0xC8 0xFF

22 Nacelles Warp Jump (Lights)
0xAA 0x05 0x07 0xC8 0xFF



---

## **📄 License**

This project is licensed under the **MIT License**. See LICENSE for details.

---

*Live long and prosper\!* 🖖

