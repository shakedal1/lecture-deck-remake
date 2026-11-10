---
name: lecture-deck-remake
description: Rebuild a boring or confusing lecture, lab or tutorial deck (PDF or PPTX) as a clear, engaging Slides artifact with analogies, worked examples, quizzes, drawn diagrams alongside the source's own pictures, animation and eye-catching commands, in English, Hebrew, or both. Use when someone shares course slides and says they are boring, unclear or hard to follow, asks to "explain it better", "remake the presentation", "make a new version of the lab/tutorial", or wants a Hebrew and/or English version of a lecture deck.
---

# Lecture deck remake

Turn a dry course deck into one a student actually learns from. The output is a
deck made from the **Slides artifact type** (one per language). Everything about
the slide format, files and publishing comes from that type's instructions. This
skill adds the teaching design, the animation and command rules, and the Hebrew RTL
technique on top.

Read `references/patterns.md` before writing the first slide. It has copy-ready
markup for every pattern named below.

## 1. Read the whole source first

- PDF: `Read` with `pages` in chunks of at most 20 until every page is seen. PPTX: use the pptx skill to extract text and images.
- While reading, list: the topics in order, every command or code snippet, every
  definition, every quiz or question already in the deck, and any slides that
  repeat the same screenshot with one callout changed. Those repeated slides are
  the first thing to merge.
- Also list **every picture worth keeping**: photos, real objects or models, tool
  screenshots, and figures that show something a hand drawing can't. Note each
  one's page and rough position in the `Read` preview, because you'll crop it later (§6).
- Keep every fact from the source: names, dates, emails, grading, links, command
  flags. Never invent numbers, quotes, or tool behaviour you can't derive from the source
  or from the logic of the example itself.

## 2. Plan the teaching, then build

State the plan in one line to the user and start. Don't wait for approval.

- **One running example.** Pick the source's own example and give it a concrete
  physical picture (a 3-bit one-hot FSM became "a 3-lamp light chaser"). Reuse it
  on every slide: properties become plain questions about it, results are explained
  in its terms.
- **One analogy per concept**, from everyday life, written in italics on the slide
  next to the formal definition (simulation vs formal = trying 1,000 keys vs a
  locksmith proving the mechanism; reset = "New game"; clock = metronome;
  bound = nearest coffee shop on a street; debugging "Why" = a detective going back
  in time).
- **Quizzes every 4–6 slides**: predict-before-run, trace-by-hand, "what if we
  change X", true/false. The answer is a pinned box that builds in on click; put
  the answer in the speaker notes too.
- **Keep the source's pictures, and draw the rest.** The user wants the original's
  relevant pictures in the deck. Don't drop them in favour of drawings. For each
  picture on your list:
  - If it shows the same thing a drawing of yours would (a resolution strip, a
    tool's real window, a before/after comparison), use the picture instead of the drawing.
  - If it adds something a drawing can't (a real photo, a real 3D model, a real
    dataset), put it next to your running-example drawing on the same slide, with a
    one-line caption that ties the two together.
  - If it's purely decorative or repeats another picture, leave it out, and name it in the report.
  Draw as SVG/divs (patterns.md) whatever the source only says in words: state
  diagrams, waveforms, timelines, number lines, step-by-step builds of the running example.
- **One idea per slide**, about 20–26 slides. Use the rhythm: dark statement slides
  for the cover, each section opener and the recap; light content slides; a
  warm-tinted background for quiz slides.
- End with a **cheat sheet**: every command in order with a 2–3 word gloss, plus a
  glossary of the key terms.

## 3. Animation is required, not optional

The user asked for more animation. Every deck must:

- Put `data-transition` on **every** section: `fade` for most, `push` going into a new
  section, `magic` between two slides that show the same diagram in two states.
- Give **every content slide at least one build sequence**: the key elements are
  `position:absolute` children with `data-build-in="<effect> <order>"` so they
  appear one at a time in Present mode. Builds only work on pinned children, so
  slides that reveal step by step are laid out with explicit `left/top/width`.
- Use builds that teach. Reveal a flow one step at a time (`rise 1`, `rise 2`, …),
  draw a state machine arrow by arrow (one pinned SVG per arrow group), walk a
  waveform cycle by cycle, reveal the analogy after the definition, and show quiz answers last.
