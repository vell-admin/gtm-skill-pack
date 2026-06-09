# GTM Skill Pack — MCP Server Scope (Foundry "tools" tier)

**Status:** scope / design doc. Not built yet.
**Why:** the free tier ships the *knowledge* (the six skills as markdown). The paid tier ships the *capability* — the same skills as **live tools** your model runs, via one remote MCP connector URL. Charge for capability/access, not static content. That is the agentic-commerce thesis, dog-fooded.

---

## 1. What it is

A remote **MCP server** (Streamable HTTP transport) that exposes the GTM skills as callable tools. A buyer adds one URL as a connector in Claude, ChatGPT, or Copilot Studio and the tools appear — no copy-paste, no Project setup. Lowest friction, highest value.

## 2. Tools (v1 — start with three)

| Tool | Input (JSON) | Output |
|---|---|---|
| `listing_audit` | product, ICP, current title/short/long, pricing model, competitors | scorecard (5 dims) + rewritten title/short/long + keyword list + fix-first punch list |
| `cosell_draft` | product, customer/opp, recipient, stage, the ask | subject + ≤120-word message + ACE summary block + 5-day follow-up |
| `pricing_story` | what you sell, current pricing, buyer/budget owner, metering basis, deal goals | value metric + 3-tier package table + pricing narrative + agent-buyer note + private-offer rec |

Each tool's `description` + JSON schema mirrors the matching `SKILL.md` so behavior is identical to the free skill — the difference is it *runs* instead of instructing. Add `agent_ready_audit`, `demandgen_kit`, `discovery_prep` in v2.

## 3. Thin vs. thick — pick the execution model

- **Thin (prompt server):** the tool assembles the expert prompt + the caller's inputs and returns it; the *calling* model does the reasoning. Zero inference cost, no model key, trivially cheap to host. Good free-but-gated tier.
- **Thick (inference server):** the server runs the skill against Bedrock/Claude and returns the finished artifact. Costs inference → **this is what justifies metering/payment** and guarantees consistent output regardless of the caller's model.

**Recommendation:** ship **thick** for the paid tier (it's the product), keep a **thin** fallback for a free "preview" call (e.g., 3 calls/day) so people feel it before paying.

## 4. Auth + gating

- Bearer token on the MCP endpoint; one key per Foundry member (issue on offer purchase, revoke on churn).
- Mirror the existing hardened pattern (QBO + Kajabi MCP are auth-enforced; verify from localhost not external; pm2 delete+start to reload env — see [[reference_kajabi_itsrondavis_embed]] / the QBO MCP exposure notes).
- Free preview tier = unauthenticated but rate-limited + watermarked output ("upgrade for the full run").

## 5. Metering / payment (Ron's rails)

- **v1:** flat Foundry membership unlocks the key (simplest; no per-call accounting).
- **v2:** meter per tool call — Stripe metered billing, or **AgentCore Pay / x402** for true pay-per-call when the *caller is itself an agent*. This is the headline dog-food: an agent discovers the tool, pays per call, and transacts without a human. Ties to [[project_billing_rails_2026_05]] + the x402/AgentCore intel in [[reference_agentic_market_x402_buyer]].

## 6. Hosting

- Reuse the existing MCP gateway stack. Candidate URL: `gtm-mcp.vell.ai` or `mcp.itsrondavis.com` (cert via DNS-01/Route53 — the working pattern, avoids the :80 LE lock-out, per the mcp.vell.ai renewal fix).
- Streamable HTTP (remote MCP) so Claude/ChatGPT connectors accept it directly; `mcp-remote` stdio bridge only if a client needs it.
- Catalog it where agents look (MCP Market / directories) — practicing "get found" for the tool itself.

## 7. Build plan

1. **Scaffold** the MCP server (TypeScript SDK), 3 tools, thin mode, JSON schemas from the SKILL.md files.
2. **Wire thick mode** to Bedrock (Claude) behind a flag; structured outputs per tool.
3. **Auth**: bearer key + Foundry-purchase issuance; rate-limited free preview.
4. **Host** at the chosen subdomain (DNS-01 cert); smoke-test from a real connector in Claude + ChatGPT.
5. **Meter** (v2): Stripe metered → then x402 / AgentCore Pay for agent-paid calls.
6. **Wire into the funnel**: add the connector URL to the on-site builder's Claude/ChatGPT output and to the Foundry unlock; the README's tier-3 row goes live.

## 8. Open decisions for Ron

- Subdomain: `gtm-mcp.vell.ai` vs `mcp.itsrondavis.com` (brand under the personal hub?).
- Free-preview generosity (calls/day) before the paywall.
- v1 flat Foundry unlock vs. jump straight to metered.
- Which 3 tools first (recommend listing_audit + cosell_draft + pricing_story — highest intent, most demo-able on a call).

---
© 2026 Ron Davis · From listed to bought.
