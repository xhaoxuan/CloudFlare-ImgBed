# img.04mc.com customization

This fork updates CloudFlare-ImgBed to upstream main
`85636c5bc7393e80e0fe229a8f73538ad5c65ee4` (2.7.8).
The previous production revision is
`75617feda5f1a6d7775de888706101b2cb14a30e`.

The generated `frontend-dist/` comes from Sanyue-ImgHub
`a33dd5276bc2bfaf0dea561dbc7b2c7381ba1858` with `frontend.patch` applied to
the frontend source before building. Bundled JavaScript was not edited by hand.
The original MIT license and copyright notices remain in the repository.

## Preserved branding

- Default page title and site name: `ihyuaan ImgHub`
- Footer name: `iHYUAAN`
- Logo and footer links: `https://blog.04mc.com`
- Page description, keywords and author reflect the existing customized HTML.
- Existing administrator page settings still override frontend defaults.

## Reproduce the frontend

Use Node.js 22 and npm. Clone the frontend alongside this repository:

```sh
git clone https://github.com/MarSeventh/Sanyue-ImgHub.git ../Sanyue-ImgHub
git -C ../Sanyue-ImgHub checkout a33dd5276bc2bfaf0dea561dbc7b2c7381ba1858
git -C ../Sanyue-ImgHub apply ../CloudFlare-ImgBed/branding/frontend.patch
cd ../Sanyue-ImgHub
npm ci
npm test
npm run build
```

Replace this repository's `frontend-dist/` with the resulting complete `dist/`
directory, including generated gzip files and entry assets. When updating the
frontend again, apply these source customizations to the matching new frontend
revision and rebuild. Resolve source-patch conflicts explicitly; do not restore
old hashed JavaScript into a newer build.

## Existing Cloudflare Pages project

Continue using the existing `cloudflare-imgbed` Pages project and its domains.
Production branch is `main`. The new build output directory is `frontend-dist`;
the project root stays empty and its existing `npm install` build command works.
Use Node.js 22 if an explicit build version is needed.

Preserve the existing `img_url` KV namespace binding and its contents. This
upgrade does not create, replace or migrate the database. Telegram channel
settings remain at `manage@sysConfig@upload`, as in the old deployment. Existing
environment-based channels remain supported via `TG_BOT_TOKEN` and
`TG_CHAT_ID`; retain those variables if configured. Never copy bot tokens into
this public repository or frontend files.

After publishing, verify `/api/userConfig`, administrator access, an existing
Telegram image URL and one new upload. Do not change or remove the old Telegram
channel while checking historical files.

To roll back, restore the previous Pages deployment from the deployment list.
Before rebuilding old code, also restore its old empty build output directory.

## Local validation

- `npm test`: 265 passed.
- `npm run test:server`: server import and Worker entry smoke checks passed.
- `npx --no-install wrangler pages functions build functions --outdir data/ci-pages-build`: compiled successfully.
- Frontend `npm test`: 238 passed.
- Frontend `npm run build`: succeeded; upstream CSS ordering and bundle-size warnings remain.

These checks use isolated test data. They do not verify the private production
Telegram credentials or send uploads to the live bot.
