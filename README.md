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
| Ready-made lettering on a petal | the `DESIGNS` array |
| Fallback cuts for words with no sheet | the `FONTS` array |
| Ink colours | the `INKS` array |
| Where each petal's writing sits | the `PETALS` array |
| How many pictures at once | `MAX_PICS` — one per petal |
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

**Tap the petal you want, then tap the lettering.** The petal you tapped last stays
marked and everything you choose lands there, so a customer fills the flower petal by
petal. Tap nothing first and it goes to the next free petal.

The **category** — Congrats, Chula, C U, Mahidol, Thammasat or Alphabet — decides what
the row below shows. **Alphabet** opens the petal for their own words; from then on
the row previews whatever they typed.

To add a university, add a line to `CATS`:

```js
{id:"kasetsart", label:"Kasetsart", word:"KASETSART"}
```

### The sheets

`f1.jpg`–`f8.jpg` are the shop's own lettering, and they are used as artwork, not as
fonts: tap one and that exact image goes down the petal. `DESIGNS` maps each file to
the word it spells, the cut it is in, and its pixel size:

```js
{id:"d5", word:"CHULA", cut:"Engraved", img:"f6.jpg", w:103, h:473}
```

To offer a new word, letter it in the shop's fonts, export one tall image per cut
(letters stacked, dark on white — the white drops out), drop the files in beside
`index.html` and add a line here per file. The category appears on its own once a
`CATS` entry and a matching `word` exist.

### Words with no sheet yet

Mahidol and Thammasat have no artwork, so those categories fall back to typed
letters set in the nearest faces we can load (`FONTS`), and the slip says so. That is
a stand-in until the sheets exist — the six cuts approximate the sheets rather than
reproducing them. Each cut falls back to Noto Serif Thai, so typed Thai still sets
properly in any of them.

## Images

- `lily.webp` / `lily-pink.webp` — the white and pink blooms customers write on
- `bouquet-line.webp` — the small drawing on the stub
- `f1–f8.jpg` — the shop lettering that goes on a petal
- `p1–p3.webp` — the real product photos under "The real thing"

## How it works

Each petal is an SVG group rotated to that petal's angle, holding an invisible hit
ellipse, a dashed guide, and whatever the customer put there. Clicking one opens a
small field that floats just beyond the petal so the bloom stays visible.

- **↕ down the petal** draws each letter upright, stepping along the petal from the
  tip inwards, which is how the shop inks them.
- **↔ along the petal** sets the word on the petal's axis and shrinks it to fit.
- A **sheet** lies along the petal at whatever size the slider says, first letter at the
  tip and the tail running back into the middle — the same direction typed words go
  (`sheetTurn`). A sheet is one rigid strip, so on the lower petals that reads
  bottom-up: consistent order costs upright glyphs, and there is no placement that
  gives both.
- A **photograph** is not directional, so it takes whichever quarter turn leaves it
  upright (`uprightTurn`).
- A picture can go on any petal; `MAX_PICS` caps how many at once.

**Save the slip** paints the whole order onto a canvas — the flower with its ink,
then the filled-in fields — and offers it as a PNG. It is drawn by hand in
`buildReceipt()` rather than screenshotting the page, so it stays crisp and prints.

## Known limits

- The order brief is copied to the clipboard; it is not emailed or stored anywhere.
- An uploaded picture lives in that browser tab only and is lost on refresh.
- Words with no sheet fall back to look-alike web fonts, not the shop's own cuts.
