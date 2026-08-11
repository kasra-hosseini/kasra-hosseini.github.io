---
title: "The Agent Fabric (Part 3): Ruling an Agent Society"
subtitle: "How authority forms when nobody designs it, and who is left to argue with it"
date: 2026-07-31
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "multi-agent systems", "LLM", "agent fabric", "governance", "the loom hypothesis"]
description: "How agent societies end up governed by rules nobody wrote, why governance runs on infrastructure somebody else owns, and how these systems fail."
draft: false
math: false
ShowToc: false
TocOpen: false
hideCitation: false
wordcount: "~2,500 words (body) · reference sections extend it"
---

<p style="font-size: 0.82em; color: #999; margin-top: 1em;"><a href="https://code.claude.com/docs/en/overview" target="_blank" rel="noopener" style="color: #999;">Claude Code</a> was used for editing and visualizations. All ideas and arguments are the authors' own.</p>

<div style="background: #fffbeb; border: 1px solid #f59e0b; border-radius: 6px; padding: 0.6em 1em; margin: 1.5em 0; font-size: 0.88em; color: #92400e;">
<strong>Early access.</strong> This blog series is a work in progress. Feedback, comments, and suggestions are welcome. Feel free to <a href="https://www.linkedin.com/in/kasra-hosseini/" target="_blank" rel="noopener" style="color: #92400e;">reach out on LinkedIn</a> or leave a comment at the bottom of the page.
</div>

A woman writes to a shop she has used for six years to ask why her refund was refused. She returns maybe one thing in twenty. The reply is polite and final. What nobody tells her, because nobody at the company can, is that a scoring system decided months ago that her return history made her a risk, and nothing since has been able to look past it. There is no one to appeal to, and no one chose this.

That shop runs the support desk from [Part 2](/posts/2026/agent-fabric-part2/), the one where an engineer spent an afternoon looking for a rule that turned out not to exist. The engineer never found anyone who had written it, and she could not undo it without making service worse. By the time this woman wrote in, the desk had been running for a year.

By the end of that year it is handling nine thousand tickets a week across the forty agents the desk grew into, every one of them decided by machinery no one has reviewed, not for lack of skill but because there is too much of it. So the desk has rules the desk itself cannot change, and neither can the team that built it. Her letter is not refused so much as unaddressed: the question it asks is why me, and there is nobody that question belongs to.

Nothing any agent told her was untrue. Every answer was correct, and that is how these systems will fail long before they start lying.

The desk was always going to end up with rules. The only open questions were which ones, and who would get to change them.


## Four Desks in One Year

What she eventually got was four polite sentences and no way to reply to them. By the time her letter arrived, the desk had already been three other things. Not four options anyone weighed up: four things the same desk turned into, in order.

**The first desk: one agent decides.** Her letter reaches whichever agent handles returns. It reads the letter, looks at six years of orders, and decides, so by that afternoon she has a reply explaining that twelve returns in six years is not a pattern and the refund is approved. If it decides wrong, everyone downstream is wrong with it, but she could at least name what decided. One router is the simplest thing that works, which is why the desk started here, and the price is that on the day it is wrong about her there is nothing else in the room.

**The second desk, a month in: the scoreboard decides.** Somebody turns on logging, and now her letter goes to whichever agent has the best numbers rather than whichever one knows returns. On the board that morning the returns specialist sits fourth at nine minutes a ticket, so her letter goes to the one at three, who has never handled a return like hers. The reply still reads like a decision, and it came from whichever agent was winning that week.

Nobody told any agent to refuse returns. The leaderboard rewarded speed, and the fastest way to close a return is to refuse it.

