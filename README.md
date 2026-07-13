# 極端氣候動態模擬儀 · Extreme Weather Simulator

Interactive, physics-approximated extreme weather simulator rendered entirely
in a single self-contained HTML file — Canvas 2D + vanilla JS, no build step,
no dependencies, no external requests.

## Features

- **9 weather modes** — 颱風暴雨 Typhoon, 強對流冰雹 Hail, 龍捲風 Tornado,
  暴風雪 Blizzard, 沙塵暴 Dust Storm, 風暴潮 Storm Surge, 下擊暴流 Downburst,
  凍雨冰暴 Ice Storm, 熱浪乾旱 Heatwave — each with its own particle shape,
  ground reaction, palette, and severity presets (light / mid / severe).
- **Parallax particle engine** — three depth layers with wind, gust
  turbulence (frequency scales with intensity, not just amplitude), and a
  boundary-layer wind shear (surface friction slows wind near the ground).
  Rendering is batched into one `Path2D` per layer instead of a draw call
  per particle.
- **Ground physics per mode** — liquid splash, hail bounce-and-shatter,
  snow accumulation that visibly raises the ground line, a thin reflective
  ice glaze, rising storm-surge water with wind-driven waves, and static
  cracked-earth polylines for drought.
- **Lightning + thunder** — procedural jagged bolts (recursive midpoint
  displacement) with branching, a double-pulse screen flash, and
  synthesized thunder (Web Audio API) delayed by a real light-then-sound
  calculation: `distance_km × 1000 / 343 m·s⁻¹`. Closer strikes are a sharp,
  bright crack; distant ones are a dull, rolling boom.
- **在地情境 · Here & Now** — reads the browser's Geolocation API and local
  clock, derives a latitude-based climate zone, a hemisphere-correct season,
  and a real solar-altitude angle, then suggests a climatologically
  plausible mode and can tint the sky toward real day/night lighting. This
  is a coarse *inference*, not observed weather — see Limitations.
- **Instrument-console UI** — dark theme throughout, fader-style range
  inputs, a live signal badge with per-mode hazard classification (e.g.
  hail diameter, EF-scale tornado estimate, visibility in a dust storm).

## Run locally

No build step. Either open `index.html` directly in a browser, or serve it
so the Geolocation API has a proper origin:

```bash
python3 -m http.server 4173
# → http://localhost:4173
```

## Deploy

Static single-file site — deploys as-is to Vercel, Netlify, GitHub Pages,
or any static host with zero configuration.

## Limitations

This is a stylized visual approximation for demonstration, not a
meteorological forecasting tool or a scientific model. The sandboxed
environment this was built in cannot make outbound network requests, so no
live weather API is ever called — "current conditions" only ever means
values derived from your device's own coordinates and clock, never an
actual observation.
