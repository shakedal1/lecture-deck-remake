# Slide patterns

Copy-ready markup for the Slides type's format (every style is inline, the canvas is 1920×1080,
and the padding is 128px). Pick colors from the palette once per deck; this one has been used and works.

## Palette and type

| Token | Hex | Use |
|---|---|---|
| ink | `#13202B` | dark slides, headings |
| paper | `#F6F4EE` | content slide background |
| quiz bg | `#ECE8DD` | quiz slide background |
| card | `#FFFDF8` | cards on paper, border `#DAD4C6` |
| muted | `#4C5A66` | secondary text |
| blue | `#2459D8` | eyebrows, "good" result (text `#1D47AE`, tint `#DCE6FB`) |
| orange | `#B4471A` | analogies, quiz pill, "bad" result (text `#9A3B12`, tint `#FBE3D4`) |
| code bg | `#1B2A36` | code listings, text `#E6EDF3`, keywords `#8DB4FF`, literals `#FFB37A`, comments `#93A4B3` |
| **command chip** | bg `#101A23`, text `#FFD08A`, bar `#FF8A3D` | **commands only** |

Fonts: `Rubik` (400..700, covers Hebrew) and `JetBrains Mono` (400;600) in the `faces` section of
`deck.json`. Type scale: 120 / 96 / 72 / 44 / 32 / 28 / 24.

## Title block (every content slide)

```html
<div style="display:flex;flex-direction:column;gap:16px">
<p style="font-size:24px;font-weight:600;letter-spacing:3px;text-transform:uppercase;color:#2459D8">Steps 1–2</p>
<h2 style="font-size:72px;font-weight:600;line-height:1.1;color:#13202B">Compile, then synthesize</h2>
</div>
```

## Command chip (the most noticeable element on a slide)

In the flow (layout handles the position):

```html
<div style="display:flex;flex-direction:row;gap:24px;align-items:center">
<p style="font-family:'JetBrains Mono', monospace;font-size:36px;font-weight:600;color:#FFD08A;background:#101A23;border-left:10px solid #FF8A3D;border-radius:12px;padding:18px 32px;box-shadow:0px 8px 24px rgba(19,32,43,0.25)">% analyze -sv09 toy.v</p>
<p style="font-size:28px;font-weight:600;color:#B4471A">compile</p>
</div>
```

Pinned, so it can build in first:

```html
<p data-build-in="pop 1" style="position:absolute;left:128px;top:300px;width:820px;font-family:'JetBrains Mono', monospace;font-size:40px;font-weight:600;color:#FFD08A;background:#101A23;border-left:10px solid #FF8A3D;border-radius:12px;padding:20px 32px;box-shadow:0px 8px 24px rgba(19,32,43,0.25)">% prove -all</p>
```

Size the chip to its text: about 0.6 × font-size per character, plus padding. Hebrew deck:
`border-right` instead of `border-left`, a `row-reverse` row, and the chip text stays plain LTR
(no isolates inside it).

## Code listing (quieter than commands)

```html
<div style="width:860px;background:#1B2A36;border-radius:16px;padding:36px 40px">
<p style="font-family:'JetBrains Mono', monospace;font-size:26px;line-height:1.4;color:#E6EDF3"><span style="color:#8DB4FF">always</span> @(<span style="color:#8DB4FF">posedge</span> clk) <span style="color:#8DB4FF">begin</span><br>&#160;&#160;state &lt;= <span style="color:#FFB37A">3'b000</span>;</p>
</div>
```

Indent with `&#160;`, because whitespace collapses. Escape `<` as `&lt;`.

## Concept + analogy card

```html
<div style="flex:1;display:flex;flex-direction:column;gap:20px;background:#FFFDF8;border:1px solid #DAD4C6;border-radius:16px;padding:44px">
<h3 style="font-size:44px;font-weight:600">“Is this always true?”</h3>
<p style="font-size:28px;line-height:1.4;color:#B4471A"><i>An elevator never moves with its doors open.</i></p>
</div>
```

## Step-by-step reveal (pinned cards)

Builds only work on `position:absolute` children, so they're pinned directly on the `<section>`.
For 6 cards across: width 256, gap 20, `left = 128 + i × 276`.

```html
<div data-build-in="rise 1" style="position:absolute;left:128px;top:340px;width:256px;display:flex;flex-direction:column;gap:14px;background:#FFFDF8;border:1px solid #DAD4C6;border-top:6px solid #2459D8;border-radius:12px;padding:28px">
<p style="font-size:44px;font-weight:700;color:#2459D8">1</p>
<p style="font-family:'JetBrains Mono', monospace;font-size:28px;font-weight:600;color:#13202B">analyze</p>
<p style="font-size:24px;line-height:1.35;color:#B4471A"><i>Proofread the recipe</i></p>
</div>
<!-- next card: left:404px; data-build-in="rise 2" … -->
```

## Source picture beside a drawing

A picture cropped from the original, pinned so it can build in after the drawing it backs up,
with a caption that links it to the running example. Size the `<img>` to the crop's aspect ratio.

