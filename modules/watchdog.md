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
| Network sweep | Several times per day | Scans every member server for banned users |
| Temp-ban expiry | Every minute | Lifts bans whose duration has expired |
| Verification cleanup | Every few minutes | Removes expired pending verification challenges |

---

## Network Sweep

The network sweep is the Watchdog's most important job. It:

1. Reads the full list of active network bans.
2. For each member server with **Auto-apply Bans** enabled:
   - Reads the current member list.
   - Cross-references every member against the ban list.
   - Bans any match it finds.
3. Skips server owners (they cannot be banned by a bot).
4. Sends a *Watchdog Sweep* notification embed to each server's network ban channel.

{% hint style="info" %}
The sweep uses different embed wording depending on how a ban was applied. A ban applied during a sweep says *Watchdog Sweep* so your moderators know it was automated rather than a fresh deployment.
{% endhint %}

---

## Ban Deployment (Real-Time)

The Watchdog also handles immediate deployment when a **new ban is approved**. This isn't scheduled — it fires instantly:

1. The ban is recorded.
2. Every member server is checked for the user's presence.
3. If found, the user is banned immediately with an *Active at Time of Ban* embed.

This is the **deploy-time layer** of SentinelFX's three-layer defence.

---

## Temporary Ban Expiry

Every minute, the Watchdog checks for temp-bans that have reached their expiry time. For each expired ban:

1. The Discord ban is lifted.
2. The temp-punishment record is removed.
3. The user can rejoin the server normally.

This means temporary bans are accurate to within a minute, even if there's a brief downtime.

---

## Why Three Layers?

<figure><img src="../sfx_scanning.svg" alt="Scanning" width="48"></figure>

No single check is bulletproof. The Watchdog's sweep exists as a safety net for edge cases:

| Scenario | How it's caught |
|----------|----------------|
| User was in server when ban was approved | Deploy-time layer |
| User joined before the ban was processed | Join-time layer |
| User was in server during brief downtime | Watchdog sweep |
| Auto-apply was just enabled after sync | Watchdog sweep |

Between these three layers, no network-banned user can remain in a member server undetected for long.
