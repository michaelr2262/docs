---
description: Network-wide FiveM server protection — one ban list, every server.
---

# SentinelFX

<figure><img src="sentinelfx-logo-dark.svg" alt="SentinelFX" width="360"></figure>

SentinelFX is a cross-server Discord protection bot for FiveM communities. When a bad actor is reported and confirmed in one server, they are automatically banned from every server in the network — with no manual action required. An optional in-game resource extends the same ban list into FiveM itself, so one decision blocks the player on Discord and in every FXServer that runs the resource.

SentinelFX is a single hosted bot. You add it to your Discord server like any other bot — there is nothing to host, install, or run yourself. Configuration happens entirely through Discord slash commands and the web dashboard.

---

## What SentinelFX Does

<table data-view="cards">
<thead><tr><th></th><th></th></tr></thead>
<tbody>
<tr><td><img src="sfx_network.svg" width="48"><br><strong>Network Bans</strong></td><td>One approved ban deploys across every connected server simultaneously.</td></tr>
<tr><td><img src="sfx_shield.svg" width="48"><br><strong>Auto-Moderation</strong></td><td>Spam, scam links, mass mentions, and banned words handled in real time.</td></tr>
<tr><td><img src="sfx_reported.svg" width="48"><br><strong>Smart Triage</strong></td><td>Reports are filtered automatically so SentinelFX staff focus on the cases that genuinely need a human.</td></tr>
<tr><td><img src="sfx_watchdog.svg" width="48"><br><strong>Watchdog</strong></td><td>Scheduled sweeps catch any banned user who slipped through.</td></tr>
<tr><td><img src="sfx_verified.svg" width="48"><br><strong>Verification</strong></td><td>Math CAPTCHA on join blocks bot accounts before they reach your members.</td></tr>
<tr><td><img src="sfx_alert.svg" width="48"><br><strong>Anti-Raid</strong></td><td>Mass-join detection triggers automatic lockdown with a one-click unlock.</td></tr>
<tr><td><img src="sfx_scanning.svg" width="48"><br><strong>Threat Scoring</strong></td><td>Every user gets a live risk score based on their cross-network history.</td></tr>
<tr><td><img src="sfx_online.svg" width="48"><br><strong>FiveM Bridge</strong></td><td>Mirror in-game bans into the network and live-monitor your FXServer from the dashboard.</td></tr>
<tr><td><img src="sfx_banned.svg" width="48"><br><strong>HWID Evasion Guard</strong></td><td>Hardware fingerprints catch alts even when the player changes every other identifier.</td></tr>
<tr><td><img src="sfx_appeal.svg" width="48"><br><strong>Real Appeals</strong></td><td>Threaded appeal conversations with human-only review.</td></tr>
</tbody>
</table>

---

## Three-Layer Defence

SentinelFX never relies on a single check. Every network-banned user faces **three independent layers**:

| Layer | When it fires | What it does |
|-------|--------------|--------------|
| **Deploy-time** | The moment a ban is approved | Immediately bans the user from every server they're currently in |
| **Join-time** | When a user joins any member server | Checks the ban list and bans automatically if matched |
| **Sweep-time** | Every six hours | Scans all member servers for banned users that were missed |

With the FiveM resource installed, these same bans are enforced on connect to your FXServer — matched against every identifier the player uses (Rockstar licence, Steam, Xbox Live, Discord, Cfx.re, and hardware tokens).

---

## Quick Links

{% content-ref url="getting-started.md" %}
Getting Started
{% endcontent-ref %}

{% content-ref url="network-protection.md" %}
How the Network Works
{% endcontent-ref %}

{% content-ref url="fivem.md" %}
FiveM Integration
{% endcontent-ref %}

{% content-ref url="commands/README.md" %}
Commands Reference
{% endcontent-ref %}

{% content-ref url="configuration.md" %}
Server Configuration
{% endcontent-ref %}
