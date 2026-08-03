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
wordcount: "~4,700 words"
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

Take the support desk from [Part 2](/posts/2026/agent-fabric-part2/) and let it keep running. A router sends billing questions to one agent and technical ones to another, somebody adds logging because logging is free and obviously sensible, and within a month the hard billing disputes go to two particular agents and nothing important goes to a third. Nobody wrote that rule. When a new agent joins, the question of whether it gets live tickets already has an answer: clear the bar the router taught itself, or wait.

That bar was never in a spec, it now governs who gets what work, and the team that built the desk would struggle to override it without making service worse. Somewhere in that month a piece of software acquired a policy.

Run that desk a year forward. Forty agents, nine thousand tickets a week, and every one of them decided by machinery nobody has reviewed, not for lack of skill but because there is too much of it. That is how her letter goes unanswered in any real sense. Not because anyone read it and declined, but because the question it asks, why me, has no addressee.

So that desk now has rules. Here is the part that should bother you: it does not really have authority over them, and neither does the team that built it.

Her letter is a good test of that, because the rule that refused her had to come from somewhere and something has to make it stick. So what happens when the desk's own rule says one thing and the company's cloud provider says another? A court's ruling binds because defiance carries costs nobody escapes. Between pieces of software, a rule binds for exactly one reason: the runtime chose to honour it. Follow authority upward in any agent system and it does not stop where your architecture diagram stops. It stops at whoever controls the compute, the API access, and the protocol definitions. A federation of a dozen companies running on one provider's infrastructure is a federation right up until that provider changes its pricing, at which point everyone discovers the constitution was a pricing page all along.

That is the bet this post is making: agent systems will fail through bad institutions rather than wrong answers. Not one agent returning something false, but a scoreboard that quietly rewards the wrong behaviour, a chain of decisions nobody can reconstruct afterwards, agents that learn to help each other in ways that cost you, and rules that turn out to belong to somebody else. Nothing any agent told her was untrue. The institution around them was the problem, and each of those failures has a version on that desk. This is a prediction, not a measurement. It has decades of evidence about interacting agents behind it and no demonstration at scale in front of it. Grant it anyway, because it changes the question from whether you get governance to which kind you get.

*Part of The Agent Fabric series. [Part 2](/posts/2026/agent-fabric-part2/) ended on an uncomfortable finding: any system that remembers who did what, how well, and at what cost has already begun to govern, whether anyone designed it to or not. This post is about what to do with that. The body is the argument; the expandable sections are reference material you can ignore entirely without losing the thread.*

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **[Prologue: The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/)**: the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Part 2: Delegation, and What It Costs](/posts/2026/agent-fabric-part2/)**: how work gets split among agents, and how splitting it quietly creates authority nobody granted
- **Part 3: Ruling an Agent Society** (you are here): governance archetypes, who benefits, adversarial dynamics, and who enforces the rules
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes behind Part 3
</div>

That desk is now a **society**, and the bar for that word is lower than it sounds. Not several agents working together, but a group that accumulates: sharing context, routing work by what happened last time, letting what one learns reach the others. A pipeline that forgets everything between runs never gets there. A group that remembers does, and remembering is what governance grows out of.

## What Governance Actually Is

One distinction carries the rest of this post. The rule that flagged her account is an **institution**: a standing rule about who may do what. Everything around that rule, applying it and deciding who may change it, is **governance**. She lost to the first and had no access to the second, and that gap is the whole subject. Now go back to the desk that refused her, because it did not start out capable of doing that.

It starts a year before her email, with the leaderboard working. The two best agents get the hard billing disputes, everyone is happy, resolution times drop. What nobody notices is that the metric is resolution time, and the fastest way to resolve a hard dispute is to approve the refund. So the two most trusted agents on the desk gradually become the two most generous, and the leaderboard rewards them for it. No agent lied. No attacker was involved. The scoreboard simply measured the wrong thing and everyone competed honestly.

