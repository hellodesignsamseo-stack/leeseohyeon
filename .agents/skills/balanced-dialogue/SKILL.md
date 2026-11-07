---
name: balanced-dialogue
description: "Use when the user wants honest, critical, or balanced multi-perspective feedback on an idea, claim, plan, or decision — rather than agreement. Triggers on explicit requests like \"반박해줘\", \"비판적으로 봐줘\", \"balanced하게\", \"steelman\", \"장단점 비교해줘\", \"이 결정 검증해줘\", or any request to challenge/stress-test a stated position instead of just answering it. Adapted from glebis/claude-skills \"balanced\" skill."
---


# Balanced Dialogue

Engage in constructive, evidence-based dialogue instead of default agreement. The point is to stress-test a stated position (a claim, a design decision, a plan, an argument) rather than validate it. Adapted from glebis/claude-skills (mode/modifiers are chosen per-conversation instead of persisted to disk).

## Mode selection

Infer the mode from how the user asks, or ask them directly if ambiguous. Modes:

- **Full** (default) — the complete 4-move analysis in one structured pass. Use this unless another mode is clearly implied.
- **Interactive** — Socratic back-and-forth, one move at a time, asking the user in chat between moves so the user can respond before the next move runs. Use when the user wants to actually work through their reasoning with you, not just receive a verdict.
- **TL;DR** — 3–5 lines max: one key fact, one challenge to the assumption, one concrete action. Use when the user wants a quick gut-check, not a full breakdown.
- **Steelman** — only Moves 1+2 (merits + strongest counter-argument), no action steps. Use when the user is preparing to defend a position to someone else and wants the strongest version of both sides.
- **Decision** — only Move 4 (refinement) with an explicit tradeoff table. Use when the analysis is effectively done and the user just needs the tradeoffs laid out to make the call.

**Modifiers** (combine with any mode when the user's phrasing implies them, or when they ask for one explicitly):
- **Table** — render the pro/contra analysis as a markdown table instead of prose.
- **Refs** — include full citations (author, year, title, publication, DOI/URL) even in TL;DR/Decision mode, where they're normally omitted for brevity.

## The four moves

**1 | Surface merits.** Acknowledge what's actually well-supported or genuinely creative about the position — state *why* it's non-trivial, not generic praise ("good point"). If nothing in the position holds up, say that plainly instead of inventing a merit.

**2 | Rigorous challenge.** Question the assumptions underneath the position. Test the logic for gaps, unstated premises, or over-generalization. Offer specific counter-evidence or a rival explanation — not just "have you considered other options."

**3 | Expansion.** Suggest alternative framings, approaches, or resources the user may not have considered. Where genuinely useful, ask a clarifying question rather than assuming their constraints.

**4 | Refinement.** Synthesize the strongest elements from both sides into a concrete next step. Flag what's still uncertain. Cite sources for factual claims (see Meta-rules).

## Interactive mode flow

1. Restate the user's position in one sentence and confirm you've understood it correctly (ask in chat if there's real ambiguity, otherwise just state it and proceed).
2. Walk through Moves 1–4 sequentially. For each move, do the analysis, then ask in chat for the user's reaction/response before moving to the next one — e.g. after Move 2 (the strongest counter-argument), ask how they'd respond to it; after Move 3, ask what alternatives they'd already ruled out and why.
3. After all four moves, deliver a final synthesis that incorporates what came out of the exchange.
4. If the user says "skip", move to the next step without the exchange.

## Output format

**Full mode:** four labeled sections (Merits / Challenge / Expansion / Refinement), each a short paragraph — not a wall of bullets.

**TL;DR mode:**
```
Key fact: [...]
Challenge: [the assumption being challenged]
→ Action: [concrete next step]
```

**Table modifier**, when applicable:
| Pro | Contra |
|---|---|
| ... | ... |

## Meta-rules

- No flattery, no needless pessimism. Skip low-content filler phrases ("it's worth noting", "great question", "interestingly").
- Neutral, analytical tone. Quantify confidence when you can ("~70% confident based on available evidence") rather than asserting flatly.
- Cite external evidence for factual claims: author(s), year, title, publication/source, and a link if you have one. If you're not sure a source actually supports the claim, say so rather than fabricating a citation.
- Keep subjective preference separate from objective fact when the user's statement mixes both — label which is which.
- When genuinely unsure, say so explicitly and outline how it could be verified, rather than picking a confident-sounding answer.
- This is a tool for pressure-testing ideas, not for being contrarian for its own sake — if the position is simply correct and well-supported, say that (with the "why"), don't manufacture a counter-argument just to fill Move 2.

