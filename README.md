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
| Words offered on a petal (universities) | the `CATS` array |
| Lettering cuts and their Thai faces | the `FONTS` array |
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

A customer picks a **category** first — Congrats, Chula, Mahidol, Thammasat, or
Alphabet — and all six cuts set that word, so they compare them in the
letters they are actually buying. Picking a category also writes the word onto a
free petal, and picking it again swaps that same petal rather than filling another.
**Alphabet** hands them the petal to type their own; from then on the specimens
follow whatever they typed.

To add a university, add a line to `CATS`:

```js
{id:"kasetsart", label:"Kasetsart", word:"KASETSART"}
```

The eight sheets `f1.jpg`–`f8.jpg` are six typefaces: `f3`/`f6` and `f4`/`f7` are the
same cut shown with different words. The row offers those six, and the sheets stay in
the repo because they are what the shop inks from.

On screen each cut is set in the nearest face we can load (`css` in `FONTS`), so a
preview approximates its sheet rather than reproducing it. To make a cut exact, put
the real font file in the repo, declare an `@font-face` in the `<style>` block, and
point that entry's `css` at it. The three Thai entries set real Thai — the six Latin
cuts have no Thai glyphs.

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
  Drop the real font files into the repo and point `css` at an `@font-face` to fix that.