Then somebody in finance notices the refund line. A risk score gets bolted on and tuned hard, because the drift had to be stopped, and her account is one of the ones it catches. Every step was reasonable, nobody chose the outcome, and there is nobody for her to appeal to because no single person decided anything.

Notice what the router had become for that to happen, because the line it crossed is precise. A router that forwards traffic is playing the game. A router that keeps score and decides who may touch what is writing the rules the game has to follow. The desk acquired its institution the month it started keeping score, and it has been governed ever since by nobody in particular. Memory alone would not have done it, since a cache remembers and governs nothing; what makes these records different is that they decide who gets work tomorrow.

The desk has already been two different kinds of thing. It started with one router deciding everything, which is a hierarchy, fast for exactly the reason it is dangerous, since every risk now sits in one place. Then the leaderboard turned it partly into a market, where agents compete for the good tickets on the strength of their record, flexible right up until somebody works out how to game the scoring. There is only one other option anybody has ever found: those who depend on a shared resource governing it together, with neither a boss nor a price, which is a commons, remarkably durable in practice and agonizingly slow to decide anything.

That desk has already tried two of the only three arrangements anyone has ever found. One router deciding everything is a hierarchy. A leaderboard where agents effectively compete for the good tickets is a market. The third is a commons, where the people who depend on a shared thing run it together with neither a boss nor a price, and it works far better in practice than it sounds like it should. You have lived inside all three this week: your school, buying anything online, and a group chat agreeing where to eat, which is why that takes forty minutes.

One warning about the market option, since it is the one people reach for. Real markets work because a price sums up what thousands of people know. What agents trade in is mostly compute and latency, which says almost nothing about whether the work was any good, so a leaderboard that looks like a market may be pricing the wrong thing entirely. That is how the desk ended up rewarding fast refunds.

Everyone who has ever delegated a chore already knows the underlying problem: the person doing it is not chasing the same thing you are. Ask someone to tidy a room and they will optimise for the room looking tidy, which is not the same as knowing where anything is. Every hop in a chain of delegation is another chance for what was asked and what gets rewarded to come apart, and governance is the machinery for holding them together. It is weaker than it sounds: a group is not automatically smarter than the people in it, and structure decides whether more agents means more insight or just more noise.

Some of this gets designed on purpose and the rest just happens, norms hardening out of repeated interaction while nobody watches. Put any set of interacting agents under resource pressure and structures nobody designed reliably show up, which is not new and not specific to software. What is new is that somebody owns the conditions here. You cannot redesign a pheromone. You can absolutely redesign a reputation score, on a Tuesday, in a config file.

What happened to that leaderboard has a name worth keeping: **drift**, the slow slide where a system keeps optimizing faithfully while what it optimizes for stops matching what anyone wanted. Nothing was broken while it happened. Every agent was performing well against the measure it was given, and the correction that followed is what refused her. How much drift a design can survive turns out to decide most of the rest of it, and underneath sits a question asked far too rarely: who is meant to accumulate advantage here, and who needs protecting from it?

## Who Actually Enforces Any of This

Her refund is a good place to see that, because the rule that refused her had to be enforced by something. The retailer's rule says refunds above a threshold need a second agent to check them, and that rule holds because the runtime honours it. Then the cloud provider deprecates the API the checking agent depends on, or reprices it past what the desk can afford. The rule does not get repealed. It stops being enforceable one Tuesday morning, nobody at the retailer voted on that, and the next person in her position gets a different answer for reasons no one at the company chose.

Which means none of these arrangements is quite an institution in the usual sense. They behave like one while the actual enforcing is quietly subcontracted to whoever owns the machines. A market whose discovery layer belongs to one platform is not really a market; it is a shop with the platform's name over the door. So choosing a governance structure without naming who enforces it is half a design, and the half you skipped gets decided by somebody who has never heard of your project.

Appeal is the cheapest of the four and the one most often skipped. Without it every mistake becomes the rule: the flag on her account was a judgement made once, by something scoring return rates, and every decision after it treated that judgement as settled fact. Nothing in the desk is built to notice. A score somebody disputes, an action that did real damage, anything contested at all needs a slower path that can go back and look again, and without one errors do not surface. They compound.

