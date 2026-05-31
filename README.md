# Doorbell – Multi Device Alert System

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

### 🌐 Dynamic Snapshot URL
- New **base_url** input ensures snapshots work across:
  - Local IPs  
  - Hostnames  
  - SSL setups  
  - Reverse proxies  
  - Nabu Casa  
- Automatically appends a **cache‑buster** to prevent stale images

### 📸 Snapshot Handling
- Triggers your snapshot switch  
- Waits for the camera to process  
- Saves a snapshot to `/config/www/temp/doorbell_snapshot.jpg`  
- Ensures the file is ready before notifications fire  

### 🔊 Speaker Announcement
- Optional speaker announcement  
- Optional chime before TTS with configurable chime duration  
- Single‑shot TTS (no double‑plays)  
- Temporary volume override + automatic restore  
- Safe volume restore when speakers are off or unavailable

### 📱 Phone Notifications
- Optional mobile notification  
- Supports **multiple devices** via comma-separated notify services  
- Each device notified independently (one failure won't block others)  
- Optional snapshot attachment  
- Fully templated title and message  
- Uses dynamic URL with cache‑buster

### 📺 TV Overlay Notification
- Optional TV overlay  
- Optional snapshot image  
- Adjustable display duration  
- VLAN‑safe image URL  
- Uses the **TvOverlay** app  
- Safe `continue_on_error` handling

### ⏸ Pause Logic
Use any Home Assistant condition to temporarily disable the automation.

---

## 🧩 Inputs Overview

### Base URL
A required input used to generate the dynamic snapshot URL.

**Example:**  
```
http://homeassistant.local:8123
```

If your Home Assistant instance uses a different hostname, IP, port, or SSL, enter that instead.

### Triggers
- Doorbell ding sensor  
- Motion sensor  
- Person detection sensor  

### Snapshot
- Snapshot trigger switch  
- Camera entity  

### Speaker
- Enable/disable  
- Media players  
- TTS engine  
- Message  
- Volume  
- Optional chime + chime duration  

### Phone
- Enable/disable  
- Notify service(s) — comma-separated for multiple devices  
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

---

## 🛠 Requirements

### 1. Snapshot Directory
Ensure this directory exists:

```
/config/www/temp/
```

### 2. Base URL
Set the **Base URL** input to match your Home Assistant instance.

Examples:
```
http://homeassistant.local:8123
http://192.168.20.10:8123
https://ha.example.com
```

### 3. TvOverlay REST Command
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

### 4. Firewall (UniFi / VLAN Users)
If your TV is on a different VLAN than Home Assistant, ensure:

- TV VLAN → HA IP (port 8123) is allowed  
- No "Drop Inter‑VLAN" rule is above your allow rule  
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

## 🔄 Updating the Blueprint

Home Assistant does **not** automatically notify you when a blueprint is updated upstream. To stay up to date:

- **Watch this repository** on GitHub (top right → Watch → Custom → Releases) to get notified when a new version is published.
- When a new version is available, re-import using the one-click button or RAW URL above. Home Assistant will prompt you to confirm the update.
- Check the [CHANGELOG](./CHANGELOG.md) for what changed before updating.

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
- Uses single‑shot TTS  
- Includes safe volume restore and error‑tolerant playback  

### Speaker volume error when speakers are off
- Safe volume restore handles unavailable speakers gracefully  
- Falls back to 0.35 if volume level cannot be read  

### Phone notification missing image
- Ensure `/config/www/temp/` exists  
- Ensure the snapshot switch is firing  
- Ensure your phone notify service supports images  
- Ensure **Base URL** is correct  

### Only one phone receiving notifications
- Ensure notify services are comma-separated in the Phone notify service(s) field  
- Example: `notify.mobile_app_johns_phone, notify.mobile_app_janes_phone`

---

## 📝 Changelog

See [CHANGELOG.md](./CHANGELOG.md) for the full version history.

---

## 📄 License

This project is licensed under the MIT License.  
See the `LICENSE` file for details.
