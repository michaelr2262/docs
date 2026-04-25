---
description: Information and diagnostic commands.
---

# Utility Commands

---

## `/userinfo`

<figure><img src="../sfx_scanning.svg" alt="Scanning" width="48"></figure>

View detailed information about a user — including their account details, server roles, warning history, and SentinelFX network ban status.

**Required permission:** Moderate Members

| Option | Required | Description |
|--------|----------|-------------|
| User | No | Member to look up (defaults to yourself) |

**What it shows:**

| Field | Details |
|-------|---------|
| Account created | Date and age — flagged if under 7 days old |
| Joined server | Date the user joined this server |
| Roles | All roles the user currently has |
| Warnings | Total count plus the most recent warnings with reasons |
| Network ban | Active ban with reason, or clean |

**Embed colour:**

| Colour | Meaning |
|--------|---------|
| 🔴 Red | User has an active network ban |
| 🟠 Orange | User has server-level warnings |
| 🟣 Purple | User is clean |

---

## `/ping`

Check the bot's current latency.

**Required permission:** None — anyone can use it.

Returns:

- **Roundtrip** — time from command to response in milliseconds.
- **WebSocket** — Discord gateway connection latency in milliseconds.