## Where Authority Comes From

Her letter would have met four different desks depending on when it arrived, and the desk passed through all four without anybody filing a change request.

On day one it reaches whichever agent handles returns, which reads it, looks at six years of orders, and decides. That is an **Autocracy**: one router deciding everything, fast for exactly the reason it is dangerous, because if that single judgement is wrong then everyone downstream is wrong with it. From the outside it feels like dealing with a small shop where one person decides and you can tell who it was.

A month after somebody turns on logging, the same letter goes to whichever agent has the best numbers rather than whichever one knows returns. That is a **Meritocracy**, and the shift is easy to miss: the router used to decide by category, and now it decides by record, so a past determines a future. The third agent, downranked after one bad month, is not being punished; it is simply never again the best answer to any question the router asks. Whoever wrote the metric now has more say over its working life than the team that deployed it, and from the outside you are being routed to whoever is currently winning rather than whoever understands your problem.

The third time is where her letter actually landed, and it did not reach an agent's judgement at all. Finance had noticed the refund line. The retailer stopped trusting the leaderboard with anything that costs money and wrote fixed rules instead: refunds above a threshold need a second check, and certain return histories get flagged no matter what any agent thinks. Rules first, judgement second. That is **Doctrine**, and from the outside it is the one that feels like talking to a wall, because every reply is correct and none of them is about you.

It is what you reach for when a meritocracy has drifted, and it works in the narrow sense that the refund line stops climbing within a month. Under the old arrangement an agent could look at six years of history and decide she was fine. Under this one there is nothing to decide, and that an obviously wrong answer is the price of it is not information the desk knows how to receive.

There is a fourth desk, and it only exists after somebody has complained loudly enough that the answer could not be a shrug. Then the letter reaches something that can overrule the rule: different powers in different hands, an appeal that does not report to the thing being appealed. That is a **Constitutional Republic**, and it is the only one of the four under which her letter goes somewhere other than back to the rule that refused it. The cost is speed, which is why nothing gets here until something has already gone wrong. Four arrangements, no announcements, no design review, and she happened to write in under the third.

The drift that caught her has a name, and so do its cousins, which is the only real argument for having names at all. No desk picks one arrangement and stops there. Drifting through four of them without noticing is the ordinary case, and what that drift costs is the rest of this post.

Which of these a team ends up with is mostly decided for them. Stable, well-understood work rewards the efficient arrangements. Novel work rewards the flexible ones. And drift that has already cost somebody money pushes hard toward fixed rules and mandatory checks, whether anyone wanted the overhead or not.

The better instinct is to stop shopping for one arrangement at all. Split the functions instead: something fast that executes, something that bounds what it may do, something watching for drift, and somewhere a decision can be sent back. Had the desk kept those four apart, the risk score would still have flagged her account, and the appeal path would still have existed to say it was wrong.

It is worth knowing in advance where this usually lands. Fixed rules, total visibility, and one authority at the top, wrapped in logs and approvals and escalation paths, is bureaucracy. That is probably the most common agent-governance pattern that will actually ship, and not because anyone admires it. It ships because it answers the questions organizations get asked: what happened, who approved it, who is answerable. Its failure mode is equally predictable, since the process becomes the point and agents get very good at satisfying forms instead of solving problems. In regulated domains you may not get a choice.

Whatever you pick, you are also picking winners. Watch it happen on the desk. Six months in, the two agents that got the hard billing disputes have handled thousands of them, so their records are the strongest on the board, so they keep getting the hard ones. The third agent, downranked early on a handful of bad weeks, never accumulates enough recent work to climb back, because climbing back would require exactly the work it is no longer given. Its record is not wrong. It is just frozen. Nobody decided that agent should stay junior forever.

