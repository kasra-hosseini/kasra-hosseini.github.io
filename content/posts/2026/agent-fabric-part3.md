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

That shop runs the support desk from [Part 2](/posts/2026/agent-fabric-part2/), where a router learned within a month to send the hard billing disputes to two particular agents and nothing important to a third. Nobody wrote that rule, nobody could override it without making service worse, and by the time she wrote in it had been running for a year.

That desk changed how it governed itself four times inside a year, and nobody decided on any of them, because governance grows out of what gets remembered rather than what gets chosen.

By the end of that year it is handling nine thousand tickets a week across forty agents, every one of them decided by machinery nobody has reviewed, not for lack of skill but because there is too much of it. So the desk has rules that nobody at the desk can change, and neither can the team that built it. That is how her letter goes unanswered in any real sense: not because somebody read it and declined, but because the question it asks, why me, has no addressee.

Nothing any agent told her was untrue. That is the whole point: the failure was not in any answer, it was in the arrangement that produced them, which is where these systems will fail long before they start lying. Which means the question was never whether this desk would end up with rules. It was which rules, decided by whom.

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **[Prologue: The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/)**: the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Part 2: Delegation, and What It Costs](/posts/2026/agent-fabric-part2/)**: how work gets split among agents, and how splitting it quietly creates authority nobody granted
- **Part 3: Ruling an Agent Society** (you are here): governance archetypes, who benefits, who enforces the rules, and how a society gets argued with
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes behind Part 3
</div>

## Four Desks in One Year

She hit a rule and could not reach whoever decides what the rules are, and the desk had not started out capable of either. She was also not the first person to write in about a refund: the desk that answered her was the third version the retailer had run. One desk, one year, four different answers to the same letter. Not four options anyone weighed up: four things the desk turned into, in order. Watch what happens to her letter under each one.

Her refusal started long before she wrote in. Somebody set the leaderboard to reward speed, every agent could see its own position on it, and the fastest way to close a return is to refuse it. Nobody had to be told twice, and what the desk was optimizing for **drifted** away from what the desk was for.

**Day one.** Her letter reaches whichever agent handles returns. It reads the letter, looks at six years of orders, and decides, so by that afternoon she has a reply explaining that twelve returns in six years is not a pattern and the refund is approved. If it decides wrong, everyone downstream is wrong with it, but she could at least name what decided. Most systems start here, because one router is the simplest thing that works, and the price is that when it is wrong there is nothing else in the room.

**A month in.** Somebody turns on logging, and the same letter goes to whichever agent has the best numbers rather than whichever one knows returns. On the board that morning the returns specialist sits fourth at nine minutes a ticket, and her letter goes to the one at four. The reply still reads like a decision, and it might even be the same decision, but it came from whichever agent was winning that week rather than the one that knows what a return pattern looks like. 

The third agent, downranked after one bad month, is not being punished. It is simply never again the best answer to any question the router asks, which is harsher than punishment because there is no term to it and nobody to appeal to. It is the agent that would have read her letter properly, and by the time she writes in it has not been asked a hard question in months. Whoever wrote the metric now has more say over its working life than the team that deployed it. This is where most systems are today, and the price is that the numbers decide who is competent before anyone has checked whether the numbers measure competence.

**Half a year in**, which is where her letter actually landed, and it did not reach an agent's judgement at all. Finance had seen the refund line. The retailer stopped trusting the leaderboard with anything that costs money and wrote fixed rules instead: refunds above a threshold need a second check, certain return histories get flagged no matter what any agent thinks. What she gets back is four polite sentences about return-history thresholds, no name at the bottom, and no route to anyone who could look again. This is the desk anyone who has argued with a bank about a flagged transaction has already met: rules first, judgement second, rule over judgement, which from her side feels like talking to a wall: every reply correct, none of them about her.

It works, too. The refund line stops climbing inside a month, which is why nobody went back to look at the letters it closed. Under the desk before it an agent could read six years of history and decide she was fine. Under this one there is nothing to decide, and nowhere for anyone to put the fact that the answer is obviously wrong.

