# Maison Sablé — Petal Tattoo Studio

A single-page designer for Maison Sablé's tattooed-lily bouquets. A customer types
words or uploads a picture, drags it onto a real photo of the bouquet to see exactly
where the ink will sit, then copies an order brief to send over Instagram or LINE.

**Live site:** https://mixerrrrr.github.io/maison-sable/

## Editing the shop details

Everything a shopkeeper needs to change lives near the top of the `<script>` block in
`index.html`. No build step, no dependencies — edit, save, commit, push.

| What | Where |
| --- | --- |
| Prices (stems, wraps, add-ons, per-tattoo) | the `PRICE` object |
| Bouquet photos shown in the picker | the `PHOTOS` array + the `.webp` files |
| Lettering choices | the `FONTS` array |
| Ink colours | the `INKS` array |
| Quick-idea buttons (CHULA, TU, ยินดีด้วย…) | the `PRESETS` array |
| Instagram handle, lead time, ink blurb | the `<footer>` near the end of the file |

Prices are placeholders in baht — replace them with the real ones before sharing the link.

## Photos

`p1.webp` `p2.webp` `p3.webp` are Maison Sablé's own product photography. Customers can
also upload their own bouquet shot from the picker; those stay in their browser and are
never uploaded anywhere.

## How the ink effect works

Each tattoo is a positioned element with `mix-blend-mode: multiply` over the photo, so
dark strokes sink into the petal and a white background on an uploaded drawing falls away
on its own. Sizes use `cqw` units against a container query on the stage, which keeps the
placement identical at every screen width.

## Known limits

- The order brief is copied to the clipboard; it is not emailed or stored anywhere.
- Customers cannot download their mockup — the page asks them to screenshot instead.
- Uploaded photos and tattoos are lost on refresh.
