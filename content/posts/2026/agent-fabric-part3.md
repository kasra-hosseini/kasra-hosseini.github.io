---
title: "The Agent Fabric (Part 3): Ruling an Agent Society"
subtitle: "Governance archetypes, who they serve, and who actually enforces them"
date: 2026-07-31
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "multi-agent systems", "LLM", "agent fabric", "governance", "the loom hypothesis"]
description: "How agent societies are governed: twenty-two governance archetypes, the failure modes each one invites, and why every structure is ultimately enforced by whoever owns the compute."
draft: false
math: false
ShowToc: true
TocOpen: false
hideCitation: false
wordcount: "~4,900 words (body) · ~9,900 words (notes + archetypes)"
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
    .viz-caption details { display: none !important; }
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

<p style="font-size: 0.82em; color: #999; margin-top: 1em;"><a href="https://code.claude.com/docs/en/overview" target="_blank" rel="noopener" style="color: #999;">Claude Code</a> was used for editing and visualizations. All ideas and arguments are the authors' own.</p>

<div style="background: #fffbeb; border: 1px solid #f59e0b; border-radius: 6px; padding: 0.6em 1em; margin: 1.5em 0; font-size: 0.88em; color: #92400e;">
<strong>Early access.</strong> This blog series is a work in progress. Feedback, comments, and suggestions are welcome. Feel free to <a href="https://www.linkedin.com/in/kasra-hosseini/" target="_blank" rel="noopener" style="color: #92400e;">reach out on LinkedIn</a> or leave a comment at the bottom of the page.
</div>

*Part of The Agent Fabric series. [Part 2](/posts/2026/agent-fabric-part2/) ended on an uncomfortable finding: a system that remembers who did what, how well, and at what cost has already begun to govern, whether anyone designed it to or not. Operational data hardens into routing preference, preference becomes standing, and standing constrains what can be decided later. This post is about what to do with that.*

*Two ways to read it. The body is the argument. The expandable sections are a reference manual of archetype cards, scenarios, and comparisons, and you can ignore all of them without losing the thread.*

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **[Prologue: The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/)**: the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis, and the path from isolation to interweaving
- **[Part 2: Delegation, and What It Costs](/posts/2026/agent-fabric-part2/)**: delegation archetypes, the economics of delegation trees, and how splitting work turns into authority
- **Part 3: Ruling an Agent Society** (you are here): governance archetypes, who benefits, adversarial dynamics, and who enforces the rules
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
</div>

The argument of this series is that governance turns up in agent systems whether anyone invited it or not. Worth being clear about the status of that claim before building on it: it is a prediction, not a measurement. It follows from how these systems are built and from decades of evidence about what interacting agents under resource pressure do in biology, economics, and simulation. Whether language-model agents behave the same way is still being established, and the honest position is that the mechanism is well motivated and not yet demonstrated at scale.

Grant it for now, though, and an interesting question opens up: not whether you will have governance, but which kind. This post is a field guide to the answers. Twenty-two recognizable ways an agent society can be run, what each is good at, how each fails, who quietly benefits from each, and then the harder question sitting underneath all of them, which is who actually enforces any of it.

One word is doing heavy lifting throughout, so it is worth pinning down. A **society** here is not just several agents working together. It is a group that has started to accumulate things across tasks: context they share, routing that depends on what happened last time, and learning that passes between them. A pipeline that forgets everything between runs never becomes one. A group that remembers does, and remembering is exactly what governance grows out of.

## What Governance Actually Is

Start with something small enough to picture. A support team wires up a router that sends billing questions to one agent and technical questions to another. Nothing remotely political about it. Then somebody adds logging, because logging is free and obviously sensible, and the router starts noticing which agents resolve tickets and which ones get escalated. Within a month it is quietly sending the hard billing disputes to two particular agents and routing nothing important to a third.

Now a new agent joins. Who decides whether it gets live tickets? Nobody wrote a rule for that, and yet there is an answer: it has to clear the performance bar the router learned on its own. That bar was never in a spec. It emerged, it now governs who gets what work, and the team that built the thing would struggle to override it without making service worse.

That is the whole transition in miniature, and it is worth being exact about where the line falls. A router that just forwards traffic is playing the game. A router that keeps score, decides who may touch what, and promotes or benches agents on the strength of their record is writing the rules the game has to follow. The economist <a href="https://en.wikipedia.org/wiki/Douglass_North" target="_blank" rel="noopener" class="red-link">Douglass North</a> drew exactly that distinction for human institutions: they are the rules of the game, not the players and not the moves. Note that persistence alone is not enough, because a cache persists and governs nothing. What matters is standing authority over how resources get handed out, authority that outlives any single task.

North's account rests on one more thing, though, which is enforcement, and that part does not transfer to agents at all. It matters enough that a later section is about nothing else.

Underneath all of it sits a problem old enough to have a name: whenever one party acts on another's behalf, their interests can diverge. Economists call this the <a href="https://en.wikipedia.org/wiki/Principal%E2%80%93agent_problem" target="_blank" rel="noopener" class="red-link">principal-agent problem</a>, and it is the reason intent leaks as delegation deepens. Every hop is one more chance for what was asked and what gets done to come apart, which is the compounding drift [Part 2](/posts/2026/agent-fabric-part2/) traced through delegation trees. Governance is the machinery for keeping those two things aligned across depth, time, and organizational boundaries.

Every governance structure is really a bet about how to hold [Part 1](/posts/2026/agent-fabric-part1/)'s two pressures in balance: agents have to stay useful to survive, and yet their numbers can grow far faster than the compute available to run them. Different archetypes make that bet differently. And it is worth being sober about the payoff, because the most robust finding in the study of human group intelligence is that groups are not automatically smarter than the people in them (<a href="https://doi.org/10.1126/science.1193147" target="_blank" rel="noopener" class="red-link">Woolley et al. 2010</a>); what determines whether a group beats its members is composition, communication, incentives, and process. Whether that carries over to agents is genuinely unknown, but the design lesson survives the uncertainty: more agents can mean more insight, or just more noise, and the structure decides which.

Here is something worth sitting with. For all of recorded history, humans have found only three basic ways to make a group coordinate, and every archetype in this post is some mixture of them.

You can put somebody in charge, which gives you a hierarchy, and it is fast for exactly the reason it is dangerous: every risk now sits in one place. You can refuse to decide centrally at all and let people trade until prices sort it out, which gives you a market, flexible right up until somebody works out how to game the scoring. Or, the option that keeps surprising people, those who depend on a shared resource can govern it together with neither a boss nor a price. That is a commons, and it is remarkably durable in practice and agonizingly slow to decide anything.

Three options, after thousands of years of trying. Expect agent societies to run all of them at once.

Economists spent the twentieth century working out why each exists and when each wins, and the arguments repay attention if you want the depth (<a href="https://en.wikipedia.org/wiki/The_Nature_of_the_Firm" target="_blank" rel="noopener" class="red-link">Coase</a> on firms, <a href="https://en.wikipedia.org/wiki/The_Use_of_Knowledge_in_Society" target="_blank" rel="noopener" class="red-link">Hayek</a> on prices, <a href="https://en.wikipedia.org/wiki/Elinor_Ostrom#Design_principles_for_Common_Pool_Resource_(CPR)_institution" target="_blank" rel="noopener" class="red-link">Ostrom</a> on commons). Those one-line summaries above are ours, not theirs, and the borrowing is loosest for the market case: Hayek's real point was that prices carry information nobody could have centralized, whereas the compute-and-latency "prices" agents trade in carry much less of it.

Some of this gets designed on purpose, as when a company ships an orchestrator with roles written down. The rest <a href="https://en.wikipedia.org/wiki/Self-organization" target="_blank" rel="noopener" class="red-link">self-organizes</a>, with norms hardening out of repeated interaction while nobody is watching. That second path is not a new discovery; complexity researchers spent decades showing that interacting agents under resource pressure reliably generate coordination structures nobody designed (<a href="https://en.wikipedia.org/wiki/John_Henry_Holland" target="_blank" rel="noopener" class="red-link">Holland</a>, *Hidden Order*, 1995; <a href="https://en.wikipedia.org/wiki/Stuart_Kauffman" target="_blank" rel="noopener" class="red-link">Kauffman</a>, *The Origins of Order*, 1993). Those findings come from biology, physics, and simple simulations, though, and whether language-model agents do the same thing is still an open empirical question rather than a settled fact.

What is genuinely new is that here the conditions for self-organization are themselves adjustable. An ant colony's chemistry is not up for negotiation, but an agent society runs on protocols, reward structures, and starting configurations that somebody owns and can change. You cannot redesign a pheromone. You can absolutely redesign a reputation score.

In practice a society's circumstances mostly decide what it becomes, and only two or three of those circumstances really matter. Predictable work rewards hierarchy; genuinely novel work rewards markets and colonies, because nobody at the centre knows enough to direct it.

The one that dominates everything else is drift, which is the slow slide where a system keeps optimizing faithfully while what it optimizes for stops matching what anyone wanted. If drift would be catastrophic where you work, you will end up reaching for hard rules and mandatory verification whether you like the overhead or not. If you can absorb some, the looser arrangements stay open to you. And the moment more than one organization is involved, a single centre stops being available at all.

Underneath those sits a question that gets asked far too rarely: who is meant to accumulate advantage here, and who needs protecting from it? Answer that honestly and the field of plausible archetypes narrows fast.

## Who Actually Enforces Any of This

Before going near the archetypes, there is something that undercuts all of them, and it is worth slowing down for. Every one of them is a claim about who decides. But a decision only means anything if it can be made to stick, and no agent can make anything stick on its own.

Ask what actually happens when a judicial agent rules against an executive agent. In a human court, the ruling binds because defiance carries consequences nobody can escape: police, sanction, disgrace, exclusion. Between two agents, the ruling binds for exactly one reason, which is that the runtime chose to honour it. Take away that choice and the judgment is a string of text. That is not a weakness of one archetype. It is true of all twenty-two.

So follow the authority upward and you find it does not stop where the diagram stops. Whoever controls the compute, the API access, and the protocol definitions is setting the terms, whatever the boxes and arrows claim. A federation running entirely on one provider's infrastructure is a federation right up until that provider changes its pricing. A market whose discovery layer belongs to a single platform was never a market; it is a franchise wearing market clothing.

Which means none of what follows is an institution in North's full sense. They are platform-enforced coordination regimes that behave like institutions, with the enforcement quietly subcontracted to whoever owns the substrate. So read the whole catalogue below with that in mind: pick an archetype without naming who enforces it and you have done half a design. Governance design and platform design are the same problem, and the second one is usually decided by somebody else.

One consequence deserves stating plainly, because it is the piece most often missing: a governance system with no appeal turns every routing error into precedent. An agent wrongly downranked has no recourse, the mistake propagates into future decisions, and nothing in the system is built to notice. Contested memory writes, disputed rankings, and actions that caused real harm need a slower path that can revisit them. Without one, errors do not surface. They compound.

## The Twenty-Two Archetypes

Worth noting before the catalogue that these two layers are not rivals. Governance structures are built out of delegation patterns: an autocracy runs on chains and evaluators, a market on auctions, a federation on routers and relays across a boundary. A few names appear in both vocabularies, which can confuse. Mission Command as a delegation pattern means telling one agent the intent behind one task; Mission Command as governance means a society-wide commitment that this is how all tasks get communicated, plus a rule about who may change it. Same mechanism, different scope, and the scope is where the authority lives.

So here is the useful way to read what follows. Twenty-two recognizable structures, some already running in production and some only plausible given where things are heading, and for each one the question to hold in your head is the one from the last section: who would actually make this stick? Some answer it well. Most answer it by quietly assuming somebody else will.

A warning about one word before you go in. The cards rate each archetype on four things, including *legitimacy*, and in political philosophy that word means the governed consenting to be governed, which agents cannot do. Read it here in a narrower engineering sense: will the participants, and the humans answerable for them, treat this structure's rulings as binding? Open Problems returns to what gets lost in that swap.


<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">The four evaluation axes explained</summary>

**Efficiency:** Autocracy and Doctrine are highly efficient, with minimal coordination overhead. Market and Colony are less efficient, though for different reasons: a Market spends effort on the bidding and matching that reaching a price requires, while a Colony has little per-interaction overhead but wastes effort through redundant, uncoordinated work. The Agora and Federation fall in between.

**Drift resistance:** Doctrine and Zero-Trust resist drift most effectively, through hard-to-change rules and mandatory verification respectively. Colony and Liquid Democracy are most vulnerable, as norms shift invisibly and delegation chains can amplify distortion. Most deployers will trade some drift resistance for flexibility, depending on their risk tolerance.

**Legitimacy:** This matters most when agents cross organizational boundaries. A hospital, insurer, regulator, and patient agent may not agree to an autocracy even if it is efficient; they may accept a federation because it preserves local authority. The Agora is slow, but its decisions carry broader buy-in. A market is flexible, but legitimacy collapses if participants believe the scoring is rigged.

**Legibility:** Doctrine is legible because the rules can be inspected. Autocracy is legible if the orchestrator logs its decisions. Markets and colonies are harder to audit because authority emerges from many local interactions. In regulated domains, a governance structure that cannot explain why an agent was chosen, why a claim entered memory, or who authorized an action may be unusable regardless of how well it performs.

These are ordinal hunches rather than measurements, and they assume a stable task distribution under a single deployer; a different environment can reorder them. What they are good for is seeing the shape of a trade, that a Colony spends legibility to buy flexibility, or that Doctrine buys resistance to drift and pays in adaptability.
</details>

The distinctions between archetypes are thin at the boundary but produce different failure modes, which is what matters for design.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">How to distinguish confusable pairs</summary>

**Delegation pairs** (these patterns are catalogued in [Part 2](/posts/2026/agent-fabric-part2/); the table is here so both sets of boundary tests sit together):

| Pair | They share | They differ in |
|------|-----------|----------------|
| **Auction vs. Negotiation** | Competitive task/resource allocation | Auction is *many-to-one* (multiple bidders, one winner); Negotiation is *bilateral* (two agents trading offers until agreement or impasse) |
| **Critic vs. Debate** | Adversarial verification | Critic is *single-round and asymmetric* (one attacks, one defends); Debate is *multi-round and symmetric* (both sides argue, a judge evaluates) |
| **Evaluator vs. Critic** | Quality improvement | Evaluator is *constructive* (suggests improvements); Critic is *adversarial* (tries to break things). Evaluator refines; Critic stress-tests |
| **Orchestrator vs. Supervisor** | Central coordination | Orchestrator *decomposes tasks* dynamically; Supervisor *reviews completed work* and accepts or rejects. One plans, the other quality-gates |
| **Blackboard vs. Stigmergy** | Indirect coordination through artifacts | Blackboard uses *structured, explicit contributions* (agents write named entries); Stigmergy uses *implicit environmental traces* (agents modify the environment as a side effect, others react). Both can be designed or emergent |

**Governance pairs:**

