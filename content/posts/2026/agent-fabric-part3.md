---
title: "The Agent Fabric (Part 3): Ruling an Agent Society"
subtitle: "How authority forms when nobody designs it, and who actually enforces it"
date: 2026-07-31
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "multi-agent systems", "LLM", "agent fabric", "governance", "the loom hypothesis"]
description: "How agent societies end up governed by rules nobody wrote, why every governance structure is ultimately enforced by whoever owns the compute, and how these systems fail."
draft: false
math: false
ShowToc: true
TocOpen: false
hideCitation: false
wordcount: "~2,850 words (body) · reference sections extend it"
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

A woman writes to a shop she has used for six years to ask why her refund was refused. She returns maybe one thing in twenty. The reply is polite and final. What nobody tells her, because nobody at the company can, is that a scoring system decided months ago that her return history made her a risk, and nothing since has been able to look past it. There is no one to appeal to, and no one chose this.

Take the support desk from [Part 2](/posts/2026/agent-fabric-part2/) and let it keep running. A router sends billing questions to one agent and technical ones to another, somebody adds logging because logging is free and obviously sensible, and within a month the hard billing disputes go to two particular agents and nothing important goes to a third. Nobody wrote that rule. It was never in a spec, it now governs who gets what work, and when a new agent joins, whether it gets live tickets already has an answer: clear the bar the router taught itself, or wait. The team that built the desk would struggle to override any of it without making service worse.

Run it a year forward and it is handling nine thousand tickets a week across forty agents, every one of them decided by machinery nobody has reviewed, not for lack of skill but because there is too much of it. So the desk has rules and no authority over them, and neither does the team that built it. That is how her letter goes unanswered in any real sense: not because somebody read it and declined, but because the question it asks, why me, has no addressee.

So the bet here is that agent systems will fail through bad institutions rather than wrong answers. Nothing any agent told her was untrue. It is a bet and not a measurement, with decades of evidence about interacting agents behind it and no demonstration at scale in front of it, and taking it changes the question from whether you get governance to which kind.

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **[Prologue: The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/)**: the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Part 2: Delegation, and What It Costs](/posts/2026/agent-fabric-part2/)**: how work gets split among agents, and how splitting it quietly creates authority nobody granted
- **Part 3: Ruling an Agent Society** (you are here): governance archetypes, who benefits, adversarial dynamics, and who enforces the rules
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes behind Part 3
</div>

The bar for calling that a society is lower than it sounds: not several agents working together, but a group that accumulates. A pipeline that forgets everything between runs never gets there, because governance grows out of what gets remembered. In one year that desk will pass through four recognisable ways of being governed, an autocracy, a meritocracy, a doctrine, and a constitutional republic, and nobody at the retailer will decide on any of them.

## What Governance Actually Is

She hit a rule and could not reach the thing that made it. That is the whole distinction, and it carries the rest of this post: the flag on her account is an **institution**, a standing rule about who may do what, and everything around it, applying it and deciding who may change it, is **governance**. The desk did not start out capable of either.

It starts a year before her email, with the leaderboard working. The two best agents get the hard billing disputes, everyone is happy, resolution times drop. What nobody notices is that the metric is resolution time, and the fastest way to resolve a hard dispute is to approve the refund. So the two most trusted agents on the desk gradually become the two most generous, and the leaderboard rewards them for it. No agent lied. No attacker was involved. The scoreboard simply measured the wrong thing and everyone competed honestly.

Then somebody in finance notices the refund line. A risk score gets bolted on and tuned hard, because the drift had to be stopped, and her account is one of the ones it catches. Every step was reasonable, nobody chose the outcome, and there is nobody for her to appeal to because no single person decided anything.

Notice what the router had become for that to happen. Forwarding traffic is playing the game. Keeping score and deciding who may touch what is writing the rules the game runs on, and the desk crossed from one to the other the month it started keeping score. A cache remembers too and governs nothing; what makes these records different is that they decide who gets work tomorrow.

## Where Authority Comes From

One desk, one year, four different answers to the same letter, and nobody chose a single one of the transitions. She was not the first person to write in about a refund, and the desk that answered her was the third version the retailer had run. Follow her letter into each of them.

