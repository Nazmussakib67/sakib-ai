# Cloudflare Pages deployment for Sakib AI

This repository is a HugoBlox Hugo site. Cloudflare Pages can deploy the same GitHub `main` branch without changing the existing Netlify setup.

## Cloudflare Pages project settings

- **Repository:** `Nazmussakib67/sakib-ai`
- **Production branch:** `main`
- **Framework preset:** Hugo (or None/Custom)
- **Root directory:** repository root (leave blank)
- **Build command:** `corepack enable && pnpm install --no-frozen-lockfile && hugo --gc --minify --baseURL "$CF_PAGES_URL" && pnpm run pagefind`
- **Build output directory:** `public`

### Environment variables

Set these in Cloudflare Pages for **Production** and **Preview**:

| Name | Value |
| --- | --- |
| `HUGO_VERSION` | `0.164.0` |
| `NODE_VERSION` | `22` |
| `GO_VERSION` | `1.24.3` |
| `PNPM_VERSION` | `10.14.0` |
| `HUGO_ENV` | `production` |

The build command uses Cloudflare's `CF_PAGES_URL` to generate correct links on the temporary `*.pages.dev` site. Hugo's production domain remains configured in `config/_default/hugo.yaml` for the final `sakib-ai.com` launch.

## Safe migration checklist

1. In [Cloudflare Workers & Pages](https://dash.cloudflare.com/?to=/:account/workers-and-pages), create a **Pages** project, connect GitHub, and select this repository.
2. Enter the settings above and deploy. Confirm the resulting `*.pages.dev` URL works, including `/blog/`, `/ai-news/`, `/research/`, images, and site navigation.
3. Only after testing, add `sakib-ai.com` under **Pages → Custom domains**. An apex domain requires Cloudflare DNS and updating nameservers at your domain registrar.
4. Preserve and review all existing DNS records before changing nameservers, especially MX, SPF, DKIM, DMARC, and other email-related records.
5. After the domain is active, update the Pages build command to `corepack enable && pnpm install --no-frozen-lockfile && hugo --gc --minify --baseURL "https://sakib-ai.com/" && pnpm run pagefind` so production canonical links use your domain. For preview deployments, retain the `$CF_PAGES_URL` variant.
6. Keep Netlify active until the Cloudflare domain and HTTPS are confirmed working.

**Note:** These instructions prepare the repository but do not create a Cloudflare project, authorize Cloudflare to access GitHub, or change DNS automatically. Review build logs if the first deployment fails.
