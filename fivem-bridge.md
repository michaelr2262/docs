# FiveM bridge — wiring notes

## 0. Required environment variables

```bash
# AES-256-GCM key for encrypting webhook secrets at rest.
# Generate ONCE and never rotate without re-encrypting existing rows.
FIVEM_KEK=<64 hex chars>

# Optional — overrides the API base URL baked into rendered config.lua blocks.
FIVEM_API_BASE=https://api.sentinelfx.net/fxbridge

# Tebex distribution — link operators see in the dashboard wizard.
FIVEM_TEBEX_URL=https://sentinelfx.tebex.io/package/7412478
FIVEM_TEBEX_PACKAGE_ID=7412478
```

Generate the KEK:

```bash
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

If `FIVEM_KEK` is missing the module still loads and the dashboard tab still
shows, but registering / rotating a server will fail loudly rather than
storing plaintext secrets.

## 1. Boot (index.js)

After the Discord client is ready, prime the bridge once:

```js
const fivem = require('./modules/fivemBridge');
fivem.setClient(client);
await fivem.init();        // creates tables (safe to call every boot)
```

## 2. HTTP (api/server.js)

Mount the router anywhere after your auth middleware is registered:

```js
const fivem = require('./fivem');
app.use('/fivem', fivem.router);
```

## 3. Watchdog loop

Automatic: wired in `index.js` at boot — runs 60 s after start and every 6 h afterwards. No extra setup needed.

---

## Distribution model

The FXServer-side resource is shipped through **Tebex / CFX Asset Escrow**, not the dashboard. Operators:

1. Register the server on the dashboard → receive `serverId` + one-time `webhookSecret` + a rendered `config.lua` block.
2. Click through to Tebex (the wizard's **Step 1 — download** button hits `FIVEM_TEBEX_URL`), claim the asset on the **same Cfx.re account that owns their `sv_licenseKey`**.
3. Drop the `sentinelfx` folder into `resources/`, paste the rendered config block into `config.lua`, add `ensure sentinelfx` to `server.cfg`, restart.

The dashboard never serves the resource zip itself. The dashboard **does** serve the rendered `config.lua` block on demand (5-minute token TTL via `mintZipToken` / `peekZipToken`).

The wizard refuses to advance past the install step until the FXServer's first heartbeat has been received — `Test connection` polls `/api/fivem/:guildId/servers/:serverId/status` for up to 60 s and unlocks **Finish setup** only on `connected: true`.

---

## Ban modes

Per-server setting on `fivem_servers.ban_mode`:

| mode     | behaviour                                                                              |
|----------|----------------------------------------------------------------------------------------|
| `auto`   | In-game ban → immediate `network.deployNetworkBan` (Discord + all guilds)              |
| `review` | Post an embed with Approve / Deny / Ignore buttons in the **SentinelFX HQ** review channel (`REPORT_REVIEW_CHANNEL_ID` in the control server) |
| `off`    | Only audit-logged to `fivem_events`; no Discord action                                 |

Default for new servers: `review`.

There is **no per-guild review channel** — review cases route to the central HQ channel staffed by network admins. Operators do not configure a channel; they only pick the mode.

### Change mode from the dashboard

```http
PATCH /api/fivem/servers/<server_id>
Authorization: Bearer <dashboard session token>
Content-Type: application/json

{ "banMode": "auto" }
# or
{ "banMode": "review" }
# or
{ "banMode": "off" }
```

### Button permissions

Only members with `BanMembers` or `Administrator` in the **control server** (the SentinelFX HQ guild) can resolve a case. Decisions are immutable — once a case is resolved, the buttons disappear and the embed colour reflects the outcome.

---

## FXServer-side resource (contract)

The in-game resource posts to:

```
POST <FIVEM_API_BASE>/ingest/<server_id>?sig=sha256=<hex hmac of raw body>
Content-Type: application/json
```

Body shape:

```json
{
  "type":  "ban",
  "ts":    1745068800000,
  "player": {
    "name":        "OG Dave",
    "identifiers": ["discord:123", "license:abc", "steam:110000112345678", "fivem:5678"],
    "hardwareId":  "<sha256 of hardwareIdentifiers>",
    "endpoint":    "1.2.3.4"
  },
  "ban": {
    "reason": "cheat menu detected (Redengine)",
    "author": "ACE/cheat-scan",
    "length": "permanent",
    "source": "txadmin"
  }
}
```

Accepted `type` values: `connect`, `disconnect`, `ban`, `flag`, `ping`.
Only `ban` triggers the review/auto pipeline — the rest feed the
identity linker, HWID ledger, and live dashboard.

`ping` responses may contain a `kicks` array; the resource walks
connected players and drops anyone whose Discord ID matches an entry.

---

## Admin-framework event coverage (`server/bans.lua`)

The resource registers handlers for:

- `txAdmin:events:playerBanned` / `playerKicked` / `playerWarned` — documented txAdmin events, reliably fire.
- `EasyAdmin:bansUpdated` / `onPlayerBanned` / `onUserBanned` — version-dependent; not all EasyAdmin builds fire all three.
- `vMenu:BannedSuccessfully` / `BannedPlayerSuccessfully` — non-native; vanilla vMenu writes a JSON file rather than firing events. Only forks that emit these are caught.
- `bMenu:BannedSuccessfully` / `BannedPlayerSuccessfully` — same caveat as vMenu.
- `SentinelFX:reportBan(src, reason, author)` — universal escape hatch for any framework. Operators add a `TriggerEvent` (or `exports.sentinelfx:reportBan(...)`) call to their own admin code wherever the ban actually happens.

The custom hook is the recommended path for anything that isn't txAdmin.

---

## Tables created

| table                | purpose                                          |
|----------------------|--------------------------------------------------|
| `fivem_servers`      | registered FXServer instances + ban_mode         |
| `fivem_identities`   | cross-platform identifier map (discord/license/…) |
| `fivem_hwids`        | opt-in HWID ledger for evasion detection         |
| `fivem_events`       | append-only audit log of every ingested event    |
| `fivem_pending`      | review queue for `banMode='review'`              |
| `fivem_bad_codes`    | cfx.re join-code watchlist                       |
