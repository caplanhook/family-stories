# Voice Notes → Long-Form Writing: Take-Home Submission

> A weekly prompt asks an older relative to tell one story out loud. The tool turns her unpolished recording into a structured written story in her own voice and delivers it to her family with the original audio attached.

## Prompt

Build a tool that turns voice notes into a written document. Focus on long-form writing.

## Product Goals

- **Valuable:** Leverages unique value from voice + AI
- **Defensible:** Differentiated from generalist AI assistants
- **Constrained:** Meaningful scope achievable within 3 months

## Definitions

I defined the two nouns in the prompt first to clarify the scope.

| | Defined as | Out of scope |
|---|---|---|
| **Voice Notes** | Self-recorded, unpolished voice from a single individual. | Recorded conversation (multi-speaker): meetings, sales calls, podcasts. |
| **Long-form** | Extended, self-contained prose (500+ words). | Note-taking. Summarization. Data entry (e.g. CRM). |

## What's great about voice + AI

- Lower barrier to entry than writing
- Supports non-linear thinking. The speaker can come back to earlier ideas
- AI can clean up thinking: untangle chronology and surface the point the speaker was circling
- Speech preserves authentic phrasing that writing may lose

## Considered four concepts for the purpose of this demo

| Concept | 1-liner | Valuable | Defensible | Constrained |
|---|---|---|---|---|
| 1. Business Decision Memo | Think out loud, get a decision memo | Med | No | No |
| 2. Occasion Speech | Ramble about someone, get the toast | Low | No | Yes |
| 3. Content Marketing | Ramble on a walk, get a newsletter | High | No | No |
| 4. Family Memories | Prompt Grandma weekly, get her stories in her own voice | High | Yes | Yes |

Family Memories won on defensibility and constraint: a generalist assistant can draft a memo or newsletter today, but it cannot run a weekly ritual between one family and one voice, and the per-story unit (one prompt, one recording, one finished piece) keeps the scope contained.

## 1. Who are you building for?

Three user groups under consideration:

- **Buyer:** Adult child, someone in their 20s through 50s who realizes their parent's stories exist only in that parent's head
- **User:** Storyteller, someone in their 70s or older. Comfortable with email but not technically savvy.
- **Audience:** The rest of the family. Mix of ages and technical fluencies.

I built the prototype entirely for the storyteller, because the core UX challenge of the product is making it easy for the older user to record themselves and edit the result prior to publishing. Every design decision serves someone who will abandon the product when there is unnecessary friction: the emailed link is the auth (no login anywhere), there is one primary action per screen, and composition never requires typing. Keyboards appear only for word-level corrections; anything story-sized is done by talking. The buyer and audience sides are real product surface, but they are conventional product surface. The storyteller's experience is the part that has to be invented, so it is the part I prototyped.

## 2. What's the most important thing to get right?

The most important feature is preserving the voice of the author. AI has a tendency to flatten the author's voice into more generic prose, and this is the one product where that failure is fatal: the written story will outlive the product and probably the speaker. The system's core rule is to **reorder and stitch, never rewrite**: the AI may untangle chronology, reposition tangents, and add minimal connective tissue, but it should not meaningfully change the phrasing. In the demo story, every sentence is verbatim from the transcript.

The second half of getting this right is honesty about uncertainty. Transcription fails most on exactly what family stories are dense with (names, dates, places), and silently guessing wrong on a family surname is the worst possible failure. So the prototype flags low-confidence words rather than papering over them, including a seeded case where the storyteller self-corrects a year mid-ramble and the system flags it instead of guessing. Corrections propagate (fix a surname once, it updates everywhere), and edit interfaces match the edit's granularity: keyboard for word-level precision, voice for story-level changes.

Attaching the original recording to every story provides both a sentimental artifact the family will treasure and the ground truth that proves the AI is being truthful.

## 3. What would this look like with 5 engineers and 3 months?

**North star metric:** a family member rates the story as "sounds like her." Supporting funnel: prompt opened → recording completed → story saved → family opened → reaction sent → next week's recording completed.

### Month 1: Quality loop and foundations

- Real streaming transcription tuned for older voices, accents, and phone-quality audio (a known weak spot for off-the-shelf ASR)
- Eval harness for the restructuring itself: verbatim-preservation rate, chronology accuracy, tangent-handling, and the human "sounds like her" rating pipeline
- Entity handling: per-family name dictionary (people, places) that improves transcription and powers confidence flagging
- Productionize the storyteller flow from the prototype, plus basic family delivery (story + audio via email/link)
- Instrument the full funnel from day one

### Month 2: The full loop and the interviewer

- **Buyer experience:** onboarding, gifting flow, prompt library, prompt scheduling and personalization
- **Family feed:** stories arrive weekly with reactions and comments, routed back to the storyteller as encouragement ("Mom, I never knew that!" is the retention engine)
- **The AI interviewer (the moat):**
  - Generates next week's prompt from this week's story ("you mentioned Ruth three times; tell me about Ruth")
  - Builds an entity graph across stories (people, places, eras) so the corpus deepens instead of just accumulating
  - Scoped carefully so it feels like a curious grandchild, not an interrogation
- **Phone-call capture:** the storyteller can simply answer a scheduled weekly phone call and talk; no smartphone or link required. This is the single biggest accessibility unlock for the oldest users.
- **Multi-language:** record in a native language; the family receives both the original and a voice-preserving translation. Immigrant family stories are a core use case, not an edge case.

### Month 3: The harvest and the household

- **The printed book:** automated layout from the year's stories, per-story QR codes that play her voice, gift-timed production (subscriptions started at Christmas mature at Christmas)
- **Photos:** family members attach photos to stories; the entity graph suggests matches ("this story mentions the house on Mitchell Street")
- **Multi-storyteller households:** both grandparents, or siblings telling the same story from different sides, woven into one volume
- **Family-driven prompts:** grandchildren can submit questions, which become prompts in the grandchild's name
- **Private family podcast feed:** the original recordings, subscribable only by the family
- **Launch motion:** pricing, holiday gifting funnel, and the natural referral loop (every family audience member is a future buyer for their own parent)

### Team shape

- 2 engineers on the transcription/restructuring quality loop and evals
- 2 product engineers across the three surfaces (storyteller, buyer, family)
- 1 engineer on audio infrastructure, telephony, and the print pipeline

### What I still wouldn't build

- Social features beyond the family, public sharing, or anything that makes this feel like content
- Rich-text composition for the storyteller; composition stays in voice, keyboards are for corrections
- The product is a ritual between one family and one voice, and the discipline is keeping it that
