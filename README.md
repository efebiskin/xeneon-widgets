# xeneon-widgets

Single-page widgets built for the **Corsair Xeneon Edge** (14.5" 2560×720 iCUE touchscreen). Each widget is a self-contained HTML file — no framework, no build step, no server. Drop the URL into iCUE's iFrame widget and you're done.

## Widgets

### Ambient — zero setup

| Widget | Folder | What it shows |
|---|---|---|
| **Clock** | [`/clock`](./clock) | Giant local clock + world-clock grid, editable |
| **Pomodoro** | [`/pomodoro`](./pomodoro) | Focus timer with ring, auto-advance, sound, keyboard shortcuts |
| **Hacker News** | [`/hackernews`](./hackernews) | Top / New / Best / Ask / Show feeds, auto-refresh |
| **Weather** | [`/weather`](./weather) | Current + 4-day forecast via Meteora API |

### Markets — live data

| Widget | Folder | What it shows |
|---|---|---|
| **Crypto** | [`/crypto`](./crypto) | Watchlist w/ sparklines, CoinGecko public API |
| **Stocks** | [`/stocks`](./stocks) | Ticker watchlist via Meteora `/v1/quotes` (Yahoo + Stooq) |

### Accounts — one-time login

| Widget | Folder | What it shows |
|---|---|---|
| **Spotify** | [`/spotify`](./spotify) | Now-playing + transport controls, OAuth implicit grant |
| **Discord** | [`/discord`](./discord) | Live presence via Lanyard (read-only by design) |

## Run locally

From the repo root:

```bash
python -m http.server 5556
```

Then point iCUE's iFrame widget at, e.g.:

- `http://localhost:5556/clock/`
- `http://localhost:5556/pomodoro/`
- `http://localhost:5556/hackernews/`
- `http://localhost:5556/crypto/`
- `http://localhost:5556/stocks/` *(requires Meteora running)*
- `http://localhost:5556/weather/?base=http://localhost:8787&key=mto_...&lat=34.17&lon=-118.87`
- `http://localhost:5556/spotify/`
- `http://localhost:5556/discord/`

## Deploy

Everything is static — drop the repo on GitHub Pages, Cloudflare Pages, Netlify, or Vercel. No env vars. Per-widget credentials are stored in `localStorage` client-side only.

## Per-widget setup

### Spotify
1. Create an app at [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard).
2. Add your widget URL to **Redirect URIs** (copy the exact URI the widget shows).
3. Paste the Client ID into the widget → Connect.

### Discord
1. Enable Developer Mode in Discord → copy your User ID.
2. Join the Lanyard server at [discord.gg/lanyard](https://discord.gg/lanyard) (stay a member).
3. Paste your ID into the widget.

### Weather / Stocks
Both use your **Meteora API**. Create a key once:

```bash
curl -X POST http://localhost:8787/v1/keys \
  -H "Content-Type: application/json" \
  -d '{"email":"you@example.com"}'
```

Then feed that `mto_...` key into either widget via its setup screen or URL params.

### Clock / Pomodoro / Hacker News / Crypto
No setup. Just open the URL.

## A note on what's *not* here

Previous versions included a Discord voice-control widget and an Apple Music/HomePod control widget. They were removed in v0.2.0 because **neither can fully work from a browser**:

- Discord voice/mute/deafen/screenshare control requires Discord's `rpc.voice.write` / `rpc.video.write` scopes, which are restricted to Discord-approved partner apps. A browser tab also cannot talk to the local Discord client (no pipe/socket access).
- HomePod has no web-callable API. Apple's only control path is Home/Shortcuts on an Apple device; the realistic workaround is Home Assistant + HomeKit Controller on a LAN device.

The current Discord widget is intentionally read-only (presence only). That works fully.

## Structure

```
xeneon-widgets/
├── index.html          ← gallery / launcher
├── clock/
├── pomodoro/
├── hackernews/
├── crypto/
├── stocks/
├── weather/
├── spotify/
├── discord/
├── shared/             ← reserved for shared assets
└── README.md
```

## License

MIT.