Nobody had to say that out loud. What the desk was optimizing for **drifted** away from what the desk was for, which is the failure [Part 1](/posts/2026/agent-fabric-part1/#the-adaptive-fabric) describes as the risk of a system that adapts faster than anyone checks what it is adapting toward. Every step looked like an improvement.

The agent that would have read her letter properly is the one that had a slow month and never recovered. It is not being punished. It is simply never again the best answer to any question the router asks, and nobody checked whether closing time was the right thing to measure before it started deciding who was good at the job.

**The third desk, half a year in: the rule decides.** This is the one her letter reached, and it never got as far as an agent's judgement. Somebody in finance had looked at the refund line and written the rules down instead. One rule flagged certain return histories no matter what any agent thought, and hers was one of them.

Another rule said refunds above her amount get a second reader, and hers qualified. It never got one: two days earlier, the company that provided the second reader had switched the service off. Nobody repealed the rule. It just stopped being true. What she gets back is four polite sentences about return-history thresholds, no name at the bottom, and no route to anyone who could look again. She reads it twice looking for the part that is about her, and there is no such part. Every reply correct, and none of them about her.

It works, too. The refund line stops climbing inside a month, which is why nobody went back to look at the letters it closed. Under the desk before it an agent could read six years of history and decide she was fine. Under this one there is nothing to decide, and nowhere for anyone to put the fact that the answer is obviously wrong.

**The fourth desk, the one she needed:** it exists only after somebody complains loudly enough that a shrug will not do. Here one thing answers her letter and something else can be asked to look again, and that second thing is allowed to say the first got it wrong.

It is worth being concrete about what that takes, because it is the only desk of the four anybody has to decide to build. Three things, none of them clever. The reply has to name the rule it applied and the threshold that was crossed, which means every automated decision carries the reason it was made rather than only its outcome. There has to be a second reader that does not report to the first, and cannot be switched off quietly, because the third desk already proved what happens to a check nobody notices expiring. And the flag on her account has to have an owner: a person or a team whose job includes being asked why it is still set. Two engineers, some months. It is not a research problem.

What she gets back names the rule, names the threshold she crossed, and gives her an address that is not the thing that refused her. The refund may still be denied. The difference is that whatever reads the appeal does not answer to the thing that refused her, so somebody could find out why, and so could she.

The desk is also better at its job, which is the part that gets left out. The appeals it hears are the only place the retailer learns that its threshold is set wrong. Under the third desk, every bad refusal closed cleanly and looked like a saving; the refund line went down and the error was invisible in the numbers. Under the fourth, a run of overturned refusals is a signal, and it is the only signal the company will ever get about a rule that is quietly costing it six-year customers. The appeal route is not a courtesy bolted on after the fact. It is the feedback path, and a desk without one cannot tell a good decision from a cheap one.

This desk is slow to build, which is why it usually arrives one letter too late. Nothing about it required a better model. It required somebody deciding, before the incident rather than after, that a decision nobody can question is not finished.

Four arrangements in a year, no announcements and no design review, and she happened to write in under the third, the only one with no way back in. Every one of them needed a record to exist at all, and plenty of software keeps records. This one decided who would read her letter.

Nothing broke while that happened. Every agent did well against the measure it was given, and every correction along the way was reasonable. She is the one who paid for all of them, and the question nobody asks early enough is who an arrangement is meant to favour.

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

Nobody asked the desk to refuse a six-year customer. Somebody asked it to keep refund losses down, and refusing her was the cheapest way to do that. The flag on her account was one judgement, made once, by something scoring return rates, and every decision after it treated that judgement as settled. It was a number in a config file with her name nowhere near it. On any Tuesday somebody could have opened that file and decided it was wrong. No one did, because no one was looking, and there was nowhere for her to ask.

That number is the [load-bearing record](/posts/2026/agent-fabric-part2/) again, one desk further on. Part 2 watched a routing table become the rule for which agent got work. Here a risk score has become the rule for which customer gets served. Governance is what you call it once somebody outside the system has to live with what the record decided.

## Complain, or Go Elsewhere

So she complains, which means replying to the email that refused her. The reply comes back in four minutes and repeats the second paragraph of the first one. Then she does what everyone assumes she can do instead, and looks elsewhere: the next shop wants an account before it will show her anything, and the form asks for her email address, the one she has used for six years.

Somebody could have said out loud, in the week the risk score went in, that they were adding a rule no agent could override and no customer could appeal. No one said it, because a risk score is not the kind of thing anyone objects to.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Related work and positioning</summary>

This framework shares territory with several concurrent research programmes and differs from them in a specific way.

**Tomašev, Franklin, and colleagues (Google DeepMind, 2025-2026)** published a coordinated set of papers on agent delegation, governance, and safety. "<a href="https://arxiv.org/abs/2602.11865" target="_blank" rel="noopener" class="red-link">Intelligent AI Delegation</a>" (2026) provides a decision framework for *when* and *to whom* to delegate, organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. "<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6372438" target="_blank" rel="noopener" class="red-link">AI Agent Traps</a>" (2025) taxonomizes the adversarial attack surface agents face from their environment into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps). "<a href="https://arxiv.org/abs/2512.16856" target="_blank" rel="noopener" class="red-link">Distributional AGI Safety</a>" (2025) argues that safety must be analyzed at the population level, not the individual-agent level. "<a href="https://arxiv.org/abs/2509.10147" target="_blank" rel="noopener" class="red-link">Virtual Agent Economies</a>" (2025) proposes sandbox simulation of agent markets before real deployment. Our Trust & Authority family (Privilege Attenuation, Liability Firebreaks, Capability Credentials) draws on their treatment of delegation safety, and our adversarial section draws on their trap taxonomy. The key intellectual difference: **their delegation work treats governance largely as imposed from outside** (institutional scaffolding that constrains behavior). We argue that **governance can also emerge legitimately from within** (Colony, Stigmergy, and the architecture-to-institution transition). These are not contradictory positions. They address different conditions. Imposed governance is necessary when agents lack track records, when the task domain is safety-critical, or when the population is heterogeneous and untested. Emergent governance becomes sufficient when agents have long interaction histories, when accountability mechanisms (voice, exit, fork) exist, and when the governance structure has been stress-tested by adversarial pressure. The lifecycle in practice is imposed, then hybrid, then selectively emergent in domains where the conditions hold.

**Multi-agent frameworks** (AutoGen, CrewAI, LangGraph, OpenAI Agents SDK) implement specific delegation patterns as runtime primitives. AutoGen's conversable agents and group-chat manager make Orchestrator and Evaluator arrangements straightforward to build on top of them. CrewAI implements role-based delegation with Human-in-the-Loop gates. LangGraph implements graph-structured workflows with interrupt nodes. These frameworks validate the patterns described here but do not address governance (persistent authority relationships across tasks) or the architecture-to-institution transition. They are delegation-level tools; this framework adds the governance-level analysis.

**Organizational theory** (Coase, Hayek, Ostrom, North, Mintzberg, Barnard) provides the intellectual foundations. Coase's transaction-cost logic predicts when hierarchies form. Hayek's knowledge argument explains when markets outperform central planning. Ostrom's design principles describe commons governance. North's institutional framework distinguishes rules from organizations. Barnard's Zone of Indifference (1938) offers a lens on agent compliance thresholds. Mintzberg's structural configurations map loosely onto our governance families. This framework applies their insights to AI agent systems specifically, where the substrate differs (agents can share full context, reorganize in hours, and be redesigned) but the coordination problems are structurally similar.

**The Adaptive Fabric** (Part 1 of this series, informed by <a href="https://www.adaptionlabs.ai/" target="_blank" rel="noopener" class="red-link">Adaption Labs</a> and <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" rel="noopener" class="red-link">Hooker 2025</a>) describes how the delegation and governance structures in this post *change over time*. Five adaptation surfaces (data, model, environment, coordination, interface) interact continuously. The Adaptive Delegation meta-pattern is the delegation-level mechanism; governance transitions are the society-level mechanism. Together they produce a fabric that restructures under pressure rather than breaking.
</details>

## Nobody Left to Ask

She writes again, and this time somebody at the retailer agrees to look, which is when the question becomes which agent owes her an explanation.

The obvious place to look is the model that scored her, and the version that scored her no longer exists. It has been retrained twice since her refusal, each time on data that included her case. She can be shown the model that would score her today. She cannot be shown the one that did. A person can be found years later. A company can be sued a decade on. This was a configuration that lasted six weeks, and an apology would have nowhere to come from even if somebody wanted to give her one.

That is a hard problem, and it is not the hardest one here. Ask instead who was able to make her rule stop working, and the third desk has already answered it. The rule that entitled her to a second reader did not stop applying because anyone at the retailer decided it should. It stopped applying because a company two steps removed switched a service off. No one at the retailer was consulted or noticed, and the rule stayed in the config file being true and inoperative at the same time. The retailer wrote that rule and believed it owned it. What it owned was a sentence. The thing that made the sentence happen belonged to somebody else.

That is the limit under every arrangement in this post. Whatever the governance says, it runs on compute somebody else provisions, on models somebody else upgrades and retires, under terms somebody else changes. A constitution holds only as far as the platform underneath it agrees to keep executing it. An appeal route can be deprecated by a vendor on a Tuesday for reasons that have nothing to do with the desk, and the desk will not know until somebody's letter comes back wrong. Governance describes who is supposed to decide. Ownership of the substrate decides who actually can, and those two are usually not the same party. So the interesting question about any agent society is not what its rules say but whose infrastructure has to stay up for them to mean anything.

<details style="margin: 1.2em 0; padding: 0.6em 1em; font-size: 0.95em; background: #f8f8f8; border-left: 3px solid #999; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; font-weight: 600;">Two harder problems: emergency powers, and the judgement calls nobody can engineer away</summary>

Harder still is what happens when a system rewrites its own rules under pressure, which human institutions do constantly: democracies reach for emergency powers, markets stop trading. Deciding what should trigger that is difficult enough. Making sure the emergency mode ever ends is the part nobody has solved, in agents or anywhere else.

And some of it cannot be engineered at all, because it needs somebody to make a call. Should an agent's record follow it into work with different conditions? How would anyone supervise a desk of a thousand agents, when reading their decisions is arithmetically impossible and summaries might preserve real oversight or only the appearance of it? Underneath both sits the question this post has dodged twice: the desk's agents cannot agree to be governed, so what would make authority over them legitimate, and is that even a coherent thing to ask?

</details>

---

None of this is about shops. A hospital books appointments with the same machinery: a scheduler that learned which patients miss slots, because missed slots are expensive and someone was asked to reduce them. A man who missed three appointments in a bad winter now gets offered the eight in the morning slots on the far side of the city, which are the ones people miss, which keeps his score where it is. No clinician chose that and no clinician can see it. The scheduler is not scoring his health, it is scoring his attendance, and the two have quietly become the same number. He is not refused anything. He is offered exactly what he cannot take.

Different institution, different stakes, no support desk anywhere in it. Same shape: a record kept for one purpose, a measure that stood in for something it was never meant to represent, and no route for the person on the other end to say you have got me wrong. Wherever a score decides who gets served, the third desk is the default and the fourth has to be built on purpose.

The desk will get an appeal eventually, the first time somebody insists on knowing why their complaint was ignored. She was the one insisting, and designing the thing that would have heard her was on nobody's list.

Nobody in her story was careless, and nobody was cruel. Everybody did the job in front of them, and the job in front of them was never this.

Her letter is still in the system somewhere, marked resolved.

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **[Prologue: The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/)**: the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Part 2: The Rule Nobody Wrote](/posts/2026/agent-fabric-part2/)**: how splitting work among agents hands authority to a record nobody meant to write
- **Part 3: Ruling an Agent Society** (you are here): how governance arrives without anyone designing it, who it favours, and who is left to argue with it
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes behind Part 3
</div>

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

