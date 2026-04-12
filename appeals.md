---
description: How network-banned users can appeal and how the team reviews them.
---

# Appeals

<figure><img src="sfx_appeal.svg" alt="Appeal" width="64"></figure>

Any user with an active SentinelFX network ban can submit an appeal. If the SentinelFX Team approves it, the ban is lifted across every connected server simultaneously.

---

## For Banned Users — How to Appeal

### Step 1 — Visit the Dashboard

Go to [sentinelfx.net](https://sentinelfx.net) and log in with your Discord account. If you have an active network ban, an **Appeal** option will be available in your account area.

### Step 2 — Write Your Appeal

You must provide between **50 and 1,000 characters** explaining why your ban should be lifted. Be specific — vague appeals are less likely to be approved.

Things to include:
- Why you believe the original ban was incorrect or unfair
- Any context the team may not have had at the time
- What has changed since the incident

### Step 3 — Wait for a Decision

Your appeal goes into the SentinelFX Team's review queue. You cannot submit a second appeal while one is already open.

The team will mark your appeal as one of the following:

| Status | Meaning |
|--------|---------|
| `open` | Submitted, awaiting review |
| `under_review` | A team member is actively reviewing it |
| `approved` | Appeal granted — ban lifted across all servers |
| `denied` | Appeal rejected — ban remains |

{% hint style="info" %}
If your appeal is approved, you will be unbanned from every network server automatically. You do not need to rejoin or contact individual server owners.
{% endhint %}

---

## For the SentinelFX Team — Reviewing Appeals

<figure><img src="sfx_approved.svg" alt="Approved" width="48"></figure>

Appeals are reviewed through the **SentinelFX Dashboard** at [sentinelfx.net](https://sentinelfx.net). The dashboard shows the team:

- The full appeal text
- The user's complete network history
- Their current threat score
- The original ban reason and evidence link
- Status controls: **Approve**, **Deny**, **Under Review**

### Discord Notification

When a new appeal is submitted, a notification also appears in the HQ server so the team knows to check the dashboard. The HQ server provides quick **Approve** and **Deny** buttons that update the dashboard status in real time.

{% hint style="warning" %}
Once an appeal is marked **approved** or **denied**, its status cannot be changed again. Review the user's full history before approving.
{% endhint %}

---

## What Happens When a Ban is Lifted

When an appeal is approved (or a manual `/network-unban` is run):

1. The user's `network_bans` record is updated to `lifted`
2. The user is unbanned from every server with Auto-apply bans enabled
3. The number of servers they were unbanned from is reported back
4. A notification is sent to the HQ log channel

The user's ban history (including `prior_bans` count) is retained in the database for future reference. If they are network banned again, their history is factored into their threat score.
