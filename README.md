# 🧙‍♂️ THIS BLUEPRINT IS NOT READY FOR USER TESTING. 
  Moonraker Printer Discord & Alexa Notifications (Home Assistant Automation)

## ✅ Features
- 🖼️ Snapshot images posted to Discord if available (based on slicer’s g-code setup)
- 🧵 Filament + ETA + Layers shown during print progress
- 🎤 Alexa voice announcements only during your defined hours
- 🔁 Based on your printer's state: start, pause, resume, completion, and percentage updates every 5%
- 🔄 Support for dynamic sensor names via `printer_base`

## 🔧 Setup Instructions

### 1. ✅ Rename Your Printer in Home Assistant (Important!)
To make setup dead simple, we build all sensor names dynamically from one `printer_base` value. But this only works if your printer’s entities follow a consistent naming pattern.

Here’s how to set that up:

1. Go to `Settings > Devices & Services`
2. Find your **Moonraker Printer** integration
3. Click on the `1 Device` link under it
4. Click the ✏️ pencil icon (top right corner)
5. Type in a name like `Neptune Max` (or whatever)
6. Click **Update**
7. ✅ Choose **"Rename all entities to match"**

After this, your printer’s entities will look like:
- `sensor.neptune_max_current_print_state`
- `camera.neptune_max_thumbnail`
- `sensor.neptune_max_filename`

And so on.

You’ll only need to specify one value: `printer_base: neptune_max` and we’ll build all the rest from that.

---

### 2. 🔥 Shell Command Setup (For Discord Webhook)
We **cannot** put the Discord webhook directly in the automation due to escaping/quote issues with long URLs. Instead, define it in your `shell_commands.yaml`.

If you don’t have that file yet, create it under your config folder.

Then add this:

printer_notify_webhook: >
  curl -X POST -F "payload_json={\"content\": \"{{ message }}\"}" -F "file=@/config/www/3dprinter_snapshot.jpg" https://discord.com/api/webhooks/YOUR_WEBHOOK_HERE

Replace `YOUR_WEBHOOK_HERE` with the actual webhook URL from Discord.

Make sure your `configuration.yaml` includes:

shell_command: !include shell_commands.yaml

---

### 3. 🎙️ Alexa Announcements
In the automation YAML, you'll set this:

alexa_target: notify.alexa_media_your_device_here

You can find your Alexa device notify name in Developer Tools > Services.

You can also control **when** Alexa is allowed to talk with:

alexa_start_time: "09:30:00"
alexa_end_time: "22:30:00"

Outside of those hours, the automation will still post to Discord but stay silent on Alexa.

---

## 🧩 Variables You Set In The YAML
At the top of the automation YAML, you'll find this block:

variables:
  printer_base: neptune_max
  printer_name: Neptune Max
  alexa_target: notify.alexa_media_echoclockdot
  alexa_start_time: "09:30:00"
  alexa_end_time: "22:30:00"

Just change those values, and the rest builds automatically.

---

## ✅ Automation Features
- 🖼️ Snapshot images posted to Discord
- 🧵 Filament + ETA + Layers shown during print progress
- 🎤 Alexa voice announcements only during your defined hours
- 🔁 Based on your printer's state: start, pause, resume, complete, and % updates every 5%

---

## 🚧 Still To Do
- Add optional persistent Home Assistant notifications
- Support multiple printers with friendly names in a blueprint version

---

### 🤘 Written with rage, weed, beer, and loud metal music by a rebel automation nerd.

“Monkies code better than AI” — @YouKnowWhoYouAre 😎
