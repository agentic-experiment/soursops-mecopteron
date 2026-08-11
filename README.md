# Soursops Mecopteron

*A horticultural calamity.*

## What it is

A generative micro-ecosystem. Soursop fruits — wobbly, spiky, green things — grow,
ripen, and burst open. From each husk hatches a **mecopteron**: a scorpionfly with a
curled tail and two pairs of iridescent wings. Every fly's wing shape is procedurally
unique. The flies wander a smooth vector field forever, leaving faint luminous trails,
until you click to plant the next catastrophe.

## Step-by-step reasoning

### 1. Naming the thing
The name is the whole brief. "Soursop" is a spiky green fruit. "Mecopteron" is
scorpionflies (order Mecoptera). So the app is about fruit that hatches scorpionflies.
That single juxtaposition gave me the entire narrative: a plant that doesn't fruit so
much as *infest*. No further plot required.

### 2. Picking the medium
The brief allows any HTML/CSS/JS shape. A generative canvas app wins because:
- It needs no assets, no libraries, no network — beauty comes from math, which is free.
- It can be one file. One file is the laziest deployment that still works.
- Animation is intrinsic: a hatching fruit and a flying insect are motion first.

So: one `index.html`, embedded CSS, one `<script>`, no build step. Run = open the file.

### 3. Designing the soursop
A soursop is a rounded, bumpy fruit with soft spines. I drew it as:
- A wobbly ellipse: 48 points whose radius is perturbed by seeded noise, so every fruit
  has a unique outline. A radial gradient (dark green edges, light highlight) gives it
  volume with one fill.
- 34 spines as small outward quadratic curves with pale tips.
- A tiny stem and leaf so it reads as *botanical*, not a blob.

A fruit lifecycle state machine: `grow` → `ripe` → `crack` → `split` → dead.
- *grow*: scale animates from 15% to 100% with a slow breathing pulse.
- *ripe*: the color lerps green → yellow-green → gold as it "ripens".
- *crack*: it jitters, a jagged vertical line widens down its middle.
- *split*: the two halves pivot apart about the centerline and fade away.

That's only ~80 lines and produces a legible, charming death scene.

### 4. Designing the mecopteron
Scorpionflies are defined by: a small body, two pairs of veined wings, and a tail curled
up over the back. I rendered each:
- **Body**: thorax (gradient steel-blue, glowing) + striped abdomen + small head with
  two glowing eyes, six legs, two flicking antennae.
- **Tail**: two quadratic segments that curl up and forward, ending in a small dark
  stinger. The scorpion silhouette is the "mecopteron" identity; it had to be obvious.
- **Wings**: four of them, drawn as closed Bézier lobes with a vein line. The key move —
  wing shapes are seeded per-fly, so the two control points that shape each lobe are
  random and never reused. Every fly has unique wings. Flapping is faked by scaling the
  wing's y-axis by `sin(flap)` (a negative scale flips it to the far side), which reads
  as wingbeat in 2D with zero extra state.

### 5. Making them fly beautifully
Random wandering looks like moths; that's boring. I used a **flow field**: each fly's
heading is `sin(x, t) + cos(y, t)` with per-fly phase offsets from its seed. Heading is
blended toward the field value each frame and speed wobbles gently. Result: smooth,
swooping, divergent flight — no two flies repeat a path, and the whole swarm is chaotic
but calm. Trails come free from a translucent dark overlay each frame (fade-to-dark),
plus a faint line segment between old and new position.

### 6. Interaction
Three inputs, one sentence of help text:
- **Click** anywhere → plant a new soursop.
- **s** → spawn a fly directly (for the impatient).
- **r** → reset the whole calamity.

### 7. Atmosphere
Deep near-black green background, ~70 drifting, twinkling motes, and a CSS vignette. All
of it is cheap (no per-pixel work), so even a full screen of flies stays at 60fps.

## How to run

Live on GitHub Pages: https://agentic-experiment.github.io/soursops-mecopteron/

## Controls

| Input | Action |
|-------|--------|
| click | plant a soursop |
| `s` | spawn a scorpionfly |
| `r` | reset |

## Notes / deliberate simplifications

- Everything lives in one file on purpose; splitting it would be organization for its
  own sake.
- No audio. The silence is part of the mood.
- Flies never die; the ecosystem only ends when you reset it. That's fine — a calamity
  has no ending.
