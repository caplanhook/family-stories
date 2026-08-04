# Family Stories — "Grandma's Stories"

A weekly prompt asks an older family member to tell **one** story out loud. The tool
turns her unpolished recording into a structured written story **in her own voice** —
reordered and stitched, never rewritten — and delivers it to her family with the
original audio attached.

**Target persona:** Grandma, ~80, non-technical. Every screen is built for someone
who has never used an app beyond email: large type, high contrast, one primary action
per screen, no navigation chrome, and voice as the way you *compose* (the keyboard only
ever fixes a single misheard word).

This is a **2-hour interview prototype** — a demo vehicle for product judgment, not
production software. The linear demo flow is: **Email → Record → Processing → Story →
Edit → Save/Deliver.**

---

## How to run

No build step, no dependencies. From this folder:

```bash
python3 -m http.server 8000
```

Then open **http://localhost:8000/** and click **Tell this story**.

Running from a `localhost` origin (rather than opening the file directly) is what lets
the browser grant microphone access for real recording. If the mic is blocked or
unavailable, the app falls back to a **"Use sample recording"** button and keeps going.

---

## Real vs. faked

**Actually works (real):**
- **Microphone recording** via `MediaRecorder` — record, pause/resume, stop, with a live
  elapsed-time indicator and *no* live transcript.
- **Graceful mic fallback** — if permission is denied or unavailable, a "Use sample
  recording" button proceeds with a soft audio tone generated in-page.
- **Audio playback** on the story page — plays back the exact recording you just made
  (or the sample).
- **Story rendering + both editing tiers**: word-level inline edit with case-insensitive
  propagation across the whole story; paragraph-level voice-first options.
- **Uncertainty flags**, the suggestion chips, the propagation toast, paragraph
  remove-with-undo, and the save/deliver confirmation.

**Deliberately faked (labeled, not hidden):**
- **The weekly email** — a static mock screen inside the app, not a real inbox.
- **Speech-to-text** — no real transcription runs. Whatever audio is recorded, the app
  "transcribes" to the canned transcript in [`transcript.txt`](transcript.txt). In the
  real product this is an ASR pass.
- **The AI restructuring** — the 5-paragraph story on the story page is hand-tuned
  content, not a live model call. In the real product this is a single LLM pass with a
  strict *"reorder and stitch, never rewrite"* system prompt. Every sentence shown is
  Grandma's own phrasing from the transcript, only reordered and lightly stitched; the
  pierogi tangent was cut (cutting/repositioning tangents is allowed, rewriting is not).
- **The paragraph re-dictation** — "That's not quite right" records for real, but the
  swapped-in replacement text is canned (wired for paragraph 4).
- **Family-side delivery and the printed book** — represented by the confirmation screen
  and a "Story 3 of 52" progress line only.

### The seeded imperfection (worth pointing out live)
In the transcript, Grandma says "1953?" then corrects herself to **1935**. The "AI"
kept the *wrong* year (1953) and **flagged it** with a dotted underline rather than
silently guessing. Tapping it offers the **1935** chip — this is the demo's honest-
uncertainty beat. The surname **Kowalski** is flagged the same way (chip suggests
"Kowalsky") and demonstrates propagation: fixing one occurrence updates them all.

> **Doc note:** §5.2 of the PRD, used verbatim, contains **two** occurrences of
> "Kowalski" (not three, as a couple of other lines in the PRD state). Rather than alter
> the verbatim story text, the propagation toast counts the *actual* occurrences at edit
> time ("appears 2 times — updated all"), so it stays correct however the text is edited.

---

## Deliberate cuts (and why)

- **Accounts / auth** — a demo doesn't need identity; it adds friction for an 80-year-old
  and nothing to the product story.
- **Buyer-side prompt picker** — the family member who sets this up would choose weekly
  prompts; out of frame for Grandma's linear flow.
- **Story library / archive** — the closing "Story 3 of 52" gestures at it; browsing past
  stories isn't part of the record-one-story narrative.
- **Book rendering** — represented by a confirmation state; real PDF/print layout is a
  fulfillment concern, not a product-judgment one.
- **Section re-recording / retakes** — recording is one honest take; re-telling happens at
  the paragraph level in editing, not as waveform surgery.
- **Rich-text composition** — cut because **the target user composes by talking; keyboards
  are for corrections.** No free-cursor text area or formatting controls, on purpose.
- **Per-word audio alignment (tap-a-word-to-hear-it)** — cut as **demo flourish vs. build
  cost**; the single top-of-page player carries the "her real voice" point.
