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

| Violation | Detection Method |
|-----------|----------------|
| **Spam** | `spam_limit` messages sent within 5 seconds (default: 5) |
| **Duplicate spam** | Same message sent 3 or more times in the last 5 messages |
| **Mass mentions** | `mention_limit` or more user/role pings in one message (default: 5) |
| **Banned words** | Any word or phrase on your server's banned list (case-insensitive) |
| **Scam / phishing links** | Discord Nitro scams, Steam phishing, free Robux, bit.ly redirects, and similar |
| **Uninvited Discord invites** | Any Discord server invite link (unless the sender is a mod) |

---

## Strike System

Auto-Mod tracks violations **per user per session** in a 10-minute rolling window. Each violation adds a strike.

**Mute thresholds:**

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

1. Counts messages in 5-second windows per channel
2. If **10 or more messages** arrive in a 5-second window, it identifies the heavy senders (anyone who sent 4+ messages in that window)
3. Applies slowmode to the channel based on spike intensity

**Slowmode levels:**

| Spike Intensity | Slowmode Applied |
|----------------|----------------|
| Moderate (10–14 msgs/5s) | 10 seconds |
| High (15–19 msgs/5s) | 15 seconds |
| Severe (20+ msgs/5s) | 30 seconds |

Heavy senders also receive strikes for contributing to the spike and may be auto-muted.

**Auto-lift:** Slowmode is automatically removed **30 seconds after** the message rate returns to normal.

---

## Enforcement Flow

For every violation detected:

```
Message sent
    ↓
Auto-Mod checks content
    ↓
Violation found → Delete message
    ↓
Add strike to user's in-session count
    ↓
Log warning to database
    ↓
DM user with violation reason and current strike count
    ↓
Check strike count → Apply mute if threshold reached
    ↓
Check DB warning count → Escalate to kick/ban if thresholds reached
    ↓
Log action to mod-log channel
```

---

## Enabling Auto-Mod

```
/config toggle auto-mod true
```

To set your banned word list:
```
/banned-words add <word>
/banned-words list
/banned-words remove <word>
```

To adjust thresholds:
```
/config thresholds spam_limit:4
```
