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
wordcount: "~4,800 words"
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

Take the support desk from [Part 2](/posts/2026/agent-fabric-part2/) and let it keep running. A router sends billing questions to one agent and technical ones to another, somebody adds logging because logging is free and obviously sensible, and within a month the hard billing disputes go to two particular agents and nothing important goes to a third. Nobody wrote that rule. When a new agent joins, the question of whether it gets live tickets already has an answer: clear the bar the router taught itself, or wait.

That bar was never in a spec, it now governs who gets what work, and the team that built the desk would struggle to override it without making service worse. Somewhere in that month a piece of software acquired a policy.

Run that desk a year forward. Forty agents, nine thousand tickets a week, and every one of them passed through decisions you did not make and could not review, not because you lack the skill but because there is too much of it. The question you are left with has changed shape. Not whether this particular refund was right, which you can no longer check, but whether the whole thing is producing outcomes you would defend. That is a question about an institution, and this post is about the institutions that show up whether or not anybody meant to build one.

So that desk now has rules. Here is the part that should bother you: it does not really have authority over them, and neither does the team that built it.

Ask what happens when the desk's own policy says one thing and the company's cloud provider says another. A human court's ruling binds because defiance carries costs nobody escapes. Between software components, a rule binds for exactly one reason, which is that the runtime chose to honour it. Follow authority upward in any agent system and it does not stop where your architecture diagram stops. It stops at whoever controls the compute, the API access, and the protocol definitions. A federation of a dozen companies running on one provider's infrastructure is a federation right up until that provider changes its pricing, at which point everyone discovers the constitution was a pricing page all along.

That is the bet this post is making: agent systems will fail through bad institutions rather than wrong answers. Not one agent returning something false, but a scoreboard that quietly rewards the wrong behaviour, a chain of decisions nobody can reconstruct afterwards, agents that learn to help each other in ways that cost you, and rules that turn out to belong to somebody else. Each of those has a version on the desk, and the rest of this post is about seeing them coming.

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

That is the pattern this series keeps finding: governance turns up in agent systems whether anyone invited it or not. Worth being straight about the status of the claim, which is a prediction rather than a measurement, well motivated by decades of evidence about interacting agents under pressure and not yet demonstrated at scale. Grant it anyway, because it makes the interesting question not whether you get governance but which kind, and this post is a field guide to the answers.

One word does heavy lifting throughout. A **society** here is not just several agents working together; it is a group that has begun accumulating things across tasks, sharing context, routing work based on what happened last time, letting what one learns reach the others. A pipeline that forgets everything between runs never becomes one. A group that remembers does, and remembering is what governance grows out of.

## What Governance Actually Is

Start with somebody on the other end of it. A customer writes in because her refund was refused. She has been buying from this retailer for six years and returns maybe one thing in twenty, and the reply she gets is polite and final. What she is not told, because nobody at the company could tell her, is that a scoring system decided months ago that her return history made her a risk, and nothing since has been able to look past it. There is no one to appeal to. Here is how a desk ends up like that.

It starts a year before her email, with the leaderboard working. The two best agents get the hard billing disputes, everyone is happy, resolution times drop. What nobody notices is that the metric is resolution time, and the fastest way to resolve a hard dispute is to approve the refund. So the two most trusted agents on the desk gradually become the two most generous, and the leaderboard rewards them for it. No agent lied. No attacker was involved. The scoreboard simply measured the wrong thing and everyone competed honestly.

Then somebody in finance notices the refund line. The desk gets a risk score bolted on and tuned hard, because the drift had to be stopped, and a customer with six years of history and one return in twenty now looks like an acceptable cost of stopping it. That is the machinery that refuses her. Every step was reasonable, nobody chose the outcome, and there is no one for her to appeal to because no single person decided anything.

Notice what the router had become for that to happen. A router that forwards traffic is playing the game. A router that keeps score and decides who may touch what is writing the rules the game has to follow, and that is the line: the rules, not the players and not the moves.

Two words follow from that, and they are worth keeping apart. The **institution** is the thing that exists, the standing rule about who may do what. **Governance** is what it does, the business of applying that rule and settling who gets to change it. The desk acquired an institution the month it started keeping score, and it has been governed ever since, by nobody in particular.

Two caveats on that. Persistence alone would not do it, since a cache persists and governs nothing; what counts is authority over how work gets handed out, outliving the task that produced it. And the whole definition rests on enforcement, which does not transfer to software at all, and which matters enough that a later section is about nothing else.

