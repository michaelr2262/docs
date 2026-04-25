---
description: View and manage your server's protection at sentinelfx.net.
---

# Web Dashboard

<figure><img src="sfx_network.svg" alt="Dashboard" width="64"></figure>

The SentinelFX Dashboard at **[sentinelfx.net](https://sentinelfx.net)** gives moderators and admins a full view of their server's protection — bans, moderation logs, active modules, and configuration — all from the browser.

---

## Logging In

Visit [sentinelfx.net](https://sentinelfx.net) and click **Login with Discord**. The dashboard uses Discord OAuth — no separate account is needed. Your access level is based on your roles in the connected server.

{% hint style="info" %}
You must be a moderator or administrator in a SentinelFX member server to access the dashboard.
{% endhint %}

---

## What You Can See

<figure><img src="sfx_shield.svg" alt="Shield" width="48"></figure>

Once logged in, select your server from the sidebar. You'll have access to the following sections:

| Section | Description |
|---------|-------------|
| **Overview** | Server name, member count, active modules, and current protection status |
| **Network Bans** | Full list of network-banned users and whether they've been removed from your server |
| **Recent Actions** | Log of recent moderation actions — bans, kicks, mutes, and warns |
| **Configuration** | View your current module toggles, channels, roles, and thresholds |
| **FiveM** | Live status, charts, and review tools for any FXServers you've registered |

---

## Network Bans View

<figure><img src="sfx_banned.svg" alt="Banned" width="48"></figure>

The **Network Bans** section shows every user on the SentinelFX ban list alongside their status in your server:

- Whether they were present when the ban was deployed
- Whether they've since attempted to join and been blocked
- The ban reason, category, and date

---

## Recent Actions

<figure><img src="sfx_reported.svg" alt="Log" width="48"></figure>

The **Recent Actions** log mirrors your mod-log channel in the dashboard. Every ban, kick, mute, warn, and purge carried out by SentinelFX in your server is recorded here with the responsible moderator, reason, and timestamp.

---

## Configuration View

<figure><img src="sfx_scanning.svg" alt="Config" width="48"></figure>

The **Configuration** section shows your server's current SentinelFX settings at a glance:

- Which modules are enabled (Anti-Raid, Auto-Mod, Verification, Network Bans)
- Your configured channels (mod log, network alerts, raid alerts, welcome)
- Your configured roles (mod, admin, verified)
- Your current thresholds (warn escalation, spam limits, raid sensitivity)

{% hint style="info" %}
Configuration changes are made via the Discord `/config` command. The dashboard shows your current settings as a read-only reference.
{% endhint %}

---

## FiveM Server View

<figure><img src="sfx_online.svg" alt="FiveM" width="48"></figure>

If you've registered one or more FXServers with SentinelFX, each one gets its own live view inside the dashboard. This is the operator's window into what's happening in-game.

### Live Status

At the top of the page:

- **Uptime** — calculated from the resource's heartbeat. A server is considered offline after a few minutes of silence.
- **Player count** — current connected players, plus peak over the selected range.
- **Heartbeat indicator** — green while the resource is talking back, red the moment it goes quiet.

### Range Selector

Every chart on the page respects a shared range toggle:

| Range | Typical use |
|-------|------------|
| 1 hour | Right-now triage — is something happening this minute? |
| 6 hours | Evening session overview |
| 24 hours | Daily rhythm |
| 7 days | Weekly patterns |
| 30 days | Long-term trends |

### Event Histogram

A bar chart of all in-game events (connects, disconnects, bans, chat flags) bucketed across the selected range. Useful for spotting raid waves, cheat-scan bursts, or unusual off-hours activity.

### Recent Events & Missed Pings

A live-updating event stream shows the last few events along with any missed heartbeats, marked in red so you can tell the difference between a quiet server and a disconnected one.

### Evasion Alerts

When the resource spots a hardware fingerprint already tied to a banned user, the evasion alert panel surfaces it with the matched account and the new identity.

### Identity Ledger

Click any player to see every identifier SentinelFX has linked to them — Discord, Rockstar licences, Steam, Xbox Live, Cfx.re account, hardware tokens. This is the full picture of who a player is across every platform they've touched.

{% hint style="info" %}
Ban mode for each FXServer can be changed directly from this page. The change takes effect immediately — no restart, no resource update.
{% endhint %}

---

## Dashboard URL

| | |
|-|-|
| Main site | [sentinelfx.net](https://sentinelfx.net) |
| Dashboard | [sentinelfx.net/dashboard](https://sentinelfx.net/dashboard) |
