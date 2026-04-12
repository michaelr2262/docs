---
description: Commands for managing the SentinelFX cross-server ban network.
---

# Network Commands

<figure><img src="../sfx_network.svg" alt="Network" width="64"></figure>

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

## Appeals

<figure><img src="../sfx_appeal.svg" alt="Appeal" width="48"></figure>

Appeals are submitted through the **SentinelFX Dashboard** — not a slash command. Banned users visit [sentinelfx.net](https://sentinelfx.net), log in with Discord, and submit their appeal from their account page.

The appeal must be between **50 and 1,000 characters** explaining why the ban should be lifted. The SentinelFX Team reviews it and responds via the dashboard.

{% hint style="warning" %}
You must currently have an active network ban to submit an appeal. Only one open appeal is allowed at a time.
{% endhint %}

---

## `/network-status`

Check network information. Has three subcommands.

**Required permission:** Moderate Members

### `/network-status check <user>`

Look up whether a specific user is on the network ban list.

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `user` | User | ✅ | The user to check |

Returns:
- <img src="../sfx_banned.svg" width="16"> **Banned** — shows reason, approval date, approver, prior ban count
- <img src="../sfx_online.svg" width="16"> **Clean** — user has no active network ban

### `/network-status stats`

View network-wide statistics including total active bans, connected servers, and pending reports.

### `/network-status banlist`

View the 15 most recent network bans with reason, category, and timestamp.

{% hint style="info" %}
`banlist` is only available in the HQ control server.
{% endhint %}

---

## `/network-history`

View the full SentinelFX history for a user, including:

- Active network ban (if any)
- Open appeal (if any)
- Server-level ban log
- Reports filed by the user

**Required permission:** Administrator

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `user_id` | String | ✅ | The Discord User ID to look up |

---

## `/network-unban`

Lift a SentinelFX network ban. This removes the user from the network ban list and unbans them from every connected server.

**Required permission:** Administrator  
**Control server only**

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `user_id` | String | ✅ | The User ID to unban |
| `reason` | String | ✅ | Reason for lifting the ban |

{% hint style="danger" %}
This command can only be run in the SentinelFX HQ server. It will be ignored in member servers.
{% endhint %}

---

## `/pending-reports`

<figure><img src="../sfx_reported.svg" alt="Reported" width="48"></figure>

View all reports currently awaiting network team review. Shows up to 10 pending reports with:

- Report ID and user
- Offence category
- Current vote tally (approvals / denials vs thresholds)
- Age of the report
- Jump link to the review message

**Required permission:** Administrator  
**Control server only**

---

## `/server-list`

List all servers connected to the SentinelFX network. Shows:

- Server name, ID, and member count
- Online (bot present) vs offline (bot removed) status
- Whether Auto-apply bans is enabled (✅) or paused (⏸️)

**Required permission:** Administrator  
**Control server only**
