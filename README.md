# 🌼 Loves Me, Loves Me Not

A tiny pixel-art browser game. Pluck the petals off a daisy under a dithered night sky and find out if they love you.

Everything lives in a single HTML file. There are no dependencies, no build step and no assets to download.

<!-- Add a screenshot or GIF here, e.g. ![Screenshot](docs/screenshot.png) -->

## Features

- **Pluck petals** by tapping or clicking them. The text flips between **LOVES ME** and **LOVES ME NOT** with each one.
- **A different daisy every time.** Each flower gets 9 to 13 petals with slightly varied lengths, widths and tints, so the ending is never fixed.
- **Floating petals.** Plucked petals drift down, land in the grass and dither away.
- **Hand-built pixel scene.** A halftone-dithered gradient sky, a glowing moon, twinkling stars, layered hills, textured grass, a stem and two leaves.
- **Built-in pixel font** for the title text.
- **Works on desktop and mobile.** The canvas scales to fit the screen and stays crisp.

## Getting started

1. Download or clone this repository.
2. Open `loves-me-not.html` in any modern browser.

That's it. To serve it locally instead:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000/loves-me-not.html
```

## Controls

| Input | Action |
| --- | --- |
| Tap / click a petal | Pluck it |
| `Space` or `Enter` | Pluck a random petal |
| `R` or the **New daisy** button | Start over with a fresh flower |

## How it works

The game draws into a small **150 × 210 pixel** framebuffer and scales it up with `image-rendering: pixelated`, so every pixel stays sharp.

- **Framebuffer.** The scene is a `Uint32Array` over an `ImageData`. The static background is built once. Each frame copies it into the buffer and draws the moving parts on top.
- **Dithering.** The sky blends between palette colours with a 4×4 Bayer matrix, which gives the halftone look. The moon glow, the grass edge and the petal fade-out use the same matrix.
- **Petals.** Each petal is rasterised pixel by pixel from a rotated, egg-shaped profile, with edge, shadow and midrib shading. Tapping converts the pointer position into each petal's local coordinates to work out which one was hit.
- **Text.** A 5×7 bitmap font covers the letters needed for the two phrases, drawn with a drop shadow.

## Customising

Tweak these near the top of the script in `loves-me-not.html`:

| What | Where |
| --- | --- |
| Canvas resolution | `W`, `H` |
| Flower position and centre-to-petal gap | `CX`, `CY`, `R0` |
| Sky palette | the `SKY` array in `buildBg()` |
| Petal colours | the `PC` object |
| Number of petals | `newDaisy()` (currently `9 + random(0..4)`) |
| Star and sparkle positions | `buildBg()` and the `sparkles` array |
| Phrases and colours | `pluck()` and the `drawText()` calls in `frame()` |

The background uses a fixed seed (`90210`), so the stars, hills and grass look the same on every load. Change it to get a different night.

## Browser support

Any browser with HTML5 canvas and typed array support (current Chrome, Edge, Firefox and Safari, including mobile).

## License

No license. This project is free to use however you like.
