---
description: Link-based verification with cross-network risk scoring.
---

# Verification System

<figure><img src="../sfx_verified.svg" alt="Verified" width="64"></figure>

The Verification module sends every new join a one-time link to the SentinelFX website. The page checks the joining account against the cross-network ban list, scores their threat profile, looks for ban-evasion signals, and confirms a real human is on the other end before granting access to your server.

It replaces the old math-CAPTCHA flow. You don't lose the per-server toggle, the verified role, or the welcome-channel fallback — just the in-Discord modal.

---

## How It Works

When a user joins your server, SentinelFX:

1. Mints a one-time verification token tied to that user and that server.
2. DMs the user a private link of the form `https://sentinelfx.net/verify/<token>`. The DM includes an **Open verification** button that opens the same link.
3. If the user's DMs are closed, posts the same prompt in your configured welcome channel.
4. Waits up to 3 minutes for the user to complete verification on the SentinelFX website.

On the website the page does two things in sequence:

| Step | What happens |
|------|--------------|
| **1. Risk scan** | Server-side: cross-checks the account against the SentinelFX network ban list, computes the user's cross-network threat score, runs alt-detection against banned-username patterns, examines the joining account's age, and looks at recent verification attempts from the same IP. |
| **2. Human check** | Page-side: collects pointer entropy, hold-button duration, locale and timezone, browser markers (headless flags, plugin count, hardware concurrency), and submits everything alongside the on-page challenge. |

Both layers feed into a single risk score. The result is one of three outcomes.

---

## Verdicts

| Outcome | What happens to the user |
|---------|--------------------------|
| **Pass** | The verified role is granted, the join is logged to mod-log as `Verified`. |
| **Manual review** | The verified role is granted, but mod-log is flagged with the elevated risk score and the specific signals that fired. The user gets in; your staff get a heads-up. |
| **Fail** | Either the user is kicked from the server with a warning record (default) or denied the verified role and quarantined for staff to review (configurable). Mod-log records `Verification Failed — Kicked` or `… — Quarantined` accordingly. |

Some signals are hard-fails on their own:

- **Headless / automation markers in the browser** — `navigator.webdriver`, headless user-agent strings, missing standard browser fields. These never depend on guild config; if a real automated client is detected, verification stops.
- **Failing the on-page hold challenge** — the button must be held for the full duration; releases or zero-pointer-motion clients fail.

Other signals add risk weight rather than auto-failing:

- Recently created Discord account (under 12 hours, 24 hours, 7 days, 30 days — heavier weight the younger the account)
- Multiple verification attempts from the same IP in the last hour
- Disabled cookies, no plugins, no detected timezone, very small or absent screen geometry

A user with no risk signals and a normal challenge response sails through in seconds. A user with several mid-weight signals lands in **manual review** so staff can decide whether to trust them.

---

## Cross-Network Signals & the Auto-Apply Network Bans Switch

Three checks involve data from the SentinelFX network as a whole, not just your server:

- **Network ban list** — does the user have an active SentinelFX network ban?
- **Account history** — what's the user's cross-server threat score (warnings, prior bans, pending reports)?
- **Evasion patterns** — does the joining username closely match a known network-banned user?

Whether a positive hit on those signals **fails** verification — kicking or quarantining the user — is gated by your **Auto-Apply Network Bans** toggle in the main Configuration tab. The toggle is the single switch that controls whether your server acts on cross-network signals **anywhere**: at join, during sweeps, and during verification. Verification respects the same policy.

| Auto-Apply Network Bans | Cross-network signal hit | Verification page shows | Verification verdict |
|---|---|---|---|
| **On** | Network ban / high threat score | **Red flag** — verification denied | Fails — user is kicked or quarantined |
| **Off** | Network ban / high threat score | **Yellow flag** — visible warning, no action | Passes — the user gets in despite the signal |
| **(either)** | No signal hit | Green check | Passes |

