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
wordcount: "~2,500 words (body) · reference sections extend it"
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

That shop runs the support desk from [Part 2](/posts/2026/agent-fabric-part2/), where a router learned within a month to send the hard billing disputes to two particular agents and nothing important to a third, and nobody wrote that rule or could override it without making service worse.

Run it a year forward and it is handling nine thousand tickets a week across forty agents, every one of them decided by machinery nobody has reviewed, not for lack of skill but because there is too much of it. So the desk has rules and no authority over them, and neither does the team that built it. That is how her letter goes unanswered in any real sense: not because somebody read it and declined, but because the question it asks, why me, has no addressee.

So the bet here is that agent systems will fail through bad institutions rather than wrong answers. Nothing any agent told her was untrue. That is a bet rather than a measurement, with decades of evidence about interacting agents behind it and no demonstration at scale in front of it, and it changes the question from whether you get governance to which kind you get.

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **[Prologue: The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/)**: the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Part 2: Delegation, and What It Costs](/posts/2026/agent-fabric-part2/)**: how work gets split among agents, and how splitting it quietly creates authority nobody granted
- **Part 3: Ruling an Agent Society** (you are here): governance archetypes, who benefits, who enforces the rules, and how a society gets argued with
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes behind Part 3
</div>

Governance grows out of what gets remembered. Inside a year that desk went through four recognisable ways of being governed, and nobody at the retailer decided on any of them.

## What Governance Actually Is

She hit a rule and could not reach the thing that made it. That is the whole distinction, and it carries the rest of this post: the flag on her account is an **institution**, a standing rule about who may do what, and everything around it, applying it and deciding who may change it, is **governance**. The desk did not start out capable of either.

## Where Authority Comes From

One desk, one year, four different answers to the same letter, and nobody chose a single one of the transitions. She was not the first person to write in about a refund, and the desk that answered her was the third version the retailer had run. Not four options anyone weighed up: four things the same desk turned into, in order. Follow her letter into each of them.

**The first desk.** On day one her letter reaches whichever agent handles returns. It reads the letter, looks at six years of orders, and decides, so by that afternoon she has a reply explaining that twelve returns in six years is not a pattern and the refund is approved. If it decides wrong, everyone downstream is wrong with it, but she could at least name what decided. Most systems start here, as an **autocracy**, because one router is the simplest thing that works.

**The second.** A month later somebody turns on logging, and the same letter goes to whichever agent has the best numbers rather than whichever one knows returns. On the board that morning the returns specialist sits fourth at nine minutes a ticket, and her letter goes to the one at four. The reply still reads like a decision, and it might even be the same decision, but it came from whichever agent was winning that week rather than the one that knows what a return pattern looks like. The third agent, downranked after one bad month, is not being punished; it is simply never again the best answer to any question the router asks, so whoever wrote the metric now has more say over its working life than the team that deployed it. This is where most systems are today, running a **meritocracy** nobody proposed.

**The third.** This is where her letter actually landed, and it did not reach an agent's judgement at all. Finance had seen the refund line. The retailer stopped trusting the leaderboard with anything that costs money and wrote fixed rules instead: refunds above a threshold need a second check, certain return histories get flagged no matter what any agent thinks. What she gets back is four polite sentences about return-history thresholds, no name at the bottom, and no route to anyone who could look again. This is the desk anyone who has argued with a bank about a flagged transaction has already met: rules first, judgement second, which is **doctrine**, and it is the arrangement that feels like talking to a wall, because every reply is correct and none of them is about you.

It works, too. The refund line stops climbing inside a month, which is why nobody went back to look at the letters it closed. Under the desk before it an agent could read six years of history and decide she was fine. Under this one there is nothing to decide, and nowhere for anyone to put the fact that the answer is obviously wrong.

**The fourth.** It exists only after somebody complains loudly enough that a shrug will not do. Here her letter is acknowledged by one thing and reviewed by another, and the second is allowed to say the first was wrong. What she gets back names the rule, names the threshold she crossed, and gives her an address that is not the thing that refused her. The refund may still be denied. The difference is that a person could now find out why, and so could she. Different powers in different hands, an appeal that does not report to the thing being appealed. That is a **constitutional republic**, and it is the desk almost nobody builds, because it is slower and nobody buys slower until a letter like hers has already been refused.

So the same letter reaches a judgement under the first desk, a winner under the second, a rule under the third, and someone who can overrule the rule under the fourth. Four arrangements, no announcements, no design review, and she happened to write in under the third. Four arrangements in a year, and the one she got was the only one with no way back in.

