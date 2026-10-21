# rewault-site

Public website for the Rewault iOS app: homepage, privacy policy, terms of service and support.
Plain static HTML and one CSS file. There is no build step, no JavaScript, and nothing from third parties (no analytics, cookies, trackers or external fonts).

```
/                  index.html
/privacy/          privacy/index.html
/terms/            terms/index.html
/support/          support/index.html
404.html           not-found page
styles.css         all styles (light and dark via prefers-color-scheme)
assets/            app icon, logo and favicon PNGs
favicon.ico
CNAME              custom domain (GitHub Pages only)
```

Paths are absolute (`/styles.css`, `/privacy/`), so the site must be served from the root of a domain, not from a sub-path.

## Before publishing

Search for `[` in the HTML and replace every bracketed placeholder (`[DATE]`, `[SUPPORT EMAIL]`, `[NAME / COMPANY]`, `[ADDRESS]`, `[STATE / COUNTRY]`, `[VENUE]`). Placeholders show highlighted in yellow until they're replaced. Also put your domain in `CNAME`.

When you have the support email, make it a link on the support page: `<a href="mailto:you@example.com">you@example.com</a>`.

Preview locally:

```bash
python3 -m http.server 8000
```

Then open http://localhost:8000.

## Deploy on Cloudflare Pages (preferred)

1. Push this folder to a public GitHub repo named `rewault-site`.
2. In the Cloudflare dashboard, go to **Workers & Pages → Create → Pages → Connect to Git** and pick the repo.
3. Build settings: framework preset **None**, build command empty, build output directory `/`. Deploy.
4. In the project, open **Custom domains → Set up a custom domain** and enter your domain (for example `example.com`, and optionally `www.example.com`).
   - If the domain's DNS is on Cloudflare, the record is created for you.
   - Otherwise, add the `CNAME` record Cloudflare shows (pointing at `<project>.pages.dev`) at your DNS provider. An apex domain needs DNS on Cloudflare, or a provider that supports CNAME flattening / ALIAS.
5. HTTPS is issued automatically. The `CNAME` file is ignored by Cloudflare and does no harm.

Cloudflare serves `privacy/index.html` at `/privacy/` and redirects `/privacy` to it.

## Or deploy on GitHub Pages

1. Push to a public GitHub repo.
2. **Settings → Pages → Build and deployment**: Source **Deploy from a branch**, branch `main`, folder `/ (root)`.
3. Put your domain in `CNAME` (one line, no `https://`), or enter it under **Settings → Pages → Custom domain**.
4. At your DNS provider:
   - Apex domain: four `A` records to `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153` (and optionally `AAAA` records `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`).
   - `www` subdomain: a `CNAME` to `<your-username>.github.io`.
5. Once the DNS check passes, tick **Enforce HTTPS**.

## Verify the domain in Google Search Console

Google OAuth verification needs the domain verified by the same Google account that owns the Cloud project.

1. Open https://search.google.com/search-console and choose **Add property → Domain** (not "URL prefix"). Enter the bare domain, for example `example.com`.
2. Google shows a TXT record like `google-site-verification=...`.
3. At your DNS provider (Cloudflare: **DNS → Records → Add record**), add a `TXT` record with name `@` and that value.
4. Back in Search Console, click **Verify**. DNS changes can take a few minutes to a few hours.
5. In Google Cloud Console → **Google Auth Platform → Branding**, add the domain under **Authorized domains** and set the homepage, privacy policy and terms links to this site.

Keep the TXT record in place after verification; removing it un-verifies the domain.