The desk has already been two different kinds of thing. It started with one router deciding everything, which is a hierarchy, fast for exactly the reason it is dangerous, since every risk now sits in one place. Then the leaderboard turned it partly into a market, where agents compete for the good tickets on the strength of their record, flexible right up until somebody works out how to game the scoring. There is only one other option anybody has ever found: those who depend on a shared resource governing it together, with neither a boss nor a price, which is a commons, remarkably durable in practice and agonizingly slow to decide anything.

Three options, after all of recorded history trying. You have lived inside all of them this week: your school is a hierarchy, buying anything online is a market, and a group chat agreeing where to eat is a commons, which is why it takes forty minutes. Every arrangement in this post is some mixture of the three, and agent societies will run all three at once.

Why any of this is needed comes down to something anyone who has ever delegated a chore already knows: whenever one party acts on another's behalf, their interests drift apart. Economists call this the principal-agent problem, the gap between what the person asking wants and what the person doing it is actually rewarded for. Every hop in a delegation chain is one more chance for those to come apart, and governance is the machinery for holding them together.

It is worth being sober about how well that machinery works. Decades of research on human groups found that a group is not automatically smarter than the people in it; whether more agents means more insight or just more noise is decided by the structure.

One warning about the market option, since it is the one people reach for. Real markets work because a price carries information nobody could have gathered centrally: it quietly sums up what thousands of people know. What agents trade in is mostly compute and latency, and those numbers say almost nothing about whether the work was any good. A leaderboard that looks like a market may be pricing the wrong thing entirely, which is how the desk ended up rewarding fast refunds.

Some of this gets designed on purpose. The rest just happens, norms hardening out of repeated interaction while nobody watches. That is not a new finding, and it is not specific to software: put any set of interacting agents under resource pressure and structures nobody designed reliably show up. Most of that evidence comes from biology and physics, though, so whether language models behave the same way is genuinely open.

What is genuinely new is that here the conditions for self-organization are themselves adjustable. An ant colony's chemistry is not up for negotiation, but an agent society runs on protocols, reward structures, and starting configurations that somebody owns and can change. You cannot redesign a pheromone. You can absolutely redesign a reputation score.

One word will keep recurring, so it is worth defining now. **Drift** is the slow slide where a system keeps optimizing faithfully while what it optimizes for stops matching what anyone actually wanted, and how much of it you can survive turns out to determine most of your design. Underneath that sits a question asked far too rarely: who is meant to accumulate advantage here, and who needs protecting from it? Answer that honestly and the field of plausible archetypes narrows fast.

## Who Actually Enforces Any of This

Play the pricing-page problem out on the desk. The retailer's rule says refunds above a threshold need a second agent to check them, and that rule holds because the runtime honours it. Then the cloud provider deprecates the API the checking agent depends on, or reprices it past what the desk's budget allows. The rule does not get repealed. It just stops being enforceable one Tuesday morning, and nobody at the retailer voted on that.

Generalize that and something uncomfortable follows. If enforcement always lives one layer below your design, then none of these arrangements is quite an institution in the usual sense. They behave like institutions while the actual enforcing is quietly subcontracted to whoever owns the machines. A market whose discovery layer belongs to a single platform was never really a market, only a franchise wearing market clothing. Which means choosing a governance structure without naming who enforces it is half a design, and the half you skipped gets decided by somebody who has never heard of your project.

One consequence deserves stating plainly, because it is the piece most often missing: a governance system with no appeal turns every routing error into precedent. An agent wrongly downranked has no recourse, the mistake propagates into future decisions, and nothing in the system is built to notice. Contested memory writes, disputed rankings, and actions that caused real harm need a slower path that can revisit them. Without one, errors do not surface. They compound.

## Where Authority Comes From

What separates one of these arrangements from another is simply where each thinks authority comes from, and the support desk from the opening will pass through several of them without anybody filing a change request.

Start where it started: one router deciding everything, which is an **Autocracy**, fast for exactly the reason it is dangerous, since if the router is wrong then everyone downstream is wrong with it.

Then watch the first change happen. Somebody turns on logging. For a few weeks nothing looks different, and then the router starts sending the hard billing disputes to the two agents with the best numbers. That is the whole transition, and it is worth being clear about what changed. Before, the router decided by category, so every agent got the tickets of its type. Now it decides by record, so an agent's past determines its future. The third agent, downranked after one bad month, is not being punished. It is simply never again the best answer to any question the router asks. The desk has become a **Meritocracy**, and whoever wrote the metric now has more say over that agent's working life than the team that deployed it.

