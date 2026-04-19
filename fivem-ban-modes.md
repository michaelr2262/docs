---
description: Pick how in-game bans turn into network bans — off, review, or auto.
---

# Ban Modes & Propagation

<figure><img src="sfx_banned.svg" alt="Ban modes" width="64"></figure>

Every FXServer registered with SentinelFX has a **ban mode** that decides what happens when your in-game scripts — a cheat detector, an admin menu, an ACE rule — issue a ban.

There are three modes. You set one when you register the server, you change it any time from the dashboard, and the change takes effect immediately.

---

## The Three Modes

| Mode     | In-game ban | Discord action | Network action | When to pick it |
|----------|-------------|----------------|----------------|-----------------|
| `off`    | Logged to audit trail | None | None | You want telemetry and HWID tracking but no automated punishment. |
| `review` | Logged, and a review case is posted to your staff channel and the dashboard | Only if staff approve | Only if staff approve | **Default.** Good balance of speed and human oversight. |
| `auto`   | Logged, and immediately deployed network-wide | Instant ban in every guild (if Discord ID is known) | Instant add to the network ban list | You have a cheat-scan you trust absolutely and want zero latency. |

---

## `off` — Log Only

<figure><img src="sfx_scanning.svg" alt="Log only" width="48"></figure>

The event lands in SentinelFX's audit log and the dashboard event stream, and the player's identifiers and hardware fingerprint are merged into the identity ledger — but nothing else happens. No Discord ban, no network ban, no staff notification.

**Pick this when:**

- You're evaluating the integration and not ready to let it act yet.
- You want the dashboard, identity ledger, and HWID evasion flags but prefer to handle punishment entirely inside your game.
- You're running a test server.

---

## `review` — Staff Decides (Default)

<figure><img src="sfx_appeal.svg" alt="Review" width="48"></figure>

When your in-game scripts issue a ban, SentinelFX opens a **review case**. An embed goes to the review channel you configured for that server, with three buttons:

| Button | Action |
|--------|--------|
| **Approve ban** | Deploys a full network ban — Discord across every guild, plus the FiveM ban list. |
| **Deny** | Marks the case as wrongly flagged. No action taken. |
| **Ignore (log only)** | Closes the case without action, but keeps the audit record. |

The review case shows the player's Discord account (if known), their in-game name, the identifiers they connected with, the HWID, the ban reason, and the source script that raised it.

### Who Can Press the Buttons

Only members with the **Ban Members** permission or **Administrator** on the Discord server can resolve a case. Clicks from anyone else are rejected with a friendly error.

### Immutable Decisions

Once a case is resolved — approved, denied, or ignored — the buttons disappear and the embed colour changes to reflect the outcome. You cannot re-open a closed case; if you change your mind, issue a fresh ban or a fresh appeal.

### Stale Cases

If a review case sits open for longer than a week, the Watchdog marks it `expired` so it stops cluttering the queue. The audit record stays.

**Pick this when:**

- You want a human in the loop before a network-wide action.
- Your cheat-scan occasionally false-positives.
- Your staff channel is active enough that cases get answered in a reasonable window.

---

## `auto` — Immediate Network Ban

<figure><img src="sfx_alert.svg" alt="Auto mode" width="48"></figure>

The ban is deployed the instant it arrives. There is no review step. SentinelFX:

1. Writes the ban into the network ban list (immediately blocks the player on every FXServer running the resource).
2. If a Discord ID is known for the player, bans them across every guild in the network.
3. Posts the action to your configured network-alerts channel.

**Pick this when:**

- You've been running in `review` mode long enough to trust your scan.
- Your scan's false-positive rate is effectively zero.
- You'd rather deal with rare appeals than give cheaters a window of extra minutes.

{% hint style="warning" %}
`auto` mode means one bad regex in your cheat-scan can cross-ban real players across the entire network. Use it deliberately.
{% endhint %}

---

## Changing the Mode

<figure><img src="sfx_approved.svg" alt="Change mode" width="48"></figure>

Open the dashboard, go to the **FiveM** tab, pick your server, and switch the mode from the dropdown. The change is live the moment you save — no restart, no resource re-download, no FXServer reboot.

You can change it as often as you like. A common pattern:

- Start in `review` for the first few weeks.
- Watch the review queue — how often does staff approve vs deny?
- If approvals are overwhelmingly the outcome and your scan looks solid, switch to `auto`.
- If you see any deny pile up, stay in `review`.

---

## What Happens to Discord

A network ban can only propagate to Discord **when SentinelFX knows the player's Discord ID**. There are two ways it finds out:

1. **Cfx.re linked accounts** — if the player has linked their Discord to their Cfx.re account, FiveM exposes the identifier and SentinelFX picks it up automatically.
2. **Prior activity** — if the player was ever banned through Discord first, or if they connected while logged into Discord on another SentinelFX-connected FXServer, the identity ledger already knows them.

If neither is true, the ban applies **only on the FiveM side** — they're blocked from every FXServer in the network, but they remain free to behave themselves on Discord. The FiveM ban list is identifier-keyed (license, Steam, Xbox, Cfx.re, HWID), so they're still caught everywhere they'd try to reconnect.

---

## See Also

{% content-ref url="fivem.md" %}
FiveM Integration Overview
{% endcontent-ref %}

{% content-ref url="fivem-resource.md" %}
Installing the Resource
{% endcontent-ref %}

{% content-ref url="network-protection.md" %}
How the Network Works
{% endcontent-ref %}
