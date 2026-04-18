# xeneon-widgets

Single-page widgets built for the **Corsair Xeneon Edge** (14.5" 2560×720 iCUE touchscreen). Each widget is a self-contained HTML file — no framework, no build step, no server. Drop the URL into iCUE's iFrame widget and you're done.

## Widgets

| Widget | Folder | What it shows |
|---|---|---|
| **Spotify** | [`/spotify`](./spotify) | Now-playing + transport controls for your Spotify account. OAuth implicit grant, polls every 4s. |
| **Discord** | [`/discord`](./discord) | Live Discord presence — avatar, status, current game, Spotify now-playing. Powered by [Lanyard](https://github.com/Phineas/lanyard). |
| **Apple Music** | [`/apple-music`](./apple-music) | Embeds any Apple Music share URL (song, album, playlist, radio). Honest note on HomePod control. |
| **Weather** | [`/weather`](./weather) | Big-number current + 4-day forecast. Driven by the [Meteora API](https://github.com/efebiskin/meteora). |

## Run locally

From the repo root:

```bash
python -m http.server 5556
```

Then point iCUE's iFrame widget at, e.g.:

- `http://localhost:5556/spotify/`
- `http://localhost:5556/discord/`
- `http://localhost:5556/apple-music/`
- `http://localhost:5556/weather/?base=http://localhost:8787&key=mto_...&lat=34.17&lon=-118.87`

## Deploy

Everything is static — drop the repo on GitHub Pages, Cloudflare Pages, Netlify, or Vercel. No env vars needed except per-widget credentials stored in `localStorage` client-side.

## Per-widget setup

### Spotify
1. Create an app at [developer.spotify.com/dashboard](https://developer.spotify.com/dashboard).
2. Add your widget URL to the app's **Redirect URIs** (exactly — the page shows the URI to paste).
3. Paste the Client ID into the widget, connect, done.

### Discord
1. Enable Developer Mode in Discord → copy your User ID.
2. Join the Lanyard server at [discord.gg/lanyard](https://discord.gg/lanyard) (required — stay a member).
3. Paste your ID into the widget.

### Apple Music
1. In Apple Music, share any song/album/playlist → Copy Link.
2. Paste into the widget. Previews work for everyone; full playback requires signing into Apple's embed.

### Weather
1. Get a free API key from the Meteora API: `POST /v1/keys {"email": "..."}`.
2. Open the widget with URL params: `?base=<meteora-url>&key=<mto_...>&lat=<N>&lon=<N>`.

## A note on HomePod control

Apple does not expose HomePod playback over any web-callable API. The only official path is Home/Shortcuts on an Apple device. The realistic workaround is **Home Assistant** with the HomeKit Controller integration running on a Mac/Pi on your network — then a widget can hit its local REST endpoint. Until that's built, the Apple Music widget controls an embed, not the physical speaker.

## Why this exists

The Xeneon Edge's iCUE widget store is tiny, and most third-party integrations assume you're on the main display. These widgets are optimized for the side-panel form factor (2560×720 landscape, and scale down gracefully).

## Structure

```
xeneon-widgets/
├── index.html          ← gallery / launcher
├── spotify/index.html
├── discord/index.html
├── apple-music/index.html
├── weather/index.html
├── shared/             ← reserved for shared assets
└── README.md
```

## License

MIT — do whatever, no warranty.
