# DG - System Admin Panel
**Advanced AI Anti-Cheat Detection System with Admin Panel**

Version: 1.2.0  
Framework Support: QBCore, ESX, Standalone  
Author: DG-Scripts  
License: All Rights Reserved (See LICENSE.txt)

---

## 🎯 Overview

**dg-adminpanel** is a comprehensive anti-cheat detection and admin management system for FiveM servers. It combines intelligent client-side and server-side detection with a powerful admin panel for investigating and managing suspicious players.

### Key Features

✅ **Multi-Framework Support** - Works seamlessly with QBCore, ESX, and Standalone  
✅ **Intelligent Detection System** - AI-powered cheat detection with weighted scoring  
✅ **Real-time Admin Panel** - Beautiful NUI for managing players and investigations  
✅ **Discord Integration** - Automatic forum thread creation for each player's activity  
✅ **Framework-Agnostic Commands** - Commands work on all supported frameworks  
✅ **No False Positives** - Context-aware detection with built-in roleplay tolerance  
✅ **Auto-Ban System** - Configure automatic bans for critical cheats (optional)  
✅ **Crash Protection** - Detects and blocks server nuke attempts  

---

## 📋 Requirements
1) Ensure oxmysql is installed and started.
2) Place the resource in resources/[dgscripts]/dg-adminpanel.
3) Add to server.cfg in this order:
   ensure oxmysql
   ensure [qb] or ensure [es_extended] (if used)
   ensure [dgscripts]
4) Configure config.lua (see below).
5) Restart the resource: restart dg-adminpanel

Configuration (config.lua)
Required for Discord forum threads:
- discordBotToken
- discordForumChannelId

Optional:
- discordWebhookUrl (admin actions / screenshots)
- enableSteamProfileFetch + steamApiKey

Detection toggles:
- detectionEnabled
- menuDetectionEnabled
- antiNukeEnabled

Admin action:
- autoKickNukeAttempt

Permissions
QB-Core:
- Uses QBCore permissions (admin/god). No ACE required.

ESX or Standalone:
- Add ACE permissions in server.cfg:
  add_ace group.admin command.dg-admin allow
  add_ace group.admin command.clearsuspicion allow
  add_ace group.admin command.checksuspicion allow
  add_ace group.admin command.anticheatpanel allow

Commands
- /anticheatpanel
- /clearsuspicion [id]
- /checksuspicion [id]

Admin panel
- Shows live suspicion scores and logs.
- Actions: warn, kick, ban, teleport, bring, freeze, revive, spectate.
- Discord thread is created on first detection when the bot is configured.

Database tables (auto-created)
- dg_aicheatdetect_logs
- dg_aicheatdetect_bans
- dg_aicheatdetect_playerinfo
- dg_discord_threads

Install checks (quick verification)
1) Server console shows the resource started without errors.
2) MySQL tables exist (see list above).
3) Run /anticheatpanel as an admin and confirm UI opens.
4) Trigger a test detection (e.g., speed threshold) and verify:
   - Log entry is created in dg_aicheatdetect_logs.
   - Discord thread is created (if bot configured).

Common issues
- Discord threads not created:
  - Check bot token and forum channel ID.
  - Ensure bot has Create Posts permission in the forum channel.
- Admin panel access denied:
  - Check QBCore permissions or ACE permissions.
- No detections:
  - Confirm detectionEnabled = true in config.lua.
  - Check that client/main.lua is loaded in fxmanifest.lua.

Security notes
- Never share your bot token, webhook URL, or database credentials.
- Use separate Discord channels for anti-cheat logs.

Changelog
- 1.2.0
  - Tebex cleanup and documentation refresh.
  - Removed unused snapshot feature and test commands.
  - Console output reduced for production use.

License
- Copyright (c) DG-Scripts.
- All rights reserved.
- This script is licensed for use by the purchaser only.
- Redistribution, resale, or re-upload is not permitted.
- For support, contact DG-Scripts.

Support
- For issues, provide console logs and config.lua values (redact tokens).
blacklistedWeapons = {
    "WEAPON_RAILGUN",
    "WEAPON_MINIGUN",
    "WEAPON_RPG",
    "WEAPON_HOMINGLAUNCHER"
}

**Blacklisted Vehicles:**
```lua
blacklistedVehicles = {
    "RHINO",  -- Tank
    "HYDRA",  -- Military jet
    "LAZER",  -- Military jet
    "OPPRESSOR2"  -- MK2
}
```

**Blacklisted Props** (server crash objects):
```lua
blacklistedProps = {
    "prop_air_biohazard_01",
    "prop_container_ld_pu_closed"
    -- See config.lua for full list
}
```

### Anti-Nuke Settings