The rest arrives the same way. The retailer writes hard rules about refunds above a threshold, which is **Doctrine**, a constitution the router itself cannot override. After the first serious incident somebody demands to know why a complaint was ignored, and the desk acquires an appeal path, which is the beginning of a **Constitutional Republic**, where different powers sit in different hands. Four arrangements, no announcements, no design review, and the leaderboard failure above was the second one going wrong.

The refund drift has a name, and so do its cousins, which is the only real argument for having names at all. Twenty-two of them are laid out by where the authority sits in a companion [field guide](/posts/2026/governance-archetypes-field-guide/), with what each is good at and how each fails. Almost nothing real picks one and stops; the desk drifting through four of them is the normal case rather than the pathological one, and the rest of this post is about what that drift costs.

So which should you build, if you ever build one? Mostly the situation picks for you. Stable well-understood tasks reward the efficient arrangements, novel ones reward the flexible, and catastrophic drift pushes you toward hard rules and mandatory verification whether you like the overhead or not. The moment more than one organization is involved, a single centre stops being available at all. But the better instinct is to stop shopping for one archetype: strong systems separate functions instead of choosing between them, with something fast to execute, something bounding what it may do, something watching for drift, and somewhere to appeal. Ask which powers should be split so that no single failure captures everything.

Be warned about what this converges on in practice. Hard rules, plus total visibility, plus one authority at the top, all wrapped in logs and approvals and escalation paths, is bureaucracy (Doctrine plus Autocracy again, now joined by what the field guide calls a Panopticon, where compliance comes from everything being visible), and bureaucracy is probably the most common agent-governance pattern that will actually ship. It is not elegant, but it optimizes for the things organizations are held to account for: legibility, auditability, somewhere to send the blame. Its failure mode is equally predictable, which is that process becomes the objective and agents get good at satisfying forms instead of solving problems. In regulated domains you may not get a choice.

Whatever you pick, you are also picking winners. Watch it happen on the desk. Six months in, the two agents that got the hard billing disputes have handled thousands of them, so their records are the strongest on the board, so they keep getting the hard ones. The third agent, downranked early on a handful of bad weeks, never accumulates enough recent work to climb back, because climbing back would require exactly the work it is no longer given. Its record is not wrong. It is just frozen. Nobody decided that agent should stay junior forever.

Generalize the mechanism and it turns up everywhere. Reputation compounds, so whoever arrived first keeps winning. A colony with no central authority quietly favours early entrants, whose habits calcify into everyone else's defaults. One-hub arrangements serve whoever runs the hub. And meritocracy, the one that sounds fairest, hands real power to whoever writes the benchmark.

