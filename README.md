# Maison Sablé — petal order slip

A single-page order slip for Maison Sablé's tattooed-lily bouquets, laid out as a
paper ticket. The customer clicks a petal on the lily, types into it, fills in the
slip, and either copies an order brief or saves the whole thing as a picture.

**Live site:** https://mixerrrrr.github.io/maison-sable/

## Editing the shop details

Everything a shopkeeper needs is in one block at the top of the `<script>` in
`index.html`. No build step, no dependencies — edit, commit, push.

| What | Where |
| --- | --- |
| Prices (switched off for now) | `SHOW_PRICES` + the `PRICE` object |
| The two blooms and their drawings | the `BLOOMS` array |
| Lettering specimens and their Thai faces | the `FONTS` array |
| Ink colours | the `INKS` array |
| Where each petal's writing sits | the `PETALS` array |
| How many pictures a customer may place | `MAX_PICS` |
| Instagram handle, lead time, ink blurb | the `<footer>` near the end |

### Turning prices on

Prices are hidden until the shop sets them. Fill in `PRICE` and flip
`SHOW_PRICES` to `true` — the brief and the saved slip both pick up a total line.

## The petal map

`PETALS` holds six entries, one per petal, in the coordinates of the drawings
(1254 × 1254, the same for both blooms):

```js
{name:"top right", a:[824,299], rot:-73.0, len:320, tip: 1}
```

`a` is the middle of the writing, `rot` is that petal's angle in degrees, `len` is
the room it gives before the text shrinks, and `tip` says which end of the axis is
the petal tip (`1` = the `+` direction). `tip` decides which way letters run and
which way a picture faces.

These spans were measured off `lily-pink.webp`, where the artwork itself says which
pixels are petal and which are the green centre. If the drawing is ever replaced,
these six lines are the only thing that needs re-measuring.

## Lettering

`f1.jpg`–`f8.jpg` are the shop's own specimen sheets — customers tick the one they
want. Each is paired with the nearest face we can load on screen (`css`), so the
preview approximates what gets inked; the sheet is what the shop works from. The
three Thai entries set real Thai and are what Thai text should use — the eight
specimens are Latin only.

## Images

- `lily.webp` / `lily-pink.webp` — the white and pink blooms customers write on
- `bouquet-line.webp` — the small drawing on the stub
- `f1–f8.jpg` — the lettering specimens
- `p1–p3.webp` — the real product photos under "The real thing"

## How it works

Each petal is an SVG group rotated to that petal's angle, holding an invisible hit
ellipse, a dashed guide, and whatever the customer put there. Clicking one opens a
small field that floats just beyond the petal so the bloom stays visible.

- **↕ down the petal** draws each letter upright, stepping along the petal from the
  tip inwards, which is how the shop inks them.
- **↔ along the petal** sets the word on the petal's axis and shrinks it to fit.
- A **picture** stands upright with its top toward the middle of the flower, and has
  its own size slider. Up to three can be placed, one per petal.

**Save the slip** paints the whole order onto a canvas — the flower with its ink,
then the filled-in fields — and offers it as a PNG. It is drawn by hand in
`buildReceipt()` rather than screenshotting the page, so it stays crisp and prints.

## Known limits

- The order brief is copied to the clipboard; it is not emailed or stored anywhere.
- An uploaded picture lives in that browser tab only and is lost on refresh.
- The specimen previews use the nearest available web font, not the exact sheet.
