# media.notechdebt.dev

Video files shown on notechdebt.dev, served by GitHub Pages at https://media.notechdebt.dev/.

They live here, not in the notechdebt.dev repo, because notechdebt.dev is served through Cloudflare, and Cloudflare's CDN terms (below Enterprise) don't allow serving video through it unless it is hosted on Cloudflare (Stream, R2). This site's DNS record is **DNS only** (grey cloud), so its traffic goes straight to GitHub Pages and never through Cloudflare's proxy.

| File | Shown on |
|---|---|
| `pearl-demo.mp4` | https://notechdebt.dev/apps/pearl/ |
| `opal-demo.mp4` | https://notechdebt.dev/apps/opal/ |

## Setup (once)

- GitHub: Settings → Pages → Deploy from a branch, `main`, `/ (root)`; custom domain `media.notechdebt.dev` (from `CNAME`); Enforce HTTPS once the certificate is issued.
- Cloudflare DNS for notechdebt.dev: `CNAME media → notechdebt.github.io`, proxy status **DNS only**.
- `.nojekyll` makes Pages serve the files as they are.

## Replacing a video

Keep the file name: the pages, their structured data and the sitemap point at it. Encode the narrated master from the app repo's `marketing/video/out/` for the web, then commit and push:

    ffmpeg -i ../pearl/marketing/video/out/pearl-demo-1080p-voice.mp4 -c:v libx264 -preset slow -crf 23 -profile:v high -level 4.0 -pix_fmt yuv420p -c:a aac -b:a 128k -ac 2 -movflags +faststart pearl-demo.mp4

Then in the notechdebt.dev repo update the video's `seconds`, `cues` (narration times, used for captions and the transcript) and `uploaded` in `tools/apps.mjs`, and its poster `img/<app>-video.webp` if the picture changed.

GitHub Pages caches files for about 10 minutes, and a single file must stay under 100 MB.
