---
description: Math CAPTCHA on join to block bot accounts before they reach your members.
---

# Verification System

<figure><img src="../sfx_verified.svg" alt="Verified" width="64"></figure>

The Verification module requires every new join to solve a simple maths problem before they can access the server. This filters out most bot accounts and automated join tools, which typically cannot interact with Discord modals.

---

## How It Works

When a user joins your server:

1. SentinelFX detects the join.
2. A verification challenge is sent to the user via DM.
3. If their DMs are closed, the challenge is posted in your configured welcome channel instead.
4. The user clicks **Verify**, which opens a modal with the maths problem.
5. They submit their answer. Correct answers grant the verified role; incorrect answers are rejected.

---

## The Challenge

The bot generates a random arithmetic problem on every join. Problems are one of three types:

- Addition (e.g. *47 + 28*)
- Subtraction (e.g. *84 − 36*)
- Multiplication (e.g. *6 × 9*)

Numbers are randomised each time. The user has a few minutes to complete verification before the challenge expires.

---

## The Verified Role

Once a user answers correctly:

1. They receive the role configured as the verified role.
2. The verification record is cleared.
3. The event is logged to your mod-log channel.

{% hint style="info" %}
Make sure the verified role is the gate to your normal channels. Members without it should only see a verification channel where the bot's welcome message appears.
{% endhint %}

---

## Enabling Verification

Use `/config toggle` to enable verification, `/config set-role` to set the verified role, and `/config set-channel` to set the welcome channel as a DM-fallback destination.

---

## Channel Setup Tip

A typical verification setup in Discord:

1. Create a verification channel visible to everyone.
2. Lock all other channels to require the verified role.
3. Set the verification channel as your welcome channel in SentinelFX.
4. The bot will post the verification challenge there if a user's DMs are closed.

{% hint style="warning" %}
If neither DMs nor the welcome channel is configured, verification challenges will be silently dropped. Always set a welcome channel as a fallback.
{% endhint %}

---

## Cleanup

Pending verification records that go unanswered are automatically removed after a few minutes by the [Watchdog](watchdog.md). This keeps the queue tidy and prevents stale challenges from hanging around.