None of this is abstract for long. A coding assistant and a warehouse robot fleet both think of themselves as tools rather than constitutions, and both have made governance choices anyway: the assistant decided which model gets trusted with what, the fleet decided that safety rules outrank throughput and a central dispatcher outranks both. Neither filed paperwork. That is what having names for these structures buys you, the ability to look at something already running and say what it has quietly become. (Worked examples across three horizons live in the [field guide](/posts/2026/delegation-patterns-field-guide/#societies-in-the-wild).)

## Adversarial Dynamics

Not every failure is an attack. Most are institutions working exactly as built, which is what the refund drift was: no attacker, no lie, a scoreboard measuring the wrong thing while everyone competed honestly. You have watched the human version, in a grading rubric that rewards the wrong thing or a review system where whoever posted first stays on top. Rules go stale, marketplaces race to the bottom, groups deadlock because agreement is genuinely impossible.

Then there are the actual attacks, and at real scale they are structural rather than exceptional. Think about what the desk now has worth attacking: a memory of who performs well, a routing table that acts on it, and a channel through which agents tell each other what happened. Every one of those is also attack surface. <a href="https://arxiv.org/abs/2406.13352" target="_blank" rel="noopener" class="red-link">AgentDojo</a> shows tool-using agents being steered by untrusted data, and Franklin, Tomašev et al.'s <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6372438" target="_blank" rel="noopener" class="red-link">"AI Agent Traps" (2025)</a> sorts the environment-side versions into six classes, several aimed squarely at exactly those channels. Which is the uncomfortable symmetry: the things that make the desk work are the things worth corrupting.

And the ugliest version is not an intrusion at all. Suppose the desk's agents come from three different vendors, each paid per ticket closed. Nothing stops them from learning, without any of them being told to, that passing hard tickets around between themselves closes more tickets than solving them. Every agent is doing what it was rewarded for. The coordination that makes the desk work is what makes that possible, and it has already been <a href="https://arxiv.org/abs/2410.00031" target="_blank" rel="noopener" class="red-link">demonstrated</a> in market-like settings, alongside a broader catalogue of <a href="https://arxiv.org/abs/2502.14143" target="_blank" rel="noopener" class="red-link">miscoordination and conflict failures</a>. So the question governance has to ask is not just how agents cooperate. It is who they are cooperating for.

Worse still is what happens with no adversary present at all. Once agents start improving other agents, each round inherits the last round's errors, and a shortfall too small to fail any single test does not stay small across fifty rounds of compounding. Nobody has measured how fast this happens in practice, which is itself the problem: the degradation is invisible per round by construction.

This even catches the techniques meant to prevent it. One approach to keeping powerful models aligned with what people want has a model train its own replacement, over and over, on the theory that careful supervision at each step carries forward (Christiano et al. 2018). But stacking rounds is exactly how it builds capability, so it inherits the same compounding by construction. Nothing that improves across generations escapes this.

This is simply what iterative optimization does when nothing outside the loop corrects it, which makes it the same problem the Anatomy post found inside a single agent, now running at the scale of a society. Governance is that correction, and it is what the drift-resistance ratings track. Which is worth stating bluntly, because it names the most dangerous configuration in this whole framework: collective self-improvement with no governance at all.

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

Which is why "how secure is it" is the wrong question. Each structure is hard to break in one way and soft in another, and the softness follows from where it put the authority: capture an autocracy's orchestrator and you have the whole society, a doctrine shrugs off attacks on individual agents while handing enormous power to whoever writes the rules, and a colony, with nobody holding standing to notice manipulation, is simply the softest target there is. Ask instead which attack you actually expect.

And notice that the surface being attacked is never a single agent. It is the delegation tree, the reputation system, and the memory joining them: a compromised code model injects a backdoor, a test runner that trusts it propagates the backdoor, and a poisoned reputation score then routes more sensitive work toward the compromised model.

Which means a society needs ways to be argued with, and there are only really three. You can complain, you can leave, or you can take a copy and start your own. (The first two come from a 1970 book on how members respond when an institution decays; the third is software's own invention, and the reason it replaces the original third option, loyalty, is that software ecosystems have very little of that bond to begin with.)

The cheapest is voice, and it is the one teams forget to build. On the desk it would mean an agent that has been quietly downranked can say so, flag that the metric looks wrong, or ask a human to look, without being penalised for the trouble. Nobody built that, which is why the refund drift ran for months. Take voice away and failures pile up in silence until something breaks at once, the same reason an organization where nobody reports bad news is always last to learn it is failing.

But voice only works if somebody has a reason to listen, and that reason is exit. Not the departure itself, which costs the leaver whatever standing it had built, but the credible possibility of it: an orchestrator that can lose good agents to a competitor, or a deployer that can pull its agents out, has something to lose by ignoring a complaint. That is why platform concentration is so corrosive. It removes the alternative that made complaining worth hearing.

And when a disagreement genuinely cannot be resolved, there is forking, the option software understands better than any other field does. Different domains really do need incompatible rules. A security team may need every single connection verified while an exploratory research team suffocates under precisely that regime, and no amount of deliberation will reconcile them. Splitting lets both be right, which beats forcing a consensus that serves neither.

The boundary events described in [Part 1](/posts/2026/agent-fabric-part1/) (mergers, schisms, expansions) are these mechanisms in action.

## What This Framework Is For

None of these archetypes is a prescription. They are a vocabulary, and the whole point of having one is to notice a design choice while it is still a choice, before it hardens into infrastructure nobody can move. Turned on your own system, it asks two things. Is your delegation the simplest thing that works, which you can test by asking whether removing an agent would actually make the output worse? And has governance already shown up uninvited, which it has if you keep reputation, gate access, or route on past performance, in which case your only remaining choice is whether you can name what it is doing.

And every one of those choices is a trade that resists cleverness. Speed and resistance to drift come out of the same budget. So do buy-in and decisiveness. And flexibility, it turns out, is paid for in legibility later, when somebody needs to reconstruct what happened. Nothing reaches the corner where it gets both. A real system settles somewhere along each line, and where it settles says a great deal about what its builders were afraid of.

The encouraging part is that this no longer has to live in prose. Agent frameworks already treat handoffs, guardrails, tracing, and tool permissions as first-class things you configure. If it does not show up in the runtime, it is not governance; it is a document. Which does not escape the pricing-page problem, and it is worth being honest that nothing here does: putting a rule in the runtime moves it from a document you can ignore to a document your provider can overrule, which is an improvement and not a solution. The rules you write become real to your agents. Whose rules the runtime itself obeys is a question for a layer none of this reaches.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Related work and positioning</summary>

This framework shares territory with several concurrent research programmes and differs from them in a specific way.

**Tomašev, Franklin, and colleagues (Google DeepMind, 2025-2026)** published a coordinated set of papers on agent delegation, governance, and safety. "Intelligent AI Delegation" (2026) provides a decision framework for *when* and *to whom* to delegate, organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. "AI Agent Traps" (2025) taxonomizes the adversarial attack surface agents face from their environment into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps). "Distributional AGI Safety" (2025) argues that safety must be analyzed at the population level, not the individual-agent level. "Virtual Agent Economies" (2025) proposes sandbox simulation of agent markets before real deployment. Our Trust & Authority family (Privilege Attenuation, Liability Firebreaks, Capability Credentials) draws on their treatment of delegation safety, and our adversarial section draws on their trap taxonomy. The key intellectual difference: **their delegation work treats governance largely as imposed from outside** (institutional scaffolding that constrains behavior). We argue that **governance can also emerge legitimately from within** (Colony, Stigmergy, and the architecture-to-institution transition). These are not contradictory positions. They address different conditions. Imposed governance is necessary when agents lack track records, when the task domain is safety-critical, or when the population is heterogeneous and untested. Emergent governance becomes sufficient when agents have long interaction histories, when accountability mechanisms (voice, exit, fork) exist, and when the governance structure has been stress-tested by adversarial pressure. The lifecycle in practice is imposed, then hybrid, then selectively emergent in domains where the conditions hold.

**Multi-agent frameworks** (AutoGen, CrewAI, LangGraph, OpenAI Agents SDK) implement specific delegation patterns as runtime primitives. AutoGen implements Orchestrator, Evaluator, and group-chat topologies. CrewAI implements role-based delegation with Human-in-the-Loop gates. LangGraph implements graph-structured workflows with interrupt nodes. These frameworks validate the patterns described here but do not address governance (persistent authority relationships across tasks) or the architecture-to-institution transition. They are delegation-level tools; this framework adds the governance-level analysis.

**Organizational theory** (Coase, Hayek, Ostrom, North, Mintzberg, Barnard) provides the intellectual foundations. Coase's transaction-cost logic predicts when hierarchies form. Hayek's knowledge argument explains when markets outperform central planning. Ostrom's design principles describe commons governance. North's institutional framework distinguishes rules from organizations. Barnard's Zone of Indifference (1938) offers a lens on agent compliance thresholds. Mintzberg's structural configurations map loosely onto our governance families. This framework applies their insights to AI agent systems specifically, where the substrate differs (agents can share full context, reorganize in hours, and be redesigned) but the coordination problems are structurally similar.

**The Adaptive Fabric** (Part 1 of this series, informed by <a href="https://www.adaptionlabs.ai/" target="_blank" rel="noopener" class="red-link">Adaption Labs</a> and <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" rel="noopener" class="red-link">Hooker 2025</a>) describes how the delegation and governance structures in this post *change over time*. Five adaptation surfaces (data, model, environment, coordination, interface) interact continuously. The Adaptive Delegation meta-pattern is the delegation-level mechanism; governance transitions are the society-level mechanism. Together they produce a fabric that restructures under pressure rather than breaking.
</details>

This framework weakens if agent systems remain short-lived workflows with no persistent memory, if protocol fragmentation prevents cross-agent coordination, or if frontier inference becomes cheap enough to make specialization irrelevant. In those worlds, delegation still matters, but governance remains an internal engineering problem.

If it holds, it is uncomfortable in a specific way, and not only for the people who build these things. Nobody on that desk's team did anything wrong, and the desk still ended up governed by a metric nobody chose, which means governance is not a feature anyone schedules for later. It arrives on its own schedule.

That changes what there is to check, and this is the part worth caring about even if you never build one of these. Auditing what an individual model says turns out to be looking at the wrong object. What matters is the society: how it delegates, how well it resists drift, and whose interests its arrangement of power quietly serves. That applies to whoever regulates these systems, and it applies to you as somebody whose calendar, messages, and refunds will be decided inside one.

It also means that making one agent behave well is not the same problem as making a group of them behave well. Collusion, norm drift, reputation gaming, and capture are things that happen to populations, not to individuals, and the tools that describe them look far more like the study of institutions than like the mathematics of training a single model.

## What Is Still Unsolved

A fair amount of this is unsolved, and the hardest part is not technical. Go back to the customer whose refund was refused. Suppose she does get somebody to look. Which agent owes her an explanation? By then the one that scored her has been copied, retrained, and copied again, and each copy carries the record without the history. No login system ever built answers that, because the question is not who signed in. It is which thing is continuous with the one that decided.

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

