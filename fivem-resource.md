---
description: Install the SentinelFX resource on your FXServer in five minutes.
---

# Installing the Resource

<figure><img src="sfx_online.svg" alt="Install" width="64"></figure>

The SentinelFX FiveM resource is a small server-side script that plugs your FXServer into the network. It's distributed through the official **Tebex / CFX Asset Escrow** channel — the same way you install any premium FiveM resource — and configured per-server through a single `config.lua` file.

{% hint style="info" %}
You only need to do this once per FXServer. If you run multiple FXServers, register each one separately on the dashboard — every server gets its own `config.lua` values.
{% endhint %}

---

## Before You Start

You'll need:

- A **SentinelFX member server** (the Discord side already set up — see [Getting Started](getting-started.md)).
- **Administrator access** on the Discord server that owns the FXServer.
- **File access** to your FXServer's `resources/` directory.
- A **Cfx.re account** that owns your FXServer's `sv_licenseKey`. You'll claim the SentinelFX asset on Tebex while signed into this exact account.
- The ability to **restart** the FXServer once.

That's it. No ports to open, no firewall rules, no database setup.

---

## ⚠ Critical — Cfx.re Account Ownership

The SentinelFX resource ships through CFX Asset Escrow. The CFX runtime checks, on every FXServer boot, that the account behind your `sv_licenseKey` owns an entitlement to the asset.

**This means:** when you click **Get** on the SentinelFX Tebex page, you must be signed in to **the same Cfx.re account that owns your server's `sv_licenseKey`** — not a personal account, not a friend's account, not a different studio account. The entitlement attaches to whichever account does the click; once it's attached, it cannot be moved.

If you claim it on the wrong account, the FXServer console will refuse to start the resource with:

```
[citizen-server-impl] You lack the required entitlement to use sentinelfx
[citizen-server-impl] Couldn't start resource sentinelfx
```

