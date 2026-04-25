---
description: Real-time message moderation with progressive escalation.
---

# Auto-Moderation

<figure><img src="../sfx_shield.svg" alt="Shield" width="64"></figure>

The Auto-Mod module watches every message sent in your server. It detects rule-breaking content instantly, deletes it, warns the user, and escalates automatically based on their history — all without a moderator needing to be present.

{% hint style="info" %}
Auto-Mod skips messages from moderators, admins, and bots. Only regular members are checked.
{% endhint %}

---

## What Auto-Mod Detects

### Content Violations

| Violation | Detection |
|-----------|-----------|
| Spam | Too many messages sent in a short rolling window |
| Duplicate spam | The same message sent repeatedly in quick succession |
| Mass mentions | Too many user or role pings in one message |
| Banned words | Any word or phrase on your server's banned list (case-insensitive) |
| Scam / phishing links | Discord Nitro scams, Steam phishing, free Robux, link shorteners, and similar |
| Uninvited Discord invites | Any Discord server invite link (unless the sender is a mod) |

The exact thresholds are tunable from `/config thresholds` — see [Server Configuration](../configuration.md).

---

## Strike System

Auto-Mod tracks violations **per user per session** in a short rolling window. Each violation adds a strike.

**Mute thresholds (cumulative within the session):**

| Strikes | Auto-Mute Duration |
|---------|--------------------|
| 3 | 5 minutes |
| 5 | 30 minutes |
| 8 | 2 hours |

After the mute, if the user's **total warning count** in the database exceeds your server's escalation thresholds, Auto-Mod will apply a kick or ban instead.

---

## Slowmode Auto-Escalation

Auto-Mod also monitors message rates **per channel** to manage sudden activity spikes without moderator input.

**How it works:**

1. Counts messages in short windows per channel.
2. If the rate climbs above a threshold, it identifies the heavy senders.
3. Applies slowmode to the channel based on spike intensity.

**Slowmode levels:**

| Spike Intensity | Slowmode Applied |
|----------------|----------------|
| Moderate | 10 seconds |
| High | 15 seconds |
| Severe | 30 seconds |

Heavy senders also receive strikes for contributing to the spike and may be auto-muted.

**Auto-lift:** Slowmode is automatically removed shortly after the message rate returns to normal.

---

## Enforcement Flow

For every violation detected:

1. The message is deleted.
2. A strike is added to the user's in-session count.
3. A warning is recorded.
4. The user gets a DM with the violation reason and current strike count.
5. If the strike threshold is reached, an auto-mute is applied.
6. If the user's total warning count exceeds your server's kick or ban threshold, they're escalated.
7. The full action is logged to your mod-log channel.

---

## Enabling Auto-Mod

Use `/config toggle` to turn Auto-Mod on. Use `/config thresholds` to tune the spam and mention limits.

To set your banned word list, use the **Web Dashboard → Auto-Mod** — add, remove, and bulk-paste words from a searchable table.