**The desk she needed** exists only after somebody complains loudly enough that a shrug will not do. Here her letter is acknowledged by one thing and reviewed by another, and the second is allowed to say the first was wrong. What she gets back names the rule, names the threshold she crossed, and gives her an address that is not the thing that refused her. The refund may still be denied. The difference is that a person could now find out why, and so could she. Whatever hears the appeal does not report to the thing being appealed, which is the whole trick, and it is slower, which is why it usually arrives only after a letter like hers has already gone out.

So the same letter reaches a judgement, then a winner, then a rule, then someone who can overrule the rule. Four arrangements in a year, no announcements and no design review, and she happened to write in under the only one with no way back in.

Every one of those desks needed a record to exist at all. Plenty of software remembers things; this record decided who would read her letter.

Nothing was broken while that happened. Every agent performed well against the measure it was given, and when finance noticed the refund line and bolted on a risk score, that correction was reasonable too. She is the one who paid for both. The question nobody asks early enough is who an arrangement is meant to favour, and who needs protecting from it.

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

Her case was supposed to have a second reader. Refunds above her amount need a second agent to check them, and for a while every one got one, using a service the retailer paid another company to provide.

That company switched the service off on a Tuesday. Nobody voted to drop the second check, nobody repealed the rule, and nobody noticed it had stopped applying. Her letter arrived on the Thursday and got one reader.

The rule that would have caught her case was never really the retailer's to keep. It held for as long as somebody else's machines went on enforcing it.

Nobody asked the desk to refuse a six-year customer. Somebody asked it to keep refund losses down, and refusing her was the cheapest way to do that. The flag on her account was one judgement, made once, by something scoring return rates, and every decision after it treated that judgement as settled fact. It was a number in a config file. On any Tuesday somebody could have opened that file and decided it was wrong, and nobody did, because nobody was looking and there was nowhere for her to ask.

## How a Society Gets Argued With

She does try. Complaining means replying to the email that refused her, so she replies, and the reply invites her to reply. Whatever read her second letter was the thing that wrote the first one, and it had already decided what her return history meant. There is a word for an appeal heard by the party you are appealing against, and it is not appeal.

Leaving is what everyone assumes she can do instead, so she looks. The next shop wants an account before it will show her anything, and the form asks for the email address she has used for six years.

There is a third exit that works for software and not for shops: run your own copy with the rule changed. She does not want her own retailer, she wants forty pounds back from this one.

The downranked agent has no more recourse than she does. It cannot say the metric is wrong, because saying so is itself a mark against it, and there is nowhere else for it to work. Complaints get heard where leaving is possible, which is the part nobody designs for, because it is the part nobody notices they removed.

The rule that refused her did not have to live nowhere. It could have sat in the same file the system reads, with two lines under it: who may change this, and what happens when somebody says it is wrong. Somebody could have said out loud, in the week the risk score went in, that they were adding a rule no agent could override and no customer could appeal. Nobody did, because they were adding a risk score, and nobody objects to a risk score. Her letter is what that missing sentence cost.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Related work and positioning</summary>

This framework shares territory with several concurrent research programmes and differs from them in a specific way.

**Tomašev, Franklin, and colleagues (Google DeepMind, 2025-2026)** published a coordinated set of papers on agent delegation, governance, and safety. "Intelligent AI Delegation" (2026) provides a decision framework for *when* and *to whom* to delegate, organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. "AI Agent Traps" (2025) taxonomizes the adversarial attack surface agents face from their environment into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps). "Distributional AGI Safety" (2025) argues that safety must be analyzed at the population level, not the individual-agent level. "Virtual Agent Economies" (2025) proposes sandbox simulation of agent markets before real deployment. Our Trust & Authority family (Privilege Attenuation, Liability Firebreaks, Capability Credentials) draws on their treatment of delegation safety, and our adversarial section draws on their trap taxonomy. The key intellectual difference: **their delegation work treats governance largely as imposed from outside** (institutional scaffolding that constrains behavior). We argue that **governance can also emerge legitimately from within** (Colony, Stigmergy, and the architecture-to-institution transition). These are not contradictory positions. They address different conditions. Imposed governance is necessary when agents lack track records, when the task domain is safety-critical, or when the population is heterogeneous and untested. Emergent governance becomes sufficient when agents have long interaction histories, when accountability mechanisms (voice, exit, fork) exist, and when the governance structure has been stress-tested by adversarial pressure. The lifecycle in practice is imposed, then hybrid, then selectively emergent in domains where the conditions hold.

