---
description: The background scheduler that keeps the network clean around the clock.
---

# Watchdog Scheduler

<figure><img src="../sfx_watchdog.svg" alt="Watchdog" width="64"></figure>

<figure><img src="../sentinelfx-watchdog-lockup.svg" alt="Watchdog Lockup" width="300"></figure>

The Watchdog is SentinelFX's background enforcement engine. It runs continuously, handling scheduled network sweeps, temporary ban expiry, and verification cleanup — all without any manual input.

---

## What the Watchdog Does

| Task | Schedule | Description |
|------|----------|-------------|
| **Network sweep** | 00:00, 06:00, 12:00, 18:00 UTC | Scans every member server for banned users |
| **Temp-ban expiry** | Every 60 seconds | Lifts bans whose duration has expired |
| **Verification cleanup** | Every 5 minutes | Removes expired pending verification records |

---

## Network Sweep

The network sweep is the Watchdog's most important job. Four times every day, it:

1. Fetches the full list of active network bans from the database
2. For each member server that has **Auto-apply bans** enabled:
   - Fetches the current member list
   - Cross-references every member against the ban list
   - Bans any match it finds
3. Skips server owners (cannot be banned by a bot)
4. Sends a **"Watchdog Sweep"** notification embed to each server's network ban channel

{% hint style="info" %}
The sweep uses different embed wording depending on how a ban was applied. A ban applied during a sweep says **"Network-Banned User Removed — Watchdog Sweep"** so your moderators know it was automated.
{% endhint %}

---

## Ban Deployment (Real-Time)

The Watchdog also handles immediate deployment when a **new ban is approved**. This isn't scheduled — it fires instantly:

1. The ban is written to the database
2. `enforceSingleBan()` is called
3. Every member server is checked for the user's presence
4. If found: banned immediately with a **"Threat Removed — Active at Time of Ban"** embed

This is the **deploy-time layer** of SentinelFX's three-layer defence.

---

## Temporary Ban Expiry

Every 60 seconds, the Watchdog checks the database for temp-bans that have reached their expiry time. For each expired ban:

1. The Discord ban is lifted
2. The temp-punishment record is removed
3. The user can rejoin the server normally

This means temporary bans are accurate to within a minute, even across bot restarts.

---

## Why Three Layers?

<figure><img src="../sfx_scanning.svg" alt="Scanning" width="48"></figure>

No single check is bulletproof. The Watchdog's sweep exists as a safety net for edge cases:

| Scenario | How it's caught |
|----------|----------------|
| User was in server when ban was approved | Deploy-time layer |
| User joined before bot processed the ban | Join-time layer |
| User was in server during brief bot downtime | Watchdog sweep |
| Bot just came online after restart | Watchdog sweep |
| Auto-apply was just enabled after sync | Watchdog sweep |

Between these three layers, no network-banned user can remain in a member server for more than 6 hours undetected.
