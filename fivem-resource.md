---
description: Install the SentinelFX resource on your FXServer in five minutes.
---

# Installing the Resource

<figure><img src="sfx_online.svg" alt="Install" width="64"></figure>

The SentinelFX FiveM resource is a small server-side script that plugs your FXServer into the network. It's built freshly for each registered server — your server's ID, its signed secret, and the correct API endpoint are baked in when you download it.

{% hint style="info" %}
You only need to do this once per FXServer. If you run multiple FXServers, register each one separately — every server gets its own secret.
{% endhint %}

---

## Before You Start

You'll need:

- A **SentinelFX member server** (the Discord side already set up — see [Getting Started](getting-started.md)).
- **Administrator access** on the Discord server that owns the FXServer.
- **File access** to your FXServer's `resources/` directory.
- The ability to **restart** the FXServer once.

That's it. No ports to open, no firewall rules, no database setup.

---

## Step 1 — Register the Server

Log into the dashboard at [sentinelfx.net](https://sentinelfx.net) and open the **FiveM** tab for your Discord server. Click **Register Server** and fill in:

| Field | What to put |
|-------|------------|
| **Name** | A label you'll recognise — e.g. `Main Roleplay`, `Dev Test`. |
| **Cfx code** *(optional)* | Your cfx.re join code, if you have one. |
| **Ban mode** | Start with `review`. You can change it any time. |

Hit **Create**.

---

## Step 2 — Pick a Ban Mode

<figure><img src="sfx_shield.svg" alt="Ban modes" width="48"></figure>

| Mode     | When to use |
|----------|-------------|
| `review` | **Default.** In-game bans queue for staff approval before hitting the network. Safest starting point. |
| `auto`   | You trust your cheat-scan completely and want zero-latency network bans. |
| `off`    | You want telemetry, HWID tracking, and dashboard stats, but no automatic network action. |

The mode is changeable from the dashboard at any time — no restart needed. See [Ban Modes & Propagation](fivem-ban-modes.md) for the full comparison.

---

## Step 3 — Download the Zip

On the server's dashboard page, click **Download Resource**. You'll get a zip named something like `sentinelfx-<your-server>.zip`.

{% hint style="warning" %}
The download contains your server's **signed secret**. Treat the zip like a password: don't commit it to a public repo, don't share it in screenshots, don't paste it in support channels.
{% endhint %}

---

## Step 4 — Install on the FXServer

1. Unzip the download. Inside is a folder called `sentinelfx`.
2. Move the `sentinelfx` folder into your FXServer's `resources/` directory.
3. Open your `server.cfg` and add this line:

```
ensure sentinelfx
```

Place it **after** any frameworks or database resources — `sentinelfx` doesn't depend on them, but starting after they settle keeps your boot log clean.

---

## Step 5 — Restart the FXServer

Stop and start your FXServer (or run `refresh` then `start sentinelfx` from the console if you prefer). In the console you should see something like:

```
[sentinelfx] bridge online — server <your-server-id>
```

---

## Step 6 — Verify the Heartbeat

<figure><img src="sfx_online.svg" alt="Heartbeat" width="48"></figure>

Go back to the dashboard and refresh the server's page. Within a few minutes you should see:

- A **green heartbeat indicator**.
- A non-zero **uptime** counter.
- Events appearing in the recent-events stream (a `connect` event for each player on the server).

If that all looks good — you're done. Your FXServer is now part of the SentinelFX network.

---

## Troubleshooting

### No heartbeat on the dashboard

- Check the FXServer console for a `[sentinelfx] bridge online` line. If it's missing, the resource didn't start — make sure `ensure sentinelfx` is in `server.cfg` and there are no Lua errors in the console.
- Confirm outbound HTTPS to `sentinelfx.net` is not blocked. Some hosts firewall outbound traffic by default.
- If the console shows `ingest failed 401` or `403`, the signed secret is wrong — re-download the zip and replace the folder.

### "Signature rejected" in the FXServer console

This means the secret stored in the resource doesn't match what the dashboard has. Usual cause: the secret was rotated on the dashboard but the resource was never re-downloaded. Fix:

1. On the dashboard, go to the server page → **Rotate Secret**.
2. Download the fresh zip.
3. Replace the `sentinelfx` folder on the FXServer.
4. Restart the resource (`restart sentinelfx`).

### Bans aren't propagating

- Check your **ban mode**. If it's `off`, no network action will ever happen. If it's `review`, bans wait for staff approval.
- For a network ban to hit the Discord side, SentinelFX needs to know the player's Discord ID. If Cfx.re hasn't exposed the Discord identifier for that player, the FiveM side will still block them, but there'll be no Discord ban to propagate.
- Check the dashboard's recent-events stream. If ban events are arriving but nothing is being actioned, open the review queue or the audit log to see what SentinelFX decided.

### The resource appears in the console but no events arrive

- The resource only forwards events while players are doing things. A completely empty server sends a heartbeat every 5 minutes and nothing else — that's normal.
- Connect a test player and watch for a `connect` event on the dashboard.

---

## Updating the Resource

Re-download the zip any time from the dashboard and replace the `sentinelfx` folder. The resource is stateless — there's nothing to migrate.

You only **need** to re-download if:

- You rotated the secret.
- SentinelFX announces a resource update (rare).
- You changed the server's name or cfx code and want that reflected in the baked config.

Ban-mode changes **do not** require re-downloading. They take effect instantly from the dashboard.