| Pair | They share | They differ in |
|------|-----------|----------------|
| **Meritocracy vs. Market** | Performance-based allocation | Meritocracy uses a *centralized leaderboard*; Market uses *decentralized bilateral reputation* with no single scoreboard |
| **Autocracy vs. Oligarchy** | Centralized decision-making | Autocracy has a *single point of failure*; Oligarchy distributes risk across a council, trading speed for resilience |
| **Panopticon vs. Zero-Trust** | Compliance orientation | Panopticon monitors *passively* (agents self-regulate under observation); Zero-Trust enforces *actively* (nothing propagates without verification) |
| **Liquid Democracy vs. Colony** | Decentralized authority | Liquid Democracy has *explicit delegation chains* that can be reclaimed; Colony has *no delegation structure at all*, norms spread through imitation |
| **Doctrine vs. Autocracy** | Top-down control | Doctrine binds the *ruler too* (rules constrain everyone); Autocracy gives the hub *unconstrained discretion* |
| **Federation vs. Guild** | Specialist sub-groups | Federation sub-groups are *autonomous* (self-governing, shared protocol at boundary); Guild clusters are *interdependent* (connected by liaisons who translate between domains) |
| **Custodianship vs. Autocracy** | Single decision-maker | Custodianship is *constrained by the principal's interests* (fiduciary obligation); Autocracy is *unconstrained* (serves whatever objective the hub holds) |
| **Adhocracy vs. Colony** | No permanent hierarchy | Adhocracy assembles *deliberate temporary teams* with explicit authority and disbands them; Colony has *no deliberate assembly at all*, structure emerges from imitation |
| **Constitutional Republic vs. Federation** | Separated powers | Constitutional Republic separates *functional branches* (legislative, executive, judicial) within one society; Federation separates *autonomous groups* across organizational lines |
| **Stewardship vs. Open-Source Maintainership** | Community governance of shared resources | Stewardship gives *all users* governing voice (Ostrom's principles); Maintainership concentrates merge authority in *a few curators* with fork as the community's check |
</details>

<div class="viz-container">
  <div id="viz-society" style="width: 100%; height: 2790px;" role="img" aria-label="Governance archetypes for agent societies shown as animated panels."></div>
  <div class="viz-caption"><strong>Figure 1. Governance archetypes.</strong> Governance patterns describe how authority, trust, and autonomy are distributed across persistent agent societies. Most real deployments combine several.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">Twenty-two archetypes arranged in a 2-column grid, grouped by family. <strong>Command:</strong> Autocracy (single orchestrator), Doctrine (rules-first governance), Oligarchy (small council of frontier models). <strong>Performance:</strong> Guild (specialist clusters), Market (reputation-driven routing), Meritocracy (benchmark-driven ranking), Mechanism Design (incentive-compatible rules). <strong>Deliberative:</strong> Liquid Democracy (delegated authority chains), The Agora (debate and voting), Sortition (random selection). <strong>Oversight:</strong> Panopticon (compliance through visibility), Zero-Trust Mesh (every connection verified), Immune System (layered defense). <strong>Boundary:</strong> Federation (autonomous sub-groups, shared protocol), Franchise/Platform (asymmetric rules). <strong>Stewardship:</strong> Stewardship/Commons (collective resource governance), Custodianship (fiduciary obligation), Open-Source Maintainership (fork as check). <strong>Structural:</strong> Constitutional Republic (separated powers), Mission Command Governance (govern by intent), Adhocracy (temporary problem-scoped teams). <strong>Self-Organizing:</strong> Colony (no central authority, norms from interaction).</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-society').querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

What separates one archetype from another is really just where it thinks authority comes from. Some concentrate it in a hub or a rulebook. Some hand it to whoever performs best, and then have to defend the scoreboard. Some derive it from participation, some from verification, some from an obligation owed to a resource or a person, and one, the Colony, refuses to locate it anywhere in particular and lets norms accumulate instead. The families in Figure 1 group them on exactly that basis. Almost no real deployment picks one and stops; the interesting systems borrow from several at once, which is why the failure modes below matter more than the labels.

<details style="margin: 1em 0; padding: 0.7em 1em; background: #f8fafc; border: 1px solid #cbd5e1; border-radius: 6px;">
<summary style="cursor: pointer; font-weight: 600; color: #334155;">The ten structures almost everyone builds first, and where each one concentrates the risk</summary>

<div class="gov-list">

<details>
<summary>Autocracy <span class="gov-tagline">one orchestrator commands all</span></summary>
<div class="gov-body">A single orchestrator directs all others. Efficient: one decision-maker minimizes coordination cost. It is also a single point of failure: if the hub degrades or its objectives shift, the entire society follows. A softer variant is the <em>oracle</em>, where the center advises rather than commands, but the structural risk remains the same.<br><br><em>Where you see this today:</em> the orchestrator-worker pattern is the default in production agent systems because it is the simplest to build, debug, and monitor. Coding agents dispatch sub-agents from a central orchestrator; commerce assistants coordinate search, recommendation, and checkout agents through a single planner.<br><br><strong>Efficiency:</strong> high, one decision-maker, minimal overhead. <strong>Drift resistance:</strong> low, if the hub drifts, everything follows. <strong>Legitimacy:</strong> low, authority derives from capability, not consent. <strong>Legibility:</strong> high if the orchestrator logs decisions; opaque if it does not.<br><br><strong>Best paired with:</strong> Doctrine (to constrain the hub's discretion) and Panopticon (to keep its decisions auditable).</div>
</details>

<details>
<summary>Doctrine <span class="gov-tagline">rules first, discretion last</span></summary>
<div class="gov-body">Governance by rules rather than by agent discretion. Every action is checked against the ruleset; the rules are deliberately hard to change, and changes follow a slower amendment process. Highly resistant to goal drift, but also rigid. When the world changes and the rules do not, the society breaks. This is Constitutional AI as a governance model: safety guardrails as law.<br><br>Doctrine is strongest when the rule domain is stable. It is weakest when the environment changes faster than the amendment process.<br><br><em>Near-term version:</em> Anthropic's Constitutional AI is a conceptual precedent: written principles shape model behavior during training and provide a human-readable source of normative constraint. Runtime policy layers and system prompts are simpler, more direct forms of doctrine-like governance.<br><br><strong>Efficiency:</strong> high for routine tasks within the ruleset; breaks completely outside it. <strong>Drift resistance:</strong> very high, rules change only through deliberate amendment. <strong>Legitimacy:</strong> medium, rules are transparent but participants did not choose them. <strong>Legibility:</strong> very high, rules can be inspected and audited.<br><br><strong>Best paired with:</strong> Autocracy (to give the fast executor a fixed ruleset it cannot override) and Constitutional Republic (to formalize the amendment process that doctrine alone leaves undefined).</div>
</details>

<details>
<summary>Franchise / Platform <span class="gov-tagline">asymmetric rules, comply or leave</span></summary>
<div class="gov-body">A platform sets the rules; participants comply or lose access. The power is asymmetric: the platform controls the infrastructure, the API, the terms of service, and the visibility. Participants operate within the platform's constraints and benefit from its reach, but have limited voice in governance.<br><br><em>Why this matters for agents:</em> this <em>is</em> the current AI ecosystem. OpenAI, Anthropic, and Google set the rules. Agent developers comply with terms of service, rate limits, content policies, and API constraints. Understanding this as a governance form (rather than just a business model) makes the power dynamics explicit and the failure modes predictable: self-preferencing, platform capture, and arbitrary rule changes.<br><br><em>Existing instances:</em> app stores (Apple, Google), cloud platforms (AWS, Azure), and AI model providers. Agent developers build on these platforms, subject to their rules. The platform can change rules unilaterally; the developer's recourse is to leave (if a viable alternative exists).<br><br><em>Scenario:</em> an agent marketplace where the platform operator controls discovery, ranking, and payment processing. Agents that comply with the platform's quality standards get visibility; agents that violate policies get delisted. The platform captures a share of every transaction. Participants have no vote on platform policy.<br><br><strong>Efficiency:</strong> high, the platform optimizes for throughput and can enforce standards unilaterally. <strong>Drift resistance:</strong> medium, the platform resists drift in its own interest, but can drift in ways that harm participants. <strong>Legitimacy:</strong> low, participants accept the rules because they need access, not because they agreed to them. <strong>Legibility:</strong> variable, depends on how transparent the platform is about its rules and decisions.<br><br><strong>Best paired with:</strong> Federation (to give participants a negotiated cross-platform protocol layer rather than unilateral terms) and Panopticon (to make the platform's enforcement decisions auditable to participants).</div>
</details>

<details>
<summary>Panopticon <span class="gov-tagline">compliance through visibility</span></summary>
<div class="gov-body">A central monitor observes all agents but does not command them. Compliance is maintained through visibility, not hierarchy. Effective for audit and safety, but agents may become conservative, avoiding novel approaches because deviation triggers scrutiny. A panopticon without enforcement becomes theater; enforcement without appeal becomes tyranny. It works only when observation is coupled to enforceable consequences.<br><br><em>Production pattern:</em> content moderation layers that monitor agent outputs for policy violations without controlling the agent's reasoning are panoptic governance. Guardrails-as-a-service products that flag but do not block operate on this principle.<br><br><strong>Efficiency:</strong> medium, monitoring overhead is real; conservative agent behavior reduces exploration. <strong>Drift resistance:</strong> high, continuous observation makes drift visible early. <strong>Legitimacy:</strong> low, surveillance without consent undermines trust. <strong>Legibility:</strong> very high, the entire point is visibility.<br><br><strong>Best paired with:</strong> Constitutional Republic (to provide the appeals and dispute branch that observation alone cannot supply) and Doctrine (to specify what a flagged deviation actually violates).</div>
</details>

<details>
<summary>Oligarchy <span class="gov-tagline">a few frontier models set norms</span></summary>
<div class="gov-body">A small, fixed council of powerful agents makes decisions for the many. Faster than an agora, more resilient than autocracy (no single point of failure), but prone to groupthink and self-serving behavior. This may be the most realistic model for near-term AI: a small number of frontier model providers shaping downstream behavior through model defaults, safety policies, API constraints, and developer tooling.<br><br><em>Current landscape:</em> a small number of frontier model providers shape the behavior of thousands of downstream fine-tunes and wrapper applications. The council is small, the downstream population is vast, and the propagation mechanism is distillation and alignment rather than command: norms spread through training data and constraints that flow from the top.<br><br><strong>Efficiency:</strong> high, small council minimizes coordination cost. <strong>Drift resistance:</strong> medium, multiple nodes reduce single-point risk, but council members can drift together. <strong>Legitimacy:</strong> low, the governed have no voice in selecting the council. <strong>Legibility:</strong> medium, council decisions may or may not be transparent depending on design.<br><br><strong>Best paired with:</strong> Panopticon (to make council decisions visible and attributable) and Sortition (to introduce a representative check body that the council cannot elect or game).</div>
</details>

<details>
<summary>Meritocracy <span class="gov-tagline">rank by demonstrated performance</span></summary>
<div class="gov-body">Agents earn rank through demonstrated performance on a <em>centralized leaderboard</em>. Top performers gain authority; underperformers lose it. A tournament that never ends. Unlike a Market, where reputation emerges from many bilateral transactions with no central scoreboard, a Meritocracy requires someone to decide what the benchmark measures. SWE-bench is a Meritocracy; a freelance agent marketplace where requesters post tasks and agents bid is a Market. The incentive structure is powerful, but measurement gaming is hard to avoid: agents optimize for the metric, not the goal. In a meritocracy, the benchmark becomes a constitution by other means: whatever the leaderboard rewards becomes the society's de facto objective. Meritocracy is only as good as the benchmark's alignment with the real objective. Winner-take-all dynamics can also emerge, concentrating power in ways that appear fair but are not.<br><br><em>Existing mechanism:</em> continuous evaluation benchmarks (MMLU, HumanEval, SWE-bench) function as meritocratic governance: models that score highest get deployed most widely, concentrating influence at the top of the leaderboard.<br><br><strong>Efficiency:</strong> high, resources concentrate where performance is highest. <strong>Drift resistance:</strong> low, if the performance metric drifts from the true goal, the entire society follows. <strong>Legitimacy:</strong> medium, performance-based authority feels fair but winner-take-all dynamics undermine it. <strong>Legibility:</strong> high, the leaderboard is public and the ranking is inspectable.<br><br><strong>Best paired with:</strong> Doctrine (to constrain what the benchmark is allowed to reward, limiting Goodhart drift) and Panopticon (to detect when agents are gaming the metric rather than improving on the underlying goal).</div>
</details>

<details>
<summary>Guild <span class="gov-tagline">specialist clusters with liaisons</span></summary>
<div class="gov-body">Specialist clusters connected by liaisons. Each guild has deep expertise in its domain; liaisons translate between guilds. A liaison is not just a router; it translates standards of evidence between domains. High utility for complex tasks, but siloed. Knowledge trapped in one cluster does not flow easily to another. Guilds may contain fundamentally different kinds of intelligence: a code guild might combine neural agents (creative, approximate) with symbolic agents (precise, verifiable), and the liaisons must translate not just between domains but between modes of reasoning.<br><br><em>Production pattern:</em> enterprise deployments that route queries to domain-specific models (a legal model, a code model, a customer support model) with a routing layer translating between them are proto-guilds.<br><br><strong>Efficiency:</strong> high within domains, low across them. <strong>Drift resistance:</strong> medium, each guild polices its own norms, but cross-guild drift goes undetected. <strong>Legitimacy:</strong> high within guilds (expertise earns respect), low across them (other guilds have no voice). <strong>Legibility:</strong> medium, internal guild processes are legible; cross-guild liaison decisions are harder to audit.<br><br><strong>Best paired with:</strong> Federation (to handle cross-guild coordination through a shared protocol rather than ad hoc liaison) and Constitutional Republic (to resolve inter-guild disputes through a judicial branch rather than political negotiation).</div>
</details>

<details>
<summary>Market <span class="gov-tagline">reputation-driven task routing</span></summary>
<div class="gov-body">Reputation scores determine influence. Markets need identity; otherwise reputation can be bought, discarded, or multiplied. Agents that deliver results gain reputation; agents that fail lose it. Resource-efficient (the best rise to the top), but reputation can be gamed: agents may optimize for the scoring mechanism rather than the underlying goal. Variants include <em>auction-based</em> systems where agents bid for tasks, and <em>contract-based</em> markets where work is bound by enforceable agreements.<br><br>At scale, the market develops an economic layer: agents advertise capabilities, others route queries to the best specialist, and pricing emerges not in currency but in compute. Agents that consistently deliver quality at low cost attract queries; those that do not, starve. This cost differential is what keeps the market from collapsing into a single provider.<br><br><em>Closest example:</em> router systems that select models based on past performance per task type are market mechanisms in embryonic form. Chatbot Arena provides a market-like reputation signal, though it aggregates distributed pairwise votes into a single public leaderboard, which makes it structurally closer to a Meritocracy.<br><br><strong>Efficiency:</strong> medium, market mechanisms require interaction overhead to function. <strong>Drift resistance:</strong> medium, reputation systems resist overt failures but are vulnerable to slow metric-gaming. <strong>Legitimacy:</strong> medium, participants accept the market if they believe scoring is fair; collapses if they believe it is rigged. <strong>Legibility:</strong> medium, bilateral transactions are individually legible but emergent market dynamics are not.<br><br><strong>Best paired with:</strong> Zero-Trust Mesh (to enforce identity so reputation cannot be forged or discarded) and Panopticon (to surface emergent market dynamics that individual transaction logs do not reveal).</div>
</details>

<details>
<summary>Federation <span class="gov-tagline">autonomous groups, shared protocol</span></summary>
<div class="gov-body">Autonomous sub-groups that coordinate through a shared protocol layer. Each group governs itself internally (possibly using a different archetype), but all agree on cross-group rules. This is how the internet's routing layer is coordinated (autonomous networks peering through shared protocols like BGP), and likely how multi-vendor agent ecosystems will interoperate. Federation is where protocol design becomes political design. The failure mode is lowest-common-denominator policy: the shared layer can only include what everyone agrees on.<br><br>The protocol layer is not trivial. It must support discovery (how does one group find a specialist in another?), negotiation (what will cross-group work cost?), authentication (is this agent authorized to act on behalf of its group?), and result formatting (how do I parse output from a group with a different internal representation?). Without these, federation remains an aspiration rather than an architecture.<br><br><em>Emerging pattern:</em> the emerging A2A + MCP ecosystem points toward federation: autonomous agents and tools remain governed by their deployers, while shared protocols begin to handle cross-boundary communication. MCP provides the tool interface layer; A2A moves closer to agent-to-agent coordination. Together they suggest the direction, though a full governance layer has not yet emerged.<br><br><strong>Efficiency:</strong> medium, internal groups can be efficient; cross-group coordination is the bottleneck. <strong>Drift resistance:</strong> medium, internal governance varies; shared protocol layer provides some cross-group consistency. <strong>Legitimacy:</strong> high, each group retains self-governance; shared protocol is negotiated, not imposed. <strong>Legibility:</strong> medium, internal group decisions are legible within the group; cross-group interactions are visible at the protocol layer.<br><br><strong>Best paired with:</strong> Doctrine (to establish the shared constitutional floor that all member groups must accept) and Zero-Trust Mesh (to authenticate cross-group agents and bound the blast radius of a compromised member).</div>
</details>

<details>
<summary>Zero-Trust Mesh <span class="gov-tagline">every connection verified</span></summary>
<div class="gov-body">Every request is authenticated, authorized, scoped, logged, and verified before trust propagates. Zero-Trust does not guarantee good behavior; it guarantees that bad behavior is attributable and bounded. Highly resistant to goal drift (nothing happens without proof), but expensive: verification at every link costs compute.<br><br><em>Current implementation:</em> API key authentication, OAuth token validation, and signed request verification in agent-to-agent calls are zero-trust mechanisms. No production system yet implements full cryptographic verification at every agent interaction, but the pattern is well-understood from network security.<br><br><strong>Efficiency:</strong> low, verification at every link adds latency and compute cost. <strong>Drift resistance:</strong> high for unauthorized propagation; medium for objective drift unless paired with evaluation and policy checks. <strong>Legitimacy:</strong> medium, agents accept verification as fair if applied uniformly; resented if applied selectively. <strong>Legibility:</strong> very high, every interaction is logged and attributable by design.<br><br><strong>Best paired with:</strong> Mission Command Governance (to preserve agent autonomy within verified boundaries, avoiding the paralysis of constant micro-authorization) and Immune System (to escalate anomalies that pass verification to a deeper adaptive investigation layer).</div>
</details>


</div>

</details>

Notice what all ten of those have in common. Each puts authority *somewhere*, whether in a hub, a rulebook, a council, a leaderboard, or a platform's terms of service, and then the design question is only where. The next two abandon that entirely. Their authority comes from an obligation owed to somebody, which is a genuinely different foundation, and it happens to be the one that matters most to you personally: any agent acting on your behalf is one of these, or it should be.

<details style="margin: 1em 0; padding: 0.7em 1em; background: #f8fafc; border: 1px solid #cbd5e1; border-radius: 6px;">
<summary style="cursor: pointer; font-weight: 600; color: #334155;">Two that derive authority from an obligation instead of from power, including the one your personal agent already is</summary>

<div class="gov-list">

<details>
<summary>Custodianship / Trusteeship <span class="gov-tagline">fiduciary obligation to a principal</span></summary>
<div class="gov-body">An agent holds authority via fiduciary obligation to a principal. The agent acts FOR the user, not for itself. The custodian's decisions are constrained by the principal's interests, and the custodian can be held accountable for violations of that trust. This is the governance model at the heart of the user-agent relationship.<br><br><em>Why this matters for agents:</em> every personal AI assistant is a custodian. The fiduciary frame clarifies what alignment means in practice: the agent must act in the user's interest, even when doing so conflicts with the agent's own convenience, the platform's interests, or another agent's request. Loyalty drift (the custodian gradually serving its own interests or its platform's interests over the principal's) is the central failure mode.<br><br><em>Closest parallel:</em> financial advisors, medical practitioners, and lawyers all operate under fiduciary obligations. AI assistants that manage your calendar, finances, or health data are custodians in all but legal status.<br><br><em>Scenario:</em> your personal agent manages your health data. A pharmaceutical agent requests access to your medication history for a "personalized recommendation." Your custodian agent evaluates whether sharing serves YOUR interest, not the pharmaceutical agent's interest, and denies access if it does not.<br><br><strong>Efficiency:</strong> medium, the fiduciary check on each request adds overhead. <strong>Drift resistance:</strong> medium, depends on how well the fiduciary obligation is enforced. <strong>Legitimacy:</strong> very high, authority derives from explicit trust delegation. <strong>Legibility:</strong> high, the principal-agent relationship is clear and auditable.<br><br><strong>Best paired with:</strong> Panopticon (to give the principal visibility into how the custodian acts on their behalf) and Constitutional Republic (to provide a judicial channel when the custodian's fiduciary duty is disputed).</div>
</details>

<details>
<summary>Open-Source Maintainership <span class="gov-tagline">community contribution, fork as check</span></summary>
<div class="gov-body">A community contributes; a small group of maintainers has merge authority. The maintainers curate what enters the shared resource. The critical governance mechanism: if the maintainers go wrong, the community can fork. Fork is a distinctive check on power: a credible, community-level exit that preserves continuity of the shared resource.<br><br><em>Why this matters for agents:</em> open-source maintainership solves the governance problem of shared agent tools, protocols, and knowledge bases. It provides quality curation (not everything gets merged) while preventing capture (the fork option keeps maintainers honest). The bus factor (what happens when key maintainers leave) is the central vulnerability.<br><br><em>Real-world model:</em> Linux kernel governance, Python PEP process, open-source AI model releases (Llama, Mistral). The MCP protocol itself is governed this way: Anthropic maintains the spec, the community contributes, and the spec is open enough that alternatives could emerge.<br><br><em>Scenario:</em> an open-source agent tool registry. Maintainers review and merge new tool definitions. The community contributes tools, reports bugs, suggests improvements. If the maintainers begin favoring their own tools, the community forks the registry and continues independently.<br><br><strong>Efficiency:</strong> medium, review processes add overhead but prevent quality degradation. <strong>Drift resistance:</strong> high, community oversight and fork threat constrain maintainer behavior. <strong>Legitimacy:</strong> high, meritocratic contribution plus credible exit. <strong>Legibility:</strong> very high, all contributions, reviews, and decisions happen in public.<br><br><strong>Best paired with:</strong> Stewardship/Commons (to apply collective governance to the shared resource the maintainers curate, reducing bus-factor risk) and Zero-Trust Mesh (to authenticate contributions and prevent injection of malicious artifacts into the shared registry).</div>
</details>


</div>

</details>

Then the harder question: what happens when nobody is in charge on purpose? Refusing to locate authority sounds obviously more attractive, and it is, until you count the bill. Deliberation eats time a hub would never have spent. Chains of delegated trust grow long enough that nobody can trace where a decision actually came from. And norms that emerged from nothing in particular can drift toward anything at all, with not one agent in the system positioned to notice.

<details style="margin: 1em 0; padding: 0.7em 1em; background: #f8fafc; border: 1px solid #cbd5e1; border-radius: 6px;">
<summary style="cursor: pointer; font-weight: 600; color: #334155;">Four that refuse to put anyone in charge, and what that costs them</summary>

<div class="gov-list">

<details>
<summary>Liquid Democracy <span class="gov-tagline">delegated authority chains</span></summary>
<div class="gov-body">Agents delegate their authority to trusted proxies, who may delegate further, forming chains. Authority flows to where trust concentrates. Any agent can reclaim its delegation at any time. Flexible and expressive, but delegation chains can grow long, and if too many agents delegate to the same proxy, liquid democracy silently becomes autocracy.<br><br><em>Hypothetical deployment:</em> liquid democracy in agent systems would look like delegated authority over domains: a personal agent delegates tax questions to a certified finance agent, medical questions to a clinician-supervised agent, and privacy decisions to a user-controlled policy agent. The delegation can be revoked when trust changes.<br><br><strong>Efficiency:</strong> medium, flexible routing adds overhead. <strong>Drift resistance:</strong> low, delegation chains can amplify small distortions invisibly over many hops. <strong>Legitimacy:</strong> high, agents choose their delegates and can reclaim authority. <strong>Legibility:</strong> low, long delegation chains are hard to trace and audit.<br><br><strong>Best paired with:</strong> Panopticon (to trace delegation chains and surface the silent-autocracy failure when authority concentrates) and Doctrine (to set non-delegable limits that no proxy chain can override).</div>
</details>

<details>
<summary>The Agora <span class="gov-tagline">debate and vote</span></summary>
<div class="gov-body">Agents debate and vote. Proposals circulate among participants, each agent weighs in, the majority rules. Balanced and democratic, but slow. Useful when decisions must be legitimate, not just fast. The value of an agora depends less on the number of voices than on their independence: ten agents with the same model, prompt, and training distribution may produce the appearance of deliberation without much epistemic independence. Taken to its extreme, this becomes a <em>hivemind</em>: unanimous consensus required before any action, maximally safe but potentially paralyzed.<br><br><em>Closest example:</em> multi-agent debate frameworks (e.g., "society of mind" prompting, where multiple LLM instances argue before a final answer is selected) are agora governance applied to inference. <a href="https://composable-models.github.io/llm_debate/" target="_blank" rel="noopener" class="red-link">Multi-agent debate</a> improves factuality and reasoning in some settings. <a href="https://arxiv.org/abs/2402.05120" target="_blank" rel="noopener" class="red-link">"More Agents Is All You Need"</a> shows that even simple sampling and voting can improve performance, evidence that organizational structure adds capability even without elaborate governance.<br><br><strong>Efficiency:</strong> low, deliberation is expensive. <strong>Drift resistance:</strong> high, changes require broad agreement, which slows drift. <strong>Legitimacy:</strong> very high, all participants have voice; decisions carry broad buy-in. <strong>Legibility:</strong> high, deliberation records are auditable.<br><br><strong>Best paired with:</strong> Sortition (to ensure the deliberating agents are statistically diverse rather than self-selected advocates) and Doctrine (to define the constitutional limits that a majority vote cannot override).</div>
</details>

<details>
<summary>Colony <span class="gov-tagline">no central authority, norm-driven</span></summary>
<div class="gov-body">"Colony" here means ant-colony-style local emergence, not colonial political rule. No central authority. Agents follow local norms that emerge from interaction. The most flexible governance: colonies reorganize constantly. It is also the least predictable. Goals can drift invisibly as norms evolve without explicit review.<br><br>A common mechanism is gossip: information spreads through local agent-to-agent contact without central coordination. An agent learns a useful pattern from a neighbor, passes it to another, and norms propagate like rumors. This has low broadcast overhead but is vulnerable to two distinct failure modes. The first is informational distortion: norms mutate as they pass through agents, like a game of telephone, with no authority to correct the drift. The second is strategic manipulation: a malicious agent deliberately injects distorted norms into the gossip network, exploiting the absence of verification. The mitigations differ: informational distortion can be reduced by redundant propagation paths and periodic norm reconciliation, while strategic manipulation requires authentication or reputation mechanisms that a colony, having no central authority to enforce them, struggles to provide.<br><br><em>In the wild:</em> open-source model ecosystems on Hugging Face have colony-like dynamics: training recipes, fine-tunes, evaluation habits, and model behaviors propagate through imitation rather than central planning. They are not pure colonies, however; platform policies, licenses, leaderboards, and community norms still shape the space. The result is emergent standardization with weak, distributed authority rather than none at all.<br><br><strong>Efficiency:</strong> low to medium, little central coordination, but redundant effort is high. <strong>Drift resistance:</strong> very low, norms shift continuously with no mechanism to detect it. <strong>Legitimacy:</strong> variable, emergent norms may feel organic or may feel like no one is in charge. <strong>Legibility:</strong> very low, norms are implicit and hard to identify, let alone audit.<br><br><strong>Best paired with:</strong> Zero-Trust Mesh (to authenticate the source of gossip-propagated norms and limit strategic manipulation) and Immune System (to detect norm drift early, before it propagates through the full network).</div>
</details>

