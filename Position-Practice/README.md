# Position Practice — Image Cards with Hover Overlay

A six-card image grid built with plain HTML and CSS. Practice project focused on CSS
positioning: each card is a `position: relative` anchor holding a `position: absolute`
overlay that fades in on hover, alongside an image zoom and a lifting box-shadow.

## Preview

![Six-card image grid with a hover overlay on the second card](images/screenshot.png)

## The core pattern

The whole project is one idea — **relative parent, absolute child**:

```css
.card       { position: relative; }  /* the anchor */
.card .text { position: absolute; inset: 0; }  /* fills the anchor */
```

An absolutely positioned element measures its `top/right/bottom/left` against the nearest
ancestor that is *not* `position: static`. Marking `.card` as `relative` — which moves it
nowhere — is what makes the overlay cover the card instead of the whole page.

`inset: 0` is shorthand for all four offsets at zero. With `width`/`height` left at `auto`,
setting both `left` and `right` forces the element to stretch, so the overlay tracks the
card's size exactly at any screen width.

## Hover effects

| Effect | How |
| --- | --- |
| **Overlay fade** | `.text` sits at `opacity: 0`; `.card:hover .text` sets it to `1`. `display: none` can't be animated, so opacity is used instead. |
| **Image zoom** | `.card:hover img` applies `transform: scale(1.1)`. `overflow: hidden` on `.card` clips the overflow so the grid doesn't shift. |
| **Shadow lift** | `.card` carries a resting `0 2px 8px` shadow that deepens to `0 10px 30px` on hover. |

All three use the same selector shape — hover the **card**, style a **descendant** — so no
JavaScript is needed.

## Layout notes

- `.container` is a 3-column grid: `grid-template-columns: repeat(3, 1fr)` with a `20px` gap.
- `.card` uses `aspect-ratio: 3 / 4` rather than a fixed height. The source images are all
  1086×1448 (3:4), so the card's shape matches the image's — with matching ratios,
  `object-fit: cover` crops nothing and `contain` leaves no bars.
- `.card img` is `width: 100%; height: 100%; object-fit: cover`. Sizing lives on the card;
  the image only fills whatever it is given.
- `.text` centers its heading and caption with a flex column and both centering properties.

## Transition gotchas this project demonstrates

- **Transitions belong on the base rule, not `:hover`.** Declared inside `:hover`, an effect
  fades in but snaps out — the rule stops applying on mouse-out and takes the transition with it.
- **Both endpoints need a real value.** `box-shadow: none` → a real shadow won't interpolate;
  the base rule declares an actual resting shadow instead.
- **Everything that doesn't change stays in the base rule.** The background, color and flex
  layout are all declared once on `.text`; `:hover` changes only `opacity`.

## Files

```
Position-Practice/
├── index.html      # six .card blocks, each an <img> plus a .text overlay
├── style.css       # all styling
├── README.md
└── images/
    ├── image-1.png … image-6.png   # the six cards
    └── screenshot.png              # preview above
```

## Running it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Known limitations

- Desktop-only — there are no media queries, so the 3-column grid stays 3 columns on a phone.
- The effects are hover-only, so the captions are unreachable on a touchscreen.
- The overlay is hidden with `opacity: 0` but is still present and selectable; it would need
  `pointer-events: none` before it could safely contain a link or button.
