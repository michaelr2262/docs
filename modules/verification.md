---
description: Math CAPTCHA on join to block bot accounts before they reach your members.
---

# Verification System

<figure><img src="../sfx_verified.svg" alt="Verified" width="64"></figure>

The Verification module requires every new join to solve a simple maths problem before they can access the server. This filters out most bot accounts and automated join tools, which typically cannot interact with Discord modals.

---

## How It Works

```
User joins server
    ↓
SentinelFX detects guildMemberAdd
    ↓
Sends verification challenge via DM
    ↓
(If DMs closed → posts to welcome channel instead)
    ↓
User clicks "Verify" button
    ↓
Discord modal opens with the maths problem
    ↓
User submits their answer
    ↓
Correct → Verified role granted, logged
Incorrect → Rejected
```

---

## The Challenge

The bot generates a random arithmetic problem on every join. Problems are one of three types:

- **Addition:** `47 + 28 = ?`
- **Subtraction:** `84 − 36 = ?`
- **Multiplication:** `6 × 9 = ?`

Numbers are randomised each time. The user has **5 minutes** to complete verification before the challenge expires.

---

## The Verified Role

Once a user answers correctly:

1. They receive the role set via `/config set-role verified_role`
2. The verification record is removed from the database
3. The event is logged to your mod-log channel

{% hint style="info" %}
Make sure the **Verified** role is the gate to your normal channels. Members without it should only see a `#verify-here` channel where the bot's welcome message appears.
{% endhint %}

---

## Enabling Verification

```
/config toggle verification true
/config set-role verified_role @Verified
/config set-channel welcome_ch #verify-here
```

---

## Channel Setup Tip

A typical verification setup in Discord:

1. Create a `#verify-here` channel visible to `@everyone`
2. Lock all other channels to require the `@Verified` role
3. Set `#verify-here` as the welcome channel in SentinelFX
4. The bot will post the verification challenge there if a user's DMs are closed

{% hint style="warning" %}
If neither DMs nor the welcome channel is configured, verification challenges will be silently dropped. Always set a welcome channel as a fallback.
{% endhint %}

---

## Cleanup

The [Watchdog](watchdog.md) runs a cleanup task every 5 minutes that removes any verification records that have been pending for more than 5 minutes. This keeps the database tidy and prevents stale challenges from consuming space.
