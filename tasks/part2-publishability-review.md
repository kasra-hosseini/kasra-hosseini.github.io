# Agent Fabric Part 2: Publishability Review

Reviewer pass: deep read of Part 1 + Part 2, web-verified citations, cross-consistency check.
Date: 2026-06-25. Focus: Part 2 (with Part 1 fixes noted where they affect Part 2).

## THIRD PASS: five-persona rhetoric/flow/coherence review (2026-07-24)

Prior passes checked citation existence and claim-vs-content. This pass ran five
personas (skeptical ML researcher, general reader, copy editor, series architect,
adversarial critic) over rhetoric, internal consistency, flow, and cross-post
coherence. Both posts kept draft: true (not published). Anatomy reviewed first
(3 rounds, converged), then Part 2 (1 round + verification). Fixes applied,
grouped by commit:

Anatomy: scoped RE-Bench to "long, open-ended research and engineering tasks";
A-Lab "reported synthesising"; dropped filler "really"; "primitives such as".

Part 2 (commits 8222033, 8130c59, 57ce997, 2253b32):
- CONFIRMED CONTRADICTION FIXED: Autocracy rated "drift resistance: low" in its
  card but cited as drift-resistant near the fidelity passage. Rewrote to list
  Doctrine/Zero-Trust/Constitutional Republic as drift-resistant; Autocracy as
  offering little protection (hub drift cascades).
- Softened "the mechanism runs one way" -> "tends to run one way... can be reset/
  kept stateless to break the ratchet" (4 personas flagged as inevitability-law).
- "measurement gaming is inevitable" -> "hard to avoid".
- CAS "not speculative" -> honest substrate-boundary scope (Holland/Kauffman are
  bio/computational; LLM self-organization is empirical).
- Immune system "maps directly"/"most sophisticated in nature" -> scoped analogy.
- Myerson-Satterthwaite scoped to bilateral trade (was overgeneralized).
- Ostrom: dropped "Nobel Prize backing" as argument, "the natural model" -> "a
  lens not a proof"; noted her preconditions do not auto-transfer.
- Sortition: added heterogeneity precondition (identical model instances give no
  diversity / no anti-gaming value).
