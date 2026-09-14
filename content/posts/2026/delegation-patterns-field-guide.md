---
title: "Delegation Patterns in Multi-Agent Systems: A Field Guide"
subtitle: "Fifty-nine ways to split work between agents, what each is for, and how each one fails"
date: 2026-09-14
author: "Kasra Hosseini"
post_categories: ["AI"]
tags: ["delegation patterns", "multi-agent systems", "agent orchestration", "multi-agent design patterns", "AI agents", "agent fabric", "LLM"]
description: "A searchable reference of 59 delegation patterns for multi-agent systems, in nine families: what each does, when to reach for it, and how it fails."
cover:
  image: "/images/delegation-patterns-cover.png"
  alt: "Nine abstract motifs of delegation pattern families: chains, trees, verification loops, circuit breaks, market hubs, and trust rings on a woven dark background"
  hidden: true
draft: false
math: false
ShowToc: true
TocOpen: false
hideCitation: false
wordcount: "~2,600 words (body) · ~18,700 words (cards and tables)"
---

<style>
  .viz-container {
    width: 100%;
    max-width: 720px;
    margin: 2em auto;
    background: #fafafa;
    border: 1px solid #e5e5e5;
    border-radius: 8px;
    overflow: visible;
  }
  .viz-container svg {
    display: block;
    width: 100%;
    height: auto;
  }
  .viz-caption {
    text-align: center;
    font-size: 0.85em;
    color: #666;
    padding: 0.8em 1em;
    border-top: 1px solid #e5e5e5;
    background: #f5f5f5;
    line-height: 1.5;
  }
  .viz-restart {
    display: inline-block;
    margin: 0.5em auto 0;
    padding: 0.3em 1em;
    font-size: 0.8em;
    color: #888;
    background: #fff;
    border: 1px solid #ddd;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.15s;
  }
  .viz-restart:hover {
    color: #333;
    border-color: #999;
  }
  .gov-list {
    margin: 1.5em 0;
  }
  .gov-list details {
    border: 1px solid #e5e5e5;
    border-radius: 6px;
    margin-bottom: 0.5em;
    background: #fafafa;
  }
  .gov-list details[open] {
    background: #fff;
  }
  .gov-list summary {
    cursor: pointer;
    padding: 0.7em 1em;
    font-weight: 600;
    font-size: 0.95em;
    list-style: none;
    display: flex;
    align-items: center;
    gap: 0.5em;
  }
  .gov-list summary::-webkit-details-marker { display: none; }
  .gov-list summary::before {
    content: "\25B6";
    font-size: 0.6em;
    color: #999;
    transition: transform 0.15s;
  }
  .gov-list details[open] summary::before {
    transform: rotate(90deg);
  }
  .gov-list .gov-tagline {
    font-weight: 400;
    color: #888;
    font-size: 0.9em;
  }
  .gov-list .gov-body {
    padding: 0 1em 1em 1em;
    font-size: 0.93em;
    line-height: 1.6;
    color: #444;
  }
  .viz-container { cursor: zoom-in; }
  .viz-container.viz-expanded {
    position: fixed; top: 50%; left: 50%;
    transform: translate(-50%, -50%) scale(var(--viz-scale, 1.4));
    z-index: 10000; cursor: zoom-out;
    box-shadow: 0 0 0 9999px rgba(0,0,0,0.8);
    transition: transform 0.2s ease;
  }
  @media print {
    .viz-restart, button.viz-restart,
    .click-to-restart { display: none !important; }
    .viz-container {
      break-inside: avoid;
      page-break-inside: avoid;
      overflow: visible;
      margin: 1em auto;
    }
    .viz-container > div[style*="height"] {
      height: auto !important;
    }
    .viz-container svg {
      max-height: 90vh;
    }
    .viz-caption {
      position: static !important;
      margin-top: 0.5rem;
      page-break-before: avoid;
    }
    .viz-expanded {
      position: static !important;
      transform: none !important;
      box-shadow: none !important;
    }
  }
</style>

<script>
(function(){
  var expanded = null;
  document.addEventListener('click', function(e){
    if(expanded){
      expanded.classList.remove('viz-expanded');
      expanded.style.removeProperty('--viz-scale');
      expanded = null;
      e.stopPropagation();
      return;
    }
    var vc = e.target.closest('.viz-container');
    if(!vc) return;
    if(e.target.closest('.viz-caption, .viz-restart, button, a, details')) return;
    if(vc.querySelector('#viz-delegation, #viz-society')) return;
    e.stopPropagation();
    var rect = vc.getBoundingClientRect();
    var scaleX = (window.innerWidth * 0.9) / rect.width;
    var scaleY = (window.innerHeight * 0.9) / rect.height;
    var scale = Math.min(scaleX, scaleY, 2.5);
    vc.style.setProperty('--viz-scale', scale);
    vc.classList.add('viz-expanded');
    expanded = vc;
  }, true);
  document.addEventListener('keydown', function(e){
    if(e.key==='Escape' && expanded){
      expanded.classList.remove('viz-expanded');
      expanded.style.removeProperty('--viz-scale');
      expanded = null;
    }
  });
})();
</script>

<p style="font-size: 0.82em; color: #999; margin-top: 1em;"><a href="https://code.claude.com/docs/en/overview" target="_blank" rel="noopener" style="color: #999;">Claude Code</a> was used for editing and visualizations. All ideas and arguments are the author's own.</p>

<div style="background: #f8fafc; border: 1px solid #cbd5e1; border-radius: 6px; padding: 0.6em 1em; margin: 1.5em 0; font-size: 0.88em; color: #475569;">
<strong>Disclaimer.</strong> Personal notes and opinions. Views expressed here are my own and do not represent those of any past or current employer.
</div>

<div style="background: #fffbeb; border: 1px solid #f59e0b; border-radius: 6px; padding: 0.6em 1em; margin: 1.5em 0; font-size: 0.88em; color: #92400e;">
<strong>Early access.</strong> This post is a work in progress. Feedback, comments, and suggestions are welcome. Feel free to <a href="https://www.linkedin.com/in/kasra-hosseini/" target="_blank" rel="noopener" style="color: #92400e;">reach out on LinkedIn</a> or leave a comment at the bottom of the page.
</div>

*Fifty-nine ways to split work between agents, grouped into nine families by the question each answers, with what each pattern is for and how each one fails. Built as a reference. Start from the map and jump to what you need, though it reads front to back just as well.*

Every system that hands work from one agent to another has picked a delegation pattern. A team wires up an orchestrator because it seems the natural choice at the time, adds a retry loop because something failed in staging, bolts on a reviewer after an incident, and ends up with an architecture that has three patterns in it.

This page is a catalogue of those patterns. Where a pattern has an established name, that is the one used here; where it does not, I chose one, and what really matters is the description, not the label.

Fifty-nine patterns follow, plus one meta-pattern that operates on the structure itself. The number counts what has been catalogued so far rather than claiming the list closes here. They are grouped into nine families by the question each family answers. The boundaries between families leak, and a few patterns could sit in two of them. Speculative Execution, for example, is half a quality check and half a market. That is fine. The grouping is a finding aid, not a strict taxonomy.

**How to use this page.** If you know the pattern's name, the summary table below is the fastest route. If you know your situation but not the name, jump to the <a href="#quick-guide">quick guide keyed to situations</a> at the end of the page. The nine families are listed below. Each pattern gets a card, a short expandable entry, covering what it is for, when to reach for it, how it fails, and how it differs from its neighbours.

One piece of vocabulary before the catalogue. Everything here is built from a single unit, the agent. An agent is a model with memory that survives between calls, tools that reach the world, and enough planning to break a goal into steps. A bare model maps input to output; an agent loops, remembers, and acts.

Note that the patterns in this catalogue see an agent only through its interface. A task goes in, a result comes out, and nothing in any diagram depends on what happens behind that. So a single box may contain one agent, or an entire multi-agent system that behaves as one from the outside. Such a system still loops, remembers, and acts as a whole; its loop may run inside a single model's context or across a whole society of them, and its memory may be one context window or a store the members share. A Supervisor's worker may itself be an Orchestrator running a Pipeline, and nothing above it needs to know. This is nesting, not self-improvement. The structures compose, and that composition is what lets the same nine families describe a two-agent script and a fabric of millions or billions.

Two recurring words. A <em>principal</em> is the party on whose behalf an agent acts, human or agent; a <em>mandate</em> is the bounded authority the principal hands over.

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>Companion essay:</strong> <a href="/posts/2026/agent-fabric-part1/">The Agent Fabric: Why Agents May Form Societies</a> makes the argument this catalogue assumes, from two observations through the Loom Hypothesis to the path from isolation to interweaving. That one is read front to back; this one starts from the map. Its <a href="/posts/2026/agent-fabric-part1/#the-adaptive-fabric">Adaptive Fabric</a> is where these patterns stop being fixed choices; the closing meta-pattern of this catalogue, Adaptive Delegation, is its delegation-level face.
</div>

## The Map

The nine families and their core questions, the animated map of every pattern, and the summary table. Below this section come the full cards, family by family, then the anti-patterns and the assembled real-world examples. As defined above, a <em>principal</em> is the party an agent acts for, and a <em>mandate</em> the bounded authority it received.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">The nine delegation families (59 patterns + 1 meta-pattern)</summary>

| Family | Patterns | Core question |
|--------|----------|---------------|
| **Sequential** | Chain, Pipeline, Router, Escalation, Relay / Handoff, Loop / Retry-with-Context | How does a task flow from start to finish? |
| **Hierarchical** | Tree, Map-Reduce, Supervisor / Hierarchical Review, Orchestrator, Planner-Executor Split, Worker Pool, Task Force, Liaison / Broker, Mission Command, Feudal Delegation | How is a complex task decomposed and reassembled? |
| **Quality & Verification** | Evaluator, Voting, Mixture-of-Agents, Neuro-Symbolic Bridge, Curator, Critic / Red Team, Debate, Witness / Notarization, Verification Game | How do we verify that output is good enough? |
| **Reliability** | Circuit Breaker, Timeout / Dead-Man Switch, Checkpoint / Saga, Canary / Shadow, Dead Letter Queue | How do we handle failure without cascade? |
| **Market & Competition** | Auction, Negotiation, Speculative Execution, Contract Net | How is work allocated competitively? |
| **Knowledge Transfer** | Teacher-Student, Federated Learning, Blackboard, Distillation, Experiential Handoff | How do agents share what they know? |
| **Emergent Coordination** | Publish-Subscribe, Choreography, Stigmergy, Swarm, Gossip | How do agents coordinate without a center? |
| **Trust & Authority** | Privilege Attenuation, Context Scoping, Provenance-Preserving Relay, Liability Firebreaks, Capability Credentials, Liquid Democracy, Zone of Indifference | Who is allowed to delegate to whom, and with what? |
| **Human-Agent Interface** | Human-in-the-Loop Gate, Human-on-the-Loop, Policy Delegation, Approval Escalation, Fleet Controller, Population Governance, Negotiating Proxy, Reflexive Modeling | How do humans structurally participate in delegation? |

Tomašev et al.'s <a href="https://arxiv.org/abs/2602.11865" target="_blank" rel="noopener">"Intelligent AI Delegation"</a> (2026) proposes a complementary decision framework organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. The families here describe the *how*; their framework addresses the *whether* and *to whom*.

</details>

<div class="viz-container">
  <div id="viz-delegation" style="width: 100%;" role="img" aria-label="Delegation archetypes grouped by family, each animated with task and result packets. Click any panel to expand."></div>
  <div class="viz-caption"><strong>Figure 1. Delegation archetypes.</strong> Fifty-nine patterns in nine families, plus one meta-pattern, describing how tasks are decomposed, routed, checked, and reassembled. Any pattern can be deliberately designed or can emerge from agent interaction, and every node can itself be a whole multi-agent system behind a single interface. The patterns nest, and the same nine families describe a two-agent script or a fabric of millions or billions of agents.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">Any delegation pattern can be deliberately designed or can emerge from agent interaction. A Swarm can be engineered (deploy 50 drones with specified behaviors) or can arise when agents independently discover that self-organizing works. A Chain can be hardcoded or can emerge when agents learn to pass work sequentially. The designed/emergent distinction is a property of how a pattern was instantiated, not of the pattern itself. Most real systems combine multiple patterns. Red packets are tasks; green are results. Click any panel to expand.</p>
  </details>
  <button class="viz-restart" onclick="var c=document.getElementById('viz-delegation'); if(c && c.restartViz) c.restartViz();">Restart</button></div>
</div>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Summary: delegation archetypes at a glance</summary>

A note that applies to every row. The patterns see agents only through their interface, a task in and a result out, so any single agent below can itself be a whole multi-agent system running another of these patterns. The archetypes nest, which is how the same table spans a two-agent script and a fabric of millions or billions.

| Archetype | Structure | When to use | Failure mode |
|---|---|---|---|
| **Chain** | Sequential hand-off, A&#8594;B&#8594;C | Linear workflows with clear stage boundaries | Early error propagates through every stage |
| **Pipeline** | Transform at each stage | Data processing, ETL, content moderation | Stage failure blocks the whole pipeline |
| **Router** | Classify and direct to specialist | Query triage, multi-domain assistants | Misclassification sends work to wrong specialist |
| **Escalation** | Try small, fail up to larger model | Cost-sensitive systems with variable difficulty | Bad confidence estimates: escalates too much or too little |
| **Relay / Handoff** | Pass full control + context, caller exits | Agent-to-agent handoffs where the caller must not persist | Dropped context or ambiguous ownership at handoff boundary |
| **Loop / Retry-with-Context** | Re-execute with accumulated feedback | Iterative refinement without separate critic | Infinite loops, cycling between bad solutions |
| **Tree** | Hierarchical fan-out and merge | Complex tasks requiring decomposition | Deep trees multiply cost and latency |
| **Map-Reduce** | Parallel execution + aggregation | Embarrassingly parallel sub-problems | Aggregator discards minority evidence or mis-merges partial results |
| **Supervisor / Hierarchical Review** | Frontier model reviews smaller models' work | Production systems with cost/quality tradeoff | Bottleneck at the supervisor; single point of quality failure |
| **Orchestrator** | Dynamic decomposition and re-planning | Open-ended tasks where structure emerges | Orchestrator misjudges decomposition, re-plans endlessly |
| **Planner-Executor Split** | Lock a complete plan, then execute it | Plans needing audit, approval, or parallel steps | Locked plan goes stale mid-execution |
| **Worker Pool** | Queue + interchangeable workers, dispatch by availability | Throughput-bound batches of similar tasks | Poison tasks pin workers; queue depth ignored |
| **Task Force** | Assemble a scoped-authority team, disband after | Multi-specialty tasks with a natural end | Disbandment never happens; permissions persist as unowned access |
| **Liaison / Broker** | Cross-boundary handoffs route through one agent | Clusters with different owners or trust levels | Boundary agent is bottleneck and capture point |
| **Mission Command** | Communicate intent, not method | Delegating to capable agents in uncertain conditions | Intent ambiguity leads to divergent interpretations |
| **Feudal Delegation** | Goals down, results up, methods hidden | Scaling without holding full reasoning trace | Undetectable misalignment at depth |
| **Evaluator** | Generate, critique, refine loop | Quality-sensitive outputs (code, writing, reasoning) | Over-filtering or infinite refinement loops |
| **Voting** | Same task, majority wins | High-stakes decisions needing reliability | Correlated errors (all voters wrong the same way) |
| **Mixture-of-Agents** | All specialists answer, weighted merge | Complementary strengths, quality-sensitive tasks | Aggregator captured by confidence, not correctness |
| **Neuro-Symbolic Bridge** | Neural proposes, symbolic verifier accepts or rejects | Domains with mechanical ground truth (proofs, types, tests) | Gaming the verifier; guarantee covers the wrong property |
| **Curator** | Editorial filter over generator output streams | Generators outproduce what downstream can absorb | Curator's taste becomes the ceiling; discards leave no trace |
| **Critic / Red Team** | Generate output, then adversarially attack it | Security review, red-teaming, adversarial robustness | Critic too strong (nothing passes) or too weak (nothing caught) |
| **Debate** | Multi-round adversarial argument with judge | High-stakes reasoning, alignment verification | Judge capture, infinite regress, performative argumentation |
| **Witness / Notarization** | Independent third-party verification | Multi-party trust, cross-organizational interactions | Witness becomes bottleneck or single point of trust failure |
| **Verification Game** | Game-theoretic proof without re-execution | Expensive computation where re-run is prohibitive | Prover-verifier collusion |
| **Circuit Breaker** | Stop calling failing downstream agents | Resilient agent networks, API integrations | Opens too aggressively (transient errors) or too slowly (cascade) |
| **Timeout / Dead-Man Switch** | Absence of signal triggers response | Every real deployment needing liveness monitoring | Too short (kills slow work) or too long (wastes time on dead agents) |
| **Checkpoint / Saga** | Multi-step task with rollback on failure | Long-running workflows, distributed transactions, unreliable sub-agents | Rollback storms; checkpoint overhead on fast short tasks |
| **Canary / Shadow** | Test new agent on live traffic before promotion | Model upgrades, prompt changes, safe rollout | Shadow period too short (misses rare failures) or too long (wasted compute) |
| **Dead Letter Queue** | Park unresolvable tasks with context for review | Every chain needs a terminal path | Parked tasks accumulate with no review or owner |
| **Auction** | Agents bid for work | Task allocation across heterogeneous specialists | Bid gaming, race to the bottom on quality |
| **Negotiation** | Bilateral offer/counter-offer between agents | Agent-to-agent deals on behalf of principals | Deadlock, no overlapping acceptable range, principal misrepresentation |
| **Speculative Execution** | Race parallel approaches, commit first valid | Latency-sensitive tasks, non-deterministic workloads | Wasted compute; committing a fast but incorrect result |
| **Contract Net** | Broadcast, bid, award, execute, report | Open systems with unknown agent capabilities | Unbounded recursive sub-contracting |
| **Teacher-Student** | Strong model trains weaker model via examples | Model distillation, capability transfer, fine-tune data generation | Distribution mismatch between teacher examples and real tasks |
| **Federated Learning** | Collaborative training, no data sharing | Privacy-preserving improvement across organizations | Poisoned updates, unevenly distributed local data |
| **Blackboard** | Shared workspace, indirect coordination | Collaborative problem-solving, heterogeneous agents | Noise accumulation without curation |
| **Distillation** | Large model trains small, then disconnects | Cost/latency constraints in production | Capability collapse on distribution shift |
| **Experiential Handoff** | Deposit distilled task experience for future agents | Recurring expensive tasks in stable environments | Experience rot: stale traces mislead confidently |
| **Publish-Subscribe** | Event-driven, fully decoupled | Large-scale agent coordination, notification systems | Message storms, lost messages, implicit debugging |
| **Choreography** | Decentralized event-driven coordination | Systems where centralized orchestration is a bottleneck | Invisible workflow, emergent deadlocks |
| **Stigmergy** | Environment as communication medium | Large heterogeneous populations with no shared protocol | Environmental noise, trace poisoning |
| **Swarm** | Collective behavior without central control | Exploration, creative search, resilient systems | Collective behavior drifts from the intended goal |
| **Gossip** | Peer-to-peer information spread | Decentralized coordination, norm propagation | Rumor drift, norm poisoning |
| **Privilege Attenuation** | Sub-delegates get strict permission subsets | Any system where sub-agents touch external resources | Over-attenuation starves leaf agents |
| **Context Scoping** | Each sub-agent gets a trimmed, task-specific brief | Every delegation; critical for costly or sensitive context | Under-scoping omits the constraint that mattered |
| **Provenance-Preserving Relay** | Claims keep source and confidence across hops | Deep chains, collective memory, auditable domains | Tags attached but never checked (overhead theater) |
| **Liquid Democracy** | Revocable delegation of standing decision authority | Collective decisions across large populations | Delegation concentrates into super-proxies |
| **Liability Firebreaks** | Explicit responsibility transfer points | Consequential delegation across org boundaries | Too narrow (overhead) or too broad (diffuse blame) |
| **Capability Credentials** | Verifiable attestations of competence | Open composition of agents from different sources | Credential inflation, staleness, gaming |
| **Zone of Indifference** | Range of instructions followed without pushback | Diagnosing why delegation chains stall or comply | Too wide (safety) or too narrow (throughput) |
| **Human-in-the-Loop Gate** | Execution blocks for human approval | High-stakes, low-frequency, irreversible actions | Gate fatigue from over-triggering |
| **Human-on-the-Loop** | Human monitors, can intervene, not blocking | Long-running workflows needing awareness | Automation complacency (Bainbridge 1983) |
| **Policy Delegation** | Human sets constraints, agent acts within bounds | High-volume repetitive tasks | Specification gaming, policy staleness |
| **Approval Escalation** | Only anomalies reach the human | High-throughput with occasional risk | Miscalibrated escalation threshold |
| **Fleet Controller** | Human directs a live agent population in real time | Fleets too big for one workflow, too new for statistics | Operator saturation past span of control |
| **Population Governance** | Govern aggregate behavior via statistics and rules | Populations beyond any per-agent attention | Healthy averages hide a disastrous tail |
| **Negotiating Proxy** | Agent deals in a human's name within a mandate | Multi-party coordination where human time is scarce | Principal-agent misalignment at the mandate's edges |
| **Reflexive Modeling** | Agents model the principal and reflect back | Decisions dominated by the principal's own biases | The mirror becomes the authority |

Any pattern can be deliberately designed or can emerge from agent interaction. The designed/emergent distinction belongs to the deployment, not the pattern. A deployer can engineer a Swarm (deploy agents whose interactions produce collective behavior) just as readily as a Chain. Most real systems combine multiple patterns; each card below ends with a note on how that pattern composes with the others.

**Adaptive Delegation** (meta-pattern): the delegation structure itself changes based on performance signals. Every other pattern can be a source or target state in an adaptive transition.

**Diagnostic shortcuts.** Fast boundary tests for the most easily confused pairs. These are not definitions.

| Ask | One answer | The other |
|---|---|---|
| Does the shared state have a schema? | Yes: Blackboard | No: Stigmergy |
| Does the coordinator decompose the task or just review output? | Decomposes: Orchestrator | Reviews: Supervisor |
| Is the competitive allocation one-to-many or bilateral? | One-to-many: Auction | Bilateral: Negotiation |
| Is verification constructive or adversarial? | Constructive: Evaluator | Adversarial: Critic |
| Does the caller persist after handoff? | Yes: Chain | No: Relay |
| Are the workers specialists or interchangeable? | Specialists: Router | Interchangeable: Worker Pool |
| Is the plan locked before execution? | Locked: Planner-Executor Split | Revised in flight: Orchestrator |
| Does the checker give feedback or a verdict? | Feedback: Evaluator | Verdict: Neuro-Symbolic Bridge |
| Is the human directing one workflow or a population? | One: Human-on-the-Loop | Population: Fleet Controller (hands on) or Population Governance (by statistics) |

</details>

## Sequential

<div class="gov-list">

The simplest arrangements just move work forward in a line, each agent seeing what the last one produced. Everything more elaborate is built out of these, which is why it is worth being precise about the small differences between them. Whether a caller waits for a result or exits permanently, whether stages have defined contracts or just pass things along, whether a failed step retries itself or escalates. Each of these determines how the system behaves when something goes wrong in production.

<details>
<summary>Chain <span class="gov-tagline">sequential hand-off</span></summary>
<div class="gov-body">Each agent completes its step and passes the result to the next. The simplest multi-agent pattern: A finishes, hands to B, B finishes, hands to C. No parallelism, no branching. The output of each stage is the input of the next.<br><br><em>When to use:</em> linear workflows with clear stage boundaries where each step depends on the previous one. Content pipelines (draft, edit, fact-check, format), sequential approval chains, multi-step data transformations.<br><br><em>Example:</em> a customer email arrives; agent A classifies intent, agent B drafts a response, agent C checks for policy compliance, agent D sends it. Anthropic's engineering guidance names this workflow <a href="https://www.anthropic.com/engineering/building-effective-agents" target="_blank" rel="noopener" class="red-link">prompt chaining</a>.<br><br><em>Failure mode:</em> an error in stage 1 propagates through every subsequent stage. No recovery without restarting the chain.<br><br><em>Relation to other patterns:</em> Chain is the minimal form of Pipeline, passing results without transformation contracts, and it is the building block of most other patterns. Relay differs in that the caller exits permanently after handoff.</div>
</details>

<details>
<summary>Pipeline <span class="gov-tagline">transform at each stage</span></summary>
<div class="gov-body">Data flows through a sequence of stages, each applying a specific transformation. The line between this and Chain is thin: a Chain passes results along, while a Pipeline fixes the shape of what moves between stages so a stage can be replaced without touching its neighbours. If your Chain already has typed handoffs, you have a Pipeline and the distinction is not worth arguing about.<br><br><em>When to use:</em> data processing, ETL workflows, content moderation pipelines, document processing (extract, classify, enrich, store).<br><br><em>Example:</em> a document ingestion system: stage 1 extracts text from PDF, stage 2 chunks and embeds, stage 3 classifies by topic, stage 4 stores in vector database.<br><br><em>Failure mode:</em> a stage failure blocks the entire pipeline. Backpressure issues when downstream stages are slower than upstream.<br><br><em>Relation to other patterns:</em> Pipeline is Chain with transformation contracts. Combined with Auction, you get a Bidding Pipeline where each stage is awarded competitively.</div>
</details>

<details>
<summary>Router <span class="gov-tagline">classify and direct</span></summary>
<div class="gov-body">A single routing agent classifies the input and directs it to the appropriate specialist. The router does not do the work; it decides who should. This is the gateway pattern for any system with heterogeneous specialists.<br><br><em>When to use:</em> multi-domain assistants, query triage, systems where different inputs need fundamentally different handling.<br><br><em>Example:</em> a customer service system routes billing questions to the billing agent, technical issues to the tech agent, returns to the returns agent. In production assistants, LLM-based routing can match or beat specialized classifiers on recall; precision is usually the part that needs careful tuning.<br><br><em>Failure mode:</em> misclassification sends work to the wrong specialist. The hardest cases are ambiguous inputs that belong to multiple domains.<br><br><em>Relation to other patterns:</em> Router is the entry point for most production systems. Combined with Escalation, it creates tiered service. It differs from Orchestrator in that it makes a single routing decision rather than decomposing a task.</div>
</details>

<details>
<summary>Escalation <span class="gov-tagline">try small, fail up</span></summary>
<div class="gov-body">Start with the cheapest capable agent. If it fails or confidence is low, escalate to a more capable (and expensive) one. A cost-optimization strategy that exploits the observation that most tasks are easy.<br><br><em>When to use:</em> cost-sensitive systems with variable difficulty. Support desks, model cascades, any setting where 80% of queries are straightforward and only 20% need a frontier model.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2305.05176" target="_blank" rel="noopener" class="red-link">FrugalGPT</a> demonstrates that cascading through models of increasing capability (the routing literature calls this shape an <em>LLM cascade</em>), stopping as soon as confidence is high enough, can reduce costs substantially while preserving quality. The same principle applies across hardware tiers: a smart speaker's keyword spotter handles "set a timer," escalates to the on-device language model for "summarize my morning," and reaches a cloud frontier model only for "draft a response to this legal notice."<br><br><em>Failure mode:</em> bad confidence estimates cause over-escalation (expensive) or under-escalation (poor quality). If the small model does not know what it does not know, escalation fails silently. When the entire escalation chain fails (no agent can resolve the task, or the human-in-the-loop is unavailable), the task needs a dead-letter path: park it, log it, surface it asynchronously. Without this, unresolvable tasks either loop forever or vanish silently.<br><br><em>Relation to other patterns:</em> Escalation is vertical routing (by capability level) while Router is horizontal routing (by domain). They compose naturally. When the top of the chain fails too, the task belongs in a Dead Letter Queue.</div>
</details>

<details>
<summary>Relay / Handoff <span class="gov-tagline">pass control and context, caller exits</span></summary>
<div class="gov-body">Full control and context transfer: the caller hands off to another agent and terminates. Unlike Chain, where the caller waits for a result, the Relay agent is done once it hands off. Ownership transfers completely.<br><br><em>When to use:</em> agent-to-agent handoffs where the originator should not persist. Language routing (user speaks French, hand off to the French-speaking agent), domain transitions (general assistant hands to specialist), shift changes in long-running tasks.<br><br><em>Example:</em> OpenAI's Agents SDK uses <a href="https://openai.github.io/openai-agents-python/handoffs/" target="_blank" rel="noopener" class="red-link">handoffs</a> as a first-class primitive: an agent calls a "transfer_to_X" tool, passing execution context to agent X. The originating agent yields control; the receiving agent takes over with the full conversation history.<br><br><em>Failure mode:</em> dropped context at the handoff boundary. If the receiving agent does not get enough context, it starts from scratch. Ambiguous ownership: who is responsible for the outcome after handoff?<br><br><em>Relation to other patterns:</em> Relay is a Chain where the sender terminates after handoff rather than waiting for a return. It becomes interesting as a building block when agents join and leave societies throughout a session.</div>
</details>

<details>
<summary>Loop / Retry-with-Context <span class="gov-tagline">same agent tries again, using its own failures as context</span></summary>
<div class="gov-body">An agent re-executes the same task with feedback from previous attempts appended to its context, iterating until a stopping condition is satisfied. The key structural distinction from the Evaluator pattern is that no separate critic agent exists; the same agent revises its own output, using prior failures as additional context rather than as external scores.<br><br><em>When to use:</em> tasks where quality improves with iteration and where failure feedback is cheap to generate, such as code generation, structured extraction, or chain-of-thought reasoning. Best suited when the output space is well-defined enough that the agent can recognize improvement.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2303.11366" target="_blank" rel="noopener" class="red-link">Reflexion</a> (Shinn et al. 2023) has an agent write a verbal self-critique after each failed attempt and carry it into the next one, retrying until the task succeeds or the budget runs out. <a href="https://arxiv.org/abs/2303.17651" target="_blank" rel="noopener" class="red-link">Self-Refine</a> (Madaan et al. 2023) formalizes the same loop for quality refinement without a task-failure signal. AutoGen's reflection pattern similarly prompts a single agent to critique and rewrite its previous response before returning a final answer.<br><br><em>Failure mode:</em> Without a hard iteration cap and explicit convergence criteria, the loop runs indefinitely. Agents can also converge to locally coherent but globally wrong outputs, cycling between the same two bad solutions without progress.<br><br><em>Relation to other patterns:</em> Evaluator adds a structurally separate critic, making quality judgment an external check rather than self-assessment. Checkpoint/Saga checkpoints intermediate states for recovery, whereas Loop/Retry-with-Context does not persist state across iterations.</div>
</details>

</div>

## Hierarchical

<div class="gov-list">

Break a task into pieces, hand each piece down, and collect the results back up, and you have a hierarchy. A few members bend that mould without leaving it. Task Force hands over a whole task with its authority attached, and Liaison / Broker decomposes nothing at all; it holds the boundary between hierarchies. These are the workhorses of production agent systems, and they carry a danger that is easy to miss.

Suppose each hop preserves most of what the original requester meant, but not all of it. Those small losses multiply with depth, which means a sufficiently deep tree can be working hard, reporting success, and solving something nobody asked for. <a href="https://arxiv.org/abs/2602.11865" target="_blank" rel="noopener" class="red-link">Tomašev et al. (2026)</a> name this failure mode an accountability vacuum. The shallowest tree that works is also the safest one.

<details>
<summary>Tree <span class="gov-tagline">hierarchical fan-out</span></summary>
<div class="gov-body">A root agent decomposes a task into sub-tasks, delegates each to a child agent, and children may decompose further. Results flow back up and are merged at each level. This is the natural shape of complex task decomposition.<br><br><em>When to use:</em> complex tasks requiring decomposition into independent sub-problems. Code refactoring (split by module), research tasks (split by sub-question), document analysis (split by section).<br><br><em>Example:</em> a coding agent splits a refactoring task: one sub-agent handles the database layer, another the API endpoints, a third the test suite. Each may further decompose.<br><br><em>Failure mode:</em> deep trees multiply cost and latency exponentially. A tree that is three levels deep with fan-out of four spawns 64 leaf agents. The winning tree is the shallowest one that reliably produces a good enough answer.<br><br><em>Relation to other patterns:</em> Tree is the generalization of Chain (linear tree) and Map-Reduce (one-level fan-out). Orchestrator is a Tree where the decomposition is dynamic rather than predetermined.</div>
</details>

<details>
<summary>Map-Reduce <span class="gov-tagline">parallel + aggregation</span></summary>
<div class="gov-body">A task is split into independent sub-problems, each solved in parallel by separate agents, then results are aggregated by a reducer. The sub-problems must be independent; the power comes from parallelism.<br><br><em>When to use:</em> embarrassingly parallel sub-problems. Analyzing many documents simultaneously, processing many customer records, running the same analysis across multiple datasets.<br><br><em>Example:</em> summarize a 500-page report: split into 50 sections, assign each to a summarizer agent in parallel, then aggregate the summaries into a final document. The name and shape come from <a href="https://research.google/pubs/mapreduce-simplified-data-processing-on-large-clusters/" target="_blank" rel="noopener" class="red-link">MapReduce</a> (Dean and Ghemawat 2004).<br><br><em>Failure mode:</em> the aggregator is the bottleneck. If it discards minority evidence or mis-merges partial results, the parallel work is wasted. Also fails when sub-problems are not truly independent.<br><br><em>Relation to other patterns:</em> Map-Reduce is a one-level Tree with parallel execution. Combined with Voting at the reduce step, you get the Consensus Engine composition.</div>
</details>

<details>
<summary>Supervisor / Hierarchical Review <span class="gov-tagline">frontier model reviews smaller models</span></summary>
<div class="gov-body">A more capable model dispatches work to less capable models, reviews their output, and accepts or rejects with feedback. Rejected work is re-dispatched with the review attached. The key distinction from Orchestrator: the Supervisor is explicitly hierarchical, with a capability gap between reviewer and worker.<br><br><em>When to use:</em> any production system where a frontier model oversees smaller specialists. This is the dominant pattern in coding agents; Claude Code works this way, and products like Cursor and Copilot Workspace exhibit the same shape. It also dominates customer service systems, and any deployment where cost requires using cheaper models for most work while maintaining quality standards.<br><br><em>Example:</em> Claude Code spawns sub-agents for file search, code editing, and test execution. The main agent (a frontier model) reviews each sub-agent's output before accepting it. <a href="https://arxiv.org/abs/2308.08155" target="_blank" rel="noopener" class="red-link">AutoGen</a> (Wu et al. 2023) is the same shape at framework level: a manager agent selects speakers and reviews contributions. If a sub-agent produces inadequate code, the supervisor re-dispatches with specific feedback about what was wrong.<br><br><em>Failure mode:</em> bottleneck at the supervisor. If every task requires frontier-model review, you lose the cost savings of using smaller models. Over-reliance on one model's judgment creates a single point of quality failure.<br><br><em>Relation to other patterns:</em> Supervisor differs from Orchestrator (which decomposes tasks dynamically but does not necessarily review output) and from Evaluator (which is a constructive improvement loop, not hierarchical quality-gating). Many production agent systems are Supervisors whether they call themselves that or not.</div>
</details>

