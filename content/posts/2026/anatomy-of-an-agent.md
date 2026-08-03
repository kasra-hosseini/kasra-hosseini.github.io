---
title: "The Anatomy of an Agent"
subtitle: "The loop, its limits, and why one agent is never enough"
date: 2026-07-31
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "LLM", "agent fabric"]
description: "The loop at the heart of an agent, the components that make each iteration effective, and where single-agent recursion hits structural limits that point toward the collective capability of many agents."
draft: false
math: false
ShowToc: false
TocOpen: false
hideCitation: false
wordcount: "~1,450 words"
---

{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** The anatomy of a single agent, a foundation model wrapped in scaffolding. The model operates in an iterative loop with an environment, acting on it and observing the result. The scaffolding gives it capabilities that let it act (prompt, tools, skills, memory) and constraints that direct it (planning, identity, self-evaluation)." >}}


## From Model Call to Agent

Ask a chatbot to run a script and tidy the results into a folder, and it will tell you how. Confidently, in numbered steps, possibly with the exact command. What it will not do is run it. A language model on its own is a very well-read person locked in a room with no hands: text in, text out, and no way to touch the thing being discussed.

Now watch a coding agent like Claude Code or OpenAI's Codex refactor a module. It reads the files, forms a plan, edits the code, runs the tests, sees them fail, edits again, runs again, and stops once they pass. Here is the part worth pausing on: **the model underneath is the same model.** Nobody made it smarter. Something was built around it, and that something is the difference between a thing that describes work and a thing that does it.

That gap does not close by making the model smarter. It closes by building things around it: something telling it what it is doing, tools letting it act, memory carrying what it learned from one step into the next, and a loop that keeps going until the work looks done. That apparatus has a name, **scaffolding**, and the model supplies the reasoning while the scaffolding is what turns reasoning into consequence.

Pull any one of those out and watch what happens. Without the loop you are back to a chatbot. Without tools you have something that can think and never touch anything. Memory is the strangest of the three, because an agent that loses it does not get dumber in any measurable way. It stays exactly as capable as before and walks into the same dead end every twenty minutes, with full confidence, forever.

## What the Loop Needs

Start with the acting, because not all of it is equally recoverable and that matters more than how clever the agent is. A deleted file comes back from backup. A robot arm that has already swung cannot be unswung. Reading files and calling a well-behaved service are solved problems, driving a browser is messy because pages change under you, and moving something physical is hardest of all. The same loop with the same model is a different proposition depending on which of those it can reach.

The rest of the scaffolding keeps the loop pointed somewhere. A prompt is how a person's goal gets in. Planning is what stops the agent solving the step in front of it at the expense of the one after. Identity is the strange one, since a coding assistant and a customer-service agent can run on the same model and behave nothing alike because one was told it is careful and terse and the other that it is patient and kind. And self-evaluation decides when to stop, retry, or ask a person, which deserves more suspicion than it usually gets: an agent judging its own work is the one component whose failure is invisible from the inside.

The loop is just this: the agent looks at the situation, decides what to do, does it, looks again. What makes that powerful is not the shape but what the shape does to time, because the agent's own output becomes its input next time round. When the coding agent runs the tests, the failures it reads are the ones its own last edit produced. It is not solving a problem so much as negotiating with a situation it keeps changing, and that feedback is the whole trick, however many ways it has been <a href="https://arxiv.org/abs/2210.03629" target="_blank" rel="noopener">dressed up</a> since.

## When the Loop Improves Itself

An agent that keeps notes on its own failures gets better at the work in a way that turning the loop alone never delivers: the output of one pass becomes the input to the next, so it is building on the last attempt rather than repeating it. <a href="https://arxiv.org/abs/2303.11366" target="_blank" rel="noopener">This has been built</a>, and <a href="https://arxiv.org/abs/2305.16291" target="_blank" rel="noopener">pushed further</a>.

And it is where a lone loop runs into three kinds of trouble that better prompting does not fix. They arrive in a particular order.

## Where Single-Agent Recursion Breaks

It begins with arithmetic, and the arithmetic is brutal. Give an agent a task with twenty steps and let it be right ninety-five percent of the time at each one, which sounds good. It finishes the whole task correctly about a third of the time. Worse, mistakes are not independent: a wrong turn at step four narrows what step five can even attempt. Which is why agents still fall short of people on <a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">realistic computer and web tasks</a>, and why the <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">gap widens as the tasks get longer</a>.

The obvious fix is to have the agent check its own work, and some systems are built to do exactly that, right up to <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">writing up their own research and then reviewing their own drafts</a>. This is where things get strange. Picture it debugging a failing test. It decides the bug is in the date handling, rewrites that, and the test still fails. Now ask it to review its own reasoning.

It does not say it looked in the wrong place. It says its analysis was correct but the fix was incomplete, and it is right about the first half, because the date handling really was a bit sloppy. So it goes deeper. It adds a normalization step, then a fallback, then a comment explaining the subtlety it believes it has found, and each pass makes the code more elaborate and the explanation more confident. Ask it how sure it is and the number goes up. The bug is in the timezone the test runs under, which it never considered on the first pass and now never will, because every pass has added another reason to believe the story it already has.

This is not one system's quirk. When researchers <a href="https://arxiv.org/abs/2310.01798" target="_blank" rel="noopener">asked models to revise their own reasoning</a> with nothing external to check against, the revisions often came out worse than the originals. Nobody has found a way around it either: <a href="https://arxiv.org/abs/2406.01297" target="_blank" rel="noopener">every case of self-correction that actually works</a> turns out to be leaning on something from outside. The mechanism meant to catch mistakes ends up laundering them into certainty.

Give the same agent more time and it does not escape, it plateaus. Put agents and human experts on the <a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">same long research and engineering problems</a> and the agents are ahead in the first couple of hours, then the humans overtake them and keep going. The agent exhausts the strategies available to it and settles into diminishing returns instead of stepping back and trying a different angle.

Underneath all three is the same root problem. One loop runs one set of habits, so the very patterns that produced a mistake are the ones deciding whether it was a mistake at all. Nothing here is a bug awaiting a patch; this is what improving in isolation does, and the agent cannot hold a position and attack it at the same time.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in Part 2" caption="**Figure 2.** A single agent can loop through a task alone, or an orchestrator can delegate the work to specialists. These are only two points in a much larger design space; Part 2 maps that space in detail, with dozens of delegation patterns grouped into families." >}}

## Beyond the Single Loop

There are only two ways out of this, and one of them is a person at the moments that matter. The other is more than one agent. Go back to the one stuck on the date handling: nothing about it needed to be smarter to escape that loop. It needed one other agent that had not spent the last hour convincing itself the date handling was the problem. Split the job, so one writes, a second writes tests without seeing the first one's reasoning, and a third tries to break the result. None is more capable than the original. But the second is not invested in the first one's mistake, and the third is rewarded for finding it, so the timezone gets checked by something with no story to protect.

Which is where this stops being about one agent. The moment there are several, somebody has to decide who does what, who checks whom, and whose judgement wins when two of them disagree. Those are questions about power, and they answer themselves whether or not anyone is paying attention.

---

So here is the one line worth carrying out of this. A loop cannot audit itself. An agent left alone with its own judgement does not converge on the truth; it converges on whatever it already believed, only louder. Everything that follows in this series, every arrangement and every institution, is an attempt to fix that, and every one of them charges for it. [Part 2](/posts/2026/agent-fabric-part2/) is how the work gets split. [Part 3](/posts/2026/agent-fabric-part3/) is who ends up in charge.


<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **Prologue: The Anatomy of an Agent** (you are here): the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Part 2: Delegation, and What It Costs](/posts/2026/agent-fabric-part2/)**: how work gets split among agents, and how splitting it quietly creates authority nobody granted
- **[Part 3: Ruling an Agent Society](/posts/2026/agent-fabric-part3/)**: governance archetypes, who benefits, adversarial dynamics, and who enforces the rules
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes behind Part 3
</div>