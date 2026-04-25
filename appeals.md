---
description: How network-banned users can appeal and how the team reviews them.
---

# Appeals

<figure><img src="sfx_appeal.svg" alt="Appeal" width="64"></figure>

Any user with an active SentinelFX network ban can submit an appeal. If the appeal is approved, the ban is lifted across every connected server simultaneously.

---

## How to Appeal

### Step 1 — Use `/appeal`

In any SentinelFX-protected server you still share with the bot, run the `/appeal` command. This opens a modal titled **Network Ban Appeal**.

If you no longer share a server with the bot, skip to the dashboard alternative below.

### Step 2 — Write Your Appeal

The modal has a single paragraph field — *Why should your ban be lifted?* — that accepts between **50 and 1,000 characters**. Be specific — vague appeals are less likely to be approved.

Things to include:

- Why you believe the original ban was incorrect or unfair
- Any context the team may not have had at the time
- What has changed since the incident

### Step 3 — Wait for a Decision

Your appeal goes into the SentinelFX review queue. You cannot submit a second appeal while one is already open.

The team will mark your appeal as one of the following:

| Status | Meaning |
|--------|---------|
| Open | Submitted, awaiting review |
| Under review | A team member is actively reviewing it |
| Approved | Appeal granted — ban lifted across all servers |
| Denied | Appeal rejected — ban remains |

{% hint style="info" %}
If your appeal is approved, you will be unbanned from every network server automatically. You do not need to rejoin or contact individual server owners.
{% endhint %}

### Alternative — Dashboard

If you don't share a server with SentinelFX, log in at [sentinelfx.net](https://sentinelfx.net) with Discord. If you have an active network ban, a **Submit Appeal** button appears next to it in your account area. The same 50–1,000 character rule applies.

---

## What Happens When a Ban is Lifted

When an appeal is approved:

1. The user is unbanned from every server with Auto-apply Bans enabled.
2. The lift is logged in your server's mod-log channel (where the original deployment was logged).
3. The user's history is retained for future reference — if they're network banned again later, the prior ban is factored into their threat score.

---

## A Note on Appeal Decisions

Appeals are reviewed by humans. Even when SentinelFX's automated triage layer was involved in the original ban, the appeal review is always a human-only step — no AI is involved in the decision to lift or uphold a ban.

Once an appeal is marked approved or denied, that decision is final. If new evidence emerges later, you can submit a fresh appeal.
