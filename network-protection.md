---
description: How the SentinelFX cross-server ban network works end to end.
---

# How the Network Works

<figure><img src="sfx_network.svg" alt="Network" width="64"></figure>

The SentinelFX network is a shared intelligence layer that sits across every member server. When one server identifies a bad actor, every other server benefits — automatically.

---

## The Journey of a Report

### 1 — A Moderator Files a Report

Any moderator in any member server runs `/report`. They select a **threat category**, provide a **reason**, and optionally attach **evidence** (screenshot link, video, Imgur album). The bot submits the report to the SentinelFX HQ server for review.

**Threat categories:**

| Category | Description |
|----------|-------------|
| `raider` | Mass-join raids, coordinated disruption |
| `griefer` | Deliberate harassment or in-game griefing |
| `scammer` | Trade scams, financial fraud, fake services |
| `cheater` | Cheat software, exploits, game manipulation |
| `toxic` | Sustained abusive behaviour, hate speech |
| `other` | Anything that doesn't fit above |

---

### 2 — The SentinelFX Team Reviews

The report is posted to a private review channel in the HQ server. The team votes:

- **Approve** — supports the ban
- **Deny** — disputes the report

The bot counts votes in real time. Once the approval threshold is reached (default: **3 approvals**), the ban is confirmed. If the denial threshold is reached first (default: **2 denials**), the report is dismissed.

{% hint style="info" %}
The SentinelFX Team can view all pending reports and their current vote tallies at any time with `/pending-reports`.
{% endhint %}

---

### 3 — Ban Deployment

<figure><img src="sfx_banned.svg" alt="Banned" width="48"></figure>

The moment a ban is confirmed, SentinelFX checks every connected server simultaneously:

1. For each server with **Auto-apply bans** enabled, it checks if the user is currently a member
2. If the user is present → they are immediately banned, a DM is sent, and the mod channel receives a notification embed
3. If the user is not present → they are added to the ban list so they cannot join

This is the **deploy-time layer**.

---

### 4 — Join Protection

<figure><img src="sfx_scanning.svg" alt="Scanning" width="48"></figure>

When any user joins a member server, SentinelFX checks them against the full network ban list in real time.

If they appear on the list:
- They are instantly banned
- The mod log channel receives a **Network-Banned User Removed** embed

This catches users who were banned while not in your server.

---

### 5 — The Watchdog Sweep

<figure><img src="sfx_watchdog.svg" alt="Watchdog" width="48"></figure>

Four times per day (00:00, 06:00, 12:00, 18:00 UTC), the Watchdog runs a full sweep of every member server. It compares all current members against the network ban list and removes anyone it finds.

This is the safety net for:
- Users who joined before auto-apply was enabled
- Users who slipped through during brief bot downtime
- Any edge case the other two layers didn't catch

---

## Ban Lifecycle

```
Report filed
    ↓
Team votes (approve / deny)
    ↓
Threshold reached
    ↓
Ban deployed to all servers ──→ [User present? → Instant ban]
    ↓                           [User joins later? → Auto-ban]
Watchdog sweeps (every 6h) ─→  [User found? → Sweep ban]
    ↓
Appeal submitted (optional)
    ↓
Team reviews appeal
    ↓
Approved → Ban lifted across all servers
Denied   → Ban remains
```

---

## Alt Account Detection

SentinelFX cross-references every new account against the full network ban list using **username similarity matching** (Levenshtein distance algorithm). If a new join has a username that is ≥75% similar to a known banned user and their account is under 30 days old, the team is alerted.

See [Alt Account Detection](modules/alt-detection.md) for full details.

---

## Threat Scoring

Every user receives a **0–100 risk score** based on their full cross-network history:

<figure><img src="sfx_alert.svg" alt="Alert" width="48"></figure>

| Score | Level | Colour |
|-------|-------|--------|
| 0–29 | LOW RISK | 🟢 Green |
| 30–69 | MEDIUM RISK | 🟡 Amber |
| 70–100 | HIGH RISK | 🔴 Red |

The score factors in active bans, prior lifted bans, network warnings, and pending reports. See [Threat Score Engine](modules/threat-score.md).

---

## Appeals

Banned users can submit an appeal through the dashboard at [sentinelfx.net](https://sentinelfx.net). The SentinelFX Team reviews it and can approve or deny it. An approved appeal lifts the ban across every server at once.

See [Appeals](appeals.md) for the full process.
