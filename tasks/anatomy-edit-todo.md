# Anatomy of an Agent: edit pass

Source: content/posts/2026/anatomy-of-an-agent.md
Figures: static/images/2026/anatomy-of-an-agent.svg (Fig 1), anatomy-delegation-styles.svg (Fig 2)

## Deep review pass (both posts, full end-to-end reads)
- Anatomy: read fully several times; clean. Cut one filler opener. Beautiful and to the point.
  - NOTE (not changed): "What the Loop Needs" precedes "The Loop" — slightly counterintuitive
    ordering but works via the "layers around a single loop" opener. Flagged, left as-is.
- Part 2: read full prose spine + all 43 delegation cards + governance intro/cards/closing.
  - Removed contested 99%/13% math from headline Core Claim box (kept in Adversarial Dynamics).
  - Reframed top-down/bottom-up governance distinction as Part 2's own (was misattributed to Part 1).
  - Fixed broken sentence in Chain card.
  - MAJOR: reordered all 43 delegation cards to match the nine-family summary table.
    Card layout had drifted (Voting/Critic/Debate were under Reliability; Knowledge Transfer
    divider held cards from 5 families). Done via deterministic script; verified 43 cards
    preserved, details balance 86/86, hugo builds clean. Removed now-stale "(Includes Relay/
    Handoff and Loop/Retry below.)" aside from Sequential divider.

## Decisions (from user + research)
- Scaffolding vs harness: NOT distinct/formalized in canonical sources (Anthropic BEA, Weng, smolagents all use neither prominently). Used loosely as synonyms. -> Lead with "scaffolding", simplify, DROP the "(sometimes called the harness)" aside since it's not really standard. Verify: yes, drop it.
- Codex IS a real current coding agent (CLI Apr 2025; comparable to Claude Code; merged into ChatGPT desktop Jul 2026). -> Add alongside Claude Code.
- GNoME/A-Lab exact numbers NOT verified (chemistry URLs blocked). -> Go qualitative, drop precise counts.
- Part 2 name stays "Division of Labour and Governance" -> just keep anatomy refs consistent with that.
- Anatomy 2nd half: light-touch collective-intelligence framing; subtitle unchanged.
- New citations (verified): OSWorld (2404.07972, 2024) OR tau-bench (2406.12045, 2024) for "agents underperform humans on realistic tasks"; Kamoi et al. 2024 (2406.01297) for self-correction-needs-external-feedback (keep Huang 2023 for "confidently wrong" mechanism).

## Todo (DONE unless noted)
1. [x] Subtitle already clean (no standalone "AI agent"); stripped standalone "AI" from description frontmatter.
2. [x] Fig 1 SVG: Action label moved up (y=-2/12 -> y=-13/0).
3. [x] Fig 1: trimmed SVG viewBox height 740->675 to remove dead white space above caption. (May tighten more after visual preview.)
4. [x] Opening paragraph added: who/why + agents = basic unit of Part 1 & 2.
5. [x] Claude -> ChatGPT for the model-call example.
6. [x] Codex added alongside Claude Code (verified real current coding agent).
7. [x] "difference is structure" -> "What turns the one into the other is architecture."
8. [x] Scaffolding simplified; harness parenthetical DROPPED (not standard per research).
9. [x] Added planning to the "without X" list.
10. [x] Bold-lead bullet colons removed (0 remaining); prose colons are legit.
11. [x] ReAct glossed "short for reason and act".
12. [x] "improves anything" -> "getting better" / "improve the work itself".
13. [x] Reframed as "recursive self-improvement"; called out why "recursive" matters.
14. [x] GNoME/A-Lab exact numbers dropped, now qualitative.
15. [x] Evaluation "automated or done by hand"; real-world testing "feeds back into the next round".
16. [x] WebArena(2023) -> OSWorld + tau-bench (2024); numbers dropped, qualitative.
17. [x] "counterintuitive" removed; toned to "tends to backfire".
18. [x] "the world" explicitly defined (signal outside the model's own judgement).
19. [x] Kamoi 2024 added; Huang 2023 kept for the "more confident" mechanism.
20. [x] Fig 2 caption + alt updated; emphasizes Part 2 maps the space with dozens of patterns in families.
21. [x] "Beyond the Single Loop" names consistent with Part 2 families.
22. [x] Collective-intelligence light-touch framing woven into Beyond + closing.
23. [x] Whole text rewritten for human flow; half-sentences and bullet rhythm removed.

## Verify (DONE)
- Body prose grew ~1,300 -> ~2,040 words (flow, not filler; under 3000-3500 ceiling). wordcount frontmatter updated to ~2,000. OPEN: user may want a tightening pass back toward ~1,500.
- Em dashes: 0. Double-dash punctuation: 0.
- Read end-to-end: flows.
