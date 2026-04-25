---
description: How the SentinelFX cross-server ban network works end to end.
---

# How the Network Works

<figure><img src="sfx_network.svg" alt="Network" width="64"></figure>

The SentinelFX network is a shared intelligence layer that sits across every member server. When one server identifies a bad actor, every other server benefits — automatically.

---

## The Journey of a Report

### 1 — A Moderator Files a Report

Any moderator in any member server uses the `/report` command. They select a **threat category**, write a **reason**, and optionally attach **evidence** (a screenshot link, video, or album). The report is sent to SentinelFX for review.

**Threat categories:**

| Category | Description |
|----------|-------------|
| Raider | Mass-join raids, coordinated disruption |
| Griefer | Deliberate harassment or in-game griefing |
| Scammer | Trade scams, financial fraud, fake services |
| Cheater | Cheat software, exploits, game manipulation |
| Toxic | Sustained abusive behaviour, hate speech |
| Other | Anything that doesn't fit above |

---

### 2 — SentinelFX Reviews the Report

Reports are reviewed by SentinelFX network admins. The team can approve, deny, or request more information. Once a report meets the approval threshold, the ban is confirmed and deployed across the network.

{% hint style="info" %}
Reports do not immediately ban the user. There is always a review step, and you'll see the outcome reflected in your server's mod-log when a ban is deployed.
{% endhint %}

---

### 3 — Smart Triage

Reports pass through an automatic triage layer before they reach a human reviewer. Obvious cases (very strong evidence, repeated offenders, machine-verified detections) can be fast-tracked or actioned immediately with a built-in undo window. Borderline cases get a second opinion from an AI analyst that produces a written rationale. Routine cases go to the normal human review queue.

The triage system has hard safety rails: it cannot ban anyone based purely on weak evidence, it cannot override staff or trusted-reporter protections, and every automated action is logged and visible in the audit trail.

---

### 4 — Ban Deployment

<figure><img src="sfx_banned.svg" alt="Banned" width="48"></figure>

The moment a ban is confirmed, SentinelFX checks every connected server simultaneously:

1. For each server with **Auto-apply bans** enabled, it checks if the user is currently a member.
2. If the user is present, they are immediately banned, a DM is sent (where possible), and the mod-log channel receives a notification embed.
3. If the user is not present, they are added to the ban list so they cannot rejoin.

This is the **deploy-time layer**.

---

### 5 — Join Protection

<figure><img src="sfx_scanning.svg" alt="Scanning" width="48"></figure>

When any user joins a member server, SentinelFX checks them against the full network ban list in real time.

If they appear on the list:

- They are instantly banned.
- Your mod-log channel receives a **Network-Banned User Removed** embed.

This catches users who were banned while not in your server.

---

### 6 — The Watchdog Sweep

<figure><img src="sfx_watchdog.svg" alt="Watchdog" width="48"></figure>

Several times per day, the Watchdog runs a full sweep of every member server. It compares all current members against the network ban list and removes anyone it finds.

This is the safety net for:

- Users who joined before auto-apply was enabled
- Users who slipped through during brief downtime
- Any edge case the other two layers didn't catch

---

## Extending Bans Into FiveM

<figure><img src="sfx_online.svg" alt="FiveM bridge" width="48"></figure>

Communities that run the optional SentinelFX FiveM resource get a fourth enforcement point: **on-connect** inside the game server itself.

When a player joins an FXServer running the resource, SentinelFX checks every identifier the client presents against the network ban list:

| Identifier | Catches |
|-----------|---------|
| Rockstar licence | Rockstar Social Club identity |
| Steam | Steam account |
| Discord | Linked Discord account (when CFX exposes it) |
| Xbox Live / Microsoft | Console account |
| Cfx.re | Cfx.re forum account |
| Hardware tokens | Physical machine fingerprint |

A network ban on **any** of these blocks the player on **every** FXServer running the resource — including servers the player has never connected to before.

### HWID Evasion

When a banned player creates a new Discord or Rockstar account to dodge the ban, the resource forwards the client's hardware tokens. The tokens are hashed on your FXServer before transmission. If the hash matches a user already on the network ban list, the new account is flagged and blocked before it ever loads into the game.

### Cross-Platform Coverage

Because identifiers are stored in a shared ledger, a ban issued from Discord propagates into FiveM the moment the player's licence or Steam ID is known, and a ban issued from an in-game cheat-scan can — with review — propagate back onto Discord across every guild in the network.

{% hint style="info" %}
Installing the resource is opt-in per FXServer. See [Installing the Resource](fivem-resource.md) for the setup flow.
{% endhint %}

---

## Alt Account Detection

SentinelFX cross-references every new account against the full network ban list using username similarity matching. If a new join has a username that closely resembles a known banned user and the account is recently created, the team is alerted. Alt detection is informational — it surfaces the match for human review rather than auto-banning.

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
