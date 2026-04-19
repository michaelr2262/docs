# FiveM bridge — wiring notes

## 0. Required environment variables

```bash
# AES-256-GCM key for encrypting webhook secrets at rest.
# Generate ONCE and never rotate without re-encrypting existing rows.
FIVEM_KEK=<64 hex chars>

# Optional — overrides the base URL baked into downloaded zips.
FIVEM_API_BASE=https://api.sentinelfx.net
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

## 3. Watchdog loop (optional)

Add to `utils/watchdog.js` so stale review cases don't sit forever:

```js
const fivem = require('../modules/fivemBridge');
setInterval(() => fivem.expireOldReviews().catch(() => {}), 6 * 3600_000);
```

---

## Ban modes

Per-server setting on `fivem_servers.ban_mode`:

| mode     | behaviour                                                                 |
|----------|---------------------------------------------------------------------------|
| `auto`   | In-game ban → immediate `network.deployNetworkBan` (Discord + all guilds) |
| `review` | Post an embed with Approve/Deny/Ignore buttons in `review_channel_id`     |
| `off`    | Only audit-logged to `fivem_events`; no Discord action                    |

Default for new servers: `review`.

### Change mode from the dashboard

```http
PATCH /fivem/servers/<server_id>
Content-Type: application/json

{ "banMode": "auto" }
# or
{ "banMode": "review", "reviewChannelId": "1234567890" }
```

### Button permissions

Only members with `BanMembers` or `Administrator` can resolve a case.
Decisions are immutable — once a case is resolved, the buttons disappear
and the embed colour reflects the outcome.

---

## FXServer-side resource (contract)

The in-game resource posts to:

```
POST https://api.sentinelfx.net/fivem/ingest
X-SentinelFX-Server:    <server_id>
X-SentinelFX-Signature: sha256=<hex hmac of raw body>
Content-Type:           application/json
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
    "length": "permanent"
  }
}
```

Accepted `type` values: `connect`, `disconnect`, `ban`, `chat`, `flag`.
Only `ban` triggers the review/auto pipeline — the rest just feed the
identity linker and HWID ledger.

---

## Tables created

| table                | purpose                                          |
|----------------------|--------------------------------------------------|
| `fivem_servers`      | registered FXServer instances + ban_mode         |
| `fivem_identities`   | cross-platform identifier map (discord/license/…)|
| `fivem_hwids`        | opt-in HWID ledger for evasion detection         |
| `fivem_events`       | append-only audit log of every ingested event    |
| `fivem_pending`      | review queue for `banMode='review'`              |
| `fivem_bad_codes`    | cfx.re join-code watchlist                       |