This means a server that wants to participate in the network *informationally* — receive reports, file reports, see who's flagged — without auto-banning its members can simply set Auto-Apply Network Bans off. Verification will still surface every cross-network signal on the page so the user (and any staff who look at the mod-log audit trail) can see the picture, but no kick fires.

---

## What the Page Asks the User to Do

A short hold-the-button challenge — a few seconds of pressing a single button until a progress bar fills. The duration scales with the preliminary risk score:

- Low risk: ~1.5 seconds
- Medium risk: ~2.5 seconds
- High risk: ~4 seconds

That's it. There is no maths, no image puzzle, no third-party reCAPTCHA. The challenge exists to confirm a real pointer is moving on a real screen — the heavy lifting is the cross-network risk scoring underneath.

The dial stays locked until all four scan blocks (Network ban list, Account history, Evasion patterns, Automation markers) finish their checks. On any hard-fail signal the dial never unlocks; the page auto-submits a forced fail so the kick happens promptly.

---

## Privacy

The verification page records only what's needed to confirm a real person is on the other end:

- Pointer movement count and total distance during the visit
- How long the hold-button was pressed
- Time on page
- Browser locale and timezone (from `Intl.DateTimeFormat`)
- Browser markers: `navigator.webdriver`, plugin count, hardware concurrency, screen size, cookies enabled
- A salted hash of the visitor's IP (used only to detect velocity from the same address)

It does **not** record password input, browsing history, or any data unrelated to the verification check. Full disclosure is in the [Privacy Policy](https://sentinelfx.net/privacy).

---

## Enabling Verification

Use `/config toggle` to enable verification, `/config set-role` to set the verified role, and `/config set-channel` to set a welcome channel as a DM-fallback destination. Members without the verified role should only be able to see a verification channel where the bot's welcome message appears.

---

## Verification Settings (Manage Modal)

The Verification toggle in the dashboard's **Configuration** tab has a **Manage** button next to it. Clicking it opens a settings panel grouped into two sections.

### Risk signals

Master switches for the three cross-network checks. Each one controls whether the corresponding scan block runs at all. Default: all on.

- **Use network ban list** — check the joining account against the SentinelFX network ban list. Whether a match auto-fails depends on the main Auto-Apply Network Bans toggle.
- **Use cross-server threat score** — check cross-network warnings, prior bans, and pending reports. Same gating rule.
- **Use alt-evader detection** — flag verification when the joining account's username closely resembles a known network-banned user. Always informational — never auto-fails on its own.

### Verdict & action

Controls what happens when verification fails.

- **Kick on failed verification** *(default on)* — when off, a failed user stays in the server but is denied the verified role; staff get a `Verification Failed — Quarantined` mod-log entry to action manually. Useful for guilds that want a quarantine workflow rather than a hard kick.
- **Kick threshold** *(default 70)* — the risk score (20–100) at which verification fails. Lower = stricter; higher = more lenient. The slider has anchor labels at Strict (20), Default (70), and Lenient (100). Changes apply on the next verification submit.

The threshold is clamped server-side to `[20, 100]` regardless of what's stored, so an out-of-range value can't accidentally disable the kick path or trigger on every join.

---

## Channel Setup Tip

A typical setup in Discord:

1. Create a verification channel visible to everyone.
2. Lock all other channels to require the verified role.
3. Set the verification channel as your welcome channel in SentinelFX.
4. Members who have DMs closed will see the verification prompt posted there instead of by DM.

{% hint style="warning" %}
If neither DMs nor the welcome channel is configured, verification prompts will be silently dropped. Always set a welcome channel as a fallback.
{% endhint %}

---

## When the Link Expires

Tokens are valid for **3 minutes** from issue. If the user closes the tab or never opens the link, the bot's watchdog handles cleanup; if they're on the page when the timer hits zero, the page itself takes over: a full-screen *Session Expired* overlay appears, the bot is told to finalise the verification immediately, and the user is removed from the server (subject to the Kick on failed verification toggle).

In both cases the verification record is preserved on the audit trail for a day before being pruned.
