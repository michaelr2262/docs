---
description: Extend SentinelFX protection into your FXServer — one ban list, every platform.
---

# FiveM Integration

<figure><img src="sfx_network.svg" alt="FiveM Integration" width="64"></figure>

SentinelFX started as a Discord-only network, but the people causing trouble on Discord and the people causing trouble in FiveM are usually the **same people**. The FiveM integration closes that loop: one ban, enforced everywhere.

It's opt-in, per FXServer, and takes about five minutes to install.

---

## What the Integration Does

<table data-view="cards">
<thead><tr><th></th><th></th></tr></thead>
<tbody>
<tr><td><img src="sfx_banned.svg" width="48"><br><strong>Ban Mirroring</strong></td><td>Network bans block matching players at the FXServer connect stage, on every server running the resource.</td></tr>
<tr><td><img src="sfx_verified.svg" width="48"><br><strong>Identity Linking</strong></td><td>Every identifier a player uses — license, Steam, Xbox, Discord, Cfx.re — is stitched into one profile so one ban covers all alts.</td></tr>
<tr><td><img src="sfx_scanning.svg" width="48"><br><strong>HWID Evasion Guard</strong></td><td>Hardware tokens catch banned users on brand-new accounts before they load in.</td></tr>
<tr><td><img src="sfx_online.svg" width="48"><br><strong>Live Stats</strong></td><td>Uptime, player counts, event histograms, review queue — all on the dashboard, in real time.</td></tr>
<tr><td><img src="sfx_alert.svg" width="48"><br><strong>In-Game Ban Review</strong></td><td>Bans from ACE cheat-scans route to the SentinelFX HQ review channel with Approve / Deny / Ignore buttons, decided by network admins.</td></tr>
<tr><td><img src="sfx_shield.svg" width="48"><br><strong>Cross-Platform Coverage</strong></td><td>A ban issued on Discord propagates into FiveM. A reviewed in-game ban propagates onto Discord. One decision, both sides.</td></tr>
</tbody>
</table>

---

## How It Works, End to End

### 1 — The Resource Calls Home

The SentinelFX resource, running inside your FXServer, sends a small signed message to SentinelFX every time a relevant event happens: a player connects, disconnects, gets banned by your cheat-scan, or sends a flagged chat line. It also pings every 5 minutes so the dashboard knows your server is alive.

### 2 — Identifiers Get Stitched Together

Every message includes the player's identifiers — their Rockstar license, Steam ID, Xbox Live ID, Cfx.re account, Discord account (when exposed), and a hardware fingerprint. SentinelFX merges them into a single profile in the **identity ledger**. The next time that player shows up on any server — even a server they've never played before — SentinelFX knows it's the same person.

### 3 — Bans Propagate

When a ban is issued in-game, what happens next depends on your server's **ban mode**:

| Mode     | What happens |
|----------|-------------|
| `off`    | Event is logged. No Discord or network action. |
| `review` | A case is posted to the **SentinelFX HQ** review channel (and the dashboard) with **Approve / Deny / Ignore** buttons. A network admin decides. |
| `auto`   | Immediate network ban — enforced across Discord and every FXServer in the network. |

See [Ban Modes & Propagation](fivem-ban-modes.md) for the full trade-off guide.

### 4 — The Network Stops the Alt

If the banned player comes back on a new Rockstar account but the same machine, the hardware fingerprint gives them away. If they swap the machine too but keep their Discord, the Discord identifier gives them away. Breaking through all of it at once is genuinely difficult, and by then there's a paper trail.

---

## What the Integration Does **Not** Do

SentinelFX is a polite houseguest. It doesn't take liberties with your FXServer.

| It does not… | Meaning |
|--------------|---------|
| Open any inbound ports | All traffic is outbound, from your server to SentinelFX. Nothing connects in. |
| Read your server files | The resource doesn't touch your framework, database, scripts, or assets. |
| Execute shell commands | No RCE surface, no privileged operations. |
| Rummage through your database | It has no credentials to anything except its own API. |
| Modify `server.cfg` or other resources | It runs alongside everything else. You stay in control. |
| Require a public IP or port forward | Only outbound HTTPS. Works on any host, behind any NAT. |

If it sounds minimal, that's the point. The resource does one job: forward a signed event and receive a signed ban list.

---

## Security Model

<figure><img src="sfx_shield.svg" alt="Security" width="48"></figure>

- **Per-server signed secret** — each FXServer gets its own secret, generated on registration, shown once. Every message is cryptographically signed with it.
- **Encrypted at rest** — the secret is encrypted on the SentinelFX side before it's stored. Even a database leak doesn't expose live secrets.
- **One-click rotation** — if a secret ever leaks, rotate it from the dashboard and re-download the resource zip. Previous signatures stop working instantly.
- **Rate limited** — both ingest and ban-list fetch are rate limited in both directions.
- **Fails quietly** — if SentinelFX is unreachable, the resource does not start kicking real players. It logs, backs off, and retries. Only verified network-ban matches ever block a connection.

---

## Getting Started

{% content-ref url="fivem-resource.md" %}
Installing the Resource
{% endcontent-ref %}

{% content-ref url="fivem-ban-modes.md" %}
Ban Modes & Propagation
{% endcontent-ref %}

{% content-ref url="dashboard.md" %}
Dashboard — FiveM Server View
{% endcontent-ref %}
