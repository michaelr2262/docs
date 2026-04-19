---
description: Add SentinelFX to your server and get protected in minutes.
---

# Getting Started

<figure><img src="sfx_shield.svg" alt="Shield" width="64"></figure>

## Step 1 — Invite the Bot

Use the official invite link from [sentinelfx.net](https://sentinelfx.net) to add SentinelFX to your Discord server. Grant it the permissions it requests — these are required for enforcement actions to work.

{% hint style="warning" %}
SentinelFX needs **Ban Members**, **Moderate Members**, **Manage Channels**, and **Send Messages** at minimum. Without these, enforcement actions will silently fail.
{% endhint %}

---

## Step 2 — Run First-Time Config

Once the bot is in your server, use `/config view` to see the current state of your configuration. Everything starts as disabled or unset.

```
/config view
```

You'll see a panel showing which modules are active and which channels/roles are configured.

---

## Step 3 — Set Your Channels and Roles

Tell SentinelFX where to send its alerts and who your staff are.

```
/config set-channel mod_log_channel #mod-log
/config set-channel network_ban_ch  #network-bans
/config set-channel raid_alert_ch   #raid-alerts
/config set-role mod_role       @Moderator
/config set-role admin_role     @Admin
/config set-role verified_role  @Verified
```

| Channel | Purpose |
|---------|---------|
| `mod_log_channel` | All moderation actions (bans, kicks, mutes, warns) |
| `network_ban_ch` | Notifications when a network ban is deployed to your server |
| `raid_alert_ch` | Anti-raid lockdown notifications and unlock button |
| `welcome_ch` | Where verification challenges are sent if DMs are closed |

---

## Step 4 — Enable Modules

Turn on the protections you want:

```
/config toggle anti-raid        true
/config toggle auto-mod         true
/config toggle verification     true
/config toggle auto-apply-bans  true
```

| Module | What it does |
|--------|-------------|
| `anti-raid` | Detects mass joins and auto-locks the server |
| `auto-mod` | Catches spam, scam links, banned words in real time |
| `verification` | Sends a math CAPTCHA to every new join |
| `auto-apply-bans` | Auto-applies network bans when a user joins or is swept |

---

## Step 5 — Sync Existing Bans

If you've just joined the network, pull all existing bans down immediately:

```
/network-sync
```

This applies every active network ban to your server's Discord ban list in one shot. From that point forward, new bans apply automatically.

---

## Step 6 — You're Protected

<figure><img src="sfx_online.svg" alt="Online" width="48"></figure>

Your server is now part of the SentinelFX network. Any user banned across the network will be removed from your server — automatically, at the moment of approval, when they join, and during every 6-hour sweep.

{% hint style="success" %}
**Tip:** Run `/network-status stats` at any time to see how many servers are in the network and how many active bans are in effect.
{% endhint %}

---

## Step 7 — Connect Your FiveM Server (optional)

<figure><img src="sfx_network.svg" alt="FiveM" width="48"></figure>

If your community runs FiveM, you can extend the same network ban list into your FXServer. The flow, at a glance:

1. Open the **FiveM** tab in the dashboard and register your server.
2. Pick a **ban mode** — `review` is the default and recommended.
3. Download the pre-baked resource zip.
4. Drop the folder into your FXServer's `resources/` directory.
5. Add `ensure sentinelfx` to your `server.cfg` and restart.

Within a couple of minutes the dashboard will show a green heartbeat and your server is live on the network.

{% content-ref url="fivem-resource.md" %}
Installing the Resource
{% endcontent-ref %}

{% content-ref url="fivem.md" %}
FiveM Integration Overview
{% endcontent-ref %}