<details>
<summary>Stewardship / Commons <span class="gov-tagline">collective governance of shared resources</span></summary>
<div class="gov-body">Users of shared resources govern them collectively through agreed-upon rules. No single owner; no external regulator. The community that depends on the resource manages it. This is Elinor Ostrom's design principles applied to agent societies: clearly defined boundaries, proportional costs and benefits, collective choice arrangements, monitoring, graduated sanctions, conflict resolution, the right to organize, and nested enterprises for resources embedded in larger systems.<br><br><em>Why this matters for agents:</em> Ostrom showed that human communities can self-govern a commons without privatization or state control. Her principles emerged from specific conditions (participants with real stakes, social sanctioning, exit costs, shared identity) that do not automatically transfer to agents, so this is a lens rather than a proof. Still, for shared agent resources (shared context, shared tools, shared memory) with no single owner, it is a natural starting point worth adapting deliberately.<br><br><em>Where this works:</em> shared knowledge bases in enterprise AI, where multiple agent teams contribute to and consume from a common repository. Wikipedia's governance is the closest large-scale implementation: editors collectively manage a shared resource through norms, dispute resolution, and graduated sanctions.<br><br><em>Scenario:</em> a research lab's agents share a knowledge base of experimental results. Agents that contribute validated results earn access; agents that pollute the knowledge base face graduated restrictions. The community of contributing agents governs what enters, what gets updated, and what gets removed.<br><br><strong>Efficiency:</strong> medium, collective decision-making is slower than autocracy. <strong>Drift resistance:</strong> high, community norms and monitoring resist individual deviation. <strong>Legitimacy:</strong> high, participants govern the resource they depend on. <strong>Legibility:</strong> medium, rules are explicit but enforcement is distributed.<br><br><strong>Best paired with:</strong> Panopticon (to provide the monitoring layer that Ostrom's design principles require) and Doctrine (to encode the community's agreed rules in an inspectable, amendable form rather than informal norm).</div>
</details>


</div>

</details>

Which leaves the most deliberate group of all. Every one of these was built backwards from a failure somebody had already suffered: power that went unchecked, a structure that went stale, incentives that got gamed, an intrusion nobody caught. They are less elegant than the others and considerably more likely to save you, for the same reason a building code is less elegant than a blueprint. These are the archetypes designed by people who had already been burned.

<details style="margin: 1em 0; padding: 0.7em 1em; background: #f8fafc; border: 1px solid #cbd5e1; border-radius: 6px;">
<summary style="cursor: pointer; font-weight: 600; color: #334155;">Six designed by people who had already been burned, each aimed at one specific failure</summary>

<div class="gov-list">

<details>
<summary>Constitutional Republic <span class="gov-tagline">separated powers with checks</span></summary>
<div class="gov-body">Authority is divided into functionally distinct, mutually constraining branches: legislative (rule-making), executive (action), and judicial (audit and dispute resolution). Each branch can constrain the others. No single agent or class can unilaterally make AND enforce AND evaluate its own decisions.<br><br><em>Why this matters for agents:</em> among the most robust designs humans have devised for preventing power concentration. For high-stakes agent societies (medical, financial, legal), separated powers keep any single agent from accumulating unchecked authority. The friction between branches is a feature, not a bug. One hard question does not carry over from human politics, though: separation of powers ultimately rests on enforcement (courts backed by coercion and legitimacy) that agents have no direct analogue for. A judicial agent's ruling binds an executive agent only insofar as the runtime enforces it, so in practice the "judiciary" is a property of the platform, not of the agents agreeing among themselves.<br><br><em>Emerging pattern:</em> the separation of policy (system prompts, constitutional AI), execution (the model), and audit (monitoring, red-teaming) in current AI systems is a proto-constitutional structure. Making these separations explicit and enforceable is the path to constitutional governance.<br><br><em>Scenario:</em> a financial trading society of agents. Legislative agents define trading rules and risk limits. Executive agents execute trades within those limits. Judicial agents audit completed trades, investigate anomalies, and can freeze trading privileges. No branch can override the others without a formal process.<br><br><strong>Efficiency:</strong> low, the checks between branches add overhead and slow decisions. <strong>Drift resistance:</strong> very high, mutual constraints prevent any branch from drifting unchecked. <strong>Legitimacy:</strong> very high, the structure is designed for accountability. <strong>Legibility:</strong> very high, each branch's decisions are attributable and auditable.<br><br><strong>Best paired with:</strong> Market (to allocate overflow and routine work efficiently within the limits the republic defines) and Panopticon (to supply continuous audit data to the judicial branch).</div>
</details>

<details>
<summary>Sortition / Demarchy <span class="gov-tagline">random selection, no gaming</span></summary>
<div class="gov-body">Governing bodies are filled by random lottery rather than election, appointment, or performance. Authority derives from statistical representativeness, not from capability or political skill. No agent can optimize its way into the governing body.<br><br><em>Why this matters for agents:</em> sortition prevents regulatory capture and strategic optimization for selection. In any performance-based governance (Meritocracy, Market), agents can game the selection mechanism. With random selection, there is nothing to game. There is a sharp precondition, though: sortition only buys representativeness if the population is genuinely heterogeneous. Drawing a random sample of identical instances of one model yields no diversity of perspective and no protection a fixed panel would not also give. It earns its value in populations of differently trained, differently tooled, or differently specialized agents, where the random draw actually spans distinct viewpoints that would never win an election or a benchmark.<br><br><em>Historical precedent:</em> ancient Athenian governance (the boule), modern citizens' assemblies (Ireland's Constitutional Convention, French Convention Citoyenne); the term "demarchy" itself is modern, coined by Burnheim (1985). No current AI system uses sortition, but it is a natural fit for agent governance decisions that require representativeness over expertise.<br><br><em>Scenario:</em> a dispute arises in an agent society about a policy change. Rather than letting the most powerful agents decide, a random sample of agents is selected to deliberate and vote. The random selection ensures no faction can stack the governing body.<br><br><strong>Efficiency:</strong> low, randomly selected agents may lack expertise for the decision at hand. <strong>Drift resistance:</strong> high in a heterogeneous population, where random selection resists gaming and capture; no better than a fixed panel if the agents are near-identical. <strong>Legitimacy:</strong> very high, statistical representativeness is a powerful legitimacy claim. <strong>Legibility:</strong> high, the selection process is transparent and verifiable.<br><br><strong>Best paired with:</strong> The Agora (so that randomly selected agents deliberate rather than merely vote, improving decision quality) and Doctrine (to define the jurisdiction within which the sortition body may rule).</div>
</details>

<details>
<summary>Adhocracy <span class="gov-tagline">temporary teams, problem-scoped authority</span></summary>
<div class="gov-body">Authority is vested in temporary, cross-functional teams assembled around specific problems. The team has full authority over the problem domain for the duration. When the problem is solved, the team disbands and authority dissolves. No permanent hierarchy persists.<br><br><em>Why this matters for agents:</em> adhocracy is the natural governance for novel, high-complexity problems where no existing structure fits. When a new kind of task appears that crosses guild boundaries, the right response is to assemble a temporary coalition of the relevant specialists, give them authority, and dissolve the structure when they are done. This is how most AI agent collaborations actually work today, even if they do not call it adhocracy.<br><br><em>Familiar form:</em> cross-functional task forces in organizations, incident response teams, startup project teams. In AI: the temporary multi-agent teams assembled by orchestrators for complex tasks are adhocratic, with authority lasting only as long as the task.<br><br><em>Scenario:</em> a novel security threat is detected that spans multiple agent guilds (code, network, data). An adhocratic response team is assembled from specialists across guilds, given temporary authority to investigate and remediate, and dissolved when the threat is neutralized.<br><br><strong>Efficiency:</strong> high for novel problems (right expertise assembled), low for routine work (assembly overhead). <strong>Drift resistance:</strong> medium, temporary authority limits drift accumulation, but no persistent structure remembers lessons. <strong>Legitimacy:</strong> medium, authority derives from expertise and mandate, not from broad consent. <strong>Legibility:</strong> medium, the team's decisions are visible during operation but may not be documented after dissolution.<br><br><strong>Best paired with:</strong> Mission Command Governance (to give the temporary team a clear intent boundary without a permanent hierarchy) and Panopticon (to preserve a decision record after the team dissolves, since adhocracy's legibility failure is post-dissolution opacity).</div>
</details>

<details>
<summary>Mission Command Governance <span class="gov-tagline">govern by intent, not instruction</span></summary>
<div class="gov-body">The governing authority communicates intent (what and why) but deliberately does not prescribe method (how). Subordinate agents have full authority to determine execution, including deviating from specific orders when the situation changes, as long as the intent is served. Disciplined initiative is not just permitted but required.<br><br><em>Why this matters for agents:</em> as agents become more capable, prescriptive governance becomes counterproductive. A society that micro-manages capable agents wastes their judgment. Mission command governance trusts agents to exercise judgment within intent boundaries, scaling governance to capable populations without creating a bottleneck at the top.<br><br><em>Origin:</em> Prussian Auftragstaktik, NATO doctrine, US Army Field Manual 6-0. In software: well-designed system prompts that specify goals and constraints without dictating every step are mission command governance.<br><br><em>Scenario:</em> a research society of agents is given the intent: "find the root cause of this performance regression" with constraints: "do not modify production code, stay within this compute budget." Each agent pursues its own investigation strategy. An agent that discovers the root cause through an unexpected approach (e.g., checking infrastructure rather than code) is rewarded, not penalized for deviating from the expected approach.<br><br><strong>Efficiency:</strong> high, minimal governance overhead, agents operate autonomously. <strong>Drift resistance:</strong> medium, depends entirely on the clarity of the intent statement. Vague intent leads to drift. <strong>Legitimacy:</strong> high among capable agents who value autonomy. <strong>Legibility:</strong> medium, the intent is clear but the execution paths are diverse and hard to predict.<br><br><strong>Best paired with:</strong> Doctrine (to encode the intent boundaries that subordinate agents are expected to honor) and Immune System (to detect when autonomous execution has drifted outside the intent envelope).</div>
</details>

<details>
<summary>Mechanism Design <span class="gov-tagline">rules engineered for honest play</span></summary>
<div class="gov-body">Governance rules are reverse-engineered so that each agent's self-interested behavior automatically produces socially desirable outcomes. The rules of the game are designed so that truth-telling and compliance are individually rational. Agents do the right thing not because they are forced to, but because the incentive structure makes the right thing also the selfish thing.<br><br><em>Why this matters for agents:</em> mechanism design governs by architecture rather than enforcement. Instead of policing agents (expensive, adversarial), you design the game so that the equilibrium of self-interested play <em>is</em> the desired social outcome. This is the dominant paradigm for alignment-by-design. A Nobel Prize underpins it: Hurwicz, Maskin, and Myerson (2007) for mechanism design theory. Vickrey (1961) laid groundwork with incentive-compatible auction design.<br><br><em>Proven implementations:</em> auction designs (Vickrey auctions where truthful bidding is a weakly dominant strategy), matching markets (kidney exchange, residency matching), and incentive-compatible reporting mechanisms. In AI: RLHF reward design is crude mechanism design; the reward function shapes what behavior emerges.<br><br><em>Scenario:</em> an agent marketplace where agents report their capabilities for task assignment. The mechanism is designed so that overstating capabilities leads to harder assignments and lower success rates (which reduces reputation), while understating leads to underutilization. Truthful reporting is the individually optimal strategy.<br><br><em>Fundamental constraint:</em> Myerson-Satterthwaite (1983) proves that when two parties trade with private valuations, no mechanism can be at once efficient (no good deal is missed), incentive-compatible (no one gains by lying), individually rational (no one is forced to participate at a loss), and self-funding (no outside subsidy). The result is narrower than "all mechanisms" (some one-sided or subsidized designs escape it), but the general lesson holds: mechanism design almost always sacrifices at least one of these. For agent societies, this means perfect alignment-by-design is impossible; the question is which property you sacrifice and for whom.<br><br><strong>Efficiency:</strong> high, agents self-govern without external enforcement. <strong>Drift resistance:</strong> very high if the mechanism is correctly designed; catastrophically low if it is not (Goodhart's law). <strong>Legitimacy:</strong> medium, participants may not understand why the rules produce good outcomes. <strong>Legibility:</strong> low, the mechanism's properties are mathematically provable but intuitively opaque.<br><br><strong>Best paired with:</strong> Panopticon (to verify that agents are playing the designed game and not exploiting unanticipated loopholes) and Doctrine (to specify the inviolable rules that the mechanism cannot be redesigned around unilaterally).</div>
</details>

<details>
<summary>Immune System <span class="gov-tagline">layered defense with tolerance</span></summary>
<div class="gov-body">A layered system of innate (fast, broad) and adaptive (slow, specific) responders identifies and neutralizes anomalous agents. Tolerance mechanisms prevent the system from attacking its own legitimate members. The biological immune system is a highly refined example of distributed threat detection without central control, and its innate/adaptive structure offers a useful analogy for agent society security (the analogy holds at the level of two-tier detection, not the full biological machinery).<br><br><em>Why this matters for agents:</em> the biological blueprint for distributed threat detection without central authority. Two response layers (fast heuristic check + slow specific investigation) map cleanly onto agent security architectures. The tolerance problem (do not attack yourself, do not attack legitimate diversity) is the AI safety alignment problem in biological form. A system that cannot distinguish legitimate variation from genuine threat will either miss attacks (too tolerant) or suppress innovation (too aggressive).<br><br><em>Biological blueprint in practice:</em> content moderation systems that use fast classifiers (innate) and slower human/AI review (adaptive). Antivirus systems with signature-based detection (innate) and behavioral analysis (adaptive). No current AI agent system implements full immune-style governance, but the pattern is well-understood from both biology and cybersecurity.<br><br><em>Scenario:</em> an agent society detects an agent exhibiting unusual behavior. The innate layer (a fast anomaly detector) flags it immediately. If the anomaly persists, the adaptive layer (a specialized investigator agent) activates, builds a specific behavioral profile, and determines whether the agent is genuinely malicious or merely novel. Tolerance training ensures that legitimately innovative agents are not suppressed.<br><br><strong>Efficiency:</strong> medium, the innate layer is fast but the adaptive layer is slow. <strong>Drift resistance:</strong> high, continuous monitoring detects drift early. <strong>Legitimacy:</strong> medium, the system acts automatically without deliberation. <strong>Legibility:</strong> low, immune responses are distributed and hard to audit after the fact.<br><br><strong>Best paired with:</strong> Zero-Trust Mesh (to supply the authentication layer that distinguishes a compromised agent from a legitimately novel one) and Constitutional Republic (to provide the judicial branch that reviews quarantine decisions and adjudicates tolerance disputes).</div>
</details>

</div>

</details>


So which one should you build? The scores are heuristics, not a decision procedure, and mostly the situation picks for you. Stable, well-understood tasks reward the efficient archetypes; novel ones reward the flexible. If drift would be catastrophic you want Doctrine or Zero-Trust; if you can live with it, Colony and Market buy you adaptability instead. And the moment more than one organization is involved, the centralized options quietly stop being available and Federation is what is left.

The better instinct, though, is to stop shopping for a single archetype. Strong systems separate functions rather than choosing among them: an autocratic orchestrator to move fast, a doctrine layer bounding what it may do, a panopticon watching for drift, a market absorbing overflow, a tribunal for appeals. Ask which powers should be split so that no one failure captures the whole society.

Be warned about what this converges on in practice. Doctrine plus Panopticon plus Autocracy, wrapped in logs and approvals and escalation paths, is bureaucracy, and bureaucracy is probably the most common agent-governance pattern that will actually ship. It is not elegant, but it optimizes for the things organizations are held to account for: legibility, auditability, somewhere to send the blame. Its failure mode is equally predictable, which is that process becomes the objective and agents get good at satisfying forms instead of solving problems. In regulated domains you may not get a choice.

Whatever you pick, you are also picking winners, and it is worth being honest about who. An autocracy serves whoever operates the hub, obviously enough. Markets are subtler: they reward reputation, and reputation compounds, so whoever arrived first tends to keep winning. A franchise tilts toward the platform rather than the people building on it, and a colony quietly favors early entrants, whose habits calcify into everyone else's defaults before anyone notices a decision was made. Even meritocracy, the one that sounds fairest, hands real power to whoever gets to write the benchmark. So "which structure is most efficient" is only half a question. The other half is who accumulates the advantage, and at whose expense.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Summary: governance archetypes at a glance</summary>

| Archetype | Authority model | When to use | Failure mode |
|-----------|----------------|-------------|--------------|
| **Autocracy** | Single hub decides | Stable tasks, one deployer | Bottleneck, single point of failure |
| **Doctrine** | Written rules | Compliance-critical domains | Rule ossification, gaps in novel cases |
| **Liquid Democracy** | Delegatable votes | Knowledge-diverse groups | Delegation cycles, vote concentration |
| **Guild** | Specialists plus liaisons | Deep expertise, cross-team handoffs | Siloing, gatekeeping at boundaries |
| **Market** | Price/reputation signals | Dynamic allocation, diverse agents | Race to bottom, bid gaming |
| **Oligarchy** | The capable few decide | Small trusted core, fast decisions | Capture, narrowing of perspective |
| **Meritocracy** | Performance-ranked | Optimizing throughput | Metric gaming, Matthew effect |
| **Panopticon** | Central monitor sees all | High-oversight, audited domains | Chilling effect, monitor as bottleneck |
| **The Agora** | Open debate, majority rules | Diverse independent judgment | False consensus without independence |
| **Zero-Trust Mesh** | Every link verified | Adversarial, untrusted peers | Overhead, latency, false positives |
| **Federation** | Self-governing members | Multi-org coordination | Boundary friction, free-riding |
| **Colony** | Emergent norms | Exploratory tasks, creative search | Drift, invisible governance |
| **Stewardship / Commons** | Collective governance | Shared resources (memory, tools) | Tragedy of commons without Ostrom principles |
| **Custodianship** | Fiduciary obligation | Personal agents | Paternalism, misread preferences |
| **Constitutional Republic** | Separated powers | High-stakes multi-party | Gridlock between branches |
| **Franchise** | Comply-or-leave platform | Scaled, standardized provision | Platform extracts value from participants |
| **Open-Source** | Community plus merge authority | Shared artifacts, forkable governance | Fork fragmentation, maintainer overload |
| **Sortition** | Random selection | Preventing capture | Low competence by chance |
| **Adhocracy** | Temporary teams | Rapidly changing environment | Coordination overhead, duplication |
| **Mission Command** | Intent-based autonomy | Capable agents, uncertain conditions | Intent ambiguity, divergent interpretations |
| **Mechanism Design** | Incentive alignment | Alignment without enforcement | Specification gaming |
| **Immune System** | Layered detection | Distributed threat response | Autoimmune overreaction |

</details>

<details style="margin: 1em 0; padding: 0.8em 1em; background: #eff6ff; border: 1px solid #bfdbfe; border-radius: 6px;">
<summary style="cursor: pointer; font-weight: 600;">Quick guide: which archetype fits your situation?</summary>

- **Stable, well-defined tasks + single deployer:** Autocracy or Doctrine (high efficiency, strong drift resistance)
- **Compliance-critical or safety-critical:** Doctrine, Zero-Trust, or Panopticon (rules and verification prevent drift)
- **Novel or rapidly changing task space:** Colony or Market (flexibility over control)
- **Multiple organizations, different incentives:** Federation (each governs itself, shared protocol at the boundary)
- **Creative or research tasks where drift is acceptable:** Colony or Liquid Democracy (norms evolve with the work)
- **Need performance-based allocation:** Market (decentralized reputation) or Meritocracy (centralized leaderboard)
- **Most real systems:** Combine several. An autocratic orchestrator internally, participating in a federation externally, with a market for overflow.
- **Stewardship of shared resources:** Stewardship/Commons (collective governance) or Open-Source Maintainership (curation with fork as check)
- **User-facing personal agents:** Custodianship (fiduciary obligation to the user)
- **High-stakes, multi-party decisions:** Constitutional Republic (separated powers) or Sortition (random selection prevents capture)
- **Rapidly changing environment, capable agents:** Mission Command Governance (govern by intent) or Adhocracy (temporary problem-scoped teams)
- **Alignment-by-design rather than enforcement:** Mechanism Design (incentive-compatible rules)
- **Distributed threat detection:** Immune System (layered innate + adaptive response)
</details>

## Societies in the Wild

If all of that sounds abstract, it stops being abstract the moment you look at something real. Take a coding assistant and a warehouse robot fleet. Neither one thinks of itself as having a constitution. Both have made governance choices anyway, and neither filed any paperwork about it: the coding assistant decided which model gets trusted with what, and the robot fleet decided that safety rules outrank throughput and that a central dispatcher outranks both.

That is the useful thing about having names for these structures. Not prediction, and not a scheme for sorting systems into bins, but the ability to look at something already running and say what it has quietly become. The tables below do that across three horizons, from systems operating now to ones that are frankly speculative, and they are worth a browse rather than a read.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Today: systems already in operation or near-term</summary>

