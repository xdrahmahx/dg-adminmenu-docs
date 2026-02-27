# DG-AdminMenu: Advanced FiveM Admin & Anti-Cheat Panel

**Version:** 1.2.0  
**Frameworks:** QBCore, ESX, Standalone  
**Author:** DG-Scripts  
**License:** All Rights Reserved (See LICENSE.txt)

---

## 🚀 Overview

DG-AdminMenu is a powerful, AI-assisted anti-cheat and admin management system for FiveM servers. It features real-time detection, Discord integration, and a modern admin panel for seamless player management and investigation.

### ✨ Features
- Multi-framework support (QBCore, ESX, Standalone)
- AI-powered cheat detection with weighted scoring
- Real-time NUI admin panel (F6)
- Discord forum thread integration (per-player)
- Context-aware detection (minimizes false positives)
- Configurable auto-ban/auto-kick
- Crash/nuke protection
- Modular, easy-to-edit config

---

## 📦 Requirements
- **oxmysql** (must be installed and started)
- FiveM server (QBCore, ESX, or Standalone)
- Discord bot (for thread integration, optional)

---

## 🛠️ Installation
1. Ensure oxmysql is installed and started before this resource.
2. Place the dg-adminmenu resource in your `resources/[dgscripts]/` folder:
   - Example: `resources/[dgscripts]/dg-adminmenu`
3. In your `server.cfg`, add the following lines in this order:
   ```
   ensure oxmysql
   ensure [qb]    # or ensure [es_extended] if using ESX
   ensure [dgscripts]
   ensure dg-adminmenu
   ```
4. Configure `config.lua` for Discord integration and detection settings (see below).
5. Restart your server or run `restart dg-adminmenu` in the console.

---

## ⚙️ Configuration
Edit `config.lua` to customize detection, Discord, and admin options.

**Required for Discord threads:**
- `discordBotToken`
- `discordForumChannelId`

**Optional:**
- `discordWebhookUrl` (admin actions/screenshots)
- `enableSteamProfileFetch` + `steamApiKey`

**Detection toggles:**
- `detectionEnabled`, `menuDetectionEnabled`, `antiNukeEnabled`

**Admin action:**
- `autoKickNukeAttempt`

**Permissions:**
- QBCore: Uses QBCore permissions (admin/god)
- ESX/Standalone: Add ACE permissions in `server.cfg` (see docs)

---

## 🕹️ Usage

### Admin Panel
- **F6**: Open/close admin panel
- View flagged players, real-time alerts, kick/ban, teleport, freeze, revive, spectate, and more

### Commands
| Command | Description |
|---------|-------------|
| `/anticheatpanel` | Open admin panel |
| `/clearsuspicion [id]` | Reset player's suspicion score |
| `/checksuspicion [id]` | Check player's suspicion score |

### Discord Integration
- Automatic forum thread creation for each flagged player
- Admin actions and logs sent to Discord (if configured)

### Database
- Auto-creates tables for logs, bans, player info, and Discord threads

---

## 🧩 Example Config Snippet
```lua
discordBotToken = "YOUR_BOT_TOKEN"
discordForumChannelId = "YOUR_FORUM_CHANNEL_ID"
detectionEnabled = true
antiNukeEnabled = true
autoKickNukeAttempt = true
blacklistedWeapons = {"WEAPON_RAILGUN", "WEAPON_MINIGUN"}
```

---

## 🛡️ Troubleshooting
- **Panel not opening:** Check F8 console for JS errors, ensure html/ files are present
- **No Discord threads:** Check bot token, permissions, and channel type
- **No detections:** Ensure detectionEnabled = true, check config and fxmanifest
- **Server lag:** Increase check intervals, disable unused detections

---

## ❓ FAQ
- **Does this script auto-ban?** No, only if you enable auto-ban in config
- **Compatible with other anti-cheats?** Yes, but may duplicate alerts
- **Can players see their score?** No, only admins via F6 panel
- **How to whitelist admins?** Add identifiers in server/main.lua
- **Is Discord required?** No, but enables advanced features

---

## 🤝 Contributing
Pull requests and suggestions are welcome! For major changes, open an issue first.

---

## 🆘 Support
- For issues, open a GitHub issue or see SUPPORT.md
- For urgent help, email xdrahmahx@gmail.com
- Join our Discord (see docs) for community support

---

## 📜 License
All rights reserved. See LICENSE.txt. Redistribution, resale, or re-upload is not permitted.

---

**Made with ❤️ for the FiveM community by DG-Scripts**
