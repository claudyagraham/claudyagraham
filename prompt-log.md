# Prompt log

## 2026-10-09: README bio and resume

**What I asked:** For an example bio, then how to finish step 5 and create RESUME.md, then for feedback on my resume draft.

**What it got wrong:** The example bio included details I never gave (such as internship goals), so it couldn't be used as is.

**How I caught it:** I compared it to my own background and wrote my own version instead.

**What I'd do differently:** Give the AI my real background up front so it doesn't make things up.
## 2026-10-30: Stage 1 FX hedging executive brief

**What I asked:** For help with the Stage 1 executive brief for my scenario (Scenario 18, ship-propulsion: EUR 5,600,000 receivable in 365 days). I asked for step-by-step instructions, for the dollar calculations from my scenario numbers and a live EURUSD quote, and then for a full draft of the brief, which my section allows.

**What it got wrong:** Before I shared my scenario email, it gave me a generic euro example and a placeholder filename (eur-receivable) instead of my real slug (ship-propulsion). It also chose the hypothesis (forward over put) in its draft, which is a judgment call that should be mine.

**How I caught it:** I pulled up my scenario email and the course scenarios.md file and compared them to the advice. GitHub also flagged a whitespace warning and I spotted a stray "x" in the docs path before committing.

**What I'd do differently:** Share the scenario email and the stage page at the start, and decide my own hypothesis before asking for a draft. I also want to re-check the dollar math on my own calculator before relying on it.

AI use: Claude (Anthropic) did the arithmetic and drafted the brief; I reviewed and edited it.