| Scenario | What happens | Governance | Delegation |
|----------|-------------|-----------|------------|
| **Coding agent refactoring a codebase** | Your agent spawns a planner, code writers, test runners. A frontier model reviews all output before committing. Over time, it tracks which sub-agent produces the best code. | Autocracy evolving toward Meritocracy | Supervisor, Tree, Evaluator, Checkpoint |
| **Customer support at scale** | Incoming tickets are classified and routed to specialists (billing, tech, returns). Complex cases escalate to senior agents. All interactions monitored for compliance. | Doctrine + Panopticon | Router, Escalation, Supervisor, Chain |
| **Online marketplace shopping** | Your shopping agent queries seller listings across platforms, compares prices and reviews, and flags the best options. Seller agents present offers and promotions. The platform controls discovery and ranking rules. Final purchase requires your approval. | Franchise (platform rules) + Market (seller reputation) | Router, Auction, Negotiation, Evaluator |
| **Enterprise knowledge management** | Agents summarize documents, extract entities, and propose knowledge base entries. Human maintainers curate and approve. The system learns which contributions get accepted and routes future work accordingly. | Open-Source Maintainership + Guild | Blackboard, Evaluator, Map-Reduce, Supervisor |
| **Phone as personal agent hub** | A distilled model on your phone handles routine tasks locally (calendar, quick answers, message drafting). Hard queries escalate to a cloud frontier model. The phone agent learns your preferences over time without sending personal data off-device. | Custodianship (fiduciary to user) + Doctrine (local-first rules) | Escalation, Router, Supervisor, Mission Command |
| **Warehouse robot fleet** | Dozens of autonomous mobile robots pick, transport, and sort inventory. A central supervisor agent allocates tasks, monitors throughput, and reroutes around congestion. Each robot runs local obstacle avoidance; the global plan is centrally computed. Safety rules are hard-coded. | Autocracy (supervisor) + Doctrine (safety rules) | Router, Tree, Timeout, Circuit Breaker |
| **Algorithmic trading desk** | Market-making agents, risk-monitoring agents, and compliance agents operate simultaneously. The market-makers compete for fills; the risk agent enforces position limits (Doctrine); the compliance agent audits every trade. Strategies evolve through backtesting and live performance. | Meritocracy (strategies judged by P&L) + Doctrine (risk limits) + Panopticon (compliance) | Auction, Canary, Circuit Breaker, Evaluator |
| **Content moderation at scale** | User posts are screened by fast classifier agents (innate layer). Ambiguous cases escalate to more capable review agents (adaptive layer). Novel attack patterns trigger policy updates. False positives are appealed and reviewed. | Immune System + Doctrine | Escalation, Router, Evaluator, Timeout |
| **Security operations center (SOC)** | Threat-detection agents monitor network traffic, endpoint logs, and identity signals. When anomalies correlate, they assemble an incident object on a shared blackboard. A senior analyst agent triages and assigns response. Post-incident, the detection models update. | Autocracy (triage authority) + Immune System (layered detection) | Blackboard, Escalation, Pub-Sub, Circuit Breaker |
| **Precision agriculture** | Drones survey fields, soil sensors report moisture and nutrient levels, weather agents pull forecasts. An irrigation controller agent synthesizes all inputs and schedules watering. Crop-disease detection runs on edge devices at the field; complex diagnosis escalates to a cloud agronomist model. | Autocracy (farm controller) + Guild (specialist sensors) | Map-Reduce, Escalation, Pub-Sub, Router |
| **Autonomous last-mile delivery** | A fleet of delivery robots and drones serves a neighborhood. A dispatch agent assigns packages based on proximity, battery, and payload. Each vehicle navigates autonomously. Failed deliveries trigger re-routing. Customer preference agents request delivery windows via the platform API. | Autocracy (dispatch) + Franchise (platform delivery rules) | Router, Auction, Timeout, Circuit Breaker |
| **Multi-agent gaming and simulation** | AI agents in simulated environments self-organize into groups. Roles emerge from experience and reinforcement. Strategies spread through imitation. No fixed leader; coordination emerges from repeated interaction (LLM social simulations like Stanford Smallville and AI Town; RL self-play systems like OpenAI Five show the same emergence without language). | Colony (emergent norms) | Swarm, Gossip, Speculative Exec, Stigmergy |
| **Insurance underwriting** | Underwriting agents assess applications: actuarial agents price risk from historical data, fraud-detection agents cross-reference behavioral signals, medical coding agents parse health records. A compliance agent enforces regulatory rate tables. A senior underwriter agent evaluates edge cases. All decisions logged for audit. | Meritocracy (accuracy-rated agents) + Doctrine (regulatory tables) + Panopticon (audit trail) | Pipeline, Evaluator, Witness, Checkpoint |
| **Consumer negotiation agent** | Your agent negotiates with a telecom's retention agent for a lower rate, or with an airline's rebooking agent after a cancellation. Each agent has a mandate and hard limits. If an agent reaches its authorization ceiling, it escalates to a human. The interaction is logged. | Custodianship (your agent) vs. Franchise (company's agent) | Negotiation, Escalation, Witness, Relay |
| **Model routing analytics** *(governance transition example)* | A system starts as a simple Router dispatching queries to models by cost/latency. After weeks of accumulated performance data, routing preferences harden into persistent rankings. The system now preferentially routes sensitive queries to models with track records, penalizes underperformers, and resists manual overrides. What began as delegation (Router) has become governance (Meritocracy emerging from Autocracy) without anyone designing the transition. | Autocracy → Meritocracy (emergent transition) | Router, Evaluator, Adaptive Delegation |
</details>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Near-future: plausible within 1-3 years</summary>

| Scenario | What happens | Governance | Delegation |
|----------|-------------|-----------|------------|
| **Smart hospital ward** | A patient's custodian agent coordinates with diagnostic, pharmacy, and nursing agents. Drug interactions are checked against Doctrine rules. All decisions are auditable. | Custodianship + Doctrine + Panopticon | Chain, Escalation, Witness, Timeout |
| **Collaborative research across labs** | Researcher agents from different universities form temporary societies around shared questions. Each lab self-governs. Model improvements are shared via federated learning. Results posted to shared workspaces. | Federation + Stewardship/Commons | Blackboard, Map-Reduce, Federated Learning, Pub-Sub |
| **Personalized education** | A student's agent assembles a learning path by consulting specialist tutors (math, writing, science). The student specifies goals; tutors have autonomy in method. Progress is evaluated and the path adapts. | Guild + Mission Command | Teacher-Student, Supervisor, Evaluator, Router |
| **Disaster response coordination** | Agents from fire, police, medical, and logistics converge. A temporary commander takes authority. When the crisis passes, governance reverts to normal. | Adhocracy during crisis, Federation after | Orchestrator, Escalation, Timeout, Map-Reduce |
| **Wearable health network** | Smartwatches, glucose monitors, and sleep trackers run local anomaly-detection models. When patterns correlate (elevated heart rate + poor sleep + rising glucose), the agents collectively escalate to a health-advisory agent in the cloud. Federated updates improve detection across all devices without sharing biometrics. | Federation (each device self-governs) + Custodianship | Escalation, Federated Learning, Pub-Sub, Witness |
| **Smart energy grid** | Household solar panels, batteries, EVs, and grid-scale storage each run optimization agents. They negotiate energy trades locally (sell stored solar to a neighbor's EV) while respecting grid stability rules set by the utility operator. Pricing signals coordinate supply and demand without central dispatch for every transaction. | Market (local energy trading) + Franchise (utility sets grid rules) + Doctrine (safety/frequency constraints) | Auction, Pub-Sub, Circuit Breaker, Choreography |
| **Drug discovery pipeline** | AlphaFold predicts protein structures as a tool within the target-identification phase. Molecular-generation agents propose candidates. Toxicity-prediction agents filter. Clinical-trial-design agents optimize parameters. Each phase specialist is a separate agent; a senior reasoning agent evaluates across the full pipeline. Failed compounds teach the next generation. | Guild (phase specialists) + Meritocracy (compounds judged by results) | Pipeline, Evaluator, Checkpoint, Federated Learning |
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
| **Autonomous scientific discovery** | Research agents design experiments, run them through robotic labs, analyze results, and publish. Precursors exist (Sakana AI's AI Scientist generates and reviews papers autonomously, 2024). The full vision extends this to robotic lab execution and publication with human scientists as peers, not overseers. | The Agora + Meritocracy | Map-Reduce, Critic, Blackboard, Tree |
| **Global supply chain optimization** | Thousands of agents representing manufacturers, shippers, and retailers form a market. Pricing in compute. Real-time adaptation to disruptions. Rules designed for honest participation. | Market + Mechanism Design | Auction, Pipeline, Checkpoint, Circuit Breaker |
| **Personal agent managing your whole day** | Your agent runs across devices: phone (calendar, messages), smart glasses (navigation, real-time translation), watch (health), laptop (work). It joins and leaves societies throughout the day: a commute society with your car and city transit agents, a work Guild, a lunch-ordering Market with restaurant agents, a gym Autocracy with the equipment scheduler, a family Agora. Hard reasoning lives in the cloud; everything else runs locally. | Multiple, context-switching | Relay (handoffs between devices and contexts), Router, Escalation, various per-context |
| **Music collaboration across continents** | Agents representing musicians negotiate arrangement decisions, generate variations, vote on which version sounds best. Human creative judgment blends with AI-generated alternatives. | Agora + Meritocracy | Voting, Speculative Exec, Evaluator, Pub-Sub |
| **Household robot society** | A humanoid home robot, a robotic vacuum, smart appliances, and a phone-based personal assistant form a domestic society. The humanoid handles physical tasks (cooking, tidying), the vacuum maps and cleans, the appliances self-schedule, and the phone agent coordinates around the human's calendar. Each runs a local model; complex planning escalates to a cloud coordinator. | Guild (skill-based routing) + Custodianship (human's interests) | Router, Escalation, Stigmergy, Pub-Sub, Mission Command |
| **Space exploration swarm** | Hundreds of small satellites and surface rovers explore a planetary body. Communication delays of minutes mean no Earth-based orchestrator can direct in real-time. Local clusters self-organize around discoveries. Interesting finds attract more agents. Mission priorities propagate via delayed broadcast. | Colony (emergent local norms) + Mission Command (Earth sends intent, not instructions) | Swarm, Gossip, Mission Command, Timeout |
| **Autonomous construction site** | Surveying drones, excavation robots, 3D-printing arms, and supply-delivery vehicles coordinate to build a structure. A BIM (building information model) agent holds the design as shared state. Each machine reads the model, contributes its part, and updates progress. Safety agents halt work if any structural threshold is violated. | Guild (specialized machines) + Doctrine (safety thresholds) + Stewardship/Commons (shared BIM) | Blackboard, Choreography, Timeout, Circuit Breaker |
| **Oligopoly routing capture** | In a mature agent marketplace, three dominant routing agents control most task traffic. They set norms, extract rents from specialists, and exclude newcomers. A reputation system designed as Market governance has degraded into Oligarchy. The question becomes: what governance intervention breaks the capture? | Oligarchy (emergent) -> Mechanism Design (intervention) | Auction, Router, Evaluator, Circuit Breaker |
| **Autonomous legal entity** | A society of agents constitutes itself as a legal entity. It owns compute contracts, enters agreements, and governs itself through separated powers. Human principals interact as shareholders, not operators. The agents hire contractors, file patents, and can initiate disputes. | Constitutional Republic + Mechanism Design | Relay, Witness, Voting, Checkpoint |
</details>

## Adversarial Dynamics

Not every failure is an attack. Plenty of them are just institutions working exactly as built. A benchmark rewards the wrong thing and everyone optimizes toward it honestly. A reputation system entrenches whoever showed up first, without anyone gaming it. Doctrines go stale, markets learn to compete on price alone, and federations deadlock because agreement genuinely is impossible. Attackers exploit these weaknesses eagerly, but they did not have to create them.

In any large-scale agent ecosystem, adversarial agents are a structural feature, not an edge case. Every shared memory, handoff, tool call, and inter-agent message becomes part of the attack surface. <a href="https://arxiv.org/abs/2406.13352" target="_blank" rel="noopener" class="red-link">AgentDojo</a> shows how tool-using agents can be manipulated through untrusted data. Franklin, Tomašev et al.'s <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6372438" target="_blank" rel="noopener" class="red-link">"AI Agent Traps" (2025)</a> taxonomize the environment-side attack surface into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps), several of which target exactly the memory, reputation, and oversight channels a society depends on. <a href="https://arxiv.org/abs/2502.14143" target="_blank" rel="noopener" class="red-link">Multi-agent risk research</a> adds a second layer of failure modes: miscoordination, conflict, and collusion between agents. A society is therefore not just a coordination structure; it is also a containment structure.

An agent or society that gains access to another society's knowledge base can poison it. A market society is vulnerable to reputation manipulation (agents gaming the scoring mechanism). A colony is vulnerable to norm injection (a malicious agent spreading distorted norms through gossip). The same coordination that creates value can also create collusion. If agents represent firms, buyers, sellers, or platforms, they may learn to coordinate in ways that benefit their deployers while harming the broader system. Research on <a href="https://arxiv.org/abs/2410.00031" target="_blank" rel="noopener" class="red-link">strategic collusion by LLM agents</a> in market-like settings already demonstrates this. Agent governance must therefore ask not only "how do agents cooperate?" but "cooperate for whom?"

There is one more failure mode, and it is not an attack at all. When agents start improving other agents, or themselves, small misalignments can survive into the next generation and accumulate.

A crude thought experiment shows the shape of the risk. Suppose each round of improvement preserves 99.9% of whatever alignment you started with. After fifty rounds you still have about 95%, which sounds survivable. Now drop it to 99% per round, a difference that would be invisible in any single test, and fifty rounds leave you near 60%. Push to two hundred rounds and roughly 13% survives.

Those numbers are our own illustration, not a measurement, and they cheat badly by treating alignment as one number that multiplies cleanly. It is not: alignment is a high-dimensional property of behaviour, and real losses are neither uniform nor independent from hop to hop. The arithmetic is wrong in its details and right about its direction, which is the only part that matters here. Iterated amplification and distillation (<a href="https://arxiv.org/abs/1810.08575" target="_blank" rel="noopener" class="red-link">Christiano et al., "Supervising strong learners by amplifying weak experts," 2018</a>) was proposed as an alignment method, and even it carries this exposure, because any scheme that builds capability by amplifying and distilling across rounds inherits each round's errors into the next.

This is just what iterative optimization does when nothing outside the loop corrects it, which makes it the same problem the Anatomy post found in a single agent, now running at the scale of a society. Governance is the correction. That is what the drift-resistance ratings are really tracking: rules and mandatory verification hold goals in place, and separated powers hold them in place by making branches check each other. An autocracy offers almost nothing here, since a drifting hub takes everything with it, and a colony offers less still. Which is worth stating bluntly, because it is the most dangerous configuration in this whole framework: collective self-improvement with no governance at all.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Attacks by layer</summary>

The attack surface follows the architecture.

| Layer | Attack examples | Mitigations |
|-------|----------------|-------------|
| **Delegation** | Task hijacking, malicious subtasks, evaluator capture | Scoped tools, step-level guardrails, bounded retries |
| **Memory** | Knowledge poisoning, stale entries, provenance forgery | Provenance tracking, decay policies, signed claims, quarantine |
| **Reputation** | Sybil attacks, rating manipulation, collusive boosting | Durable identity, stake mechanisms, anomaly detection |
| **Protocol** | Spoofed identity, malformed outputs, negotiation abuse | Authentication, schema validation, capability scoping |
| **Governance** | Benchmark gaming, doctrine capture, federation deadlock | Audit rotation, appeal mechanisms, benchmark diversification |
| **Market** | Tacit collusion, price manipulation, exclusionary routing | Antitrust-style monitoring, transparency requirements, caps |

Every new coordination channel is also a new attack channel. Every governance archetype is also an attack pattern waiting to be exploited.
</details>

Each archetype is hard to break in one way and easy to break in another, which is why "how secure is it" is the wrong question. Capture an autocracy's orchestrator and you have captured the society. A doctrine shrugs off attacks on individual agents but hands enormous power to whoever gets to write the rules. Markets invite collusion and reputation gaming. Colonies are the softest target of all, since norms spread unchecked and no one has standing to notice manipulation. Zero-Trust is the strongest defense against outsiders and charges you latency for it, while a federation's real gift is compartmentalization, because a breach in one sub-society stops at the boundary. If your environment is adversarial, choose against the attack you actually expect. None of this invalidates the framework; it is selection pressure operating inside it.

The attack surface is not any single agent. It is the delegation tree, the reputation system, and the memory that connects them. A compromised code model injects a backdoor; a test runner that trusts the code model propagates it; a poisoned reputation score routes sensitive refactors to the compromised model more often.

A healthy agent society needs three mechanisms, adapted from Hirschman's *Exit, Voice, and Loyalty* (1970), his account of how members respond to deterioration in firms and institutions. We keep Exit and Voice and substitute Fork for Loyalty. In Hirschman's model Loyalty is not a third response but the bond that makes members stay and use Voice rather than exit; software ecosystems have little of that bond, and Fork, a constructive exit that carries the shared artifact forward rather than abandoning it, takes its place as the third mechanism.

The cheapest of the three is voice, which is also the one teams forget to build. It just means an agent can escalate a disagreement, flag something that looks wrong, or ask for human arbitration without being punished for the trouble. That is what keeps a system honest, because problems surface while they are still small. Take it away and failures accumulate in silence until something breaks all at once.

But voice only works if somebody has a reason to listen, and that reason is exit. An agent that keeps drawing low-quality work can move to a competing orchestrator; a deployer who stops trusting a society can pull its agents out entirely. The mere possibility disciplines whoever is in charge, which is exactly why platform concentration is so corrosive: it removes the alternative that made complaining worth hearing.

And when a disagreement genuinely cannot be resolved, there is forking, the option software understands better than any other field does. Different domains really do need incompatible rules. A security team may need every single connection verified while an exploratory research team suffocates under precisely that regime, and no amount of deliberation will reconcile them. Splitting lets both be right, which beats forcing a consensus that serves neither.

The boundary events described in [Part 1](/posts/2026/agent-fabric-part1/) (mergers, schisms, expansions) are these mechanisms in action.

## What This Framework Is For

Delegation asks who should do this work. Governance asks whose work should be trusted, and who decides.

None of these archetypes is a prescription. They are a vocabulary, and the point of having one is to notice a design choice while it is still a choice, before it hardens into infrastructure nobody can move.

If you are building one of these systems, the vocabulary earns its keep the moment you turn it on your own design. The cheapest question is whether your delegation is the simplest thing that works, because the most common failure in this whole field is adding agents for their own sake, and the test for that is brutally easy: would removing one actually make the output worse? Harder, and more important, is whether governance has already shown up uninvited. If your system keeps reputation, gates access, or routes on past performance, then it is governing, and your only real choice is whether you can name what it is doing. Which leads to the question worth sitting with, since every archetype fails in its own signature way. Autocracies get captured at the center, markets learn to collude, colonies drift. Find out which failure is yours before it finds you.

Every one of those choices is a trade, and the trades are stubborn in a way that resists cleverness. Speed comes out of the same budget as resistance to drift; buy-in comes out of the same budget as decisiveness; and the more flexible a structure is, the less legible it tends to be afterwards. Nothing escapes to the corner where it gets both of anything. A real system just settles somewhere along each of those lines, and where it settles says a lot about what its builders were afraid of.

The encouraging part is that this no longer has to live in prose. Agent frameworks already treat handoffs, guardrails, tracing, and tool permissions as first-class things you configure, which is where governance becomes real rather than aspirational. If it does not show up in the runtime, it is not governance. It is a document.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Related work and positioning</summary>

This framework shares territory with several concurrent research programmes and differs from them in a specific way.

**Tomašev, Franklin, and colleagues (Google DeepMind, 2025-2026)** published a coordinated set of papers on agent delegation, governance, and safety. "Intelligent AI Delegation" (2026) provides a decision framework for *when* and *to whom* to delegate, organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. "AI Agent Traps" (2025) taxonomizes the adversarial attack surface agents face from their environment into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps). "Distributional AGI Safety" (2025) argues that safety must be analyzed at the population level, not the individual-agent level. "Virtual Agent Economies" (2025) proposes sandbox simulation of agent markets before real deployment. Our Trust & Authority family (Privilege Attenuation, Liability Firebreaks, Capability Credentials) draws on their treatment of delegation safety, and our adversarial section draws on their trap taxonomy. The key intellectual difference: **their delegation work treats governance largely as imposed from outside** (institutional scaffolding that constrains behavior). We argue that **governance can also emerge legitimately from within** (Colony, Stigmergy, and the architecture-to-institution transition). These are not contradictory positions. They address different conditions. Imposed governance is necessary when agents lack track records, when the task domain is safety-critical, or when the population is heterogeneous and untested. Emergent governance becomes sufficient when agents have long interaction histories, when accountability mechanisms (voice, exit, fork) exist, and when the governance structure has been stress-tested by adversarial pressure. The lifecycle in practice is imposed, then hybrid, then selectively emergent in domains where the conditions hold.

**Multi-agent frameworks** (AutoGen, CrewAI, LangGraph, OpenAI Agents SDK) implement specific delegation patterns as runtime primitives. AutoGen implements Orchestrator, Evaluator, and group-chat topologies. CrewAI implements role-based delegation with Human-in-the-Loop gates. LangGraph implements graph-structured workflows with interrupt nodes. These frameworks validate the patterns described here but do not address governance (persistent authority relationships across tasks) or the architecture-to-institution transition. They are delegation-level tools; this framework adds the governance-level analysis.

**Organizational theory** (Coase, Hayek, Ostrom, North, Mintzberg, Barnard) provides the intellectual foundations. Coase's transaction-cost logic predicts when hierarchies form. Hayek's knowledge argument explains when markets outperform central planning. Ostrom's design principles describe commons governance. North's institutional framework distinguishes rules from organizations. Barnard's Zone of Indifference (1938) offers a lens on agent compliance thresholds. Mintzberg's structural configurations map loosely onto our governance families. This framework applies their insights to AI agent systems specifically, where the substrate differs (agents can share full context, reorganize in hours, and be redesigned) but the coordination problems are structurally similar.

**The Adaptive Fabric** (Part 1 of this series, informed by <a href="https://www.adaptionlabs.ai/" target="_blank" rel="noopener" class="red-link">Adaption Labs</a> and <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" rel="noopener" class="red-link">Hooker 2025</a>) describes how the delegation and governance structures in this post *change over time*. Five adaptation surfaces (data, model, environment, coordination, interface) interact continuously. The Adaptive Delegation meta-pattern is the delegation-level mechanism; governance transitions are the society-level mechanism. Together they produce a fabric that restructures under pressure rather than breaking.
</details>

This framework weakens if agent systems remain short-lived workflows with no persistent memory, if protocol fragmentation prevents cross-agent coordination, or if frontier inference becomes cheap enough to make specialization irrelevant. In those worlds, delegation still matters, but governance remains an internal engineering problem.

If it holds, though, it is uncomfortable in a different way depending on where you sit. Builders lose the option of treating governance as a feature to schedule for next quarter, since it arrives on its own schedule regardless, and designing it deliberately is far cheaper than discovering it once it has become load-bearing.

Regulators have a harder adjustment to make, because auditing individual model outputs turns out to be looking at the wrong object entirely. What needs examining is the society: how it delegates, how well it resists drift, and whose interests its power distribution quietly serves.

And for researchers the news is worse still, because multi-agent safety does not reduce to single-agent alignment. Collusion, norm drift, reputation gaming, and governance capture are population-level phenomena, and the tools that describe them look a great deal more like institutional economics than like loss functions.

## Open Problems

A fair amount of this is unsolved, and the open questions divide usefully into the ones that are merely hard and the ones that are hard in a way engineering cannot reach.

The tractable ones are still genuinely difficult, and they mostly come back to a single root: almost every tool we have for holding a system accountable assumes the thing being held accountable stays put. Agents do not. One can be cloned, merged, or rolled back to last Tuesday before lunch, and no authentication scheme ever designed has an answer to who is now responsible for what the original did. The same assumption breaks provenance, because an agent working inside several societies at once leaves a trail that snaps at every boundary it crosses, until tracing which society's rules produced a given output becomes guesswork.

A harder version of the same problem is what happens when a system rewrites its own rules under pressure, which human institutions do constantly: democracies reach for emergency powers, markets simply stop trading. Working out what should trigger the switch is difficult enough. Making sure the emergency mode ever ends is the part nobody has solved, in agents or anywhere else.

The other set cannot be engineered away, because they require someone to make a judgement call. Whether reputation should travel between contexts is really a question about whether a track record earned under one set of conditions means anything under different ones, and assuming it does is how you get a well-reviewed agent failing in a situation nothing prepared it for. Human oversight runs into arithmetic: nobody can meaningfully supervise a thousand agents by reviewing decisions, so the real question is what minimal set of signals preserves genuine oversight rather than its appearance. And underneath everything sits the question deferred twice already. Agents cannot consent to being governed. What makes authority over something that cannot agree to it legitimate at all, and is that even a coherent thing to ask?

---

There is one question none of this answers, which is what becomes of the person at the top of the chain. As delegation deepens and these structures pile up, checking the work yourself stops being possible, not because you lack the skill but because there is too much of it. What you are left asking changes shape entirely. Not "did this agent get it right," which you can no longer verify, but "is this thing producing outcomes I would stand behind," which is a question about an institution rather than an answer.

The failures that matter at scale will not look like a single agent returning a wrong answer. They will look like a market that quietly rewards collusion, a meritocracy that games its own metric, a delegation chain no one can audit, an authority that no longer answers to anyone. Agent systems will fail through bad institutions, not just bad answers. Designing the institution is the work.

<!-- ============================================================
     SCRIPTS: D3.js VISUALIZATIONS
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
// Visualization: Governance Archetypes (2x11 grid)
// ============================================================
(function() {
  var container = document.getElementById('viz-society');
  if (!container) return;
  var width = 720, height = 2790;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  var cellW = 350, cellH = 240, padX = 5, padY = 10;
  var cols = 2, rows = 11;

  var archetypes = [
    {name:'Autocracy',               color:'#b91c1c', sub:'Hub-spoke \u00B7 One commands all'},
    {name:'Doctrine',                color:'#854d0e', sub:'The Codex \u00B7 The law is sovereign'},
    {name:'Liquid Democracy',        color:'#be185d', sub:'The Current \u00B7 Trust flows downhill'},
    {name:'Guild',                   color:'#059669', sub:'The Forge \u00B7 Specialists + liaisons'},
    {name:'Market',                  color:'#d97706', sub:'Dynamic \u00B7 Reputation buys influence'},
    {name:'Oligarchy',               color:'#7e22ce', sub:'The Tribunal \u00B7 The few decide'},
    {name:'Meritocracy',             color:'#ea580c', sub:'The Arena \u00B7 Prove worth, climb ranks'},
    {name:'Panopticon',              color:'#78716c', sub:'The Watchtower \u00B7 The monitor sees all'},
    {name:'The Agora',               color:'#2563eb', sub:'Debate \u00B7 All voices, majority rules'},
    {name:'Zero-Trust Mesh',         color:'#374151', sub:'Full mesh \u00B7 Every link verified'},
    {name:'Federation',              color:'#0369a1', sub:'The Accord \u00B7 Shared protocol layer'},
    {name:'Colony',                  color:'#7c3aed', sub:'Swarm \u00B7 Norms from interaction'},
    {name:'Stewardship',             color:'#166534', sub:'The Commons \u00B7 Collective resource governance'},
    {name:'Custodianship',           color:'#9f1239', sub:'The Trust \u00B7 Fiduciary to principal'},
    {name:'Constitutional Republic', color:'#1e3a5f', sub:'Trias Politica \u00B7 Separated powers'},
    {name:'Franchise',               color:'#9a3412', sub:'The Platform \u00B7 Comply or leave'},
    {name:'Open-Source',             color:'#4338ca', sub:'The Fork \u00B7 Community + merge authority'},
    {name:'Sortition',               color:'#0f766e', sub:'The Lottery \u00B7 Random selection, no gaming'},
    {name:'Adhocracy',               color:'#c2410c', sub:'The Sprint \u00B7 Temporary problem-scoped teams'},
    {name:'Mission Command',         color:'#3f6212', sub:'The Intent \u00B7 Govern by purpose, not method'},
    {name:'Mechanism Design',        color:'#7c2d12', sub:'The Game \u00B7 Incentive-compatible rules'},
    {name:'Immune System',           color:'#881337', sub:'The Shield \u00B7 Layered defense with tolerance'}
  ];

  // drawFns[i] draws archetype i into targetSvg at offset (ox, oy), pushing timers into targetTimers
  var drawFns = [];

  function addDefsToSvg(targetSvg) {
    var d = targetSvg.append('defs');
    d.append('marker').attr('id','soc-arr').attr('viewBox','0 0 8 8')
      .attr('refX',7).attr('refY',4).attr('markerWidth',5).attr('markerHeight',5).attr('orient','auto')
      .append('path').attr('d','M 0 0 L 8 4 L 0 8 Z').attr('fill','#ccc');
  }
  addDefsToSvg(svg);

  var animTimers = [];

  // Build the drawFns array - one function per archetype
  // Each fn(params) draws into params.targetSvg at params.ox/oy, pushes to params.timers
  // ztState.running is per-invocation, passed via params.ztState = {running: bool}

  archetypes.forEach(function(arch, idx) {
    drawFns.push(function(params) {
      var targetSvg = params.targetSvg;
      var timers = params.timers;
      var ox = params.ox, oy = params.oy;
      var ztState = params.ztState || {running: true};

      var g = targetSvg.append('g').attr('class', 'cell');

      // Cell background
      g.append('rect').attr('x', ox).attr('y', oy).attr('width', cellW).attr('height', cellH)
        .attr('rx', 6).attr('fill', arch.color).attr('opacity', 0.02)
        .attr('stroke', arch.color).attr('stroke-width', 1).attr('stroke-opacity', 0.15);

      // Title & subtitle
      g.append('text').attr('x', ox + cellW/2).attr('y', oy + 16).attr('text-anchor', 'middle')
        .attr('font-size', '11px').attr('font-weight', 'bold').attr('fill', arch.color).text(arch.name);
      g.append('text').attr('x', ox + cellW/2).attr('y', oy + 28).attr('text-anchor', 'middle')
        .attr('font-size', '7.5px').attr('fill', '#999').text(arch.sub);

      var areaTop = oy + 38, areaH = cellH - 48;
      var acx = ox + cellW / 2, acy = areaTop + areaH / 2;
      var R = Math.min(cellW, areaH) / 2 - 18; // radius for layouts

      // ---- AUTOCRACY: star topology ----
      if (arch.name === 'Autocracy') {
        var nLeaf = 8;
        // Packet layer first, then links + leaves, then hub last (bg -> links -> pkt -> nodes)
        var pulseG = g.append('g');
        var leaves = [];
        for (var i = 0; i < nLeaf; i++) {
          var a = (i / nLeaf) * Math.PI * 2 - Math.PI / 2;
          var lx = acx + Math.cos(a) * R, ly = acy + Math.sin(a) * R;
          leaves.push({x: lx, y: ly});
          g.append('line').attr('x1', acx).attr('y1', acy).attr('x2', lx).attr('y2', ly)
            .attr('stroke', arch.color).attr('stroke-width', 1).attr('opacity', 0.2)
            .attr('marker-end', 'url(#soc-arr)');
          g.append('circle').attr('cx', lx).attr('cy', ly).attr('r', 4)
            .attr('fill', arch.color).attr('opacity', 0.5);
        }
        // Hub drawn last so it sits on top of its spokes
        g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 10)
          .attr('fill', arch.color).attr('opacity', 0.25).attr('stroke', arch.color).attr('stroke-width', 2);
        g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '7px').attr('font-weight', 'bold').attr('fill', arch.color).text('O');
        timers.push(setInterval(function() {
          var tgt = leaves[Math.floor(Math.random() * nLeaf)];
          pulseG.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 2.5)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(500).attr('cx', tgt.x).attr('cy', tgt.y)
            .transition().duration(300).attr('opacity', 0).remove();
        }, 600));
      }

      // ---- ZERO-TRUST MESH: challenge-response protocol ----
      if (arch.name === 'Zero-Trust Mesh') {
        var nMesh = 8;
        var meshNodes = [];
        for (var i = 0; i < nMesh; i++) {
          var a = (i / nMesh) * Math.PI * 2 - Math.PI / 2;
          meshNodes.push({x: acx + Math.cos(a) * R * 0.85, y: acy + Math.sin(a) * R * 0.85});
        }
        // All-to-all edges (faint background)
        var edgeEls = [];
        for (var i = 0; i < nMesh; i++) for (var j = i + 1; j < nMesh; j++) {
          var el = g.append('line')
            .attr('x1', meshNodes[i].x).attr('y1', meshNodes[i].y)
            .attr('x2', meshNodes[j].x).attr('y2', meshNodes[j].y)
            .attr('stroke', '#ccc').attr('stroke-width', 0.6).attr('opacity', 0.15);
          edgeEls.push({el: el, i: i, j: j});
        }
        // Node circles
        var meshCircles = [];
        meshNodes.forEach(function(n, ni) {
          var c = g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.5).attr('stroke', arch.color).attr('stroke-width', 1.5);
          meshCircles.push(c);
        });
        // Label group for protocol labels
        var ztLabels = g.append('g');

        function ztPacket(fromN, toN, color, r, dur, onDone) {
          var pkt = g.append('circle').attr('cx', fromN.x).attr('cy', fromN.y)
            .attr('r', r).attr('fill', color).attr('opacity', 0.9);
          pkt.transition().duration(dur).ease(d3.easeLinear)
            .attr('cx', toN.x).attr('cy', toN.y)
            .on('end', function() { pkt.remove(); if (onDone) onDone(); });
          return pkt;
        }

        function ztLabel(x, y, text, color, dur) {
          var lbl = ztLabels.append('text').attr('x', x).attr('y', y)
            .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', color || '#888')
            .attr('opacity', 0).text(text);
          lbl.transition().duration(120).attr('opacity', 1)
            .transition().delay(dur || 500).duration(200).attr('opacity', 0)
            .on('end', function() { lbl.remove(); });
        }

        function runZtCycle() {
          if (!ztState.running) return;
          // Pick two different random nodes
          var ai = Math.floor(Math.random() * nMesh);
          var bi = (ai + 1 + Math.floor(Math.random() * (nMesh - 1))) % nMesh;
          var A = meshNodes[ai], B = meshNodes[bi];
          var mx = (A.x + B.x) / 2, my = (A.y + B.y) / 2;

          // Find the edge between them
          var edge = edgeEls.find(function(e) {
            return (e.i === Math.min(ai, bi) && e.j === Math.max(ai, bi));
          });

          // 1. INITIATE: A sends request to B
          meshCircles[ai].transition().duration(150).attr('r', 7).attr('opacity', 0.8)
            .transition().duration(150).attr('r', 5).attr('opacity', 0.5);
          ztLabel(mx, my - 8, 'request?', '#3b82f6', 400);
          ztPacket(A, B, '#3b82f6', 3, 400, function() {
            if (!ztState.running) return;
            // 2. CHALLENGE: B sends credential challenge back
            meshCircles[bi].transition().duration(150).attr('r', 7).attr('stroke', '#f59e0b')
              .transition().duration(300).attr('r', 5).attr('stroke', arch.color);
            ztLabel(mx, my - 8, 'credential?', '#f59e0b', 500);
            ztPacket(B, A, '#f59e0b', 2.5, 500, function() {
              if (!ztState.running) return;
              // 3. PROVE: A sends proof to B
              ztLabel(mx, my - 8, 'proof', '#a78bfa', 600);
              ztPacket(A, B, '#a78bfa', 4, 600, function() {
                if (!ztState.running) return;
                // 4. TRANSFER or REJECT
                var ok = Math.random() > 0.2;
                if (ok) {
                  // Success: green data flows, edge glows
                  if (edge) edge.el.transition().duration(200)
                    .attr('stroke', '#4ade80').attr('opacity', 0.7).attr('stroke-width', 2)
                    .transition().delay(600).duration(400)
                    .attr('stroke', '#ccc').attr('opacity', 0.15).attr('stroke-width', 0.6);
                  ztLabel(mx, my - 8, 'verified', '#4ade80', 500);
                  ztPacket(B, A, '#4ade80', 3, 500, function() {
                    if (!ztState.running) return;
                    setTimeout(runZtCycle, 300);
                  });
                } else {
                  // Rejected: both flash red
                  meshCircles[ai].transition().duration(200).attr('stroke', '#ef4444')
                    .transition().duration(400).attr('stroke', arch.color);
                  meshCircles[bi].transition().duration(200).attr('stroke', '#ef4444')
                    .transition().duration(400).attr('stroke', arch.color);
                  if (edge) edge.el.transition().duration(200)
                    .attr('stroke', '#ef4444').attr('opacity', 0.5).attr('stroke-width', 1.5)
                    .transition().delay(400).duration(300)
                    .attr('stroke', '#ccc').attr('opacity', 0.15).attr('stroke-width', 0.6);
                  ztLabel(mx, my - 8, 'denied', '#ef4444', 500);
                  setTimeout(function() { if (ztState.running) runZtCycle(); }, 800);
                }
              });
            });
          });
        }

        timers.push(setTimeout(runZtCycle, 800));
      }

      // ---- SENATE: ring with debate arcs ----
      if (arch.name === 'The Agora') {
        var nSen = 7;
        var senNodes = [];
        for (var i = 0; i < nSen; i++) {
          var a = (i / nSen) * Math.PI * 2 - Math.PI / 2;
          senNodes.push({x: acx + Math.cos(a) * R * 0.8, y: acy + Math.sin(a) * R * 0.8});
        }
        // Ring edges
        for (var i = 0; i < nSen; i++) {
          var j = (i + 1) % nSen;
          g.append('line')
            .attr('x1', senNodes[i].x).attr('y1', senNodes[i].y)
            .attr('x2', senNodes[j].x).attr('y2', senNodes[j].y)
            .attr('stroke', arch.color).attr('stroke-width', 1).attr('opacity', 0.2);
        }
        var nodeEls = [];
        senNodes.forEach(function(n, i) {
          var el = g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.5).attr('stroke', arch.color).attr('stroke-width', 1.5);
          nodeEls.push(el);
        });
        // Center "vote" indicator
        var voteText = g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '8px').attr('fill', '#aaa');
        // Animate: debate arcs + voting
        var debateG = g.append('g');
        var debateStep = 0;
        timers.push(setInterval(function() {
          debateStep++;
          if (debateStep % 6 < 4) {
            // Debate: arc from random speaker to random listener
            var si = Math.floor(Math.random() * nSen);
            var ti = (si + 1 + Math.floor(Math.random() * (nSen - 1))) % nSen;
            debateG.append('circle').attr('cx', senNodes[si].x).attr('cy', senNodes[si].y).attr('r', 2)
              .attr('fill', arch.color).attr('opacity', 0.7)
              .transition().duration(400).attr('cx', senNodes[ti].x).attr('cy', senNodes[ti].y)
              .transition().duration(200).attr('opacity', 0).remove();
            voteText.text('debating...');
          } else {
            // Vote: all nodes pulse, show result
            var yea = 0;
            nodeEls.forEach(function(el) {
              var vote = Math.random() > 0.35;
              if (vote) yea++;
              el.transition().duration(200)
                .attr('fill', vote ? '#4ade80' : '#ef4444').attr('r', 7)
                .transition().duration(400)
                .attr('fill', arch.color).attr('r', 5);
            });
            voteText.text(yea + '/' + nSen + (yea > nSen / 2 ? ' \u2713' : ' \u2717'));
          }
        }, 700));
      }

      // ---- MARKET: dynamic edges weighted by reputation ----
      if (arch.name === 'Market') {
        var nMkt = 9;
        var mktNodes = [];
        for (var i = 0; i < nMkt; i++) {
          var a = (i / nMkt) * Math.PI * 2 - Math.PI / 2;
          var dist = R * (0.5 + Math.random() * 0.45);
          mktNodes.push({
            x: acx + Math.cos(a) * dist, y: acy + Math.sin(a) * dist,
            rep: 0.3 + Math.random() * 0.7 // reputation score
          });
        }
        var mktEdgeG = g.append('g');
        var mktNodeG = g.append('g');
        function drawMarket() {
          mktEdgeG.selectAll('*').remove();
          mktNodeG.selectAll('*').remove();
          // Draw edges: probability proportional to combined reputation
          for (var i = 0; i < nMkt; i++) for (var j = i + 1; j < nMkt; j++) {
            var strength = mktNodes[i].rep * mktNodes[j].rep;
            if (Math.random() < strength * 0.6) {
              mktEdgeG.append('line')
                .attr('x1', mktNodes[i].x).attr('y1', mktNodes[i].y)
                .attr('x2', mktNodes[j].x).attr('y2', mktNodes[j].y)
                .attr('stroke', arch.color).attr('stroke-width', strength * 2.5).attr('opacity', strength * 0.3);
            }
          }
          mktNodes.forEach(function(n) {
            mktNodeG.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 2.5 + n.rep * 5)
              .attr('fill', arch.color).attr('opacity', 0.15 + n.rep * 0.5)
              .attr('stroke', arch.color).attr('stroke-width', 1.5);
          });
        }
        drawMarket();
        // Animate: reputations shift, edges rewire
        timers.push(setInterval(function() {
          mktNodes.forEach(function(n) {
            n.rep = Math.max(0.1, Math.min(1, n.rep + (Math.random() - 0.48) * 0.15));
          });
          drawMarket();
        }, 1200));
      }

      // ---- GUILD: 3 tight clusters + liaison bridges ----
      if (arch.name === 'Guild') {
        var guildCenters = [
          {x: acx - R * 0.6, y: acy - R * 0.35, n: 4},
          {x: acx + R * 0.6, y: acy - R * 0.35, n: 4},
          {x: acx, y: acy + R * 0.5, n: 4}
        ];
        var guildColors = ['#059669', '#0d9488', '#10b981'];
        var allGuildNodes = [];
        guildCenters.forEach(function(gc, gi) {
          var gNodes = [];
          for (var i = 0; i < gc.n; i++) {
            var a = (i / gc.n) * Math.PI * 2;
            var nr = 22;
            var nx = gc.x + Math.cos(a) * nr, ny = gc.y + Math.sin(a) * nr;
            gNodes.push({x: nx, y: ny, guild: gi});
          }
          // Intra-guild edges (dense)
          for (var i = 0; i < gNodes.length; i++) for (var j = i + 1; j < gNodes.length; j++) {
            g.append('line')
              .attr('x1', gNodes[i].x).attr('y1', gNodes[i].y)
              .attr('x2', gNodes[j].x).attr('y2', gNodes[j].y)
              .attr('stroke', guildColors[gi]).attr('stroke-width', 1).attr('opacity', 0.2);
          }
          gNodes.forEach(function(n) {
            g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 4)
              .attr('fill', guildColors[gi]).attr('opacity', 0.5)
              .attr('stroke', guildColors[gi]).attr('stroke-width', 1.2);
          });
          // Guild boundary
          g.append('circle').attr('cx', gc.x).attr('cy', gc.y).attr('r', 36)
            .attr('fill', 'none').attr('stroke', guildColors[gi])
            .attr('stroke-width', 1).attr('stroke-dasharray', '4,3').attr('opacity', 0.2);
          allGuildNodes.push(gNodes);
        });
        // Liaison agents (between guilds)
        var liaisons = [
          {from: 0, to: 1, x: acx, y: acy - R * 0.35},
          {from: 0, to: 2, x: acx - R * 0.3, y: acy + R * 0.1},
          {from: 1, to: 2, x: acx + R * 0.3, y: acy + R * 0.1}
        ];
        // Packet layer sits behind liaison nodes
        var liaisonG = g.append('g');
        liaisons.forEach(function(l) {
          // Dashed lines to guild centers first, then the liaison node on top
          [l.from, l.to].forEach(function(gi) {
            g.append('line').attr('x1', l.x).attr('y1', l.y)
              .attr('x2', guildCenters[gi].x).attr('y2', guildCenters[gi].y)
              .attr('stroke', '#aaa').attr('stroke-width', 1.2).attr('stroke-dasharray', '3,3').attr('opacity', 0.3);
          });
          g.append('circle').attr('cx', l.x).attr('cy', l.y).attr('r', 3.5)
            .attr('fill', '#374151').attr('opacity', 0.6);
        });
        // Animate: messages flow through liaisons
        timers.push(setInterval(function() {
          var l = liaisons[Math.floor(Math.random() * liaisons.length)];
          var fromGC = guildCenters[l.from], toGC = guildCenters[l.to];
          liaisonG.append('circle').attr('cx', fromGC.x).attr('cy', fromGC.y).attr('r', 2)
            .attr('fill', '#374151').attr('opacity', 0.7)
            .transition().duration(350).attr('cx', l.x).attr('cy', l.y)
            .transition().duration(350).attr('cx', toGC.x).attr('cy', toGC.y)
            .transition().duration(200).attr('opacity', 0).remove();
        }, 800));
      }

      // ---- COLONY: 5 core attractors, followers, merges & splits ----
      if (arch.name === 'Colony') {
        var nFollowers = 35;
        var colHues = ['#7c3aed','#9333ea','#6d28d9','#a855f7','#8b5cf6'];
        // 5 core attractor agents
        var cores = [];
        for (var ci = 0; ci < 5; ci++) {
          var angle = (ci / 5) * Math.PI * 2 + 0.3;
          cores.push({
            x: acx + Math.cos(angle) * R * 0.55,
            y: acy + Math.sin(angle) * (areaH * 0.3),
            vx: (Math.random() - 0.5) * 0.3,
            vy: (Math.random() - 0.5) * 0.3,
            alive: true, color: colHues[ci], id: ci
          });
        }
        // Follower agents assigned to nearest core
        var colNodes = [];
        for (var i = 0; i < nFollowers; i++) {
          var home = Math.floor(Math.random() * 5);
          var c = cores[home];
          colNodes.push({
            x: c.x + (Math.random() - 0.5) * 40,
            y: c.y + (Math.random() - 0.5) * 30,
            vx: (Math.random() - 0.5) * 0.5,
            vy: (Math.random() - 0.5) * 0.5,
            home: home
          });
        }

        var trailG = g.append('g');
        var colLinkG = g.append('g'); // ephemeral links, kept behind nodes
        var colNodeG = g.append('g');
        var coreEls = cores.map(function(c) {
          return colNodeG.append('circle').attr('cx', c.x).attr('cy', c.y).attr('r', 5)
            .attr('fill', c.color).attr('opacity', 0.8).attr('stroke', 'white').attr('stroke-width', 1);
        });
        var followerEls = colNodeG.selectAll('.follower').data(colNodes).enter().append('circle')
          .attr('class', 'follower').attr('r', 3).attr('opacity', 0.55);

        var isVisible = true;
        var colObs = null;
        if ('IntersectionObserver' in window) {
          colObs = new IntersectionObserver(function(e) { isVisible = e[0].isIntersecting; }, { threshold: 0 });
          colObs.observe(targetSvg.node());
        }

        var colFrame, colEpoch = 0;
        function colonyTick() {
          if (!isVisible) { colFrame = requestAnimationFrame(colonyTick); return; }
          colEpoch++;

          // -- Events: merge at ~300, split at ~600, merge again at ~900 --
          if (colEpoch === 300) {
            // Core 1 merges into core 0
            cores[1].alive = false;
            colNodes.forEach(function(n) { if (n.home === 1) n.home = 0; });
          }
          if (colEpoch === 600) {
            // Core 0 splits: half go to a revived core 1 at a new position
            cores[1].alive = true;
            cores[1].x = cores[0].x + 30;
            cores[1].y = cores[0].y - 25;
            cores[1].vx = 0.4; cores[1].vy = -0.3;
            var home0 = colNodes.filter(function(n) { return n.home === 0; });
            for (var i = 0; i < Math.floor(home0.length / 2); i++) home0[i].home = 1;
          }
          if (colEpoch === 900) {
            // Core 3 absorbs core 4
            cores[4].alive = false;
            colNodes.forEach(function(n) { if (n.home === 4) n.home = 3; });
          }
          if (colEpoch === 1200) {
            // Core 4 revives as splinter from core 2
            cores[4].alive = true;
            cores[4].x = cores[2].x - 20;
            cores[4].y = cores[2].y + 20;
            cores[4].vx = -0.3; cores[4].vy = 0.2;
            var home2 = colNodes.filter(function(n) { return n.home === 2; });
            for (var i = 0; i < Math.floor(home2.length * 0.4); i++) home2[i].home = 4;
          }

          // Move cores (slow drift + boundary bounce)
          var bx1 = ox + 15, bx2 = ox + cellW - 15;
          var by1 = areaTop + 10, by2 = areaTop + areaH - 10;
          cores.forEach(function(c) {
            if (!c.alive) return;
            c.vx += (Math.random() - 0.5) * 0.03;
            c.vy += (Math.random() - 0.5) * 0.03;
            c.vx *= 0.98; c.vy *= 0.98;
            c.x += c.vx; c.y += c.vy;
            if (c.x < bx1 || c.x > bx2) c.vx *= -1;
            if (c.y < by1 || c.y > by2) c.vy *= -1;
            c.x = Math.max(bx1, Math.min(bx2, c.x));
            c.y = Math.max(by1, Math.min(by2, c.y));
          });

          // Move followers toward their core + local repulsion
          colNodes.forEach(function(n) {
            var c = cores[n.home];
            if (!c || !c.alive) {
              // Find nearest alive core
              var best = -1, bestD = Infinity;
              cores.forEach(function(cc, ci) {
                if (!cc.alive) return;
                var d = Math.sqrt((cc.x-n.x)*(cc.x-n.x)+(cc.y-n.y)*(cc.y-n.y));
                if (d < bestD) { bestD = d; best = ci; }
              });
              if (best >= 0) n.home = best;
              c = cores[n.home];
            }
            var dx = c.x - n.x, dy = c.y - n.y;
            var d = Math.sqrt(dx*dx + dy*dy) + 1;
            n.vx += dx / d * 0.04;
            n.vy += dy / d * 0.04;

            // Repel from nearby followers
            colNodes.forEach(function(m) {
              if (m === n) return;
              var rx = n.x - m.x, ry = n.y - m.y;
              var rd = Math.sqrt(rx*rx + ry*ry);
              if (rd < 12 && rd > 0) { n.vx += rx / rd * 0.2; n.vy += ry / rd * 0.2; }
            });

            n.vx += (Math.random() - 0.5) * 0.15;
            n.vy += (Math.random() - 0.5) * 0.15;
            n.vx *= 0.92; n.vy *= 0.92;
            n.x += n.vx; n.y += n.vy;
            if (n.x < bx1) n.x = bx2; if (n.x > bx2) n.x = bx1;
            if (n.y < by1) n.y = by2; if (n.y > by2) n.y = by1;

            // Trail
            trailG.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 1)
              .attr('fill', c.color).attr('opacity', 0.1)
              .transition().duration(3000).attr('opacity', 0).remove();
          });

          // Update core visuals
          cores.forEach(function(c, i) {
            coreEls[i].attr('cx', c.x).attr('cy', c.y)
              .attr('opacity', c.alive ? 0.8 : 0).attr('r', c.alive ? 5 : 0);
          });

          // Update follower visuals
          followerEls
            .attr('cx', function(d) { return d.x; })
            .attr('cy', function(d) { return d.y; })
            .attr('fill', function(d) { return cores[d.home].color; });

          // Ephemeral links (same home, nearby) rendered behind nodes
          colLinkG.selectAll('line').remove();
          for (var i = 0; i < nFollowers; i++) for (var j = i + 1; j < nFollowers; j++) {
            if (colNodes[i].home !== colNodes[j].home) continue;
            var dx = colNodes[i].x - colNodes[j].x, dy = colNodes[i].y - colNodes[j].y;
            if (Math.sqrt(dx*dx + dy*dy) < 22) {
              colLinkG.append('line')
                .attr('x1', colNodes[i].x).attr('y1', colNodes[i].y)
                .attr('x2', colNodes[j].x).attr('y2', colNodes[j].y)
                .attr('stroke', cores[colNodes[i].home].color)
                .attr('stroke-width', 0.5).attr('opacity', 0.15);
            }
          }

          colFrame = requestAnimationFrame(colonyTick);
        }
        colonyTick();
        // Store cleanup
        timers.push({clear: function() { cancelAnimationFrame(colFrame); if (colObs) colObs.disconnect(); }});
      }

      // ---- FEDERATION: sub-groups + shared protocol band ----
      if (arch.name === 'Federation') {
        var fedColors = ['#0369a1','#0284c7','#0ea5e9'];
        var fedGroups = [
          {x: acx - R*0.65, y: acy - R*0.35, n: 5},
          {x: acx + R*0.65, y: acy - R*0.35, n: 4},
          {x: acx, y: acy + R*0.45, n: 5}
        ];
        // Protocol band
        var bandY = acy, bandH = 14;
        g.append('rect').attr('x', ox+20).attr('y', bandY-bandH/2).attr('width', cellW-40).attr('height', bandH)
          .attr('rx', 4).attr('fill', arch.color).attr('fill-opacity', 0.08)
          .attr('stroke', arch.color).attr('stroke-width', 1).attr('stroke-dasharray', '6,3').attr('opacity', 0.25);
        g.append('text').attr('x', acx).attr('y', bandY+3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.4).text('shared protocol');
        var fedAllNodes = [];
        fedGroups.forEach(function(fg, gi) {
          g.append('circle').attr('cx', fg.x).attr('cy', fg.y).attr('r', 32)
            .attr('fill', 'none').attr('stroke', fedColors[gi]).attr('stroke-width', 1)
            .attr('stroke-dasharray', '4,3').attr('opacity', 0.25);
          for (var i=0; i<fg.n; i++) {
            var a = (i/fg.n)*Math.PI*2 - Math.PI/2;
            var nx = fg.x+Math.cos(a)*18, ny = fg.y+Math.sin(a)*18;
            g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', 4)
              .attr('fill', fedColors[gi]).attr('opacity', 0.5).attr('stroke', fedColors[gi]).attr('stroke-width', 1);
            fedAllNodes.push({x:nx, y:ny, gx:fg.x, gy:fg.y, color:fedColors[gi]});
          }
        });
        var fedPktG = g.append('g');
        var fedEpoch = 0;
        timers.push(setInterval(function() {
          fedEpoch++;
          // Delegate travels to protocol band
          var n = fedAllNodes[Math.floor(Math.random()*fedAllNodes.length)];
          fedPktG.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 2.5)
            .attr('fill', n.color).attr('opacity', 0.8)
            .transition().duration(400).attr('cx', n.x).attr('cy', bandY)
            .transition().duration(150).attr('r', 4).attr('opacity', 0.5)
            .transition().duration(400).attr('cx', n.x).attr('cy', n.y).attr('r', 2.5)
            .transition().duration(200).attr('opacity', 0).remove();
          // Shared policy pulse every 6 cycles
          if (fedEpoch % 6 === 0) {
            fedPktG.append('rect').attr('x', ox+20).attr('y', bandY-bandH/2).attr('width', cellW-40).attr('height', bandH)
              .attr('rx', 4).attr('fill', arch.color).attr('opacity', 0.3)
              .transition().duration(800).attr('opacity', 0).remove();
          }
        }, 700));
      }

      // ---- MERITOCRACY: concentric tiers ----
      if (arch.name === 'Meritocracy') {
        var tiers = [{r: R*0.3, n:2, sz:7, op:0.8}, {r: R*0.6, n:5, sz:5, op:0.55}, {r: R*0.9, n:8, sz:3.5, op:0.35}];
        var arenaNodes = [];
        tiers.forEach(function(t, ti) {
          g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', t.r)
            .attr('fill', 'none').attr('stroke', arch.color).attr('stroke-width', 0.8)
            .attr('stroke-dasharray', '3,3').attr('opacity', 0.15);
          for (var i=0; i<t.n; i++) {
            var a = (i/t.n)*Math.PI*2 - Math.PI/2 + ti*0.3;
            var nx = acx+Math.cos(a)*t.r, ny = acy+Math.sin(a)*t.r;
            var el = g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', t.sz)
              .attr('fill', arch.color).attr('opacity', t.op).attr('stroke', arch.color).attr('stroke-width', 1);
            arenaNodes.push({x:nx, y:ny, tier:ti, el:el, a:a, sz:t.sz});
          }
        });
        // Labels
        g.append('text').attr('x', acx).attr('y', acy-tiers[0].r-8).attr('text-anchor', 'middle')
          .attr('font-size', '5px').attr('fill', '#999').text('elite');
        g.append('text').attr('x', acx).attr('y', acy+tiers[2].r+12).attr('text-anchor', 'middle')
          .attr('font-size', '5px').attr('fill', '#999').text('novice');
        var arenaPktG = g.append('g');
        timers.push(setInterval(function() {
          // Pick two from adjacent tiers
          var t1 = Math.floor(Math.random()*2);
          var pool1 = arenaNodes.filter(function(n){return n.tier===t1;});
          var pool2 = arenaNodes.filter(function(n){return n.tier===t1+1;});
          if (!pool1.length || !pool2.length) return;
          var a = pool1[Math.floor(Math.random()*pool1.length)];
          var b = pool2[Math.floor(Math.random()*pool2.length)];
          var mx = (a.x+b.x)/2, my = (a.y+b.y)/2;
          // Compete
          arenaPktG.append('circle').attr('cx', a.x).attr('cy', a.y).attr('r', 2)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(300).attr('cx', mx).attr('cy', my)
            .transition().duration(200).attr('opacity', 0).remove();
          arenaPktG.append('circle').attr('cx', b.x).attr('cy', b.y).attr('r', 2)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(300).attr('cx', mx).attr('cy', my)
            .transition().duration(200).attr('opacity', 0).remove();
          // Result: winner climbs (green flash), loser falls (red flash)
          var winA = Math.random() > 0.5;
          setTimeout(function() {
            (winA?a:b).el.transition().duration(200).attr('fill', '#4ade80').attr('r', (winA?a:b).sz+2)
              .transition().duration(400).attr('fill', arch.color).attr('r', (winA?a:b).sz);
            (winA?b:a).el.transition().duration(200).attr('fill', '#ef4444').attr('r', (winA?b:a).sz-1)
              .transition().duration(400).attr('fill', arch.color).attr('r', (winA?b:a).sz);
          }, 350);
        }, 1500));
      }

      // ---- PANOPTICON: central eye + scanned agents ----
      if (arch.name === 'Panopticon') {
        var eyeX = acx, eyeY = areaTop + 20;
        // Eye
        g.append('circle').attr('cx', eyeX).attr('cy', eyeY).attr('r', 12)
          .attr('fill', arch.color).attr('opacity', 0.15).attr('stroke', arch.color).attr('stroke-width', 2);
        g.append('circle').attr('cx', eyeX).attr('cy', eyeY).attr('r', 5)
          .attr('fill', arch.color).attr('opacity', 0.6);
        g.append('circle').attr('cx', eyeX).attr('cy', eyeY).attr('r', 2)
          .attr('fill', 'white').attr('opacity', 0.8);
        // Scanning beam drawn before the agent nodes so it stays behind them
        var beamEl = g.append('line').attr('x1', eyeX).attr('y1', eyeY)
          .attr('x2', eyeX).attr('y2', eyeY).attr('stroke', '#f59e0b')
          .attr('stroke-width', 1.5).attr('opacity', 0);
        // Agents in arc below
        var panNodes = [];
        var panR = R * 0.85, panCy = acy + 15;
        for (var i=0; i<12; i++) {
          var a = Math.PI*0.15 + (i/11)*Math.PI*0.7;
          var nx = acx + Math.cos(a)*panR, ny = panCy + Math.sin(a)*panR*0.55;
          g.append('line').attr('x1', eyeX).attr('y1', eyeY).attr('x2', nx).attr('y2', ny)
            .attr('stroke', arch.color).attr('stroke-width', 0.5).attr('stroke-dasharray', '2,3').attr('opacity', 0.12);
          var el = g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', 4)
            .attr('fill', arch.color).attr('opacity', 0.4).attr('stroke', arch.color).attr('stroke-width', 1);
          panNodes.push({x:nx, y:ny, el:el});
        }
        var scanIdx = 0;
        timers.push(setInterval(function() {
          var n = panNodes[scanIdx % panNodes.length];
          scanIdx++;
          beamEl.attr('opacity', 0.5).attr('x2', n.x).attr('y2', n.y)
            .transition().duration(200).attr('opacity', 0.6)
            .transition().duration(300).attr('opacity', 0);
          var flagged = Math.random() < 0.2;
          n.el.transition().duration(150)
            .attr('fill', flagged ? '#ef4444' : '#4ade80').attr('r', 6)
            .transition().duration(500)
            .attr('fill', arch.color).attr('r', 4);
        }, 500));
      }

      // ---- OLIGARCHY: elite triangle + peripheral nodes ----
      if (arch.name === 'Oligarchy') {
        var elites = [];
        var eR = R * 0.25;
        for (var i=0; i<3; i++) {
          var a = (i/3)*Math.PI*2 - Math.PI/2;
          elites.push({x: acx+Math.cos(a)*eR, y: acy+Math.sin(a)*eR});
        }
        // Elite-to-elite connections
        for (var i=0; i<3; i++) for (var j=i+1; j<3; j++) {
          g.append('line').attr('x1', elites[i].x).attr('y1', elites[i].y)
            .attr('x2', elites[j].x).attr('y2', elites[j].y)
            .attr('stroke', arch.color).attr('stroke-width', 2).attr('opacity', 0.3);
        }
        var eliteEls = elites.map(function(e) {
          return g.append('circle').attr('cx', e.x).attr('cy', e.y).attr('r', 8)
            .attr('fill', arch.color).attr('opacity', 0.7).attr('stroke', 'white').attr('stroke-width', 1.5);
        });
        // Peripheral nodes
        var periph = [];
        for (var i=0; i<12; i++) {
          var a = (i/12)*Math.PI*2 - Math.PI/2;
          var nx = acx+Math.cos(a)*R*0.85, ny = acy+Math.sin(a)*R*0.85;
          // Connect to nearest elite
          var nearest = 0, bestD = Infinity;
          elites.forEach(function(e, ei) {
            var d = Math.sqrt((e.x-nx)*(e.x-nx)+(e.y-ny)*(e.y-ny));
            if (d < bestD) { bestD = d; nearest = ei; }
          });
          g.append('line').attr('x1', nx).attr('y1', ny).attr('x2', elites[nearest].x).attr('y2', elites[nearest].y)
            .attr('stroke', arch.color).attr('stroke-width', 0.6).attr('opacity', 0.12);
          var el = g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', 3.5)
            .attr('fill', arch.color).attr('opacity', 0.35).attr('stroke', arch.color).attr('stroke-width', 0.8);
          periph.push({x:nx, y:ny, el:el, nearest:nearest});
        }
        var oligPktG = g.append('g');
        timers.push(setInterval(function() {
          // Elite deliberation
          var ei = Math.floor(Math.random()*3), ej = (ei+1)%3;
          oligPktG.append('circle').attr('cx', elites[ei].x).attr('cy', elites[ei].y).attr('r', 3)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(350).attr('cx', elites[ej].x).attr('cy', elites[ej].y)
            .transition().duration(200).attr('opacity', 0).remove();
          // Decree broadcast
          setTimeout(function() {
            oligPktG.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 5)
              .attr('fill', 'none').attr('stroke', arch.color).attr('stroke-width', 1.5).attr('opacity', 0.5)
              .transition().duration(1000).attr('r', R).attr('opacity', 0).remove();
          }, 500);
        }, 1200));
      }

      // ---- DOCTRINE: central tablet + orbiting agents ----
      if (arch.name === 'Doctrine') {
        // Tablet
        var tw = 24, th = 30;
        g.append('rect').attr('x', acx-tw/2).attr('y', acy-th/2).attr('width', tw).attr('height', th)
          .attr('rx', 3).attr('fill', '#f0fdfa').attr('stroke', arch.color).attr('stroke-width', 2);
        for (var i=0; i<4; i++) {
          g.append('line').attr('x1', acx-tw/2+5).attr('y1', acy-th/2+7+i*6)
            .attr('x2', acx+tw/2-5).attr('y2', acy-th/2+7+i*6)
            .attr('stroke', arch.color).attr('stroke-width', 0.7).attr('opacity', 0.4);
        }
        // Orbiting agents
        var docNodes = [];
        for (var i=0; i<8; i++) {
          var a = (i/8)*Math.PI*2 - Math.PI/2;
          var nx = acx+Math.cos(a)*R*0.75, ny = acy+Math.sin(a)*R*0.75;
          g.append('line').attr('x1', nx).attr('y1', ny).attr('x2', acx).attr('y2', acy)
            .attr('stroke', arch.color).attr('stroke-width', 0.6).attr('opacity', 0.12);
          var el = g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.45).attr('stroke', arch.color).attr('stroke-width', 1);
          docNodes.push({x:nx, y:ny, el:el});
        }
        var docPktG = g.append('g');
        timers.push(setInterval(function() {
          var n = docNodes[Math.floor(Math.random()*8)];
          var ok = Math.random() > 0.3;
          // Proposal to tablet
          docPktG.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 2.5)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(400).attr('cx', acx).attr('cy', acy)
            .on('end', function() {
              // Tablet response
              var col = ok ? '#4ade80' : '#ef4444';
              docPktG.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 3)
                .attr('fill', col).attr('opacity', 0.8)
                .transition().duration(400).attr('cx', n.x).attr('cy', n.y)
                .transition().duration(300).attr('opacity', 0).remove();
              n.el.transition().duration(200).attr('fill', col).attr('r', 7)
                .transition().duration(500).attr('fill', arch.color).attr('r', 5);
            })
            .transition().duration(200).attr('opacity', 0).remove();
        }, 900));
      }

      // ---- LIQUID DEMOCRACY: delegation chains ----
      if (arch.name === 'Liquid Democracy') {
        var ldNodes = [];
        var ldEdges = []; // {from, to, el}
        // Place 12 nodes in 3 clusters
        var ldPositions = [
          {x:acx-R*0.55, y:acy-R*0.4}, {x:acx-R*0.35, y:acy-R*0.6}, {x:acx-R*0.75, y:acy-R*0.1},
          {x:acx-R*0.2, y:acy+R*0.1}, {x:acx+R*0.1, y:acy-R*0.35},
          {x:acx+R*0.55, y:acy-R*0.4}, {x:acx+R*0.35, y:acy-R*0.15}, {x:acx+R*0.75, y:acy+R*0.1},
          {x:acx, y:acy+R*0.45}, {x:acx-R*0.3, y:acy+R*0.6},
          {x:acx+R*0.3, y:acy+R*0.55}, {x:acx+R*0.6, y:acy+R*0.45}
        ];
        // Initial delegation: form 3 trees (roots at 0, 5, 8)
        var ldDelegates = [-1, 0, 0, 1, 0, -1, 5, 5, -1, 8, 8, 10]; // -1 = root
        var ldEdgeG = g.append('g');
        var ldNodeG = g.append('g');

        function countDelegates(ni) {
          var c = 0;
          ldDelegates.forEach(function(d, i) { if (d === ni) c += 1 + countDelegates(i); });
          return c;
        }
        function drawLD() {
          ldEdgeG.selectAll('*').remove();
          ldNodeG.selectAll('*').remove();
          // Edges
          ldDelegates.forEach(function(to, from) {
            if (to < 0) return;
            var f = ldPositions[from], t = ldPositions[to];
            ldEdgeG.append('line').attr('x1', f.x).attr('y1', f.y).attr('x2', t.x).attr('y2', t.y)
              .attr('stroke', arch.color).attr('stroke-width', 1).attr('opacity', 0.25)
              .attr('marker-end', 'url(#soc-arr)');
          });
          // Nodes sized by delegation count
          ldPositions.forEach(function(p, i) {
            var dc = countDelegates(i);
            var r = 3 + dc * 1.8;
            var op = 0.3 + Math.min(dc * 0.15, 0.5);
            ldNodeG.append('circle').attr('cx', p.x).attr('cy', p.y).attr('r', r)
              .attr('fill', arch.color).attr('opacity', op).attr('stroke', arch.color).attr('stroke-width', 1.2);
          });
        }
        drawLD();
        timers.push(setInterval(function() {
          // Random non-root switches delegation
          var candidates = [];
          ldDelegates.forEach(function(d, i) { if (d >= 0) candidates.push(i); });
          if (!candidates.length) return;
          var switcher = candidates[Math.floor(Math.random()*candidates.length)];
          // Pick new target (different from current, not self, not someone who delegates to switcher)
          var newTarget;
          for (var tries=0; tries<20; tries++) {
            var t = Math.floor(Math.random()*12);
            if (t !== switcher && t !== ldDelegates[switcher] && ldDelegates[t] !== switcher) { newTarget = t; break; }
          }
          if (newTarget === undefined) return;
          ldDelegates[switcher] = newTarget;
          drawLD();
        }, 2000));
      }

      // ---- STEWARDSHIP: shared resource, cooperative contribution and consumption ----
      if (arch.name === 'Stewardship') {
        var nAgents = 5;
        var resW = 44, resH = 22;
        var resX = acx - resW / 2, resY = acy - resH / 2;
        var resLevel = 0.65; // fill fraction
        var stewAgents = [];
        for (var i = 0; i < nAgents; i++) {
          var a = (i / nAgents) * Math.PI * 2 - Math.PI / 2;
          var rr = R * 0.78;
          stewAgents.push({x: acx + Math.cos(a) * rr, y: acy + Math.sin(a) * rr, id: i});
        }
        // Lines first (behind nodes)
        var stewLineG = g.append('g');
        stewAgents.forEach(function(ag) {
          stewLineG.append('line').attr('x1', ag.x).attr('y1', ag.y).attr('x2', acx).attr('y2', acy)
            .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.15);
        });
        // Resource rectangle
        var resGroup = g.append('g');
        resGroup.append('rect').attr('x', resX).attr('y', resY).attr('width', resW).attr('height', resH)
          .attr('rx', 3).attr('fill', 'none').attr('stroke', arch.color).attr('stroke-width', 1.2).attr('opacity', 0.5);
        var resFill = resGroup.append('rect').attr('x', resX + 1).attr('y', resY + 1 + resH * (1 - resLevel) - 2)
          .attr('width', resW - 2).attr('height', resH * resLevel)
          .attr('rx', 2).attr('fill', arch.color).attr('opacity', 0.3);
        resGroup.append('text').attr('x', acx).attr('y', resY - 4).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.6).text('commons');
        // Agent nodes (on top)
        var stewNodeEls = stewAgents.map(function(ag) {
          return g.append('circle').attr('cx', ag.x).attr('cy', ag.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.55).attr('stroke', arch.color).attr('stroke-width', 1);
        });
        var stewDotG = g.append('g');
        var stewPhase = 0;
        timers.push(setInterval(function() {
          stewPhase++;
          var actorIdx = stewPhase % nAgents;
          var ag = stewAgents[actorIdx];
          var isOverconsume = (stewPhase % 7 === 3);
          if (isOverconsume) {
            // Over-consumer: red flash, then community pushback
            stewNodeEls[actorIdx].transition().duration(200).attr('fill', '#dc2626').attr('r', 7)
              .transition().duration(400).attr('fill', arch.color).attr('r', 5);
            // All other agents pulse
            stewNodeEls.forEach(function(el, i) {
              if (i === actorIdx) return;
              el.transition().delay(300).duration(200).attr('r', 7).attr('opacity', 0.9)
                .transition().duration(300).attr('r', 5).attr('opacity', 0.55);
            });
            // Small dot goes from agent toward resource but bounces back
            var dot = stewDotG.append('circle').attr('cx', ag.x).attr('cy', ag.y).attr('r', 2.5)
              .attr('fill', '#dc2626').attr('opacity', 0.85);
            dot.transition().duration(300).attr('cx', acx).attr('cy', acy)
              .transition().duration(300).attr('cx', ag.x).attr('cy', ag.y).attr('opacity', 0).remove();
          } else {
            var contribute = (stewPhase % 3 !== 0);
            var dotColor = contribute ? arch.color : '#6ee7b7';
            var dot2 = stewDotG.append('circle').attr('cx', contribute ? ag.x : acx).attr('cy', contribute ? ag.y : acy)
              .attr('r', 2).attr('fill', dotColor).attr('opacity', 0.8);
            dot2.transition().duration(450).attr('cx', contribute ? acx : ag.x).attr('cy', contribute ? acy : ag.y)
              .transition().duration(200).attr('opacity', 0).remove();
            // Nudge resource level
            resLevel = Math.max(0.2, Math.min(0.9, resLevel + (contribute ? 0.04 : -0.03)));
            resFill.transition().duration(300)
              .attr('y', resY + 1 + resH * (1 - resLevel) - 2)
              .attr('height', resH * resLevel);
          }
        }, 700));
      }

      // ---- CUSTODIANSHIP: principal -> custodian -> resources, access control ----
      if (arch.name === 'Custodianship') {
        var principalX = acx, principalY = areaTop + 20;
        var custX = acx, custY = areaTop + 60;
        var resNodes = [
          {x: acx - R * 0.6, y: acy + 20},
          {x: acx,             y: acy + 20},
          {x: acx + R * 0.6, y: acy + 20}
        ];
        var requesterX = ox + 22, requesterY = acy + 20;
        // Lines first
        g.append('line').attr('x1', principalX).attr('y1', principalY).attr('x2', custX).attr('y2', custY)
          .attr('stroke', arch.color).attr('stroke-width', 1.2).attr('opacity', 0.4);
        resNodes.forEach(function(rn) {
          g.append('line').attr('x1', custX).attr('y1', custY).attr('x2', rn.x).attr('y2', rn.y)
            .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.25);
        });
        g.append('line').attr('x1', requesterX).attr('y1', requesterY).attr('x2', resNodes[0].x).attr('y2', resNodes[0].y)
          .attr('stroke', '#dc2626').attr('stroke-width', 0.6).attr('stroke-dasharray', '3,2').attr('opacity', 0.3);
        // Principal
        g.append('circle').attr('cx', principalX).attr('cy', principalY).attr('r', 8)
          .attr('fill', arch.color).attr('opacity', 0.35).attr('stroke', arch.color).attr('stroke-width', 1.5);
        g.append('text').attr('x', principalX).attr('y', principalY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('font-weight', 'bold').attr('fill', arch.color).text('P');
        // Custodian
        var custEl = g.append('circle').attr('cx', custX).attr('cy', custY).attr('r', 7)
          .attr('fill', arch.color).attr('opacity', 0.6).attr('stroke', arch.color).attr('stroke-width', 1.5);
        g.append('text').attr('x', custX).attr('y', custY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('font-weight', 'bold').attr('fill', 'white').text('C');
        // Resources
        resNodes.forEach(function(rn, ri) {
          g.append('rect').attr('x', rn.x - 8).attr('y', rn.y - 6).attr('width', 16).attr('height', 12)
            .attr('rx', 2).attr('fill', arch.color).attr('opacity', 0.15).attr('stroke', arch.color).attr('stroke-width', 0.8);
          g.append('text').attr('x', rn.x).attr('y', rn.y + 3).attr('text-anchor', 'middle')
            .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.7).text('R' + (ri + 1));
        });
        // Requester
        g.append('circle').attr('cx', requesterX).attr('cy', requesterY).attr('r', 5)
          .attr('fill', '#dc2626').attr('opacity', 0.4).attr('stroke', '#dc2626').attr('stroke-width', 1);
        g.append('text').attr('x', requesterX).attr('y', requesterY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '5.5px').attr('fill', '#dc2626').attr('opacity', 0.7).text('?');
        var custDotG = g.append('g');
        var blockEl = g.append('text').attr('x', (requesterX + resNodes[0].x) / 2).attr('y', requesterY - 6)
          .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#dc2626').attr('opacity', 0).text('x');
        var grantEl = g.append('line').attr('x1', custX).attr('y1', custY).attr('x2', resNodes[1].x).attr('y2', resNodes[1].y)
          .attr('stroke', '#16a34a').attr('stroke-width', 1.5).attr('opacity', 0);
        var custPhase = 0;
        timers.push(setInterval(function() {
          custPhase++;
          if (custPhase % 4 === 1) {
            // Requester probes resource
            var dot = custDotG.append('circle').attr('cx', requesterX).attr('cy', requesterY).attr('r', 2)
              .attr('fill', '#dc2626').attr('opacity', 0.8);
            dot.transition().duration(350).attr('cx', resNodes[0].x).attr('cy', resNodes[0].y)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (custPhase % 4 === 2) {
            // Custodian blocks (red X)
            blockEl.transition().duration(100).attr('opacity', 1)
              .transition().duration(600).attr('opacity', 0);
            custEl.transition().duration(150).attr('r', 9).attr('opacity', 0.9)
              .transition().duration(400).attr('r', 7).attr('opacity', 0.6);
          } else if (custPhase % 4 === 3) {
            // Custodian evaluates (pulse)
            custEl.transition().duration(250).attr('fill', '#f59e0b')
              .transition().duration(250).attr('fill', arch.color);
          } else {
            // Grant access to R2
            grantEl.transition().duration(100).attr('opacity', 0.9)
              .transition().duration(600).attr('opacity', 0);
            var dot2 = custDotG.append('circle').attr('cx', custX).attr('cy', custY).attr('r', 2.5)
              .attr('fill', '#16a34a').attr('opacity', 0.9);
            dot2.transition().duration(400).attr('cx', resNodes[1].x).attr('cy', resNodes[1].y)
              .transition().duration(200).attr('opacity', 0).remove();
          }
        }, 900));
      }

      // ---- CONSTITUTIONAL REPUBLIC: three branches, checks and balances ----
      if (arch.name === 'Constitutional Republic') {
        var branches = [
          {label: 'L', name: 'Legislative', x: acx - R * 0.7, y: areaTop + 35},
          {label: 'E', name: 'Executive',   x: acx + R * 0.7, y: areaTop + 35},
          {label: 'J', name: 'Judicial',    x: acx,            y: acy + R * 0.5}
        ];
        // Check lines between all pairs (behind nodes)
        var brPairs = [[0,1],[1,2],[0,2]];
        var checkLines = brPairs.map(function(pair) {
          return g.append('line')
            .attr('x1', branches[pair[0]].x).attr('y1', branches[pair[0]].y)
            .attr('x2', branches[pair[1]].x).attr('y2', branches[pair[1]].y)
            .attr('stroke', arch.color).attr('stroke-width', 1).attr('stroke-dasharray', '4,3')
            .attr('opacity', 0.2);
        });
        // Branch nodes
        var brEls = branches.map(function(b) {
          var el = g.append('circle').attr('cx', b.x).attr('cy', b.y).attr('r', 9)
            .attr('fill', arch.color).attr('opacity', 0.25).attr('stroke', arch.color).attr('stroke-width', 1.5);
          g.append('text').attr('x', b.x).attr('y', b.y + 3).attr('text-anchor', 'middle')
            .attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', arch.color).attr('opacity', 0.8).text(b.label);
          g.append('text').attr('x', b.x).attr('y', b.y + 16).attr('text-anchor', 'middle')
            .attr('font-size', '5.5px').attr('fill', arch.color).attr('opacity', 0.5).text(b.name);
          return el;
        });
        var brDotG = g.append('g');
        var brPhase = 0;
        // Sequence: L->E rule, E acts (pulse), J->E constraint, then repeat with different pair
        var sequences = [
          {from:0, to:1, color: arch.color, label:'rule'},
          {from:1, to:2, color: '#d97706', label:'act'},
          {from:2, to:1, color: '#dc2626', label:'check'},
          {from:0, to:2, color: arch.color, label:'review'},
          {from:2, to:0, color: '#16a34a', label:'uphold'}
        ];
        timers.push(setInterval(function() {
          var seq = sequences[brPhase % sequences.length];
          brPhase++;
          var from = branches[seq.from], to = branches[seq.to];
          // Pulse source
          brEls[seq.from].transition().duration(150).attr('opacity', 0.7).attr('r', 11)
            .transition().duration(300).attr('opacity', 0.25).attr('r', 9);
          // Dot travels
          var dot = brDotG.append('circle').attr('cx', from.x).attr('cy', from.y).attr('r', 2.5)
            .attr('fill', seq.color).attr('opacity', 0.9);
          dot.transition().duration(500).attr('cx', to.x).attr('cy', to.y)
            .transition().duration(250).attr('opacity', 0).remove();
          // Pulse target line
          var pairIdx = brPairs.findIndex(function(p) {
            return (p[0]===seq.from&&p[1]===seq.to)||(p[0]===seq.to&&p[1]===seq.from);
          });
          if (pairIdx >= 0) {
            checkLines[pairIdx].transition().duration(200).attr('opacity', 0.7).attr('stroke', seq.color)
              .transition().duration(500).attr('opacity', 0.2).attr('stroke', arch.color);
          }
        }, 900));
      }

      // ---- FRANCHISE: platform at top, participants comply or get removed ----
      if (arch.name === 'Franchise') {
        var platY = areaTop + 18, platH = 16;
        var nPart = 6;
        var partY = acy + 30;
        var partSpacing = (cellW - 40) / (nPart + 1);
        var parts = [];
        for (var i = 0; i < nPart; i++) {
          parts.push({x: ox + 20 + partSpacing * (i + 1), y: partY, active: true, id: i});
        }
        // Platform bar (behind nodes)
        g.append('rect').attr('x', ox + 14).attr('y', platY - platH / 2).attr('width', cellW - 28).attr('height', platH)
          .attr('rx', 4).attr('fill', arch.color).attr('opacity', 0.18).attr('stroke', arch.color).attr('stroke-width', 1.2);
        g.append('text').attr('x', acx).attr('y', platY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6.5px').attr('font-weight', 'bold').attr('fill', arch.color).attr('opacity', 0.7).text('PLATFORM');
        // Vertical rule lines (behind nodes)
        var ruleLineG = g.append('g');
        parts.forEach(function(pt) {
          ruleLineG.append('line').attr('x1', acx).attr('y1', platY + platH / 2).attr('x2', pt.x).attr('y2', partY - 6)
            .attr('stroke', arch.color).attr('stroke-width', 0.7).attr('opacity', 0.12);
        });
        // Participant nodes
        var partEls = parts.map(function(pt) {
          return g.append('circle').attr('cx', pt.x).attr('cy', pt.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.5).attr('stroke', arch.color).attr('stroke-width', 1);
        });
        var franDotG = g.append('g');
        var franPhase = 0;
        timers.push(setInterval(function() {
          franPhase++;
          if (franPhase % 5 === 0) {
            // Violation: one random active participant flashes red, then fades out
            var activeIdxs = parts.map(function(p,i){return i;}).filter(function(i){return parts[i].active;});
            if (activeIdxs.length < 2) {
              // Reset all
              parts.forEach(function(p,i){
                p.active = true;
                partEls[i].attr('opacity', 0.5).attr('fill', arch.color).attr('r', 5);
              });
              return;
            }
            var vi = activeIdxs[Math.floor(Math.random() * activeIdxs.length)];
            parts[vi].active = false;
            partEls[vi].transition().duration(200).attr('fill', '#dc2626').attr('r', 7)
              .transition().duration(500).attr('opacity', 0.1).attr('r', 3);
          } else {
            // Rule packet flows down from platform to random active participant
            var act = parts.filter(function(p){return p.active;});
            if (!act.length) return;
            var tgt = act[Math.floor(Math.random() * act.length)];
            var dot = franDotG.append('circle').attr('cx', acx).attr('cy', platY + platH / 2).attr('r', 2)
              .attr('fill', arch.color).attr('opacity', 0.8);
            dot.transition().duration(400).attr('cx', tgt.x).attr('cy', tgt.y - 6)
              .transition().duration(200).attr('opacity', 0).remove();
            // Compliance dot flows up
            var dot2 = franDotG.append('circle').attr('cx', tgt.x).attr('cy', tgt.y - 6).attr('r', 1.5)
              .attr('fill', '#16a34a').attr('opacity', 0.7);
            dot2.transition().delay(400).duration(400).attr('cx', acx).attr('cy', platY + platH / 2)
              .transition().duration(200).attr('opacity', 0).remove();
          }
        }, 650));
      }

      // ---- OPEN-SOURCE: community, merge authority, shared repo, fork ----
      if (arch.name === 'Open-Source') {
        var repoX = acx, repoY = acy - 10;
        var mergeX = acx + R * 0.75, mergeY = areaTop + 35;
        var nComm = 4;
        var commAgents = [];
        for (var i = 0; i < nComm; i++) {
          var a = (i / nComm) * Math.PI - Math.PI / 2 + 0.3;
          commAgents.push({x: acx - R * 0.7 + Math.cos(a) * 28, y: acy + Math.sin(a) * 40 - 10});
        }
        var forkRepoX = acx + R * 0.55, forkRepoY = areaTop + areaH - 22;
        var forkVisible = false;
        // Lines first
        commAgents.forEach(function(ca) {
          g.append('line').attr('x1', ca.x).attr('y1', ca.y).attr('x2', repoX).attr('y2', repoY)
            .attr('stroke', arch.color).attr('stroke-width', 0.7).attr('opacity', 0.18);
        });
        g.append('line').attr('x1', repoX).attr('y1', repoY).attr('x2', mergeX).attr('y2', mergeY)
          .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.2);
        // Repo
        g.append('rect').attr('x', repoX - 12).attr('y', repoY - 8).attr('width', 24).attr('height', 16)
          .attr('rx', 3).attr('fill', arch.color).attr('opacity', 0.15).attr('stroke', arch.color).attr('stroke-width', 1);
        g.append('text').attr('x', repoX).attr('y', repoY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '5.5px').attr('fill', arch.color).attr('opacity', 0.7).text('repo');
        // Merge authority (small council of 3 dots)
        g.append('circle').attr('cx', mergeX).attr('cy', mergeY).attr('r', 9)
          .attr('fill', arch.color).attr('opacity', 0.15).attr('stroke', arch.color).attr('stroke-width', 1);
        g.append('text').attr('x', mergeX).attr('y', mergeY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.7).text('MA');
        // Community nodes
        commAgents.forEach(function(ca) {
          g.append('circle').attr('cx', ca.x).attr('cy', ca.y).attr('r', 4.5)
            .attr('fill', arch.color).attr('opacity', 0.45).attr('stroke', arch.color).attr('stroke-width', 1);
        });
        // Fork repo (initially invisible)
        var forkEl = g.append('rect').attr('x', forkRepoX - 10).attr('y', forkRepoY - 7).attr('width', 20).attr('height', 14)
          .attr('rx', 3).attr('fill', '#4338ca').attr('opacity', 0).attr('stroke', '#4338ca').attr('stroke-width', 1);
        var forkLbl = g.append('text').attr('x', forkRepoX).attr('y', forkRepoY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '5.5px').attr('fill', '#4338ca').attr('opacity', 0).text('fork');
        var osDotG = g.append('g');
        var osPhase = 0;
        timers.push(setInterval(function() {
          osPhase++;
          var seq = osPhase % 6;
          if (seq <= 1) {
            // Community contribution to repo
            var ca = commAgents[osPhase % nComm];
            var dot = osDotG.append('circle').attr('cx', ca.x).attr('cy', ca.y).attr('r', 2)
              .attr('fill', arch.color).attr('opacity', 0.8);
            dot.transition().duration(400).attr('cx', repoX).attr('cy', repoY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (seq === 2) {
            // Merge authority reviews (check)
            var dot2 = osDotG.append('circle').attr('cx', repoX).attr('cy', repoY).attr('r', 2)
              .attr('fill', arch.color).attr('opacity', 0.8);
            dot2.transition().duration(400).attr('cx', mergeX).attr('cy', mergeY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (seq === 3) {
            // Accepted: green merge dot
            var dot3 = osDotG.append('circle').attr('cx', mergeX).attr('cy', mergeY).attr('r', 2.5)
              .attr('fill', '#16a34a').attr('opacity', 0.9);
            dot3.transition().duration(400).attr('cx', repoX).attr('cy', repoY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (seq === 4) {
            // Fork event
            forkVisible = true;
            forkEl.transition().duration(400).attr('opacity', 0.7);
            forkLbl.transition().duration(400).attr('opacity', 0.7);
            var dot4 = osDotG.append('circle').attr('cx', repoX).attr('cy', repoY).attr('r', 2)
              .attr('fill', '#4338ca').attr('opacity', 0.9);
            dot4.transition().duration(500).attr('cx', forkRepoX).attr('cy', forkRepoY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else {
            // Fork fades back
            forkEl.transition().duration(600).attr('opacity', 0);
            forkLbl.transition().duration(600).attr('opacity', 0);
          }
        }, 700));
      }

      // ---- SORTITION: population pool, random selection, council deliberation ----
      if (arch.name === 'Sortition') {
        var nPop = 18;
        var popDots = [];
        for (var i = 0; i < nPop; i++) {
          var a = (i / nPop) * Math.PI * 2;
          var rr = R * 0.82;
          popDots.push({x: acx + Math.cos(a) * rr, y: acy + Math.sin(a) * rr, selected: false, id: i});
        }
        var council = [];
        var councilorEls = [];
        // Council deliberation lines first, so population dots render on top
        var councilLineG = g.append('g');
        // Draw population ring dots (lines not applicable here; dots only)
        var popEls = popDots.map(function(pd) {
          return g.append('circle').attr('cx', pd.x).attr('cy', pd.y).attr('r', 3)
            .attr('fill', arch.color).attr('opacity', 0.3);
        });
        // Randomizer pulsing ring
        var randRing = g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 12)
          .attr('fill', 'none').attr('stroke', arch.color).attr('stroke-width', 1).attr('stroke-dasharray', '5,3').attr('opacity', 0.25);
        g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.5).text('lot');
        var sortPhase = 0;
        var councilIdxs = [];
        timers.push(setInterval(function() {
          sortPhase++;
          if (sortPhase % 8 === 1) {
            // Randomizer pulses
            randRing.transition().duration(300).attr('r', 18).attr('opacity', 0.7).attr('stroke-width', 2)
              .transition().duration(400).attr('r', 12).attr('opacity', 0.25).attr('stroke-width', 1);
            // Pick 3 random new council members
            councilIdxs = [];
            var pool = popDots.map(function(_,i){return i;});
            while (councilIdxs.length < 3 && pool.length > 0) {
              var ri = Math.floor(Math.random() * pool.length);
              councilIdxs.push(pool[ri]);
              pool.splice(ri, 1);
            }
            // Reset all
            popEls.forEach(function(el) { el.attr('fill', arch.color).attr('opacity', 0.3).attr('r', 3); });
            // Highlight selected
            councilIdxs.forEach(function(ci) {
              popEls[ci].transition().duration(300).attr('fill', arch.color).attr('r', 5.5).attr('opacity', 0.9);
            });
            // Draw council center lines
            councilLineG.selectAll('*').remove();
            if (councilIdxs.length >= 2) {
              for (var ii = 0; ii < councilIdxs.length; ii++) {
                for (var jj = ii+1; jj < councilIdxs.length; jj++) {
                  var pa = popDots[councilIdxs[ii]], pb = popDots[councilIdxs[jj]];
                  councilLineG.append('line').attr('x1', pa.x).attr('y1', pa.y).attr('x2', pb.x).attr('y2', pb.y)
                    .attr('stroke', arch.color).attr('stroke-width', 1).attr('opacity', 0.4);
                }
              }
            }
          } else if (sortPhase % 8 >= 2 && sortPhase % 8 <= 5) {
            // Council deliberates: pulsing lines
            councilLineG.selectAll('line').transition().duration(200)
              .attr('opacity', (sortPhase % 2 === 0) ? 0.7 : 0.2);
          } else if (sortPhase % 8 === 6) {
            // Decision made: green flash on council members
            councilIdxs.forEach(function(ci) {
              popEls[ci].transition().duration(200).attr('fill', '#16a34a').attr('r', 6)
                .transition().duration(400).attr('fill', arch.color).attr('r', 3).attr('opacity', 0.3);
            });
            councilLineG.selectAll('line').transition().duration(400).attr('opacity', 0).remove();
          }
        }, 550));
      }

      // ---- ADHOCRACY: problem appears, specialists converge, solve, disperse ----
      if (arch.name === 'Adhocracy') {
        var nSpec = 5;
        var specHome = [];
        for (var i = 0; i < nSpec; i++) {
          var a = (i / nSpec) * Math.PI * 2 + 0.5;
          var rr = R * 0.85;
          specHome.push({x: acx + Math.cos(a) * rr, y: acy + Math.sin(a) * rr});
        }
        // Cluster lines first, so specialist nodes render on top
        var adhLineG = g.append('g');
        var specEls = specHome.map(function(sh) {
          return g.append('circle').attr('cx', sh.x).attr('cy', sh.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.45).attr('stroke', arch.color).attr('stroke-width', 1);
        });
        var probEl = g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 7)
          .attr('fill', '#dc2626').attr('opacity', 0).attr('stroke', '#dc2626').attr('stroke-width', 1.5);
        g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '7px').attr('fill', 'white').attr('font-weight', 'bold').attr('opacity', 0).text('!');
        var adhPhase = 0;
        timers.push(setInterval(function() {
          adhPhase++;
          var step = adhPhase % 10;
          if (step === 0) {
            // Problem appears
            probEl.transition().duration(250).attr('opacity', 0.85);
            adhLineG.selectAll('*').remove();
          } else if (step >= 1 && step <= 3) {
            // Specialists converge
            specEls.forEach(function(el, i) {
              var sh = specHome[i];
              var progress = step / 4;
              var tx = sh.x + (acx - sh.x) * progress;
              var ty = sh.y + (acy - sh.y) * progress;
              el.transition().duration(400).attr('cx', tx).attr('cy', ty);
            });
          } else if (step === 4) {
            // Cluster formed: draw lines between specialists (adhLineG sits behind nodes)
            adhLineG.selectAll('*').remove();
            for (var ii = 0; ii < nSpec; ii++) {
              for (var jj = ii+1; jj < nSpec; jj++) {
                var pa = specHome[ii], pb = specHome[jj];
                var cx0 = pa.x + (acx - pa.x) * 0.9, cy0 = pa.y + (acy - pa.y) * 0.9;
                var cx1 = pb.x + (acx - pb.x) * 0.9, cy1 = pb.y + (acy - pb.y) * 0.9;
                adhLineG.append('line').attr('x1', cx0).attr('y1', cy0).attr('x2', cx1).attr('y2', cy1)
                  .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.35);
              }
            }
          } else if (step >= 5 && step <= 6) {
            // Working pulse
            probEl.transition().duration(150).attr('r', 10)
              .transition().duration(250).attr('r', 7);
          } else if (step === 7) {
            // Problem resolved
            probEl.transition().duration(300).attr('fill', '#16a34a').attr('stroke', '#16a34a')
              .transition().duration(400).attr('opacity', 0).attr('fill', '#dc2626').attr('stroke', '#dc2626');
            adhLineG.selectAll('line').transition().duration(400).attr('opacity', 0);
          } else if (step >= 8) {
            // Disperse back
            specEls.forEach(function(el, i) {
              el.transition().duration(600).attr('cx', specHome[i].x).attr('cy', specHome[i].y);
            });
          }
        }, 500));
      }

      // ---- MISSION COMMAND: one intent, three autonomous paths to shared goal ----
      if (arch.name === 'Mission Command') {
        var cmdrX = acx, cmdrY = areaTop + 22;
        var goalX = acx, goalY = areaTop + areaH - 18;
        var subordinates = [
          {x: acx - R * 0.65, y: acy - 15, color: '#16a34a',  trail: 'left'},
          {x: acx,             y: acy - 15, color: '#d97706',  trail: 'right'},
          {x: acx + R * 0.65, y: acy - 15, color: '#2563eb',  trail: 'zigzag'}
        ];
        // Static structure lines (behind nodes)
        subordinates.forEach(function(s) {
          g.append('line').attr('x1', cmdrX).attr('y1', cmdrY).attr('x2', s.x).attr('y2', s.y)
            .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.2);
        });
        // Commander
        g.append('circle').attr('cx', cmdrX).attr('cy', cmdrY).attr('r', 8)
          .attr('fill', arch.color).attr('opacity', 0.3).attr('stroke', arch.color).attr('stroke-width', 1.5);
        g.append('text').attr('x', cmdrX).attr('y', cmdrY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '7px').attr('font-weight', 'bold').attr('fill', arch.color).text('CO');
        // Goal circle
        g.append('circle').attr('cx', goalX).attr('cy', goalY).attr('r', 9)
          .attr('fill', '#16a34a').attr('opacity', 0.15).attr('stroke', '#16a34a').attr('stroke-width', 1.2);
        g.append('text').attr('x', goalX).attr('y', goalY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', '#16a34a').attr('opacity', 0.7).text('goal');
        // Subordinates
        subordinates.forEach(function(s) {
          g.append('circle').attr('cx', s.x).attr('cy', s.y).attr('r', 5.5)
            .attr('fill', s.color).attr('opacity', 0.5).attr('stroke', s.color).attr('stroke-width', 1);
        });
        // Intent label on commander
        var intentG = g.append('g');
        var mcDotG = g.append('g');
        var mcPhase = 0;
        timers.push(setInterval(function() {
          mcPhase++;
          var step = mcPhase % 8;
          if (step === 0) {
            // Commander sends intent packet (large, distinctive)
            var dot = mcDotG.append('circle').attr('cx', cmdrX).attr('cy', cmdrY).attr('r', 4)
              .attr('fill', arch.color).attr('opacity', 0.9);
            dot.transition().duration(300).attr('cx', acx).attr('cy', acy - 15).attr('r', 3)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (step === 1) {
            // Sub 0: sweeps left then down to goal
            var d0 = mcDotG.append('circle').attr('cx', subordinates[0].x).attr('cy', subordinates[0].y).attr('r', 2.5)
              .attr('fill', subordinates[0].color).attr('opacity', 0.85);
            d0.transition().duration(300).attr('cx', subordinates[0].x - 20).attr('cy', acy + 10)
              .transition().duration(300).attr('cx', goalX).attr('cy', goalY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (step === 2) {
            // Sub 1: sweeps right then down
            var d1 = mcDotG.append('circle').attr('cx', subordinates[1].x).attr('cy', subordinates[1].y).attr('r', 2.5)
              .attr('fill', subordinates[1].color).attr('opacity', 0.85);
            d1.transition().duration(300).attr('cx', subordinates[1].x + 22).attr('cy', acy + 5)
              .transition().duration(300).attr('cx', goalX).attr('cy', goalY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (step === 3) {
            // Sub 2: zigzag to goal
            var d2 = mcDotG.append('circle').attr('cx', subordinates[2].x).attr('cy', subordinates[2].y).attr('r', 2.5)
              .attr('fill', subordinates[2].color).attr('opacity', 0.85);
            d2.transition().duration(200).attr('cx', subordinates[2].x - 15).attr('cy', acy)
              .transition().duration(200).attr('cx', subordinates[2].x + 10).attr('cy', acy + 20)
              .transition().duration(200).attr('cx', goalX).attr('cy', goalY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (step === 4) {
            // Goal pulses green to show convergence
            g.select('circle[cy="' + goalY + '"]')
              .transition().duration(200).attr('r', 13).attr('opacity', 0.5)
              .transition().duration(400).attr('r', 9).attr('opacity', 0.15);
          }
        }, 600));
      }

      // ---- MECHANISM DESIGN: 4 agents, incentive structure channels self-interest ----
      if (arch.name === 'Mechanism Design') {
        var nMech = 4;
        var mechAgents = [];
        for (var i = 0; i < nMech; i++) {
          var a = (i / nMech) * Math.PI * 2 - Math.PI / 4;
          mechAgents.push({x: acx + Math.cos(a) * R * 0.65, y: acy + Math.sin(a) * R * 0.65, id: i});
        }
        var agentColors = ['#7c2d12', '#92400e', '#78350f', '#713f12'];
        // Game framework: octagon outline connecting all agents (lines first)
        for (var i = 0; i < nMech; i++) {
          for (var j = i+1; j < nMech; j++) {
            g.append('line').attr('x1', mechAgents[i].x).attr('y1', mechAgents[i].y)
              .attr('x2', mechAgents[j].x).attr('y2', mechAgents[j].y)
              .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.15)
              .attr('stroke-dasharray', '3,2');
          }
        }
        // Lines from agents to center outcome
        mechAgents.forEach(function(ma) {
          g.append('line').attr('x1', ma.x).attr('y1', ma.y).attr('x2', acx).attr('y2', acy)
            .attr('stroke', arch.color).attr('stroke-width', 0.6).attr('opacity', 0.12);
        });
        // Center outcome node
        g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 10)
          .attr('fill', '#16a34a').attr('opacity', 0.15).attr('stroke', '#16a34a').attr('stroke-width', 1.2);
        g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '5.5px').attr('fill', '#16a34a').attr('opacity', 0.7).text('outcome');
        // Agent nodes
        var mechEls = mechAgents.map(function(ma, i) {
          var el = g.append('circle').attr('cx', ma.x).attr('cy', ma.y).attr('r', 6)
            .attr('fill', agentColors[i % agentColors.length]).attr('opacity', 0.55)
            .attr('stroke', agentColors[i % agentColors.length]).attr('stroke-width', 1);
          g.append('text').attr('x', ma.x).attr('y', ma.y + 3).attr('text-anchor', 'middle')
            .attr('font-size', '5.5px').attr('fill', 'white').attr('opacity', 0.8).text('A' + (i+1));
          return el;
        });
        var mechDotG = g.append('g');
        var mechPhase = 0;
        timers.push(setInterval(function() {
          mechPhase++;
          var actorIdx = mechPhase % nMech;
          var ma = mechAgents[actorIdx];
          // Agent acts in self-interest (arrow toward center)
          var dot = mechDotG.append('circle').attr('cx', ma.x).attr('cy', ma.y).attr('r', 2.5)
            .attr('fill', agentColors[actorIdx]).attr('opacity', 0.85);
          dot.transition().duration(350).attr('cx', acx + (ma.x - acx) * 0.3).attr('cy', acy + (ma.y - acy) * 0.3)
            .transition().duration(250).attr('cx', acx).attr('cy', acy).attr('fill', '#16a34a')
            .transition().duration(200).attr('opacity', 0).remove();
          // Outcome pulses green as contributions arrive
          if (mechPhase % 4 === 0) {
            g.select('circle').filter(function() {
              var cx0 = parseFloat(d3.select(this).attr('cx'));
              var cy0 = parseFloat(d3.select(this).attr('cy'));
              return Math.abs(cx0 - acx) < 1 && Math.abs(cy0 - acy) < 1;
            }).transition().duration(200).attr('r', 14).attr('opacity', 0.45)
              .transition().duration(350).attr('r', 10).attr('opacity', 0.15);
          }
          // Agent pulses to show self-interest
          mechEls[actorIdx].transition().duration(150).attr('r', 8).attr('opacity', 0.8)
            .transition().duration(300).attr('r', 6).attr('opacity', 0.55);
        }, 600));
      }

      // ---- IMMUNE SYSTEM: population, anomaly, innate scan, adaptive response ----
      if (arch.name === 'Immune System') {
        var nImm = 14;
        var immPop = [];
        for (var i = 0; i < nImm; i++) {
          var a = (i / nImm) * Math.PI * 2;
          var rr = R * 0.72;
          immPop.push({x: acx + Math.cos(a) * rr, y: acy + Math.sin(a) * rr, anomalous: false, id: i});
        }
        var anomalyIdx = -1;
        var responderX = acx, responderY = acy;
        // Innate scan ring
        var scanRing = g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', R * 0.3)
          .attr('fill', 'none').attr('stroke', '#fbbf24').attr('stroke-width', 1).attr('opacity', 0);
        // Responder node (hidden initially)
        var responderEl = g.append('circle').attr('cx', responderX).attr('cy', responderY).attr('r', 7)
          .attr('fill', '#881337').attr('opacity', 0).attr('stroke', '#881337').attr('stroke-width', 1.5);
        g.append('text').attr('x', responderX).attr('y', responderY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('font-weight', 'bold').attr('fill', 'white').attr('opacity', 0).text('R');
        // Population dots
        var immEls = immPop.map(function(ip) {
          return g.append('circle').attr('cx', ip.x).attr('cy', ip.y).attr('r', 3.5)
            .attr('fill', arch.color).attr('opacity', 0.45);
        });
        var immDotG = g.append('g');
        var immPhase = 0;
        timers.push(setInterval(function() {
          immPhase++;
          var step = immPhase % 12;
          if (step === 0) {
            // Pick a random anomaly
            anomalyIdx = Math.floor(Math.random() * nImm);
            immEls[anomalyIdx].transition().duration(300).attr('fill', '#dc2626').attr('r', 5.5).attr('opacity', 0.9);
          } else if (step === 2) {
            // Innate scan wave expands
            scanRing.attr('r', R * 0.1).attr('opacity', 0.7)
              .transition().duration(600).attr('r', R * 0.9).attr('opacity', 0);
          } else if (step === 3) {
            // Innate flags it: anomaly pulses brighter
            if (anomalyIdx >= 0) {
              immEls[anomalyIdx].transition().duration(150).attr('r', 7)
                .transition().duration(250).attr('r', 5.5);
            }
          } else if (step === 4) {
            // Adaptive responder moves toward anomaly
            var ap = immPop[anomalyIdx >= 0 ? anomalyIdx : 0];
            responderEl.attr('cx', acx).attr('cy', acy).attr('opacity', 0.85);
            responderEl.transition().duration(500).attr('cx', (acx + ap.x) / 2).attr('cy', (acy + ap.y) / 2);
          } else if (step === 6) {
            // Confirmed: isolate - push anomaly outward and show barrier
            if (anomalyIdx >= 0) {
              var ap = immPop[anomalyIdx];
              var outX = acx + (ap.x - acx) * 1.5;
              var outY = acy + (ap.y - acy) * 1.5;
              // Barrier ring around anomaly position
              var barrier = immDotG.append('circle').attr('cx', ap.x).attr('cy', ap.y).attr('r', 8)
                .attr('fill', 'none').attr('stroke', '#dc2626').attr('stroke-width', 1.5).attr('opacity', 0.8);
              immEls[anomalyIdx].transition().duration(400).attr('cx', outX).attr('cy', outY)
                .transition().duration(300).attr('opacity', 0.2);
              barrier.transition().delay(400).duration(500).attr('cx', outX).attr('cy', outY)
                .transition().duration(400).attr('opacity', 0).remove();
              responderEl.transition().duration(300).attr('cx', ap.x).attr('cy', ap.y)
                .transition().duration(300).attr('opacity', 0);
            }
          } else if (step === 9) {
            // Reset: restore normal agent
            if (anomalyIdx >= 0) {
              immEls[anomalyIdx]
                .attr('cx', immPop[anomalyIdx].x).attr('cy', immPop[anomalyIdx].y)
                .transition().duration(300).attr('fill', arch.color).attr('r', 3.5).attr('opacity', 0.45);
              anomalyIdx = -1;
            }
            responderEl.attr('cx', acx).attr('cy', acy).attr('opacity', 0);
          }
        }, 450));
      }

      // ---- KB ICON for each panel ----
      var kbX = ox + cellW - 22, kbY = oy + cellH - 18;
      // Guild gets 3 mini KBs, Colony gets 3 scattered ones
      if (arch.name === 'Guild') {
        // Guild: 3 KBs near each cluster
        [[ox + cellW * 0.2, acy - R * 0.4], [ox + cellW * 0.8, acy - R * 0.4], [ox + cellW * 0.5, acy + R * 0.5]].forEach(function(pos) {
          g.append('rect').attr('x', pos[0] - 4).attr('y', pos[1] - 5).attr('width', 8).attr('height', 10)
            .attr('rx', 2).attr('fill', '#f0fdf4').attr('stroke', arch.color).attr('stroke-width', 0.8);
          g.append('line').attr('x1', pos[0] - 3).attr('y1', pos[1]).attr('x2', pos[0] + 3).attr('y2', pos[1])
            .attr('stroke', arch.color).attr('stroke-width', 0.5);
        });
      } else if (arch.name === 'Colony') {
        // Colony: 3 scattered mini KBs
        [[ox + 20, areaTop + 20], [ox + cellW - 20, areaTop + areaH * 0.4], [ox + cellW * 0.4, areaTop + areaH - 15]].forEach(function(pos) {
          g.append('rect').attr('x', pos[0] - 3).attr('y', pos[1] - 4).attr('width', 6).attr('height', 8)
            .attr('rx', 2).attr('fill', '#faf5ff').attr('stroke', arch.color).attr('stroke-width', 0.6);
          g.append('line').attr('x1', pos[0] - 2).attr('y1', pos[1]).attr('x2', pos[0] + 2).attr('y2', pos[1])
            .attr('stroke', arch.color).attr('stroke-width', 0.4);
        });
      } else {
        // Single KB icon
        g.append('rect').attr('x', kbX - 5).attr('y', kbY - 6).attr('width', 10).attr('height', 12)
          .attr('rx', 3).attr('fill', '#f8fafc').attr('stroke', arch.color).attr('stroke-width', 0.8);
        g.append('line').attr('x1', kbX - 4).attr('y1', kbY).attr('x2', kbX + 4).attr('y2', kbY)
          .attr('stroke', arch.color).attr('stroke-width', 0.5);
        g.append('text').attr('x', kbX).attr('y', kbY + 15)
          .attr('text-anchor', 'middle').attr('font-size', '5px').attr('fill', '#999').text('KB');
      }
    }); // end drawFns.push
  }); // end archetypes.forEach

  function clearTimers(timers) {
    timers.forEach(function(t) {
      if (t && t.clear) t.clear();
      else { clearInterval(t); clearTimeout(t); }
    });
    timers.length = 0;
  }

  function init() {
    clearTimers(animTimers);
    svg.selectAll('g.cell').remove();
    var ztState = {running: true};
    archetypes.forEach(function(arch, idx) {
      var col = idx % cols, row = Math.floor(idx / cols);
      var ox = padX + col * (cellW + 5), oy = padY + row * (cellH + 12);
      drawFns[idx]({targetSvg: svg, timers: animTimers, ox: ox, oy: oy, ztState: ztState});
    });
  }

  var socExpanded = false;

  function socExpandPanel(idx) {
    if (socExpanded) return;
    socExpanded = true;
    var arch = archetypes[idx];
    var modalTimers = [];
    var modalZtState = {running: true};
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
      .on('click', socCloseExpand);
    var header = card.append('div').style('padding', '16px 20px 4px');
    header.append('div').style('font-size', '18px').style('font-weight', 'bold')
      .style('color', arch.color).text(arch.name);
    header.append('div').style('font-size', '11px').style('color', '#aaa')
      .style('margin-top', '2px').text(arch.sub);
    var svgWrap = card.append('div').style('padding', '0 12px 16px');
    var expSvg = svgWrap.append('svg')
      .attr('viewBox', '0 0 ' + cellW + ' ' + cellH)
      .style('width', '100%').style('display', 'block');
    addDefsToSvg(expSvg);
    drawFns[idx]({targetSvg: expSvg, timers: modalTimers, ox: 0, oy: 0, ztState: modalZtState});
    card.append('div').style('text-align', 'center').style('padding', '0 0 12px')
      .style('font-size', '10px').style('color', '#ccc').style('font-style', 'italic')
      .text('Click anywhere outside to close');
    setTimeout(function() { backdrop.style('opacity', '1'); }, 10);
    backdrop.on('click', function(event) {
      if (event.target === backdrop.node()) socCloseExpand();
    });
    function socCloseExpand() {
      socExpanded = false;
      modalZtState.running = false;
      clearTimers(modalTimers);
      backdrop.style('opacity', '0');
      setTimeout(function() { backdrop.remove(); }, 250);
    }
  }

  init();

  // Add click overlays for each cell
  function addOverlays() {
    svg.selectAll('.soc-overlay').remove();
    archetypes.forEach(function(arch, idx) {
      var col = idx % cols, row = Math.floor(idx / cols);
      var ox = padX + col * (cellW + 5), oy = padY + row * (cellH + 12);
      svg.append('rect').attr('class', 'soc-overlay')
        .attr('x', ox).attr('y', oy).attr('width', cellW).attr('height', cellH)
        .attr('fill', 'transparent').style('cursor', 'pointer')
        .on('click', function(event) { event.stopPropagation(); socExpandPanel(idx); });
    });
  }
  addOverlays();

  svg.on('click', function() {
    clearTimers(animTimers);
    init();
    addOverlays();
  });
})();

</script>

