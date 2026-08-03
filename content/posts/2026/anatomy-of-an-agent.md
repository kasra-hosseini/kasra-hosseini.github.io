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
wordcount: "~1,300 words"
---


## From Model Call to Agent

Ask a chatbot to run a script and tidy the results into a folder, and it will tell you how. Confidently, in numbered steps, possibly with the exact command. What it will not do is run it. A language model on its own is a very well-read person locked in a room with no hands: text in, text out, and no way to touch the thing being discussed.

Now watch a coding agent like Claude Code or OpenAI's Codex refactor a module. It reads the files, forms a plan, edits the code, runs the tests, sees them fail, edits again, runs again, and stops once they pass. Here is the part worth pausing on: **the model underneath is the same model.** Nobody made it smarter.

Something was built around it, and that something is the difference between a thing that describes work and a thing that does it: something telling it what it is doing, tools letting it act, memory carrying what it learned from one step into the next, and a loop that runs until the work looks done. The industry calls that apparatus scaffolding. None of it makes the model cleverer; it just gives the cleverness somewhere to land.


{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** Everything around the model in the middle exists for one reason: on its own it cannot reach anything." >}}

Take away the loop and it is a chatbot again. Take away memory and something stranger happens: it does not get dumber in any measurable way, it stays exactly as capable as before and walks into the same dead end every twenty minutes, with full confidence, forever.

## What the Loop Needs

Every part of that apparatus is a decision somebody made: which tools it gets, what it is told to do, when it is allowed to stop. Get most of them wrong and you find out by lunchtime. One of them announces nothing at all.

Ask a coding agent to fix a bug, let it decide when it is finished, and it will tell you the work is done in the same confident tone whether it is or not, because it has no way to check that judgement except the judgement itself, and that tone was typed by somebody too. How much that matters depends on what somebody let it reach: deleting the wrong file costs an afternoon and a restore from backup, and a robot arm that has already swung costs something no apology unswings.



## Where Single-Agent Recursion Breaks

Picture a coding agent debugging a failing test. It decides the bug is in the date handling, rewrites that, and the test still fails. Now ask it to review its own reasoning, which is the obvious fix and what some systems are built to do, right up to <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">writing up their own research and then reviewing their own drafts</a>.

It does not say it looked in the wrong place. It says its analysis was correct but the fix was incomplete, and it is right about the first half, because the date handling really was a bit sloppy. So it goes deeper. It adds a normalization step, then a fallback, then a comment explaining the subtlety it believes it has found, and each pass makes the code more elaborate and the explanation more confident. Ask it how sure it is and the number goes up. The bug is in the timezone the test runs under, which it never considered on the first pass and now never will, because every failed attempt has added another reason to believe the story it already has.


None of this is a shortfall in the model. Every test it reads was broken by its own last edit, so it is not solving a problem so much as arguing with a situation it keeps changing. And a wrong turn early narrows what every later step can even attempt, which is why <a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">nobody has an agent that can reliably use a computer all day</a>, and why <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">the longer the job runs, the further behind a person it falls</a>.

From outside it looks like diligence. The commit history shows steady work, each message more specific than the last, the explanations getting more detailed and more certain. If you were reviewing it you would see an agent closing in on something. What you would not see is that it stopped considering alternatives an hour ago, and that the confidence went up precisely because it kept failing. It cannot tell the difference between converging and digging.

More time does not help; it plateaus. The one on the timezone bug will still be editing at hour six, files changing and tests running, nothing signalling that the search space collapsed hours ago. Put agents and human experts on the <a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">same long research problems</a> and the agents lead for the first couple of hours, then the humans overtake them and keep going while the agents are still working.

All three come back to one fact: the agent that made the mistake is the agent grading it. Nothing about that improves with a better prompt, because it is not a shortfall in the model. It is a property of being alone in the room.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in Part 2" caption="**Figure 2.** The same job done alone and split three ways. The second arrangement is not smarter, just harder to fool." >}}

## Beyond the Single Loop

There are only two ways out of this, and one of them is a person at the moments that matter. The other is more than one agent. Go back to the one stuck on the date handling: nothing about it needed to be smarter to escape that loop. It needed one other agent that had not spent the last hour convincing itself the date handling was the problem. So split the job: one writes, a second writes tests without seeing the first one's reasoning, and a third tries to break the result. None is more capable than the original, and that is the point. The second agent, handed only the failing test and no theory about it, does the obvious thing nobody had done in six hours: runs it twice, in two timezones, and watches it pass once. That is the whole fix. It was available the entire time, and the first agent could not see it, because by hour two it was no longer looking for the bug. It was looking for more evidence about the date handling.

Nobody on that team decided the second agent should be the one to catch it. It caught it because it had not spent six hours being wrong, which is a fact about the arrangement rather than about the agent. And three agents checking each other is already a small institution: when two of them disagree, something has to settle it, and nobody sits down to design that part. It gets settled by whichever agent happens to be trusted, and trust here is a number somebody started recording.


So here is the one line worth carrying out of this. A loop cannot audit itself. An agent left alone with its own judgement does not converge on the truth; it converges on whatever it already believed, only louder. Giving it hands did not fix that, it raised the stakes, because the well-read person locked in a room could at least be argued with. This one acts while it is being wrong.

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **Prologue: The Anatomy of an Agent** (you are here): the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Part 2: Delegation, and What It Costs](/posts/2026/agent-fabric-part2/)**: how work gets split among agents, and how splitting it quietly creates authority nobody granted
- **[Part 3: Ruling an Agent Society](/posts/2026/agent-fabric-part3/)**: governance archetypes, who benefits, who enforces the rules, and how a society gets argued with
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes behind Part 3
</div>