- Legitimacy axis: added structural-vs-normative scoping note (agents can't consent).
- Fidelity math: front-loaded the "illustration not measurement" caveat + noted
  alignment isn't a single scalar.
- Constitutional Republic: "most robust" -> "among the most robust"; added the
  enforcement caveat (judiciary is a property of the runtime, not the agents).
- Specialist market: signpost sentence framing it as the federation-not-
  consolidation bet.
- Coase/Hayek/Ostrom adjectives de-attributed (our summary, not their claims).
- Hirschman: acknowledged Fork-for-Loyalty substitution.
- Liquid Democracy mislabel in personal-agent scenario -> "emergent Meritocracy".
- Colony efficiency: unified two conflicting explanations.
- "billion-agent scale" -> anchored to Part 1's scale.
- "Adaptation Loop" link label -> "Adaptive Fabric" (matches Part 1 heading).
- Terminology bridge: augmented foundation model = Anatomy's "model + scaffolding".
- Dedup seams: society definition, delegation/governance contrast, "operational
  data hardens" pull-quote now read as callbacks.
- Copy-edit consistency: subject-verb, archetype capitalization, judgment/behavior/
  favor spelling, "The Agora" in tables, italic emphasis vs all-caps, six-class
  trap list matched, heading title-case, "just" filler, L742 fragment.

Build clean (exit 0), 0 em dashes, 0 prose double-dashes, 86/86 details balanced.
Still draft: true. Not yet done: Part 2 Round 2 verification of edited passages
(regression check in flight).

## STATUS: all findings fixed (2026-06-25)

All BLOCKING/MAJOR/MINOR rows below were applied to Part 1 and Part 2. Key
correction discovered during fixing: **"AI Agent Traps" IS a real paper**
(SSRN 6372438, Franklin/Tomašev/Jacobs/Leibo/Osindero, Google DeepMind, 2025) —
the arxiv-only search missed it. It is a taxonomy of *adversarial attacks on
agents* (content injection, semantic manipulation, cognitive-state, behavioural
control, systemic, human-in-the-loop traps), NOT a structural design-anti-pattern
taxonomy. So it was mis-cited for the anti-patterns table; it is now cited
correctly in the Adversarial Dynamics section with the SSRN link.

Other resolved facts:
- "Intelligent AI Delegation" = Feb 2026 (Tomašev, Franklin, Osindero); the
  "six dimensions" framing was not in the paper — replaced with the paper's real
  framing (authority/responsibility/accountability/specifications/intent/trust).
- The "multiplicative degradation" claim is the authors' own reasoning, now
  presented as such (the paper raises it only qualitatively as "accountability
  vacuum").
- Christiano 1810.08575 real title "Supervising strong learners by amplifying
  weak experts" — fixed in both posts.
- Hooker SSRN 5877662 = "On the slow death of scaling"; the Llama-3 8B / Aya 8B
  vs Falcon 180B / BLOOM 176B (4.5% params) claims VERIFIED verbatim from the PDF.
- METR: real figure is ~7-month doubling (~2-2.5x/yr), not 10x/yr — fixed in P1.
- Senate → Agora renamed throughout Part 1 viz (JS verified still balanced).

## SECOND PASS: claim-vs-content audit (2026-06-26)

The first pass checked that citations exist and are attributed correctly. This
pass checked whether the SPECIFIC CLAIM each post makes about a source actually
matches that source's content (read the abstract/findings, not just the title).
Covered all ~122 citations across both posts. Findings + fixes:

PART 1:
- Feng et al. "Heterogeneous Swarms": claimed "18.5% over single-model baselines"
  — actually vs 15 role/weight (multi-agent) baselines. Reworded qualitatively
  (no number, "outperforms a range of role- and weight-based baselines"). Real
  arxiv is 2502.04510; the NeurIPS proceedings link in the post is valid.
- Prokopenko et al. 2009: claimed it "showed self-organization can be formalized
  as information compression" — it's a conceptual primer, not a formalization.
  Reworded to "survey how ... can be given information-theoretic readings."
- AlphaChip: "superhuman" → "superhuman or comparable", "months" → "weeks"
  (matches DeepMind's own wording).
- May 1972: dropped imprecise "(connectance times interaction strength)"; the
  real instability quantity is sigma*sqrt(S*C). Now qualitative.
- Milo et al. 2002: tightened — each network class has its OWN characteristic
  motifs, and some networks fall into shared motif superfamilies (not "all share
  the same families"). VERIFIED myself: the agent's "factual inversion" call was
  overstated; the original was defensible, fix is precision only.
- VERIFIED-OK content claims: AI Scientist (<$15/paper, exact), Voyager (skill
  library + transfer, exact), DSPy ("significantly outperforms" justified),
  Kaplan, Hooker (diminishing-returns thesis correct), METR (7-month doubling,
  exact), alignment-faking, o1 system card ("instrumentally faked alignment",
  exact), Guardian (post's hedge matches article), Foundation Protocol (Liu 2026),
  Bo et al. PEFT-as-persistent-state, UCP, Lawrence ~2000 bits/min, Fontana&Buss,
  Vila/Greenstadt/Molnar lemons market, evolutionary model merging.

PART 2:
- McMahan 2016: "keyboard next-word prediction" example isn't in the 2016 FedAvg
  paper (it's the later applied work). Reworded to "text entry and speech ...
  model updates rather than raw data."
- CoALA: was credited with the "model+memory+tools+planning" decomposition; CoALA
  actually uses memory+action-space+decision-cycle. Recredited it specifically
  for the 4-part memory taxonomy (which IS CoALA's).
- Multi-agent risk (Hammond et al.): "miscoordinate, conflict, collude, or amplify
  destabilizing dynamics" implied 4 failure modes; paper has 3 modes + risk
  factors. Reworded.
- TRINITY: "learnable routing parameters" → "learns how to delegate" (it uses an
  evolutionary delegation strategy, not gradient-learned routing params).
- VERIFIED-OK content claims: Magentic-One (lead orchestrator tracks/replans,
  exact), More Agents (harder-task scaling, exact), Mixture-of-Agents (beats GPT-4o
  on AlpacaEval, exact), DyLAN (dynamic team selection beats static, exact),
  MetaGPT (roles beat chat-based, exact), HuggingGPT (controller pipeline, exact),
  AB-MCTS, Agora "Communication Trilemma" (exact phrase), protocol survey,
  Hinton distillation, strategic-collusion (Cournot monopolization, exact),
  Toolformer, AgentDojo, Anthropic Building Effective Agents (both claims exact),
  Irving 2018 (link added + verified). FrugalGPT and DSPy "metric threshold"
  phrasings left as-is (defensible, qualitative).

Net: no remaining factual inversions or fabricated claims in either post. All
precise numbers that were fragile have been made qualitative per user preference.

## Verdict

Part 2 is close to publishable and genuinely strong: the prose is clear, the
"architecture to institution" argument is original and well-built, the
honesty boxes ("Established vs. hypothesized") are exemplary. **It is NOT
publishable as-is because of a cluster of citation problems around Tomasev et
al. and Christiano that are factually false and independently verifiable.**
Fix the BLOCKING rows below and it is ready.

## The one thing to fix first

The post leans on Tomasev et al. as its main external grounding (~13 mentions)
and gets the load-bearing facts wrong. All four claims below were verified
against arxiv directly:

- **"AI Agent Traps" (Tomasev et al. 2025) does not exist.** The arxiv API
  returns zero results for that title; Tomasev's only three agent papers are
  *Intelligent AI Delegation*, *Virtual Agent Economies*, *Distributional AGI
  Safety*. The anti-patterns table is attributed to a paper that cannot be
  cited.
- **"Tomasev et al. prove alignment degradation is multiplicative with
  delegation depth"** (stated 4x). The paper makes no such proof; it discusses
  long delegation chains qualitatively. The multiplicative-fidelity math is the
  authors' own (correct) thought experiment and must be presented as such.
- **The "six dimensions" (observability, competence uncertainty, value
  alignment, reversibility, time pressure, scope)** are not the paper's
  framework. The paper uses different axes (authority/responsibility/
  accountability/specifications/intent/trust).
- **"Intelligent AI Delegation" is February 2026, not 2025.**
- **Christiano et al. (1810.08575) is titled "Supervising strong learners by
  amplifying weak experts," not "Iterated Distillation and Amplification,"**
  and it does not formalize multiplicative fidelity loss.

## Detailed findings table

Severity: BLOCKING (false / will embarrass on publication), MAJOR (misleading or
weakens the argument), MINOR (polish).

| # | Sev | Location | Issue | Suggested fix |
|---|-----|----------|-------|---------------|
| 1 | BLOCKING | L799, L1306 | "AI Agent Traps" Tomasev et al. 2025 cited as source for anti-patterns. Paper does not exist on arxiv. | Remove the citation. Attribute the anti-pattern taxonomy to your own synthesis (it is good and original). If a real Tomasev trap-related paper exists, cite that with its true title + ID. |
| 2 | BLOCKING | L536, L560, L949, L1258 | "Tomasev et al. prove alignment degradation is multiplicative with depth." Paper does not prove this. | Reframe as the authors' own argument: "multiplicative fidelity loss is a natural consequence of chained delegation; Tomasev et al. (2026) raise the qualitative concern of accountability vacuums in long chains." Keep the math as your illustration. |
| 3 | BLOCKING | L442, L1306 | Six dimensions misattributed to "Intelligent AI Delegation." | Replace with the paper's actual framework, or drop the specific six-dimension list and cite the paper's general thesis. |
| 4 | BLOCKING | L1258 | Christiano paper title wrong + claim it "formalized multiplicative fidelity loss." | Use the real title ("Supervising strong learners by amplifying weak experts" / Iterated Amplification). Remove the "formalized fidelity loss" claim; cite it only as context for iterative-training concerns. |
| 5 | MAJOR | All Tomasev cites | "Intelligent AI Delegation" dated 2025; it is Feb 2026. | Change to 2026; the other two Tomasev papers (Virtual Agent Economies 2025, Distributional AGI Safety 2025) keep their year. |
| 6 | BLOCKING (consistency) | P2 L335 vs P1 throughout | Core claim box asserts the delegation→governance transition is a "structural inevitability." Part 1 deliberately avoided "inevitable" (the intended framing was "strong attractor"). The body of P2 (L904) actually hedges this correctly ("today it is mostly latent... once infrastructure arrives, this becomes structural"). The box overclaims relative to both the body and Part 1. | Soften the box to match the body: "a strong structural tendency, not a design choice" / "any system that remembers performance and acts on it begins to govern." Keep the sharp claim; drop the absolute. This also closes the Part 1/Part 2 register gap. |
| 7 | BLOCKING (consistency) | P2 L178 | P2 says "Part 1 distinguished deployer-designed and user-assembled societies" with the claim that the former tend to hierarchy/doctrine, latter to markets/custodianship. Part 1 makes no such named distinction or claim. | Either add this distinction to Part 1 (it is a good one), or reword P2 to introduce it fresh: "We can distinguish two kinds of societies..." rather than attributing it to Part 1. |
| 8 | MAJOR | P1 viz code `arch:'Senate'` vs P2 "The Agora" | Part 1's governance visualization labels the deliberative archetype "Senate"; Part 2's taxonomy calls it "The Agora." Readers of both see a mismatch. | Rename Part 1's archetype label to "The Agora" (the zone is already named that). One-word fix in the SVG/JS data. |
| 9 | MAJOR | P1 viz (10 archetypes) vs P2 (22) | Part 1 illustrates ~10 governance archetypes with no stated total; P2 opens with 22, more than double. No "illustrative sample" signal in Part 1. (Note: the old memory says "12 archetypes" — that is stale; reality is P2 has 22.) | In Part 1's governance section/caption, add "ten illustrative examples; Part 2 develops the full set." Manages expectations and removes the apparent jump. |
| 10 | MAJOR | L1205 | "OpenAI Five" grouped with Stanford Smallville / AI Town as LLM self-organizing agents. OpenAI Five is RL self-play (Dota 2), not LLM-based. | Drop OpenAI Five, or label the group "RL self-play (OpenAI Five) and LLM social simulations (Smallville, AI Town)." |
| 11 | MAJOR | P2 L178 vs P1 L288 | P2's summary of Observation 2 drops Part 1's co-evolution hedge ("bounds shift as societies reshape their environment"). Hardens scarcity into a flat claim that then justifies governance urgency. | Restore the qualifier in one clause: "resources are finite (though the bounds shift as societies adapt their environment)." |
| 12 | MINOR | L599 | "More Agents Is All You Need... scaling near-linearly." The paper shows roughly logarithmic / diminishing returns. | Change to "improving with ensemble size, especially on harder tasks." Drop "near-linearly." |
| 13 | MINOR | L1281 | Hirschman *Exit, Voice, and Loyalty* described as "framework for organizational decline." It is about member responses to quality deterioration. | "...framework for how members respond to organizational deterioration." |
| 14 | MINOR | P1 L256 / P2 L953 | Woolley et al. 2010 (human lab groups) applied to AI agent societies without flagging the extrapolation. Same sentence appears near-verbatim in both posts. | Add "(observed in human groups; an open question for agents)" once, and vary the wording between the two posts to avoid the copy-paste look. |
| 15 | MINOR | P2 L337, L1258 | The 99%/200-gen → 13% math is arithmetically correct (verified: 0.99^200 = 0.134, 0.999^50 = 0.951, 0.99^50 = 0.605). No change needed to numbers — only to the attribution (rows 2, 4). | Keep numbers; fix attribution only. |

## Structural / editorial notes (not blocking, worth considering)

- **The core-claim box does two jobs and the second is weaker.** The second
  paragraph ("Why urgency matters... 13% of the original alignment") front-loads
  the contested multiplicative claim into the most prominent box on the page.
  Once row 2 is fixed, consider moving the urgency math out of the headline box
  and into the Adversarial Dynamics section where it already appears, so the box
  states the clean structural claim and nothing contestable.
- **Scale of the taxonomy vs. the body-word-limit goal.** 43 delegation + 22
  governance archetypes is encyclopedic (good for the "definitive reference"
  ambition) but the body still has to carry a reader past two giant figures and
  three scenario tables. The spine prose is well under control; the risk is the
  page *feels* like a catalog. The "What This Framework Is For" section is the
  payload and is excellent — consider signposting it earlier (a one-line "if you
  read one section, read the last one" is too cute, but the intro could promise
  the three diagnostic questions explicitly).
- **"From architecture to institution" is the best part of the post** and the
  genuinely novel contribution. The four worked examples (coding fleet, research
  synthesis, support fleet, personal network) are the strongest pages. This is
  the thesis; make sure the title/subtitle and intro point at it, not just at
  "delegation + governance archetypes."
- **Two near-verbatim repeats across posts:** the Woolley sentence (row 14) and
  the orchestrator-worker "easier to build/debug" line appears twice within
  Part 2 (L1021, governance Autocracy card). Trim the second.
- **Citations that check out (verified):** Anthropic Building Effective Agents,
  CoALA, Toolformer, Wei CoT, ReAct, ToT, Reflexion, FrugalGPT, AgentDojo,
  Hammond et al. multi-agent risk, strategic-collusion paper, North 1990,
  Barnard 1938 Zone of Indifference, Bainbridge 1983, Holland 1995, Kauffman
  1993, McMahan 2016, Hinton 2015, Kaplan 2020, Irving et al. 2018 (Debate),
  Sakana AI Scientist, Contract Net (Smith 1980), MoA, DyLAN, MetaGPT, AutoGen,
  CAMEL, Magentic-One, HuggingGPT, Agora protocol, TRINITY. These are solid.

## Part 1 changes recommended (to serve Part 2)

1. Rename "Senate" → "The Agora" in Part 1's governance + world visualizations (row 8).
2. Add "ten illustrative examples; the full set is in Part 2" to Part 1's governance caption (row 9).
3. Either add the deployer-designed / user-assembled distinction to Part 1, or stop attributing it to Part 1 in Part 2 (row 7).
4. Optional: align the three-claims box register with the more-hedged Loom Hypothesis section so Part 2's softened "strong tendency" framing is consistent end to end.
