---
description: Detecting network-banned users returning under new accounts.
---

# Alt Account Detection

<figure><img src="../sfx_scanning.svg" alt="Scanning" width="64"></figure>

When a user is network banned, they sometimes create a new ("alt") account with a similar username to try to rejoin servers. The Alt Detection module catches this by comparing every new join's username against the full list of network-banned users.

---

## How It Works

Every time a user joins a server, Alt Detection runs two checks.

### Check 1 — Account Age

If the account is recently created (under a month old), it proceeds to the username check. Older accounts are not flagged because they're more likely to be legitimate.

### Check 2 — Username Similarity

The module compares the new account's username to every network-banned user's username, ignoring case and non-letter characters, and asks: *how close are these two strings?*

If the two are very similar — small spelling changes, extra letters, number-for-letter substitutions — and both names are at least a few characters long, the account is flagged as a potential alt.

---

## Example

| New Account | Banned User | Flagged? |
|-------------|-------------|---------|
| X4erus_alt | Xaerus | Possibly — depends on closeness threshold |
| XaerusFX | Xaerus | Yes |
| Xar3us | Xaerus | Yes |

---

## What Happens When an Alt is Flagged

When a potential alt is detected, SentinelFX is alerted with:

- The new account's username and join date.
- The matched banned user's username and ban reason.
- The confidence percentage of the match.
- The new account's age in days.

The team then reviews the alert and decides whether to action it. **Alt Detection does not automatically ban** — it surfaces the information for human review.

---

## Threshold

The default similarity threshold is tuned to catch obvious alts (small spelling changes, extra letters, number substitutions) while minimising false positives from genuinely different usernames that happen to share common letters.

Both names must also be at least a few characters long — very short names are excluded from matching to avoid false positives.