- Use magic move to animate a change: same `id` on the matching pinned element of
  two consecutive slides (a lamp lighting up, a highlight moving to the next cycle,
  a bound interval narrowing).
- Vary effects with purpose: `rise` for lists and steps, `pop` for a result or a
  check, `left`/`right` for things that arrive from a direction, `fade` for
  annotations, `scale` for the big reveal. Don't use more than about 6 builds per slide.

## 4. Commands must jump off the slide

The user asked for more noticeable commands. Every command, flag, or Tcl or shell
line uses the **command chip** from patterns.md, and nothing else on the deck
looks like it:

- Its own block (`<p>`), never inline in a sentence. Spans can only carry color, so
  `font-family` on a span is silently dropped.
- A monospace face (JetBrains Mono) at 32–40px, bold. Dark chip background, a
  bright accent color reserved **only** for commands, a thick accent bar on the
  leading edge, and a prompt glyph (`%` for Tcl, `$` for shell).
- Next to each chip, a short label saying what it does ("compile", "prove all").
- On a step slide the command chip is the biggest thing after the title, and it
  builds in first.
- Code listings (whole files) use a separate, quieter code-card style so commands
  still stand out from them.

## 5. Languages

If the user asks for two languages, make two separate decks from the type, with the same
slide ids and content. Build the source language (or English) first, then the
other. Hebrew needs the RTL technique in patterns.md: isolate every Hebrew text
block with `&#8295;…&#8297;`, wrap inline code inside Hebrew in `&#8294;…&#8297;`,
use `text-align:right` and `row-reverse` rows, reverse table column order, use `←` in
sequences, and isolate each `<br>`-separated line separately. Keep code, command
chips, waveforms and GUI wireframes left-to-right. Rubik covers Hebrew and Latin.

## 6. Build and publish

Follow the Slides type's own instructions for files and calls, with these rules learned from use:

- Write `project/deck.json` (full `order`, `sections`, `faces`) and the cover,
  publish, then write and publish about 4 slides per round.
- A publish with `edits` can't also carry `files`. Edit with the Edit tool, then
  publish the files.
- If a publish fails with a network error, resend the same files. That's safe.
- **Cropping source pictures from a PDF** (do this before writing the slides that use them):
  - Copy the PDF into its own new folder in the scratchpad, and write the crops to a separate `assets` folder.
  - Get the page size with `python -I -c "import pypdf,sys;print(pypdf.PdfReader(sys.argv[1]).pages[0].mediabox)" <pdf>`.
    PyMuPDF and Pillow aren't installed here; `pdftoppm` (MiKTeX) is. Ignore its "requires Windows 10" warning.
  - Crop straight from the page: `pdftoppm -f N -l N -r 250 -x X -y Y -W W -H H -png -singlefile <pdf> <assets>/<name>`.
    Coordinates come from the `Read` preview: scale = (250 / 72) × page width in pt / preview width in px.
    A 720 pt page previewed 1000 px wide gives ×2.5.
  - `Read` every crop once and fix its edges before uploading: a sliver of the neighbouring picture is common.
  - Split a strip of panels (e.g. one image at 7 resolutions) into one crop per panel, so the panels can build in one by one.
  - Upload all the crops in one call: Artifact publish with `url`, `asset: true` and `file_paths` (up to 25).
    Use each returned `/_blob/<id>` verbatim in `<img src>`, with `object-fit:contain`
    and a width/height that match the crop's aspect ratio.
  - From a PPTX, take the images the pptx skill extracts, then upload them the same way.
- Budget the height: 824px inside the margins. Title block ≈130px, table row ≈
  2.1 × font-size per line, pinned boxes must end above y 952. Shorten text before
  shrinking it, and keep text at 24px or larger.
- Don't render or screenshot to verify unless the user asks.

## 7. Report

Give each deck's link, what changed compared with the original (the running example,
the analogies, the illustrations, which source pictures were kept, where, and which were
left out, the quizzes, the animations, the command styling, which repetitive slides
were merged), and what you didn't check. Remind the user
that the decks are private until they're shared.
