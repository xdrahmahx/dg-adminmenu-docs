# dg-adminmenu Documentation

Welcome to the official documentation for the dg-adminmenu FiveM admin script.

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [FAQ](#faq)
- [Contributing](#contributing)
- [Support](#support)

## Installation
To install dg-adminmenu on your FiveM server:

1. Ensure you have oxmysql installed and started before this resource.
2. Place the dg-adminmenu resource in your resources/[dgscripts]/ folder:
	- Example: `resources/[dgscripts]/dg-adminmenu`
3. In your `server.cfg`, add the following lines in this order:
	```
	ensure oxmysql
	ensure [qb]    # or ensure [es_extended] if using ESX
	ensure [dgscripts]
	ensure dg-adminmenu
	```
4. Configure `config.lua` for Discord integration and detection settings (see below for required/optional config options).
5. Restart your server or run `restart dg-adminmenu` in the console.

### Configuration (config.lua)
Required for Discord forum threads:
- `discordBotToken`
- `discordForumChannelId`

Optional:
- `discordWebhookUrl` (admin actions / screenshots)
- `enableSteamProfileFetch` + `steamApiKey`

Detection toggles:
- `detectionEnabled`
- `menuDetectionEnabled`
- `antiNukeEnabled`

Admin action:
- `autoKickNukeAttempt`

See the main README in the script folder for full configuration and permissions details.

## Usage
How to use the admin menu, commands, permissions, and configuration options.

## FAQ
Frequently asked questions and troubleshooting tips.

## Contributing
Guidelines for contributing to this project. Pull requests are welcome!

## Support
See SUPPORT.md for help and contact information.
