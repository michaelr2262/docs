---
description: Commands for interacting with the SentinelFX cross-server ban network.
---

# Network Commands

<figure><img src="../sfx_network.svg" alt="Network" width="64"></figure>

Most day-to-day network operations — reviewing reports, managing servers, looking up users, lifting bans — have moved to the [Web Dashboard](../dashboard.md). Two commands remain in Discord for the moments when chat is the right tool.

---

## `/report`

Submit a user to the SentinelFX network for review. If approved by the team, the user is banned across every connected server.

**Required permission:** Moderate Members

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `user` | User | ✅ | The user to report |
| `category` | Choice | ✅ | Type of offence (see below) |
| `reason` | String | ✅ | Full description of the behaviour |
| `evidence` | String | ❌ | Link to screenshot, video, or Imgur album |

**Threat categories:**

| Value | Label |
|-------|-------|
| `raider` | Raider |
| `griefer` | Griefer |
| `scammer` | Scammer |
| `cheater` | Cheater |
| `toxic` | Toxic Player |
| `other` | Other |

{% hint style="info" %}
Reports are reviewed by the SentinelFX Team before any action is taken. Submitting a report does not immediately ban the user.
{% endhint %}

---

## `/appeal`

Submit an appeal against an active network ban. Opens a modal where you write 50–1,000 characters explaining why the ban should be lifted.

**Required permission:** None — available to everyone in any server where the bot is present.

| Field | Limits | Description |
|-------|--------|-------------|
| Statement | 50–1,000 chars | Your case for why the ban should be lifted |

The appeal is posted to the SentinelFX HQ appeals channel along with your current threat score and ban details. The team reviews it and — if the appeal succeeds — lifts the ban across every connected server.

{% hint style="info" %}
Don't have an active network ban? `/appeal` will tell you so. Only users on the network ban list can open an appeal.
{% endhint %}

---

## Moved to the Dashboard

The following have been retired from Discord because they're a better experience on the web:

| Was | Now |
|-----|-----|
| `/network-status check` / `stats` / `banlist` | **Dashboard → Network** — user lookup, live stats, sortable ban list |
| `/network-history` | **Dashboard → User profile** — full network history on one page |
| `/network-unban` | **Dashboard → Ban row → Lift** — one click with context pre-filled |
| `/pending-reports` | **Dashboard → Review queue** — Approve / Deny with evidence preview |
| `/server-list` | **Dashboard → Servers** — live health, ban-mode, heartbeat |

See the [Web Dashboard](../dashboard.md) page for how to access each view.
