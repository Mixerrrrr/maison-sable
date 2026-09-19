# Maison Sablé — petal order slip

A single-page order slip for Maison Sablé's tattooed-lily bouquets, laid out as a
paper ticket. The customer clicks a petal on the line-art lily, types into it, fills in
the slip, and copies an order brief to send over Instagram or LINE.

**Live site:** https://mixerrrrr.github.io/maison-sable/

## Editing the shop details

Everything a shopkeeper needs is in one block at the top of the `<script>` in
`index.html`. No build step, no dependencies — edit, commit, push.

| What | Where |
| --- | --- |
| Prices per stem count, per written petal, per picture | the `PRICE` object |
| Lettering choices (and which support Thai) | the `FONTS` array |
| Ink colours | the `INKS` array |
| Flower colours in the dots row | the `BLOOMS` array |
| Where each petal's writing sits | the `PETALS` array |
| Instagram handle, lead time, ink blurb | the `<footer>` near the end |

Prices are placeholders in baht — replace them with the real ones before sharing the link.

## The petal map

`PETALS` holds six entries, one per petal, in the coordinates of `lily.webp`
(1254 × 1254):

```js
{name:"top right", a:[795,395], rot:-73.0, len:420}
```

`a` is where the writing sits, `rot` is the angle of that petal in degrees, and `len`
is how much room the petal gives before the text shrinks to fit. If the drawing is ever
replaced, these six numbers are the only thing that needs re-measuring.

## Images

- `lily.webp` — the line drawing customers write on
- `bouquet-line.webp` — the small drawing on the stub
- `p1–p3.webp` — the real product photos under "The real thing"

## How it works

Each petal is an SVG group rotated to that petal's angle, holding an invisible hit
ellipse, a dashed guide, and the customer's text. Clicking one opens a small field that
floats just beyond the petal so the bloom stays visible while you type. Text scales down
automatically when it outgrows the petal. The flower-colour dots tint a wash behind the
drawing, which shows through because the line art is composited with `multiply`.

## Known limits

- The order brief is copied to the clipboard; it is not emailed or stored anywhere.
- An uploaded picture lives in that browser tab only and is lost on refresh.
- Customers cannot download their slip — the page asks them to screenshot it.
