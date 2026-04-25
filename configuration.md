---
description: Full reference for every configurable setting in SentinelFX.
---

# Server Configuration

<figure><img src="sfx_shield.svg" alt="Config" width="64"></figure>

Every SentinelFX setting is stored per-server. Nothing is shared between servers except the network ban list itself. Use the `/config view` command at any time to see the current state of your configuration.

---

## Viewing Your Config

The `/config view` command returns a single embed showing all your current settings — which modules are on, which channels are set, which roles are configured, and what your thresholds are.

---

## Modules

Enable or disable each module independently using `/config toggle`.

| Module | Default | Description |
|--------|---------|-------------|
| Anti-Raid | Off | Automatic raid detection and server lockdown |
| Auto-Mod | Off | Real-time message monitoring and violation response |
| Verification | Off | Math CAPTCHA for every new join |
| Auto-apply Bans | Off | Auto-apply and auto-enforce network bans |

{% hint style="info" %}
Auto-apply Bans must be enabled for your server to receive ban deployments and Watchdog sweeps. If it's off, your server is still on the network list but bans won't auto-apply.
{% endhint %}

---

## Channels

Tell SentinelFX where to post its messages using `/config set-channel`.

| Channel | When it's used |
|------|---------------|
| Mod log channel | Every moderation action: bans, kicks, mutes, warns, purges, slowmode changes |
| Network ban channel | Network ban deployment notifications — when a banned user is removed from your server |
| Raid alert channel | Anti-raid lockdown alerts and the unlock button |
| Welcome channel | Fallback for verification challenges if a user's DMs are closed |

{% hint style="warning" %}
If the mod log channel is not set, moderation actions are still applied — they just won't be logged anywhere you can see them. Always set a mod log channel.
{% endhint %}

---

## Roles

Tell SentinelFX who your staff are using `/config set-role`.

| Role | How it's used |
|------|--------------|
| Moderator role | Members with this role are exempt from Auto-Mod and can use moderation commands |
| Admin role | Additional admin-tier exemptions |
| Verified role | Granted to users who pass verification |

---

## Thresholds

Fine-tune the numeric limits that drive automatic behaviour using `/config thresholds`.

### Warning Escalation

| Setting | Default | Description |
|--------|---------|-------------|
| Warn-mute threshold | 3 | Total warnings before auto-mute (1 hour) |
| Warn-kick threshold | 5 | Total warnings before auto-kick |
| Warn-ban threshold | 7 | Total warnings before auto-ban |

### Auto-Mod Sensitivity

| Setting | Default | Description |
|--------|---------|-------------|
| Spam limit | 5 | Messages per 5-second window before spam fires |
| Mention limit | 5 | User or role mentions per message before mass-mention fires |

### Anti-Raid Sensitivity

| Setting | Default | Description |
|--------|---------|-------------|
| Raid threshold | 10 | Joins within the window before lockdown triggers |
| Raid window | 10 | Seconds to use as the join counting window |

---

## Recommended Starting Setup

For a typical FiveM community server:

1. Turn on Anti-Raid, Auto-Mod, Verification, and Auto-apply Bans.
2. Point the mod log, network ban, raid alert, and welcome channels at the channels you want them in.
3. Assign your moderator, admin, and verified roles.
4. Leave the thresholds at their defaults to start; you can tune them later if you find a category too sensitive or too lax.
5. Run `/network-sync` once to pull every existing network ban into your server.

From that point on, SentinelFX runs in the background — you don't need to touch it again unless you want to adjust thresholds or change which channel something logs to.
