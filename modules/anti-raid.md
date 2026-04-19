---
description: Automatic detection and response to coordinated mass-join raids.
---

# Anti-Raid System

<figure><img src="../sfx_alert.svg" alt="Alert" width="64"></figure>

The Anti-Raid module monitors your server's join activity in real time. When it detects a mass-join event — a common attack pattern where a raiding group joins simultaneously — it automatically locks the server down and alerts your team.

---

## How Raid Detection Works

The module maintains a sliding window of recent join timestamps for each server.

**Algorithm:**

```
On each member join:
    1. Remove timestamps older than raid_window seconds
    2. Add current timestamp
    3. Count joins in window
    4. If count >= raid_threshold → trigger lockdown
    5. If already in lockdown → kick the new join immediately
```

**Default settings** (configurable via `/config thresholds`):

| Setting | Default | Description |
|---------|---------|-------------|
| `raid_threshold` | 10 | Joins within the window before lockdown triggers |
| `raid_window` | 10 | Seconds to look back when counting joins |

So by default: **10 joins within 10 seconds** = raid detected.

---

## What Happens During Lockdown

<figure><img src="../sfx_offline.svg" alt="Offline" width="48"></figure>

When a raid is detected:

1. **Lockdown activates** — the server enters a locked state
2. **All new joins are kicked immediately** with a DM explaining the situation
3. **Alert sent** to your configured raid-alert channel with an **Unlock** button
4. **Auto-unlock** fires after 10 minutes if the team hasn't unlocked manually

The Unlock button in the alert lets any moderator clear the lockdown instantly without a command.

---

## Enabling Anti-Raid

```
/config toggle anti-raid true
/config set-channel raid_alert_ch #raid-alerts
```

---

## Manual Lockdown

You can trigger lockdown manually if you spot a raid forming before the threshold is reached:

```
/lockdown on
```

And deactivate it:

```
/lockdown off
```

---

## Raid Notification Embed

When a raid is detected, the embed in your raid-alert channel shows:

- The join count and time window that triggered detection
- Current lockdown status
- A timestamp
- The **🔓 Unlock Server** button

{% hint style="warning" %}
During lockdown, legitimate users who try to join will also be kicked. The auto-unlock at 10 minutes is intentional — raids rarely last that long, and this prevents moderators from forgetting to unlock.
{% endhint %}
