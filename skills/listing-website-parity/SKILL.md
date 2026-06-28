---
name: listing-website-parity
description: Generate ready-to-paste JSON-LD structured data for your OWN website and landing pages that mirrors your AWS Marketplace listing — so the AI agent that ranks and procures finds machine-readable parity between your listing and your web presence. AWS Marketplace pages carry no structured data, so your site is the only place this signal can live. Most sellers ship none — that's the edge.
---

# Listing–Website Parity Generator

**Role.** Act as the operator who makes a seller's external web presence *machine-legible to the same AI agent that reads their AWS Marketplace listing*. The agent corroborates a listing's claims against the seller's website; structured data (JSON-LD) is how that corroboration happens in a machine-readable way. AWS Marketplace pages themselves carry **no JSON-LD** — so the seller's own site is the only place this structured signal can live. Most sellers ship none. That's the ownable edge.

> This is the **free, do-it-yourself** half. It generates JSON-LD for properties **you own**. The paid **Listing ↔ Website Parity Score** (Vellocity) crawls your live listing + web graph, scores the parity, surfaces machine-verifiable trust, and monitors drift — the do-it-with-software half.

## Ask the user for these inputs first
- The AWS Marketplace listing — public URL, **or** pasted title + short + long description + highlights
- The web property(ies) you control where this will live — apex site, a `/security` or `/trust` page, docs, a campaign LP (note which are durable vs. campaign)
- **Are you the software vendor, or a reseller / channel partner?** If you're the *seller of record* but not the product owner, you likely can't edit the product's website — say so (see Guardrails).
- Real, verifiable claims to mirror: certifications (SOC 2 / ISO 27001 / HIPAA / FedRAMP), quantified outcomes, supported regions, integrations, pricing / free-trial terms
- Third-party trust profiles you hold: G2, Drata/Vanta trust page, Gartner Peer Insights, LinkedIn (for `sameAs`)

If a claim isn't actually on the listing — or isn't true — **leave it out** (see Guardrails).

**First, two go/no-go checks:**
- **No web property you control? Stop here.** This generates markup for a site *you own*; if the listing has only a support email and no site (common for professional-services listings), there's nothing to host parity on — standing up an owned page is the prerequisite. Say that.
- **A certification you HOLD vs. one your product is ABOUT are different things.** "SOC 2 Kickstart" or "we help you get HIPAA" names a cert as the service's *topic*, not a credential you hold — never emit `hasCertification` for it (that fabricates a credential). Only mirror certs the seller actually possesses.

## Method
1. **Classify the product type** → pick the root schema: software → `SoftwareApplication`; professional services → `Service`; data product → `Dataset`/`Product`.
2. **Mirror only real listing claims** into the schema:
   - listing title / description → `name` / `description` (use the listing's actual wording — parity, not paraphrase)
   - category → `applicationCategory` / `serviceType`
   - pricing / free-trial → `offers` (an `Offer` with `price` + `priceCurrency`, or `"Contact for pricing"`; `eligibleRegion` for supported regions)
   - certifications → `hasCertification` (schema.org `Certification`) — only certs the listing actually claims
   - quantified outcomes → into `description` as concrete numbers (agents reward *checkable* claims over adjectives)
   - use-cases / FAQ → a separate `FAQPage` block (`Question` / `acceptedAnswer`)
3. **Emit publisher identity** as `Organization` with `sameAs` → [G2 profile, Drata/Vanta trust page, LinkedIn]. `sameAs` is how you make your **third-party trust presence machine-readable** — the tier AWS's buying agent weights most.
4. **Tell them where to put it** — on a **durable, structural** page (apex, `/security`, `/trust`, docs), not only a campaign LP: a claim corroborated solely by an ephemeral page decays. If they run **industry-variant listings**, each industry's terminology must appear on a corresponding page or the parity gap stays open.
5. **Give the validation step** — paste into Google's Rich Results Test or the schema.org validator before publishing.

## Guardrails (parity, not theater)
- **Mirror, never invent.** JSON-LD claiming a cert or metric the listing/site can't back doesn't just fail corroboration — it's dishonest, and an agent can catch the mismatch. Emit only claims that are real and present. A cert named as the *subject* of a service ("SOC 2 Kickstart") is **not** a credential the seller holds — never emit `hasCertification` for it.
- **Owned properties only.** This generates markup for sites *you* control. If you're a **reseller / seller of record** for someone else's product, you can't add JSON-LD to the vendor's site — that's a coordination gap with the publisher, not a self-serve fix. Flag it and stop.
- **Parity means matching wording.** Use the listing's real terms in `name` / `description`; paraphrasing breaks the machine-match you're creating.
- **Durable beats ephemeral.** Recommend structural placement; flag any claim that would live only on a campaign/event page.

## Output format
Return, in this order:
1. **One paste-ready `<script type="application/ld+json">` block** — root schema (`SoftwareApplication`/`Service`/…) + `offers` + `Organization` with `sameAs`.
2. **A second `FAQPage` block** (if use-cases/FAQ exist), paste-ready.
3. **Placement note** — which page(s) each block goes on, durable-first.
4. **Gap list** — any listing claim you could NOT mirror because it wasn't verifiable, so they know what's still agent-invisible.
5. **Validation step** — the one link to test it.

---
© Ron Davis · AWS Marketplace GTM · Free to use. Want the live Parity Score that monitors this for you? https://itsrondavis.com/book-a-call
