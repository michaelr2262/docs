---
description: How SentinelFX calculates a user's risk score across the entire network.
---

# Threat Score Engine

<figure><img src="../sfx_alert.svg" alt="Alert" width="64"></figure>

The Threat Score Engine assigns every user a risk score between **0 and 100** based on their full cross-network history. This score surfaces in `/userinfo`, report review panels, and alt detection alerts — giving your team an instant read on how much of a threat someone is before taking action.

---

## Score Breakdown

The score is calculated by summing weighted factors, with caps to prevent any single factor from dominating.

| Factor | Points | Cap |
|--------|--------|-----|
| Active network ban | +50 | — |
| Prior lifted bans | +20 each | 30 pts |
| Cross-network warnings | +5 each | 30 pts |
| Pending reports | +15 each | 15 pts |

**Maximum possible score: 100**

### Example Calculations

| User History | Score |
|-------------|-------|
| No history | 0 |
| 4 warnings | 20 |
| 1 pending report | 15 |
| 1 prior lifted ban + 3 warnings | 35 |
| 2 prior lifted bans + active ban | 80 |
| Active ban + 2 prior + 4 warnings + 1 pending | 100 (capped) |

---

## Risk Levels

<figure><img src="../sfx_online.svg" alt="Online" width="32"> <img src="../sfx_alert.svg" alt="Alert" width="32"> <img src="../sfx_banned.svg" alt="Banned" width="32"></figure>

| Score Range | Level | Indicator |
|-------------|-------|-----------|
| 0 – 29 | **LOW RISK** | 🟢 Green |
| 30 – 69 | **MEDIUM RISK** | 🟡 Amber |
| 70 – 100 | **HIGH RISK** | 🔴 Red |

---

## Where the Score Appears

- **On-join alert** — when a user joins any member server, their score is computed. If they land in the medium or high risk tier, an alert is pushed to your mod-log.
- **Network Ban Registered embed** — posted to the HQ ban channel every time a new network ban is deployed, with a 0–100 score bar and risk label.
- **Appeal review embed** — when a user submits an appeal, the HQ review post includes their current score so staff can weigh it against the appeal.
- **Alt Detection alerts** — shown on the alt-match embed when a potential alt is flagged on join.

---

## Algorithm Detail

```
score = 0

if user has active network ban:
    score += 50

prior_bans = number of previously lifted network bans
score += min(prior_bans × 20, 30)

warning_count = total network warnings
score += min(warning_count × 5, 30)

pending_count = open reports against user
score += min(pending_count × 15, 15)

score = min(score, 100)
```

The score is **computed live** every time it's requested — it always reflects the current state of the database.
