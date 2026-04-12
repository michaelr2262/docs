---
description: Server configuration and administrative commands.
---

# Admin Commands

<figure><img src="../sfx_alert.svg" alt="Admin" width="64"></figure>

Admin commands require the **Administrator** permission unless otherwise noted. These control how SentinelFX behaves in your server.

---

## `/config`

View and update your server's SentinelFX configuration. All settings are stored per-server.

**Required permission:** Administrator

### `/config view`

Display the current configuration — all modules, channels, roles, and thresholds in a single embed.

### `/config toggle <module> <enabled>`

Enable or disable a module.

| Module | What it controls |
|--------|-----------------|
| `anti-raid` | Automatic raid detection and lockdown |
| `auto-mod` | Real-time message moderation |
| `verification` | Math CAPTCHA for new joins |
| `network-bans` | Auto-apply network bans on join and sweep |

### `/config set-channel <type> <channel>`

Configure where SentinelFX sends its messages.

| Type | Purpose |
|------|---------|
| `mod-log` | All moderation action logs |
| `network` | Network ban deployment notifications |
| `raid-alert` | Anti-raid lockdown alerts and unlock button |
| `welcome` | Verification challenge fallback (if DMs are closed) |

### `/config set-role <type> <role>`

Tell SentinelFX which roles belong to which staff tier.

| Type | Purpose |
|------|---------|
| `mod` | Moderator role (exempted from auto-mod, can use mod commands) |
| `admin` | Admin role |
| `verified` | Role granted after passing verification |

### `/config thresholds <options>`

Adjust numeric limits for auto-escalation and detection.

| Option | Default | Description |
|--------|---------|-------------|
| `warn_mute_at` | 3 | Warning count that triggers auto-mute |
| `warn_kick_at` | 5 | Warning count that triggers auto-kick |
| `warn_ban_at` | 7 | Warning count that triggers auto-ban |
| `spam_limit` | 5 | Messages per 5 seconds before spam detection fires |
| `mention_limit` | 5 | Mentions per message before mass-mention fires |
| `raid_threshold` | 10 | Joins within the raid window to trigger lockdown |
| `raid_window` | 10 | Time window in seconds for counting joins |

---

## `/banned-words`

Manage the per-server list of banned words and phrases. The [Auto-Mod](../modules/auto-mod.md) module uses this list to delete violating messages in real time.

**Required permission:** Manage Guild

### `/banned-words list`

Show all banned words currently set for this server.

### `/banned-words add <word>`

Add a word or phrase to the filter. Matching is case-insensitive.

### `/banned-words remove <word>`

Remove a word or phrase from the filter.

---

## `/lockdown`

Manually activate or deactivate server lockdown. When locked down, all new joins are automatically kicked.

**Required permission:** Administrator

| Option | Value | Description |
|--------|-------|-------------|
| `action` | `on` | Activate lockdown — kicks all new joins, auto-unlocks in 10 minutes |
| `action` | `off` | Deactivate lockdown manually |

{% hint style="warning" %}
Lockdown normally activates automatically when the [Anti-Raid](../modules/anti-raid.md) module detects a mass-join. Use `/lockdown on` to trigger it manually if you spot a raid early.
{% endhint %}

---

## `/network-sync`

Immediately apply all active network bans to your server. This is a one-time bulk sync — it checks every current member against the full network ban list and bans any matches.

**Required permission:** Administrator

{% hint style="info" %}
Run this when you first join the network or after a period of bot downtime to ensure you haven't missed any bans. From this point forward, bans are auto-applied via the join-time check and 6-hour Watchdog sweep.
{% endhint %}
