# Case Study: Capturing Family Stories with Voice + AI

> A weekly prompt asks an older relative to tell one story out loud. The tool turns her unpolished recording into a structured written story in her own voice and delivers it to her family with the original audio attached.

## Prompt

Build a tool that turns voice notes into a written document. Focus on long-form writing.

## Definitions

I defined the two nouns in the prompt first to clarify the scope.

| | Defined as | Out of scope |
|---|---|---|
| **Voice Notes** | Self-recorded, unpolished voice from a single individual. | Recorded conversation (multi-speaker): meetings, sales calls, podcasts. |
| **Long-form** | Extended, self-contained prose (500+ words). | Note-taking. Summarization. Data entry (e.g. CRM). |

## What's great about voice + AI

- Lower barrier to entry than writing
- Speaking is more accessible than typing for many users
- Supports non-linear thinking. The speaker can come back to earlier ideas
- AI can clean up thinking: untangle chronology and surface the point the speaker was circling
- Speech can itself be an artifact that captures tone and personality in a way that writing may flatten

## Product Goals
I set 3 product goals to evaluate potential directions for this take home.
- **Valuable:** Leverages unique value from voice + AI
- **Defensible:** Differentiated from generalist AI assistants
- **Constrained:** Meaningful scope achievable within 3 months

## Four concepts I considered

| Concept | 1-liner | Valuable | Defensible | Constrained |
|---|---|---|---|---|
| 1. Business Decision Memo | Think out loud, get a decision memo | Med | No | No |
| 2. Content Marketing | Ramble on a walk, get a LinkedIn post or blog | High | No | No |
| ⭐ **3. Family Memories** | **Prompt Grandma weekly, get her stories in her own voice** | **High** | **Yes** | **Yes** |
| 4. Occasion Speech | Ramble about someone, get the toast | Low | No | Yes |

**⭐ Chosen direction.** Family Memories won on defensibility and constraint: a generalist assistant can draft a memo, newsletter, or speech today, but it is far too complex to facilitate the desired behavior for an older family member who is not tech savvy.

## 1. Who are you building for?

Three user groups under consideration:

- **User:** Storyteller, someone in their 60s or older. Comfortable with email but not technically savvy.
- **Buyer:** Adult child, someone in their 20s through 50s who realizes their parent's stories exist only in that parent's head
- **Audience:** The rest of the family. Mix of ages and technical fluencies.

I built the prototype entirely for the storyteller, because the core UX challenge of the product is making it easy for the older user to record themselves and edit the result prior to publishing. 

Every product and design decision serves someone who will abandon the product when there is unnecessary friction:

-   The emailed link is the auth (no login anywhere)
-   There is one primary action per screen, and composition never requires typing.
-   The flow and typography are tuned for phones as well as desktop. 
-   Keyboards appear only for word-level corrections; anything story-sized is done by talking.

## 2. What's the most important thing to get right?

The most important thing to get right is **preserving the author's voice.** Generic AI flattens a person's phrasing into smoother, blander prose — and here that failure is fatal, because the written story will outlive the product and probably the speaker. The prototype protects the voice three ways:
- **Reorder and stitch, never rewrite.** The AI may untangle chronology, reposition tangents, and add minimal connective tissue, but it never changes the phrasing. In the demo, every sentence in the finished story is verbatim from the transcript.
- **Always show the ground truth.** Every story ships with the original recording attached — both a keepsake the family will treasure and proof that the written version reflects what she actually said.
- **Acknowledge uncertainty instead of guessing.** Transcription fails most on the proper nouns family stories are full of (names, places). Rather than guess, the prototype flags low-confidence words for a one-tap fix — like the seeded year the speaker corrects mid-sentence — and those corrections compound into a per-family corpus of people and places over the subscription.


## 3. What would this look like with 5 engineers and 3 months?

