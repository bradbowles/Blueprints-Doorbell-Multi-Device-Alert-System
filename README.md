# Doorbell – Multi‑Device Alert System (Home Assistant Blueprint)
This Home Assistant blueprint sends a snapshot and alerts to multiple devices when your smart doorbell is triggered.

## Features
📸 Snapshot capture from your doorbell camera<br>
🔔 Optional chime sound before announcement<br>
🗣 Sonos TTS announcement<br>
📱 Phone notification with snapshot<br>
📺 TV notifications (Chromecast / Google TV / Cast-enabled TVs)<br>
🖼 Optional PiP‑style overlay using a custom HTML page<br>
🎛 Only notifies TVs that are currently ON<br>

## 📥 Import Blueprint
Click below to import directly into Home Assistant:

Import Blueprint (my.home-assistant.io in Bing)

## 📂 Installation
1. Copy the PiP overlay file<br>
Place pip.html into your Home Assistant: /config/www/pip.html<br>
This enables the optional picture‑in‑picture overlay on Chromecast / Google TV.

2. Reload Home Assistant
Go to: Settings → Developer Tools → YAML → Reload Templates & Blueprints

3. Create a new automation
Go to: Settings → Automations & Scenes → Create Automation → From Blueprint  
Select: Doorbell – Multi‑Device Alert System

## 🧰 Blueprint Inputs
You will configure:<br>
Doorbell trigger (binary sensor)<br>
Doorbell camera<br>
Snapshot switch<br>
Sonos media group<br>
Phone notify service<br>
TV group (all TVs / Chromecasts)<br>
TTS engine<br>
Optional chime file<br>
Optional PiP overlay<br>

## 📄 Files in this Repository

| File | Purpose |
| --- | --- |
| ``doorbell_multi_device_alert.yaml`` | The Home Assistant blueprint |
| ``pip.html`` | HTML overlay for PiP-style TV notifications |
| ``README.md`` | Documentation |
| ``LICENSE`` | MIT License |