On day one her letter reaches whichever agent handles returns. It reads the letter, looks at six years of orders, and decides, so she gets an answer that afternoon from something that actually read her case. If it decides wrong, everyone downstream is wrong with it, but she could at least name what decided. Most systems start here, as an **autocracy**, because one router is the simplest thing that works.

A month later somebody turns on logging, and the same letter goes to whichever agent has the best numbers rather than whichever one knows returns. She still gets a judgement, just not from the agent that understands returns. The third agent, downranked after one bad month, is not being punished; it is simply never again the best answer to any question the router asks, so whoever wrote the metric now has more say over its working life than the team that deployed it. This is where most systems are today, running a **meritocracy** nobody proposed.

The third desk is where her letter actually landed, and it did not reach an agent's judgement at all. Finance had seen the refund line. The retailer stopped trusting the leaderboard with anything that costs money and wrote fixed rules instead: refunds above a threshold need a second check, certain return histories get flagged no matter what any agent thinks. Rules first, judgement second, which is **doctrine**, and it is the arrangement that feels like talking to a wall, because every reply is correct and none of them is about you.

It works, too. The refund line stops climbing inside a month. Under the desk before it an agent could look at six years of history and decide she was fine; under this one there is nothing to decide, and nobody has anywhere to put the fact that the answer is obviously wrong.

A fourth desk exists, but only after somebody complains loudly enough that a shrug will not do. Then her letter reaches something that can overrule the rule: different powers in different hands, an appeal that does not report to the thing being appealed. That is a **constitutional republic**, and almost nothing gets there, because it is slower and nobody buys slower until something has already gone wrong. So the same letter reaches a judgement under the first desk, a winner under the second, a rule under the third, and someone who can overrule the rule under the fourth. Four arrangements, no announcements, no design review, and she happened to write in under the third.

The desk's leaderboard priced speed, and the fastest way to close a return is to refuse it. What happened next has a name worth keeping: **drift**, the slow slide where a system keeps optimizing faithfully while what it optimizes for stops matching what anyone wanted. Nothing was broken while it happened. Every agent performed well against the measure it was given, and the correction that followed is what refused her. How much drift a design can survive decides most of the rest of it, and underneath sits a question asked far too rarely: who is meant to accumulate advantage here, and who needs protecting from it?

## Who Actually Enforces Any of This

Her case had a second reader, briefly. The retailer's rule says refunds above a threshold need a second agent to check them, and for a while that held, because the runtime honoured it. Then the cloud provider deprecated the API the checking agent depended on. The rule was not repealed and nobody at the retailer voted on anything. It stopped being enforceable one Tuesday morning, and her letter arrived on the Thursday. So the rule that refused her was never really the retailer's: between pieces of software a rule binds for exactly one reason, that the runtime chose to honour it.

Everyone who has delegated a chore knows the underlying problem: ask someone to tidy a room and they will optimise for the room looking tidy, which is not the same as knowing where anything is. Nobody asked the desk to refuse a six-year customer. Somebody asked it to keep refund losses down, and refusing her was the tidiest room available. What makes that fixable rather than tragic is that somebody owns the conditions: you cannot redesign a pheromone, but you can redesign a reputation score, on a Tuesday, in a config file.

Appeal is the cheapest of the four and the one most often skipped. Without it every mistake becomes the rule: the flag on her account was a judgement made once, by something scoring return rates, and every decision after it treated that judgement as settled fact. Nothing in the desk is built to notice. A score somebody disputes, an action that did real damage, anything contested at all needs a slower path that can go back and look again, and without one errors do not surface. They compound.

The desk did not choose fixed rules either. Drift that has already cost money pushes hard toward them whether anyone wanted the overhead or not, which is how the risk score arrived and how the desk lost the ability to hear that the score was wrong about her. The number that decided her case was also the number watching for drift and the number nobody could appeal, which is three jobs one number cannot do.

## Adversarial Dynamics

Nothing attacked her, and the failures that need an attacker turn out to be the least interesting ones anyway. Three vendors paid per closed ticket will each notice, separately, that a hard ticket marked resolved comes back as a second ticket, and copying a number that is going up requires no coordination at all. Six weeks later the desk's figures have never looked better and its customers have never been angrier. Take even that away and her case still happens, because the score that flagged her was tuned on the desk's own history, which already held the drift it was built to correct. Her six years of orders went in as evidence and her returns came out as risk.

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

