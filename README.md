# Plane Game

A pixel-art plane game set over Cape Town, built with [Three.js](https://threejs.org).

Fly low. The closer you skim the rooftops, the faster the points stack — but the
ground is solid and so is every wall. **Low is good.**

**[▶ Play it](https://adolphvancoller.github.io/Plane-Game/)** *(once GitHub Pages is switched on — see below)*

---

## Controls

| Input | Action |
| --- | --- |
| `Space` / `↑` / `Enter` | Flap |
| Click or tap anywhere | Flap |
| Speaker button (top left) | Mute |

The first flap starts a run. Before that the game sits in attract mode with the
plane flying itself.

## How it plays

- **Scoring is inverted.** Points only accumulate below 12 m of clearance and climb
  steeply the closer you get. Cruising at altitude is safe and pays nothing.
- **The gaps are the game.** Landmarks stand on open ground, so you can dive into
  the space between two buildings for near-zero clearance — as long as you climb
  out before the next wall.
- **Gold wings** give you a wingman: double points and one spare plane. While both
  are flying, taps *alternate* between them, and a lime chevron marks which plane
  your next tap controls. Lose the leader and the wingman is promoted. The run ends
  when both are down.
- **Cleared** counts the landmarks you've passed. **Closest pass** is the tightest
  clearance you survived — the number actually worth chasing.

Your best score is kept in `localStorage`, so it is per-browser and never leaves
your machine.

## Running it

There is no build step and no dependencies to install. The whole game is one
self-contained `index.html`.

```bash
git clone https://github.com/AdolphVanColler/Plane-Game.git
cd Plane-Game
```

Then either open `index.html` in a browser directly, or serve the folder:

```bash
python -m http.server 8000
# then visit http://localhost:8000
```

Three.js (r128) and the Press Start 2P typeface load from CDNs, so the page needs
an internet connection the first time.

## Publishing to GitHub Pages

The game is already named `index.html`, so it is ready to serve:

1. Go to **Settings → Pages** in this repository.
2. Under *Build and deployment*, set **Source** to `Deploy from a branch`.
3. Choose branch `main`, folder `/ (root)`, and press **Save**.

It goes live at `https://adolphvancoller.github.io/Plane-Game/` a minute or so later.

## How it's built

It is a 2D pixel game rendered through a 3D library, which is a slightly odd thing
to do, so the parts worth knowing:

- **Orthographic pixel engine.** The scene renders into a low-resolution
  `WebGLRenderTarget` (around 480×270, sized to your window's aspect) which is then
  upscaled to the screen through a full-screen quad with `NearestFilter`. Every
  layer therefore shares one true pixel grid, and sprite positions snap to whole
  pixels. That single step is what separates pixel art from a smooth render
  imitating it.
- **All art is generated in code.** There are no image files in this repository.
  Table Mountain, Devil's Peak and Lion's Head come out of a silhouette function
  with the rock face split into vertical buttress bands; the stadium is built from
  ellipse maths — roof ring, seating bowl, radial ribs and a glass facade extruded
  down from the roof's front arc. Everything is painted onto `<canvas>` elements at
  load time and uploaded as textures.
- **Collision is read from the art.** Each landmark's hit profile is a per-column
  scan of its own sprite's alpha channel. Clearance is measured against the actual
  painted pixels — the curve of the stadium roof, the notch between two buildings,
  the exact top of a tree — rather than an approximating box.
- **Parallax** runs across six bands (mountain, hills, skyline, bushes, landmarks,
  coast), each wrapping two copies of a 960 px tile.
- **Sound** is synthesised with the Web Audio API at runtime. No audio files.

## Credits

Made for Die Van Collers. Inspired by the low-flying format of
[Flappy Link](https://www.flappylink.com/).

Three.js is MIT licensed. Press Start 2P is licensed under the SIL Open Font License.
