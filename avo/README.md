# Alliance Virtual Offices demo site

A single page (`index.html`) modeled on alliancevirtualoffices.com, with the Quiq chat
widget embedded. No build step and no dependencies; the only external request the
page makes is for the widget script itself.

## Chat widget

Wired exactly as AI Studio's sample code specifies:

- `<script src="https://demo12.quiq-api.com/app/chat-ui/index.js" charset="UTF-8">` in `<head>`
- `Quiq({ pageConfigurationId: 'avo' })` in the body

To point the page at a different agent, change `pageConfigurationId` (bottom of
`index.html`). If the tenant changes, change the script `src` host too.

Every "Chat with us" / "Check availability" button (anything with `data-open-chat`)
and the hero location search open the widget: they try the widget's open API and fall
back to clicking the launcher. If neither works in your widget version, the launcher
bubble still opens chat normally.

## Serving it

Any static host works. On quiq-dev.github.io drop the folder in as `avo/` and it
serves at `https://quiq-dev.github.io/avo/index.html`. Locally:

```sh
python3 -m http.server 8080
# http://localhost:8080
```

The widget needs the page served over http(s); opening `index.html` as a `file://`
URL will load the page but the widget generally won't initialize.

**Before demoing:** the hosting domain usually has to be allowed in the Quiq page
configuration (`avo`, tenant `demo12`). If the launcher never appears, check the
browser console for a CORS/origin rejection and add `quiq-dev.github.io` in AI Studio.

## Content and the agent

Plans, prices, hours and policies on the page come from the same knowledge base the
`avo-ai-agent` answers from (`tenants/demo12/datasets/avo-kb.ndjson`), so what a
visitor reads and what the assistant says agree: city prices, Platinum / Platinum Plus,
Live Receptionist bundles ($125 / $175 / $260 / $550), $1.75/min overage, Form 1583,
6-month term.

The "Featured centers" cards mirror the mock meeting-room inventory in
`tenants/demo12/services/avo-core/lib/rooms.py` (Austin Congress Avenue and Domain
North, Denver Seventeenth Street and Cherry Creek, Sacramento Capitol Mall) so a
presenter can ask the assistant about a center by name. Room names are deliberately
NOT on the page; they are eval props. If the inventory changes, update the `CENTERS`
array near the bottom of `index.html`.

## Imagery

`img/` holds Alliance's own assets, downloaded from their public Cloudinary CDN
(`res.cloudinary.com/alliance-virtual-offices/...`) and served locally because the
site itself sits behind bot protection:

- `logo.png`, `icon.png` — `alliancevirtualoffices/common/alliance-virtual-offices-logo-t` and `-icon`
- `city-*.jpg` — `alliancevirtualoffices/common/top-cities/<City>-virtual-offices`, fetched at `w_700,h_480,c_fill`
- `icon-*.png` — the six homepage service illustrations under `alliancevirtualoffices/pages/home-page/`

These are Alliance's copyrighted assets, used here for a demo built for Alliance.
Fine for that; don't reuse this folder for another brand's demo.

## Editing content

Cities and featured centers live in two arrays near the bottom of `index.html`
(`CITIES` and `CENTERS`). Everything else is plain markup.

---

Internal demo material — not affiliated with or endorsed by Alliance Virtual Offices.
Plans, prices, centers and copy are illustrative.
