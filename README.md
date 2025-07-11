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
* **YAML**: Adjust the name of your Enterprise model (should start with Pm_USSE_) in the ble.yaml file. Drop `ble.yaml` into your ESPHome projects.

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


## **🛠️ Command & Communication Reference**

Name starts with: `Pm_USSE_`

**Service UUID:** `bc2f4cc6-aaef-4351-9034-d66268e328f0`  
**Write Characteristic UUID:** `06d1e5e7-79ad-4a71-8faa-373789f7d93c`

🔧 Hex Payload Commands – Sorted by Category & Code
| #  | Command Name                          | Hex Payload   | Description                             | Type   |
|----|---------------------------------------|---------------|-----------------------------------------|--------|
| **Audio Only**                             |               |                                         |        |
| 01 | Bridge Ambient                        | AA0801C8FF    | Subtle bridge hum                       | Audio  |
| 02 | Boatswain’s Whistle                   | AA0802C8FF    | Signal whistle                          | Audio  |
| 03 | Run Astrogator                        | AA0803C8FF    | Navigation scanner sound                | Audio  |
| 04 | Dilithium Core Removed                | AA0804C8FF    | Voice line                              | Audio  |
| 05 | Fire Photon Torpedo (Audio Only)      | AA0805C8FF    | Torpedo fire sound                      | Audio  |
| 06 | This is Captain Kirk                  | AA0806C8FF    | Voice line                              | Audio  |
| 07 | Red Alert Siren                       | AA0807C8FF    | Audio-only red alert                    | Audio  |
| 08 | Live Long and Prosper                 | AA0808C8FF    | Voice line                              | Audio  |
| 09 | Dilithium Core Restored               | AA0809C8FF    | Voice line                              | Audio  |
| 10 | Enter Warp Drive (Confirmation)       | AA080AC8FF    | Warp drive confirmation sound           | Audio  |
| 11 | Dematerialization (Transporter)       | AA080BC8FF    | Transporter sound                       | Audio  |
| **Static Lights**                          |               |                                         |        |
| 12 | Console Lights (Static)               | AA0601C8FF    | Bridge consoles blue (solid)            | Light  |
| 13 | Console Lights OFF                    | AA060100FF    | Bridge console lights Off               | Light  |
| 14 | Red Alert (Static)                    | AA0602C8FF    | Solid red alert lights                  | Light  |
| 15 | Red Alert OFF                         | AA060200FF    | Turns off red alert                     | Light  |
| 16 | Photon Torpedo Light (Static)         | AA0603C8FF    | Solid torpedo indicator                 | Light  |
| 17 | Photon Torpedo Light OFF              | AA060300FF    | Torpedo light Off                       | Light  |
| 18 | Side Console (Blue Static)            | AA0604C8FF    | Warp core console – blue light          | Light  |
| 19 | Side Console (Blue OFF)               | AA060400FF    | Blue side console Off                   | Light  |
| 20 | Side Console (Red Static)             | AA0605C8FF    | Warp core console – red light           | Light  |
| 21 | Side Console (Red OFF)                | AA060500FF    | Red side console Off                    | Light  |
| 22 | Dilithium Core (Static)               | AA0606C8FF    | Solid core chamber light                | Light  |
| 23 | Dilithium Core OFF                    | AA060600FF    | Turns off core light                    | Light  |
| 24 | Warp Nacelles (Static)                | AA0607C8FF    | Solid nacelle glow                      | Light  |
| **Pulsing / Blinking Lights**              |               |                                         |        |
| 25 | Console Lights (Pulsing)              | AA0401C8FF    | Blinking bridge consoles                | Light  |
| 26 | Console Lights OFF                    | AA040100FF    | Bridge console pulsing Off              | Light  |
| 27 | Red Alert (Pulsing)                   | AA0402C8FF    | Flashing red alert light                | Light  |
| 28 | Red Alert OFF                         | AA040200FF    | Turns off red alert light               | Light  |
| 29 | Photon Torpedo Light (Pulsing)        | AA0403C8FF    | Torpedo charging/blinking               | Light  |
| 30 | Photon Torpedo Light OFF              | AA040300FF    | Torpedo light Off                       | Light  |
| 31 | Side Console (Red Blinking)           | AA0404C8FF    | Flashing red side light (core)          | Light  |
| 32 | Dilithium Core (Pulsing)              | AA0406C8FF    | Glowing core chamber                    | Light  |
| 33 | Warp Nacelles (Pulsing)               | AA0407C8FF    | Animated nacelle glow                   | Light  |
| **Special Effects (Light Only)**           |               |                                         |        |
| 34 | Photon Torpedo x2                     | AA0501C8FF    | Double torpedo light flash              | Light  |
| 35 | Nacelle Warp Jump                     | AA0507C8FF    | Nacelle jump lighting                   | Light  |
| **Mixed Light + Audio Effects**            |               |                                         |        |
| 36 | Red Alert Combo                       | AA070200FF    | Flashing lights + red alert siren       | Mixed  |
| 37 | Photon Torpedo Combo                  | AA070300FF    | Torpedo fire with sound + light         | Mixed  |
| 38 | Warp Drive Combo                      | AA0701FFFF    | Warp lighting + confirmation audio      | Mixed  |
| **Utility / System**                       |               |                                         |        |
| 39 | Toggle Power / Reset                  | AA070400FF    | Turn off all lights & Audio (reset)     | Other  |



Brightness & Volume Controll

Structure of a BLE Command
Each BLE command is 5 bytes long, formatted as:

AA XX YY ZZ FF
Or in full hex:

0xAA 0xXX 0xYY 0xZZ 0xFF
Byte-by-byte explanation:
Byte	Position	Meaning	Example (AA0603C8FF)
AA	Byte 1	Constant prefix (start byte)	AA = All commands start with this
XX	Byte 2	Command Category / Group	06 = Static light group
YY	Byte 3	Specific action/device	03 = Photon torpedo light
ZZ	Byte 4	Intensity/Volume/Setting	C8 = Brightness 200/255 (~90%)
FF	Byte 5	End byte / confirmation / checksum*	FF = Always ends with this

 Brightness / Volume Intensity Lookup Table
% Level	Decimal	Hex (4th Byte)	Example Command (Static Torpedo Light)
10%	25	19	AA060319FF ← Static torpedo @ 10%
20%	51	33	AA060333FF ← Static torpedo @ 20%
25%	64	40	AA060340FF ← Static torpedo @ 25%
30%	77	4D	AA06034DFF ← Static torpedo @ 30%
40%	102	66	AA060366FF ← Static torpedo @ 40%
50%	128	80	AA060380FF ← Static torpedo @ 50%
60%	153	99	AA060399FF ← Static torpedo @ 60%
70%	179	B3	AA0603B3FF ← Static torpedo @ 70%
75%	192	C0	AA0603C0FF ← Static torpedo @ 75%
80%	204	CC	AA0603CCFF ← Static torpedo @ 80%
90%	230	E6	AA0603E6FF ← Static torpedo @ 90%
100%	255	FF	AA0603FFFF ← Static torpedo @ 100%



Example: Static Photon Torpedo Light
Command Purpose	Hex Payload	Brightness Level
50% Brightness	AA060380FF	Medium
App-default Brightness	AA0603C8FF	 
Full Overdrive Brightness	AA0603FFFF	Ultra Bright




---

## **📄 License**

This project is licensed under the **MIT License**. See LICENSE for details.

---

*Live long and prosper\!* 🖖