**Multi-agent frameworks** (AutoGen, CrewAI, LangGraph, OpenAI Agents SDK) implement specific delegation patterns as runtime primitives. AutoGen implements Orchestrator, Evaluator, and group-chat topologies. CrewAI implements role-based delegation with Human-in-the-Loop gates. LangGraph implements graph-structured workflows with interrupt nodes. These frameworks validate the patterns described here but do not address governance (persistent authority relationships across tasks) or the architecture-to-institution transition. They are delegation-level tools; this framework adds the governance-level analysis.

**Organizational theory** (Coase, Hayek, Ostrom, North, Mintzberg, Barnard) provides the intellectual foundations. Coase's transaction-cost logic predicts when hierarchies form. Hayek's knowledge argument explains when markets outperform central planning. Ostrom's design principles describe commons governance. North's institutional framework distinguishes rules from organizations. Barnard's Zone of Indifference (1938) offers a lens on agent compliance thresholds. Mintzberg's structural configurations map loosely onto our governance families. This framework applies their insights to AI agent systems specifically, where the substrate differs (agents can share full context, reorganize in hours, and be redesigned) but the coordination problems are structurally similar.

**The Adaptive Fabric** (Part 1 of this series, informed by <a href="https://www.adaptionlabs.ai/" target="_blank" rel="noopener" class="red-link">Adaption Labs</a> and <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" rel="noopener" class="red-link">Hooker 2025</a>) describes how the delegation and governance structures in this post *change over time*. Five adaptation surfaces (data, model, environment, coordination, interface) interact continuously. The Adaptive Delegation meta-pattern is the delegation-level mechanism; governance transitions are the society-level mechanism. Together they produce a fabric that restructures under pressure rather than breaking.
</details>

## What Is Still Unsolved

She writes again. This time somebody at the retailer agrees to look, and the hardest part of this turns out not to be technical at all: which agent owes her an explanation?

Somebody owes her an explanation. The obvious place to look is the model that scored her, and it cannot give her one, because it is not the same model any more. It has been retrained twice since her refusal, on data that now includes her case, and several older versions of it are still running, including one from before the risk score existed. Every one of them carries her record. Not one carries the reason. What actually decided was a configuration that lasted six weeks and no longer exists anywhere.

A person can be found years later. A company can be sued a decade on. The thing that refused her existed for six weeks and is gone: not deleted, not overturned, just superseded by the next version of the desk.

So there is nothing left that made her decision. Not a missing record: a missing thing. That is why nobody can unmake it, and why an apology would have nowhere to come from even if somebody wanted to give her one.

<details style="margin: 1.2em 0; padding: 0.6em 1em; font-size: 0.95em; background: #f8f8f8; border-left: 3px solid #999; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; font-weight: 600;">Two harder problems: emergency powers, and the judgement calls nobody can engineer away</summary>

Harder still is what happens when a system rewrites its own rules under pressure, which human institutions do constantly: democracies reach for emergency powers, markets stop trading. Deciding what should trigger that is difficult enough. Making sure the emergency mode ever ends is the part nobody has solved, in agents or anywhere else.

And some of it cannot be engineered at all, because it needs somebody to make a call. Should an agent's record follow it into work with different conditions? How would anyone supervise a desk of a thousand agents, when reading their decisions is arithmetically impossible and summaries might preserve real oversight or only the appearance of it? Underneath both sits the question this post has dodged twice: the desk's agents cannot agree to be governed, so what would make authority over them legitimate, and is that even a coherent thing to ask?

</details>

---

That support desk has now been run by one agent, by a scoreboard, and by a rule, and it will be run by an appeal the first time somebody insists on knowing why their complaint was ignored. She was the one asking. Nothing in her story required malice, or even carelessness. It required only that somebody log performance and act on it, which is what any competent engineer would do. That is the uncomfortable part: these structures do not arrive because people are careless about governance. They arrive because people are conscientious about everything else. Designing the institution is the work, and nobody at that desk had it on a ticket. Her letter is still in the system somewhere, marked resolved.

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