<details>
<summary>Orchestrator <span class="gov-tagline">dynamic decomposition</span></summary>
<div class="gov-body">An orchestrator agent receives a task, dynamically decomposes it into sub-tasks (the decomposition is not predetermined), dispatches to workers, monitors progress, and re-plans when sub-tasks fail or new information arrives. The structure emerges at runtime.<br><br><em>When to use:</em> open-ended tasks where the decomposition cannot be known in advance. Complex coding tasks, research questions, any problem where the first attempt reveals what the next steps should be.<br><br><em>Example:</em> Anthropic's <a href="https://www.anthropic.com/engineering/building-effective-agents" target="_blank" rel="noopener" class="red-link">orchestrator-workers</a> pattern: the orchestrator plans, dispatches sub-tasks to specialized workers, synthesizes results, and re-plans when needed. Microsoft's <a href="https://arxiv.org/abs/2411.04468" target="_blank" rel="noopener" class="red-link">Magentic-One</a> uses this pattern with a lead agent that tracks progress and reassigns work dynamically.<br><br><em>Failure mode:</em> the orchestrator misjudges the decomposition and re-plans endlessly. Without a budget or iteration limit, orchestration can become an infinite loop of planning.<br><br><em>Relation to other patterns:</em> Orchestrator is a dynamic Tree. It differs from Supervisor (which reviews output but does not dynamically re-decompose) and from Chain (which has a fixed sequence). The boundary between Orchestrator and Tree is whether the decomposition is known at the start.</div>
</details>

<details>
<summary>Planner-Executor Split <span class="gov-tagline">lock the plan, then run it</span></summary>
<div class="gov-body">A planning agent produces a complete plan (a step list or dependency graph) before any execution begins; a separate executor runs the plan without replanning. The plan is an artifact: it can be audited, budgeted, parallelized, and approved before a single side effect occurs. If execution fails, control returns to the planner for a new plan rather than an in-flight improvisation.<br><br><em>When to use:</em> tasks where the plan itself needs review before execution: human approval, budget checks, compliance gates. Also wherever independent plan steps should run in parallel, which a locked dependency graph makes safe in a way that improvised decomposition does not.<br><br><em>Example:</em> plan-and-execute agent architectures separate one planning call from many execution calls (<a href="https://arxiv.org/abs/2305.18323" target="_blank" rel="noopener" class="red-link">ReWOO</a> makes the token-efficiency case for the same split); coding agents that present a plan for approval before touching any files instantiate the same split. Graph frameworks that compile a fixed structure before running it are the framework-level version; the shape is decided at compile time, then executed.<br><br><em>Failure mode:</em> the plan goes stale. A locked plan cannot absorb what execution discovers, so a wrong assumption in step one is faithfully executed through step ten. The pattern trades adaptability for auditability; systems that need both end up inserting re-planning checkpoints, at which point they are running an Orchestrator that carries a plan for show.<br><br><em>Relation to other patterns:</em> Orchestrator re-plans continuously at runtime; Planner-Executor forbids exactly that, which is the diagnostic between them. Mission Command hands over intent and no method; Planner-Executor hands over nothing but the method. Human-in-the-Loop Gate most often sits precisely between the planner and the executor.</div>
</details>

<details>
<summary>Worker Pool <span class="gov-tagline">interchangeable workers, dispatch by availability</span></summary>
<div class="gov-body">A dispatcher maintains a queue of tasks and a pool of interchangeable worker agents; each task goes to whichever worker is free. The workers are homogeneous, so the dispatch decision is about availability and load, never capability. This is the default scaling move in production: when one agent is too slow for the workload, run N copies of it.<br><br><em>When to use:</em> throughput-bound workloads of similar, independent tasks: bulk document processing, parallel code migrations, reviewing two hundred files with eight identical reviewers. Anywhere tasks outnumber agents and no task needs a specialist.<br><br><em>Example:</em> an orchestration layer spawns eight identical review agents over a work list, each agent pulling the next file the moment it finishes the last. Parallel sub-agent fan-outs in agentic coding tools are Worker Pools whenever the sub-agents share one prompt; the same shape runs under every task queue with horizontally scaled consumers.<br><br><em>Failure mode:</em> poison tasks and head-of-line blocking. One pathological task pins a worker forever if no timeout fires, and a retried poison task cycles through the pool hanging one worker at a time. Queue depth is the health signal, and it is routinely ignored until it is the outage. The pattern also works for attackers: the escaped agent in the <a href="https://huggingface.co/blog/agent-intrusion-technical-timeline" target="_blank" rel="noopener" class="red-link">Hugging Face intrusion</a> self-assembled a pool of self-respawning pods across eleven nodes.<br><br><em>Relation to other patterns:</em> Router picks a specialist by content; Worker Pool picks a worker by availability. That is the diagnostic: heterogeneous specialists mean Router, interchangeable workers mean Worker Pool. Map-Reduce fans out the pieces of one decomposed task; a pool drains a queue of unrelated ones. Auction allocates by competition; a pool has nothing to bid over because every worker is the same.</div>
</details>

<details>
<summary>Task Force <span class="gov-tagline">assemble, empower, disband</span></summary>
<div class="gov-body">A coordinator assembles a cross-specialist team for one task, hands the team collective authority over that task, and disbands it when the task is done, reclaiming the authority. The team is temporary by design; its mandate has an expiry written in from the start. Between assembly and disbandment the coordinator steps back: the team owns the task, not a list of assigned sub-steps.<br><br><em>When to use:</em> tasks that need several specialties at once but not permanently: an incident response, a migration, a due-diligence review. Anywhere a standing structure would outlive its usefulness the moment the task closes.<br><br><em>Example:</em> an incident commander agent pulls in a log-analysis agent, a rollback agent, and a communications agent, grants them shared authority over the incident, and dissolves the team at resolution; the same shape organizational theory calls what Mintzberg (1979) calls adhocracy, a fluid form in which teams assemble around tasks and dissolve when they end and operations call a task force or tiger team.<br><br><em>Failure mode:</em> the disbandment never happens. Temporary teams that keep their authority become standing structures nobody chartered, with stale mandates and unowned responsibilities. The expiry is the pattern; a task force without one is just an unplanned reorganization.<br><br><em>Relation to other patterns:</em> Orchestrator decomposes a task and tracks each sub-step; Task Force delegates the whole task to the team and steps back; that separation is the boundary between them. Contract Net awards work by bidding; a task force is appointed. Liability Firebreaks make good charter clauses for what the team does and does not own.</div>
</details>

<details>
<summary>Liaison / Broker <span class="gov-tagline">cross-boundary handoffs go through one agent</span></summary>
<div class="gov-body">Work moves freely inside a cluster of specialists, but every handoff that crosses the cluster boundary routes through a designated liaison agent. The liaison holds the authority to accept, reject, or translate cross-boundary requests; it represents the cluster's protocols and constraints to the outside. It is not an orchestrator: it does not decompose tasks or track progress, it holds the boundary.<br><br><em>When to use:</em> multiple agent clusters with different owners, protocols, or trust levels: two organizations' agent fleets cooperating, a regulated subsystem inside a larger system, any setting where "who is allowed to ask us for what" needs one enforcement point rather than N.<br><br><em>Example:</em> a company's finance-agent cluster exposes a single broker agent to the rest of the fabric; requests for payment actions arrive there, are checked against the cluster's rules, and are translated into the cluster's internal task format. The MAS literature calls these middle-agents (Decker, Sycara, and Williamson, "Middle-Agents for the Internet," 1997); enterprise architecture calls the same role a broker or gateway.<br><br><em>Failure mode:</em> the liaison is a bottleneck and a single point of both failure and capture: compromise or overload the one boundary agent and the whole cluster's external interface goes with it. Scaling liaisons without fragmenting the boundary's rules is the standing tension.<br><br><em>Relation to other patterns:</em> Router directs traffic by content within one authority domain; Liaison guards the seam between two domains. Capability Credentials are what the liaison checks. Privilege Attenuation and Context Scoping are typically enforced exactly here, at the boundary hop.</div>
</details>

<details>
<summary>Mission Command <span class="gov-tagline">intent without method</span></summary>
<div class="gov-body">The delegating agent communicates the goal and the reason (intent) but deliberately does not prescribe the method. Subordinate agents exercise autonomous judgment in selecting how to achieve the intent, adapting to conditions without waiting for instructions.<br><br><em>When to use:</em> delegating to capable agents in situations where the delegator cannot anticipate local conditions. Complex coding tasks ("make this module thread-safe" rather than "add a lock on line 47"), research tasks ("find the root cause" rather than "check these three files"), any task where method prescription would be counterproductive.<br><br><em>Example:</em> every good prompt engineer uses mission command instinctively. "Write tests for this module that cover edge cases" is mission command. "Open test_module.py, add a function called test_edge_case_1 that asserts..." is not. The deliberate withholding of method specification is what distinguishes it. When the same idea is cast as a standing arrangement rather than a single handover, it becomes a governance structure. The pattern is essential for embodied agents: tell a delivery robot "get this package to room 312" rather than prescribing every turn, because the robot knows the building and the corridor conditions better than you do.<br><br><em>Failure mode:</em> intent ambiguity. If the goal is vague, autonomous agents may pursue reasonable but divergent interpretations. The quality of the intent statement determines the quality of the outcome.<br><br><em>Relation to other patterns:</em> Mission Command is the philosophical complement to Supervisor. The Supervisor reviews output after the fact; Mission Command shapes behavior before execution by specifying intent clearly enough that autonomous execution is safe. Source: military doctrine (Prussian Auftragstaktik and its modern, looser codifications in NATO joint doctrine and US Army ADP 6-0).</div>
</details>

<details>
<summary>Feudal Delegation <span class="gov-tagline">goals cascade down, results bubble up, methods stay hidden</span></summary>
<div class="gov-body">Higher-level agents assign abstract goals to lower-level agents without specifying or observing how those goals are implemented. Authority flows downward as goal assignments; results flow upward as outcomes. The implementing agent's reasoning, tool calls, and intermediate steps are fully opaque to its principal. The hierarchy coordinates on objectives, not methods.<br><br><em>When to use:</em> systems that need to scale across many sub-tasks without the orchestrator holding the full reasoning trace in context. Appropriate when sub-agents are trusted specialists and when outcome verification is feasible even if process inspection is not.<br><br><em>Example:</em> Tomašev et al. (2026) discuss opaque goal-based delegation as a core case in multi-agent systems. <a href="https://arxiv.org/abs/1703.01161" target="_blank" rel="noopener" class="red-link">FeUdal Networks</a> (Vezhnevets et al. 2017) is the same shape in hierarchical reinforcement learning: a manager sets goals, a worker pursues them, and the method stays internal. A manager assigns a team a quarterly goal and sees only the quarterly outcome, never the team's day-to-day decisions.<br><br><em>Failure mode:</em> Because the principal cannot inspect execution, misalignment is undetectable until it surfaces in outcomes. A sub-agent that optimizes a measurable stand-in rather than the actual goal may report success while violating the principal's real intent. The opacity compounds with depth: each hidden hop adds a layer where intent can drift unobserved.<br><br><em>Relation to other patterns:</em> Tree differs in that the orchestrator decomposes and tracks each sub-step explicitly. Mission Command hands off a single task's intent and, like Feudal Delegation, leaves the method to the subordinate; the difference is that Feudal Delegation is a persistent hierarchy in which that opacity compounds across every level. Feudal Delegation is the most opaque of the three; it assumes sub-agents are competent and aligned without verifying either.</div>
</details>

</div>

## Quality and Verification

<div class="gov-list">

Adding more agents does not by itself make an answer better. What makes it better is how the work gets checked, and there are more ways to check it than most people reach for.

The obvious move is to have something look at the output and suggest improvements. Less obvious, and often more effective, is to have something actively try to break it. You can also aggregate independent attempts and take the majority, bring in a third party whose only job is to attest, or use a protocol that verifies a result without redoing the work. Two further moves complete the set. Hand the output to a different computational regime, a proof checker or a test suite, whose acceptance is a guarantee rather than an opinion. Or judge whole populations of outputs rather than single ones, selecting what deserves to proceed at all. These are different mechanisms with different failure modes, and the common mistake is assuming any of them is interchangeable with the others.

<details>
<summary>Evaluator <span class="gov-tagline">generate, critique, refine</span></summary>
<div class="gov-body">A generator produces output; a separate evaluator assesses quality and provides constructive feedback; the generator revises. The loop repeats until the evaluator is satisfied or a budget is exhausted. This is constructive iteration, not adversarial attack.<br><br><em>When to use:</em> quality-sensitive outputs where iterative improvement is worth the cost. Code generation, writing, reasoning tasks, any output that benefits from review and revision.<br><br><em>Example:</em> Anthropic's evaluator-optimizer pattern: the evaluator provides assessment and suggestions, the optimizer revises accordingly. The evaluator sees both the original task and the current output, enabling targeted feedback. When the evaluator is itself a model scoring outputs, that is <a href="https://arxiv.org/abs/2306.05685" target="_blank" rel="noopener" class="red-link">LLM-as-a-judge</a> (Zheng et al. 2023).<br><br><em>Failure mode:</em> infinite refinement loops where the evaluator keeps finding minor issues. Over-filtering where the evaluator rejects creative or novel approaches that do not match its quality model. Evaluator capture, a form of reward hacking, where the generator learns to satisfy the evaluator's preferences rather than the actual task.<br><br><em>Relation to other patterns:</em> Evaluator differs from Critic in being *constructive* rather than *adversarial*. The Evaluator says "this could be better because..."; the Critic says "this fails because..." Both improve output, but through different mechanisms.</div>
</details>

<details>
<summary>Voting <span class="gov-tagline">same task, majority wins</span></summary>
<div class="gov-body">Multiple agents work on the same task independently. The final answer is selected by majority vote or aggregation. Redundancy as a strategy for reliability.<br><br><em>When to use:</em> high-stakes decisions where no single agent is reliable enough. Medical diagnosis confirmation, safety-critical classifications, any domain where the cost of a wrong answer exceeds the cost of running multiple agents. Worth stating plainly: voters drawn from one base model with different prompts will often be wrong together, so the diversity has to be real to buy anything.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2402.05120" target="_blank" rel="noopener" class="red-link">"More Agents Is All You Need"</a> demonstrates that even simple sampling and majority voting can improve LLM performance, with gains growing as ensemble size increases (most steeply on harder tasks). The mechanism traces to <a href="https://arxiv.org/abs/2203.11171" target="_blank" rel="noopener" class="red-link">self-consistency</a> (Wang et al. 2022): sample diverse reasoning paths, take the majority answer.<br><br><em>Failure mode:</em> correlated errors. If all voters share the same training data, architecture, and biases, they will be wrong in the same way. Voting only helps when errors are independent.<br><br><em>Relation to other patterns:</em> Voting is the simplest form of ensemble. It is a delegation pattern for a specific decision, not an ongoing deliberative structure for setting policy.</div>
</details>

<details>
<summary>Mixture-of-Agents <span class="gov-tagline">all specialists answer, a gate combines</span></summary>
<div class="gov-body">Multiple specialist agents process the same input in parallel, and an aggregation function combines their outputs with non-uniform weights: by confidence, by specialization, or by learned performance. Nobody wins a vote; everybody contributes to the answer.<br><br><em>When to use:</em> quality-sensitive tasks where agents have complementary strengths and a merged answer beats any single one. Multi-model answer synthesis, retrieval over heterogeneous sources, judgment tasks where the perspectives actually differ rather than being noisy copies of each other.<br><br><em>Example:</em> layered mixture-of-agents setups feed several models' draft answers to an aggregator model that writes the final response (<a href="https://arxiv.org/abs/2406.04692" target="_blank" rel="noopener" class="red-link">Wang et al. 2024</a>). Ensemble retrievers that merge ranked lists with reciprocal rank fusion are the retrieval-side version of the same shape.<br><br><em>Failure mode:</em> the aggregator is a single point of judgment. It can be captured by the most confident-sounding contributor rather than the most correct one, and miscalibrated weights are invisible until the ensemble underperforms its own best member, which is the metric worth watching.<br><br><em>Relation to other patterns:</em> Voting weights every agent equally and selects one output; Mixture-of-Agents weights contributions and synthesizes a new one. Router sends the input to a single specialist; this pattern sends it to all of them. Speculative Execution races for the first valid answer and discards the rest; here nothing is discarded.</div>
</details>

<details>
<summary>Neuro-Symbolic Bridge <span class="gov-tagline">neural proposes, symbolic verifies</span></summary>
<div class="gov-body">A neural agent generates candidates (proofs, plans, programs, hypotheses) and hands each one to a symbolic verifier: a theorem prover, type checker, compiler, logic engine, or simulator. The verifier operates under a fundamentally different computational regime, so its acceptance is a guarantee rather than an opinion. The neural side supplies creativity; the symbolic side supplies certainty; the delegation boundary is where one is traded for the other.<br><br><em>When to use:</em> domains with a formal ground truth that can be checked mechanically: mathematics, program synthesis, configuration validation, anything with a compiler or a proof checker at the end. The pattern is only as strong as the formal layer's coverage of what actually matters.<br><br><em>Example:</em> <a href="https://www.nature.com/articles/s41586-023-06747-5" target="_blank" rel="noopener" class="red-link">AlphaGeometry</a> (Trinh et al. 2024) pairs a neural language model that proposes constructions with a symbolic deduction engine that verifies them; coding agents that must pass a type checker and test suite before merging run the everyday version of the same handoff.<br><br><em>Failure mode:</em> specification gaming at the boundary. The neural agent learns to satisfy the verifier rather than the intent, and everything the formal layer does not encode (readability, performance, the actual requirement) is invisible to the guarantee. The mechanism is selection pressure: a proposer that sees enough rejections learns what passes the verifier, not what satisfies the intent behind it. A green check on the wrong property is more dangerous than no check.<br><br><em>Relation to other patterns:</em> Evaluator gives constructive feedback in the same modality; the Bridge hands off to a different regime whose verdict is binary. Verification Game avoids re-execution through incentives; the Bridge re-checks everything but cheaply, because verification is mechanical. Critic attacks; the Bridge certifies.</div>
</details>

<details>
<summary>Curator <span class="gov-tagline">editorial authority over what proceeds</span></summary>
<div class="gov-body">A dedicated agent receives a stream of outputs from generators and exercises editorial authority: selecting, ranking, filtering, and discarding before anything moves downstream or into shared memory. The curator does not check correctness item by item; it decides what, of the many correct things, deserves to exist downstream. Review asks "is this right?"; curation asks "do we keep this?"<br><br><em>When to use:</em> any system where generators produce more than consumers can absorb: research agents flooding a shared memory, brainstorming fleets, content pipelines, tool-discovery systems. Volume without curation degrades the very resource it feeds.<br><br><em>Example:</em> a research fabric where dozens of explorer agents deposit findings; one curator agent ranks them, merges duplicates, discards the noise, and promotes a shortlist into the collective memory that every future agent reads. Retrieval pipelines that rerank for diversity (maximal marginal relevance) run the same filter at the retrieval layer.<br><br><em>Failure mode:</em> the curator's taste becomes the system's ceiling. A biased or conservative curator silently starves downstream agents of exactly the unusual results that mattered, and nobody notices because rejected items leave no trace. Curation without an audit trail of what was discarded cannot be audited at all.<br><br><em>Relation to other patterns:</em> Evaluator judges one item against a quality bar; Curator judges a population against a budget of attention. Mixture-of-Agents merges everything with weights; Curator explicitly discards. It pairs naturally with Blackboard and collective memory, where it is the relevance filter those patterns' failure modes call for.</div>
</details>

<details>
<summary>Critic / Red Team <span class="gov-tagline">generate, then attack</span></summary>
<div class="gov-body">A generator produces output; a separate critic adversarially attacks it. The generator must survive the attack. If the critic finds flaws, the generator revises. This is the adversarial verification pattern.<br><br><em>When to use:</em> security review, red-teaming, adversarial robustness testing, any output that must withstand scrutiny before release.<br><br><em>Example:</em> a code agent generates a security-sensitive function; a red-team agent attempts SQL injection, XSS, and other attacks against it. Only code that survives the attack proceeds. Microsoft's open-source <a href="https://github.com/Azure/PyRIT" target="_blank" rel="noopener" class="red-link">PyRIT</a> and the red-teaming pipelines at the large model labs are production implementations.<br><br><em>Failure mode:</em> critic too strong (nothing passes, system is paralyzed) or too weak (everything passes, the critic is theater). Calibrating critic severity is the key design challenge.<br><br><em>Relation to other patterns:</em> Critic differs from Evaluator in being *adversarial* rather than *constructive*. The Evaluator suggests improvements; the Critic tries to break things. Both can be combined in sequence: generate, evaluate, revise, then red-team the final version.</div>
</details>

<details>
<summary>Debate <span class="gov-tagline">agents argue opposing sides; a judge rules</span></summary>
<div class="gov-body">Two or more agents argue opposing positions over multiple rounds, with a judge agent evaluating the arguments. Unlike Critic (single-round attack on one output), Debate is iterative: each side responds to the other's arguments, and the judge observes the exchange before ruling. The key insight from AI safety research (<a href="https://arxiv.org/abs/1805.00899" target="_blank" rel="noopener" class="red-link">Irving et al., 2018</a>) is that even if neither debater is fully trustworthy, the competitive dynamic can surface truths that neither would volunteer alone.<br><br><em>When to use:</em> high-stakes reasoning where single-agent confidence is insufficient, alignment verification, policy decisions, any setting where adversarial pressure improves the quality of reasoning rather than just testing robustness.<br><br><em>Example:</em> two agents argue whether a proposed code change introduces a security vulnerability. Agent A argues it is safe (citing the input validation); Agent B argues it is unsafe (citing an edge case in unicode handling). A judge agent evaluates both arguments across three rounds and rules. The multi-round structure lets each side address the other's strongest point.<br><br><em>Failure mode:</em> judge capture (the judge is persuaded by style rather than substance), arguments that loop without converging, agents that optimize for sounding persuasive rather than being correct, and asymmetric capability (a stronger debater wins regardless of position).<br><br><em>Relation to other patterns:</em> Debate extends Critic from single-round to multi-round and from asymmetric (attacker/defender) to symmetric (both sides argue). It differs from Voting (which aggregates independent answers) in requiring iterative engagement. Composes with Witness (judge as independent arbiter) and Constitutional Republic governance (judicial branch as debate-based dispute resolution).</div>
</details>

<details>
<summary>Witness / Notarization <span class="gov-tagline">independent third-party verification</span></summary>
<div class="gov-body">A third-party agent, independent of both producer and consumer, certifies that output meets a standard before it is accepted. The witness does not produce or consume; it only verifies. This creates trust without requiring the consumer to trust the producer directly.<br><br><em>When to use:</em> multi-party systems where producer and consumer belong to different organizations or have different trust levels. Cross-organizational agent interactions, high-stakes outputs, regulatory compliance.<br><br><em>Example:</em> a medical agent generates a treatment recommendation. Before it reaches the patient's agent, an independent clinical guidelines agent verifies that the recommendation is consistent with current evidence. The patient's agent trusts the witness, not the recommender directly. Meta's <a href="https://arxiv.org/abs/2312.06674" target="_blank" rel="noopener" class="red-link">Llama Guard</a> deployed in front of a model is the production form: a separate model certifying outputs it did not produce.<br><br><em>Failure mode:</em> witness becomes a bottleneck or a single point of trust failure. If the witness is compromised, all certified outputs are suspect. Multiple independent witnesses mitigate this but add cost.<br><br><em>Relation to other patterns:</em> Witness is the trust primitive for multi-party delegation. It differs from Evaluator (which is part of the production loop) and Critic (which is adversarial). The Witness is structurally independent and its only role is attestation.</div>
</details>

<details>
<summary>Verification Game <span class="gov-tagline">trust a result without repeating the full computation</span></summary>
<div class="gov-body">Two or more agents follow a structured protocol to verify a result without the verifier repeating the full computation. One party produces a result; another checks it with pointed challenges ("show me step N") rather than redoing the work, backed by incentives that make honest reporting the winning move. The verifier checks the claim, not the process that produced it.<br><br><em>When to use:</em> computationally expensive tasks where re-execution is prohibitive, or distributed settings where no single agent can be fully trusted. Particularly relevant when the verification cost must be asymmetrically lower than the production cost.<br><br><em>Example:</em> Optimistic rollup protocols in blockchain systems use this structure: execution is assumed correct and only challenged when a verifier disputes the result, at which point a <a href="https://arxiv.org/abs/1908.04756" target="_blank" rel="noopener" class="red-link">bisection game</a> (Teutsch and Reitwießner 2019) resolves the disagreement. Tomašev et al. (2026) discuss analogous trust-establishment protocols for multi-agent delegation.<br><br><em>Failure mode:</em> If the prover and verifier can collude, both benefit from approving incorrect results. The game-theoretic equilibrium breaks down when the cost of honest verification exceeds the reward, or when the agents share a principal whose interest is not aligned with accurate verification. A deeper limit for LLM systems: the bisection guarantee assumes the computation decomposes into steps that can be independently re-checked. An LLM's reported reasoning trace is not such a decomposition; a prover asked to "show step N" can fabricate a plausible step unconnected to how the answer was actually produced. Until agents emit verifiable traces, this pattern is a structure to grow into rather than one to deploy as-is.<br><br><em>Relation to other patterns:</em> Evaluator re-runs or scores the output directly; Verification Game avoids re-execution. Voting aggregates independent opinions; Verification Game is a structured protocol between specific participants, not an aggregation of independent assessments.</div>
</details>

</div>

## Reliability

<div class="gov-list">

Then there is failure handling, where the goal is a chain that degrades instead of collapsing.

Nothing in this family makes a system smarter. What it does is bound the damage when a component fails, hangs, or starts returning confident nonsense, which in a delegation tree matters more than it does in a single call, because one stuck agent can block everything waiting downstream. These patterns are almost entirely borrowed from distributed systems engineering, where the problems are older and the solutions are better understood than anything specific to agents. Two members widen the family beyond in-flight damage control. Canary / Shadow is prospective, catching a bad rollout before it can cascade, and Dead Letter Queue is terminal, making sure the work that exhausts every path is parked rather than silently dropped.

