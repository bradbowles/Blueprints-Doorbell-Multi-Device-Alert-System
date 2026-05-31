# Doorbell – Multi Device Alert System (v2.1.2)

A highly configurable Home Assistant blueprint that unifies doorbell events, motion detection, and person detection into a single automation. It captures a snapshot, sends optional speaker announcements, phone notifications, and TV overlay alerts — all with clean timing, VLAN-safe image delivery, and robust logic.

This blueprint is vendor‑agnostic and works with a wide range of cameras, doorbells, speakers, and TV devices.

---

## ✨ Features

### 🔔 Multi‑Trigger Support
Trigger the automation using any combination of:
- Doorbell ding  
- Motion detection  
- Person detection  

Each trigger is optional and selectable in the UI.

### 📸 Snapshot Handling
- Triggers your snapshot switch  
- Waits for the camera to process  
- Saves a snapshot to `/config/www/temp/doorbell_snapshot.jpg`  
- Ensures the file is ready before notifications fire  

### 🔊 Speaker Announcement
- Optional speaker announcement  
- Optional chime before TTS  
- Single‑shot TTS (no double‑plays)  
- Temporary volume override + automatic restore  

### 📱 Phone Notifications
- Optional mobile notification  
- Optional snapshot attachment  
- Fully templated title and message  

### 📺 TV Overlay Notification
- Optional TV overlay  
- Optional snapshot image  
- Adjustable display duration  
- VLAN‑safe image URL  
- Uses the **TvOverlay** app (no PiP HTML required)

### ⏸ Pause Logic
Use any Home Assistant condition to temporarily disable the automation.

---

## 🧩 Inputs Overview

### Triggers
- **Doorbell ding sensor**
- **Motion sensor**
- **Person detection sensor**

### Snapshot
- Snapshot trigger switch  
- Camera entity  

### Speaker
- Enable/disable  
- Media players  
- TTS engine  
- Message  
- Volume  
- Optional chime  

### Phone
- Enable/disable  
- Notify service  
- Title + message  
- Include snapshot  

### TV Overlay
- Enable/disable  
- Caption  
- Include snapshot  
- Display duration (seconds)  

---

## 🧪 Tested With

This blueprint has been tested and verified working with:

- **Arlo Video Doorbell Gen 2** (Aarlo integration)  
- **Sonos** multi‑room speakers  
- **Google TV / Chromecast** running the **TvOverlay** app  
- **UniFi Network** (UDM‑SE, VLAN segmentation, inter‑VLAN firewalling)  
- **Home Assistant OS** running on **Proxmox**

These devices were used during development, but the blueprint is not limited to them.

---

## 🌐 Compatibility

This blueprint is designed to be vendor‑agnostic and should work with any Home Assistant setup that provides:

- A binary sensor for ding, motion, or person detection  
- A camera entity capable of snapshots  
- Optional `media_player` entities for announcements  
- Optional `notify.*` services for phone alerts  
- Optional REST endpoint for TV overlay (TvOverlay or similar)

Although tested with Arlo, Sonos, UniFi, and Google TV, the blueprint is expected to work with a wide range of doorbells, cameras, speakers, and TV devices supported by Home Assistant.

---

## 🛠 Requirements

### 1. Snapshot Directory
Ensure this directory exists:

```
/config/www/temp/
```

### 2. TvOverlay REST Command
Add this to `configuration.yaml`:

```yaml
rest_command:
  tvoverlay_doorbell:
    url: "http://<YOUR_OVERLAY_SERVER_IP>:5001/notify"
    method: POST
    content_type: "application/json"
    payload: >
      {
        "title": "{{ caption }}",
        "message": "",
        "image": "{{ image_path }}",
        "duration": {{ duration | default(10) }}
      }
```

Replace `<YOUR_OVERLAY_SERVER_IP>` with your overlay server address.

### 3. Firewall (UniFi / VLAN Users)
If your TV is on a different VLAN than Home Assistant, ensure:

- TV VLAN → HA IP (port 8123) is allowed  
- No “Drop Inter‑VLAN” rule is above your allow rule  
- The rule is placed in the correct **zone‑pair** (e.g., Media → HAOS)

---

## 📂 Installation

### Option 1 — One‑Click Import (Recommended)

Click the button below to import the blueprint directly into your Home Assistant:

[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](
https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https://raw.githubusercontent.com/bradbowles/Blueprints-Doorbell-Multi-Device-Alert-System/main/Blueprint/doorbell_multi_device_alert.yaml
)

---

### Option 2 — Manual Import via RAW URL

1. Copy this RAW URL:

```
https://raw.githubusercontent.com/bradbowles/Blueprints-Doorbell-Multi-Device-Alert-System/main/Blueprint/doorbell_multi_device_alert.yaml
```

2. In Home Assistant, go to:  
   **Settings → Automations & Scenes → Blueprints → Import Blueprint**

3. Paste the URL and click **Preview** → **Import**.

---

### Option 3 — Manual File Installation

1. Download the blueprint YAML file  
2. Place it in:

```
config/blueprints/automation/doorbell_multi_device_alert.yaml
```

3. Reload automations or restart Home Assistant.


---

## 🧪 Testing

Trigger any of the following:
- Doorbell ding  
- Motion detection  
- Person detection  

You should see:
- Snapshot saved  
- Speaker announcement (if enabled)  
- Phone notification (if enabled)  
- TV overlay (if enabled)  

---

## 🛠 Troubleshooting

### TV overlay shows text but no image
- Ensure the TV can reach the HA snapshot URL  
- Check UniFi firewall rules  
- Ensure the snapshot directory exists  
- Ensure TvOverlay is running on the Google TV device  

### Speaker TTS double‑plays
- v2.1.2 uses **single‑shot TTS** to eliminate this issue  
- If you still hear two announcements, check for duplicate automations  

### Phone notification missing image
- Ensure `/config/www/temp/` exists  
- Ensure the snapshot switch is firing  
- Ensure your phone notify service supports images  

---

## 📝 Changelog

### v2.1.2
- Added ding/motion/person multi-trigger support  
- Improved snapshot timing  
- Added adjustable TV display duration  
- Simplified TTS to single-shot  
- Cleaned variable handling  
- Improved VLAN compatibility for image delivery  
- Removed PiP overlay requirement (TvOverlay replaces it fully)

---

## 📄 License

This project is licensed under the MIT License.  
See the `LICENSE` file for details.