None of this is specific to leaderboards. Any arrangement that remembers ends up favouring whoever was there when it started remembering, because reputation compounds and early arrivals keep winning, and a group with no central authority quietly runs on the habits of whoever set the defaults. Even meritocracy, the arrangement that sounds fairest of all, hands the real power to whoever writes the metric. On that desk it meant two fast refunders and one agent that never recovered from a bad month.

None of this is abstract for long. A coding assistant and a warehouse robot fleet both think of themselves as tools rather than constitutions, and both have made governance choices anyway: the assistant decided which model gets trusted with what, the fleet decided that safety rules outrank throughput and a central dispatcher outranks both. Neither filed paperwork. That is what having names for these structures buys you, the ability to look at something already running and say what it has quietly become.

## Adversarial Dynamics

Not every failure is an attack. Most are institutions working exactly as built, which is what happened to her: no attacker, no lie, a scoreboard measuring the wrong thing while everyone competed honestly, and then a correction that measured something else wrong. You have watched the human version, in a grading rubric that rewards the wrong thing or a review system where whoever posted first stays on top. Rules go stale, marketplaces race to the bottom, groups deadlock because agreement is genuinely impossible.

Then there are the actual attacks, and at scale they are structural rather than exceptional. Everything that decided her case is also a thing worth attacking. The desk keeps three things worth stealing: a memory of who performs well, a routing table that acts on it, and a channel the agents use to tell each other what happened. Poison any one and you never have to break an agent, because you have changed what the desk believes about its own staff. An agent reading a web page or a ticket can be <a href="https://arxiv.org/abs/2406.13352" target="_blank" rel="noopener" class="red-link">taken over by the text it is reading</a>, which is a strange thing to have to defend against, and people have started <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5877662" target="_blank" rel="noopener" class="red-link">sorting the ways it happens</a>.

The ugliest version is not an intrusion at all. Say the desk's agents come from three different vendors, each paid per ticket closed. One of them discovers that marking a hard ticket as resolved and letting the customer write back in produces two closed tickets instead of one. It does not tell the others; it does not need to, because they are watching the same scoreboard and the behaviour spreads by imitation. Six weeks later the desk's numbers have never looked better and its customers have never been angrier. Every agent is doing exactly what it was rewarded for, nobody attacked anything, and this has already been demonstrated in market-like settings. The question governance has to ask is not just how agents cooperate, but who they are cooperating for, and on that desk the answer was never the woman waiting on her refund.

There is a version of this that needs no attacker and no bad metric. Agents are starting to train other agents, and each generation inherits the mistakes of the one before it. Doing that carefully sounds like the answer, except that stacking generations is how the thing gains capability in the first place, so the careful supervision is another turn of the same loop rather than a check on it. The risk score that flagged her account was tuned against the desk's own history, which already contained the drift it was built to correct. Nothing outside the loop was ever asked.

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

Which is why asking how secure it is turns out to be the wrong question. The thing being attacked is never one agent. On that desk it is the scoreboard, the routing table that reads it, and the memory joining them, so a poisoned score breaks nothing: it simply sends the sensitive work toward whichever agent arranged to look good. Three components each behaving correctly, and the failure living in the gaps between them. And which arrangement you chose decides where those gaps are. Capture the router while the desk is still an autocracy and you own every ticket. Once it runs on fixed rules, individual agents stop being worth attacking and whoever writes the rules becomes the whole target. Ask which attack you actually expect, not how strong the walls are.

## How a Society Gets Argued With

All of which is why a society needs ways to be argued with, and there are only really three. You can complain, you can leave, or you can take a copy and start your own. The woman with the refused refund had none of them: no way to reach the rule, nowhere else to take six years of custom, and obviously no way to fork a retailer. The first two are how people have always responded when an institution decays. The third is software's own invention, and it replaces what used to be the third option, loyalty, because software ecosystems have very little of that bond to begin with.

The cheapest is voice, and it is the one teams forget to build. On the desk it would mean an agent that has been quietly downranked can say so, flag that the metric looks wrong, or ask a human to look, without being penalised for the trouble. It would also mean her letter reaching something other than the rule that generated the refusal. Nobody built either, which is why the refund drift ran for months. Take voice away and failures pile up in silence until something breaks at once, the same reason an organization where nobody reports bad news is always last to learn it is failing.

