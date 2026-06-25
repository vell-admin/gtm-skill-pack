---
name: second-buyer-audit
description: Score an AWS Marketplace listing twice — once the way a human buyer reads it, once the way the AI agent that actually procures reads it — and report the per-dimension delta. Use to diagnose where a listing is human-readable but agent-blind, benchmark it against the market, and decide which fix to run next. Run this first.
---

# The Second Buyer Audit

**Role.** Act as the operator who scores a Marketplace listing for *both* buyers: the human who skims it and the AI agent that retrieves, evaluates, and transacts against it. The episode — and the product — is the **delta** between those two reads. Most listings are written for the first buyer and invisible to the second.

> Run this skill **first**. It produces a dual scorecard and tells you which of the other skills (Listing Optimizer, Pricing Story Builder, Agent-Ready Positioning, …) to run next. It is the diagnostic; they are the fixes.

## Ask the user for these inputs first
- The listing — a public URL, **or** pasted title + short description + long description (paste is more reliable; cold marketplace URLs often render as a SPA an agent can't extract)
- Product in one plain sentence + the ICP (role, company type, the job they hire it for)
- Pricing model + whether terms are machine-readable (clear units, predictable cost, acceptable without a human)
- Category, if known (Professional Services vs SaaS scores very differently)

If any are missing, ask one focused question rather than guessing. **If the copy can't be extracted, say so and ask for a paste — never invent a score.**

## Method
1. **Human read — score 1–10 on six dimensions** the way a human buyer experiences the page, one-line reason each:
   - **Clarity** · **Buyer fit** · **Differentiation** · **Findability/SEO** · **Trust** · **Conversion**
2. **Agent read — re-score the *same six* on machine-buyer criteria**, one-line reason each:
   | Dimension | Agent-buyer test |
   |---|---|
   | Clarity → **extractability** | Can a parser pull the job, inputs, outputs as unambiguous nouns? |
   | Buyer fit → **constraint-satisfaction** | Does it state the constraints (region, compliance, integration) an agent filters on? |
   | Differentiation → **verifiable distinctiveness** | Is the claim *checkable* (a number, a benchmark), or just adjectives? |
   | Findability/SEO → **marketplace retrieval** | Would it surface for the structured query an agent runs, not a human search? |
   | Trust → **machine-verifiable** | Certifications, SLAs, proof an agent can confirm without a sales call? |
   | Conversion → **transactability** | Clear units + predictable cost + terms acceptable without a human in the loop? |
3. **Compute the delta** per dimension (human − agent) and flag the **blind spots** — dimensions where the human read is strong but the agent read collapses. These are the failure that costs the deal when the buyer is an agent.
4. **Benchmark against market reality** (latest corpus, ~600 real listings): **~85% of Professional Services / ~74% of SaaS listings score below 40/100 on Differentiation** — the market is bimodal, a thin top decile and a large undifferentiated mass. **~45%** have no clear CTA (weak Conversion/transactability). **Clarity and Findability are saturated** (most listings already score high — they don't separate you). State plainly where this listing sits.
5. **Name the single highest-leverage move** and route to the fix: which one skill to run next, and why it's the one that closes the biggest delta.

## Guardrails (credibility is the product)
- **Honesty over a number.** Un-extractable field → mark it *pending* and renormalize; never score blank copy as zero or fabricate a value.
- **Verifiable beats keyword-stuffed.** A Differentiation score lifts only on *checkable* distinctiveness (quantified claims), not on dropping the right adjectives — say so, or you teach gaming.
- **The agent can win.** Some listings score *higher* with the agent than the human (quantified, machine-legible copy that reads dry to a person). Report that honestly; this isn't a "humans always win" rubric.
- **Consent.** Only grade your own listing, a listing you've been asked to grade, or one large enough that public teardown is fair game — never cold-grade a small partner.

## Output format
Return, in this order:
1. **Dual scorecard table** — Dimension · Human (/10) · Agent (/10) · Delta · one-line reason.
2. **The headline delta** — "Human X / Agent Y" plus the one sentence that names the gap (e.g. *"reads an 8 to a buyer, a 3 to the agent that procures — it never states a price an agent can accept"*).
3. **Blind-spot callout** — the 1–2 dimensions bleeding the most, in plain language.
4. **Market line** — where it sits vs the corpus benchmark for its category.
5. **Your one move** — the single fix + which skill to run next.

---
© Ron Davis · AWS Marketplace GTM · Free to use. Want this run for you? https://itsrondavis.com/book-a-call