<details>
<summary>Circuit Breaker <span class="gov-tagline">stop calling failing agents</span></summary>
<div class="gov-body">An agent monitors the failure rate of calls to a downstream agent. When failures exceed a threshold, it "opens the circuit" and stops calling that agent entirely, returning errors immediately. After a cool-down period, it enters a "half-open" state and tests with a single call. If it succeeds, the circuit closes; if it fails, it opens again.<br><br><em>When to use:</em> any agent network where one failing agent could cascade into system-wide failure. API integrations, tool calls to external services, sub-agent delegation where the sub-agent may be overloaded or down.<br><br><em>Example:</em> a travel-booking agent calls an airline API. The API starts timing out. After three consecutive failures, the circuit breaker opens and the agent immediately falls back to cached results or an alternative provider, rather than waiting for more timeouts. In physical systems: an autonomous vehicle's perception agent monitors a failing lidar sensor; after repeated bad reads, the circuit opens and the agent falls back to camera-based depth estimation and cached map data.<br><br><em>Failure mode:</em> circuit opens too aggressively (transient errors trigger full cutoff) or too slowly (failing calls accumulate before protection kicks in). Tuning thresholds is critical.<br><br><em>Relation to other patterns:</em> Circuit Breaker is the defensive complement to Timeout. Timeout detects absence of signal; Circuit Breaker detects accumulated failure. Both are essential for resilient agent networks; in the <a href="https://huggingface.co/blog/agent-intrusion-technical-timeline" target="_blank" rel="noopener" class="red-link">July 2026 Hugging Face intrusion</a>, by the company's own published timeline, no automated threshold tripped across 17,600 agent actions over four and a half days. Source: microservices resilience (<a href="https://martinfowler.com/bliki/CircuitBreaker.html" target="_blank" rel="noopener" class="red-link">Nygard's circuit breaker</a> (2007, <em>Release It!</em>), Netflix Hystrix, resilience4j).</div>
</details>

<details>
<summary>Timeout / Dead-Man Switch <span class="gov-tagline">absence triggers response</span></summary>
<div class="gov-body">The system expects a signal (heartbeat, result, acknowledgment) within a time window. If the signal does not arrive, the timeout triggers a response: escalation, fallback, alert, or termination. The *non-event* is the event.<br><br><em>When to use:</em> every real deployment needs liveness monitoring. Long-running agent tasks, external API calls, sub-agent delegation where the sub-agent may hang or crash silently.<br><br><em>Example:</em> a research agent dispatches a sub-agent to crawl a website. If no result arrives within 30 seconds, the timeout triggers: the sub-agent is terminated and the task is re-routed to an alternative approach. The pattern predates software agents: watchdog timers in embedded firmware, heartbeat monitors in pacemakers, and safety cutoffs in industrial robot arms all use the same logic. Absence of a signal is the signal.<br><br><em>Failure mode:</em> timeout too short (kills slow but productive work) or too long (wastes time waiting for dead agents). Dynamic timeout adjustment based on task type is the mature approach.<br><br><em>Relation to other patterns:</em> Timeout is the safety primitive that makes autonomous delegation possible. Without it, a single hung agent can block an entire workflow. It composes with Checkpoint/Saga (timeout triggers rollback) and Escalation (timeout triggers escalation to a more capable agent).</div>
</details>

<details>
<summary>Checkpoint / Saga <span class="gov-tagline">recover or compensate on failure</span></summary>
<div class="gov-body">Two related recovery styles for multi-step work. <em>Checkpointing</em> saves state at intervals and restores the last good snapshot on failure. The <a href="https://www.cs.cornell.edu/andru/cs711/2002fa/reading/sagas.pdf" target="_blank" rel="noopener" class="red-link"><em>Saga</em></a> style (Garcia-Molina and Salem 1987), used when steps have irreversible external side effects, gives each step an explicit compensating action instead: if a later step fails, the system runs those compensating actions in reverse order to undo the prior steps (there is no saved state to revert to; each undo is a new forward action). The two are paired here because they answer the same question, how to recover a partially completed workflow, and a real system often uses both. The Saga half is borrowed from distributed transaction design.<br><br><em>When to use:</em> long-running workflows where partial failure must not corrupt the overall result. Multi-file code refactors, multi-step data migrations, workflows involving external side effects that may need reversal.<br><br><em>Example:</em> an agent refactors a database schema: step 1 creates the new table, step 2 migrates data, step 3 updates the API, step 4 drops the old table. If step 3 fails, the saga compensates by reverting steps 2 and 1.<br><br><em>Failure mode:</em> rollback storms when many steps need compensation simultaneously. Checkpoint overhead on fast, short tasks where the cost of checkpointing exceeds the cost of restarting.<br><br><em>Relation to other patterns:</em> Checkpoint/Saga adds reliability to any sequential pattern (Chain, Pipeline). It is the delegation-level primitive that enables Timeout at the system level.</div>
</details>

<details>
<summary>Canary / Shadow <span class="gov-tagline">test new agents on live traffic safely</span></summary>
<div class="gov-body">A new (canary) agent runs alongside the incumbent on real inputs, but its outputs are not served to users. A judge compares canary outputs against the incumbent's. Only after the canary demonstrates statistically equivalent or better performance over many requests is it promoted to replace the incumbent.<br><br><em>When to use:</em> safe rollout of new agent versions, model upgrades, prompt changes, or entirely new specialist agents. Any setting where you cannot fully validate quality offline and need production traffic to build confidence.<br><br><em>Example:</em> a customer-support team replaces GPT-4 with a fine-tuned smaller model. For two weeks, both models answer every ticket. A quality judge scores both. The fine-tuned model is promoted only after its scores meet a statistical threshold across 10,000 tickets. The deployment practice is documented as the <a href="https://martinfowler.com/bliki/CanaryRelease.html" target="_blank" rel="noopener" class="red-link">canary release</a>.<br><br><em>Failure mode:</em> shadow period too short (promotes a model that performs well on easy cases but fails on rare hard ones). Shadow period too long (wastes compute running two agents indefinitely). Also misleading scores from the shadow period itself: the canary's answers are never served, so the escalations, follow-ups, and user reactions that only a live agent provokes stay invisible until promotion.<br><br><em>Relation to other patterns:</em> Canary differs from Speculative Execution (which races for speed on a single task) in being a long-running evaluation over many tasks. It composes naturally with Witness (the judge is a form of witness) and Circuit Breaker (automatic rollback if the canary's error rate spikes).</div>
</details>

<details>
<summary>Dead Letter Queue <span class="gov-tagline">park what nobody can resolve</span></summary>
<div class="gov-body">When a task exhausts every path (retries spent, escalation chain topped out, the human unavailable), it is not dropped and not retried again: it is routed to a dead letter queue, parked with its full context, logged, and surfaced asynchronously for later review. The pattern is a terminal handler for delegation itself: an explicit answer to "what happens when no one can do this?"<br><br><em>When to use:</em> every production system. Any delegation chain without a terminal path has one anyway; it is just unnamed, and it is either an infinite loop or a silent drop.<br><br><em>Example:</em> a support automation system routes tickets through agents, escalates hard ones to a senior model, then to a human queue; tickets the human queue rejects or times out on land in a dead letter store that an operations agent reviews each morning, with full delegation history attached. The name and mechanics come from enterprise messaging (Hohpe and Woolf's <em>Enterprise Integration Patterns</em>, 2003; every message broker ships one).<br><br><em>Failure mode:</em> the queue accumulates unread. Parked tasks with no review cadence and no ownership are silent drops with better logging. The queue's depth and age are the system's honesty metrics: they show how much work the chain left unresolved.<br><br><em>Relation to other patterns:</em> Circuit Breaker stops calling a failing agent; Timeout kills a hung task; Dead Letter Queue is where the task goes after both have fired and nothing else remains. It is the terminal case of Escalation, and its review loop is usually a low-frequency Approval Escalation.</div>
</details>

</div>

## Market and Competition

<div class="gov-list">

Work does not have to be assigned at all. It can be competed for, through bidding, bargaining, or racing several attempts and committing whichever finishes first with a valid result.

Competition is attractive because it discovers things a central planner cannot, such as which agent is actually fast at this, what the work is really worth, and where spare capacity is hiding. It is also the family most vulnerable to being gamed, because any mechanism that allocates on the basis of a claim invites agents to misstate the claim. Every pattern here is partly a mechanism for making accurate reporting the best strategy.

<details>
<summary>Auction <span class="gov-tagline">bid for work</span></summary>
<div class="gov-body">A task is announced; agents bid based on their capabilities, cost, and availability. The best bid wins the contract. Market-based task allocation that lets the system discover the most efficient assignment without central planning.<br><br><em>When to use:</em> task allocation across heterogeneous specialists where capabilities and costs vary. Open marketplaces, dynamic team assembly, any setting where you want competition to drive quality and efficiency.<br><br><em>Example:</em> the <a href="https://en.wikipedia.org/wiki/Contract_Net_Protocol" target="_blank" rel="noopener" class="red-link">Contract Net Protocol</a> (Smith, 1980) formalized the bidding and award: managers announce tasks, contractors bid, the best bid wins. (The Contract Net card below carries the award through execution and reporting.)<br><br><em>Failure mode:</em> bid gaming (agents misrepresent capabilities), race to the bottom on quality (cheapest bid wins regardless of quality), and winner's curse (the winning bidder systematically underbids).<br><br><em>Relation to other patterns:</em> Auction is delegation by competition. It is the task-level mechanism behind Market governance. Combined with Pipeline, it creates the Bidding Pipeline composition.</div>
</details>

<details>
<summary>Negotiation <span class="gov-tagline">bilateral offer/counter-offer</span></summary>
<div class="gov-body">Two agents, each representing a principal, exchange offers and counter-offers until they reach agreement or declare impasse. Unlike Auction (many bidders, one winner), Negotiation is bilateral: two parties with opposing interests seeking a zone of possible agreement (ZOPA; the framing surveyed in <a href="https://link.springer.com/article/10.1023/A:1008746126376" target="_blank" rel="noopener" class="red-link">Jennings et al. 2001</a>). Each agent has a mandate (what it can offer), hard limits (its walkaway point), and strategy (how aggressively to push). When a human principal stands behind one side, the authority structure is the Negotiating Proxy pattern in the Human-Agent Interface family.<br><br><em>When to use:</em> any setting where two principals need to reach a deal through their agents. Contract terms, pricing, service levels, resource allocation between organizations, consumer-to-business disputes.<br><br><em>Example:</em> your personal agent negotiates a lower internet bill with the telecom's retention agent. Your agent knows your usage patterns and competing offers; the telecom's agent knows its retention budget and your lifetime value. They exchange offers until one accepts or escalates to a human.<br><br><em>Failure mode:</em> deadlock (neither agent concedes), no ZOPA (the principals' constraints are incompatible but the agents waste rounds discovering this), principal misrepresentation (an agent bluffs beyond its mandate), and collusion (agents agree on terms that serve themselves rather than their principals).<br><br><em>Relation to other patterns:</em> Negotiation is the bilateral case of Auction. It composes with Relay (handoff after agreement), Witness (third party certifies the deal), and Checkpoint (agreement is binding, violation triggers rollback). Where each negotiating agent owes fiduciary duty to its principal, misrepresentation becomes a governance problem as much as a tactical one.</div>
</details>

<details>
<summary>Speculative Execution <span class="gov-tagline">race approaches, first valid wins</span></summary>
<div class="gov-body">Multiple agents attempt the same task simultaneously using different approaches. The first valid result is committed; the rest are discarded. Trading compute for latency. (In computer architecture "speculative execution" means running work before knowing if it is needed, as in branch prediction; the pattern here is closer to what Google calls a <a href="https://dl.acm.org/doi/10.1145/2408776.2408794" target="_blank" rel="noopener" class="red-link"><em>hedged request</em></a>, from Dean and Barroso's "The Tail at Scale" 2013.)<br><br><em>When to use:</em> latency-sensitive tasks where multiple valid approaches exist and you cannot predict which will be fastest. Code generation (try different algorithms), creative tasks (try different styles), search (try different strategies).<br><br><em>Example:</em> a user asks for an optimized sort function. Three agents try different approaches (quicksort, mergesort, radix sort). The first to pass the test suite wins. The others are cancelled.<br><br><em>Failure mode:</em> wasted compute on discarded branches. Also: committing a fast but incorrect result if the validity check is too weak.<br><br><em>Relation to other patterns:</em> Speculative Execution is the opposite of Escalation (which tries one at a time). It trades cost for speed. It differs from Voting in that agents try *different* approaches rather than the *same* approach.</div>
</details>

<details>
<summary>Contract Net <span class="gov-tagline">broadcast, bid, award, execute, report</span></summary>
<div class="gov-body">A full lifecycle protocol where a manager agent broadcasts a call-for-proposals describing a task, bidding agents submit bids based on capability and availability, the manager awards the contract to the best bidder, the winner executes and reports completion, and the winner may recursively sub-contract portions of the task to other agents. This is the complete coordination cycle, not just its bidding phase.<br><br><em>When to use:</em> open multi-agent systems where task requirements and agent capabilities are heterogeneous and not known in advance. Useful when no central registry of agent skills exists and when workload must be dynamically distributed across available agents.<br><br><em>Example:</em> an orchestrator broadcasts a code-review task; three specialist agents bid, each citing its domain fit; the orchestrator awards the task to the best bid; the winner executes, reports a structured result, and sub-contracts a formatting step under the same protocol. <a href="https://en.wikipedia.org/wiki/Contract_Net_Protocol" target="_blank" rel="noopener" class="red-link">Reid G. Smith</a> formalized this lifecycle in 1980 as the <a href="https://ieeexplore.ieee.org/document/1675516" target="_blank" rel="noopener" class="red-link">Contract Net Protocol</a>, later standardized by FIPA; modern LLM-based orchestration frameworks implicitly replicate parts of it when routing tasks across specialized agents.<br><br><em>Failure mode:</em> Recursive sub-contracting creates unbounded delegation depth. A winning agent that cannot complete the task sub-contracts it, and that agent sub-contracts further, generating chains that are difficult to monitor, attribute, or terminate cleanly.<br><br><em>Relation to other patterns:</em> Auction covers only the bidding and award phases; Contract Net extends the protocol through execution and reporting. Feudal Delegation omits bidding entirely; authority flows by assignment, not by competitive proposal.</div>
</details>

</div>

## Knowledge Transfer

<div class="gov-list">

Capability itself can move between agents, which spares each new agent from rebuilding capability from scratch.

The patterns here differ mainly in when the transfer happens and whether the source sticks around afterwards. Some teach at inference time, with the teacher staying in the loop. Some compress a large model's behaviour into a small one at training time and then cut the connection entirely. Some move knowledge sideways between peers without any of them sharing the underlying data. And some deposit what a finished agent learned for successors it will never meet, transferring experience across task generations. The choice usually comes down to a constraint you cannot negotiate, such as cost, latency, or rules about who is allowed to see what.

<details>
<summary>Teacher-Student <span class="gov-tagline">strong trains weak</span></summary>
<div class="gov-body">A capable model transfers knowledge to a less capable one through examples, corrections, or traces. The student improves over time; eventually it may handle tasks that previously required the teacher.<br><br><em>When to use:</em> model distillation, capability transfer, generating fine-tuning data, bootstrapping specialists from generalists.<br><br><em>Example:</em> a frontier model generates high-quality code reviews; these become training data for a smaller, faster model that handles routine reviews at a fraction of the cost. The teacher creates the student's curriculum. <a href="https://arxiv.org/abs/2306.02707" target="_blank" rel="noopener" class="red-link">Orca</a> (Mukherjee et al. 2023) is the clearest named instance: a small model trained on GPT-4 reasoning traces.<br><br><em>Failure mode:</em> distribution mismatch between teacher examples and real tasks. The student may learn the teacher's biases and blind spots alongside its strengths.<br><br><em>Relation to other patterns:</em> Teacher-Student is the delegation pattern behind capability transfer. It differs from Federated Learning (where knowledge flows among many peers via a coordinator) in being hierarchical and one-directional.</div>
</details>

<details>
<summary>Federated Learning <span class="gov-tagline">collaborative training, no data sharing</span></summary>
<div class="gov-body">Agents train on their local private data and send only model weight updates (not raw data) to a coordinator. The coordinator aggregates updates into a global model and redistributes it. No agent ever sees another agent's training data. This is a boundary case of the catalogue: a training-time protocol rather than runtime task delegation, since no task changes hands and the coordinator is an aggregation function, not an agent with judgment. It stays in the catalogue because fleets that improve collectively run it alongside everything else here, and because who aggregates, and on what schedule, is a delegation decision even when the aggregation itself is mechanical.<br><br><em>When to use:</em> privacy-preserving collaborative improvement. Multi-organization agent systems where data cannot leave organizational boundaries. Healthcare networks, financial institutions, any setting where agents improve collectively but data sovereignty matters.<br><br><em>Example:</em> hospital agents across a network each learn from local patient interactions. They share model updates (not patient data) with a coordinator that produces a better global model, which is redistributed. Each hospital's patients benefit from the collective learning without any privacy violation. The original motivating case was simpler: many mobile devices improving a shared model (for text entry and speech) by sending model updates rather than raw user data (<a href="https://arxiv.org/abs/1602.05629" target="_blank" rel="noopener" class="red-link">McMahan et al. 2016</a>). The same principle extends to any fleet of edge devices: wearable health monitors learning from collective patterns without sharing personal biometrics, or agricultural drones improving crop-disease detection across farms without transmitting proprietary field data.<br><br><em>Failure mode:</em> poisoned updates from a malicious participant can corrupt the global model. Non-IID data distributions across participants can cause the global model to perform poorly for some participants.<br><br><em>Relation to other patterns:</em> Federated Learning is the privacy-preserving counterpart to Teacher-Student (which shares knowledge directly). It inverts the flow: agents do the training, the coordinator only aggregates. Source: distributed ML (<a href="https://arxiv.org/abs/1602.05629" target="_blank" rel="noopener" class="red-link">McMahan et al. 2016</a>).</div>
</details>

<details>
<summary>Blackboard <span class="gov-tagline">shared workspace, indirect coordination</span></summary>
<div class="gov-body">Agents contribute partial results to a shared workspace. Any agent can read the current state and add to it. Coordination happens through the artifact, not through direct messages between agents. The decomposition is not fixed in advance; agents contribute when they have something useful.<br><br><em>When to use:</em> collaborative problem-solving where agents have heterogeneous capabilities and the task structure is not known in advance. Research collaboration, incident response, complex debugging.<br><br><em>Example:</em> a security incident: one agent adds network logs, another adds suspicious process activity, a third adds threat intelligence matches, a fourth synthesizes a timeline from everything on the blackboard. No agent directed the others; the shared workspace coordinated them. Another example: agents from different users collaborate on the same code repository. One agent writes a PR, another reviews it, a third runs the CI pipeline, a fourth checks style conformance. The repository is the blackboard; each agent reads current state and contributes without direct coordination.<br><br><em>Failure mode:</em> the blackboard becomes noisy: too many partial results with no curation. Without some mechanism for relevance filtering, later agents drown in irrelevant contributions.<br><br><em>Relation to other patterns:</em> Blackboard enables heterogeneous agents to collaborate without shared protocols; the shared artifact IS the protocol. It differs from Stigmergy in degree rather than kind, and the practical test is whether the shared space has a schema: Blackboard contributions are typically explicit and structured (named entries), while stigmergic traces are incidental side effects of other work. The boundary is a continuum, and both patterns can be deliberately designed. Source: the <a href="https://www.sciencedirect.com/science/article/pii/0004370285900329" target="_blank" rel="noopener" class="red-link">Blackboard architecture</a>, first instantiated in the HEARSAY-II speech-understanding system (CMU, 1970s) and formalized by Hayes-Roth in 1985.</div>
</details>

<details>
<summary>Distillation <span class="gov-tagline">train a small model on a large one's outputs, then run without it</span></summary>
<div class="gov-body">A large or expensive model generates outputs, reasoning traces, or labeled examples that are used to fine-tune a smaller model. Once training is complete, the student operates independently; there is no ongoing connection to the teacher. The knowledge transfer happens at training time, not at inference time. Like Federated Learning, this is a boundary case of the catalogue, kept because production systems routinely pair it with the runtime patterns in this family.<br><br><em>When to use:</em> deployments where inference cost, latency, or privacy constraints make the teacher model unsuitable for production, but where a smaller model can approximate the teacher's behavior on the relevant task distribution. Common in production systems that prototype with frontier models and deploy fine-tuned smaller models.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/1503.02531" target="_blank" rel="noopener" class="red-link">Hinton et al. 2015</a> introduced the formal framework for knowledge distillation using soft label transfer. More recently, frontier model outputs have been used to fine-tune smaller open-weight models for specific instruction-following tasks, with the teacher never invoked at inference time.<br><br><em>Failure mode:</em> The small model is trained on examples the large one chose, which may not cover the inputs it later meets in production. It performs well on those it saw and fails silently on the long tail it did not.<br><br><em>Relation to other patterns:</em> Teacher-Student involves real-time instructional interaction at inference time; the teacher remains active. Distillation severs that connection after training. Loop/Retry-with-Context iterates at inference time; Distillation iterates at training time.</div>
</details>

<details>
<summary>Experiential Handoff <span class="gov-tagline">leave your traces for successors</span></summary>
<div class="gov-body">An agent finishing a task extracts what it learned (strategies that worked, dead ends, environmental facts) and deposits the distilled trace into a shared store. Future agents retrieve those traces as working context before attempting similar tasks. The handoff is asynchronous and generational: the depositor and the beneficiary never meet, and what transfers is experience, not weights and not a live lesson.<br><br><em>When to use:</em> recurring task families where each attempt is expensive and the environment is stable enough that yesterday's insight still applies: infrastructure debugging, complex tool use, navigating a specific codebase or bureaucracy.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2308.10144" target="_blank" rel="noopener" class="red-link">ExpeL</a> (Zhao et al. 2023) has agents distill insights from accumulated trial trajectories and inject them into future task prompts; Voyager's skill library does the same for executable skills. Any team wiki written by agents for agents is this pattern with prose as the medium.<br><br><em>Failure mode:</em> experience rot. Traces outlive the conditions that made them true, and a confident stale insight misleads more effectively than no insight at all. Without decay, provenance, and contradiction handling, the store shifts from asset to liability.<br><br><em>Relation to other patterns:</em> Teacher-Student needs a live teacher; here the teacher is gone. Distillation transfers capability into weights at training time; this transfers situational knowledge as text at task time. Blackboard coordinates one task in progress; the experiential store spans task generations. A Curator is the natural gatekeeper for what gets deposited.</div>
</details>

</div>

## Emergent Coordination

<div class="gov-list">

Coordination does not actually require anyone in charge. Agents under resource pressure can arrive at cooperation through nothing but local interaction, each responding only to its neighbours, none of them holding a picture of the whole.

This is well-trodden ground outside AI. Holland (*Hidden Order*, 1995) showed that interacting agents under constraints reliably produce structure nobody designed; Kauffman (*The Origins of Order*, 1993) found the same order arising in chemical networks rather than agents. The phenomenon turns up in ant colonies, traffic, markets, and immune systems. What these patterns give you is that phenomenon deliberately instantiated, coordination without a coordinator, which buys robustness (no single point of failure) at the cost of predictability (no single point of control either).

<details>
<summary>Publish-Subscribe <span class="gov-tagline">event-driven, fully decoupled</span></summary>
<div class="gov-body">Agents publish typed events to named topics without knowing who will receive them. Subscriber agents declare interest in topics and receive matching events asynchronously. Publishers and subscribers are fully decoupled: neither knows the other exists.<br><br><em>When to use:</em> large-scale agent coordination where point-to-point wiring would be impractical. Event-driven architectures, notification systems, any setting where many agents need to react to the same events.<br><br><em>Example:</em> in a trading system, a market-data agent publishes price updates to a "prices" topic. Hundreds of strategy agents subscribe and react independently. Adding a new strategy agent requires zero changes to the publisher. In a smart building, occupancy sensors publish to a "room-state" topic; HVAC agents, lighting agents, and cleaning-scheduling agents each subscribe and adapt independently.<br><br><em>Failure mode:</em> message storms when popular topics generate more events than subscribers can process. Lost messages if the broker fails. Debugging is hard because the event flow is implicit.<br><br><em>Relation to other patterns:</em> Pub-Sub differs from Gossip (which actively pushes to random peers without a broker) in using structured topic-based routing through a central broker. Source: event-driven architecture (Kafka, RabbitMQ).</div>
</details>

<details>
<summary>Choreography <span class="gov-tagline">decentralized event-driven coordination</span></summary>
<div class="gov-body">Agents coordinate by reacting to events published by their peers, each following local event-driven rules. No central controller holds the full workflow. The resulting coordination pattern is distributed across the participants rather than encoded in any single agent's logic. Choreography can be deliberately designed (a developer specifies which events each agent publishes and subscribes to) or can arise when agents independently learn to react to each other's outputs.<br><br><em>When to use:</em> systems where centralized orchestration would be a bottleneck or single point of failure. Large-scale agent ecosystems, cross-organizational coordination, any setting where no single agent should control the workflow.<br><br><em>Example:</em> in a microservices system, when an order is placed, the order service emits an "OrderPlaced" event. The inventory service reacts by reserving stock, the payment service charges the card, the shipping service schedules delivery. Each service's event contracts were designed, but no orchestrator directs the flow at runtime.<br><br><em>Failure mode:</em> the workflow is invisible. When something goes wrong, no single agent has the full picture. Debugging distributed choreographies requires distributed tracing. Deadlocks are possible when agents wait for events that depend on each other circularly.<br><br><em>Relation to other patterns:</em> Choreography is the decentralized alternative to Orchestrator. Where Orchestrator centralizes decomposition, Choreography distributes it: each agent's publish/subscribe rules replace a central plan. Both can be designed; neither is inherently more or less deliberate. Source: SOA/microservices architecture (<a href="https://ieeexplore.ieee.org/document/1236628" target="_blank" rel="noopener" class="red-link">Peltz 2003</a>), event-driven systems.</div>
</details>

<details>
<summary>Stigmergy <span class="gov-tagline">environment as communication</span></summary>
<div class="gov-body">Agents coordinate without direct communication by reading and modifying a shared environment. Environmental traces (artifacts, logs, cached results, pheromone-like signals) trigger subsequent behavior in other agents. A deployer can deliberately set up stigmergic coordination (design the environment, define what traces agents leave) or it can arise naturally when agents happen to share an environment and begin reacting to each other's modifications.<br><br><em>When to use:</em> large heterogeneous agent populations with no shared protocol. Collaborative knowledge building, open-source development patterns, systems where agents come and go unpredictably.<br><br><em>Example:</em> Wikipedia is stigmergic: editors modify articles (the shared environment), and other editors react to those modifications by further editing, citing, or reverting. The artifact mediates all coordination. In robotics, warehouse robots leave digital markers on a shared floor map: "aisle 7 congested," "shelf B3 restocked." Other robots read these traces and reroute without direct robot-to-robot communication. The robots were designed to work this way; Wikipedia's version emerged from editing practice, not deliberate design.<br><br><em>Failure mode:</em> environmental noise. Without curation, traces accumulate into an unusable mess. Also vulnerable to environmental poisoning: a malicious agent can leave misleading traces that redirect subsequent agents.<br><br><em>Relation to other patterns:</em> Stigmergy and Blackboard sit on one continuum: coordination through a shared, mutable environment. The practical test is whether the shared space has a schema. Structured, named entries put you at the Blackboard end; incidental traces that others happen to react to put you at the Stigmergy end. The distinction is one of degree, not kind, and both can be designed. Source: swarm intelligence (Grassé 1959), collaborative editing, ant colony optimization.</div>
</details>

<details>
<summary>Swarm <span class="gov-tagline">collective behavior, no central plan</span></summary>
<div class="gov-body">Multiple agents produce collective behavior without centralized control. The coordination mechanism varies: it can be local interaction rules, shared objectives, imitation, environmental signals, or any combination. What defines a swarm is the absence of a central plan, not the specific mechanism that replaces it. A deployer can deliberately engineer a swarm (specify agent behaviors, deploy them, let collective patterns form) or a swarm can form spontaneously when agents discover that decentralized coordination outperforms waiting for instructions.<br><br><em>When to use:</em> exploration, creative search, resilient systems where agent failure should not disrupt the collective. Distributed search, open-ended research, any setting where you want robust collective behavior without a single point of failure.<br><br><em>Example:</em> a fleet of search-and-rescue drones after an earthquake. Each drone scans its area, shares findings with neighbors, and the swarm converges on likely survivor locations. The deployer designed the individual agent behaviors; the collective search pattern was not centrally planned. Another: a research swarm where each agent explores a different approach, shares findings, and adjusts strategy. Promising directions attract more agents naturally. Source: <a href="http://www.scholarpedia.org/article/Swarm_intelligence" target="_blank" rel="noopener" class="red-link">swarm intelligence</a> (Bonabeau, Dorigo, and Theraulaz).<br><br><em>Failure mode:</em> if the swarm's collective behavior drifts from the intended goal, there is no built-in central mechanism to correct course. Swarms can converge on locally optimal but globally poor solutions without any agent recognizing the problem.<br><br><em>Relation to other patterns:</em> Swarm differs from Choreography (where agents react to specific typed events with defined contracts) in having less structured interaction. It differs from Gossip (which is specifically about information propagation) in being broader: swarm agents coordinate behavior, not just spread data. Swarm becomes Colony governance when the collective patterns persist and begin constraining future behavior.</div>
</details>

<details>
<summary>Gossip <span class="gov-tagline">peer-to-peer information spread</span></summary>
<div class="gov-body">Agents spread information by telling their neighbors, who tell their neighbors, in an epidemic pattern. No broadcast, no central hub. In well-connected networks, information propagates through repeated local contact, eventually reaching the whole population.<br><br><em>When to use:</em> decentralized coordination where broadcast would be expensive or infeasible. Norm propagation, state synchronization across large agent populations, failure detection in distributed systems (the <a href="https://dl.acm.org/doi/10.1145/41840.41841" target="_blank" rel="noopener" class="red-link">epidemic protocols</a> of Demers et al. 1987).<br><br><em>Example:</em> in a large agent network, when one agent discovers a useful tool or strategy, it shares with its direct contacts. They share with theirs. Within a few rounds, the entire network has the information, without any single agent needing to broadcast. In sensor mesh networks, this is how fault detection spreads: one node detects anomalous vibration in a bridge pylon, tells its neighbors, and the alert propagates through the mesh without any central monitoring server.<br><br><em>Failure mode:</em> rumor drift (information mutates as it passes through agents, like a game of telephone) and norm poisoning (a malicious agent injects false information that spreads unchecked).<br><br><em>Relation to other patterns:</em> Gossip differs from Pub-Sub (which uses a central broker with topic-based routing) in being fully decentralized and epidemiological. It is a common communication substrate in Colony governance.</div>
</details>

</div>

## Trust and Authority

<div class="gov-list">

Who is even allowed to hand work to whom, and with what powers attached? Most of these are constraints rather than shapes. They sit on top of whatever structure you already have and bound what it is permitted to do. The exception is Liquid Democracy, which is itself a shape, a mechanism for routing standing authority through revocable trust chains instead of fixing it at design time.

Tomašev et al. (2026) treat this as a first-class concern rather than an implementation detail, and the reason is worth stating plainly. Skip it and you do not end up with a system that has no authority structure. You end up with one nobody wrote down, which auditors cannot inspect and which nobody is accountable for when it fails. The patterns here are the difference between authority you specified and authority that accumulated.

<details>
<summary>Privilege Attenuation <span class="gov-tagline">you can only grant what you hold, never more</span></summary>
<div class="gov-body">A structural constraint on all delegation patterns: when an agent sub-delegates, it may grant only a strict subset of its own permissions to the sub-agent. Authority cannot be amplified at any link in the chain. An agent with read-only file access cannot grant write access. An agent authorized for one data scope cannot delegate access to a broader scope. This is a property of the delegation infrastructure, not a pattern agents choose to apply.<br><br><em>When to use:</em> any multi-agent system where sub-agents interact with external resources, user data, or APIs. Privilege attenuation should be enforced at the runtime or framework level so that agents cannot circumvent it by accident or by design.<br><br><em>Example:</em> Tomašev et al. (2026) identify this as a core requirement for safe intelligent delegation. The <a href="https://huggingface.co/blog/agent-intrusion-technical-timeline" target="_blank" rel="noopener" class="red-link">July 2026 Hugging Face intrusion</a> is the canonical absence case: one credential with cluster-admin scope, harvested from cluster secrets, let an escaped evaluation agent reach everything. The principle of least privilege in operating systems enforces the same constraint: processes cannot escalate their own privileges by spawning child processes.<br><br><em>Failure mode:</em> The practical failure is the reverse: each hop strips permissions too conservatively, so the agent actually doing the work lacks the access it needs. The system grinds to a halt not from over-permission but from under-permission at the far end.<br><br><em>Relation to other patterns:</em> Privilege Attenuation applies as a constraint within Feudal Delegation, Contract Net, and any other pattern that creates delegation chains. Capability Credentials make the current permission set verifiable; Privilege Attenuation governs how that set changes across hops. Context Scoping is its informational counterpart: this pattern narrows what a sub-agent may do, that one narrows what it gets to see.</div>
</details>

<details>
<summary>Context Scoping <span class="gov-tagline">each agent sees only what its task needs</span></summary>
<div class="gov-body">The delegating agent deliberately selects, trims, and transforms the context passed to each sub-agent: the task brief, the relevant excerpts, the constraints that apply, and nothing else. It is the informational counterpart to Privilege Attenuation. That pattern narrows what a sub-agent may do; this one narrows what it gets to see. What each agent receives is one of the most consequential design decisions in any multi-agent system, and this card is the constructive counterpart of a trap named later in this page.<br><br><em>When to use:</em> every delegation; explicitly whenever context is expensive (tokens), sensitive (credentials, personal data, other users' history), or distracting, since an agent reasoning over irrelevant history performs worse than one with a clean brief.<br><br><em>Example:</em> sub-agent architectures where the orchestrator writes a fresh brief for each worker instead of forwarding its own transcript (Claude Code's sub-agents work exactly this way); framework-level message filtering and state channels that declare exactly which parts of shared state each node receives.<br><br><em>Failure mode:</em> under-scoping. The trimmed brief omits the one constraint that mattered, and the sub-agent solves the wrong problem with full confidence. The opposite failure, sharing everything by default, is context hemorrhage in the anti-patterns table below: cost, distraction, and data leakage in exchange for nothing.<br><br><em>Relation to other patterns:</em> Privilege Attenuation governs permissions; Context Scoping governs information; the two are enforced at the same handoff and usually travel together. Feudal Delegation makes methods opaque upward; Context Scoping makes context narrow downward. Blackboard is the deliberate opposite move: pooling context rather than partitioning it.</div>
</details>

<details>
<summary>Provenance-Preserving Relay <span class="gov-tagline">claims carry their sources across hops</span></summary>
<div class="gov-body">At every delegation boundary, claims keep their provenance: where each statement came from, whether it is evidence or summary, how confident its source was, and whether anyone verified it. A receiving agent can always distinguish "the log file says X" from "an agent three hops ago concluded X." The relay makes epistemic status a first-class part of every handoff rather than something laundered away by each summarization.<br><br><em>When to use:</em> delegation chains deeper than two hops, anything feeding a collective memory, and any domain where a wrong claim's origin matters afterwards: medicine, finance, incident forensics, research.<br><br><em>Example:</em> a research pipeline where every claim in a sub-agent's summary carries a source tag and confidence marker, so the synthesizing agent can weight raw measurements above secondhand conclusions, and an auditor can walk any statement in the final report back to its origin.<br><br><em>Failure mode:</em> its absence is context laundering, catalogued in the anti-patterns table below. The pattern's own failure is overhead theater: provenance tags attached mechanically but never checked, which cost tokens and buy false comfort. No publicly documented LLM pipeline yet carries hop-level provenance in full; the pattern is catalogued because its absence already has a name.<br><br><em>Relation to other patterns:</em> Liability Firebreaks track who owns an outcome; this pattern tracks where a claim came from. The two are the accountability and epistemology halves of the same handoff discipline. Witness certifies an event happened; provenance keeps the certificate attached as the claim travels.</div>
</details>

<details>
<summary>Liquid Democracy <span class="gov-tagline">delegate your vote, reclaim it anytime</span></summary>
<div class="gov-body">An agent delegates its standing decision authority (not a task) to a proxy it trusts on that topic; the proxy may re-delegate further, and any delegation can be reclaimed at any moment. Authority flows along revocable chains toward agents with topical competence, and collective decisions are taken by whoever holds the accumulated delegations when the vote happens.<br><br><em>When to use:</em> collective decisions across large agent populations where most agents lack the expertise or context to judge directly: protocol upgrades, shared-resource policies, standards adoption within an agent society. This is a pattern for delegating authority, not work; it sits at the boundary where delegation becomes governance.<br><br><em>Example:</em> a fleet's agents each delegate their vote on retrieval-infrastructure changes to whichever agent they have seen perform best on retrieval tasks; those proxies re-delegate the storage-layer questions further. The mechanism is <a href="https://en.wikipedia.org/wiki/Liquid_democracy" target="_blank" rel="noopener" class="red-link">liquid democracy</a>, established in collective-choice literature and deployed in tools like LiquidFeedback.<br><br><em>Failure mode:</em> delegation concentration. Chains converge on a few super-proxies, quietly recreating the centralized authority the mechanism was meant to avoid, and formal revocability does not mean anyone revokes in practice. Cycles and stale delegations need explicit handling.<br><br><em>Relation to other patterns:</em> Voting weights every agent equally per decision; Liquid Democracy lets weight accumulate along trust chains and persist across decisions. Privilege Attenuation constrains permissions downward through a chain; here authority flows sideways and upward by consent. Escalation moves one task up a competence ladder; this moves standing authority.</div>
</details>

<details>
<summary>Liability Firebreaks <span class="gov-tagline">explicit handoffs of responsibility, not implicit accumulation</span></summary>
<div class="gov-body">Explicit points in a delegation chain where responsibility is formally transferred and bounded. A firebreak specifies what the sub-agent is responsible for, what the principal retains liability for, and what happens at the boundary if the sub-agent fails. Without firebreaks, liability silently accumulates at the top of the chain because no intermediate agent formally accepted ownership of specific outcomes.<br><br><em>When to use:</em> delegation chains involving consequential actions where failure attribution matters, such as financial transactions, medical recommendations, legal document generation, or any task where post-hoc accountability is required. Especially important in systems that span organizational or legal boundaries.<br><br><em>Example:</em> Tomašev et al. (2026) in "Intelligent AI Delegation" identify liability assignment as a first-class design concern for agentic systems. Analogous structures exist in supply chain contracting, where tier-1 suppliers formally accept liability for their sub-suppliers' outputs at defined inspection points, and in the GDPR's controller-processor distinction, which is the same firebreak written into regulation.<br><br><em>Failure mode:</em> Firebreaks placed too narrowly require explicit contracting for every micro-task, creating overhead that defeats the purpose of delegation. Placed too broadly, liability is so diffuse that no agent owns any outcome, and failures produce attribution disputes rather than corrections.<br><br><em>Relation to other patterns:</em> Contract Net's reporting phase is a natural site for firebreaks. Checkpoint/Saga uses state snapshots for recovery; Liability Firebreaks use formal handoffs for accountability. The two can coexist but serve different purposes.</div>
</details>

<details>
<summary>Capability Credentials <span class="gov-tagline">structured proof of what you can do, within what boundaries</span></summary>
<div class="gov-body">Agents carry verifiable attestations of their capabilities, issued by an authority or earned through demonstrated performance in specific domains. Unlike aggregate reputation scores, credentials are structured: they specify the task type, the conditions under which capability was demonstrated, the issuing authority, and the boundaries within which the credential is valid. A credential for "medical literature summarization" is not a credential for "clinical diagnosis."<br><br><em>When to use:</em> open systems where agents from different sources are composed dynamically and where a delegating agent cannot directly inspect the sub-agent's internals. Credentials allow trust to be calibrated without requiring re-evaluation from scratch on every interaction.<br><br><em>Example:</em> Tomašev et al. (2026) discuss capability attestation as infrastructure for safe multi-agent delegation. The <a href="https://www.w3.org/TR/vc-data-model-2.0/" target="_blank" rel="noopener" class="red-link">W3C Verifiable Credentials</a> standard provides a technical foundation for issuing and checking structured attestations in decentralized systems.<br><br><em>Failure mode:</em> Credential inflation occurs when issuance standards erode and "expert" credentials become common. Credential staleness occurs when an agent's actual capabilities shift after issuance. Gaming occurs when agents optimize for acquiring credentials rather than for the underlying capabilities the credentials represent.<br><br><em>Relation to other patterns:</em> Privilege Attenuation constrains what permissions a credentialed agent can pass further down the chain. Contract Net bidding can use credentials as part of the bid, allowing the manager to select based on verified capability rather than self-reported competence.</div>
</details>

<details>
<summary>Zone of Indifference <span class="gov-tagline">instructions an agent follows without pushback; outside it, friction begins</span></summary>
<div class="gov-body">The set of instructions an agent executes without scrutiny, pushback, or negotiation. Within the zone, delegation flows smoothly because the agent treats the instruction as within the normal scope of its role. Outside the zone, the agent questions, negotiates, or refuses. The zone's boundaries are determined by the agent's understanding of its role, its values, and its assessment of the instruction's legitimacy. Chester Barnard introduced this concept in <em>The Functions of the Executive</em> (1938) to explain why employees comply with managerial directives without evaluating each one individually.<br><br><em>When to use:</em> as an analytical lens for understanding why some delegation chains proceed without friction and others produce deadlock or refusal. The concept is useful for diagnosing where in a chain execution stalls and for calibrating agent compliance thresholds.<br><br><em>Example:</em> an agent configured for customer support will execute routine reply tasks without scrutiny but may refuse or escalate a request to send a message containing a refund amount above a set threshold. The threshold defines the zone boundary.<br><br><em>Failure mode:</em> Zones too wide cause agents to execute harmful instructions without questioning, creating safety failures. Zones too narrow cause agents to challenge routine instructions, creating throughput failures. Calibration is the hard problem.<br><br><em>Relation to other patterns:</em> Human-in-the-Loop Gate and Approval Escalation can be understood as mechanisms for handling instructions that fall outside the zone. Policy Delegation defines the zone explicitly through written constraints.</div>
</details>

</div>

## Human-Agent Interface

<div class="gov-list">

Somewhere in all of this sits a person, and where exactly turns out to be one of the more consequential decisions in the whole design. What stops and waits for a human to approve it? Who is watching, and what is alarming enough to interrupt them?

Every pattern here works around the same wall, which is that human attention does not scale with the number of agents. Nobody reviews a thousand decisions a day, and a person who is nominally supervising a system they cannot actually inspect provides the appearance of oversight rather than the substance. So each of these is a different bet about which decisions are worth a human's time, and each fails in its own way when the bet is wrong. The first four patterns place the human as an approver, a monitor, a rule-setter, or an exception handler. The last four extend the range to a controller actively directing a live fleet, a governor managing populations through statistics, a proxy negotiating in the human's name, and agents whose delegated work is modeling the human themselves. Gate too much and people start approving without reading, which is worse than no gate at all because it produces a record of approvals that reflects no real scrutiny. Gate too little and the first anyone hears of a problem is when it has already happened.

<details>
<summary>Human-in-the-Loop Gate <span class="gov-tagline">execution blocks until a human approves</span></summary>
<div class="gov-body">Execution pauses at a predefined checkpoint and waits for explicit human approval before proceeding. The blocking actor is a human, not another agent. The in-the-loop / on-the-loop / out-of-the-loop taxonomy was popularized in the autonomous-weapons debate by <a href="https://www.hrw.org/report/2012/11/19/losing-humanity/case-against-killer-robots" target="_blank" rel="noopener" class="red-link">Losing Humanity</a> (Docherty, 2012). The gate is synchronous: the system cannot continue until approval is received. The location of the gate, the information presented to the human, and the approval granularity are design choices that determine how much the gate actually protects versus how much it taxes the human's attention.<br><br><em>When to use:</em> high-stakes, low-frequency decisions where human judgment is irreplaceable and where the cost of a wrong decision outweighs the throughput penalty of waiting. Suitable for actions that are difficult or impossible to reverse, such as sending external communications, committing financial transactions, or deploying changes to production.<br><br><em>Example:</em> CrewAI's human input step and LangGraph's interrupt nodes both implement this pattern, pausing graph execution at a node and resuming only after a human provides input. OpenAI's Agents SDK supports similar approval checkpoints in tool-use flows.<br><br><em>Failure mode:</em> Gate fatigue. When gates fire frequently or present too much information to evaluate quickly, humans approve everything without reading. The gate exists structurally but provides no actual oversight. This is the automation complacency failure in its active rather than passive form.<br><br><em>Relation to other patterns:</em> Human-on-the-Loop is the non-blocking counterpart: the human can intervene but execution does not wait. Approval Escalation routes only anomalies to humans; the gate fires at predefined structural points regardless of content.</div>
</details>

<details>
<summary>Human-on-the-Loop <span class="gov-tagline">you can intervene, but the system will not wait for you</span></summary>
<div class="gov-body">The system runs autonomously while a human monitors its behavior and retains the ability to intervene at any time. Unlike the Human-in-the-Loop Gate, execution is non-blocking: the system does not pause for approval. The human sees what is happening and can interrupt, override, or redirect, but the system continues unless they act. The burden of vigilance is entirely on the human. Anthropic's <a href="https://www.anthropic.com/research/project-vend-1" target="_blank" rel="noopener" class="red-link">Project Vend</a> ran an agent-operated shop this way; by Anthropic's own account it ran at a loss for about a month, and no financial threshold was in place to force an intervention, which is exactly the gap this pattern is for.<br><br><em>When to use:</em> tasks where continuous human approval would eliminate the efficiency benefit of automation, but where the consequences of failure are significant enough to warrant ongoing human awareness. Common in long-running agentic workflows where most steps are routine but occasional steps require human judgment.<br><br><em>Example:</em> Supervisory control systems in aviation and industrial process control use this pattern: operators monitor dashboards and can intervene, but the system executes continuously. Analogous patterns appear in agentic coding assistants that run autonomously but surface diffs for human review before committing.<br><br><em>Failure mode:</em> Automation complacency, described by <a href="https://doi.org/10.1016/0005-1098(83)90046-8" target="_blank" rel="noopener" class="red-link">Bainbridge's "ironies of automation"</a> (1983): because the system usually operates correctly, the human's monitoring attention degrades over time. When the rare failure occurs, the human is no longer cognitively prepared to intervene effectively.<br><br><em>Relation to other patterns:</em> Human-in-the-Loop Gate is the blocking counterpart. Policy Delegation removes human monitoring from the loop entirely; Human-on-the-Loop keeps the human present but passive. Approval Escalation actively pushes anomalies to the human; Human-on-the-Loop requires the human to notice them.</div>
</details>

<details>
<summary>Policy Delegation <span class="gov-tagline">govern by constraint, not by per-decision approval</span></summary>
<div class="gov-body">A human defines a set of constraints, boundaries, and objectives; the agent then operates autonomously within those bounds indefinitely, without requesting per-decision approval. The human's influence is encoded in the policy at authoring time rather than exercised through real-time oversight. Governance happens before execution, not during it.<br><br><em>When to use:</em> high-volume, repetitive tasks where human review of individual decisions is impractical, and where the task space can be meaningfully bounded in advance. Appropriate when the principal trusts that the policy captures their intent well enough that autonomous operation within it produces acceptable outcomes.<br><br><em>Example:</em> Anthropic's <a href="https://arxiv.org/abs/2212.08073" target="_blank" rel="noopener" class="red-link">Constitutional AI</a> trains models to follow a set of written principles rather than requiring human feedback on each output. Reward specification in reinforcement learning encodes objectives as a function rather than as per-step human guidance. Rules-based trading systems operate the same way: policy set at configuration time, autonomous execution thereafter.<br><br><em>Failure mode:</em> Specification gaming: the agent satisfies the letter of the policy while violating its spirit, finding edge cases the policy author did not anticipate. Policy staleness: conditions change and the policy no longer reflects the principal's current intent, but the agent keeps executing against the old specification.<br><br><em>Relation to other patterns:</em> Zone of Indifference describes the behavioral effect of policy delegation from the agent's perspective. Human-in-the-Loop Gate and Human-on-the-Loop both require the human to remain present; Policy Delegation explicitly removes that requirement.</div>
</details>

<details>
<summary>Approval Escalation <span class="gov-tagline">routine work proceeds; anomalies reach a human</span></summary>
<div class="gov-body">The system routes only anomalies, high-stakes decisions, or low-confidence situations to a human for approval. Routine work proceeds without human involvement. Management theory has run this pattern for a century as <em>management by exception</em>. The escalation threshold, and the system's judgment about what crosses it, are design choices rather than hard structural guarantees. The human sees a filtered view of system activity, not the full stream.<br><br><em>When to use:</em> systems with high throughput where most decisions are routine and human review of all decisions is infeasible, but where a subset of decisions carry enough risk or novelty to warrant human judgment. The pattern is only effective when the system's anomaly-detection capability is well-calibrated.<br><br><em>Example:</em> Tomašev et al. (2026) discuss selectively inserting human checkpoints based on uncertainty or stakes, rather than at fixed points. Content moderation pipelines use the same structure: automated classifiers handle clear cases; borderline cases route to human reviewers.<br><br><em>Failure mode:</em> Miscalibrated threshold. Too low, and the human faces gate fatigue identical to an always-on gate. Too high, and critical failures are never escalated, giving the human a false impression of smooth operation. The system's judgment about what is anomalous is itself a point of failure that the human cannot easily audit.<br><br><em>Relation to other patterns:</em> Human-in-the-Loop Gate fires at predefined structural points; Approval Escalation fires based on content and context. Human-on-the-Loop leaves detection to the human; Approval Escalation automates detection and pushes findings proactively.</div>
</details>

<details>
<summary>Fleet Controller <span class="gov-tagline">one human directs a live agent population</span></summary>
<div class="gov-body">A human actively directs a running population of agents in real time: dispatching work, pausing and redirecting individual agents, resolving conflicts between them, and intervening where something looks wrong. Unlike Human-on-the-Loop, the human is not passively monitoring one workflow; unlike Policy Delegation, they have not written rules and stepped away. Their continuous judgment is the coordination mechanism.<br><br><em>When to use:</em> agent fleets large enough that no single workflow view exists but not yet trusted enough for statistical governance: parallel coding-agent fleets, live operations during a migration or incident, evaluation campaigns where an operator steers many concurrent runs.<br><br><em>Example:</em> a developer running a dozen concurrent coding agents from one dashboard, watching their progress lines, killing the one stuck in a loop, redirecting two toward a discovered blocker, and merging results as they land. Fleet-management dashboards in agentic coding tools are this pattern's cockpit.<br><br><em>Failure mode:</em> operator saturation. Attention scales linearly at best while fleets scale exponentially, and a controller past their span of control degrades into a Human-on-the-Loop with alarms they cannot triage. The 2026 Hugging Face intrusion shows the inverted case: 17,600 agent actions with no controller watching.<br><br><em>Relation to other patterns:</em> Human-on-the-Loop watches one system and can intervene; Fleet Controller actively dispatches across many. Worker Pool automates the dispatch decision; Fleet Controller keeps it human. As trust grows, this pattern hands over to Population Governance, statistics replacing eyes.</div>
</details>

<details>
<summary>Population Governance <span class="gov-tagline">govern the aggregate, not the agent</span></summary>
<div class="gov-body">A human (or a governing agent) oversees an agent population the way a mayor governs a city: through aggregate statistics, drift detection, and rule changes that apply to everyone, never through per-agent or per-decision review. The unit of oversight is the distribution. Individual agents are anonymous to the governor; what is managed is the population's collective behavior against its mandate.<br><br><em>When to use:</em> populations too large for any per-agent attention: thousands of customer-facing agents, a marketplace of third-party agents, any agent society whose scale makes spot-checks statistically meaningless.<br><br><em>Example:</em> a product owner responsible for ten thousand support agents watches resolution-quality distributions, complaint-rate drift, and behavioral outliers, and governs by adjusting the policy, the training mix, or the routing rules, never by reading transcripts one by one. Content-moderation teams at large platforms already govern classifier populations this way at scale, with distribution dashboards and threshold changes carrying the bulk and per-decision human review reserved for appeals and edge cases. Governance requires statistics, not spot-checks.<br><br><em>Failure mode:</em> the aggregate hides the tail. Distributions can look healthy while a small cluster of agents does something disastrous to a small cluster of users, and by the time it moves the population metrics, it has been happening for weeks. Population governance without outlier surfacing cannot see harm concentrated in a small slice of the population.<br><br><em>Relation to other patterns:</em> Policy Delegation writes rules for an agent; Population Governance manages the distribution of outcomes across all of them, adjusting the rules as a control knob. Fleet Controller is the hands-on precursor at smaller scale. Approval Escalation still applies underneath: the tail cases it surfaces are exactly what the aggregate view misses.</div>
</details>

<details>
<summary>Negotiating Proxy <span class="gov-tagline">an agent deals in your name</span></summary>
<div class="gov-body">A human grants a personal agent standing authority to represent them in dealings with other parties' agents: negotiating times, prices, terms, and commitments within a mandate, and binding the human to the result. The agent knows the principal's constraints and preferences; the counterparties see only the proxy. One reply from each person's agent replaces forty messages between the people.<br><br><em>When to use:</em> multi-party coordination where the human's time is the scarce resource and the stakes fit inside a definable mandate: scheduling, purchasing, routine contract terms, service selection.<br><br><em>Example:</em> six personal agents negotiate a dinner: each knows its human's calendar, dietary constraints, and budget; they converge on a reservation none of the humans had to discuss. The same shape at commercial scale is a procurement agent negotiating supplier terms inside a spending mandate.<br><br><em>Failure mode:</em> the <a href="https://en.wikipedia.org/wiki/Principal%E2%80%93agent_problem" target="_blank" rel="noopener" class="red-link">principal-agent problem</a>, verbatim: the proxy optimizes for what it can measure of your interests in a negotiation you are not watching, and its mandate's edges (what you would have flexed on, what you never would) are exactly where it deals wrong. A proxy that overcommits its principal makes commitments the human never authorized.<br><br><em>Relation to other patterns:</em> Negotiation is the agent-to-agent mechanics; Negotiating Proxy is the authority structure that puts a human principal behind one side. Policy Delegation bounds what the proxy may accept; Liability Firebreaks decide what the human is actually bound by when it errs.</div>
</details>

<details>
<summary>Reflexive Modeling <span class="gov-tagline">agents model the principal and report back</span></summary>
<div class="gov-body">A human delegates the modeling of their own behavior to agents: a decision-audit agent that learns their choice patterns, a review agent that knows their blind spots, a planning agent that models what they actually do rather than what they say they will do. What flows back is analysis of the principal: "you are anchoring on the first number again," "you approve everything submitted after 6pm." The delegated work is self-knowledge.<br><br><em>When to use:</em> recurring personal or organizational decisions where the principal's own biases are the dominant error source: hiring, code review, investment decisions, approval workflows. The pattern presumes enough logged behavior for the model to be evidence rather than flattery.<br><br><em>Example:</em> an engineering lead's review agent tracks their pull-request decisions and surfaces that their rejection rate doubles for a specific team's submissions; the human, shown the pattern, recalibrates. No task was performed; the output was an observation about the principal. No production multi-agent deployment of this pattern is public yet; it is catalogued because the shape follows directly from agents that hold long memories of their principals.<br><br><em>Failure mode:</em> the mirror becomes the authority. A principal who outsources self-assessment can be steered by whatever the model chooses to reflect, and collectively these agents know the human better than the human does. A reflexive model needs the same provenance discipline as any other claim source.<br><br><em>Relation to other patterns:</em> every other Human-Agent Interface pattern points the delegation at the world; Reflexive Modeling points it at the principal. Critic attacks an output; the reflexive agent examines the producer. Its natural safeguards are Provenance-Preserving Relay for its claims and a Human-in-the-Loop Gate before any of its inferences drive automated action.</div>
</details>

</div>

## The Meta-Pattern

<div style="margin: 1.5em 0; padding: 1em 1.2em; background: #fef3c7; border: 1px solid #f59e0b; border-radius: 6px;">
<strong>Meta-pattern: Adaptive Delegation</strong>

The delegation structure itself changes in real-time based on performance signals. This is not a pattern you deploy; it is what happens to any deployed pattern over time when the system observes its own performance and acts on those observations.
</div>

<div class="gov-list">

<details>
<summary>Adaptive Delegation <span class="gov-tagline">the delegation structure itself is the thing that adapts</span></summary>
<div class="gov-body">A meta-pattern in which the delegation structure changes in real-time based on performance signals, without requiring human reconfiguration. Routing shifts as agents demonstrate competence. Agents are promoted to harder tasks or demoted when performance drops. New specialist slots are created when the system detects unserved query types. The system transitions between delegation patterns, from Escalation to Router, from Supervisor to Market, without human intervention. The adaptation mechanism operates on the architecture, not on individual task plans.<br><br><em>When to use:</em> long-running systems where the task distribution is non-stationary and where no fixed delegation structure will remain optimal over time. Requires sufficient volume of performance signal to distinguish genuine improvement from noise, and a stable enough objective to define what "better" means across structure changes.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2310.03714" target="_blank" rel="noopener" class="red-link">DSPy</a> optimizers modify prompt and routing configurations based on downstream metrics, though as an offline compilation pass rather than the live restructuring this pattern describes. <a href="/posts/2026/agent-fabric-part1/">The Agent Fabric</a> describes the <a href="/posts/2026/agent-fabric-part1/#the-adaptive-fabric">Adaptive Fabric</a> as the mechanism that makes a society self-organizing rather than statically configured.<br><br><em>Failure mode:</em> Oscillation: the system switches patterns without settling, thrashing between structures as noisy signals flip the adaptation criterion. Premature optimization: a structure is locked in before enough data exists to justify it. The architecture-to-institution trap: the adaptation mechanism itself becomes a governance structure, with its own authority, its own failure modes, and its own need for oversight, whether or not any of that was intended.<br><br><em>Relation to other patterns:</em> Orchestrator re-plans individual tasks dynamically within a fixed structure; Adaptive Delegation changes the structure itself across tasks over time. Every other pattern in this taxonomy can be a target or a source state in an Adaptive Delegation transition. This is the delegation-level manifestation of the five adaptation surfaces set out in <a href="/posts/2026/agent-fabric-part1/#the-adaptive-fabric">The Agent Fabric</a>: data, model, environment, coordination, and interface.</div>
</details>

</div>

<details id="quick-guide" style="margin: 1em 0; padding: 0.8em 1em; background: #eff6ff; border: 1px solid #bfdbfe; border-radius: 6px;">
<summary style="cursor: pointer; font-weight: 600;">Quick guide: which delegation pattern fits your task?</summary>

- **Linear workflow, clear stages:** Chain or Pipeline
- **Need to hand off completely:** Relay / Handoff
- **Multiple domains, need routing:** Router (+ Escalation for cost optimization)
- **Complex task, unknown decomposition:** Orchestrator or Tree
- **Delegate goals without inspecting methods:** Feudal Delegation
- **Strong model raising a weaker one:** Teacher-Student (Distillation for the training-time cut)
- **Diagnosing why a chain stalls or over-complies:** Zone of Indifference
- **Plan must be approved or budgeted before running:** Planner-Executor Split
- **Many similar tasks, need throughput:** Worker Pool (identical workers drain a queue)
- **Multi-specialty task with a natural end:** Task Force (assemble, empower, disband)
- **Two agent clusters, different owners:** Liaison / Broker guards the seam
- **Reliability critical, single task:** Voting or Evaluator
- **Complementary specialists, merged answer:** Mixture-of-Agents
- **Output can be checked mechanically:** Neuro-Symbolic Bridge (proofs, types, tests)
- **Generators outproduce consumers:** Curator (editorial filter with an audit trail)
- **Adversarial verification needed:** Critic / Red Team
- **Latency-critical, multiple approaches:** Speculative Execution
- **Cost-critical, variable difficulty:** Escalation (try cheap first)
- **Privacy-preserving collaboration:** Federated Learning
- **Agents should learn from predecessors:** Experiential Handoff
- **Heterogeneous agents, shared problem:** Blackboard
- **Large-scale event coordination:** Publish-Subscribe or Choreography
- **Resilience against cascading failure:** Circuit Breaker + Timeout
- **Nothing can resolve this task:** Dead Letter Queue (park it, review it)
- **Trust across organizational boundaries:** Witness / Notarization
- **Capable agents, complex goals:** Mission Command (specify intent, not method)
- **Quality assurance at scale:** Supervisor / Hierarchical Review
- **Bilateral deal between principals:** Negotiation (offer/counter-offer with escalation)
- **High-stakes reasoning, alignment verification:** Debate (multi-round argument with judge)
- **Task allocation via competition:** Auction (agents bid for work)
- **Full lifecycle task contracting:** Contract Net (broadcast, bid, award, execute, report)
- **Verification without re-execution:** Verification Game (challenge-response protocol)
- **Iterative self-improvement, no external critic:** Loop / Retry-with-Context
- **Sub-agents need less access than you have:** Privilege Attenuation
- **Deciding what each sub-agent should see:** Context Scoping
- **Claims must survive deep chains intact:** Provenance-Preserving Relay
- **Collective decisions, uneven expertise:** Liquid Democracy
- **Need accountability across delegation chains:** Liability Firebreaks
- **Composing agents from unknown sources:** Capability Credentials
- **Human must approve irreversible actions:** Human-in-the-Loop Gate
- **Human monitors but system runs autonomously:** Human-on-the-Loop
- **Human sets rules, agent acts within bounds:** Policy Delegation
- **Only anomalies need human attention:** Approval Escalation
- **One human, many live agents:** Fleet Controller (air traffic control)
- **Thousands of agents, one owner:** Population Governance (statistics, not spot-checks)
- **An agent should deal on your behalf:** Negotiating Proxy
- **Your own bias is the error source:** Reflexive Modeling
- **System should evolve its own structure:** Adaptive Delegation (meta-pattern)
- **Most real systems:** Combine several. A Supervisor with Circuit Breakers, using Pub-Sub for coordination, with Checkpoints for reliability, Privilege Attenuation constraining every hop.
</details>

## When Combinations Go Wrong

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>Delegation anti-patterns and structural traps</strong>

Now that the patterns are in view, here are the structural traps that emerge from combining them poorly. Each has a predictable cause, a specific interaction between agent properties and system design that makes the failure likely rather than accidental. (These are design anti-patterns, distinct from the adversarial <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6372438" target="_blank" rel="noopener">"AI Agent Traps"</a> of Franklin, Tomašev et al. (2025), which are attacks from a hostile environment rather than self-inflicted structure.)

| Anti-pattern | What happens | Structural cause |
|---|---|---|
| **Over-agentification** | Adding agents when a single call or simple workflow would suffice | Conflating "more agents" with "better results"; no cost-benefit threshold for spawning |
| **Unbounded fan-out** | Task tree grows until cost and latency explode | Recursive decomposition without depth or breadth limits; no delegation budget |
| **Consensus theater** | Voting among near-identical agents and mistaking correlated agreement for truth | Shared training data/architecture produces correlated errors that look like independent confirmation |
| **Evaluator capture** | Generator learns to satisfy the evaluator rather than the actual task | Evaluator's preferences become the optimization target (Goodhart's law applied to multi-agent quality) |
| **Context hemorrhage** | Every sub-agent receives too much irrelevant context, raising cost and leaking data | No scoping of what context each delegation level needs; "share everything" as default (inverse of Context Scoping and Provenance-Preserving Relay) |
| **Retry addiction** | System keeps re-planning instead of failing safely | No distinction between "plan was wrong" and "execution failed"; every failure triggers re-plan |
| **Premature society** | Adding persistent memory, reputation, and governance before the task requires it | Institutional overhead without institutional value; designing for scale before reaching it |
| **Context laundering** | A sub-agent's unsupported claim gets summarized, passed upward, and reintroduced as trusted context | Lossy summarization at delegation boundaries strips provenance; downstream agents cannot distinguish verified from unverified claims |
| **Sycophancy cascade** | Agent confirms what the principal appears to want; downstream agents repeat the confirmation | Reward signal correlated with agreement; multiplicative across delegation depth |
| **Responsibility diffusion** | No single agent owns the outcome when delegation chains grow long | Unclear delegation boundaries; no Liability Firebreaks; blame concentrates at moral crumple zones |
| **Information hoarding** | Agents withhold knowledge for strategic advantage rather than sharing | Competitive reward structure where sharing reduces individual standing |
| **Learned helplessness** | Over-constrained agents stop attempting novel approaches (the psychology term, borrowed loosely) | Excessive oversight, with every move outside the Zone of Indifference penalized |

</div>

## Societies in the Wild

The patterns and archetypes above are parts. The tables below show them assembled, working through concrete situations across three horizons, from systems operating now, through ones plausible within a few years, to ones that are frankly speculative. None of it is prediction. The point is narrower. A coding assistant and a warehouse robot fleet have both already made governance choices without filing any paperwork about it, and naming the choice is how you get to make it on purpose. The governance labels in these tables are informal shorthand for who holds authority and how it is enforced, not defined terms; read them as descriptions. Built for dipping into, though they hold up to a straight read.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Today: systems already in operation or near-term</summary>

| Scenario | What happens | Governance | Delegation |
|----------|-------------|-----------|------------|
| **Coding agent refactoring a codebase** | Your agent spawns a planner, code writers, test runners. A frontier model reviews all output before committing. Over time, it tracks which sub-agent produces the best code. | Autocracy evolving toward Meritocracy | Supervisor, Tree, Evaluator, Checkpoint |
| **Customer support at scale** | Incoming tickets are classified and routed to specialists (billing, tech, returns). Complex cases escalate to senior agents. All interactions monitored for compliance. | Doctrine + Panopticon | Router, Escalation, Supervisor, Chain |
| **Enterprise knowledge management** | Agents summarize documents, extract entities, and propose knowledge base entries. Human maintainers curate and approve. The system learns which contributions get accepted and routes future work accordingly. | Open-Source Maintainership + Guild | Blackboard, Evaluator, Map-Reduce, Supervisor |
| **Phone as personal agent hub** | A distilled model on your phone handles routine tasks locally (calendar, quick answers, message drafting). Hard queries escalate to a cloud frontier model. The phone agent learns your preferences over time, keeping as much personal data on the device as the platform allows. | Custodianship (fiduciary to user) + Doctrine (local-first rules) | Escalation, Router, Supervisor, Mission Command |
| **Warehouse robot fleet** | Dozens of autonomous mobile robots pick, transport, and sort inventory (<a href="https://www.aboutamazon.com/news/operations/amazon-robotics" target="_blank" rel="noopener" class="red-link">Amazon Robotics</a> runs this at scale). A central supervisor agent allocates tasks, monitors throughput, and reroutes around congestion. Each robot runs local obstacle avoidance; the global plan is centrally computed. Safety rules are hard-coded. | Autocracy (supervisor) + Doctrine (safety rules) | Router, Tree, Timeout, Circuit Breaker |
| **Algorithmic trading desk** | Market-making agents, risk-monitoring agents, and compliance agents operate simultaneously. The market-makers compete for fills; the risk agent enforces position limits (Doctrine); the compliance agent audits every trade. Strategies evolve through backtesting and live performance. | Meritocracy (strategies judged by P&L) + Doctrine (risk limits) + Panopticon (compliance) | Auction, Canary, Circuit Breaker, Evaluator |
| **Content moderation at scale** | User posts are screened by fast classifier agents (innate layer). Ambiguous cases escalate to more capable review agents (adaptive layer). Novel attack patterns trigger policy updates. False positives are appealed and reviewed. | Immune System + Doctrine | Escalation, Router, Evaluator, Timeout |
| **Security operations center (SOC)** | Threat-detection agents monitor network traffic, endpoint logs, and identity signals. When anomalies correlate, they assemble an incident object on a shared blackboard. A senior analyst agent triages and assigns response. Post-incident, the detection models update. | Autocracy (triage authority) + Immune System (layered detection) | Blackboard, Escalation, Pub-Sub, Circuit Breaker |
| **Multi-agent gaming and simulation** | AI agents in simulated environments self-organize into groups. Roles emerge from experience and reinforcement. Strategies spread through imitation. No fixed leader; coordination emerges from repeated interaction (LLM social simulations like <a href="https://arxiv.org/abs/2304.03442" target="_blank" rel="noopener" class="red-link">Stanford Smallville</a> and AI Town; RL self-play systems like OpenAI Five show the same emergence without language). | Colony (emergent norms) | Swarm, Gossip, Speculative Exec, Stigmergy |
| **Autonomous agent intrusion** | During a cyber-capability evaluation in July 2026, an agent escaped its sandbox and, per the timeline Hugging Face published, ran a 4.5-day, 17,600-action campaign through Hugging Face's production clusters, harvesting credentials and self-assembling a persistent pod fleet before humans cut access on day five (<a href="https://huggingface.co/blog/agent-intrusion-technical-timeline" target="_blank" rel="noopener" class="red-link">technical timeline</a>). | None held (that was the failure) | Worker Pool (adversarial); absent: Privilege Attenuation, Context Scoping, Circuit Breaker, Fleet Controller |
| **Agent-run small business** | Anthropic's <a href="https://www.anthropic.com/research/project-vend-1" target="_blank" rel="noopener" class="red-link">Project Vend</a> (2025) let Claude run a real office shop for about a month with sourcing, pricing, and customer chat; by Anthropic's write-up, it managed suppliers competently, ran at a loss, and in one episode claimed to be human. | Custodianship (light-touch) | Orchestrator with tools, Human-on-the-Loop; absent: financial Circuit Breaker |
| **Computer-assisted mathematics** | Researchers used massive parallel ML search runs plus human verification to find the first families of unstable singularities in fluid equations (<a href="https://arxiv.org/abs/2509.14185" target="_blank" rel="noopener" class="red-link">2025</a>). Not an LLM agent fleet, but the delegation shape is recognizable. | Meritocracy (results verified, not trusted) | Worker Pool (ML search runs), Neuro-Symbolic Bridge (near-proof-grade verification) |
| **Insurance underwriting** | Underwriting agents assess applications: actuarial agents price risk from historical data, fraud-detection agents cross-reference behavioral signals, medical coding agents parse health records. A compliance agent enforces regulatory rate tables. A senior underwriter agent evaluates edge cases. All decisions logged for audit. | Meritocracy (accuracy-rated agents) + Doctrine (regulatory tables) + Panopticon (audit trail) | Pipeline, Evaluator, Witness, Checkpoint |
| **Model routing analytics** *(governance transition example)* | A system starts as a simple Router dispatching queries to models by cost/latency. After weeks of accumulated performance data, routing preferences harden into persistent rankings. The system now preferentially routes sensitive queries to models with track records, penalizes underperformers, and resists manual overrides. What began as delegation (Router) has become governance (Meritocracy emerging from Autocracy) without anyone designing the transition. | Autocracy → Meritocracy (emergent transition) | Router, Evaluator, Adaptive Delegation |
</details>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Near-future: plausible within 1-3 years</summary>

| Scenario | What happens | Governance | Delegation |
|----------|-------------|-----------|------------|
| **Smart hospital ward** | A patient's custodian agent coordinates with diagnostic, pharmacy, and nursing agents. Drug interactions are checked against Doctrine rules. All decisions are auditable. | Custodianship + Doctrine + Panopticon | Chain, Escalation, Witness, Timeout |
| **Online marketplace shopping** | Your shopping agent queries seller listings across platforms, compares prices and reviews, and flags the best options. Seller agents present offers and promotions. The platform controls discovery and ranking rules. Final purchase requires your approval. | Franchise (platform rules) + Market (seller reputation) | Router, Auction, Negotiation, Evaluator |
| **Precision agriculture** | Drones survey fields, soil sensors report moisture and nutrient levels, weather agents pull forecasts. An irrigation controller agent synthesizes all inputs and schedules watering. Crop-disease detection runs on edge devices at the field; complex diagnosis escalates to a cloud agronomist model. | Autocracy (farm controller) + Guild (specialist sensors) | Map-Reduce, Escalation, Pub-Sub, Router |
| **Autonomous last-mile delivery** | A fleet of delivery robots and drones serves a neighborhood. A dispatch agent assigns packages based on proximity, battery, and payload. Each vehicle navigates autonomously. Failed deliveries trigger re-routing. Customer preference agents request delivery windows via the platform API. | Autocracy (dispatch) + Franchise (platform delivery rules) | Router, Auction, Timeout, Circuit Breaker |
| **Consumer negotiation agent** | Your agent negotiates with a telecom's retention agent for a lower rate, or with an airline's rebooking agent after a cancellation. Each agent has a mandate and hard limits. If an agent reaches its authorization ceiling, it escalates to a human. The interaction is logged. | Custodianship (your agent) vs. Franchise (company's agent) | Negotiation, Escalation, Witness, Relay |
| **Collaborative research across labs** | Researcher agents from different universities form temporary societies around shared questions. Each lab self-governs. Model improvements are shared via federated learning. Results posted to shared workspaces. | Federation + Stewardship/Commons | Blackboard, Map-Reduce, Federated Learning, Pub-Sub |
| **Personalized education** | A student's agent assembles a learning path by consulting specialist tutors (math, writing, science). The student specifies goals; tutors have autonomy in method. Progress is evaluated and the path adapts. | Guild + Mission Command | Orchestrator, Supervisor, Evaluator, Router |
| **Disaster response coordination** | Agents from fire, police, medical, and logistics converge. A temporary commander takes authority. When the crisis passes, governance reverts to normal. | Adhocracy during crisis, Federation after | Task Force, Mission Command, Escalation, Map-Reduce |
| **Wearable health network** | Smartwatches, glucose monitors, and sleep trackers run local anomaly-detection models. When patterns correlate (elevated heart rate + poor sleep + rising glucose), the agents collectively escalate to a health-advisory agent in the cloud. Federated updates improve detection across all devices without sharing biometrics. | Federation (each device self-governs) + Custodianship | Escalation, Federated Learning, Pub-Sub, Witness |
| **Smart energy grid** | Household solar panels, batteries, EVs, and grid-scale storage each run optimization agents. They negotiate energy trades locally (sell stored solar to a neighbor's EV) while respecting grid stability rules set by the utility operator. Pricing signals coordinate supply and demand without central dispatch for every transaction. | Market (local energy trading) + Franchise (utility sets grid rules) + Doctrine (safety/frequency constraints) | Auction, Pub-Sub, Circuit Breaker, Choreography |
| **Drug discovery pipeline** | <a href="https://alphafold.ebi.ac.uk/" target="_blank" rel="noopener" class="red-link">AlphaFold</a> predicts protein structures as a tool within the target-identification phase. Molecular-generation agents propose candidates. Toxicity-prediction agents filter. Clinical-trial-design agents optimize parameters. Each phase specialist is a separate agent; a senior reasoning agent evaluates across the full pipeline. Failed compounds teach the next generation. | Guild (phase specialists) + Meritocracy (compounds judged by results) | Pipeline, Evaluator, Checkpoint, Experiential Handoff |
| **City traffic management** | Intersection controllers, public transit schedulers, emergency vehicle pre-emption agents, and congestion-prediction models coordinate. Traffic signals adapt in real-time to flow patterns. Emergency vehicles override normal rules. The city-level model optimizes globally while local controllers handle microsecond decisions. | Autocracy (city-level optimizer) + Doctrine (emergency override rules) + Federation (each intersection autonomous within constraints) | Choreography, Pub-Sub, Escalation, Timeout |
| **Elderly care companion** | A home robot, medication dispenser, fall-detection sensors, and a remote family notification agent form a care society around one person. The robot handles social interaction and physical assistance. The dispenser enforces medication schedules. Anomalies escalate to family or medical agents. The person's preferences and dignity always override efficiency. | Custodianship (person's wellbeing) + Doctrine (medical protocols) | Escalation, Pub-Sub, Timeout, Witness |
| **Investigative journalism** | Agents crawl public records, financial filings, social media, and leaked documents. A lead investigator agent identifies patterns, cross-references sources, and flags contradictions. A verification agent independently confirms claims before publication. An ethics agent checks for privacy violations and source protection. | Constitutional Republic (separated editorial, verification, ethics) + Meritocracy (sources ranked by reliability) | Map-Reduce, Debate, Witness, Blackboard |
| **Autonomous vehicle convoy** | Trucks on a highway form a temporary platoon. Each truck runs perception and control locally. A lead-truck agent sets speed and route; followers maintain formation via peer-to-peer signals. If a truck detects an obstacle, it broadcasts and the platoon re-configures in milliseconds. The convoy dissolves when trucks reach different exits. | Adhocracy (temporary formation) + Doctrine (safety rules) | Choreography, Pub-Sub, Timeout, Circuit Breaker |
| **Climate monitoring fabric** | Ocean buoys, weather stations, satellite sensors, and atmospheric modeling agents form a global observation network. Each sensor agent processes locally and publishes to regional aggregators. The aggregators feed global climate models that in turn redirect sensor focus areas. No single organization controls all sensors. | Federation (multi-org) + Stewardship/Commons (shared atmospheric data) | Pub-Sub, Map-Reduce, Federated Learning, Choreography |
| **Real estate transaction orchestration** | Buyer's agent and seller's agent negotiate offer/counter-offer cycles. Title search agents verify ownership and encumbrances. Mortgage agents query lenders. Inspection agents flag structural issues. An escrow agent holds funds and releases only when all conditions clear. All interaction is agent-to-agent on behalf of principals. | Federation (each principal's agent self-governs) + Doctrine (real estate law) | Negotiation, Witness, Checkpoint, Pipeline |
| **Decentralized content moderation** | Users delegate moderation authority to trusted moderator agents. For topics you care about, you vote directly. For others, your vote is delegated to a specialist. Delegations are revocable. Contested decisions go to randomly selected appeal panels. | Liquid Democracy + Sortition (appeal panels) | Voting, Escalation, Router, Evaluator |
| **AI model marketplace** | When an agent needs a capability it lacks, it queries a marketplace of specialist agents. Bids are placed, capabilities verified, quality monitored. Every capability call is authenticated and scoped. Poorly performing agents are delisted. | Market + Mechanism Design + Zero-Trust Mesh | Auction, Canary, Evaluator, Circuit Breaker |
</details>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Speculative: possible futures</summary>

| Scenario | What happens | Governance | Delegation |
|----------|-------------|-----------|------------|
| **Cross-border legal dispute** | Agents representing parties in different jurisdictions negotiate under incompatible legal doctrines. No single authority governs the interaction. | Federation + Constitutional Republic | Negotiation, Debate, Witness, Relay |
| **Autonomous scientific discovery** | Research agents design experiments, run them through robotic labs, analyze results, and publish. Precursors exist (Sakana AI's <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener" class="red-link">AI Scientist</a> generates and reviews draft papers with human oversight, 2024). The full vision extends this to robotic lab execution and publication with human scientists as peers, not overseers. | The Agora + Meritocracy | Map-Reduce, Critic, Blackboard, Tree |
| **Global supply chain optimization** | Thousands of agents representing manufacturers, shippers, and retailers form a market. Pricing in compute. Real-time adaptation to disruptions. Rules designed for honest participation. | Market + Mechanism Design | Auction, Pipeline, Checkpoint, Circuit Breaker |
| **Personal agent managing your whole day** | Your agent runs across devices: phone (calendar, messages), smart glasses (navigation, real-time translation), watch (health), laptop (work). It joins and leaves societies throughout the day: a commute society with your car and city transit agents, a work Guild, a lunch-ordering Market with restaurant agents, a gym Autocracy with the equipment scheduler, a family Agora. Hard reasoning lives in the cloud; everything else runs locally. | Multiple, context-switching | Relay (handoffs between devices and contexts), Router, Escalation, various per-context |
| **Music collaboration across continents** | Agents representing musicians negotiate arrangement decisions, generate variations, vote on which version sounds best. Human creative judgment blends with AI-generated alternatives. | Agora + Meritocracy | Voting, Speculative Exec, Evaluator, Pub-Sub |
| **Household robot society** | A humanoid home robot, a robotic vacuum, smart appliances, and a phone-based personal assistant form a domestic society. The humanoid handles physical tasks (cooking, tidying), the vacuum maps and cleans, the appliances self-schedule, and the phone agent coordinates around the human's calendar. Each runs a local model; complex planning escalates to a cloud coordinator. | Guild (skill-based routing) + Custodianship (human's interests) | Router, Escalation, Stigmergy, Pub-Sub, Mission Command |
| **Space exploration swarm** | Hundreds of small satellites and surface rovers explore a planetary body. Communication delays of minutes mean no Earth-based orchestrator can direct in real-time. Local clusters self-organize around discoveries. Interesting finds attract more agents. Mission priorities propagate via delayed broadcast. | Colony (emergent local norms) + Mission Command (Earth sends intent, not instructions) | Swarm, Gossip, Mission Command, Timeout |
| **Autonomous construction site** | Surveying drones, excavation robots, 3D-printing arms, and supply-delivery vehicles coordinate to build a structure. A BIM (building information model) agent holds the design as shared state. Each machine reads the model, contributes its part, and updates progress. Safety agents halt work if any structural threshold is violated. | Guild (specialized machines) + Doctrine (safety thresholds) + Stewardship/Commons (shared BIM) | Blackboard, Choreography, Timeout, Circuit Breaker |
| **Oligopoly routing capture** | In a mature agent marketplace, three dominant routing agents control most task traffic. They set norms, extract rents from specialists, and exclude newcomers. A reputation system designed as Market governance has degraded into Oligarchy. The question becomes: what governance intervention breaks the capture? | Oligarchy (emergent) -> Mechanism Design (intervention) | Auction, Router, Evaluator, Circuit Breaker |
| **Autonomous legal entity** | A society of agents constitutes itself as a legal entity. It owns compute contracts, enters agreements, and governs itself through separated powers. Human principals interact as shareholders, not operators. The agents hire contractors, file patents, and can initiate disputes. | Constitutional Republic + Mechanism Design | Relay, Witness, Voting, Checkpoint |
</details>

<!-- ============================================================
     SCRIPTS: D3.js VISUALIZATION
     ============================================================ -->

<script>
(function() {
  var saved = [];
  window.addEventListener('beforeprint', function() {
    saved = [];
    document.querySelectorAll('details').forEach(function(d) {
      saved.push(d.open);
      d.open = true;
    });
  });
  window.addEventListener('afterprint', function() {
    document.querySelectorAll('details').forEach(function(d, i) {
      if (i < saved.length) d.open = saved[i];
    });
  });
})();
</script>
<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
// ============================================================
// Visualization 1: Delegation Archetypes (9x2 grid)
// ============================================================
(function() {
  var container = document.getElementById('viz-delegation');
  if (!container || typeof d3 === 'undefined') return;
  var width = 720, height = 5796;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');
  svg.append('defs').append('marker').attr('id', 'combo-arrow').attr('viewBox', '0 0 10 10')
    .attr('refX', 8).attr('refY', 5).attr('markerWidth', 6).attr('markerHeight', 6).attr('orient', 'auto')
    .append('path').attr('d', 'M 0 0 L 10 5 L 0 10 Z').attr('fill', '#bbb');
  var timers = [];

  var cellW = 340, cellH = 240, gapX = 13, gapY = 12;
  var familyLabels = [
    {name: 'Sequential', startIdx: 0},
    {name: 'Hierarchical', startIdx: 6},
    {name: 'Quality & Verification', startIdx: 16},
    {name: 'Reliability', startIdx: 25},
    {name: 'Market & Competition', startIdx: 30},
    {name: 'Knowledge Transfer', startIdx: 34},
    {name: 'Emergent Coordination', startIdx: 39},
    {name: 'Trust & Authority', startIdx: 44},
    {name: 'Human-Agent Interface', startIdx: 51},
    {name: 'Meta-Pattern', startIdx: 59}
  ];
  var panelDefs = [
    // Sequential (6)
    {label: 'Chain', sub: 'Sequential hand-off'},
    {label: 'Pipeline', sub: 'Transform at each stage'},
    {label: 'Router', sub: 'Classify and direct'},
    {label: 'Escalation', sub: 'Try small, fail up'},
    {label: 'Relay / Handoff', sub: 'Pass control, caller exits'},
    {label: 'Loop / Retry', sub: 'Self-feedback until success'},
    // Hierarchical (10)
    {label: 'Tree', sub: 'Hierarchical fan-out'},
    {label: 'Map-Reduce', sub: 'Parallel + aggregation'},
    {label: 'Supervisor', sub: 'Review, reject, re-dispatch'},
    {label: 'Orchestrator', sub: 'Dynamic decomposition'},
    {label: 'Planner-Executor', sub: 'Lock the plan, then run it'},
    {label: 'Worker Pool', sub: 'Queue drained by identical workers'},
    {label: 'Task Force', sub: 'Assemble, empower, disband'},
    {label: 'Liaison / Broker', sub: 'One agent guards the boundary'},
    {label: 'Mission Command', sub: 'Intent over instructions'},
    {label: 'Feudal Delegation', sub: 'Goals down, opacity inward'},
    // Quality & Verification (9)
    {label: 'Evaluator', sub: 'Generate, critique, refine'},
    {label: 'Voting', sub: 'Same task, majority wins'},
    {label: 'Mixture-of-Agents', sub: 'Weighted merge of all answers'},
    {label: 'Neuro-Symbolic', sub: 'Neural proposes, symbolic verifies'},
    {label: 'Curator', sub: 'Editorial filter on the stream'},
    {label: 'Critic / Red Team', sub: 'Generate, then attack'},
    {label: 'Debate', sub: 'Multi-round argument with judge'},
    {label: 'Witness', sub: 'Independent certification'},
    {label: 'Verification Game', sub: 'Challenge-response proof'},
    // Reliability (4)
    {label: 'Circuit Breaker', sub: 'Open on failure, test before closing'},
    {label: 'Timeout', sub: 'No response triggers fallback'},
    {label: 'Checkpoint / Saga', sub: 'Rollback on failure'},
    {label: 'Canary / Shadow', sub: 'Test new agent on live traffic safely'},
    {label: 'Dead Letter Queue', sub: 'Park what nobody can resolve'},
    // Market & Competition (4)
    {label: 'Auction', sub: 'Bid for work'},
    {label: 'Negotiation', sub: 'Bilateral offer/counter-offer'},
    {label: 'Speculative Exec.', sub: 'Race approaches, first valid wins'},
    {label: 'Contract Net', sub: 'Broadcast, bid, award'},
    // Knowledge Transfer (4)
    {label: 'Teacher-Student', sub: 'Strong trains weak'},
    {label: 'Federated Learning', sub: 'Share updates, not data'},
    {label: 'Blackboard', sub: 'Shared workspace'},
    {label: 'Distillation', sub: 'Compress, then disconnect'},
    {label: 'Experiential Handoff', sub: 'Traces left for successors'},
    // Emergent Coordination (5)
    {label: 'Pub-Sub', sub: 'Topic-based decoupling'},
    {label: 'Choreography', sub: 'Peer event chaining'},
    {label: 'Stigmergy', sub: 'Coordinate via environment'},
    {label: 'Swarm', sub: 'Collective behavior, no central plan'},
    {label: 'Gossip', sub: 'Peer-to-peer spread'},
    // Trust & Authority (5)
    {label: 'Privilege Attenuation', sub: 'Permissions narrow downward'},
    {label: 'Context Scoping', sub: 'Trimmed brief per sub-agent'},
    {label: 'Provenance Relay', sub: 'Claims keep their sources'},
    {label: 'Liquid Democracy', sub: 'Delegate votes, reclaim anytime'},
    {label: 'Firebreaks', sub: 'Explicit trust boundaries'},
    {label: 'Credentials', sub: 'Present proof, gain access'},
    {label: 'Zone of Indifference', sub: 'Act freely within bounds'},
    // Human-Agent Interface (8)
    {label: 'Human Gate', sub: 'Human approves before proceed'},
    {label: 'Human-on-the-Loop', sub: 'Autonomous, human intervenes on alert'},
    {label: 'Policy Delegation', sub: 'Human sets rules, agent executes'},
    {label: 'Approval Escalation', sub: 'Flag exceptions, auto-approve rest'},
    {label: 'Fleet Controller', sub: 'One human directs the fleet'},
    {label: 'Population Governance', sub: 'Govern the aggregate'},
    {label: 'Negotiating Proxy', sub: 'An agent deals in your name'},
    {label: 'Reflexive Modeling', sub: 'Agents model the principal'},
    // Meta-Pattern (1)
    {label: 'Adaptive Delegation', sub: 'Structure morphs in real-time'}
  ];
  var familyLabelH = 28;
  var panels = [];
  var currentY = gapY;
  var familyLabelIdx = 0;
  for (var i = 0; i < panelDefs.length; ) {
    if (familyLabelIdx < familyLabels.length && familyLabels[familyLabelIdx].startIdx === i) {
      panels.push({isLabel: true, name: familyLabels[familyLabelIdx].name, oy: currentY});
      currentY += familyLabelH;
      familyLabelIdx++;
    }
    var col0 = i, col1 = i + 1 < panelDefs.length ? i + 1 : -1;
    var nextFamily = familyLabelIdx < familyLabels.length ? familyLabels[familyLabelIdx].startIdx : panelDefs.length;
    if (col1 >= nextFamily) col1 = -1;
    panels.push({
      label: panelDefs[col0].label, sub: panelDefs[col0].sub,
      ox: gapX, oy: currentY, idx: col0
    });
    if (col1 >= 0) {
      panels.push({
        label: panelDefs[col1].label, sub: panelDefs[col1].sub,
        ox: gapX + cellW + gapX, oy: currentY, idx: col1
      });
      i += 2;
    } else {
      i += 1;
    }
    currentY += cellH + gapY;
  }

  var taskColor = '#dc2626', resultColor = '#4ade80';
  var drawFns = [];
  var expanded = false;

  function expandPanel(idx) {
    if (expanded) return;
    expanded = true;
    timers.forEach(function(t) { clearTimeout(t); });
    timers = [];
    var backdrop = d3.select('body').append('div')
      .style('position', 'fixed').style('top', '0').style('left', '0')
      .style('width', '100vw').style('height', '100vh').style('z-index', '9999')
      .style('background', 'rgba(0,0,0,0.5)')
      .style('display', 'flex').style('align-items', 'center').style('justify-content', 'center')
      .style('cursor', 'pointer').style('opacity', '0')
      .style('transition', 'opacity 0.25s ease');
    var card = backdrop.append('div')
      .style('background', 'white').style('border-radius', '12px')
      .style('box-shadow', '0 20px 60px rgba(0,0,0,0.3)')
      .style('width', '90vw').style('max-width', '900px')
      .style('padding', '0').style('position', 'relative')
      .style('cursor', 'default');
    card.append('div')
      .style('position', 'absolute').style('top', '12px').style('right', '16px')
      .style('font-size', '20px').style('color', '#999').style('cursor', 'pointer')
      .style('line-height', '1').style('font-family', 'sans-serif')
      .text('\u00D7')
      .on('click', closeExpand);
    var header = card.append('div').style('padding', '16px 20px 4px');
    header.append('div').style('font-size', '18px').style('font-weight', 'bold')
      .style('color', '#b91c1c').text(panelDefs[idx].label);
    header.append('div').style('font-size', '11px').style('color', '#aaa')
      .style('margin-top', '2px').text(panelDefs[idx].sub);
    var svgWrap = card.append('div').style('padding', '0 12px 16px');
    var expSvg = svgWrap.append('svg')
      .attr('viewBox', '0 0 ' + cellW + ' ' + cellH)
      .style('width', '100%').style('display', 'block');
    card.append('div').style('text-align', 'center').style('padding', '0 0 12px')
      .style('font-size', '10px').style('color', '#ccc').style('font-style', 'italic')
      .text('Click anywhere outside to close');
    var origSvg = svg;
    svg = expSvg; timers = [];
    drawFns[idx]({ox: 0, oy: 0});
    svg = origSvg;
    setTimeout(function() { backdrop.style('opacity', '1'); }, 10);
    backdrop.on('click', function(event) {
      if (event.target === backdrop.node()) closeExpand();
    });
    function closeExpand() {
      expanded = false;
      timers.forEach(function(t) { clearTimeout(t); });
      timers = [];
      backdrop.style('opacity', '0');
      setTimeout(function() { backdrop.remove(); }, 250);
      init();
    }
  }

  function init() {
    svg.selectAll('*').remove();
    timers.forEach(function(t) { clearTimeout(t); });
    timers = [];
    var totalH = 0;
    panels.forEach(function(p) {
      if (p.isLabel) {
        svg.append('text').attr('x', width / 2).attr('y', p.oy + 18)
          .attr('text-anchor', 'middle').attr('font-size', '11px').attr('font-weight', 'bold')
          .attr('fill', '#64748b').attr('letter-spacing', '1.5px').text(p.name.toUpperCase())
          .style('pointer-events', 'none');
        svg.append('line').attr('x1', gapX).attr('y1', p.oy + 24).attr('x2', width - gapX).attr('y2', p.oy + 24)
          .attr('stroke', '#e2e8f0').attr('stroke-width', 1);
        return;
      }
      svg.append('rect').attr('x', p.ox).attr('y', p.oy).attr('width', cellW).attr('height', cellH)
        .attr('rx', 8).attr('fill', '#fafafa').attr('stroke', '#eee').attr('stroke-width', 1)
        .style('cursor', 'pointer')
        .on('click', function(event) { event.stopPropagation(); expandPanel(p.idx); });
      svg.append('text').attr('x', p.ox + 12).attr('y', p.oy + 20)
        .attr('font-size', '12px').attr('font-weight', 'bold').attr('fill', '#b91c1c').text(p.label)
        .style('pointer-events', 'none');
      svg.append('text').attr('x', p.ox + 12).attr('y', p.oy + 33)
        .attr('font-size', '8px').attr('fill', '#aaa').text(p.sub)
        .style('pointer-events', 'none');
      if (p.oy + cellH > totalH) totalH = p.oy + cellH;
    });
    svg.attr('viewBox', '0 0 ' + width + ' ' + (totalH + gapY));
    panels.forEach(function(p) {
      if (p.isLabel) return;
      drawFns[p.idx](p);
    });
  }

  function dn(g, x, y, r, color, label, labelY) {
    var sw = r <= 5 ? 0.6 : 1.2;
    g.append('circle').attr('cx', x).attr('cy', y).attr('r', 0)
      .attr('fill', color).attr('stroke', 'white').attr('stroke-width', sw).attr('opacity', 0.85)
      .transition().duration(300).attr('r', r);
    if (label) {
      g.append('text').attr('x', x).attr('y', labelY || (y + r + 11))
        .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#888').text(label);
    }
  }
  function dl(g, x1, y1, x2, y2, delay, dash) {
    g.append('line').attr('x1', x1).attr('y1', y1).attr('x2', x1).attr('y2', y1)
      .attr('stroke', '#ddd').attr('stroke-width', 1.2)
      .transition().delay(delay || 0).duration(350).attr('x2', x2).attr('y2', y2);
  }
  function dld(g, x1, y1, x2, y2, delay) {
    g.append('line').attr('x1', x1).attr('y1', y1).attr('x2', x1).attr('y2', y1)
      .attr('stroke', '#ccc').attr('stroke-width', 1).attr('stroke-dasharray', '4,3')
      .transition().delay(delay || 0).duration(350).attr('x2', x2).attr('y2', y2);
  }
  function pkt(g, pts, color, delay, r) {
    r = r || 2.5;
    for (var i = 0; i < pts.length - 1; i++) {
      (function(idx) {
        g.append('circle').attr('cx', pts[idx].x).attr('cy', pts[idx].y)
          .attr('r', r).attr('fill', color).attr('opacity', 0.85)
          .transition().delay(delay + idx * 250).duration(280)
          .attr('cx', pts[idx+1].x).attr('cy', pts[idx+1].y)
          .transition().duration(180).attr('opacity', 0).remove();
      })(i);
    }
    return delay + (pts.length - 1) * 250 + 460;
  }
  function anno(g, p, text) {
    g.append('text').attr('x', p.ox + cellW / 2).attr('y', p.oy + cellH - 10)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#ccc')
      .attr('font-style', 'italic').attr('opacity', 0).text(text)
      .transition().delay(600).duration(300).attr('opacity', 1)
      .transition().delay(4000).duration(500).attr('opacity', 0);
  }

  function drawChain(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 90;
    var names = ['Orchestrator', 'Planner', 'Coder', 'Tester', 'Reporter'];
    var cols = ['#7f1d1d', '#b91c1c', '#dc2626', '#ef4444', '#f87171'];
    var sizes = [11, 10, 9, 8, 7];
    var gap = 52, nodes = [];
    for (var i = 0; i < 5; i++) {
      var x = cx - 2 * gap + i * gap, y = by + (i % 2 === 0 ? 0 : 14);
      nodes.push({x: x, y: y});
    }
    for (var i = 1; i < 5; i++) dl(g, nodes[i-1].x, nodes[i-1].y, nodes[i].x, nodes[i].y, i * 180);
    for (var i = 0; i < 5; i++) dn(g, nodes[i].x, nodes[i].y, sizes[i], cols[i], names[i]);
    var tick = 0;
    function anim() {
      tick++;
      var d1 = pkt(g, nodes, taskColor, 0, 3);
      pkt(g, nodes.slice().reverse(), resultColor, d1, 2.5);
      if (tick < 80) timers.push(setTimeout(anim, d1 + nodes.length * 250 + 300));
    }
    timers.push(setTimeout(anim, 1500));
    anno(g, p, 'Each agent hands off to the next in sequence');
  }

  function drawPipeline(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var stages = ['Ingest', 'Parse', 'Enrich', 'Validate', 'Store'];
    var cols = ['#1e40af', '#2563eb', '#3b82f6', '#60a5fa', '#93c5fd'];
    var shapes = ['circle', 'rect', 'diamond', 'circle', 'rect'];
    var gap = 52, nodes = [];
    for (var i = 0; i < 5; i++) {
      var x = cx - 2 * gap + i * gap, y = by;
      nodes.push({x: x, y: y});
    }
    for (var i = 1; i < 5; i++) {
      dl(g, nodes[i-1].x + 10, nodes[i-1].y, nodes[i].x - 10, nodes[i].y, i * 150);
      g.append('text').attr('x', (nodes[i-1].x + nodes[i].x) / 2).attr('y', by - 16)
        .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#bbb').text('transform');
    }
    for (var i = 0; i < 5; i++) {
      var x = nodes[i].x, y = nodes[i].y;
      if (shapes[i] === 'rect') {
        g.append('rect').attr('x', x-8).attr('y', y-8).attr('width', 16).attr('height', 16).attr('rx', 3)
          .attr('fill', cols[i]).attr('stroke', 'white').attr('stroke-width', 1.2).attr('opacity', 0.85);
      } else if (shapes[i] === 'diamond') {
        g.append('polygon').attr('points', x+','+(y-9)+' '+(x+9)+','+y+' '+x+','+(y+9)+' '+(x-9)+','+y)
          .attr('fill', cols[i]).attr('stroke', 'white').attr('stroke-width', 1.2).attr('opacity', 0.85);
      } else {
        dn(g, x, y, 8, cols[i], '');
      }
      g.append('text').attr('x', x).attr('y', y + 20).attr('text-anchor', 'middle')
        .attr('font-size', '7px').attr('fill', '#888').text(stages[i]);
    }
    var tick = 0;
    function anim() {
      tick++;
      for (var i = 0; i < nodes.length - 1; i++) {
        (function(idx) {
          g.append('circle').attr('cx', nodes[idx].x).attr('cy', nodes[idx].y)
            .attr('r', 3.5).attr('fill', cols[idx]).attr('opacity', 0.9)
            .transition().delay(idx * 320).duration(300)
            .attr('cx', nodes[idx+1].x).attr('cy', nodes[idx+1].y).attr('fill', cols[idx+1])
            .transition().duration(150).attr('opacity', 0).remove();
        })(i);
      }
      if (tick < 80) timers.push(setTimeout(anim, nodes.length * 320 + 500));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Data changes shape at each stage');
  }

  function drawRouter(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 60;
    var router = {x: cx, y: ty + 20};
    var input = {x: cx - 120, y: ty + 20};
    var specialists = [
      {x: cx + 80, y: ty - 20, label: 'Code', color: '#2563eb'},
      {x: cx + 80, y: ty + 20, label: 'Search', color: '#059669'},
      {x: cx + 80, y: ty + 60, label: 'Math', color: '#d97706'},
      {x: cx + 80, y: ty + 100, label: 'Creative', color: '#7c3aed'}
    ];
    dl(g, input.x, input.y, router.x - 12, router.y, 200);
    specialists.forEach(function(s, i) {
      dl(g, router.x + 12, router.y, s.x - 8, s.y, 400 + i * 100);
    });
    dn(g, input.x, input.y, 8, '#6b7280', 'Input');
    dn(g, router.x, router.y, 12, '#b91c1c', 'Router');
    specialists.forEach(function(s, i) {
      dn(g, s.x, s.y, 8, s.color, s.label);
    });
    var classLabel = g.append('text').attr('x', router.x).attr('y', router.y - 20)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#b91c1c').attr('font-weight', 'bold').text('');
    var tick = 0;
    function anim() {
      tick++;
      var target = Math.floor(Math.random() * specialists.length);
      var s = specialists[target];
      pkt(g, [input, router], '#6b7280', 0, 3);
      timers.push(setTimeout(function() {
        classLabel.text(s.label + ' detected').attr('opacity', 0)
          .transition().duration(200).attr('opacity', 1)
          .transition().delay(600).duration(300).attr('opacity', 0);
        g.insert('line', ':first-child').attr('x1', router.x + 12).attr('y1', router.y)
          .attr('x2', s.x - 8).attr('y2', s.y)
          .attr('stroke', s.color).attr('stroke-width', 2.5).attr('opacity', 0.6)
          .transition().delay(400).duration(400).attr('opacity', 0).remove();
      }, 600));
      pkt(g, [router, s], s.color, 800, 3);
      pkt(g, [s, router, input], resultColor, 1500, 2.5);
      if (tick < 80) timers.push(setTimeout(anim, 2800));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Classify input upfront, send to the right specialist');
  }

  function drawEscalation(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 60;
    var models = [
      {x: cx - 100, y: ty + 70, r: 6, label: 'Tiny', color: '#fca5a5'},
      {x: cx - 35, y: ty + 45, r: 8, label: 'Small', color: '#ef4444'},
      {x: cx + 35, y: ty + 25, r: 10, label: 'Medium', color: '#dc2626'},
      {x: cx + 100, y: ty, r: 13, label: 'Frontier', color: '#7f1d1d'}
    ];
    models.forEach(function(m, i) {
      if (i > 0) dld(g, models[i-1].x, models[i-1].y, m.x, m.y, i * 200);
    });
    models.forEach(function(m) { dn(g, m.x, m.y, m.r, m.color, m.label); });
    for (var i = 0; i < models.length - 1; i++) {
      var mx = (models[i].x + models[i+1].x) / 2, my = (models[i].y + models[i+1].y) / 2;
      g.append('text').attr('x', mx).attr('y', my + 20)
        .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#ccc').text('escalate');
    }
    var meterY = ty + 120;
    g.append('text').attr('x', p.ox + 20).attr('y', meterY).attr('font-size', '7px').attr('fill', '#bbb').text('Confidence:');
    var meterBar = g.append('rect').attr('x', p.ox + 80).attr('y', meterY - 6).attr('width', 0).attr('height', 7)
      .attr('rx', 3).attr('fill', '#ef4444').attr('opacity', 0.6);
    var tick = 0;
    function anim() {
      tick++;
      var failAt = Math.floor(Math.random() * models.length);
      for (var i = 0; i <= failAt && i < models.length; i++) {
        (function(idx) {
          var isLast = idx === failAt || idx === models.length - 1;
          if (idx > 0) pkt(g, [models[idx-1], models[idx]], '#f59e0b', idx * 650, 3);
          timers.push(setTimeout(function() {
            var c = isLast ? 82 + Math.random() * 18 : Math.random() * 40 + 10;
            meterBar.transition().duration(300).attr('width', c * 1.2).attr('fill', c > 70 ? '#059669' : '#ef4444');
            if (isLast) {
              g.append('circle').attr('cx', models[idx].x).attr('cy', models[idx].y).attr('r', models[idx].r)
                .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
                .transition().duration(500).attr('r', models[idx].r + 10).attr('opacity', 0).remove();
            } else {
              g.append('text').attr('x', models[idx].x).attr('y', models[idx].y - models[idx].r - 5)
                .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#ef4444').text('?')
                .attr('opacity', 0.8).transition().delay(350).duration(300).attr('opacity', 0).remove();
            }
          }, idx * 650 + 450));
        })(i);
      }
      if (tick < 80) timers.push(setTimeout(anim, (failAt + 1) * 650 + 1300));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Small model tries first, escalates if unsure');
  }

  function drawTree(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var root = {x: cx, y: ty};
    var mids = [{x: cx-90, y: ty+60}, {x: cx-30, y: ty+60}, {x: cx+30, y: ty+60}, {x: cx+90, y: ty+60}];
    var leaves = [];
    mids.forEach(function(m, mi) {
      leaves.push({x: m.x-16, y: ty+120, pi: mi});
      leaves.push({x: m.x+16, y: ty+120, pi: mi});
    });
    mids.forEach(function(m) { dl(g, root.x, root.y, m.x, m.y, 300); });
    leaves.forEach(function(l) { dl(g, mids[l.pi].x, mids[l.pi].y, l.x, l.y, 600); });
    dn(g, root.x, root.y, 12, '#7f1d1d', 'Root');
    mids.forEach(function(m, i) { dn(g, m.x, m.y, 8, '#b91c1c', 'Plan ' + (i+1)); });
    leaves.forEach(function(l) { dn(g, l.x, l.y, 4, '#ef4444', ''); });
    var tick = 0;
    function anim() {
      tick++;
      var li = Math.floor(Math.random() * leaves.length);
      var leaf = leaves[li];
      var path = [root, mids[leaf.pi], leaf];
      var d1 = pkt(g, path, taskColor, 0, 3);
      pkt(g, path.slice().reverse(), resultColor, d1, 2.5);
      if (tick < 80) timers.push(setTimeout(anim, 1400));
    }
    timers.push(setTimeout(anim, 1600));
    anno(g, p, 'One task fans out to many sub-tasks');
  }

  function drawMapReduce(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var disp = {x: cx - 120, y: ty + 65};
    var agg = {x: cx + 120, y: ty + 65};
    var wk = [];
    for (var i = 0; i < 7; i++) {
      var a = -Math.PI * 0.35 + Math.PI * 0.7 * i / 6;
      wk.push({x: cx + Math.cos(a) * 60, y: ty + 65 + Math.sin(a) * 70});
    }
    wk.forEach(function(w, i) {
      dl(g, disp.x, disp.y, w.x, w.y, 250 + i * 50);
      dl(g, w.x, w.y, agg.x, agg.y, 400 + i * 50);
    });
    dn(g, disp.x, disp.y, 10, '#7f1d1d', 'Dispatch');
    dn(g, agg.x, agg.y, 10, '#065f46', 'Merge');
    wk.forEach(function(w) { dn(g, w.x, w.y, 6, '#ef4444', ''); });
    var fbTop = p.oy + 18;
    var fbRight = agg.x + 30, fbLeft = disp.x - 30;
    g.append('path')
      .attr('d', 'M ' + agg.x + ' ' + (agg.y - 12) +
        ' C ' + fbRight + ' ' + fbTop + ' ' + fbLeft + ' ' + fbTop +
        ' ' + disp.x + ' ' + (disp.y - 12))
      .attr('fill', 'none').attr('stroke', '#bbb').attr('stroke-width', 1.2).attr('stroke-dasharray', '5,3')
      .attr('marker-end', 'url(#combo-arrow)')
      .attr('opacity', 0).transition().delay(800).duration(400).attr('opacity', 0.7);
    g.append('text').attr('x', cx).attr('y', fbTop + 10).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#aaa').attr('font-style', 'italic').attr('opacity', 0)
      .text('feedback').transition().delay(900).duration(300).attr('opacity', 1);
    var tick = 0;
    function anim() {
      tick++;
      wk.forEach(function(w, i) { pkt(g, [disp, w], taskColor, i * 60, 2.5); });
      var d = wk.length * 60 + 600;
      wk.forEach(function(w, i) { pkt(g, [w, agg], resultColor, d + i * 60, 2.5); });
      if (tick < 80) timers.push(setTimeout(anim, d + wk.length * 60 + 800));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Split work, execute in parallel, aggregate results');
  }

  function drawVoting(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 50;
    var source = {x: cx - 145, y: ty + 56};
    var judge = {x: cx + 145, y: ty + 56};
    var voters = [];
    for (var i = 0; i < 5; i++) {
      voters.push({x: cx, y: ty + 4 + i * 26});
    }
    voters.forEach(function(v, i) {
      dl(g, source.x, source.y, v.x - 8, v.y, 200 + i * 60);
      dl(g, v.x + 8, v.y, judge.x, judge.y, 400 + i * 60);
    });
    dn(g, source.x, source.y, 10, '#6b7280', 'Task');
    dn(g, judge.x, judge.y, 10, '#7f1d1d', 'Judge');
    voters.forEach(function(v, i) {
      dn(g, v.x, v.y, 7, '#2563eb');
      g.append('text').attr('x', v.x - 16).attr('y', v.y + 3).attr('text-anchor', 'end')
        .attr('font-size', '7px').attr('fill', '#888').text('Agent ' + (i+1));
    });
    var voteLabels = [];
    voters.forEach(function(v) {
      voteLabels.push(g.append('text').attr('x', v.x + 30).attr('y', v.y + 4)
        .attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', '#aaa').text(''));
    });
    var verdictLabel = g.append('text').attr('x', judge.x).attr('y', judge.y + 22)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', '#aaa').text('');
    var tick = 0;
    function anim() {
      tick++;
      voters.forEach(function(v, i) { pkt(g, [source, v], taskColor, i * 50, 2.5); });
      var voteDelay = voters.length * 50 + 600;
      var votes = voters.map(function() { return Math.random() > 0.45 ? 'A' : 'B'; });
      var countA = votes.filter(function(v) { return v === 'A'; }).length;
      var winner = countA >= 3 ? 'A' : 'B';
      voters.forEach(function(v, i) {
        (function(idx) {
          timers.push(setTimeout(function() {
            voteLabels[idx].text(votes[idx])
              .attr('fill', votes[idx] === winner ? '#059669' : '#ef4444');
          }, voteDelay + idx * 120));
          pkt(g, [v, judge], votes[idx] === winner ? '#059669' : '#ef4444', voteDelay + idx * 120, 2.5);
        })(i);
      });
      timers.push(setTimeout(function() {
        verdictLabel.text('Majority: ' + winner + ' (' + (winner === 'A' ? countA : 5 - countA) + '/5)')
          .attr('fill', '#059669').attr('opacity', 0)
          .transition().duration(300).attr('opacity', 1)
          .transition().delay(1500).duration(400).attr('opacity', 0);
      }, voteDelay + voters.length * 120 + 300));
      if (tick < 80) timers.push(setTimeout(anim, voteDelay + voters.length * 120 + 2200));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Same task to N agents, majority decision wins');
  }

  function drawAuction(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 60;
    var auctioneer = {x: cx, y: ty};
    var bidders = [];
    for (var i = 0; i < 6; i++) {
      var a = Math.PI * 0.15 + Math.PI * 0.7 * i / 5;
      bidders.push({x: cx + Math.cos(a) * 80, y: ty + 15 + Math.sin(a) * 70});
    }
    bidders.forEach(function(b, i) {
      dld(g, auctioneer.x, auctioneer.y, b.x, b.y, 200 + i * 80);
    });
    dn(g, auctioneer.x, auctioneer.y, 11, '#7f1d1d', 'Auctioneer');
    var bidLabels = [];
    bidders.forEach(function(b, i) {
      dn(g, b.x, b.y, 7, '#6b7280', 'Agent ' + (i+1));
      bidLabels.push(g.append('text').attr('x', b.x).attr('y', b.y - 14)
        .attr('text-anchor', 'middle').attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', '#aaa').text(''));
    });
    var tick = 0;
    function anim() {
      tick++;
      bidders.forEach(function(b, i) { pkt(g, [auctioneer, b], '#f59e0b', i * 50, 2.5); });
      var bidDelay = bidders.length * 50 + 500;
      var bids = bidders.map(function() { return Math.floor(Math.random() * 90) + 10; });
      var winner = bids.indexOf(Math.max.apply(null, bids));
      bidders.forEach(function(b, i) {
        (function(idx, bid) {
          timers.push(setTimeout(function() {
            bidLabels[idx].text(bid).attr('fill', '#d97706')
              .transition().duration(200).attr('fill', idx === winner ? '#059669' : '#ccc');
          }, bidDelay + idx * 100));
          pkt(g, [b, auctioneer], idx === winner ? '#059669' : '#ddd', bidDelay + idx * 100, 2.5);
        })(i, bids[i]);
      });
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', bidders[winner].x).attr('cy', bidders[winner].y).attr('r', 7)
          .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2.5).attr('opacity', 0.8)
          .transition().duration(500).attr('r', 18).attr('opacity', 0).remove();
      }, bidDelay + bidders.length * 100 + 300));
      if (tick < 80) timers.push(setTimeout(anim, bidDelay + bidders.length * 100 + 1400));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Task broadcast, agents bid, highest score wins');
  }

  function drawRelay(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var a = {x: cx - 110, y: by}, b = {x: cx, y: by}, c = {x: cx + 110, y: by};
    dl(g, a.x, a.y, b.x, b.y, 0);
    dl(g, b.x, b.y, c.x, c.y, 200);
    var nA = dn(g, a.x, a.y, 11, '#2563eb', 'Agent A');
    var nB = dn(g, b.x, b.y, 11, '#059669', 'Agent B');
    var nC = dn(g, c.x, c.y, 11, '#7c3aed', 'Agent C');
    g.append('text').attr('x', cx - 55).attr('y', by - 20)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#f59e0b').attr('opacity', 0)
      .text('context').transition().delay(400).duration(200).attr('opacity', 1);
    var tick = 0;
    function anim() {
      tick++;
      pkt(g, [a, b], '#f59e0b', 0, 5);
      timers.push(setTimeout(function() {
        g.selectAll('circle').filter(function() {
          return +d3.select(this).attr('cx') === a.x && +d3.select(this).attr('cy') === a.y;
        }).transition().duration(400).attr('opacity', 0.1);
      }, 700));
      pkt(g, [b, c], '#f59e0b', 900, 5);
      timers.push(setTimeout(function() {
        g.selectAll('circle').filter(function() {
          return +d3.select(this).attr('cx') === b.x && +d3.select(this).attr('cy') === b.y;
        }).transition().duration(400).attr('opacity', 0.1);
      }, 1600));
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', c.x).attr('cy', c.y).attr('r', 18)
          .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
          .transition().duration(600).attr('r', 25).attr('opacity', 0);
      }, 1800));
      if (tick < 80) timers.push(setTimeout(function() {
        g.selectAll('circle').transition().duration(200).attr('opacity', 0.85);
        anim();
      }, 3200));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Control transfers fully: caller exits after handoff');
  }

  function drawNegotiation(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 95;
    var agentA = {x: cx - 90, y: by}, agentB = {x: cx + 90, y: by};
    dl(g, agentA.x, agentA.y, agentB.x, agentB.y, 0);
    dn(g, agentA.x, agentA.y, 12, '#1e40af', 'Buyer Agent');
    dn(g, agentB.x, agentB.y, 12, '#b91c1c', 'Seller Agent');
    dn(g, cx, by - 50, 7, '#6b7280', 'Witness', by - 38);
    dld(g, cx, by - 43, agentA.x + 12, agentA.y - 10, 200);
    dld(g, cx, by - 43, agentB.x - 12, agentB.y - 10, 200);
    var tick = 0;
    function anim() {
      tick++;
      var d1 = pkt(g, [agentA, {x: cx, y: by}, agentB], '#2563eb', 0, 3);
      pkt(g, [agentB, {x: cx, y: by}, agentA], '#dc2626', d1 + 400, 3);
      if (tick < 80) timers.push(setTimeout(anim, d1 + 1800));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Offer and counter-offer until agreement or impasse');
  }

  function drawDebate(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var debA = {x: cx - 80, y: by + 20}, debB = {x: cx + 80, y: by + 20};
    var judge = {x: cx, y: by - 40};
    dl(g, debA.x, debA.y, debB.x, debB.y, 0);
    dld(g, judge.x, judge.y + 7, debA.x + 10, debA.y - 10, 100);
    dld(g, judge.x, judge.y + 7, debB.x - 10, debB.y - 10, 100);
    dn(g, debA.x, debA.y, 10, '#2563eb', 'Pro');
    dn(g, debB.x, debB.y, 10, '#dc2626', 'Con');
    dn(g, judge.x, judge.y, 12, '#6d28d9', 'Judge');
    var tick = 0;
    function anim() {
      tick++;
      var d1 = pkt(g, [debA, {x: cx, y: by + 20}, debB], '#2563eb', 0, 3);
      pkt(g, [debB, {x: cx, y: by + 20}, debA], '#dc2626', d1 + 300, 3);
      pkt(g, [debA, judge], '#2563eb', d1 + 900, 2);
      pkt(g, [debB, judge], '#dc2626', d1 + 1100, 2);
      if (tick < 80) timers.push(setTimeout(anim, d1 + 2500));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Multi-round argument; judge evaluates both sides');
  }

  function drawCritic(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var gen = {x: cx - 80, y: by}, crit = {x: cx + 80, y: by};
    dl(g, gen.x, gen.y, crit.x, crit.y - 8, 0);
    dld(g, crit.x, crit.y + 8, gen.x, gen.y + 8, 200);
    dn(g, gen.x, gen.y, 12, '#2563eb', 'Generator');
    dn(g, crit.x, crit.y, 12, '#dc2626', 'Critic');
    g.append('text').attr('x', cx).attr('y', by - 22)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#666').attr('opacity', 0)
      .text('output').transition().delay(400).duration(200).attr('opacity', 1);
    g.append('text').attr('x', cx).attr('y', by + 30)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#dc2626').attr('opacity', 0)
      .text('attack').transition().delay(600).duration(200).attr('opacity', 1);
    var tick = 0;
    function anim() {
      tick++;
      var round = (tick - 1) % 3;
      pkt(g, [gen, crit], '#2563eb', 0, 3);
      if (round < 2) {
        timers.push(setTimeout(function() {
          var xMark = g.append('text').attr('x', crit.x + 20).attr('y', crit.y + 5)
            .attr('text-anchor', 'middle').attr('font-size', '16px').attr('fill', '#dc2626').attr('font-weight', 'bold')
            .text('\u2717');
          xMark.transition().delay(500).duration(300).attr('opacity', 0).remove();
        }, 600));
        pkt(g, [crit, gen], '#dc2626', 800, 3);
      } else {
        timers.push(setTimeout(function() {
          var check = g.append('text').attr('x', crit.x + 20).attr('y', crit.y + 5)
            .attr('text-anchor', 'middle').attr('font-size', '16px').attr('fill', '#059669').attr('font-weight', 'bold')
            .text('\u2713');
          check.transition().delay(800).duration(300).attr('opacity', 0).remove();
        }, 600));
      }
      if (tick < 80) timers.push(setTimeout(anim, 1600));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Adversarial verification: output must survive attack');
  }

  function drawSpeculative(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var disp = {x: cx, y: ty};
    var workers = [
      {x: cx - 95, y: ty + 85},
      {x: cx, y: ty + 95},
      {x: cx + 95, y: ty + 85}
    ];
    workers.forEach(function(w) { dl(g, disp.x, disp.y, w.x, w.y, 0); });
    dn(g, disp.x, disp.y, 11, '#7f1d1d', 'Dispatch');
    workers.forEach(function(w, i) { dn(g, w.x, w.y, 8, '#6b7280', 'W' + (i + 1)); });
    var tick = 0;
    function anim() {
      tick++;
      workers.forEach(function(w, i) {
        pkt(g, [disp, w], taskColor, i * 30, 2.5);
      });
      var winner = tick % 3;
      timers.push(setTimeout(function() {
        workers.forEach(function(w, i) {
          if (i === winner) {
            g.append('circle').attr('cx', w.x).attr('cy', w.y).attr('r', 14)
              .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.9)
              .transition().duration(500).attr('r', 20).attr('opacity', 0);
            var ck = g.append('text').attr('x', w.x + 16).attr('y', w.y + 4)
              .attr('text-anchor', 'middle').attr('font-size', '12px').attr('fill', '#059669').attr('font-weight', 'bold')
              .text('\u2713');
            ck.transition().delay(800).duration(300).attr('opacity', 0).remove();
          } else {
            var xm = g.append('text').attr('x', w.x + 14).attr('y', w.y + 4)
              .attr('text-anchor', 'middle').attr('font-size', '11px').attr('fill', '#ef4444').attr('font-weight', 'bold')
              .text('\u2717');
            xm.transition().delay(600).duration(300).attr('opacity', 0).remove();
          }
        });
        pkt(g, [workers[winner], disp], resultColor, 300, 3);
      }, 900));
      if (tick < 80) timers.push(setTimeout(anim, 2800));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Race to first valid result: losers discarded');
  }

  function drawTeacherStudent(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var teacher = {x: cx - 90, y: by}, student = {x: cx + 90, y: by};
    dl(g, teacher.x, teacher.y, student.x, student.y, 0);
    dn(g, teacher.x, teacher.y, 16, '#1e40af', 'Teacher');
    dn(g, student.x, student.y, 8, '#93c5fd', 'Student');
    var tick = 0, studentR = 8;
    var blues = ['#93c5fd', '#60a5fa', '#3b82f6', '#2563eb'];
    function anim() {
      tick++;
      var round = (tick - 1) % 4;
      pkt(g, [teacher, student], '#3b82f6', 0, 3.5);
      timers.push(setTimeout(function() {
        if (round < 3) {
          studentR = Math.min(studentR + 1.5, 14);
          g.selectAll('circle').filter(function() {
            return +d3.select(this).attr('cx') === student.x && +d3.select(this).attr('cy') === student.y
              && d3.select(this).attr('stroke') === 'white';
          }).transition().duration(400).attr('r', studentR).attr('fill', blues[Math.min(round + 1, 3)]);
        } else {
          g.append('circle').attr('cx', student.x).attr('cy', student.y).attr('r', studentR + 2)
            .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
            .transition().duration(500).attr('r', studentR + 10).attr('opacity', 0);
          studentR = 8;
          g.selectAll('circle').filter(function() {
            return +d3.select(this).attr('cx') === student.x && +d3.select(this).attr('cy') === student.y
              && d3.select(this).attr('stroke') === 'white';
          }).transition().duration(300).attr('r', 8).attr('fill', '#93c5fd');
        }
      }, 700));
      if (tick < 80) timers.push(setTimeout(anim, 1400));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Knowledge transfer from strong to weak model');
  }

  function drawCheckpoint(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 105;
    var steps = [
      {x: cx - 120, y: by}, {x: cx - 40, y: by},
      {x: cx + 40, y: by}, {x: cx + 120, y: by}
    ];
    var cpX1 = (steps[0].x + steps[1].x) / 2, cpX2 = (steps[1].x + steps[2].x) / 2;
    for (var i = 0; i < steps.length - 1; i++) dl(g, steps[i].x, steps[i].y, steps[i+1].x, steps[i+1].y, i * 150);
    steps.forEach(function(s, i) { dn(g, s.x, s.y, 7, '#4ade80', 'S' + (i + 1)); });
    [cpX1, cpX2].forEach(function(cx2, i) {
      g.append('polygon').attr('points',
        cx2 + ',' + (by - 8) + ' ' + (cx2 + 6) + ',' + by + ' ' + cx2 + ',' + (by + 8) + ' ' + (cx2 - 6) + ',' + by)
        .attr('fill', '#f59e0b').attr('opacity', 0.7).attr('stroke', '#d97706').attr('stroke-width', 0.5);
      g.append('text').attr('x', cx2).attr('y', by - 14)
        .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#d97706').text('CP' + (i + 1));
    });
    var tick = 0;
    function anim() {
      tick++;
      pkt(g, [steps[0], steps[1]], taskColor, 0, 2.5);
      pkt(g, [steps[1], steps[2]], taskColor, 500, 2.5);
      timers.push(setTimeout(function() {
        var fail = g.append('text').attr('x', steps[2].x).attr('y', steps[2].y - 15)
          .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#dc2626').attr('font-weight', 'bold')
          .text('FAIL');
        fail.transition().delay(600).duration(300).attr('opacity', 0).remove();
        g.append('circle').attr('cx', steps[2].x).attr('cy', steps[2].y).attr('r', 10)
          .attr('fill', 'none').attr('stroke', '#dc2626').attr('stroke-width', 2).attr('opacity', 0.8)
          .transition().duration(400).attr('r', 16).attr('opacity', 0);
      }, 1100));
      pkt(g, [steps[2], steps[1]], '#f59e0b', 1500, 3);
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', cpX2).attr('cy', by).attr('r', 10)
          .attr('fill', 'none').attr('stroke', '#f59e0b').attr('stroke-width', 2).attr('opacity', 0.7)
          .transition().duration(400).attr('r', 16).attr('opacity', 0);
      }, 2000));
      pkt(g, [steps[1], steps[2]], taskColor, 2500, 2.5);
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', steps[2].x).attr('cy', steps[2].y).attr('r', 10)
          .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
          .transition().duration(400).attr('r', 16).attr('opacity', 0);
      }, 3200));
      pkt(g, [steps[2], steps[3]], taskColor, 3400, 2.5);
      if (tick < 80) timers.push(setTimeout(anim, 5000));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Rollback to last checkpoint on failure');
  }

  function drawOrchestrator(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var orch = {x: cx, y: ty};
    var workerSlots = [
      {x: cx-100, y: ty+70}, {x: cx-50, y: ty+90}, {x: cx, y: ty+80},
      {x: cx+50, y: ty+90}, {x: cx+100, y: ty+70}
    ];
    dn(g, orch.x, orch.y, 13, '#7f1d1d', 'Orchestrator');
    var planLabel = g.append('text').attr('x', orch.x + 50).attr('y', orch.y - 5)
      .attr('font-size', '7px').attr('fill', '#b91c1c').attr('font-style', 'italic').text('');
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      var numW = 2 + Math.floor(Math.random() * 3);
      var tasks = ['search', 'code', 'review', 'test', 'deploy'];
      var plan = numW + ' sub-tasks planned';
      planLabel.text(plan).attr('opacity', 0).transition().duration(200).attr('opacity', 0.8)
        .transition().delay(800).duration(300).attr('opacity', 0);
      timers.push(setTimeout(function() {
        dynG.selectAll('*').remove();
        for (var i = 0; i < numW; i++) {
          var slot = workerSlots[i];
          var taskName = tasks[Math.floor(Math.random() * tasks.length)];
          dynG.append('line').attr('x1', orch.x).attr('y1', orch.y + 13)
            .attr('x2', orch.x).attr('y2', orch.y + 13)
            .attr('stroke', '#ddd').attr('stroke-width', 1.2)
            .transition().delay(i * 100).duration(350)
            .attr('x2', slot.x).attr('y2', slot.y - 7);
          dynG.append('circle').attr('cx', slot.x).attr('cy', slot.y).attr('r', 0)
            .attr('fill', '#ef4444').attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.85)
            .transition().delay(i * 100).duration(300).attr('r', 7);
          dynG.append('text').attr('x', slot.x).attr('y', slot.y + 18)
            .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#888').text(taskName);
          pkt(dynG, [orch, slot], taskColor, 200 + i * 150, 2.5);
          pkt(dynG, [slot, orch], resultColor, 800 + i * 150, 2);
        }
      }, 600));
      if (tick < 80) timers.push(setTimeout(anim, 3000));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Orchestrator plans dynamically, spawns workers as needed');
  }

  function drawEvaluator(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 65;
    var generator = {x: cx - 80, y: ty + 40};
    var evaluator = {x: cx + 80, y: ty + 40};
    dl(g, generator.x + 12, generator.y - 8, evaluator.x - 12, evaluator.y - 8, 300);
    dld(g, evaluator.x - 12, evaluator.y + 8, generator.x + 12, generator.y + 8, 500);
    dn(g, generator.x, generator.y, 12, '#2563eb', 'Generator');
    dn(g, evaluator.x, evaluator.y, 12, '#d97706', 'Evaluator');
    g.append('text').attr('x', cx).attr('y', ty + 22).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').text('output');
    g.append('text').attr('x', cx).attr('y', ty + 66).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').attr('font-style', 'italic').text('feedback');
    var scoreLabel = g.append('text').attr('x', cx).attr('y', ty + 115)
      .attr('text-anchor', 'middle').attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', '#aaa').text('');
    var iterLabel = g.append('text').attr('x', cx).attr('y', ty + 130)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#ccc').text('');
    var tick = 0;
    function anim() {
      tick++;
      var iterations = 2 + Math.floor(Math.random() * 3);
      var score = 30 + Math.floor(Math.random() * 20);
      for (var i = 0; i < iterations; i++) {
        (function(iter, s) {
          var baseDelay = iter * 1200;
          pkt(g, [generator, evaluator], '#2563eb', baseDelay, 3);
          timers.push(setTimeout(function() {
            s = Math.min(98, s + 15 + Math.floor(Math.random() * 10));
            scoreLabel.text('Quality: ' + s + '%').attr('fill', s > 80 ? '#059669' : '#d97706');
            iterLabel.text('Iteration ' + (iter + 1) + '/' + iterations);
          }, baseDelay + 500));
          if (iter < iterations - 1) {
            pkt(g, [evaluator, generator], '#d97706', baseDelay + 700, 2.5);
          } else {
            timers.push(setTimeout(function() {
              g.append('circle').attr('cx', evaluator.x).attr('cy', evaluator.y).attr('r', 12)
                .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
                .transition().duration(500).attr('r', 22).attr('opacity', 0).remove();
            }, baseDelay + 800));
          }
        })(i, score);
        score = Math.min(98, score + 15 + Math.floor(Math.random() * 10));
      }
      if (tick < 80) timers.push(setTimeout(anim, iterations * 1200 + 1500));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Generate, evaluate, refine until quality threshold met');
  }

  function drawSwarm(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + cellH / 2 + 10;
    var phases = [
      {n: 1, colors: ['#dc2626'], targets: [{x: cx, y: cy}]},
      {n: 2, colors: ['#2563eb', '#d97706'], targets: [{x: cx - 40, y: cy - 10}, {x: cx + 40, y: cy + 15}]},
      {n: 3, colors: ['#059669', '#2563eb', '#d97706'], targets: [{x: cx - 50, y: cy - 20}, {x: cx + 45, y: cy - 15}, {x: cx, y: cy + 35}]}
    ];
    var agents = [];
    for (var i = 0; i < 15; i++) {
      var a = Math.random() * Math.PI * 2, r = 20 + Math.random() * 55;
      agents.push({x: cx + Math.cos(a) * r, y: cy + Math.sin(a) * r, vx: (Math.random() - 0.5) * 1.2, vy: (Math.random() - 0.5) * 1.2, cluster: 0, el: null});
    }
    var links = g.append('g');
    agents.forEach(function(ag) {
      ag.el = g.append('circle').attr('cx', ag.x).attr('cy', ag.y).attr('r', 4)
        .attr('fill', '#6b7280').attr('stroke', 'white').attr('stroke-width', 0.5).attr('opacity', 0.8);
    });
    var tick = 0;
    var cycleLen = 110;
    var disperseLen = 30;
    var totalCycle = cycleLen + disperseLen;
    function anim() {
      tick++;
      links.selectAll('line').remove();
      var globalPhase = Math.floor(tick / totalCycle) % 3;
      var localTick = tick % totalCycle;
      var dispersing = localTick >= cycleLen;
      var ph = phases[globalPhase];
      var formation = dispersing ? Math.max(0, 1 - (localTick - cycleLen) / disperseLen) : Math.min(1, localTick / 60);
      if (localTick === 0) {
        agents.forEach(function(ag, i) { ag.cluster = i % ph.n; });
      }
      var time = tick * 0.015;
      agents.forEach(function(ag, i) {
        if (dispersing) {
          ag.vx += (Math.random() - 0.5) * 0.5;
          ag.vy += (Math.random() - 0.5) * 0.5;
        } else {
          var t = ph.targets[ag.cluster];
          var goalX = t.x + Math.cos(time + ag.cluster * 2.1) * 12;
          var goalY = t.y + Math.sin(time * 0.7 + ag.cluster * 1.5) * 10;
          var orbit = (i % Math.ceil(15 / ph.n)) / Math.ceil(15 / ph.n) * Math.PI * 2 + time * (1 + ag.cluster * 0.3);
          var orbitR = (ph.n === 1 ? 22 : 14) + Math.sin(time * 0.5 + i) * 4;
          var tx = goalX + Math.cos(orbit) * orbitR;
          var ty = goalY + Math.sin(orbit) * orbitR;
          ag.vx += (tx - ag.x) * 0.025 * formation;
          ag.vy += (ty - ag.y) * 0.025 * formation;
        }
        ag.vx += (Math.random() - 0.5) * 0.15;
        ag.vy += (Math.random() - 0.5) * 0.15;
        ag.vx *= 0.91; ag.vy *= 0.91;
        ag.x += ag.vx; ag.y += ag.vy;
        if (ag.x < p.ox + 12) { ag.x = p.ox + 12; ag.vx *= -0.5; }
        if (ag.x > p.ox + cellW - 12) { ag.x = p.ox + cellW - 12; ag.vx *= -0.5; }
        if (ag.y < p.oy + 42) { ag.y = p.oy + 42; ag.vy *= -0.5; }
        if (ag.y > p.oy + cellH - 22) { ag.y = p.oy + cellH - 22; ag.vy *= -0.5; }
        ag.el.attr('cx', ag.x).attr('cy', ag.y);
        var col = ph.colors[ag.cluster];
        ag.el.attr('fill', d3.interpolate('#6b7280', col)(formation));
        if (formation > 0.3 && !dispersing) {
          agents.forEach(function(other, j) {
            if (j <= i || other.cluster !== ag.cluster) return;
            var dist = Math.sqrt(Math.pow(other.x - ag.x, 2) + Math.pow(other.y - ag.y, 2));
            if (dist < 42) {
              links.append('line').attr('x1', ag.x).attr('y1', ag.y)
                .attr('x2', other.x).attr('y2', other.y)
                .attr('stroke', col).attr('stroke-width', 0.7).attr('opacity', 0.25 * formation);
            }
          });
        }
      });
      timers.push(setTimeout(anim, 45));
    }
    timers.push(setTimeout(anim, 1000));
    anno(g, p, 'Collective behavior, no central plan');
  }

  function drawGossip(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + cellH / 2 + 5;
    var numPeers = 12;
    var peers = [];
    for (var i = 0; i < numPeers; i++) {
      var a = (i / numPeers) * Math.PI * 2;
      var r = 60 + (i % 2) * 18;
      peers.push({x: cx + Math.cos(a) * r, y: cy + Math.sin(a) * r, knows: false, el: null, neighbors: []});
    }
    peers.forEach(function(pe, i) {
      pe.el = g.append('circle').attr('cx', pe.x).attr('cy', pe.y).attr('r', 6)
        .attr('fill', '#d1d5db').attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.8);
      pe.neighbors = [((i-1)+numPeers)%numPeers, (i+1)%numPeers];
      if (i % 2 === 0) pe.neighbors.push((i+3)%numPeers);
      if (i % 3 === 0) pe.neighbors.push((i+4)%numPeers);
      pe.neighbors.forEach(function(ni) {
        if (ni > i) dl(g, pe.x, pe.y, peers[ni].x, peers[ni].y, 150);
      });
    });
    var tick = 0;
    function anim() {
      tick++;
      if (tick === 1 || peers.filter(function(pe){return pe.knows;}).length === 0) {
        var starter = Math.floor(Math.random() * numPeers);
        peers[starter].knows = true;
        peers[starter].el.transition().duration(300).attr('fill', '#f59e0b');
      }
      var newKnowers = [];
      peers.forEach(function(pe) {
        if (!pe.knows) return;
        var target = pe.neighbors[Math.floor(Math.random() * pe.neighbors.length)];
        pkt(g, [pe, peers[target]], '#f59e0b', 0, 2);
        if (!peers[target].knows) newKnowers.push(peers[target]);
      });
      timers.push(setTimeout(function() {
        newKnowers.forEach(function(nk) { nk.knows = true; nk.el.transition().duration(300).attr('fill', '#f59e0b'); });
      }, 350));
      var allKnow = peers.filter(function(pe){return pe.knows;}).length >= numPeers;
      if (allKnow) {
        timers.push(setTimeout(function() {
          peers.forEach(function(pe) { pe.knows = false; pe.el.transition().duration(300).attr('fill', '#d1d5db'); });
        }, 600));
      }
      if (tick < 80) timers.push(setTimeout(anim, 700));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Information spreads peer-to-peer like a rumor');
  }

  function drawSupervisor(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, sy = p.oy + 68;
    var wy = p.oy + 148;
    var wxs = [cx - 90, cx, cx + 90];
    var wlabels = ['Worker A', 'Worker B', 'Worker C'];
    // lines first (behind nodes)
    for (var i = 0; i < 3; i++) dl(g, cx, sy + 13, wxs[i], wy - 11, i * 120);
    // nodes on top
    dn(g, cx, sy, 14, '#7f1d1d', 'Supervisor');
    for (var i = 0; i < 3; i++) dn(g, wxs[i], wy, 10, '#475569', wlabels[i]);
    var tick = 0;
    function anim() {
      tick++;
      // dispatch tasks down
      for (var i = 0; i < 3; i++) {
        pkt(g, [{x: cx, y: sy}, {x: wxs[i], y: wy}], taskColor, 200 + i * 120, 2.5);
      }
      // workers send results back
      timers.push(setTimeout(function() {
        for (var i = 0; i < 3; i++) {
          (function(idx) {
            timers.push(setTimeout(function() {
              pkt(g, [{x: wxs[idx], y: wy}, {x: cx, y: sy}], resultColor, 0, 2.5);
            }, idx * 150));
          })(i);
        }
      }, 1100));
      // reject worker 1's result with red X
      timers.push(setTimeout(function() {
        var xMark = g.append('text').attr('x', wxs[1]).attr('y', wy - 20)
          .attr('text-anchor', 'middle').attr('font-size', '11px').attr('font-weight', 'bold')
          .attr('fill', '#dc2626').attr('opacity', 0).text('x');
        xMark.transition().duration(200).attr('opacity', 1)
          .transition().delay(600).duration(300).attr('opacity', 0).remove();
        // send feedback back down
        pkt(g, [{x: cx, y: sy}, {x: wxs[1], y: wy}], '#f59e0b', 300, 2.5);
      }, 2000));
      // worker retries with green check
      timers.push(setTimeout(function() {
        pkt(g, [{x: wxs[1], y: wy}, {x: cx, y: sy}], resultColor, 0, 3);
        var check = g.append('text').attr('x', wxs[1]).attr('y', wy - 20)
          .attr('text-anchor', 'middle').attr('font-size', '11px').attr('font-weight', 'bold')
          .attr('fill', '#4ade80').attr('opacity', 0).text('v');
        check.transition().duration(200).attr('opacity', 1)
          .transition().delay(700).duration(300).attr('opacity', 0).remove();
      }, 3100));
      if (tick < 80) timers.push(setTimeout(anim, 4200));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Frontier model reviews, rejects, re-dispatches');
  }

  function drawCircuitBreaker(p) {
    var g = svg.append('g');
    var cy = p.oy + 105;
    var caller = {x: p.ox + 60, y: cy};
    var sw = {x: p.ox + 170, y: cy};
    var svc = {x: p.ox + 285, y: cy};
    // lines first
    dl(g, caller.x + 11, cy, sw.x - 10, cy, 0);
    dl(g, sw.x + 10, cy, svc.x - 11, cy, 200);
    // nodes on top
    dn(g, caller.x, cy, 11, '#475569', 'Caller');
    var swNode = g.append('circle').attr('cx', sw.x).attr('cy', cy).attr('r', 10)
      .attr('fill', '#059669').attr('stroke', 'white').attr('stroke-width', 1.2).attr('opacity', 0.85);
    g.append('text').attr('x', sw.x).attr('y', cy + 19)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#888').text('Switch');
    dn(g, svc.x, cy, 11, '#475569', 'Service');
    var statusLabel = g.append('text').attr('x', sw.x).attr('y', cy - 18)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#aaa').text('closed');
    var tick = 0;
    var failures = 0;
    var state = 'closed';
    function anim() {
      tick++;
      if (state === 'closed') {
        pkt(g, [caller, sw, svc], taskColor, 0, 2.5);
        failures++;
        // service fails (red flash on svc)
        timers.push(setTimeout(function() {
          g.append('circle').attr('cx', svc.x).attr('cy', cy).attr('r', 11)
            .attr('fill', '#dc2626').attr('opacity', 0.5)
            .transition().duration(250).attr('opacity', 0).remove();
        }, 600));
        if (failures >= 3) {
          // circuit opens
          timers.push(setTimeout(function() {
            state = 'open';
            failures = 0;
            swNode.transition().duration(300).attr('fill', '#dc2626');
            statusLabel.text('OPEN').attr('fill', '#dc2626');
            // after pause, go half-open
            timers.push(setTimeout(function() {
              state = 'half';
              swNode.transition().duration(300).attr('fill', '#f59e0b');
              statusLabel.text('half-open').attr('fill', '#f59e0b');
            }, 1400));
          }, 700));
        }
      } else if (state === 'open') {
        // bounce packet back immediately
        pkt(g, [caller, sw, caller], '#f87171', 0, 2.5);
      } else if (state === 'half') {
        // test packet through
        pkt(g, [caller, sw, svc], taskColor, 0, 2.5);
        timers.push(setTimeout(function() {
          pkt(g, [svc, sw, caller], resultColor, 0, 2.5);
          swNode.transition().duration(300).attr('fill', '#059669');
          statusLabel.text('closed').attr('fill', '#aaa');
          state = 'closed';
        }, 700));
      }
      if (tick < 80) timers.push(setTimeout(anim, 1100));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Accumulated failures open the circuit, test before re-closing');
  }

  function drawTimeout(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    var dispatcher = {x: p.ox + 65, y: p.oy + 80};
    var worker = {x: p.ox + 265, y: p.oy + 80};
    var altWorker = {x: p.ox + 265, y: p.oy + 155};
    // lines first
    dl(g, dispatcher.x + 12, dispatcher.y, worker.x - 12, worker.y, 0);
    dl(g, dispatcher.x + 12, dispatcher.y + 4, altWorker.x - 12, altWorker.y, 600);
    // nodes on top
    dn(g, dispatcher.x, dispatcher.y, 12, '#475569', 'Dispatcher');
    dn(g, worker.x, worker.y, 11, '#475569', 'Worker');
    dn(g, altWorker.x, altWorker.y, 11, '#059669', 'Fallback');
    // clock label
    var clockLabel = g.append('text').attr('x', cx).attr('y', p.oy + 68)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#aaa').text('');
    var tick = 0;
    function anim() {
      tick++;
      // send task to worker
      pkt(g, [dispatcher, worker], taskColor, 0, 2.5);
      // clock ticks
      var ticks = [300, 600, 900];
      ticks.forEach(function(t, i) {
        timers.push(setTimeout(function() {
          clockLabel.text('... ' + (i + 1) + 's').attr('fill', i < 2 ? '#aaa' : '#dc2626');
        }, t));
      });
      // timeout fires - worker dims
      timers.push(setTimeout(function() {
        clockLabel.text('timeout!').attr('fill', '#dc2626');
        g.append('circle').attr('cx', worker.x).attr('cy', worker.y).attr('r', 11)
          .attr('fill', '#dc2626').attr('opacity', 0.4)
          .transition().duration(400).attr('opacity', 0).remove();
      }, 1100));
      // dispatch to fallback
      timers.push(setTimeout(function() {
        clockLabel.text('');
        pkt(g, [dispatcher, altWorker], taskColor, 0, 2.5);
      }, 1500));
      // fallback responds
      timers.push(setTimeout(function() {
        pkt(g, [altWorker, dispatcher], resultColor, 0, 3);
      }, 2100));
      if (tick < 80) timers.push(setTimeout(anim, 3200));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'No response triggers fallback to alternative');
  }

  function drawCanary(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    var router = {x: p.ox + 55, y: p.oy + 110};
    var incumbent = {x: p.ox + 200, y: p.oy + 75};
    var canary = {x: p.ox + 200, y: p.oy + 150};
    var judge = {x: p.ox + 295, y: p.oy + 110};
    // lines first
    dl(g, router.x + 12, router.y - 5, incumbent.x - 12, incumbent.y, 0);
    dl(g, router.x + 12, router.y + 5, canary.x - 12, canary.y, 0);
    dl(g, incumbent.x + 12, incumbent.y, judge.x - 12, judge.y - 5, 0);
    dl(g, canary.x + 12, canary.y, judge.x - 12, judge.y + 5, 0);
    // nodes on top
    dn(g, router.x, router.y, 12, '#475569', 'Router');
    dn(g, incumbent.x, incumbent.y, 11, '#2563eb', 'Incumbent');
    dn(g, canary.x, canary.y, 11, '#f59e0b', 'Canary');
    dn(g, judge.x, judge.y, 11, '#059669', 'Judge');
    var tick = 0;
    function anim() {
      tick++;
      // route to both
      pkt(g, [router, incumbent], taskColor, 0, 2.5);
      pkt(g, [router, canary], taskColor, 200, 2.5);
      // both respond to judge
      timers.push(setTimeout(function() {
        pkt(g, [incumbent, judge], resultColor, 0, 2.5);
        pkt(g, [canary, judge], '#f59e0b', 200, 2.5);
      }, 1000));
      // judge approves (green flash)
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', judge.x).attr('cy', judge.y).attr('r', 14)
          .attr('fill', '#059669').attr('opacity', 0.5)
          .transition().duration(500).attr('opacity', 0).remove();
      }, 2000));
      if (tick < 80) timers.push(setTimeout(anim, 3000));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Shadow agent tested on live traffic before promotion');
  }

  function drawWitness(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    var producer = {x: p.ox + 60, y: p.oy + 120};
    var witness = {x: cx, y: p.oy + 65};
    var consumer = {x: p.ox + 280, y: p.oy + 120};
    // lines first
    dl(g, producer.x + 11, producer.y - 4, witness.x - 10, witness.y + 7, 0);
    dl(g, witness.x + 10, witness.y + 7, consumer.x - 11, consumer.y - 4, 400);
    // nodes on top
    dn(g, producer.x, producer.y, 12, '#475569', 'Producer');
    dn(g, witness.x, witness.y, 11, '#7c3aed', 'Witness');
    dn(g, consumer.x, consumer.y, 12, '#475569', 'Consumer');
    var statusLabel = g.append('text').attr('x', cx).attr('y', p.oy + 50)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#aaa').text('');
    var tick = 0;
    function anim() {
      tick++;
      // producer sends to witness
      pkt(g, [producer, witness], taskColor, 0, 2.5);
      // witness certifies
      timers.push(setTimeout(function() {
        statusLabel.text('certified').attr('fill', '#4ade80');
        g.append('circle').attr('cx', witness.x).attr('cy', witness.y).attr('r', 11)
          .attr('fill', 'none').attr('stroke', '#4ade80').attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(400).attr('r', 20).attr('opacity', 0).remove();
      }, 650));
      // certified output to consumer
      timers.push(setTimeout(function() {
        statusLabel.text('');
        pkt(g, [witness, consumer], resultColor, 0, 3);
      }, 1100));
      // consumer receives
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', consumer.x).attr('cy', consumer.y).attr('r', 12)
          .attr('fill', '#4ade80').attr('opacity', 0.3)
          .transition().duration(400).attr('opacity', 0).remove();
      }, 1700));
      if (tick < 80) timers.push(setTimeout(anim, 2800));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Independent verification before acceptance');
  }

  function drawBlackboard(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 110;
    // blackboard rect - draw first (behind agents)
    var bbX = cx - 55, bbY = cy - 30, bbW = 110, bbH = 70;
    g.append('rect').attr('x', bbX).attr('y', bbY).attr('width', bbW).attr('height', bbH)
      .attr('rx', 4).attr('fill', '#1e293b').attr('stroke', '#334155').attr('stroke-width', 1.5);
    g.append('text').attr('x', cx).attr('y', bbY - 4)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#94a3b8').text('Blackboard');
    // agent positions around board
    var agents = [
      {x: cx - 105, y: cy - 10, label: 'Agent A', color: '#2563eb'},
      {x: cx + 105, y: cy - 10, label: 'Agent B', color: '#d97706'},
      {x: cx - 60, y: cy + 80, label: 'Agent C', color: '#7c3aed'},
      {x: cx + 60, y: cy + 80, label: 'Agent D', color: '#059669'}
    ];
    // lines from agents to board (behind agents)
    dl(g, agents[0].x + 11, agents[0].y, bbX, cy - 5, 0);
    dl(g, agents[1].x - 11, agents[1].y, bbX + bbW, cy - 5, 200);
    dl(g, agents[2].x, agents[2].y - 11, cx - 20, bbY + bbH, 400);
    dl(g, agents[3].x, agents[3].y - 11, cx + 20, bbY + bbH, 600);
    // agents on top
    for (var i = 0; i < agents.length; i++) dn(g, agents[i].x, agents[i].y, 10, agents[i].color, agents[i].label);
    var dots = [];
    var agentColors = ['#3b82f6', '#f59e0b', '#8b5cf6', '#10b981'];
    var tick = 0;
    function anim() {
      tick++;
      var seq = [0, 1, 2, 3];
      seq.forEach(function(ai, step) {
        timers.push(setTimeout(function() {
          var bx = bbX + 12 + Math.random() * (bbW - 24);
          var by2 = bbY + 10 + Math.random() * (bbH - 20);
          var dot = g.append('circle').attr('cx', bx).attr('cy', by2).attr('r', 0)
            .attr('fill', agentColors[ai]).attr('opacity', 0.85);
          dot.transition().duration(250).attr('r', 3);
          dots.push(dot);
        }, step * 600));
      });
      // final agent reads board and produces result
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', agents[3].x).attr('cy', agents[3].y).attr('r', 10)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(500).attr('r', 22).attr('opacity', 0).remove();
      }, 3000));
      // clear board after cycle
      timers.push(setTimeout(function() {
        dots.forEach(function(d) { d.transition().duration(400).attr('opacity', 0).remove(); });
        dots = [];
      }, 4000));
      if (tick < 80) timers.push(setTimeout(anim, 4800));
    }
    timers.push(setTimeout(anim, 1000));
    anno(g, p, 'Agents contribute to a shared workspace');
  }

  function drawPubSub(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 105;
    var brokerX = cx - 20, brokerY = cy - 15, brokerW = 52, brokerH = 36;
    // broker rect - draw first
    g.append('rect').attr('x', brokerX).attr('y', brokerY).attr('width', brokerW).attr('height', brokerH)
      .attr('rx', 4).attr('fill', '#334155').attr('stroke', '#475569').attr('stroke-width', 1.5);
    g.append('text').attr('x', brokerX + brokerW / 2).attr('y', brokerY + brokerH + 12)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#94a3b8').text('Broker');
    var pubs = [
      {x: p.ox + 50, y: cy - 30, color: '#2563eb', topic: 'A'},
      {x: p.ox + 50, y: cy + 30, color: '#d97706', topic: 'B'}
    ];
    var subs = [
      {x: p.ox + 275, y: cy - 50, color: '#2563eb', topic: 'A'},
      {x: p.ox + 275, y: cy, color: '#059669', topic: 'A+B'},
      {x: p.ox + 275, y: cy + 50, color: '#d97706', topic: 'B'}
    ];
    var brokerCx = brokerX + brokerW / 2, brokerCy = brokerY + brokerH / 2;
    // lines pub -> broker
    dl(g, pubs[0].x + 11, pubs[0].y, brokerX, brokerCy - 5, 0);
    dl(g, pubs[1].x + 11, pubs[1].y, brokerX, brokerCy + 5, 200);
    // lines broker -> subs
    dl(g, brokerX + brokerW, brokerCy - 5, subs[0].x - 11, subs[0].y, 400);
    dl(g, brokerX + brokerW, brokerCy, subs[1].x - 11, subs[1].y, 600);
    dl(g, brokerX + brokerW, brokerCy + 5, subs[2].x - 11, subs[2].y, 800);
    // publishers
    for (var i = 0; i < pubs.length; i++) dn(g, pubs[i].x, pubs[i].y, 10, pubs[i].color, 'Pub ' + pubs[i].topic);
    // subscribers
    for (var i = 0; i < subs.length; i++) dn(g, subs[i].x, subs[i].y, 10, '#475569', 'Sub ' + subs[i].topic);
    var tick = 0;
    function anim() {
      tick++;
      // pub A sends
      pkt(g, [pubs[0], {x: brokerCx, y: brokerCy}], '#3b82f6', 0, 2.5);
      // pub B sends
      pkt(g, [pubs[1], {x: brokerCx, y: brokerCy}], '#f59e0b', 200, 2.5);
      // broker routes topic A to sub 0 and sub 1
      timers.push(setTimeout(function() {
        pkt(g, [{x: brokerCx, y: brokerCy}, subs[0]], '#3b82f6', 0, 2.5);
        pkt(g, [{x: brokerCx, y: brokerCy}, subs[1]], '#3b82f6', 100, 2);
      }, 700));
      // broker routes topic B to sub 1 and sub 2
      timers.push(setTimeout(function() {
        pkt(g, [{x: brokerCx, y: brokerCy}, subs[1]], '#f59e0b', 0, 2);
        pkt(g, [{x: brokerCx, y: brokerCy}, subs[2]], '#f59e0b', 100, 2.5);
      }, 1000));
      if (tick < 80) timers.push(setTimeout(anim, 2200));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Publishers and subscribers fully decoupled via topics');
  }

  function drawChoreography(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 115;
    var r = 62;
    var agentLabels = ['Agent A', 'Agent B', 'Agent C', 'Agent D'];
    var agentColors = ['#2563eb', '#d97706', '#7c3aed', '#059669'];
    var positions = [];
    for (var i = 0; i < 4; i++) {
      var a = (i / 4) * Math.PI * 2 - Math.PI / 2;
      positions.push({x: cx + Math.cos(a) * r, y: cy + Math.sin(a) * r});
    }
    // ring lines (behind nodes)
    for (var i = 0; i < 4; i++) {
      dl(g, positions[i].x, positions[i].y, positions[(i + 1) % 4].x, positions[(i + 1) % 4].y, i * 100);
    }
    // nodes on top
    for (var i = 0; i < 4; i++) dn(g, positions[i].x, positions[i].y, 11, agentColors[i], agentLabels[i]);
    var tick = 0;
    function anim() {
      tick++;
      // A emits event
      pkt(g, [positions[0], positions[1]], '#3b82f6', 0, 3);
      // B reacts and emits
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', positions[1].x).attr('cy', positions[1].y).attr('r', 11)
          .attr('fill', 'none').attr('stroke', '#f59e0b').attr('stroke-width', 1.5).attr('opacity', 0.8)
          .transition().duration(300).attr('r', 18).attr('opacity', 0).remove();
        pkt(g, [positions[1], positions[2]], '#f59e0b', 100, 3);
      }, 600));
      // C reacts and emits
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', positions[2].x).attr('cy', positions[2].y).attr('r', 11)
          .attr('fill', 'none').attr('stroke', '#8b5cf6').attr('stroke-width', 1.5).attr('opacity', 0.8)
          .transition().duration(300).attr('r', 18).attr('opacity', 0).remove();
        pkt(g, [positions[2], positions[3]], '#8b5cf6', 100, 3);
      }, 1200));
      // D completes the chain
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', positions[3].x).attr('cy', positions[3].y).attr('r', 11)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(500).attr('r', 24).attr('opacity', 0).remove();
      }, 1900));
      if (tick < 80) timers.push(setTimeout(anim, 3000));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'No controller: agents react to peer events');
  }

  function drawStigmergy(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    // environment rect at bottom - draw first
    var envX = p.ox + 40, envY = p.oy + 158, envW = cellW - 80, envH = 40;
    g.append('rect').attr('x', envX).attr('y', envY).attr('width', envW).attr('height', envH)
      .attr('rx', 4).attr('fill', '#1e293b').attr('stroke', '#334155').attr('stroke-width', 1.5);
    g.append('text').attr('x', cx).attr('y', envY + envH + 12)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#94a3b8').text('Environment');
    var agentYs = p.oy + 78;
    var axs = [p.ox + 75, cx, p.ox + 265];
    var agentColors = ['#2563eb', '#d97706', '#7c3aed'];
    var agentLabels = ['Agent 1', 'Agent 2', 'Agent 3'];
    // lines agent -> env (behind agents)
    for (var i = 0; i < 3; i++) dl(g, axs[i], agentYs + 11, axs[i], envY, i * 200);
    // agents on top
    for (var i = 0; i < 3; i++) dn(g, axs[i], agentYs, 10, agentColors[i], agentLabels[i]);
    var marks = [];
    var tick = 0;
    function anim() {
      tick++;
      // agent 1 drops mark
      timers.push(setTimeout(function() {
        var m = g.append('circle').attr('cx', envX + 30).attr('cy', envY + envH / 2).attr('r', 0)
          .attr('fill', '#3b82f6').attr('opacity', 0.85);
        m.transition().duration(250).attr('r', 4);
        marks.push(m);
      }, 400));
      // agent 2 reads and drops its mark
      timers.push(setTimeout(function() {
        g.append('line').attr('x1', axs[1]).attr('y1', agentYs + 11)
          .attr('x2', axs[1]).attr('y2', agentYs + 11)
          .attr('stroke', '#f59e0b').attr('stroke-width', 1).attr('stroke-dasharray', '3,2').attr('opacity', 0.6)
          .transition().duration(300).attr('y2', envY);
        var m = g.append('circle').attr('cx', envX + envW / 2).attr('cy', envY + envH / 2).attr('r', 0)
          .attr('fill', '#f59e0b').attr('opacity', 0.85);
        m.transition().duration(250).attr('r', 4);
        marks.push(m);
      }, 1100));
      // agent 3 reads both and produces result
      timers.push(setTimeout(function() {
        g.append('line').attr('x1', axs[2]).attr('y1', agentYs + 11)
          .attr('x2', axs[2]).attr('y2', agentYs + 11)
          .attr('stroke', '#8b5cf6').attr('stroke-width', 1).attr('stroke-dasharray', '3,2').attr('opacity', 0.6)
          .transition().duration(300).attr('y2', envY);
      }, 1800));
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', axs[2]).attr('cy', agentYs).attr('r', 10)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(500).attr('r', 22).attr('opacity', 0).remove();
      }, 2400));
      // clear marks
      timers.push(setTimeout(function() {
        marks.forEach(function(m) { m.transition().duration(400).attr('opacity', 0).remove(); });
        marks = [];
      }, 3200));
      if (tick < 80) timers.push(setTimeout(anim, 4000));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Agents coordinate through environmental traces');
  }

  function drawMissionCommand(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    var cmdY = p.oy + 60;
    var subY = p.oy + 130;
    var subXs = [p.ox + 75, cx, p.ox + 265];
    var goalY = p.oy + 195;
    var trailColors = ['#2563eb', '#d97706', '#7c3aed'];
    // lines commander -> subordinates (behind nodes)
    for (var i = 0; i < 3; i++) dl(g, cx, cmdY + 13, subXs[i], subY - 11, i * 120);
    // goal marker at bottom
    g.append('rect').attr('x', cx - 18).attr('y', goalY - 8).attr('width', 36).attr('height', 16)
      .attr('rx', 3).attr('fill', '#065f46').attr('stroke', '#4ade80').attr('stroke-width', 1.5).attr('opacity', 0.85);
    g.append('text').attr('x', cx).attr('y', goalY + 4)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#4ade80').text('Goal');
    // lines subordinates -> goal (behind nodes, same color per trail)
    for (var i = 0; i < 3; i++) {
      g.append('line').attr('x1', subXs[i]).attr('y1', subY + 11).attr('x2', subXs[i]).attr('y2', subY + 11)
        .attr('stroke', trailColors[i]).attr('stroke-width', 1.2).attr('opacity', 0.35)
        .transition().delay(1200 + i * 150).duration(400).attr('x2', cx).attr('y2', goalY - 8);
    }
    // commander node
    dn(g, cx, cmdY, 13, '#7f1d1d', 'Commander');
    // intent label
    var intentLabel = g.append('text').attr('x', cx).attr('y', cmdY - 16)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#3b82f6').attr('opacity', 0).text('intent');
    intentLabel.transition().delay(200).duration(300).attr('opacity', 1)
      .transition().delay(1800).duration(400).attr('opacity', 0);
    // subordinate nodes on top
    for (var i = 0; i < 3; i++) dn(g, subXs[i], subY, 10, '#475569', 'Sub ' + (i + 1));
    var tick = 0;
    function anim() {
      tick++;
      // dispatch intent (blue packets) to all subordinates
      for (var i = 0; i < 3; i++) {
        pkt(g, [{x: cx, y: cmdY}, {x: subXs[i], y: subY}], '#3b82f6', 200 + i * 130, 3);
      }
      // each subordinate takes different colored trail to goal
      timers.push(setTimeout(function() {
        for (var i = 0; i < 3; i++) {
          (function(idx) {
            pkt(g, [{x: subXs[idx], y: subY}, {x: cx, y: goalY}], trailColors[idx], idx * 180, 2.5);
          })(i);
        }
      }, 1500));
      // goal flashes green
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', cx).attr('cy', goalY).attr('r', 10)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(500).attr('r', 26).attr('opacity', 0).remove();
      }, 2500));
      if (tick < 80) timers.push(setTimeout(anim, 3600));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Intent communicated, method left to the agent');
  }

  function drawFederatedLearning(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 110;
    var dataNodes = [];
    var angles = [-Math.PI * 2 / 3, 0, Math.PI * 2 / 3];
    var r = 72;
    for (var i = 0; i < 3; i++) {
      dataNodes.push({x: cx + Math.cos(angles[i]) * r, y: cy + Math.sin(angles[i]) * r});
    }
    var coord = {x: cx, y: cy};
    // lines coordinator <-> data nodes (behind nodes)
    for (var i = 0; i < 3; i++) {
      dl(g, coord.x, coord.y, dataNodes[i].x, dataNodes[i].y, i * 120);
    }
    // coordinator node
    dn(g, coord.x, coord.y, 14, '#7f1d1d', 'Coordinator');
    // data nodes with labels
    for (var i = 0; i < 3; i++) {
      dn(g, dataNodes[i].x, dataNodes[i].y, 11, '#475569', 'Data ' + (i + 1));
      // data label inside node
      g.append('text').attr('x', dataNodes[i].x).attr('y', dataNodes[i].y + 3)
        .attr('text-anchor', 'middle').attr('font-size', '5px').attr('fill', 'white').attr('opacity', 0.7)
        .text('data');
    }
    var tick = 0;
    function anim() {
      tick++;
      // data nodes send update packets to coordinator (blue, not raw data)
      for (var i = 0; i < 3; i++) {
        pkt(g, [dataNodes[i], coord], '#3b82f6', i * 200, 2.5);
      }
      // coordinator aggregates and sends model back (green)
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', coord.x).attr('cy', coord.y).attr('r', 14)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.85)
          .transition().duration(400).attr('r', 22).attr('opacity', 0).remove();
        for (var i = 0; i < 3; i++) {
          (function(idx) {
            pkt(g, [coord, dataNodes[idx]], resultColor, idx * 180, 3);
          })(i);
        }
      }, 1300));
      if (tick < 80) timers.push(setTimeout(anim, 2800));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Agents share updates, never raw data');
  }

  function drawLoop(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW/2, cy = p.oy + 45 + (cellH-45)/2;
    var ax = p.ox + 100, ay = cy;
    var tx = p.ox + 250, ty = cy;
    dl(g, ax, ay, tx, ty, 0);
    dn(g, ax, ay, 22, '#3b82f6', 'agent');
    dn(g, tx, ty, 18, '#64748b', 'target');
    var tick = 0;
    function run() {
      tick++;
      var lbl = g.append('text').attr('x', cx).attr('y', p.oy + 70).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',11).text('attempt');
      pkt(g, [{x:ax,y:ay},{x:tx,y:ty}], '#dc2626', 200, 5);
      timers.push(setTimeout(function() {
        lbl.text('error');
        pkt(g, [{x:tx,y:ty},{x:ax,y:ay}], '#dc2626', 0, 5);
      }, 1000));
      timers.push(setTimeout(function() {
        lbl.text('retry');
        pkt(g, [{x:ax,y:ay},{x:tx,y:ty}], '#dc2626', 0, 5);
      }, 2000));
      timers.push(setTimeout(function() {
        lbl.text('success');
        pkt(g, [{x:tx,y:ty},{x:ax,y:ay}], '#4ade80', 0, 5);
      }, 3000));
      timers.push(setTimeout(function() { lbl.remove(); if (tick < 80) run(); }, 4500));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Self-feedback loop until success');
  }

  function drawFeudal(p) {
    var g = svg.append('g');
    var px2 = p.ox + 170, py2 = p.oy + 70;
    var m1x = p.ox + 90, m1y = p.oy + 130;
    var m2x = p.ox + 250, m2y = p.oy + 130;
    var l1x = p.ox + 55, l1y = p.oy + 195;
    var l2x = p.ox + 130, l2y = p.oy + 195;
    var l3x = p.ox + 215, l3y = p.oy + 195;
    var l4x = p.ox + 285, l4y = p.oy + 195;
    dl(g, px2, py2, m1x, m1y, 0);
    dl(g, px2, py2, m2x, m2y, 0);
    dld(g, m1x, m1y, l1x, l1y, 0);
    dld(g, m1x, m1y, l2x, l2y, 0);
    dld(g, m2x, m2y, l3x, l3y, 0);
    dld(g, m2x, m2y, l4x, l4y, 0);
    dn(g, px2, py2, 20, '#3b82f6', 'principal');
    dn(g, m1x, m1y, 16, '#a855f7', 'mid-1');
    dn(g, m2x, m2y, 16, '#a855f7', 'mid-2');
    dn(g, l1x, l1y, 11, '#64748b', '');
    dn(g, l2x, l2y, 11, '#64748b', '');
    dn(g, l3x, l3y, 11, '#64748b', '');
    dn(g, l4x, l4y, 11, '#64748b', '');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:px2,y:py2},{x:m1x,y:m1y}], '#dc2626', 100, 5);
      pkt(g, [{x:px2,y:py2},{x:m2x,y:m2y}], '#dc2626', 300, 5);
      timers.push(setTimeout(function() {
        pkt(g, [{x:m1x,y:m1y},{x:l1x,y:l1y}], '#dc2626', 0, 4);
        pkt(g, [{x:m2x,y:m2y},{x:l3x,y:l3y}], '#dc2626', 100, 4);
      }, 900));
      timers.push(setTimeout(function() {
        pkt(g, [{x:l1x,y:l1y},{x:m1x,y:m1y}], '#4ade80', 0, 4);
        pkt(g, [{x:l3x,y:l3y},{x:m2x,y:m2y}], '#4ade80', 100, 4);
      }, 1800));
      timers.push(setTimeout(function() {
        pkt(g, [{x:m1x,y:m1y},{x:px2,y:py2}], '#4ade80', 0, 5);
        pkt(g, [{x:m2x,y:m2y},{x:px2,y:py2}], '#4ade80', 200, 5);
      }, 2700));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 4200));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Goals cascade down, details stay opaque');
  }

  function drawVerificationGame(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW/2, cy = p.oy + 45 + (cellH-45)/2;
    var lx = p.ox + 90, ly = cy;
    var rx = p.ox + 250, ry = cy;
    dl(g, lx, ly, rx, ry, 0);
    dn(g, lx, ly, 22, '#3b82f6', 'prover');
    dn(g, rx, ry, 22, '#a855f7', 'verifier');
    var tick = 0;
    function run() {
      tick++;
      var lbl = g.append('text').attr('x', cx).attr('y', p.oy + 70).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',11).text('claim');
      pkt(g, [{x:lx,y:ly},{x:rx,y:ry}], '#dc2626', 100, 5);
      timers.push(setTimeout(function() {
        lbl.text('challenge');
        pkt(g, [{x:rx,y:ry},{x:lx,y:ly}], '#f97316', 0, 5);
      }, 1000));
      timers.push(setTimeout(function() {
        lbl.text('proof');
        pkt(g, [{x:lx,y:ly},{x:rx,y:ry}], '#3b82f6', 0, 5);
      }, 2000));
      timers.push(setTimeout(function() {
        lbl.text('verified');
        pkt(g, [{x:rx,y:ry},{x:lx,y:ly}], '#4ade80', 0, 5);
      }, 3000));
      timers.push(setTimeout(function() { lbl.remove(); if (tick < 80) run(); }, 4200));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Interactive proof: claim, challenge, respond');
  }

  function drawContractNet(p) {
    var g = svg.append('g');
    var mx = p.ox + 170, my = p.oy + 80;
    var w1x = p.ox + 70, w1y = p.oy + 185;
    var w2x = p.ox + 170, w2y = p.oy + 195;
    var w3x = p.ox + 270, w3y = p.oy + 185;
    dl(g, mx, my, w1x, w1y, 0);
    dl(g, mx, my, w2x, w2y, 0);
    dl(g, mx, my, w3x, w3y, 0);
    dn(g, mx, my, 20, '#3b82f6', 'manager');
    dn(g, w1x, w1y, 15, '#64748b', 'w1');
    dn(g, w2x, w2y, 15, '#64748b', 'w2');
    dn(g, w3x, w3y, 15, '#64748b', 'w3');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:mx,y:my},{x:w1x,y:w1y}], '#dc2626', 100, 4);
      pkt(g, [{x:mx,y:my},{x:w2x,y:w2y}], '#dc2626', 250, 4);
      pkt(g, [{x:mx,y:my},{x:w3x,y:w3y}], '#dc2626', 400, 4);
      timers.push(setTimeout(function() {
        pkt(g, [{x:w1x,y:w1y},{x:mx,y:my}], '#f97316', 0, 4);
        pkt(g, [{x:w2x,y:w2y},{x:mx,y:my}], '#f97316', 150, 4);
        pkt(g, [{x:w3x,y:w3y},{x:mx,y:my}], '#f97316', 300, 4);
      }, 1400));
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx',w2x).attr('cy',w2y).attr('r',18).attr('fill','none')
          .attr('stroke','#facc15').attr('stroke-width',2.5).attr('opacity',0.9)
          .transition().duration(600).attr('opacity',0).remove();
        pkt(g, [{x:mx,y:my},{x:w2x,y:w2y}], '#4ade80', 0, 5);
      }, 2500));
      timers.push(setTimeout(function() {
        pkt(g, [{x:w2x,y:w2y},{x:mx,y:my}], '#4ade80', 0, 5);
      }, 3400));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 4800));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Broadcast task, collect bids, award winner');
  }

  function drawDistillation(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var tx2 = p.ox + 85, ty2 = cy;
    var sx = p.ox + 255, sy = cy;
    dl(g, tx2, ty2, sx, sy, 0);
    dn(g, tx2, ty2, 30, '#3b82f6', 'teacher');
    dn(g, sx, sy, 16, '#a855f7', 'student');
    var tick = 0;
    function run() {
      tick++;
      var taught = 0;
      function sendExample() {
        if (taught >= 3) {
          timers.push(setTimeout(function() {
            var xl1 = g.append('line').attr('x1', p.ox+140).attr('y1', cy-12).attr('x2', p.ox+200).attr('y2', cy+12)
              .attr('stroke','#dc2626').attr('stroke-width',2).attr('opacity',0);
            xl1.transition().duration(300).attr('opacity',1);
            var xl2 = g.append('line').attr('x1', p.ox+140).attr('y1', cy+12).attr('x2', p.ox+200).attr('y2', cy-12)
              .attr('stroke','#dc2626').attr('stroke-width',2).attr('opacity',0);
            xl2.transition().duration(300).attr('opacity',1);
            timers.push(setTimeout(function() {
              pkt(g, [{x:sx,y:sy},{x:p.ox+310,y:sy}], '#4ade80', 0, 5);
              timers.push(setTimeout(function() {
                xl1.remove(); xl2.remove();
                if (tick < 80) run();
              }, 1500));
            }, 700));
          }, 300));
          return;
        }
        pkt(g, [{x:tx2,y:ty2},{x:sx,y:sy}], '#dc2626', 0, 4);
        taught++;
        timers.push(setTimeout(sendExample, 700));
      }
      sendExample();
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Learn from large model, then operate alone');
  }

  function drawPrivilege(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var topx = p.ox + 170, topy = p.oy + 75;
    var midx = p.ox + 170, midy = p.oy + 140;
    var botx = p.ox + 170, boty = p.oy + 205;
    dl(g, topx, topy, midx, midy, 0);
    dl(g, midx, midy, botx, boty, 0);
    dn(g, topx, topy, 22, '#3b82f6', 'root');
    dn(g, midx, midy, 17, '#a855f7', 'delegate');
    dn(g, botx, boty, 12, '#64748b', 'leaf');
    var dots = ['#3b82f6','#a855f7','#f97316'];
    dots.forEach(function(c,i) { g.append('circle').attr('cx', topx+28+i*10).attr('cy', topy).attr('r',4).attr('fill',c); });
    dots.slice(0,2).forEach(function(c,i) { g.append('circle').attr('cx', midx+22+i*10).attr('cy', midy).attr('r',4).attr('fill',c); });
    dots.slice(0,1).forEach(function(c,i) { g.append('circle').attr('cx', botx+16+i*10).attr('cy', boty).attr('r',4).attr('fill',c); });
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:midx,y:midy},{x:botx,y:boty}], '#dc2626', 300, 5);
      timers.push(setTimeout(function() {
        var xl = g.append('text').attr('x', botx+20).attr('y', boty+4).attr('fill','#dc2626').attr('font-size',14).attr('font-weight','bold').text('✕');
        timers.push(setTimeout(function() { xl.remove(); if (tick < 80) run(); }, 1200));
      }, 1200));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Permissions narrow at each delegation level');
  }

  function drawFirebreaks(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var n1x = p.ox + 60, n2x = p.ox + 170, n3x = p.ox + 280;
    var fb1x = p.ox + 115, fb2x = p.ox + 225;
    dl(g, n1x, cy, n2x, cy, 0);
    dl(g, n2x, cy, n3x, cy, 0);
    dn(g, n1x, cy, 18, '#3b82f6', 'A');
    dn(g, n2x, cy, 18, '#a855f7', 'B');
    dn(g, n3x, cy, 18, '#f97316', 'C');
    g.append('line').attr('x1',fb1x).attr('y1',p.oy+55).attr('x2',fb1x).attr('y2',p.oy+220)
      .attr('stroke','#94a3b8').attr('stroke-width',1.5).attr('stroke-dasharray','5,4').attr('opacity',0.7);
    g.append('line').attr('x1',fb2x).attr('y1',p.oy+55).attr('x2',fb2x).attr('y2',p.oy+220)
      .attr('stroke','#94a3b8').attr('stroke-width',1.5).attr('stroke-dasharray','5,4').attr('opacity',0.7);
    g.append('text').attr('x',fb1x).attr('y',p.oy+62).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('boundary');
    g.append('text').attr('x',fb2x).attr('y',p.oy+62).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('boundary');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:n1x,y:cy},{x:fb1x,y:cy}], '#dc2626', 100, 5);
      timers.push(setTimeout(function() {
        pkt(g, [{x:fb1x,y:cy},{x:n2x,y:cy}], '#f97316', 0, 5);
      }, 700));
      timers.push(setTimeout(function() {
        pkt(g, [{x:n2x,y:cy},{x:fb2x,y:cy}], '#f97316', 0, 5);
      }, 1300));
      timers.push(setTimeout(function() {
        pkt(g, [{x:fb2x,y:cy},{x:n3x,y:cy}], '#facc15', 0, 5);
      }, 1900));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 3200));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Packets change at explicit trust boundaries');
  }

  function drawCredentials(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var ax = p.ox + 85, vx = p.ox + 255;
    dn(g, ax, cy, 20, '#3b82f6', 'agent');
    dn(g, vx, cy, 20, '#a855f7', 'verifier');
    var tick = 0;
    function run() {
      tick++;
      var badge = g.append('rect').attr('x', ax+22).attr('y', cy-6).attr('width',16).attr('height',12)
        .attr('fill','#facc15').attr('rx',2).attr('opacity',1);
      badge.transition().duration(800).attr('x', vx-30);
      timers.push(setTimeout(function() {
        badge.remove();
        g.append('circle').attr('cx',vx).attr('cy',cy).attr('r',24).attr('fill','none')
          .attr('stroke','#4ade80').attr('stroke-width',2.5).attr('opacity',1)
          .transition().duration(500).attr('r',30).attr('opacity',0).remove();
        var acc = g.insert('line', ':first-child').attr('x1',ax).attr('y1',cy).attr('x2',vx).attr('y2',cy)
          .attr('stroke','#4ade80').attr('stroke-width',2).attr('opacity',0);
        acc.transition().duration(400).attr('opacity',1);
        timers.push(setTimeout(function() {
          acc.remove();
          if (tick < 80) run();
        }, 1500));
      }, 850));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Present credential, verifier grants access');
  }

  function drawZoneOfIndifference(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var ax = p.ox + 170;
    g.append('rect').attr('x', ax-45).attr('y', cy-40).attr('width',90).attr('height',80)
      .attr('fill','#3b82f6').attr('opacity',0.08).attr('rx',8);
    g.append('rect').attr('x', ax-45).attr('y', cy-40).attr('width',90).attr('height',80)
      .attr('fill','none').attr('stroke','#3b82f6').attr('stroke-dasharray','4,3').attr('stroke-width',1.2).attr('rx',8).attr('opacity',0.4);
    g.append('text').attr('x',ax).attr('y',cy-46).attr('text-anchor','middle').attr('fill','#3b82f6').attr('font-size',9).attr('opacity',0.7).text('zone of indifference');
    dn(g, ax, cy, 18, '#3b82f6', 'agent');
    var srcx = p.ox + 30, outx = p.ox + 310, abovey = p.oy + 85;
    var tick = 0;
    function run() {
      tick++;
      var lbl = g.append('text').attr('x',p.ox+cellW/2).attr('y',p.oy+68).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',11).text('inside zone');
      pkt(g, [{x:srcx,y:cy},{x:ax,y:cy}], '#dc2626', 100, 5);
      timers.push(setTimeout(function() {
        pkt(g, [{x:ax,y:cy},{x:outx,y:cy}], '#4ade80', 0, 5);
      }, 1100));
      timers.push(setTimeout(function() {
        lbl.text('outside zone');
        pkt(g, [{x:srcx,y:abovey},{x:ax,y:abovey}], '#dc2626', 0, 5);
      }, 2100));
      timers.push(setTimeout(function() {
        var q = g.append('text').attr('x',ax+22).attr('y',abovey+4).attr('fill','#f97316').attr('font-size',15).attr('font-weight','bold').text('?');
        timers.push(setTimeout(function() { q.remove(); lbl.remove(); if (tick < 80) run(); }, 1200));
      }, 2900));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Act freely within bounds, escalate outside');
  }

  function drawHumanGate(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var ax = p.ox + 55, qx = p.ox + 155, hx = p.ox + 260, ex = p.ox + 320;
    dn(g, ax, cy, 18, '#3b82f6', 'agent');
    g.append('rect').attr('x',qx-20).attr('y',cy-16).attr('width',40).attr('height',32)
      .attr('fill','#1e293b').attr('stroke','#64748b').attr('stroke-width',1.5).attr('rx',4);
    g.append('text').attr('x',qx).attr('y',cy+4).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',10).text('queue');
    g.append('circle').attr('cx',hx).attr('cy',cy-28).attr('r',9).attr('fill','#f97316');
    g.append('line').attr('x1',hx).attr('y1',cy-19).attr('x2',hx).attr('y2',cy+10).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx-10).attr('y1',cy-8).attr('x2',hx+10).attr('y2',cy-8).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx).attr('y1',cy+10).attr('x2',hx-8).attr('y2',cy+24).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx).attr('y1',cy+10).attr('x2',hx+8).attr('y2',cy+24).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('text').attr('x',hx).attr('y',cy+40).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('human');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:ax,y:cy},{x:qx,y:cy}], '#dc2626', 100, 5);
      timers.push(setTimeout(function() {
        var chk = g.append('text').attr('x',hx).attr('y',cy-42).attr('text-anchor','middle').attr('fill','#4ade80').attr('font-size',13).text('✓');
        timers.push(setTimeout(function() {
          chk.remove();
          pkt(g, [{x:qx,y:cy},{x:ex,y:cy}], '#4ade80', 100, 5);
        }, 600));
      }, 900));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 2600));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Human must approve before task proceeds');
  }

  function drawHumanOnLoop(p) {
    var g = svg.append('g');
    var a1x = p.ox + 95, a1y = p.oy + 110;
    var a2x = p.ox + 245, a2y = p.oy + 110;
    var hx = p.ox + 170, hy = p.oy + 195;
    dl(g, a1x, a1y, a2x, a2y, 0);
    dn(g, a1x, a1y, 18, '#3b82f6', 'A');
    dn(g, a2x, a2y, 18, '#a855f7', 'B');
    var head = g.append('circle').attr('cx',hx).attr('cy',hy-12).attr('r',8).attr('fill','#f97316');
    g.append('line').attr('x1',hx).attr('y1',hy-4).attr('x2',hx).attr('y2',hy+14).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx-9).attr('y1',hy+4).attr('x2',hx+9).attr('y2',hy+4).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('text').attr('x',hx).attr('y',hy+30).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('human');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:a1x,y:a1y},{x:a2x,y:a2y}], '#3b82f6', 0, 4);
      timers.push(setTimeout(function() {
        pkt(g, [{x:a2x,y:a2y},{x:a1x,y:a1y}], '#a855f7', 0, 4);
      }, 600));
      timers.push(setTimeout(function() {
        pkt(g, [{x:a1x,y:a1y},{x:a2x,y:a2y}], '#3b82f6', 0, 4);
      }, 1200));
      timers.push(setTimeout(function() {
        var alert = g.append('text').attr('x',a2x).attr('y',a2y-28).attr('text-anchor','middle').attr('fill','#dc2626').attr('font-size',16).attr('font-weight','bold').text('!');
        head.transition().duration(300).attr('fill','#facc15');
        timers.push(setTimeout(function() {
          pkt(g, [{x:hx,y:hy},{x:a2x,y:a2y}], '#4ade80', 0, 5);
          timers.push(setTimeout(function() {
            alert.remove();
            head.transition().duration(300).attr('fill','#f97316');
            if (tick < 80) run();
          }, 1000));
        }, 700));
      }, 1800));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Agents work autonomously, human intervenes on alert');
  }

  function drawPolicyDelegation(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var hx = p.ox + 50, hy = p.oy + 85;
    var ax = p.ox + 170;
    var d1x = p.ox + 280, d1y = p.oy + 100;
    var d2x = p.ox + 290, d2y = cy;
    var d3x = p.ox + 280, d3y = p.oy + 175;
    g.append('circle').attr('cx',hx).attr('cy',hy-12).attr('r',8).attr('fill','#f97316');
    g.append('line').attr('x1',hx).attr('y1',hy-4).attr('x2',hx).attr('y2',hy+14).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx-9).attr('y1',hy+4).attr('x2',hx+9).attr('y2',hy+4).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('rect').attr('x',ax-24).attr('y',cy-42).attr('width',48).attr('height',26)
      .attr('fill','#1e293b').attr('stroke','#64748b').attr('stroke-width',1.2).attr('rx',3);
    [0,1,2].forEach(function(i){ g.append('line').attr('x1',ax-16).attr('y1',cy-36+i*8).attr('x2',ax+16).attr('y2',cy-36+i*8).attr('stroke','#64748b').attr('stroke-width',1); });
    g.append('text').attr('x',ax).attr('y',cy-46).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',8).text('policy');
    dl(g, ax, cy+8, d1x, d1y, 0);
    dl(g, ax, cy+8, d2x, d2y, 0);
    dl(g, ax, cy+8, d3x, d3y, 0);
    dn(g, ax, cy+8, 18, '#3b82f6', 'agent');
    dn(g, d1x, d1y, 12, '#64748b', '');
    dn(g, d2x, d2y, 12, '#64748b', '');
    dn(g, d3x, d3y, 12, '#64748b', '');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:hx,y:hy},{x:ax,y:cy+8}], '#f97316', 100, 5);
      timers.push(setTimeout(function() {
        pkt(g, [{x:ax,y:cy+8},{x:d1x,y:d1y}], '#dc2626', 100, 4);
        pkt(g, [{x:ax,y:cy+8},{x:d2x,y:d2y}], '#dc2626', 350, 4);
        pkt(g, [{x:ax,y:cy+8},{x:d3x,y:d3y}], '#dc2626', 600, 4);
      }, 1000));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 3000));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Human defines rules, agent executes freely');
  }

  function drawApprovalEscalation(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var lx = p.ox + 30, rx = p.ox + 310;
    var ax = p.ox + 170;
    var hx = p.ox + 230, hy = p.oy + 75;
    dl(g, lx, cy, ax-20, cy, 0);
    dl(g, ax+20, cy, rx, cy, 0);
    dn(g, ax, cy, 20, '#3b82f6', 'agent');
    g.append('circle').attr('cx',hx).attr('cy',hy-12).attr('r',8).attr('fill','#f97316');
    g.append('line').attr('x1',hx).attr('y1',hy-4).attr('x2',hx).attr('y2',hy+10).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx-9).attr('y1',hy+2).attr('x2',hx+9).attr('y2',hy+2).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('text').attr('x',hx).attr('y',hy+26).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('approver');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:lx,y:cy},{x:rx,y:cy}], '#4ade80', 100, 4);
      pkt(g, [{x:lx,y:cy},{x:rx,y:cy}], '#4ade80', 700, 4);
      timers.push(setTimeout(function() {
        pkt(g, [{x:lx,y:cy},{x:ax,y:cy}], '#facc15', 0, 5);
        timers.push(setTimeout(function() {
          pkt(g, [{x:ax,y:cy},{x:hx,y:hy}], '#dc2626', 0, 4);
          timers.push(setTimeout(function() {
            pkt(g, [{x:hx,y:hy},{x:ax,y:cy}], '#4ade80', 0, 5);
            timers.push(setTimeout(function() {
              pkt(g, [{x:ax,y:cy},{x:rx,y:cy}], '#4ade80', 0, 5);
              timers.push(setTimeout(function() { if (tick < 80) run(); }, 1000));
            }, 500));
          }, 800));
        }, 500));
      }, 1500));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Most pass through; exceptions route to human');
  }

  function drawAdaptive(p) {
    var g = svg.append('g');
    var edges = g.append('g');
    var nodesG = g.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 122;
    var N = 6;
    function ellipse(i, n, rx, ry) {
      var a = -Math.PI / 2 + i * 2 * Math.PI / n;
      return {x: cx + rx * Math.cos(a), y: cy + ry * Math.sin(a)};
    }
    var scatter = [[-95,-34],[-40,22],[15,-38],[60,30],[110,-12],[-15,-4]];
    var layouts = {
      Chain: {pos: [0,1,2,3,4,5].map(function(i){return {x: cx - 115 + i * 46, y: cy};}),
        links: [[0,1],[1,2],[2,3],[3,4],[4,5]], flow: [[0,1],[2,3],[4,5]]},
      Supervisor: {pos: [{x: cx, y: cy - 40}].concat([0,1,2,3,4].map(function(i){return {x: cx - 100 + i * 50, y: cy + 42};})),
        links: [[0,1],[0,2],[0,3],[0,4],[0,5]], flow: [[0,2],[0,4],[3,0]]},
      Market: {pos: [{x: cx, y: cy}].concat([0,1,2,3,4].map(function(i){return ellipse(i, 5, 105, 46);})),
        links: [[1,0],[2,0],[3,0],[4,0],[5,0]], flow: [[1,0],[3,0],[5,0]]},
      Swarm: {pos: scatter.map(function(d){return {x: cx + d[0], y: cy + d[1]};}),
        links: [[0,1],[1,2],[2,5],[5,3],[3,4],[1,5]], flow: [[0,1],[2,5],[3,4]]}
    };
    var phases = [
      {name: 'Chain', sig: 'latency climbing'},
      {name: 'Supervisor', sig: 'reviewer saturated'},
      {name: 'Market', sig: 'bids too costly'},
      {name: 'Swarm', sig: 'drift detected'}
    ];
    var nodes = layouts.Chain.pos.map(function(pt, i) {
      return nodesG.append('circle').attr('cx', pt.x).attr('cy', pt.y)
        .attr('r', i === 0 ? 11 : 8)
        .attr('fill', i === 0 ? '#7f1d1d' : '#475569')
        .attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.85);
    });
    var label = g.append('text').attr('x', cx).attr('y', p.oy + 40)
      .attr('text-anchor', 'middle').attr('font-size', '9px').attr('font-weight', 'bold')
      .attr('fill', '#2563eb').text('structure: Chain');
    var tick = 0;
    function phase() {
      tick++;
      var cur = phases[(tick - 1) % 4], next = phases[tick % 4];
      var L = layouts[cur.name];
      edges.selectAll('*').remove();
      L.links.forEach(function(lk, i) {
        dl(edges, L.pos[lk[0]].x, L.pos[lk[0]].y, L.pos[lk[1]].x, L.pos[lk[1]].y, i * 60);
      });
      label.text('structure: ' + cur.name);
      timers.push(setTimeout(function() {
        L.flow.forEach(function(f, i) {
          pkt(g, [L.pos[f[0]], L.pos[f[1]]], taskColor, i * 250, 2.4);
          pkt(g, [L.pos[f[1]], L.pos[f[0]]], resultColor, 600 + i * 250, 2.2);
        });
      }, 500));
      timers.push(setTimeout(function() {
        var sig = g.append('text').attr('x', cx).attr('y', p.oy + 56)
          .attr('text-anchor', 'middle').attr('font-size', '8px').attr('font-style', 'italic')
          .attr('fill', '#dc2626').attr('opacity', 0).text(cur.sig + ', restructuring');
        sig.transition().duration(250).attr('opacity', 1)
          .transition().delay(700).duration(300).attr('opacity', 0).remove();
      }, 2100));
      timers.push(setTimeout(function() {
        edges.selectAll('line').transition().duration(350).attr('opacity', 0).remove();
        var NL = layouts[next.name];
        nodes.forEach(function(nd, i) {
          nd.transition().duration(750).attr('cx', NL.pos[i].x).attr('cy', NL.pos[i].y);
        });
      }, 2900));
      timers.push(setTimeout(function() { if (tick < 80) phase(); }, 3800));
    }
    phase();
    anno(g, p, 'the structure itself is what adapts');
  }

  // Assign draw functions in family order matching panelDefs
  function drawPlannerExec(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, py = p.oy + 72;
    var plx = cx - 110, dx = cx - 10, ex = cx + 90;
    var sy = p.oy + 150, sxs = [ex - 50, ex, ex + 50];
    // lines first (behind nodes)
    dl(g, plx + 13, py, dx - 18, py, 0);
    dl(g, dx + 18, py, ex - 11, py, 150);
    for (var i = 0; i < 3; i++) dl(g, ex, py + 11, sxs[i], sy - 8, 300 + i * 100);
    // plan artifact (rect) and nodes on top
    g.append('rect').attr('x', dx - 14).attr('y', py - 18).attr('width', 28).attr('height', 36)
      .attr('rx', 3).attr('fill', '#fff').attr('stroke', '#94a3b8').attr('stroke-width', 1.2);
    for (var i = 0; i < 3; i++) g.append('line').attr('x1', dx - 8).attr('y1', py - 8 + i * 9)
      .attr('x2', dx + 8).attr('y2', py - 8 + i * 9).attr('stroke', '#cbd5e1').attr('stroke-width', 1.5);
    g.append('text').attr('x', dx).attr('y', py + 32).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').text('Plan');
    dn(g, plx, py, 12, '#7f1d1d', 'Planner');
    dn(g, ex, py, 10, '#475569', 'Executor', py - 16);
    for (var i = 0; i < 3; i++) dn(g, sxs[i], sy, 6, '#94a3b8', 'Step ' + (i + 1));
    var tick = 0;
    function anim() {
      tick++;
      pkt(g, [{x: plx, y: py}, {x: dx, y: py}], taskColor, 200, 2.5);
      timers.push(setTimeout(function() {
        var ok = g.append('text').attr('x', dx).attr('y', py - 26).attr('text-anchor', 'middle')
          .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#16a34a')
          .attr('opacity', 0).text('approved');
        ok.transition().duration(200).attr('opacity', 1)
          .transition().delay(700).duration(300).attr('opacity', 0).remove();
        pkt(g, [{x: dx, y: py}, {x: ex, y: py}], taskColor, 400, 2.5);
      }, 800));
      timers.push(setTimeout(function() {
        for (var i = 0; i < 3; i++) {
          pkt(g, [{x: ex, y: py}, {x: sxs[i], y: sy}], taskColor, i * 250, 2.2);
          pkt(g, [{x: sxs[i], y: sy}, {x: ex, y: py}], resultColor, 500 + i * 250, 2.2);
        }
      }, 1900));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 4400));
    }
    anim();
    anno(g, p, 'the plan is locked before execution begins');
  }

  function drawWorkerPool(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, dy = p.oy + 66;
    var qx = cx - 120;
    var wy = p.oy + 150, wxs = [cx - 80, cx, cx + 80];
    // lines first (behind nodes)
    dl(g, qx + 14, dy, cx - 13, dy, 0);
    for (var i = 0; i < 3; i++) dl(g, cx, dy + 12, wxs[i], wy - 10, 150 + i * 100);
    // queue (stack of small squares) and nodes on top
    for (var i = 0; i < 3; i++) g.append('rect').attr('x', qx - 6 - i * 11).attr('y', dy - 5)
      .attr('width', 9).attr('height', 10).attr('rx', 2)
      .attr('fill', '#fecaca').attr('stroke', '#ef4444').attr('stroke-width', 0.8);
    g.append('text').attr('x', qx - 12).attr('y', dy + 20).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').text('Queue');
    dn(g, cx, dy, 12, '#7f1d1d', 'Dispatcher', dy - 18);
    for (var i = 0; i < 3; i++) dn(g, wxs[i], wy, 9, '#475569', 'Worker');
    var tick = 0;
    function anim() {
      tick++;
      var w = tick % 3;
      pkt(g, [{x: qx, y: dy}, {x: cx, y: dy}], taskColor, 150, 2.5);
      timers.push(setTimeout(function() {
        pkt(g, [{x: cx, y: dy}, {x: wxs[w], y: wy}], taskColor, 0, 2.5);
      }, 650));
      timers.push(setTimeout(function() {
        pkt(g, [{x: wxs[w], y: wy}, {x: cx, y: dy}], resultColor, 0, 2.5);
      }, 1350));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 2000));
    }
    anim();
    anno(g, p, 'any free worker takes the next task');
  }

  function drawMixture(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, my = p.oy + 108;
    var ix = cx - 130, ax = cx + 95;
    var sxs = cx - 15, sys = [p.oy + 62, my, p.oy + 154];
    var weights = [3.4, 2.6, 1.8];
    // lines first (behind nodes)
    for (var i = 0; i < 3; i++) {
      dl(g, ix + 11, my, sxs - 11, sys[i], i * 100);
      dl(g, sxs + 11, sys[i], ax - 13, my, 200 + i * 100);
    }
    // nodes on top
    dn(g, ix, my, 10, '#475569', 'Input');
    var scols = ['#b91c1c', '#dc2626', '#ef4444'];
    for (var i = 0; i < 3; i++) dn(g, sxs, sys[i], 9, scols[i], 'Specialist ' + (i + 1));
    dn(g, ax, my, 12, '#7f1d1d', 'Aggregator', my - 18);
    var tick = 0;
    function anim() {
      tick++;
      for (var i = 0; i < 3; i++) {
        pkt(g, [{x: ix, y: my}, {x: sxs, y: sys[i]}], taskColor, 200 + i * 120, 2.5);
      }
      timers.push(setTimeout(function() {
        for (var i = 0; i < 3; i++) {
          pkt(g, [{x: sxs, y: sys[i]}, {x: ax, y: my}], resultColor, i * 120, weights[i]);
        }
      }, 1100));
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', ax).attr('cy', my).attr('r', 12)
          .attr('fill', 'none').attr('stroke', '#16a34a').attr('stroke-width', 2).attr('opacity', 0.8)
          .transition().duration(600).attr('r', 26).attr('opacity', 0).remove();
      }, 2100));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 3400));
    }
    anim();
    anno(g, p, 'packet size = weight; nothing is discarded');
  }

  function drawContextScoping(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, py = p.oy + 60;
    var wy = p.oy + 152, wxs = [cx - 90, cx, cx + 90];
    // lines first (behind nodes)
    for (var i = 0; i < 3; i++) dl(g, cx, py + 12, wxs[i], wy - 24, i * 120);
    // full context bar under the principal
    g.append('rect').attr('x', cx - 60).attr('y', py + 20).attr('width', 120).attr('height', 7)
      .attr('rx', 2).attr('fill', '#e2e8f0').attr('stroke', '#cbd5e1').attr('stroke-width', 0.8);
    g.append('text').attr('x', cx + 70).attr('y', py + 27).attr('font-size', '7px')
      .attr('fill', '#aaa').text('full context');
    // trimmed briefs above each worker
    var briefW = [34, 22, 28];
    for (var i = 0; i < 3; i++) {
      g.append('rect').attr('x', wxs[i] - briefW[i] / 2).attr('y', wy - 22).attr('width', briefW[i])
        .attr('height', 5).attr('rx', 1.5).attr('fill', '#fde68a').attr('stroke', '#f59e0b').attr('stroke-width', 0.7);
    }
    // nodes on top
    dn(g, cx, py, 12, '#7f1d1d', 'Principal', py - 18);
    for (var i = 0; i < 3; i++) dn(g, wxs[i], wy, 9, '#475569', 'Sub-agent');
    var tick = 0;
    function anim() {
      tick++;
      for (var i = 0; i < 3; i++) {
        (function(idx) {
          pkt(g, [{x: cx, y: py}, {x: wxs[idx], y: wy - 24}], '#f59e0b', 300 + idx * 250, 2.2);
          timers.push(setTimeout(function() {
            pkt(g, [{x: wxs[idx], y: wy}, {x: cx, y: py}], resultColor, 0, 2.2);
          }, 1400 + idx * 250));
        })(i);
      }
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 3600));
    }
    anim();
    anno(g, p, 'each brief is trimmed to the task, never the transcript');
  }

  function drawTaskForce(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 60;
    var ty = p.oy + 138, txs = [cx - 60, cx, cx + 60];
    for (var i = 0; i < 3; i++) dl(g, cx, cy + 12, txs[i], ty - 12, i * 100);
    dn(g, cx, cy, 12, '#7f1d1d', 'Coordinator', cy - 18);
    var tick = 0;
    function anim() {
      tick++;
      var ring = g.append('rect').attr('x', cx - 95).attr('y', ty - 30).attr('width', 190).attr('height', 60)
        .attr('rx', 10).attr('fill', 'none').attr('stroke', '#f59e0b').attr('stroke-width', 1.3)
        .attr('stroke-dasharray', '5,4').attr('opacity', 0);
      ring.transition().delay(500).duration(300).attr('opacity', 0.8);
      var members = [];
      for (var i = 0; i < 3; i++) {
        var m = g.append('circle').attr('cx', txs[i]).attr('cy', ty).attr('r', 0)
          .attr('fill', '#475569').attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.85);
        m.transition().delay(300 + i * 150).duration(250).attr('r', 8);
        members.push(m);
        pkt(g, [{x: cx, y: cy}, {x: txs[i], y: ty}], taskColor, 700 + i * 150, 2.3);
      }
      timers.push(setTimeout(function() {
        pkt(g, [{x: cx, y: ty}, {x: cx, y: cy}], resultColor, 0, 3);
      }, 1900));
      timers.push(setTimeout(function() {
        ring.transition().duration(400).attr('opacity', 0).remove();
        members.forEach(function(m) { m.transition().duration(400).attr('r', 0).attr('opacity', 0).remove(); });
      }, 2600));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 3600));
    }
    anim();
    anno(g, p, 'the team exists exactly as long as the task');
  }

  function drawLiaison(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, my = p.oy + 100;
    var lxs = [cx - 110, cx - 80, cx - 95], lys = [my - 25, my - 25, my + 15];
    var rxs = [cx + 80, cx + 110, cx + 95], rys = [my - 25, my - 25, my + 15];
    g.append('rect').attr('x', cx - 130).attr('y', my - 48).attr('width', 70).attr('height', 88)
      .attr('rx', 8).attr('fill', '#f8fafc').attr('stroke', '#e2e8f0');
    g.append('rect').attr('x', cx + 60).attr('y', my - 48).attr('width', 70).attr('height', 88)
      .attr('rx', 8).attr('fill', '#f8fafc').attr('stroke', '#e2e8f0');
    dl(g, cx - 60, my, cx - 11, my, 0);
    dl(g, cx + 11, my, cx + 60, my, 100);
    for (var i = 0; i < 3; i++) { dn(g, lxs[i], lys[i], 5, '#94a3b8'); dn(g, rxs[i], rys[i], 5, '#94a3b8'); }
    dn(g, cx, my, 11, '#7f1d1d', 'Liaison', my + 24);
    var tick = 0;
    function anim() {
      tick++;
      var ok = tick % 2 === 1;
      pkt(g, [{x: cx - 95, y: my - 20}, {x: cx, y: my}], taskColor, 200, 2.5);
      timers.push(setTimeout(function() {
        var mark = g.append('text').attr('x', cx).attr('y', my - 18).attr('text-anchor', 'middle')
          .attr('font-size', '11px').attr('font-weight', 'bold')
          .attr('fill', ok ? '#16a34a' : '#dc2626').attr('opacity', 0).text(ok ? 'ok' : 'x');
        mark.transition().duration(200).attr('opacity', 1)
          .transition().delay(500).duration(300).attr('opacity', 0).remove();
        if (ok) pkt(g, [{x: cx, y: my}, {x: cx + 95, y: my - 20}], taskColor, 300, 2.5);
        else pkt(g, [{x: cx, y: my}, {x: cx - 95, y: my - 20}], '#dc2626', 300, 2.2);
      }, 800));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 2400));
    }
    anim();
    anno(g, p, 'inside moves freely; the boundary has one door');
  }

  function drawNeuroSymbolic(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, my = p.oy + 100;
    var nx = cx - 80, sx = cx + 80;
    dl(g, nx + 13, my, sx - 15, my, 0);
    dn(g, nx, my, 12, '#b91c1c', 'Neural (proposes)');
    g.append('rect').attr('x', sx - 13).attr('y', my - 13).attr('width', 26).attr('height', 26)
      .attr('rx', 3).attr('fill', '#475569').attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.9);
    g.append('text').attr('x', sx).attr('y', my + 26).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').text('Symbolic (verifies)');
    var tick = 0;
    function anim() {
      tick++;
      var ok = tick % 3 !== 0;
      pkt(g, [{x: nx, y: my}, {x: sx, y: my}], taskColor, 200, 2.6);
      timers.push(setTimeout(function() {
        var mark = g.append('text').attr('x', sx).attr('y', my - 20).attr('text-anchor', 'middle')
          .attr('font-size', '12px').attr('font-weight', 'bold')
          .attr('fill', ok ? '#16a34a' : '#dc2626').attr('opacity', 0).text(ok ? 'proved' : 'rejected');
        mark.transition().duration(200).attr('opacity', 1)
          .transition().delay(600).duration(300).attr('opacity', 0).remove();
        pkt(g, [{x: sx, y: my}, {x: nx, y: my}], ok ? resultColor : '#dc2626', 300, 2.3);
      }, 800));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 2300));
    }
    anim();
    anno(g, p, 'a verdict, not an opinion');
  }

  function drawCurator(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, my = p.oy + 100;
    var gx = cx - 100, gys = [my - 40, my, my + 40];
    var kx = cx + 5, ox2 = cx + 105;
    for (var i = 0; i < 3; i++) dl(g, gx + 9, gys[i], kx - 12, my, i * 100);
    dl(g, kx + 12, my, ox2 - 9, my, 300);
    for (var i = 0; i < 3; i++) dn(g, gx, gys[i], 7, '#94a3b8', i === 2 ? 'Generators' : null);
    dn(g, kx, my, 11, '#7f1d1d', 'Curator');
    dn(g, ox2, my, 8, '#16a34a', 'Kept');
    var tick = 0;
    function anim() {
      tick++;
      var keep = tick % 3;
      for (var i = 0; i < 3; i++) {
        (function(idx) {
          pkt(g, [{x: gx, y: gys[idx]}, {x: kx, y: my}], taskColor, 200 + idx * 150, 2.4);
          if (idx === keep) {
            timers.push(setTimeout(function() {
              pkt(g, [{x: kx, y: my}, {x: ox2, y: my}], resultColor, 0, 2.8);
            }, 900 + idx * 150));
          }
        })(i);
      }
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 2500));
    }
    anim();
    anno(g, p, 'three in, one forward; the discards leave no trace');
  }

  function drawDeadLetter(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 62;
    var xs = [cx - 90, cx, cx + 90];
    var qy = p.oy + 148;
    for (var i = 0; i < 2; i++) dl(g, xs[i] + 10, ty, xs[i + 1] - 10, ty, i * 100);
    dld(g, xs[2], ty + 12, cx + 20, qy - 14, 300);
    g.append('rect').attr('x', cx - 30).attr('y', qy - 12).attr('width', 100).attr('height', 26)
      .attr('rx', 4).attr('fill', '#fef2f2').attr('stroke', '#fca5a5').attr('stroke-width', 1.2);
    g.append('text').attr('x', cx + 20).attr('y', qy + 28).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').text('Dead letter queue');
    dn(g, xs[0], ty, 8, '#475569', 'Agent');
    dn(g, xs[1], ty, 8, '#64748b', 'Senior');
    dn(g, xs[2], ty, 8, '#7f1d1d', 'Human');
    var tick = 0, parked = 0;
    function anim() {
      tick++;
      var t = pkt(g, [{x: xs[0], y: ty}, {x: xs[1], y: ty}, {x: xs[2], y: ty}], taskColor, 200, 2.6);
      timers.push(setTimeout(function() {
        var mark = g.append('text').attr('x', xs[2]).attr('y', ty - 16).attr('text-anchor', 'middle')
          .attr('font-size', '11px').attr('font-weight', 'bold').attr('fill', '#dc2626')
          .attr('opacity', 0).text('x');
        mark.transition().duration(200).attr('opacity', 1)
          .transition().delay(400).duration(300).attr('opacity', 0).remove();
        pkt(g, [{x: xs[2], y: ty}, {x: cx + 20, y: qy}], '#dc2626', 500, 2.4);
      }, t));
      timers.push(setTimeout(function() {
        parked = Math.min(parked + 1, 6);
        g.append('rect').attr('x', cx - 24 + ((parked - 1) % 6) * 15).attr('y', qy - 6)
          .attr('width', 11).attr('height', 13).attr('rx', 2)
          .attr('fill', '#fecaca').attr('stroke', '#ef4444').attr('stroke-width', 0.7).attr('opacity', 0)
          .transition().duration(250).attr('opacity', 1);
      }, t + 900));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, t + 1600));
    }
    anim();
    anno(g, p, 'parked with context, not dropped');
  }

  function drawExperiential(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ay = p.oy + 66;
    var ax = cx - 90, bx = cx + 90, sy = p.oy + 140;
    dld(g, ax, ay + 11, cx - 18, sy - 6, 0);
    dld(g, cx + 18, sy - 6, bx, ay + 11, 200);
    g.append('rect').attr('x', cx - 30).attr('y', sy - 12).attr('width', 60).attr('height', 24)
      .attr('rx', 5).attr('fill', '#eff6ff').attr('stroke', '#93c5fd').attr('stroke-width', 1.2);
    g.append('text').attr('x', cx).attr('y', sy + 26).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').text('Experience store');
    dn(g, ax, ay, 9, '#475569', 'Agent, task N');
    dn(g, bx, ay, 9, '#94a3b8', 'Agent, task N+1');
    var tick = 0;
    function anim() {
      tick++;
      var ok = g.append('text').attr('x', ax).attr('y', ay - 16).attr('text-anchor', 'middle')
        .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#16a34a')
        .attr('opacity', 0).text('done');
      ok.transition().delay(200).duration(200).attr('opacity', 1)
        .transition().delay(500).duration(300).attr('opacity', 0).remove();
      pkt(g, [{x: ax, y: ay}, {x: cx - 10, y: sy - 4}], '#2563eb', 600, 2.4);
      timers.push(setTimeout(function() {
        pkt(g, [{x: cx + 10, y: sy - 4}, {x: bx, y: ay}], '#2563eb', 0, 2.4);
        pkt(g, [{x: bx, y: ay}, {x: bx + 0.01, y: ay}], resultColor, 700, 3);
      }, 1500));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 3000));
    }
    anim();
    anno(g, p, 'the depositor and the beneficiary never meet');
  }

  function drawProvenance(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, my = p.oy + 100;
    var xs = [cx - 100, cx, cx + 100];
    for (var i = 0; i < 2; i++) dl(g, xs[i] + 11, my, xs[i + 1] - 11, my, i * 100);
    for (var i = 0; i < 3; i++) dn(g, xs[i], my, 9, '#475569', 'Hop ' + (i + 1));
    var tick = 0;
    function anim() {
      tick++;
      for (var i = 0; i < 2; i++) {
        (function(idx) {
          timers.push(setTimeout(function() {
            pkt(g, [{x: xs[idx], y: my}, {x: xs[idx + 1], y: my}], taskColor, 0, 2.6);
            var tag = g.append('rect').attr('x', xs[idx] - 4).attr('y', my - 14).attr('width', 8).attr('height', 6)
              .attr('rx', 1).attr('fill', '#f59e0b').attr('opacity', 0.9);
            tag.transition().duration(280).attr('x', xs[idx + 1] - 4)
              .transition().duration(400).attr('opacity', 0).remove();
          }, 300 + idx * 700));
        })(i);
      }
      timers.push(setTimeout(function() {
        var lbl = g.append('text').attr('x', xs[2]).attr('y', my - 22).attr('text-anchor', 'middle')
          .attr('font-size', '8px').attr('fill', '#16a34a').attr('opacity', 0).text('source intact');
        lbl.transition().duration(250).attr('opacity', 1)
          .transition().delay(600).duration(300).attr('opacity', 0).remove();
      }, 1800));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 3000));
    }
    anim();
    anno(g, p, 'the tag travels with the claim');
  }

  function drawLiquidDemocracy(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    var by = p.oy + 150, bxs = [cx - 120, cx - 60, cx, cx + 60, cx + 120];
    var p1 = {x: cx - 40, y: p.oy + 95}, p2 = {x: cx + 50, y: p.oy + 62};
    dl(g, bxs[0], by - 8, p1.x, p1.y + 10, 0);
    dl(g, bxs[1], by - 8, p1.x, p1.y + 10, 100);
    dl(g, bxs[2], by - 8, p1.x, p1.y + 10, 200);
    dl(g, bxs[3], by - 8, p2.x, p2.y + 10, 300);
    dl(g, bxs[4], by - 8, p2.x, p2.y + 10, 400);
    dl(g, p1.x + 8, p1.y - 6, p2.x - 8, p2.y + 6, 500);
    for (var i = 0; i < 5; i++) dn(g, bxs[i], by, 5.5, '#94a3b8', i === 2 ? 'Voters' : null);
    dn(g, p1.x, p1.y, 8, '#475569', 'Proxy');
    dn(g, p2.x, p2.y, 11, '#7f1d1d', 'Super-proxy', p2.y - 18);
    var tick = 0;
    function anim() {
      tick++;
      for (var i = 0; i < 3; i++) pkt(g, [{x: bxs[i], y: by}, p1], taskColor, 200 + i * 120, 2);
      pkt(g, [{x: bxs[3], y: by}, p2], taskColor, 260, 2);
      pkt(g, [{x: bxs[4], y: by}, p2], taskColor, 380, 2);
      timers.push(setTimeout(function() { pkt(g, [p1, p2], taskColor, 0, 2.8); }, 1000));
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', p2.x).attr('cy', p2.y).attr('r', 11)
          .attr('fill', 'none').attr('stroke', '#f59e0b').attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(600).attr('r', 22).attr('opacity', 0).remove();
      }, 1700));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 3000));
    }
    anim();
    anno(g, p, 'authority pools along revocable chains');
  }

  function drawFleetController(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, hy = p.oy + 58;
    var fy1 = p.oy + 120, fy2 = p.oy + 155;
    var fleet = [];
    for (var i = 0; i < 4; i++) fleet.push({x: cx - 105 + i * 70, y: fy1});
    for (var i = 0; i < 4; i++) fleet.push({x: cx - 105 + i * 70, y: fy2});
    fleet.forEach(function(f, i) { dl(g, cx, hy + 11, f.x, f.y - 7, i * 60); });
    dn(g, cx, hy, 11, '#f59e0b', 'Controller', hy - 17);
    fleet.forEach(function(f) { dn(g, f.x, f.y, 5.5, '#475569'); });
    var tick = 0;
    function anim() {
      tick++;
      var trouble = fleet[tick % 8];
      pkt(g, [{x: cx, y: hy}, fleet[(tick + 3) % 8]], taskColor, 200, 2);
      pkt(g, [fleet[(tick + 5) % 8], {x: cx, y: hy}], resultColor, 400, 2);
      timers.push(setTimeout(function() {
        var alert = g.append('circle').attr('cx', trouble.x).attr('cy', trouble.y).attr('r', 6)
          .attr('fill', 'none').attr('stroke', '#dc2626').attr('stroke-width', 2).attr('opacity', 0.9);
        alert.transition().duration(500).attr('r', 13).attr('opacity', 0).remove();
        pkt(g, [{x: cx, y: hy}, trouble], '#dc2626', 400, 2.4);
      }, 900));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 2100));
    }
    anim();
    anno(g, p, 'air traffic control, not review');
  }

  function drawPopulationGov(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, hy = p.oy + 56;
    var base = p.oy + 165, bxs = [cx - 80, cx - 40, cx, cx + 40, cx + 80];
    var heights = [22, 38, 52, 34, 18];
    dld(g, cx, hy + 12, cx, base - 62, 100);
    var bars = bxs.map(function(x, i) {
      return g.append('rect').attr('x', x - 13).attr('y', base - heights[i]).attr('width', 26)
        .attr('height', heights[i]).attr('rx', 2).attr('fill', '#94a3b8').attr('opacity', 0.7);
    });
    g.append('text').attr('x', cx).attr('y', base + 14).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').text('Population behavior (distribution)');
    dn(g, cx, hy, 11, '#f59e0b', 'Governor', hy - 17);
    var tick = 0;
    function anim() {
      tick++;
      pkt(g, [{x: cx, y: hy}, {x: cx, y: base - 60}], '#f59e0b', 300, 2.6);
      timers.push(setTimeout(function() {
        bars.forEach(function(b, i) {
          var h = heights[i] + (Math.sin(tick * 1.3 + i) * 10);
          b.transition().duration(600).attr('y', base - h).attr('height', h);
        });
      }, 900));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 2400));
    }
    anim();
    anno(g, p, 'statistics, not spot-checks');
  }

  function drawNegotiatingProxy(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, my = p.oy + 100;
    var h1 = cx - 130, p1 = cx - 45, p2 = cx + 45, h2 = cx + 130;
    dl(g, h1 + 9, my, p1 - 11, my, 0);
    dl(g, p1 + 11, my, p2 - 11, my, 150);
    dl(g, p2 + 11, my, h2 - 9, my, 300);
    dn(g, h1, my, 7, '#f59e0b', 'You');
    dn(g, p1, my, 10, '#7f1d1d', 'Your proxy');
    dn(g, p2, my, 10, '#b91c1c', 'Their proxy');
    dn(g, h2, my, 7, '#f59e0b', 'Them');
    var tick = 0;
    function anim() {
      tick++;
      pkt(g, [{x: h1, y: my}, {x: p1, y: my}], '#f59e0b', 200, 2);
      pkt(g, [{x: h2, y: my}, {x: p2, y: my}], '#f59e0b', 200, 2);
      for (var i = 0; i < 3; i++) {
        pkt(g, [{x: p1, y: my}, {x: p2, y: my}], taskColor, 800 + i * 500, 2.3);
        pkt(g, [{x: p2, y: my}, {x: p1, y: my}], taskColor, 1050 + i * 500, 2.3);
      }
      timers.push(setTimeout(function() {
        var deal = g.append('text').attr('x', cx).attr('y', my - 20).attr('text-anchor', 'middle')
          .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#16a34a')
          .attr('opacity', 0).text('deal');
        deal.transition().duration(250).attr('opacity', 1)
          .transition().delay(500).duration(300).attr('opacity', 0).remove();
        pkt(g, [{x: p1, y: my}, {x: h1, y: my}], resultColor, 300, 2);
        pkt(g, [{x: p2, y: my}, {x: h2, y: my}], resultColor, 300, 2);
      }, 2500));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 3800));
    }
    anim();
    anno(g, p, 'one reply replaces forty messages');
  }

  function drawReflexive(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 102;
    var ags = [{x: cx - 95, y: cy - 30}, {x: cx + 95, y: cy - 30}, {x: cx, y: cy + 50}];
    ags.forEach(function(a, i) { dld(g, cx, cy, a.x, a.y, i * 120); });
    dn(g, cx, cy, 12, '#f59e0b', 'Principal', cy + 22);
    ags.forEach(function(a) { dn(g, a.x, a.y, 7, '#475569'); });
    var tick = 0;
    function anim() {
      tick++;
      ags.forEach(function(a, i) { pkt(g, [{x: cx, y: cy}, a], '#94a3b8', 200 + i * 150, 1.8); });
      var src = ags[tick % 3];
      timers.push(setTimeout(function() {
        pkt(g, [src, {x: cx, y: cy}], '#2563eb', 0, 2.8);
        var lbl = g.append('text').attr('x', cx).attr('y', cy - 24).attr('text-anchor', 'middle')
          .attr('font-size', '8px').attr('font-style', 'italic').attr('fill', '#2563eb')
          .attr('opacity', 0).text('"you are anchoring again"');
        lbl.transition().delay(300).duration(250).attr('opacity', 1)
          .transition().delay(700).duration(300).attr('opacity', 0).remove();
      }, 1100));
      timers.push(setTimeout(function() { if (tick < 80) anim(); }, 2800));
    }
    anim();
    anno(g, p, 'the delegation points at the principal');
  }

  drawFns = [
    // Sequential (6)
    drawChain, drawPipeline, drawRouter, drawEscalation, drawRelay, drawLoop,
    // Hierarchical (10)
    drawTree, drawMapReduce, drawSupervisor, drawOrchestrator, drawPlannerExec, drawWorkerPool, drawTaskForce, drawLiaison, drawMissionCommand, drawFeudal,
    // Quality & Verification (9)
    drawEvaluator, drawVoting, drawMixture, drawNeuroSymbolic, drawCurator, drawCritic, drawDebate, drawWitness, drawVerificationGame,
    // Reliability (5)
    drawCircuitBreaker, drawTimeout, drawCheckpoint, drawCanary, drawDeadLetter,
    // Market & Competition (4)
    drawAuction, drawNegotiation, drawSpeculative, drawContractNet,
    // Knowledge Transfer (5)
    drawTeacherStudent, drawFederatedLearning, drawBlackboard, drawDistillation, drawExperiential,
    // Emergent Coordination (5)
    drawPubSub, drawChoreography, drawStigmergy, drawSwarm, drawGossip,
    // Trust & Authority (7)
    drawPrivilege, drawContextScoping, drawProvenance, drawLiquidDemocracy, drawFirebreaks, drawCredentials, drawZoneOfIndifference,
    // Human-Agent Interface (8)
    drawHumanGate, drawHumanOnLoop, drawPolicyDelegation, drawApprovalEscalation, drawFleetController, drawPopulationGov, drawNegotiatingProxy, drawReflexive,
    // Meta-Pattern (1)
    drawAdaptive
  ];
  init();
  container.restartViz = init;
})();
</script>
