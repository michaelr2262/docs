---
description: The background systems that power SentinelFX.
---

# Modules Overview

Modules are the automated systems running behind every SentinelFX server. They don't need commands — they watch, analyse, and act on their own.

---

<table data-view="cards">
<thead><tr><th></th><th></th></tr></thead>
<tbody>
<tr><td><img src="../sfx_alert.svg" width="48"><br><strong>Threat Score Engine</strong></td><td>Assigns every user a 0–100 risk score from their cross-network history.</td></tr>
<tr><td><img src="../sfx_watchdog.svg" width="48"><br><strong>Watchdog Scheduler</strong></td><td>Runs network sweeps and temp-ban expiry automatically, throughout the day.</td></tr>
<tr><td><img src="../sfx_shield.svg" width="48"><br><strong>Auto-Moderation</strong></td><td>Detects spam, scam links, banned words, and mass mentions in real time.</td></tr>
<tr><td><img src="../sfx_alert.svg" width="48"><br><strong>Anti-Raid</strong></td><td>Detects mass-join waves and auto-locks the server.</td></tr>
<tr><td><img src="../sfx_verified.svg" width="48"><br><strong>Verification</strong></td><td>Math CAPTCHA on every join to block bot accounts.</td></tr>
<tr><td><img src="../sfx_scanning.svg" width="48"><br><strong>Alt Detection</strong></td><td>Flags new accounts with similar usernames to known banned users.</td></tr>
</tbody>
</table>

---

{% content-ref url="threat-score.md" %}Threat Score Engine{% endcontent-ref %}
{% content-ref url="watchdog.md" %}Watchdog Scheduler{% endcontent-ref %}
{% content-ref url="auto-mod.md" %}Auto-Moderation{% endcontent-ref %}
{% content-ref url="anti-raid.md" %}Anti-Raid System{% endcontent-ref %}
{% content-ref url="verification.md" %}Verification System{% endcontent-ref %}
{% content-ref url="alt-detection.md" %}Alt Account Detection{% endcontent-ref %}
