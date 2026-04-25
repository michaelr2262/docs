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

### `/config toggle`

Enable or disable a module.

| Module | What it controls |
|--------|-----------------|
| Anti-Raid | Automatic raid detection and lockdown |
| Auto-Mod | Real-time message moderation |
| Verification | Math CAPTCHA for new joins |
| Auto-apply Bans | Auto-apply network bans on join and during sweeps |

### `/config set-channel`

Configure where SentinelFX sends its messages.

| Channel | Purpose |
|------|---------|
| Mod log channel | All moderation action logs |
| Network ban channel | Network ban deployment notifications |
| Raid alert channel | Anti-raid lockdown alerts and unlock button |
| Welcome channel | Verification challenge fallback (if DMs are closed) |

### `/config set-role`

Tell SentinelFX which roles belong to which staff tier.

| Role | Purpose |
|------|---------|
| Moderator role | Moderator role (exempted from auto-mod, can use mod commands) |
| Admin role | Admin role (additional exemptions) |
| Verified role | Role granted after passing verification |

### `/config thresholds`

Adjust numeric limits for auto-escalation and detection.

| Setting | Default | Description |
|--------|---------|-------------|
| Warn-mute threshold | 3 | Warning count that triggers auto-mute |
| Warn-kick threshold | 5 | Warning count that triggers auto-kick |
| Warn-ban threshold | 7 | Warning count that triggers auto-ban |
| Spam limit | 5 | Messages per 5 seconds before spam detection fires |
| Mention limit | 5 | User or role mentions per message before mass-mention fires |
| Raid threshold | 10 | Joins within the raid window to trigger lockdown |
| Raid window | 10 | Time window in seconds for counting joins |

---

{% hint style="info" %}
**Managing banned words?** The banned-words list lives on the [Web Dashboard](../dashboard.md) — paste-in-bulk, search, and edit are all faster there than a slash command.
{% endhint %}

---

## `/lockdown`

Manually activate or deactivate server lockdown. When locked down, all new joins are automatically kicked.

**Required permission:** Administrator

| Action | Description |
|--------|-------------|
| On | Activate lockdown — kicks all new joins, auto-unlocks in 10 minutes |
| Off | Deactivate lockdown manually |

{% hint style="warning" %}
Lockdown normally activates automatically when the [Anti-Raid](../modules/anti-raid.md) module detects a mass-join. Use lockdown manually if you spot a raid early.
{% endhint %}

---

## `/network-sync`

Immediately apply all active network bans to your server. This is a one-time bulk sync — it checks every current member against the full network ban list and bans any matches.

**Required permission:** Administrator

{% hint style="info" %}
Run this when you first join the network or after an extended period away. From this point forward, bans are auto-applied via the join-time check and Watchdog sweeps.
{% endhint %}