```lua
antiNukeEnabled = true
autoDeleteBlacklistedProps = true  -- Auto-delete dangerous props
propCheckInterval = 1000  -- Check every second
maxPropsInWorld = 500  -- Max props before flagging
entitySpawnRateLimit = 50  -- Max entities/second per player
nukeDetectionThreshold = 100  -- Flag if >100 entities in 5s
autoKickNukeAttempt = true  -- Auto-kick nuke attempts
```

---

## 🎮 Admin Commands

All commands require admin permissions (defined in `server/main.lua`).

### Detection Management

| Command | Description | Example |
|---------|-------------|---------|
| `/clearsuspicion [playerID]` | Reset player's score | `/clearsuspicion 5` |

### Admin Panel

| Keybind | Action |
|---------|--------|
| **F6** | Open/close admin panel |

**Panel Features:**
- View all flagged players
- See real-time detection alerts
- Kick/ban players with custom reasons
- Trigger admin vote for player removal
- Clear suspicion scores

---

## 🤖 AI Suggestion System

### How It Works

The AI system tracks **repeated identical detections** per player. After the configured threshold (default: 5), it posts a suggestion to the player's Discord thread.

### Example Suggestions

**Speed Hack False Positive:**
```
🔍 Pattern Detected
Player repeatedly flagged for "speed_hack" (5 times)

💭 Possible Cause
Player may be using custom vehicles with high speed stats, or legitimate speed boosts

⚙️ Recommended Config Changes
-- In config.lua, increase speed threshold:
speedThreshold = 30.0,  -- Current: 25.0
speedViolationsRequired = 4,  -- Current: 3

🔄 Alternative Actions
- Whitelist specific vehicle models with high speeds
- Check for server-side speed boost resources
```

**Godmode During Events:**
```
🔍 Pattern Detected
Player repeatedly flagged for "godmode" (5 times)

💭 Possible Cause
Player may be participating in server events with temporary invincibility

⚙️ Recommended Config Changes
-- In config.lua, adjust godmode thresholds:
godmodeViolationsRequired = 5,  -- Current: 3
godmodeHealthThreshold = 250,  -- Current: 200

🔄 Alternative Actions
- Add event-specific protection periods in server events
- Whitelist players during active events
```

### Configuration

```lua
enableAISuggestions = true  -- Enable/disable system
repeatedDetectionThreshold = 5  -- Suggestions after X detections
```

---

## 💾 Database Structure

### dg_cheat_detections
Stores individual detection events.

| Column | Type | Description |
|--------|------|-------------|
| `id` | INT | Auto-increment primary key |
| `license` | VARCHAR(100) | Player's FiveM license |
| `player_name` | VARCHAR(100) | Player name at time of detection |
| `detection_type` | VARCHAR(50) | Type of cheat detected |
| `suspicion_score` | INT | Current total score |
| `details` | TEXT | JSON details about detection |
| `timestamp` | TIMESTAMP | When detection occurred |

### dg_player_suspicion
Tracks current suspicion scores.

| Column | Type | Description |
|--------|------|-------------|
| `license` | VARCHAR(100) | Primary key |
| `player_name` | VARCHAR(100) | Current player name |
| `suspicion_score` | INT | Total suspicion score |
| `last_updated` | TIMESTAMP | Last score update |

### dg_discord_threads
Maps players to Discord threads.

| Column | Type | Description |
|--------|------|-------------|
| `license` | VARCHAR(100) | Primary key |
| `thread_id` | VARCHAR(50) | Discord thread/post ID |
| `player_name` | VARCHAR(100) | Player name |
| `created_at` | TIMESTAMP | Thread creation time |

### dg_admin_actions
Logs admin actions (kicks/bans/votes).

| Column | Type | Description |
|--------|------|-------------|
| `id` | INT | Auto-increment primary key |
| `admin_license` | VARCHAR(100) | Admin's license |
| `admin_name` | VARCHAR(100) | Admin name |
| `target_license` | VARCHAR(100) | Target player's license |
| `target_name` | VARCHAR(100) | Target player name |
| `action_type` | VARCHAR(50) | kick/ban/vote |
| `reason` | TEXT | Action reason |
| `timestamp` | TIMESTAMP | When action occurred |

---

## 🔧 Troubleshooting

### Discord Issues

**❌ Threads not being created**

1. Verify bot token is correct in `config.lua`
2. Ensure bot has "Create Posts" permission in forum channel
3. Check that channel ID is for a **Forum** channel, not text channel
4. Check server console for error messages (HTTP status codes)
5. Verify Discord bot is online in your server
6. Ensure firewall isn't blocking Discord API connections

**❌ Duplicate threads being created**

- This was fixed in v1.1.0. Update to latest version.
- Manually archive/delete duplicate threads in Discord if needed

**❌ Bot offline or not responding**

1. Verify bot is online in Discord (green status)
2. Check bot has proper permissions in server roles
3. Check firewall isn't blocking Discord API (discord.com)
4. Verify your bot token hasn't expired or been reset

