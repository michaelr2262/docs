---
description: Automatic detection and response to coordinated mass-join raids.
---

# Anti-Raid System

<figure><img src="../sfx_alert.svg" alt="Alert" width="64"></figure>

The Anti-Raid module monitors your server's join activity in real time. When it detects a mass-join event — a common attack pattern where a raiding group joins simultaneously — it automatically locks the server down and alerts your team.

---

## How Raid Detection Works

The module tracks recent join timestamps for your server in a rolling window. If the join count inside the window crosses a threshold, lockdown triggers.

**Default settings (configurable via `/config thresholds`):**

| Setting | Default | Description |
|---------|---------|-------------|
| Raid threshold | 10 | Joins within the window before lockdown triggers |
| Raid window | 10 | Seconds to look back when counting joins |

By default this means **10 joins within 10 seconds** triggers a raid response.

---

## What Happens During Lockdown

<figure><img src="../sfx_offline.svg" alt="Offline" width="48"></figure>

When a raid is detected:

1. Lockdown activates — the server enters a locked state.
2. All new joins are kicked immediately with a DM explaining the situation.
3. An alert is sent to your configured raid-alert channel with an **Unlock** button.
4. Auto-unlock fires after 10 minutes if the team hasn't unlocked manually.

The Unlock button in the alert lets any moderator clear the lockdown instantly without a command.

---

## Enabling Anti-Raid

Turn Anti-Raid on with `/config toggle` and point it at a raid-alert channel using `/config set-channel`.

---

## Manual Lockdown

You can trigger lockdown manually if you spot a raid forming before the threshold is reached using the `/lockdown` command. Use the same command to deactivate it.

---

## Raid Notification Embed

When a raid is detected, the embed in your raid-alert channel shows:

- The join count and time window that triggered detection.
- Current lockdown status.
- A timestamp.
- The **Unlock Server** button.

{% hint style="warning" %}
During lockdown, legitimate users who try to join will also be kicked. The auto-unlock at 10 minutes is intentional — raids rarely last that long, and this prevents moderators from forgetting to unlock.
{% endhint %}
