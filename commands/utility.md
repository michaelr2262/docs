---
description: Information and diagnostic commands.
---

# Utility Commands

---

## `/userinfo`

<figure><img src="../sfx_scanning.svg" alt="Scanning" width="48"></figure>

View detailed information about a user — including their account details, server roles, warning history, and SentinelFX network ban status.

**Required permission:** Moderate Members

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `user` | User | ❌ | Member to look up (defaults to yourself) |

**What it shows:**

| Field | Details |
|-------|---------|
| Account created | Date and age — flagged ⚠️ if under 7 days old |
| Joined server | Date the user joined this server |
| Roles | All roles the user currently has |
| Warnings | Total count plus the 3 most recent warnings with reasons |
| Network ban | <img src="../sfx_banned.svg" width="14"> Active ban with reason, or <img src="../sfx_online.svg" width="14"> clean |

**Embed colour:**

| Colour | Meaning |
|--------|---------|
| 🔴 Red | User has an active network ban |
| 🟠 Orange | User has server-level warnings |
| 🟣 Purple | User is clean |

---

## `/ping`

Check the bot's current latency.

**Required permission:** None (any user)

Returns:
- **Roundtrip** — time from command to response (ms)
- **WebSocket** — Discord gateway connection latency (ms)