### Detection Issues

**❌ Too many false positives**

1. Increase detection thresholds in `config.lua`
2. Increase `violationsRequired` for specific detection types
3. Enable AI suggestions to get automatic recommendations
4. Increase protection periods (`spawnProtectionDuration`, etc.)

**❌ Detections not triggering**

1. Ensure `detectionEnabled = true` in config
2. Check client console (F8) for errors
3. Verify framework detection in server console
4. Check that detection intervals aren't too high
5. Ensure `violationsRequired` thresholds are reachable

**❌ Admin panel (F6) not opening**

1. Check browser console (F8) for JavaScript errors
2. Verify `html/` files are present
3. Ensure NUI is not disabled in FiveM settings
4. Try different keybind if F6 conflicts

### Performance Issues

**❌ Server lag/frame drops**

1. Increase check intervals in config (e.g., `speedCheckInterval = 2000`)
2. Disable unused detection types
3. Reduce scan distances (`blacklistedPropsDistance`, etc.)
4. Increase object/vehicle spawn check intervals
5. Set `blacklistedPropsCheckInterval = 10000` (currently disabled by default)

**❌ Database filling up**

1. Periodically clean old detections:
   ```sql
   DELETE FROM dg_cheat_detections WHERE timestamp < DATE_SUB(NOW(), INTERVAL 30 DAY);
   ```
2. Add to scheduled task for automatic cleanup

---

## 🚀 Performance Optimization

### Recommended Settings for Different Server Sizes

**Small Server (< 32 players):**
```lua
-- Keep default intervals
speedCheckInterval = 1500
godmodeCheckInterval = 2000
```

**Medium Server (32-64 players):**
```lua
-- Slightly increase intervals
speedCheckInterval = 2000
godmodeCheckInterval = 2500
weaponCheckInterval = 6000
vehicleCheckInterval = 6000
```

**Large Server (64+ players):**
```lua
-- Increase all intervals
speedCheckInterval = 2500
godmodeCheckInterval = 3000
weaponCheckInterval = 8000
vehicleCheckInterval = 8000
blacklistedPropsCheckInterval = 15000
vehicleSpawnSpamCheckInterval = 12000
objectSpawnSpamCheckInterval = 18000
```

### Disabling Specific Detections

If certain detections cause issues on your server:

```lua
-- In server/main.lua, comment out detection initializers:
-- Citizen.CreateThread(function() speedHackDetection() end)
```

Or increase thresholds to effectively disable:
```lua
speedViolationsRequired = 999  -- Effectively disabled
```

---

## ❓ FAQ

**Q: Does this script ban players automatically?**  
A: No. It only detects, scores, and reports. Admins must manually review and take action.

**Q: Is this compatible with [other anti-cheat]?**  
A: Yes, generally. This script focuses on detection and logging, not blocking. May cause duplicate alerts.

**Q: Can players see their suspicion score?**  
A: No, only admins can see scores via the F6 panel.

**Q: How do I whitelist admins from detection?**  
A: Add admin identifiers to whitelist in `server/main.lua` (see code comments).

**Q: Does Discord integration require a paid bot?**  
A: No, Discord bots are free. No verification required for server-only bots.

**Q: Can I use this without Discord integration?**  
A: Yes, leave `discordBotToken` empty. Detections will only log to database and console.

**Q: What's the difference between webhook and bot?**  
A: Bot creates forum threads (per-player). Webhook posts to single channel (admin logs only).

**Q: How do I add custom detection types?**  
A: See `server/main.lua` for detection structure. Add new detection functions and call `addSuspicion()`.

**Q: Is the AI system actual machine learning?**  
A: No, it's pattern recognition. "AI" refers to intelligent suggestion logic, not ML models.

**Q: Can I use this on a non-English server?**  
A: Yes, but Discord embeds and console messages are in English. Translation support not yet implemented.

---

## 📝 License

**Proprietary License**  
This script is provided for use on your FiveM server. Redistribution, resale, or modification for commercial purposes is prohibited.

---

## 🆘 Support

**Issues:** Check [Troubleshooting](#troubleshooting) section first  
**Discord:** https://discord.gg/KBU3Kt7uSJ 
**Documentation:** This README

---

## 📊 Changelog

### Version 1.1.0 (Current)
- ✅ Discord forum thread integration
- ✅ AI suggestion system for false positives
- ✅ Fixed duplicate thread creation
- ✅ Removed IP addresses from logs (privacy)
- ✅ Clean embed formatting with emojis
- ✅ Per-player detection tracking
- ✅ Admin panel improvements
- ✅ Performance optimizations

### Version 1.0.0
- Initial release
- Basic detection systems
- Database logging
- Admin commands

---

**Made with ❤️ for the FiveM community**
