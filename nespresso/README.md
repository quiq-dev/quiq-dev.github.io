# Nespresso demo storefront

A single page (`index.html`) modeled on nespresso.com/us/en, with the Quiq chat
widget embedded. No build step and no dependencies; the only external request the
page makes is for the widget script itself.

## Chat widget

Wired exactly as AI Studio's sample code specifies:

- `<script src="https://nespresso-ai-studio-playground.quiq-api.com/app/chat-ui/index.js" charset="UTF-8">` in `<head>`
- `Quiq({ pageConfigurationId: 'shelf-agent-test' })` in the body

To point the page at a different agent, change `pageConfigurationId` (bottom of
`index.html`). If the tenant changes, change the script `src` host too.

## Serving it

Any static host works. Locally:

```sh
python3 -m http.server 8080
# http://localhost:8080
```

The widget needs the page served over http(s) — opening `index.html` as a `file://`
URL will load the page but the widget generally won't initialize.

**Before demoing:** the hosting domain usually has to be allowed in the Quiq page
configuration (`shelf-agent-test`). If the launcher never appears, check the browser
console for a CORS/origin rejection and add the domain in AI Studio.

## Imagery

`img/` holds real Nespresso product photography, downloaded from their public
product pages and served locally (their CDN sits behind bot protection, so
hotlinking is unreliable). Provenance: each file came from the `og`-level product
shot on that product's own PDP under `nespresso.com/us/en/order/...`, fetched at
`?impolicy=product&imwidth=600`. `hero.jpg` is their homepage banner
(`Nespresso-HP-Banner-952x912.jpg`), CSS-cropped to hide its baked-in promo text.

These are Nespresso's copyrighted assets, used here for a demo built for Nespresso.
Fine for that; don't reuse this folder for another brand's demo.

To refresh or add one: find the product in `https://www.nespresso.com/us/sitemap.xml`,
fetch the PDP with a browser User-Agent, and grep for
`/ecom/medias/sys_master/public/<id>/...2000x2000.png`. Not every PDP server-renders
its product shot — Bianco Forte and Vertuo Creatista don't, which is why the
catalog uses Intenso and Gran Lattissima.

## Editing content

Products live in two arrays near the bottom of `index.html` — `CAPSULES` and
`MACHINES`. Each entry points at a filename in `img/`. Everything else is plain markup.

The "Chat with us" button in the support band tries the widget's open API and falls
back to clicking the launcher; if neither works in your widget version, the launcher
bubble still opens chat normally.

---

Internal demo material — not affiliated with or endorsed by Nespresso. Names, prices
and copy are illustrative.