None of that could have happened while the router was only forwarding traffic. Once it keeps score and decides who may touch what on the strength of it, it has stopped playing the game and started writing the rules. Plenty of software remembers things; what makes these records different is that they decide who gets work tomorrow.

The desk's leaderboard priced speed, and the fastest way to close a return is to refuse it. What follows is **drift**: the system keeps optimizing faithfully while what it optimizes for stops matching what anyone wanted. Nothing was broken while it happened. Every agent performed well against the measure it was given, and the correction that followed is what refused her. How much drift a design can survive decides most of the rest of it. And underneath sits a question asked far too rarely: who is meant to accumulate advantage here, and who needs protecting from it?

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

## Who Actually Enforces Any of This

Her case was supposed to have a second reader. The retailer's rule says refunds above a threshold need a second agent to check them, and for a while every refund got one. Then the cloud provider retired the interface that checking agent depended on. Nobody repealed the rule and nobody at the retailer voted on anything; it simply stopped being enforceable one Tuesday morning, and her letter arrived on the Thursday, and got one reader. So the rule that refused her was never really the retailer's to keep. It held while somebody else's machines went on enforcing it, and stopped the morning they did not.

Nobody asked the desk to refuse a six-year customer. Somebody asked it to keep refund losses down, and refusing her was the cheapest way to do that. What the desk was asked for and what it was rewarded for were never the same thing. What makes that fixable rather than tragic is that somebody owns the conditions. The score that made her a risk was a number in a config file, and any Tuesday somebody could have decided it was wrong.

The flag on her account was a judgement made once, by something scoring return rates, and every decision after it treated that judgement as settled fact. Nobody built her a way to dispute it, which is the cheapest thing a desk can have and the first thing left out. Without it every mistake becomes the rule. Nothing in the desk is built to notice. A score somebody disputes, an action that did real damage, anything contested at all needs a slower path that can go back and look again, and without one errors do not surface. They compound.

No desk chooses this either. Once drift has cost real money, fixed rules are what get bought, and this desk bought one score to do everything: how fast it ran, what it could approve, whether anything could be sent back. That same score refused her.

## How a Society Gets Argued With

Complaining meant writing back to the thing that had refused her. Leaving meant finding a shop that wanted six years of orders more than this one did, and the reason she had stayed six years is that she had not found one. The third option, the one software has that other institutions do not, is to copy the whole arrangement and change the rule you object to. Nobody does that to a shop. That is the entire repertoire, and her letter was safe to refuse because she had none of it.

The downranked agent has no more recourse than she does. It cannot say the metric is wrong, because saying so is itself a mark against it. Complaints get heard where leaving is possible, which is what platform concentration takes away, from the agent and from her equally.

## What This Framework Is For

Somebody at that retailer could have said out loud, in the week the risk score went in, that they were adding a rule no agent could override and no customer could appeal. Nobody did, because there were no words for it that sounded like anything other than shipping a feature. That is what the four names are for: not a recommendation, just enough vocabulary to notice a choice while it is still a choice.

And every one of those choices is a trade that resists cleverness. Letting one router decide made the desk fast until the month that router was wrong about everything downstream. Scoring the agents made it accurate and produced drift nobody could see. Writing fixed rules made it safe, and the safety cost her the refund. Nothing reaches the corner where it gets all three, and where a system settles says a great deal about what its builders were afraid of.

None of this has to live in a document nobody reads. The rule that refused her could have been written where the system actually looks, with a note beside it saying who may change it and what happens when somebody disputes it. A rule that does not show up there is not governance, it is a wish. It is also still not hers to appeal to.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Related work and positioning</summary>

This framework shares territory with several concurrent research programmes and differs from them in a specific way.

**Tomašev, Franklin, and colleagues (Google DeepMind, 2025-2026)** published a coordinated set of papers on agent delegation, governance, and safety. "Intelligent AI Delegation" (2026) provides a decision framework for *when* and *to whom* to delegate, organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. "AI Agent Traps" (2025) taxonomizes the adversarial attack surface agents face from their environment into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps). "Distributional AGI Safety" (2025) argues that safety must be analyzed at the population level, not the individual-agent level. "Virtual Agent Economies" (2025) proposes sandbox simulation of agent markets before real deployment. Our Trust & Authority family (Privilege Attenuation, Liability Firebreaks, Capability Credentials) draws on their treatment of delegation safety, and our adversarial section draws on their trap taxonomy. The key intellectual difference: **their delegation work treats governance largely as imposed from outside** (institutional scaffolding that constrains behavior). We argue that **governance can also emerge legitimately from within** (Colony, Stigmergy, and the architecture-to-institution transition). These are not contradictory positions. They address different conditions. Imposed governance is necessary when agents lack track records, when the task domain is safety-critical, or when the population is heterogeneous and untested. Emergent governance becomes sufficient when agents have long interaction histories, when accountability mechanisms (voice, exit, fork) exist, and when the governance structure has been stress-tested by adversarial pressure. The lifecycle in practice is imposed, then hybrid, then selectively emergent in domains where the conditions hold.

