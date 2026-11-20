# Scene block for the fixed template

Replace the contents between the two marked lines, keeping both markers. Everything else stays unchanged. No dependencies, runtime downloads, external fonts, images, or extra script tags. Use plain UTF-8.

Write uniquely named `function drawSomething(t) { ... }` declarations on their own starting lines. Every visible mark must come from `glyph(x, y, asciiChar, color, alpha)` in a named layer. Use integer cell positions on a 72 × 66 grid (x: 0–71, y: 0–65), single printable ASCII characters, six-digit CSS colors, and optional alpha 0–1. `seed(x,y)`, `clamp(x)`, and `smooth(x)` are supplied. Do not use canvas calls, time sources, DOM, or mutable state in the scene block. Given `t`, a layer must draw the same frame contents every time. Each opening layer must draw at least one mark at `t=0`.

Define `const scene` with:

- `lang`: document language, normally `ko` or `en`; `title` and `description`: scene name and accessible description; `bg`: canvas background color.
- `duration`: requested motion duration in seconds (default 12, not counting the 5.16-second opening build); `stillAt`: optional representative motion time in seconds.
- `layers`: ordered objects `{ fn, label, weight }`. Use background-to-foreground order, no more than 8 layers. `weight` is optional and positive; it controls how much of the opening build goes to this layer.
- `motion`: `[time, layerFunction, label]` tuples starting at 0 with strictly increasing time, no later than `duration`. Each function must be in `layers`. Point each cue to the actual layer responsible for the effect at that time.

Example shape (not a scene to copy):

```js
function drawSky(t) { /* original glyph calls, including at t=0 */ }
function drawSubject(t) { /* original glyph calls with timed change */ }
const scene = {
  lang: 'en', title: 'Scene name', description: 'What is visible', bg: '#080f12',
  duration: 12, stillAt: 9,
  layers: [{fn: drawSky, label: 'Sky'}, {fn: drawSubject, label: 'Subject', weight: 2}],
  motion: [[0, drawSky, 'Opening'], [2, drawSubject, 'Main action']]
};
```

A scene brief such as “보름달이 뜬 파리의 에펠탑” suggests separate moon, skyline, and tower layers. Let the moon and skyline establish the subject; use lights rising along the tower as the payoff. Design new geometry and timing for the actual prompt rather than copying this interpretation mechanically.