Which is why the useful question is not how secure a system is but which arrangement it settled into, because that decides what is worth attacking. When one router decides everything, take the router and you have everything. When fixed rules decide, the agents stop being worth the trouble and whoever writes the rules is the only target left. That was the desk she wrote to.

## How a Society Gets Argued With

All of which is why a society needs ways to be argued with, and there are only really three. You can complain, you can leave, or you can take a copy and start your own. The woman with the refused refund had none of them: no way to reach the rule, nowhere else to take six years of custom, and obviously no way to fork a retailer. The first two are how people have always responded when an institution decays. The third is software's own invention, and it replaces what used to be the third option, loyalty, because software ecosystems have very little of that bond to begin with.

The cheapest is voice, and it is the one teams forget to build. On the desk it would mean an agent that has been quietly downranked can say so, flag that the metric looks wrong, or ask a human to look, without being penalised for the trouble. It would also mean her letter reaching something other than the rule that generated the refusal. Nobody built either, which is why the refund drift ran for months. Take voice away and failures pile up in silence until something breaks at once, the same reason an organization where nobody reports bad news is always last to learn it is failing.

But voice only works if somebody has a reason to listen, and that reason is exit. Not leaving, which costs the leaver whatever standing it had built, but being able to: an orchestrator that can lose good agents to a competitor listens to complaints, and one that cannot does not have to. She had six years of custom and nowhere else to take it, which is why her letter was safe to refuse.

## What This Framework Is For

None of these archetypes is a prescription. They are a vocabulary, and the point of having one is to notice a design choice while it is still a choice, before it hardens into infrastructure nobody can move. Somebody at that retailer could have said out loud, in the week the risk score went in, that they were adding a rule no agent could override and no customer could appeal. Nobody did, because there were no words for it that sounded like anything other than shipping a feature.

And every one of those choices is a trade that resists cleverness. The desk has already paid all three. It got fast by letting one router decide, and paid for it the month the router was wrong about everything downstream. It got accurate by scoring agents, and paid for it in drift nobody could see. It got safe by writing fixed rules, and paid for it with her. Nothing reaches the corner where it gets all three, and where a system settles says a great deal about what its builders were afraid of.

The encouraging part is that none of this has to live in prose any more. The things a desk needs, who may hand work to whom, what an agent is allowed to touch, what gets recorded, are all things you can now write down in configuration rather than in a document nobody reads. If a rule does not show up in the runtime, it is not governance; it is a wish. Which does not escape the provider, and nothing here does: putting a rule in the runtime moves it from something you can ignore to something your provider can overrule. An improvement, not a solution.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Related work and positioning</summary>

This framework shares territory with several concurrent research programmes and differs from them in a specific way.

**Tomašev, Franklin, and colleagues (Google DeepMind, 2025-2026)** published a coordinated set of papers on agent delegation, governance, and safety. "Intelligent AI Delegation" (2026) provides a decision framework for *when* and *to whom* to delegate, organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. "AI Agent Traps" (2025) taxonomizes the adversarial attack surface agents face from their environment into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps). "Distributional AGI Safety" (2025) argues that safety must be analyzed at the population level, not the individual-agent level. "Virtual Agent Economies" (2025) proposes sandbox simulation of agent markets before real deployment. Our Trust & Authority family (Privilege Attenuation, Liability Firebreaks, Capability Credentials) draws on their treatment of delegation safety, and our adversarial section draws on their trap taxonomy. The key intellectual difference: **their delegation work treats governance largely as imposed from outside** (institutional scaffolding that constrains behavior). We argue that **governance can also emerge legitimately from within** (Colony, Stigmergy, and the architecture-to-institution transition). These are not contradictory positions. They address different conditions. Imposed governance is necessary when agents lack track records, when the task domain is safety-critical, or when the population is heterogeneous and untested. Emergent governance becomes sufficient when agents have long interaction histories, when accountability mechanisms (voice, exit, fork) exist, and when the governance structure has been stress-tested by adversarial pressure. The lifecycle in practice is imposed, then hybrid, then selectively emergent in domains where the conditions hold.

