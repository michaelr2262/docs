---
description: Detecting network-banned users returning under new accounts.
---

# Alt Account Detection

<figure><img src="../sfx_scanning.svg" alt="Scanning" width="64"></figure>

When a user is network banned, they sometimes create a new ("alt") account with a similar username to try and rejoin servers. The Alt Detection module catches this by comparing every new join's username against the full list of network-banned users using a similarity algorithm.

---

## How It Works

Every time a user joins a server, Alt Detection runs two checks:

### Check 1 — Account Age

If the account is **30 days old or newer**, it proceeds to the username check. Older accounts are not flagged (they're more likely to be legitimate).

### Check 2 — Username Similarity

The module normalises both usernames (lowercase, all non-letter characters removed) and computes the **Levenshtein edit distance** between them.

**Levenshtein distance** counts the minimum number of single-character edits (insertions, deletions, substitutions) needed to turn one string into another.

```
normalized_new  = lowercase(remove non-alpha chars from new username)
normalized_ban  = lowercase(remove non-alpha chars from banned username)

similarity = 1 - (levenshtein(a, b) / max(len(a), len(b)))

if similarity >= 0.75 AND len(a) >= 3 AND len(b) >= 3:
    → Flag as potential alt
    → Report best match and confidence percentage
```

---

## Example

| New Account | Banned User | Normalised New | Normalised Ban | Similarity | Flagged? |
|-------------|-------------|----------------|----------------|------------|---------|
| `X4erus_alt` | `Xaerus` | `xaerusalt` | `xaerus` | 65% | ❌ No |
| `XaerusFX` | `Xaerus` | `xaerusfx` | `xaerus` | 78% | ✅ Yes |
| `Xar3us` | `Xaerus` | `xarus` | `xaerus` | 80% | ✅ Yes |

---

## What Happens When an Alt is Flagged

When a potential alt is detected, the SentinelFX Team is alerted with:

- The new account's username and join date
- The matched banned user's username and ban reason
- The confidence percentage
- The new account's age in days

The team then reviews the alert and decides whether to action it. Alt Detection does **not** automatically ban — it surfaces the information for human review.

---

## Threshold

The default similarity threshold is **75%**. This is tuned to catch obvious alts (small spelling changes, extra letters, number substitutions) while minimising false positives from genuinely different usernames that happen to share common letters.

Both normalised names must also be **at least 3 characters long** — very short names are excluded from matching to avoid false positives.