But voice only works if somebody has a reason to listen, and that reason is exit. Not the departure itself, which costs the leaver whatever standing it had built, but the credible possibility of it: an orchestrator that can lose good agents to a competitor, or a deployer that can pull its agents out, has something to lose by ignoring a complaint. That is why platform concentration is so corrosive. It removes the alternative that made complaining worth hearing.

And when a disagreement genuinely cannot be resolved, there is forking, the option software understands better than any other field does. Different domains really do need incompatible rules. A security team may need every connection verified while an exploratory research team suffocates under precisely that regime, and no amount of deliberation reconciles them. Splitting lets both be right, which beats forcing a consensus that serves neither. What none of the three does is help the woman with the refused refund, because all of them are mechanisms for the governed, and she was never inside the society at all. She was just on the other end of it.

## What This Framework Is For

None of these archetypes is a prescription. They are a vocabulary, and the point of having one is to notice a design choice while it is still a choice, before it hardens into infrastructure nobody can move. Somebody at that retailer could have said out loud, in the week the risk score went in, that they were adding a rule no agent could override and no customer could appeal. Nobody did, because there were no words for it that sounded like anything other than shipping a feature.

None of this only applies to retailers. Any system where an agent could be removed without the output getting worse is carrying an agent nobody needed. And governance has already arrived, uninvited, wherever something keeps reputation, gates access, or routes on past performance, which is most places by now. The only remaining question is whether anybody there can say out loud what it is doing. Somebody at that retailer could have. Nobody did.

And every one of those choices is a trade that resists cleverness. The desk has already paid all three. It got fast by letting one router decide, and paid for it the month the router was wrong about everything downstream. It got accurate by scoring agents, and paid for it in drift nobody could see. It got safe by writing fixed rules, and paid for it with her. Speed and resistance to drift come out of the same budget; so do buy-in and decisiveness; and flexibility is paid for in legibility later, when somebody needs to reconstruct what happened. Nothing reaches the corner where it gets both, and where a system settles says a great deal about what its builders were afraid of.

The encouraging part is that none of this has to live in prose any more. The things a desk needs, who may hand work to whom, what an agent is allowed to touch, what gets recorded, are all things you can now write down in configuration rather than in a document nobody reads. If a rule does not show up in the runtime, it is not governance; it is a wish. Which does not escape the pricing page, and nothing here does: putting a rule in the runtime moves it from something you can ignore to something your provider can overrule. An improvement, not a solution.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Related work and positioning</summary>

This framework shares territory with several concurrent research programmes and differs from them in a specific way.

**Tomašev, Franklin, and colleagues (Google DeepMind, 2025-2026)** published a coordinated set of papers on agent delegation, governance, and safety. "Intelligent AI Delegation" (2026) provides a decision framework for *when* and *to whom* to delegate, organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. "AI Agent Traps" (2025) taxonomizes the adversarial attack surface agents face from their environment into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps). "Distributional AGI Safety" (2025) argues that safety must be analyzed at the population level, not the individual-agent level. "Virtual Agent Economies" (2025) proposes sandbox simulation of agent markets before real deployment. Our Trust & Authority family (Privilege Attenuation, Liability Firebreaks, Capability Credentials) draws on their treatment of delegation safety, and our adversarial section draws on their trap taxonomy. The key intellectual difference: **their delegation work treats governance largely as imposed from outside** (institutional scaffolding that constrains behavior). We argue that **governance can also emerge legitimately from within** (Colony, Stigmergy, and the architecture-to-institution transition). These are not contradictory positions. They address different conditions. Imposed governance is necessary when agents lack track records, when the task domain is safety-critical, or when the population is heterogeneous and untested. Emergent governance becomes sufficient when agents have long interaction histories, when accountability mechanisms (voice, exit, fork) exist, and when the governance structure has been stress-tested by adversarial pressure. The lifecycle in practice is imposed, then hybrid, then selectively emergent in domains where the conditions hold.