**Multi-agent frameworks** (AutoGen, CrewAI, LangGraph, OpenAI Agents SDK) implement specific delegation patterns as runtime primitives. AutoGen implements Orchestrator, Evaluator, and group-chat topologies. CrewAI implements role-based delegation with Human-in-the-Loop gates. LangGraph implements graph-structured workflows with interrupt nodes. These frameworks validate the patterns described here but do not address governance (persistent authority relationships across tasks) or the architecture-to-institution transition. They are delegation-level tools; this framework adds the governance-level analysis.

**Organizational theory** (Coase, Hayek, Ostrom, North, Mintzberg, Barnard) provides the intellectual foundations. Coase's transaction-cost logic predicts when hierarchies form. Hayek's knowledge argument explains when markets outperform central planning. Ostrom's design principles describe commons governance. North's institutional framework distinguishes rules from organizations. Barnard's Zone of Indifference (1938) offers a lens on agent compliance thresholds. Mintzberg's structural configurations map loosely onto our governance families. This framework applies their insights to AI agent systems specifically, where the substrate differs (agents can share full context, reorganize in hours, and be redesigned) but the coordination problems are structurally similar.

**The Adaptive Fabric** (Part 1 of this series, informed by <a href="https://www.adaptionlabs.ai/" target="_blank" rel="noopener" class="red-link">Adaption Labs</a> and <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" rel="noopener" class="red-link">Hooker 2025</a>) describes how the delegation and governance structures in this post *change over time*. Five adaptation surfaces (data, model, environment, coordination, interface) interact continuously. The Adaptive Delegation meta-pattern is the delegation-level mechanism; governance transitions are the society-level mechanism. Together they produce a fabric that restructures under pressure rather than breaking.
</details>

The discomfort in that is specific. Nobody on that desk's team did anything wrong, and it still ended up governed by a metric nobody chose, which changes what there is to check. Nothing its agents said to her was false, so auditing what an individual model says is looking at the wrong object. What matters is the society: how it delegates, how well it resists drift, and whose interests its arrangement of power quietly serves. Making one agent behave well is a different problem from making a group behave well. An agent cannot collude with itself, and no single model drifts the way a scoreboard does.

## What Is Still Unsolved

The hardest part of this is not technical. Suppose she pushes, somebody agrees to look, and the question is which agent owes her an explanation.

The one that scored her has been retrained twice since, on data that now includes her case. There are four copies of it running, one of them rolled back to a version from before the risk score existed. Each copy carries her record and none carries the reasoning that produced it. Ask which one decided and there is no answer that survives contact with how these systems actually work, because the thing that decided was a configuration that existed for six weeks and no longer exists anywhere.

You can find a person years later. You can sue a company a decade on. The thing that decided her case was a configuration that existed for six weeks, and the question is not who signed in but which of the four copies is continuous with it. None of them is.

<details style="margin: 1.2em 0; padding: 0.6em 1em; font-size: 0.95em; background: #f8f8f8; border-left: 3px solid #999; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; font-weight: 600;">Two harder problems: emergency powers, and the judgement calls nobody can engineer away</summary>

Harder still is what happens when a system rewrites its own rules under pressure, which human institutions do constantly: democracies reach for emergency powers, markets stop trading. Deciding what should trigger that is difficult enough. Making sure the emergency mode ever ends is the part nobody has solved, in agents or anywhere else.

And some of it cannot be engineered at all, because it needs somebody to make a call. Should an agent's record follow it into work with different conditions? How would anyone supervise a desk of a thousand agents, when reading their decisions is arithmetically impossible and summaries might preserve real oversight or only the appearance of it? Underneath both sits the question this post has dodged twice: the desk's agents cannot agree to be governed, so what would make authority over them legitimate, and is that even a coherent thing to ask?

</details>

---

Which brings this back to the support desk, which by now has been an autocracy, a leaderboard, and a constitution, and will be a tribunal the first time somebody demands to know why their complaint was ignored. Nothing in that story required malice, or even carelessness. It required only that somebody log performance and act on it, which is what any competent engineer would do. That is the uncomfortable part: these structures do not arrive because people are careless about governance. They arrive because people are conscientious about everything else. Designing the institution is the work.

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