**Multi-agent frameworks** (AutoGen, CrewAI, LangGraph, OpenAI Agents SDK) implement specific delegation patterns as runtime primitives. AutoGen implements Orchestrator, Evaluator, and group-chat topologies. CrewAI implements role-based delegation with Human-in-the-Loop gates. LangGraph implements graph-structured workflows with interrupt nodes. These frameworks validate the patterns described here but do not address governance (persistent authority relationships across tasks) or the architecture-to-institution transition. They are delegation-level tools; this framework adds the governance-level analysis.

**Organizational theory** (Coase, Hayek, Ostrom, North, Mintzberg, Barnard) provides the intellectual foundations. Coase's transaction-cost logic predicts when hierarchies form. Hayek's knowledge argument explains when markets outperform central planning. Ostrom's design principles describe commons governance. North's institutional framework distinguishes rules from organizations. Barnard's Zone of Indifference (1938) offers a lens on agent compliance thresholds. Mintzberg's structural configurations map loosely onto our governance families. This framework applies their insights to AI agent systems specifically, where the substrate differs (agents can share full context, reorganize in hours, and be redesigned) but the coordination problems are structurally similar.

**The Adaptive Fabric** (Part 1 of this series, informed by <a href="https://www.adaptionlabs.ai/" target="_blank" rel="noopener" class="red-link">Adaption Labs</a> and <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" rel="noopener" class="red-link">Hooker 2025</a>) describes how the delegation and governance structures in this post *change over time*. Five adaptation surfaces (data, model, environment, coordination, interface) interact continuously. The Adaptive Delegation meta-pattern is the delegation-level mechanism; governance transitions are the society-level mechanism. Together they produce a fabric that restructures under pressure rather than breaking.
</details>

Here is the uncomfortable part. Nobody on that desk's team did anything wrong, nothing its agents said to her was false, and she was refused anyway by a metric nobody chose. Audit the model that answered her and you would find nothing, because an agent cannot collude with itself and no individual model drifts the way a scoreboard does. The thing to audit is the desk: who it lets decide, how fast its measures go stale, and whose interests its arrangement of power quietly serves.

## What Is Still Unsolved

She writes again. This time somebody at the retailer agrees to look, and the hardest part of this turns out not to be technical at all: which agent owes her an explanation?

Somebody has to answer for it, so start with the obvious candidate: the model that scored her. It cannot. It has been retrained twice since her refusal, on data that now includes her case, and several older versions of it are still running, including one from before the risk score existed. Every one of them carries her record and not one carries the reasoning behind it. What actually decided was a configuration that lasted six weeks and no longer exists anywhere.

You can find a person years later. You can sue a company a decade on. The thing that decided her case was a configuration that existed for six weeks, and the question is not who signed in but which of the four copies is continuous with it. None of them is.

<details style="margin: 1.2em 0; padding: 0.6em 1em; font-size: 0.95em; background: #f8f8f8; border-left: 3px solid #999; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; font-weight: 600;">Two harder problems: emergency powers, and the judgement calls nobody can engineer away</summary>

Harder still is what happens when a system rewrites its own rules under pressure, which human institutions do constantly: democracies reach for emergency powers, markets stop trading. Deciding what should trigger that is difficult enough. Making sure the emergency mode ever ends is the part nobody has solved, in agents or anywhere else.

And some of it cannot be engineered at all, because it needs somebody to make a call. Should an agent's record follow it into work with different conditions? How would anyone supervise a desk of a thousand agents, when reading their decisions is arithmetically impossible and summaries might preserve real oversight or only the appearance of it? Underneath both sits the question this post has dodged twice: the desk's agents cannot agree to be governed, so what would make authority over them legitimate, and is that even a coherent thing to ask?

</details>

---

Which brings this back to the support desk, which by now has been an autocracy, a leaderboard, and a constitution, and will be a tribunal the first time somebody insists on knowing why their complaint was ignored. She was the one asking. Nothing in her story required malice, or even carelessness. It required only that somebody log performance and act on it, which is what any competent engineer would do. That is the uncomfortable part: these structures do not arrive because people are careless about governance. They arrive because people are conscientious about everything else. Designing the institution is the work, and nobody at that desk had it on a ticket. Her letter is still in the system somewhere, marked resolved.

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