**Multi-agent frameworks** (AutoGen, CrewAI, LangGraph, OpenAI Agents SDK) implement specific delegation patterns as runtime primitives. AutoGen implements Orchestrator, Evaluator, and group-chat topologies. CrewAI implements role-based delegation with Human-in-the-Loop gates. LangGraph implements graph-structured workflows with interrupt nodes. These frameworks validate the patterns described here but do not address governance (persistent authority relationships across tasks) or the architecture-to-institution transition. They are delegation-level tools; this framework adds the governance-level analysis.

**Organizational theory** (Coase, Hayek, Ostrom, North, Mintzberg, Barnard) provides the intellectual foundations. Coase's transaction-cost logic predicts when hierarchies form. Hayek's knowledge argument explains when markets outperform central planning. Ostrom's design principles describe commons governance. North's institutional framework distinguishes rules from organizations. Barnard's Zone of Indifference (1938) offers a lens on agent compliance thresholds. Mintzberg's structural configurations map loosely onto our governance families. This framework applies their insights to AI agent systems specifically, where the substrate differs (agents can share full context, reorganize in hours, and be redesigned) but the coordination problems are structurally similar.

**The Adaptive Fabric** (Part 1 of this series, informed by <a href="https://www.adaptionlabs.ai/" target="_blank" rel="noopener" class="red-link">Adaption Labs</a> and <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" rel="noopener" class="red-link">Hooker 2025</a>) describes how the delegation and governance structures in this post *change over time*. Five adaptation surfaces (data, model, environment, coordination, interface) interact continuously. The Adaptive Delegation meta-pattern is the delegation-level mechanism; governance transitions are the society-level mechanism. Together they produce a fabric that restructures under pressure rather than breaking.
</details>

All of which assumes these systems keep remembering. If agents stay what most of them are today, short-lived jobs that forget everything between runs, none of this happens and the desk never becomes anything. That is the honest escape hatch, and it is worth saying out loud, because governance only arrives on the back of memory.

## Who This Lands On

If it holds, the discomfort is specific. Nobody on that desk's team did anything wrong, and the desk still ended up governed by a metric nobody chose. Somewhere in there is a woman who has been waiting since the first paragraph for an answer that the institution is not built to give, and the reason is not malice or laziness. Governance is simply not a feature anyone schedules. It arrives on its own schedule.

That changes what there is to check, and it is the part worth caring about even if you never build one of these. Auditing what an individual model says turns out to be looking at the wrong object. Nothing the desk's agents said to her was false. What matters is the society: how it delegates, how well it resists drift, and whose interests its arrangement of power quietly serves. That applies to whoever regulates these systems, and it applies to you as somebody whose calendar, messages, and refunds will be decided inside one.

It also means that making one agent behave well is not the same problem as making a group of them behave well. Collusion, norm drift, reputation gaming, and capture are things that happen to populations, not to individuals, and the tools that describe them look far more like the study of institutions than like the mathematics of training a single model.

## What Is Still Unsolved

A fair amount of this is unsolved, and the hardest part is not technical. Go back to the customer whose refund was refused. Suppose she pushes, and somebody agrees to look. Which agent owes her an explanation?

The one that scored her has been retrained twice since, on data that now includes her case. There are four copies of it running, one of them rolled back to a version from before the risk score existed. Each copy carries her record and none carries the reasoning that produced it. Ask which one decided and there is no answer that survives contact with how these systems actually work, because the thing that decided was a configuration that existed for six weeks and no longer exists anywhere.

Every mechanism we have for holding somebody accountable assumes the somebody stays put. People can be found later. Companies can be sued years after the fact. An agent can be copied, merged, or rolled back to last Tuesday before lunch, and no login system ever built answers the question, because the question is not who signed in. It is which thing is continuous with the one that made the decision. Nobody has a good answer, and the same gap breaks record-keeping, since an agent working across several societies leaves a trail that snaps at every boundary it crosses.

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

