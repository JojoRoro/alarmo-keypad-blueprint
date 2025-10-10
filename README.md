# Frient Keypad with Alarm Control Panel (Alarmo Integration)
A Home Assistant blueprint for Alarmo keypad integration via MQTT — manage all codes directly in Alarmo.”

**Author:** Neliss, based on blueprint by AndrejDelany  
**Modified:** DopeHead200 with ChatGPT
**Updated:** October 8, 2025  
**Compatible with:** Home Assistant 2024.6+  
**License:** MIT  
** Fully Tested with Frient intelligent keypad KEPZB-110

---

## 🧩 Overview

This Home Assistant blueprint integrates the hardware keypads, especially tested with **Frient Zigbee Keypad** with the **Alarmo** alarm control system.  
Unlike older versions, this blueprint no longer requires PIN codes or RFID tags to be managed inside the blueprint itself — all user codes and tags are handled **directly by Alarmo**.
The keypad only sends actions and codes to Alarmo, simplifying configuration and keeping all security data centralized.

---

## 🚀 Features

- Direct integration between Frient Keypad and Alarmo
- PIN and RFID management handled entirely in Alarmo
- Supports all alarm states:
  - Arm Away
  - Arm Home (Day)
  - Arm Night
  - Disarm
  - Entry Delay
  - Exit Delay
  - Alarm Trigger
  - Panic
- Customizable actions for each Alarmo state
- Works with **Zigbee2MQTT**

---

## 🛠 Requirements

- **Home Assistant 2024.6.0** or later  
- **Alarmo** installed and configured  
- **Zigbee2MQTT** integration with your Frient Keypad  
- MQTT topics for both state and set communication

---

## 📦 Installation

1. Copy the YAML file into your Home Assistant configuration directory:
      config/blueprints/automation/johnny/frient_keypad_alarmo.yaml
   How to do it: you can do it manually using Studio Code Server (creating subdirectory under blueprints/automation/johnny and creating empty file frient_keypad_alarmo_modified.yaml and then pasting the code.
2. Restart Home Assistant.

3. Go to Settings → Automations & Scenes → Blueprints → Import Blueprint
and select this blueprint URL to import it.

4. Create a new automation from the blueprint.

## 🚀 Direct Import
Click below to import this blueprint directly into Home Assistant:
[![Import Blueprint](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?url=https://raw.githubusercontent.com/DopeHead200/alarmo-keypad-blueprint/main/blueprints/automation/dopehead/frient-keypad-alarmo.yaml)

⚙️ Configuration

When creating an automation from this blueprint, you’ll be prompted to set:

Input	Description	Example
MQTT State Topic	Zigbee2MQTT topic for keypad state updates	zigbee2mqtt/Keypad
MQTT Set Topic	Zigbee2MQTT topic for commands to keypad	zigbee2mqtt/Keypad/set
Control Panel	Select your Alarmo control panel entity	alarm_control_panel.alarmo
(optional) Actions	Custom actions for each Alarmo state	e.g. play a sound, send a notification
🔄 How It Works
1. Alarmo → Keypad

When Alarmo changes its state, the keypad receives an MQTT message updating its LED or buzzer status to reflect:

arming
armed (away/home/night)
disarmed
triggered
pending entry/exit

2. Keypad → Alarmo
When a user enters a code or uses an RFID tag:
The keypad sends an MQTT message with action and action_code
The blueprint forwards the request directly to Alarmo using:
alarm_control_panel.alarm_arm_away
alarm_control_panel.alarm_arm_home
alarm_control_panel.alarm_arm_night
alarm_control_panel.alarm_disarm

3. Code Verification

Alarmo handles the verification of the entered code or tag.
The blueprint does not store or check any credentials — improving both security and maintenance simplicity.

🧠 Tips

Manage user codes and RFID tags only in the Alarmo interface.
Ensure the MQTT topics match exactly your Zigbee2MQTT configuration.
You can use the optional action_... inputs to trigger lights, sounds, or notifications for each state change.

🧩 Example MQTT Messages

From keypad (user action):
{
  "action": "arm_all_zones",
  "action_code": "1234"
}

From Alarmo (state update):
{
  "arm_mode": { "mode": "arm_all_zones" }
}

📜 Version History
Version	Date	Notes
1.0.0	2025-10-08	Initial public release with Alarmo-based PIN/RFID management
🛡 License

This project is licensed under the MIT License
.

❤️ Credits

Based on the original Frient Keypad integration blueprint by the Home Assistant community - nelis and AndrejDelany.
Modified by DopeHead to use Alarmo for centralized credential management.
