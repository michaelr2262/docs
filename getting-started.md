---
description: Add SentinelFX to your server and get protected in minutes.
---

# Getting Started

<figure><img src="sfx_shield.svg" alt="Shield" width="64"></figure>

SentinelFX is a single hosted bot. There's nothing to install on your machine — you simply add it to your Discord server and configure it from inside Discord and the dashboard.

---

## Step 1 — Invite the Bot

Use the official invite link from [sentinelfx.net](https://sentinelfx.net) to add SentinelFX to your Discord server. Grant it the permissions it requests during the invite — these are required for enforcement actions to work.

{% hint style="warning" %}
SentinelFX needs **Ban Members**, **Moderate Members**, **Manage Channels**, and **Send Messages** at minimum. Without these, enforcement actions will silently fail.
{% endhint %}

---

## Step 2 — Run First-Time Config

Once the bot is in your server, use the `/config view` command to see the current state of your configuration. Everything starts disabled or unset until you turn it on.

You'll see a panel showing which modules are active and which channels and roles are configured.

---

## Step 3 — Set Your Channels and Roles

Tell SentinelFX where to send its alerts and who your staff are. These are configured through the `/config set-channel` and `/config set-role` commands.

| Channel | Purpose |
|---------|---------|
| Mod log channel | All moderation actions (bans, kicks, mutes, warns) |
| Network ban channel | Notifications when a network ban is deployed to your server |
| Raid alert channel | Anti-raid lockdown notifications and unlock button |
| Welcome channel | Where verification challenges are sent if DMs are closed |

| Role | Purpose |
|------|---------|
| Moderator role | Members exempt from auto-mod; can use moderation commands |
| Admin role | Additional admin-tier exemptions |
| Verified role | Granted to users who pass verification |

---

## Step 4 — Enable Modules

Turn on the protections you want using `/config toggle`.

| Module | What it does |
|--------|-------------|
| Anti-Raid | Detects mass joins and auto-locks the server |
| Auto-Mod | Catches spam, scam links, banned words in real time |
| Verification | Sends a math CAPTCHA to every new join |
| Auto-apply Bans | Auto-applies network bans when a user joins or is swept |

---

## Step 5 — Sync Existing Bans

If you've just joined the network, pull all existing bans down immediately with the `/network-sync` command. This applies every active network ban to your server's Discord ban list in one shot. From that point forward, new bans apply automatically.

---

## Step 6 — You're Protected

<figure><img src="sfx_online.svg" alt="Online" width="48"></figure>

Your server is now part of the SentinelFX network. Any user banned across the network will be removed from your server — automatically, at the moment of approval, when they join, and during every sweep.

{% hint style="success" %}
**Tip:** Open the [Web Dashboard](dashboard.md) at any time to see how many servers are in the network, how many active bans are in effect, and live stats for your own server.
{% endhint %}

---

## Step 7 — Connect Your FiveM Server (optional)

<figure><img src="sfx_network.svg" alt="FiveM" width="48"></figure>

If your community runs FiveM, you can extend the same network ban list into your FXServer:

1. Open the **FiveM** tab in the dashboard and register your server.
2. Pick a **ban mode** — review is the default and recommended.
3. Download the resource from Tebex.
4. Paste the per-server config values from the dashboard into the resource's config file.
5. Add the resource to your `server.cfg` and restart.

Within a couple of minutes the dashboard will show a green heartbeat and your server is live on the network.

{% content-ref url="fivem-resource.md" %}
Installing the Resource
{% endcontent-ref %}

{% content-ref url="fivem.md" %}
FiveM Integration Overview
{% endcontent-ref %}
