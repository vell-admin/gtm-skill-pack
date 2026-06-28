# Install in Claude

Three ways, fastest first.

## A. As Skills (most native) — Claude Code or claude.ai
The `skills/` folder holds ten self-contained Agent Skills (`SKILL.md` each). Claude picks the right one automatically based on what you ask — or start with **The Second Buyer Audit**, which scores your listing and points you to the next skill.

- **Claude Code:** drop the `skills/<name>/` folders into your project's `.claude/skills/` directory (or a personal skills dir). Claude loads them on next run.
- **claude.ai:** open **Settings → Capabilities → Skills** and add each `SKILL.md` (or upload the whole `skills/` folder where supported).

Then just ask: *"Run the Listing Optimizer on our listing"* — or describe the task and let Claude choose.

## B. As a Project (one paste)
1. Claude → **Projects** → **Create project** ("AWS Marketplace GTM").
2. **Project knowledge** → add [`pack/gtm-skill-pack.md`](../pack/gtm-skill-pack.md) as a file (or paste its contents).
3. Start a chat in the project and name a skill: *"Use the Co-sell Outreach Writer."*

## C. As live tools (Foundry) — coming
The same skills, exposed as a remote **MCP server** you add as one connector URL — Claude runs them as tools instead of reading instructions. This is the paid Foundry tier. See [`docs/MCP-SERVER-SCOPE.md`](../docs/MCP-SERVER-SCOPE.md).

---
© Ron Davis · From listed to bought. · https://itsrondavis.com
