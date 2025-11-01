# 📘 RealParking – Script Description

## 🧩 Overview
**RealParking** is an automatic parking system that detects vehicle ownership tables in the database and impounds all vehicles left outside on server restart.  
It supports **ox_inventory**, **ox_target**, and both **English and Hungarian** languages.

The system works fully independently and automatically adapts to any vehicle shop system without manual configuration.

---

## ⚙️ Main Features

- 🅿️ Configurable parking spots (default test grid included, expandable manually)  
- 🚗 Automatic impounding on server restart  
- 💳 Buy parking tickets from an NPC (using bank or cash)  
- 📡 Discord logging for every event (park, retrieve, impound, etc.)  
- 🌍 Dual language support (Hungarian / English)  
- 🧠 Auto-detects vehicle ownership table – no manual SQL setup required  
- 🧍 NPC interaction with ox_target or “E” key  
- ⚙️ Fully customizable via config.lua  

---

## 📦 Requirements

- `es_extended` (ESX)  
- `oxmysql`  
- `ox_inventory`  
- *(optional)* `ox_target`  
- *(optional)* `ox_lib`  

---

## 🛠️ Installation

1. Copy the **realparking** folder into your resources directory.
2. Add to ox_inventory parking_ticket.png  
3. Add this to your **server.cfg**:
   ```cfg
   ensure oxmysql
   ensure es_extended
   ensure ox_inventory
   ensure realparking
4. Open config.lua and set up:
Language (hu or en)
Payment method
NPC locations and coordinates
Discord webhook URL
5. Restart your server.
🧠 Auto Table Detection

When starting, the script automatically scans your database to detect which table contains vehicle data.
It looks for columns such as:

plate

owner or citizenid

vehicle, vehicle_props, or mods

If it finds a match, it uses that table automatically.
If any RealParking-specific columns (stored, impounded, realparking_active) are missing, they are added automatically.
If no matching table is found, the script creates its own owned_vehicles table.

📡 Discord Logging
If enabled, Discord logs will show:
✅ Park → Green
🚗 Retrieve → Yellow
🚓 Impound → Red
🔵 Redeem → Blue

Logs may appear with a slight 1–2 second delay — this is normal.

🧾 Database Fallback Table

If no vehicle table is detected, RealParking will create the following fallback table:

CREATE TABLE IF NOT EXISTS `owned_vehicles` (
  `owner` VARCHAR(60) NOT NULL,
  `plate` VARCHAR(12) NOT NULL,
  `vehicle` LONGTEXT DEFAULT NULL,
  `stored` TINYINT(1) DEFAULT 1,
  `impounded` TINYINT(1) DEFAULT 0,
  `realparking_active` TINYINT(1) DEFAULT 0,
  PRIMARY KEY (`plate`)
);

🧱 Notes & Behavior

On the first server start, SQL warnings or errors may appear in the console.
This is normal — the script checks and adds any missing columns automatically.
After a second restart, no more SQL errors should appear.

When vehicles are impounded on the first restart,
the console will display the total number of vehicles impounded.
On the next immediate restart (within milliseconds),
it will show 0 impounded vehicles, which is expected.

Other players cannot take, move, or interact with parked vehicles.
They also cannot finish parking someone else’s vehicle.

If you need to delete a vehicle because a player has been inactive for a long time or no longer uses it,
you must do it manually in the database.
In this case, the vehicle is completely removed and will not appear among impounded vehicles.
You can delete it from the realparking_spots table using the vehicle’s plate.

⚠️ Important:
When deleting a vehicle from realparking_spots,
make sure to also remove it from the owned_vehicles table manually
to avoid leaving unused data behind.

🅿️ RealParking does not require any base garage system (like esx_vehicleshop or esx_garage).
It handles parking, storing, and impounding independently.
The base garage’s vehicle states do not affect RealParking.

🚔 Note for Police Impounds:
If the police impound a vehicle through another system (for example, via a police menu or command),
the vehicle will disappear from the map, as police impounding is not integrated with RealParking.
Only RealParking’s own impound logic handles vehicles persistently in the database.

🧾 The parking ticket requires an image file named parking_ticket.png,
which is included in the default files.
You can replace it with any design,
but the file name must remain parking_ticket.png.

🔒 Obfuscation / Encryption

The required files are encrypted.
If you encounter any issues or need assistance, join the Discord server,
where you can request help directly:

👉 Discord: https://discord.gg/dpFeSwfm3x

💬 Summary

RealParking is a fully automated parking system
that impounds all vehicles left outside after a server restart,
auto-detects database tables,
and offers a realistic parking experience for players.
