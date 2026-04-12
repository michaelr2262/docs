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
| `network-bans` | Off | Auto-apply and auto-enforce network bans |

{% hint style="info" %}
`network-bans` must be enabled for your server to receive ban deployments and Watchdog sweeps. If it's off, your server is still on the network list but bans won't auto-apply.
{% endhint %}

---

## Channels

Tell SentinelFX where to post its messages.

```
/config set-channel <type> <#channel>
```

| Type | When it's used |
|------|---------------|
| `mod-log` | Every moderation action: bans, kicks, mutes, warns, purges, slowmode changes |
| `network` | Network ban deployment notifications — when a banned user is removed from your server |
| `raid-alert` | Anti-raid lockdown alert with the unlock button |
| `welcome` | Fallback for verification challenges if a user's DMs are closed |

{% hint style="warning" %}
If `mod-log` is not set, moderation actions are still applied — they just won't be logged anywhere you can see them. Always set a mod log channel.
{% endhint %}

---

## Roles

Tell SentinelFX who your staff are.

```
/config set-role <type> <@role>
```

| Type | How it's used |
|------|--------------|
| `mod` | Members with this role are exempt from Auto-Mod, can use moderation commands |
| `admin` | Additional admin-tier exemptions |
| `verified` | Granted to users who pass verification |

---

## Thresholds

Fine-tune the numeric limits that drive automatic behaviour.

```
/config thresholds [options]
```

### Warning Escalation

| Option | Default | Description |
|--------|---------|-------------|
| `warn_mute_at` | 3 | Total warnings before auto-mute (1 hour) |
| `warn_kick_at` | 5 | Total warnings before auto-kick |
| `warn_ban_at` | 7 | Total warnings before auto-ban |

### Auto-Mod Sensitivity

| Option | Default | Description |
|--------|---------|-------------|
| `spam_limit` | 5 | Messages per 5-second window before spam fires |
| `mention_limit` | 5 | Mentions in one message before mass-mention fires |

### Anti-Raid Sensitivity

| Option | Default | Description |
|--------|---------|-------------|
| `raid_threshold` | 10 | Joins within the window before lockdown triggers |
| `raid_window` | 10 | Seconds to use as the join counting window |

---

## Recommended Starting Config

For a typical FiveM community server:

```
/config toggle anti-raid     true
/config toggle auto-mod      true
/config toggle verification  true
/config toggle network-bans  true

/config set-channel mod-log    #mod-log
/config set-channel network    #network-bans
/config set-channel raid-alert #raid-alerts
/config set-channel welcome    #verify-here

/config set-role mod       @Moderator
/config set-role admin     @Admin
/config set-role verified  @Verified

/config thresholds warn_mute_at:3 warn_kick_at:5 warn_ban_at:7
```

Then sync existing bans:

```
/network-sync
```
