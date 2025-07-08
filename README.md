# **Playmobil Enterprise BLE Controller**

![file_00000000ba1c61f4959f70e35bb5e774](https://github.com/user-attachments/assets/58fd951f-fb68-4574-a117-a1bb356548e5)





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

Name starts with: Pm_USSE_

Service UUID: bc2f4cc6-aaef-4351-9034-d66268e328f0

Write Characteristic UUID: 06d1e5e7-79ad-4a71-8faa-373789f7d93c


Hex Payload:

| #   | Command Name                    | Hex Payload   | Decimal Bytes           | Description                    | Type   |
|-----|---------------------------------|------------ --|----------------------- -|--------------------------------|--------|
| 1   | Engage Warp Drive (Light&Audio) | AA0701FFFF    | [170, 7, 1, 255, 255]  | Alternate Warp Start            | Mixed  |
| 2   | Photon Torpedo (Light&Audio)    | AA070300FF    | [170, 7, 3, 0, 255]    | Torpedo Trigger (Audio + Light) | Mixed  |
| 3   | Red Alert (Light&Audio)         | AA070200FF    | [170, 7, 2, 0, 255]    | Audio + Light Alert Trigger     | Mixed  |
| 4   | Toggle Power                    | AA070400FF    | [170, 7, 4, 0, 255]    | General Light Reset / Off       | Other  |
| 5   | Run Astrogator (Audio)          | AA0803C8FF    | [170, 8, 3, 200, 255]  | Nav Sensor Scan                 | Audio  |
| 6   | Photon Torpedo Fire (Audio)     | AA0805C8FF    | [170, 8, 5, 200, 255]  | Photon Torpedo Fire (Audio)     | Audio  |
| 7   | Bridge Ambient (Audio)          | AA0801C8FF    | [170, 8, 1, 200, 255]  | Low Hum + Light                 | Audio  |
| 8   | Boatswain’s Whistle (Audio)     | AA0802C8FF    | [170, 8, 2, 200, 255]  | Signal Whistle                  | Audio  |
| 9   | Red Alert (Audio)               | AA0807C8FF    | [170, 8, 7, 200, 255]  | Red Alert Siren                 | Audio  |
| 10  | Dilithium Core Removed (Audio)  | AA0804C8FF    | [170, 8, 4, 200, 255]  | Voice Line                      | Audio  |
| 11  | This Is Captain James Kirk(Audio)| AA0806C8FF   | [170, 8, 6, 200, 255]  | Audio Only                      | Audio  |
| 12  | Enter Warp Drive (Audio)        | AA080AC8FF    | [170, 8, 10, 200, 255] | Confirmation Audio              | Audio  |
| 13  | Live Long and Prosper (Audio)   | AA0808C8FF    | [170, 8, 8, 200, 255]  | Audio Only                      | Audio  |
| 14  | Dilithium Core Restored (Audio) | AA0809C8FF    | [170, 8, 9, 200, 255]  | Voice Line                      | Audio  |
| 15  | Dematerialization (Audio)       | AA080BC8FF    | [170, 8, 11, 200, 255] | Transporter Sound               | Audio  |
| 16  | Red Alert (Pulsing Lights)      | AA0402C8FF    | [170, 4, 2, 200, 255]  | Flashing Red Alert Lights       | Light  |
| 17  | Console Lights (Pulsing)        | AA0401C8FF    | [170, 4, 1, 200, 255]  | Bridge / Eng. Panel Blinking    | Light  |
| 18  | Photon Torpedo Full (Lights)    | AA0403C8FF    | [170, 4, 3, 200, 255]  | Full-Power Torpedo Charging     | Light  |
| 19  | Dilithium Core (Pulsing)        | AA0406C8FF    | [170, 4, 6, 200, 255]  | Glowing Chamber Animation       | Light  |
| 20  | Photon Torpedo Twice (Lights)   | AA0501C8FF    | [170, 5, 1, 200, 255]  | Double Torpedo Firing           | Light  |
| 21  | Warp Nacelles (Pulsing)         | AA0407C8FF    | [170, 4, 7, 200, 255]  | Nacelles Glow                   | Light  |
| 22  | Nacelle Warp Jump (Lights)      | AA0507C8FF    | [170, 5, 7, 200, 255]  | Jump Effect Lighting            | Light  |
| 23  | Red Alert (Static Lights)       | AA0602C8FF    | [170, 6, 2, 200, 255]  | Non-blinking Red Alert Lights   | Light  |
| 24  | Console Lights (Static)         | AA0601C8FF    | [170, 6, 1, 200, 255]  | Bridge / Eng. Panel Solid       | Light  |
| 25  | Dilithium Core (Static)         | AA0606C8FF    | [170, 6, 6, 200, 255]  | Fixed Chamber Illumination      | Light  |
| 26  | Warp Nacelles (Static)          | AA0607C8FF    | [170, 6, 7, 200, 255]  | Solid Nacelle Lighting          | Light  |


---

## **📄 License**

This project is licensed under the **MIT License**. See LICENSE for details.

---

*Live long and prosper\!* 🖖