**North star: Weekly Active Rate** — the share of subscribers who record and send a story in a given week (active ÷ subscribed). The product is a weekly ritual, so weekly-active is the honest test of whether the habit is forming; as a *rate* it captures activation and retention together, on the cadence we actually sell.

**Supporting metrics**
- **Quality guardrail:** "Sounds like her": share of family recipients who rate a story as sounding like the storyteller. If this drops, nothing else matters: it's what protects the voice-preservation promise above.
- **Storyteller activation:** story completion rate: % who finish recording and send after opening a prompt.


### Month 1: Quality loop and foundations

- Real streaming transcription tuned for older voices, accents, and phone-quality audio (a known weak spot for off-the-shelf ASR)
- Eval harness for the restructuring itself: verbatim-preservation rate, chronology accuracy, tangent-handling, and the human "sounds like her" rating pipeline
- Entity handling: per-family name dictionary (people, places) that improves transcription and powers confidence flagging
- Productionize the storyteller flow from the prototype, plus basic family delivery (story + audio via email/link)
- Instrument the full funnel from day one
- **Read-only family view:** family members receive, read, and listen to each story, but never edit it
- **Starter questions at purchase:** the buyer chooses which questions Grandma will be asked, as part of the gifting flow

### Month 2: The full loop and the interviewer

- **Buyer experience:** onboarding, gifting flow, prompt library, prompt scheduling and personalization
- **Browsable story library:** a read-only archive of Grandma's stories (text + audio) that she and her family can revisit, aiding discovery and re-engagement. No comments or likes (see *What I deprioritized*)
- **The AI interviewer (the moat):**
  - Generates next week's prompt from this week's story ("you mentioned Ruth three times; tell me about Ruth")
  - Builds an entity graph across stories (people, places, eras) so the corpus deepens instead of just accumulating
  - Scoped carefully so it feels like a curious grandchild, not an interrogation
- **Phone-call capture:** the storyteller can simply answer a scheduled weekly phone call and talk; no smartphone or link required. This is the single biggest accessibility unlock for the oldest users.
- **Multi-language:** record in a native language; the family receives both the original and a voice-preserving translation. Immigrant family stories are a core use case, not an edge case.

### Month 3: The harvest and the household

- **The printed book:** automated layout from the year's stories, per-story QR codes that play her voice, gift-timed production (subscriptions started at Christmas mature at Christmas)
- **Photos:** family members attach photos to stories; the entity graph suggests matches ("this story mentions the house on Mitchell Street")
- **In-session follow-ups:** before ending a session, the interviewer asks 1–2 clarifying questions ("what did the hall look like?") to fill gaps while the memory is fresh, deepening the interviewer moat
- **Family-driven prompts:** grandchildren can submit questions, which become prompts in the grandchild's name
- **Private family podcast feed:** the original recordings, subscribable only by the family
- **Launch motion:** pricing, holiday gifting funnel, and the natural referral loop (every family audience member is a future buyer for their own parent)

### Team shape

- 2 engineers on the transcription/restructuring quality loop and evals
- 2 product engineers across the three surfaces (storyteller, buyer, family)
- 1 engineer on audio infrastructure, telephony, and the print pipeline

### What I deprioritized (and why)

| Deprioritized | Why |
|---|---|
| Live transcript while recording | Watching words appear in real time distracts an older speaker and makes them self-conscious, which suppresses natural storytelling. The prototype deliberately shows none. |
| In-app social layer (comments, likes, feeds) and public sharing | Little sustained value over a family member simply texting Grandma, and it forces yet another platform on people to learn. This is a private ritual, not a content platform; family delivery stays read-only. |
| Multi-author composition (Grandma + Grandpa in one story) | Adds real feature complexity and makes activation harder. A family that wants both voices is served by two subscriptions. |
| Rich-text / keyboard composition | The storyteller composes by talking; the keyboard exists only for word-level corrections, never for authoring. |

The throughline: this is a ritual between one family and one voice, and the discipline is keeping it that.
