# **Playmobil Enterprise BLE Controller**

A replacement for the official **Playmobil Star Trek Enterprise AR** app—which has been crashing on launch—and an integration guide for Home Assistant. Use your Android phone or an ESP32 to control the U.S.S. Enterprise bridge via BLE with one-tap buttons or Home Assistant switches.

---

## **📖 Background**

The original Playmobil AR app (com.playmobil.ar.startrekenterprise) was designed to showcase the iconic U.S.S. Enterprise model in augmented reality and wirelessly trigger sounds, lights, and effects on the Playmobil bridge. Unfortunately, the app has become unstable and crashes on startup.

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

## **🛠️ Command Reference**

All commands use the packet structure:

\[0xAA, FRAME, COMMAND, PARAMETER, 0xFF\]

#
Command Name
Hex Payload
Bytes Array
Description
1
Warpdrive
AA070100FF
0xAA 0x07 0x01 0x00 0xFF
Engage warp drive
2
Photon Torpedo
AA070300FF
0xAA 0x07 0x03 0x00 0xFF
Fire photon torpedo
3
Alert
AA070200FF
0xAA 0x07 0x02 0x00 0xFF
Red alert
4
Toggle Power
AA070400FF
0xAA 0x07 0x04 0x00 0xFF
Power on/off
5
Run Astrogator
AA0803C8FF
0xAA 0x08 0x03 0xC8 0xFF
Run astrogator sequence
6
PhotonTorpedo Fire
AA0805C8FF
0xAA 0x08 0x05 0xC8 0xFF
“Photon torpedo fire” audio/FX
7
Bridge Ambient
AA0801C8FF
0xAA 0x08 0x01 0xC8 0xFF
Bridge ambient lighting/sound
8
Boatswain Whistle
AA0802C8FF
0xAA 0x08 0x02 0xC8 0xFF
Boatswain’s whistle
9
Red Alert
AA0807C8FF
0xAA 0x08 0x07 0xC8 0xFF
Siren and red-flash
10
Dilithium Core Removed
AA0804C8FF
0xAA 0x08 0x04 0xC8 0xFF
“Dilithium core removed” alert
11
This Is Captain James Kirk
AA0806C8FF
0xAA 0x08 0x06 0xC8 0xFF
“This is James T. Kirk” audio
12
Enter Warp Drive
AA080AC8FF
0xAA 0x08 0x0A 0xC8 0xFF
“Engage warp drive” confirmation
13
Live Long and Prosper
AA0808C8FF
0xAA 0x08 0x08 0xC8 0xFF
“Live long and prosper” audio
14
Dilithium Core Restored
AA0809C8FF
0xAA 0x08 0x09 0xC8 0xFF
Dilithium core restored alert
15
Dematerialization
AA080BC8FF
0xAA 0x08 0x0B 0xC8 0xFF
Transporter dematerialization FX
16
Alert (panel)
AA0402C8FF
0xAA 0x04 0x02 0xC8 0xFF
Secondary panel alert flash
17
Console Lights
AA0401C8FF
0xAA 0x04 0x01 0xC8 0xFF
Console light sequence
18
Photon Torpedo Full
AA0403C8FF
0xAA 0x04 0x03 0xC8 0xFF
Full-power torpedo charging lights
19
Dilithium Core
AA0502C8FF
0xAA 0x05 0x02 0xC8 0xFF
Dilithium core pulsing
20
Photon Torpedo Twice
AA0501C8FF
0xAA 0x05 0x01 0xC8 0xFF
Double torpedo firing sequence
21
Warp Nacelles
AA0407C8FF
0xAA 0x04 0x07 0xC8 0xFF
Warp nacelles glow/pulse
22
Nacelles Warp Jump
AA0507C8FF
0xAA 0x05 0x07 0xC8 0xFF
Nacelle warp-jump animation



Command
Hex Code
Decimal Bytes

WARP DRIVE 
AA 07 01 00 FF
[170, 7, 1, 0, 255]
PHOTON TORPEDO 
AA 07 03 00 FF
[170, 7, 3, 0, 255]
ALERT 
AA 07 02 00 FF
[170, 7, 2, 0, 255]
ON/OFF (toggle) 
AA 07 04 00 FF
[170, 7, 4, 0, 255]
RUN ASTROGATOR
AA 08 03 C8 FF
[170, 8, 3, 200, 255]
PHOTON TORPEDO FIRE 
AA 08 05 C8 FF
[170, 8, 5, 200, 255]
BRIDGE AMBIENT 
AA 08 01 C8 FF
[170, 8, 1, 200, 255]
BOATSWAIN WHISTLE 
AA 08 02 C8 FF
[170, 8, 2, 200, 255]
RED ALERT
AA 08 07 C8 FF
[170, 8, 7, 200, 255]
DILITHIUMCORE REMOVED 
AA 08 04 C8 FF
[170, 8, 4, 200, 255]
“CAPTAIN JAMES KIRK” 
AA 08 06 C8 FF
[170, 8, 6, 200, 255]
ENTER WARP DRIVE 
AA 08 0A C8 FF
[170, 8, 10, 200, 255]
LIVE LONG AND PROSPER 
AA 08 08 C8 FF
[170, 8, 8, 200, 255]
DILITHIUMCORE RESTORED 
AA 08 09 C8 FF
[170, 8, 9, 200, 255]
DEMATERIALIZATION 
AA 08 0B C8 FF
[170, 8, 11, 200, 255]
ALERT (red) 
AA 04 02 C8 FF
[170, 4, 2, 200, 255]
CONSOLE LIGHTS 
AA 04 01 C8 FF
[170, 4, 1, 200, 255]
PHOTON TORPEDO (full) 
AA 04 03 C8 FF
[170, 4, 3, 200, 255]
DILITHIUMCORE 
AA 05 02 C8 FF
[170, 5, 2, 200, 255]
WARP NACELLES 
AA 04 07 C8 FF
[170, 4, 7, 200, 255]
PHOTON TORPEDO TWICE 
AA 05 01 C8 FF
[170, 5, 1, 200, 255]
NACELLES WARP JUMP 
AA 05 07 C8 FF
[170, 5, 7, 200, 255]



---

## **📄 License**

This project is licensed under the **MIT License**. See LICENSE for details.

---

*Live long and prosper\!* 🖖