The fix in that situation is to either generate a fresh `sv_licenseKey` under the entitled account at [keymaster.fivem.net](https://keymaster.fivem.net/), or re-claim the asset on the right account. There is no entitlement transfer.

**How to verify which account owns your key:** sign in to [keymaster.fivem.net](https://keymaster.fivem.net/) — the keys listed there belong to the account you're signed in as. The one matching the `sv_licenseKey` line in your `server.cfg` is the account you must use on Tebex.

---

## Step 1 — Register the Server on the Dashboard

Log into the dashboard at [sentinelfx.net](https://sentinelfx.net) and open the **FiveM** tab for your Discord server. Enable the bridge, then run through the setup wizard. In the **Configure Server** step you'll fill in:

| Field | What to put |
|-------|------------|
| **Server name** | A label you'll recognise — e.g. `Main Roleplay`, `Dev Test`. |
| **Cfx code** *(optional)* | Your cfx.re join code, if you have one. |
| **Ban mode** | Start with `review`. You can change it any time. |

Hit **Create server**. The dashboard generates the per-server config block (`serverId`, `webhookSecret`, `apiBase`, `serverName`) for you to paste in later.

---

## Step 2 — Pick a Ban Mode

<figure><img src="sfx_shield.svg" alt="Ban modes" width="48"></figure>

| Mode     | When to use |
|----------|-------------|
| `review` | **Default.** In-game bans queue at SentinelFX HQ for network-admin approval before hitting the network. Safest starting point. |
| `auto`   | You trust your cheat-scan completely and want zero-latency network bans. |
| `off`    | You want telemetry, HWID evasion detection, and dashboard stats, but no automatic network action. |

The mode is changeable from the dashboard at any time — no restart needed. There is **no per-guild review channel**: review cases are routed to the central SentinelFX HQ review channel staffed by network admins. See [Ban Modes & Propagation](fivem-ban-modes.md) for the full comparison.

---

## Step 3 — Download the Resource from Tebex

The wizard's **Download & Install** step links to:

> [https://sentinelfx.tebex.io/package/7412478](https://sentinelfx.tebex.io/package/7412478)

Click **Open SentinelFX on Tebex**, sign in **with the Cfx.re account that owns your `sv_licenseKey`** (see the warning above), and click **Get**. Tebex returns a zip — extract it into your FXServer's `resources/` directory so you end up with `resources/sentinelfx/`.

{% hint style="warning" %}
The asset is encrypted by CFX Asset Escrow. The entitlement is bound permanently to the Cfx.re account that clicked **Get**. Take the time to log in to the right account before clicking — re-claiming on a different account costs you nothing but won't migrate the entitlement.
{% endhint %}

---

## Step 4 — Edit `config.lua`

Open `resources/sentinelfx/config.lua`. It contains placeholders:

```lua
Config = {
  serverId      = "PASTE_FROM_DASHBOARD",
  webhookSecret = "PASTE_FROM_DASHBOARD",
  apiBase       = "https://api.sentinelfx.net/fxbridge",
  serverName    = "PASTE_FROM_DASHBOARD",
  ...
}
```

Back on the dashboard's **Edit config.lua** step, you'll see the exact block ready to copy. Either paste each value into the placeholders, or replace the whole file contents with the dashboard block — both are equivalent.

{% hint style="warning" %}
The `webhookSecret` is shown **once** during setup. Copy it now or lose it. If you lose it, use the **Rotate Secret** button on the dashboard to generate a fresh value, then update `config.lua` again.
{% endhint %}

---

## Step 5 — Add `ensure sentinelfx` to `server.cfg`

Open your `server.cfg` and add:

```
ensure sentinelfx
```

Place it **after** any frameworks or database resources — `sentinelfx` doesn't depend on them, but starting after they settle keeps your boot log clean.

---

## Step 6 — Restart and Verify

Stop and start your FXServer. In the console you should see:

```
[sentinelfx] config loaded for server <your-server-id>
[sentinelfx] bridge online — server <your-server-id>
[sentinelfx] ban listener active
```

Back on the dashboard wizard, the **Test connection** button waits for the first heartbeat from your FXServer (it polls for up to 60 s). When the heartbeat lands, the box turns green and the **Finish setup** button unlocks.

{% hint style="info" %}
The wizard cannot be exited until **Test connection** passes. Refreshing the dashboard, closing the tab, or clicking elsewhere returns you to this step until the bridge is verified. Use the **Abort & disable bridge** link at the bottom of the step if you genuinely need to start over.
{% endhint %}

---

## Troubleshooting

### `You lack the required entitlement to use sentinelfx`

You claimed the Tebex asset on a Cfx.re account different from the one that owns your `sv_licenseKey`. Two ways to fix:

1. **Match the key to the entitled account.** Sign in to [keymaster.fivem.net](https://keymaster.fivem.net/) as the account that holds the entitlement, generate a new `sv_licenseKey`, and replace the line in your `server.cfg`. Restart.
2. **Re-claim on the right account.** Sign out of Tebex, sign in as the account that owns your `sv_licenseKey`, and click **Get** again on the package page.

### No heartbeat on the dashboard

- Check the FXServer console for `[sentinelfx] bridge online`. If it's missing, the resource didn't start — make sure `ensure sentinelfx` is in `server.cfg` and there are no Lua errors above.
- Confirm outbound HTTPS to `api.sentinelfx.net` is not blocked. Some hosts firewall outbound traffic by default.
- If the console shows `ingest failed 401` or `403`, the `webhookSecret` in `config.lua` is wrong — rotate it from the dashboard and paste the new block in.

### `[sentinelfx] CRITICAL: config.lua is not configured.`

You haven't replaced the `PASTE_FROM_DASHBOARD` placeholders. Open the dashboard's setup wizard (or the server's config view) and copy the rendered config block into `config.lua`.

### Bans aren't propagating

- Check your **ban mode**. If it's `off`, no network action will ever happen. If it's `review`, bans wait for SentinelFX HQ network-admin approval.
- For a network ban to hit the Discord side, SentinelFX needs to know the player's Discord ID. If Cfx.re hasn't exposed the Discord identifier for that player, the FiveM side will still block them, but there's no Discord ban to propagate.
- If you're using an admin framework other than txAdmin, see [Admin-framework capture coverage](#admin-framework-capture-coverage) below.

### The resource appears in the console but no events arrive

- The resource only forwards events while players are doing things. A completely empty server sends a heartbeat every minute and nothing else — that's normal.
- Connect a test player and watch for a `connect` event on the dashboard.

---

## Admin-framework capture coverage

The resource registers Lua event handlers for the major in-game admin scripts:

| Framework | Events listened for | Status |
|-----------|--------------------|--------|
| **txAdmin** | `txAdmin:events:playerBanned`, `:playerKicked`, `:playerWarned` | ✅ Reliable — these are documented txAdmin events. |
| **EasyAdmin** | `EasyAdmin:bansUpdated`, `EasyAdmin:onPlayerBanned`, `EasyAdmin:onUserBanned` | ⚠ Version-dependent. Newer EasyAdmin versions may use different event names. If your bans aren't captured, see the custom hook below. |
| **vMenu** | `vMenu:BannedSuccessfully`, `vMenu:BannedPlayerSuccessfully` | ⚠ Not native — vanilla vMenu writes bans to a JSON file rather than firing events. Only forks that emit these events will be captured automatically. |
| **bMenu** | `bMenu:BannedSuccessfully`, `bMenu:BannedPlayerSuccessfully` | ⚠ Same as vMenu — not native. |

**For frameworks that don't fire ban events to other resources** (most non-txAdmin admin scripts), use the SentinelFX custom hook from your own admin code:

```lua
TriggerEvent('SentinelFX:reportBan', source, 'reason text', 'author label')
```

or the export form:

```lua
exports.sentinelfx:reportBan(source, 'reason text', 'author label')
```

Drop one of these calls into the place where your framework actually issues the ban (right after it writes the ban record). That guarantees the SentinelFX bridge sees the event regardless of what the framework's event surface looks like.

---

## Updating the Resource

Re-download from Tebex when SentinelFX announces a new version. Replace the `sentinelfx` folder, then re-paste your existing `config.lua` values (or rotate the secret if you've misplaced it). The resource is stateless — there's nothing to migrate.

You only **need** to re-download if:

- SentinelFX announces a resource update (rare).
- You want a feature shipped in a newer release.

You only **need** to re-edit `config.lua` if:

- You rotated the secret.
- You changed the server's name on the dashboard and want it reflected in the bake.

Ban-mode changes **do not** require re-downloading or editing config. They take effect instantly from the dashboard.

---

## What the Resource Sends

Each connect / disconnect / ban event transmitted by the resource carries:

- **Player identifiers** provided by the FXServer runtime: `license`, `license2`, `steam`, `discord`, `fivem`, `xbl`, `live`, and the player's endpoint (IP).
- **Hardware fingerprint tokens** issued by CFX/FiveM. These are always collected, hashed with HMAC-SHA256 on your server before transmission, and used exclusively for cross-account ban-evasion detection. Raw tokens never leave the FXServer.
- **Display name** at the time of the event, the server's SentinelFX-issued `server_id`, and a UNIX timestamp.
- Every payload is signed with your server's `webhookSecret`; unsigned or tampered payloads are rejected by the API.

The resource never transmits chat logs, file contents, gameplay data, or any information beyond the fields above. Full disclosure is in the [Privacy Policy](https://sentinelfx.net/privacy).
