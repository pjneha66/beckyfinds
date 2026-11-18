# ReelFind

Mobile-first, single-page HTML site for searching movies/series and finding download links from three Stremio addons.

> **This is a downloader / link finder, not a streaming service.** ReelFind hosts, stores, streams and plays no media. It only searches addons for download links and hands those links to you.

## Run it

The whole app is in `index.html` — no build step, no dependencies.

Open it directly in a browser, or serve it locally (recommended):

```bash
cd /storage/emulated/0/Ob/007MKV
python3 -m http.server 8080
```

Then visit `http://127.0.0.1:8080`.

To publish, upload `index.html` to any static host (GitHub Pages, Netlify, Cloudflare Pages, etc.).

## How it works

1. **Search** — title lookup uses Cinemeta's Stremio search catalog (`/catalog/{type}/top/search={query}.json`). Note: Stremio extras must be in the URL path, not a query string.
2. **Pick a title** — detailed metadata (poster, description, year, rating, episodes) comes from Cinemeta's meta endpoint.
3. **Series** — choose a season and episode; the episode's Stremio ID (`tt…:S:E`) is used for the stream lookup.
4. **Links** — the site asks all three addons for streams in parallel and shows only entries with a real `url` (sponsor/discord `externalUrl` entries are hidden). Results can be filtered by quality and source, copied, or opened in a new tab.

## Sources

- NoTorrent — `https://addon.notorrent2.workers.dev`
- HdHub — configured manifest URL (2160p/1080p/720p, descending)
- PenguPlay — `https://pengu.uk` with the auth token you supplied

All four APIs used (Cinemeta search/meta and the three addon stream endpoints) return `Access-Control-Allow-Origin: *`, so the site works as a pure client-side page with no proxy.

## Notes

- The PenguPlay auth token is embedded in `index.html` (in `ADDONS`). Anyone who can view the site can see and use that token — replace it with your own or a server-side proxy if that matters.
- The site hosts no media and does not guarantee any link works; links depend on the addons and expire over time.
- Download links open in a new tab. Most hosts will show their own download page. ReelFind never plays or streams the files itself.
- Pixeldrain blocks direct cross-site downloads (`hotlink_detected`). ReelFind turns `pixeldrain.dev/api/file/ID?download` links into the file's `pixeldrain.dev/u/ID` page — press **Download** there. The **Copy** button still copies the direct URL, which works in download managers or when pasted into the address bar.
