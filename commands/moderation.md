---
description: Standard moderation commands with full audit logging.
---

# Moderation Commands

<figure><img src="../sfx_shield.svg" alt="Shield" width="64"></figure>

All moderation actions are logged to your configured mod-log channel. Users receive a DM where possible before enforcement actions are taken.

---

## `/ban`

Ban a member from the server. Supports permanent and temporary bans.

**Required permission:** Ban Members

| Option | Required | Description |
|--------|----------|-------------|
| User | Yes | Member to ban |
| Reason | No | Reason for the ban |
| Duration | No | Temp-ban duration, e.g. *7d*, *24h*, *30m* |
| Delete days | No | Days of message history to delete (0–7) |

**How it works:**

1. The bot DMs the user with the reason (if possible).
2. The Discord ban is applied with optional message deletion.
3. If a duration is set, the ban is auto-lifted when it expires.
4. The action is logged to the mod-log channel.
5. A *Report to Network* button is shown — clicking it pre-fills a `/report` for escalation to the network.

{% hint style="info" %}
Temporary bans are handled by the [Watchdog](../modules/watchdog.md). The user is automatically unbanned when the duration expires, even if there's a brief downtime.
{% endhint %}

---

## `/kick`

Kick a member from the server. They can rejoin immediately.

**Required permission:** Kick Members

| Option | Required | Description |
|--------|----------|-------------|
| User | Yes | Member to kick |
| Reason | No | Reason for the kick |

---

## `/mute`

Apply a Discord timeout (mute) to a member. They cannot send messages, react, speak in voice, or join voice channels while muted.

**Required permission:** Moderate Members

| Option | Required | Description |
|--------|----------|-------------|
| User | Yes | Member to mute |
| Duration | Yes | How long to mute, e.g. *10m*, *1h*, *1d* (max 28 days) |
| Reason | No | Reason for the mute |

**Valid duration formats:** seconds, minutes, hours, days — e.g. *30s*, *10m*, *2h*, *1d*, *7d*.

---

## `/unban`

Unban a user from the server by their Discord User ID.

**Required permission:** Ban Members

| Option | Required | Description |
|--------|----------|-------------|
| User ID | Yes | Discord User ID of the user to unban |
| Reason | No | Reason for unbanning |

---

## `/warn`

Issue a formal warning to a member. Warnings accumulate and trigger automatic escalation.

**Required permission:** Moderate Members

| Option | Required | Description |
|--------|----------|-------------|
| User | Yes | Member to warn |
| Reason | Yes | Reason for the warning |

**Auto-escalation thresholds (configurable via `/config thresholds`):**

| Warning Count | Default Action |
|---------------|---------------|
| 3 or more | Auto-mute for 1 hour |
| 5 or more | Auto-kick |
| 7 or more | Auto-ban |

{% hint style="info" %}
Thresholds are fully configurable per server. See [Server Configuration](../configuration.md).
{% endhint %}

---

## `/warnings`

View all warnings on record for a member.

**Required permission:** Moderate Members

| Option | Required | Description |
|--------|----------|-------------|
| User | Yes | Member to look up |

Returns up to 15 warnings with IDs, timestamps, reasons, and the moderator who issued each one.

---

## `/unwarn`

Remove a single warning by its ID.

**Required permission:** Moderate Members

| Option | Required | Description |
|--------|----------|-------------|
| ID | Yes | The warning ID to remove |

---

## `/clearwarnings`

Clear **all** warnings for a member at once.

**Required permission:** Administrator

| Option | Required | Description |
|--------|----------|-------------|
| User | Yes | Member to clear |

---

## `/purge`

Bulk-delete messages in the current channel. Discord only allows deletion of messages younger than 14 days.

**Required permission:** Manage Messages

| Option | Required | Description |
|--------|----------|-------------|
| Amount | Yes | Number of messages to delete (1–100) |
| User | No | If set, only deletes messages from this user |

---

## `/slowmode`

Set the slowmode (rate-limit) for a channel. A value of zero disables slowmode.

**Required permission:** Manage Channels

| Option | Required | Description |
|--------|----------|-------------|
| Seconds | Yes | Slowmode delay in seconds (0–21600; max is 6 hours) |
| Channel | No | Target channel (defaults to current) |
