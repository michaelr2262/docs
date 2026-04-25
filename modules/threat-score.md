---
description: How SentinelFX calculates a user's risk score across the entire network.
---

# Threat Score Engine

<figure><img src="../sfx_alert.svg" alt="Alert" width="64"></figure>

The Threat Score Engine assigns every user a risk score between **0 and 100** based on their full cross-network history. This score surfaces in `/userinfo`, in network ban embeds, in appeal review panels, and in alt detection alerts — giving your team an instant read on how much of a threat someone is before taking action.

---

## What Goes Into the Score

The score sums weighted factors, with caps that prevent any single factor from dominating.

| Factor | Effect |
|--------|--------|
| Active network ban | Strong increase |
| Prior lifted bans | Moderate increase per ban, capped overall |
| Cross-network warnings | Small increase per warning, capped overall |
| Pending reports | Small increase |

The maximum possible score is 100. The score is **computed live** every time it's requested — it always reflects the current state of the user's record.

---

## Risk Levels

<figure><img src="../sfx_online.svg" alt="Online" width="32"> <img src="../sfx_alert.svg" alt="Alert" width="32"> <img src="../sfx_banned.svg" alt="Banned" width="32"></figure>

| Score Range | Level | Indicator |
|-------------|-------|-----------|
| 0–29 | **LOW RISK** | 🟢 Green |
| 30–69 | **MEDIUM RISK** | 🟡 Amber |
| 70–100 | **HIGH RISK** | 🔴 Red |

---

## Where the Score Appears

- **On-join alert** — when a user joins any member server, their score is computed. If they land in the medium or high risk tier, an alert is pushed to your mod-log.
- **Network ban embed** — every network ban deployment includes the user's current score and risk level.
- **Appeal review** — when a user submits an appeal, the review includes their current score so SentinelFX can weigh it against the appeal.
- **Alt detection alerts** — shown on the alt-match notification when a potential alt is flagged on join.
- **Userinfo lookup** — the `/userinfo` command shows the score for the looked-up user.

---

## How the Score Changes Over Time

The score is dynamic. It rises as new bans, warnings, or reports land on the user, and it falls when bans are lifted on appeal. It is never frozen in place — a clean record over time naturally reduces a user's score, because the heaviest weights are tied to *active* bans rather than historical ones.

This means the threat score is a "right now" measure of risk, not a permanent label.
