---
description: Full reference for every configurable setting in SentinelFX.
---

# Server Configuration

<figure><img src="sfx_shield.svg" alt="Config" width="64"></figure>

Every SentinelFX setting is stored per-server. Nothing is shared between servers except the network ban list itself. Use `/config view` at any time to see the current state of your configuration.

---

## Viewing Your Config

```
/config view
```

This returns a single embed showing all your current settings — which modules are on, which channels are set, which roles are configured, and what your thresholds are.

---

## Modules

Enable or disable each module independently.

```
/config toggle <module> <true|false>
```

| Module | Default | Description |
|--------|---------|-------------|
| `anti-raid` | Off | Automatic raid detection and server lockdown |
| `auto-mod` | Off | Real-time message monitoring and violation response |
| `verification` | Off | Math CAPTCHA for every new join |
| `auto-apply-bans` | Off | Auto-apply and auto-enforce network bans |

{% hint style="info" %}
`auto-apply-bans` must be enabled for your server to receive ban deployments and Watchdog sweeps. If it's off, your server is still on the network list but bans won't auto-apply.
{% endhint %}

---

## Channels

Tell SentinelFX where to post its messages.

```
/config set-channel <type> <#channel>
```

| Type | When it's used |
|------|---------------|
| `mod_log_channel` | Every moderation action: bans, kicks, mutes, warns, purges, slowmode changes |
| `network_ban_ch` | Network ban deployment notifications — when a banned user is removed from your server |
| `raid_alert_ch` | Anti-raid lockdown alert with the unlock button |
| `welcome_ch` | Fallback for verification challenges if a user's DMs are closed |

{% hint style="warning" %}
If `mod_log_channel` is not set, moderation actions are still applied — they just won't be logged anywhere you can see them. Always set a mod log channel.
{% endhint %}

---

## Roles

Tell SentinelFX who your staff are.

```
/config set-role <type> <@role>
```

| Type | How it's used |
|------|--------------|
| `mod_role` | Members with this role are exempt from Auto-Mod, can use moderation commands |
| `admin_role` | Additional admin-tier exemptions |
| `verified_role` | Granted to users who pass verification |

---

## Thresholds

Fine-tune the numeric limits that drive automatic behaviour.

```
/config thresholds [options]
```

### Warning Escalation

| Option | Default | Description |
|--------|---------|-------------|
| `warn_mute` | 3 | Total warnings before auto-mute (1 hour) |
| `warn_kick` | 5 | Total warnings before auto-kick |
| `warn_ban` | 7 | Total warnings before auto-ban |

### Auto-Mod Sensitivity

| Option | Default | Description |
|--------|---------|-------------|
| `spam_limit` | 5 | Messages per 5-second window before spam fires |

Per-message mention limits are part of the server's Auto-Mod config (stored as `mention_limit` in the guild row, default 5) and are not exposed as a `/config thresholds` option in the current build.

### Anti-Raid Sensitivity

| Option | Default | Description |
|--------|---------|-------------|
| `raid_threshold` | 10 | Joins within the window before lockdown triggers |
| `raid_window` | 10 | Seconds to use as the join counting window |

---

## Recommended Starting Config

For a typical FiveM community server:

```
/config toggle anti-raid        true
/config toggle auto-mod         true
/config toggle verification     true
/config toggle auto-apply-bans  true

/config set-channel mod_log_channel #mod-log
/config set-channel network_ban_ch  #network-bans
/config set-channel raid_alert_ch   #raid-alerts
/config set-channel welcome_ch      #verify-here

/config set-role mod_role       @Moderator
/config set-role admin_role     @Admin
/config set-role verified_role  @Verified

/config thresholds warn_mute:3 warn_kick:5 warn_ban:7
```

Then sync existing bans:

```
/network-sync
```
