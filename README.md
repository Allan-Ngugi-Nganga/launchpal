# LaunchPal landing page

This repository holds the **built** landing page for [LaunchPal](https://launchpal-site.pages.dev/), a
Chrome extension that audits a website built with AI tools and reports launch-blocking problems with
paste-ready fix prompts.

It is generated output. Do not edit the HTML here by hand: the next publish overwrites it.

## Live

- Canonical: https://launchpal-site.pages.dev/ (Cloudflare Pages, serves security headers from _headers)
- Mirror: https://allan-ngugi-nganga.github.io/launchpal/ (GitHub Pages, ignores _headers)

Both are pushed by the same command, and both carry a canonical link to the Cloudflare deployment.

## Where the source lives

The page is built from `vibecheck/site` in the private project tree:

- `site/index.html`, `site/privacy.html` : markup with `{{PLACEHOLDERS}}`
- `site/config.json` : prices, FX rates, checkout links, support address, legal name
- `scripts/build-site.mjs` : fills the placeholders, writes robots.txt, sitemap.xml, llms.txt, _headers
- `scripts/deploy-pages.mjs` : publishes to Cloudflare Pages and this repository

Rebuild and publish, in that order:

```
node scripts/build-site.mjs
npx wrangler pages deploy site-dist --project-name launchpal-site --branch main
node scripts/deploy-pages.mjs
```

## Changing a price

Prices and indicative currency conversions are in `site/config.json`, never in the HTML. Update
`prices` there and, if needed, `fx`. The KES figures are the ones actually charged; USD and EUR are
conversions shown for reference and are marked with an approximation sign.

If the price changes in Paystack, the same numbers must change in `worker/src/config.ts` (`PLAN_AMOUNTS`),
otherwise the licence service will refuse to issue keys on the new price.

## Checks that must pass before publishing

```
node scripts/check-contrast.cjs   # every text and background pair meets WCAG AA
node scripts/check-slop.cjs       # scores the page against 16 known AI-generated design tells
```

Both exit non-zero on regression.

## Contact

maverick.develops@gmail.com