```html
<div data-build-in="pop 4" style="position:absolute;left:1232px;top:320px;width:560px;display:flex;flex-direction:column;gap:10px">
<img src="/_blob/<id>" alt="What the picture shows, in words" style="width:560px;height:526px;object-fit:contain;border-radius:12px">
<p style="font-size:24px;line-height:1.35;color:#4C5A66">A full character: the same idea, thousands of faces.</p>
</div>
```

To make room, shrink the drawing panels (for example three 400px panels become three
320px panels) rather than the picture. A tool screenshot that replaces a drawn wireframe
keeps the numbered markers (56px orange circles pinned over it) and the callouts that build in beside it.

## Diagram drawn piece by piece

Draw the static part (circles) as one pinned SVG with no build. Then pin one SVG per arrow
(or arrow group) **at the same canvas coordinates**, each with a build order. State labels are
pinned `<p>`s over the circles, because SVG `<text>` gets no fonts. Arrowheads are small
polygons: tip E, base `E − 18u`, half-width 9 along the normal.

```html
<svg aria-label="States" viewBox="0 0 880 500" width="880" height="500" style="position:absolute;left:128px;top:330px;width:880px;height:500px">…circles…</svg>
<svg data-build-in="fade 1" aria-label="000 to 001" viewBox="0 0 880 500" width="880" height="500" style="position:absolute;left:128px;top:330px;width:880px;height:500px"><line x1="216" y1="170" x2="286" y2="170" stroke="#2459D8" stroke-width="4"/><polygon points="304,170 286,161 286,179" fill="#2459D8"/></svg>
```

Reachable states: fill `#DCE6FB`, stroke `#2459D8`. Unreachable: fill `#ECE8DD`, stroke `#7C848C`,
`stroke-dasharray="10 8"`. Explain the coloring in words too, not by color alone.

## Waveform, cycle by cycle

A static SVG for the clock and the signal rows, then one pinned highlight box per cycle with
`data-build-in="fade N"`, plus a pinned callout ("cover hit") with `pop` last. A value bus is a
hexagon polygon per cycle: `x+6,mid x+20,top x2-20,top x2-6,mid x2-20,bottom x+20,bottom`.

## Magic move between two slides

Both slides get `data-transition="magic"`, and the moving element has the **same `id`** and is
pinned on both:

```html
<!-- slide A -->
<div id="glow" style="position:absolute;left:760px;top:600px;width:72px;height:72px;border-radius:50%;background:#FFB37A;box-shadow:0px 0px 36px #FFB37A"></div>
<!-- slide B: same id, new left -->
<div id="glow" style="position:absolute;left:672px;top:600px;width:72px;height:72px;border-radius:50%;background:#FFB37A;box-shadow:0px 0px 36px #FFB37A"></div>
```

Good uses: the light hopping between lamps, a highlight moving to the next cycle, a bound
interval narrowing from "1 –" to "1 – 3" to "3".

## Quiz slide

The quiz background is `#ECE8DD`, with an orange pill (`background:#B4471A;color:#FFFDF8;border-radius:999px`).
The question and the blanks are in the flow. The answer is a pinned dark box that builds last:

```html
<div data-build-in="rise" style="position:absolute;left:128px;top:744px;width:1664px;display:flex;flex-direction:column;gap:10px;background:#13202B;border-radius:16px;padding:32px 40px">
<p style="font-size:32px;line-height:1.4;color:#F6F4EE"><b style="color:#FFB37A">Answer:</b> 001 → 010 → 100 → 000 → 001</p>
</div>
<aside>Answer also here for the presenter.</aside>
```

Make sure the box ends above y 952: estimate lines × font-size × line-height + padding.

## Results table

Status cells say the word as well as the symbol and the color: `✓ Covered`, `✗ CEX`. Set
`width:N%` on every cell of the first row. Use 26–28px, and keep each cell to one line where possible.

## Hebrew (RTL) kit

| Need | How |
|---|---|
| Hebrew text block | `<p style="…;text-align:right">&#8295;טקסט&#8297;</p>` (RLI…PDI). The paragraph base stays LTR, and the isolate fixes word order and punctuation. |
| code inside a Hebrew sentence | `&#8294;state[0] &lt;= state&#8297;` (LRI…PDI), otherwise `-sv09` turns into `sv09-` |
| multi-line text with `<br>` | isolate **each line** separately, or use separate `<p>`s |
| rows of cards | `flex-direction:row-reverse`, so the first child lands on the right |
| explanation + code side by side | children `[explanation, code]` with `row-reverse`, which puts the explanation on the right |
| accent bars | `border-right` + `padding:… 24px … 0px` |
| pills aligned | the parent column gets `align-items:end` |
| sequences | `000 ← 001 ← 010` (the arrow points left) |
| tables | write the columns in reverse order, and put `text-align:right` on each `<th>` |
| keep LTR | command chips, code listings, waveforms, GUI wireframes, state diagrams |
| quotes | „…” |
| names of people and Jasper GUI terms | leave them in Latin script |
