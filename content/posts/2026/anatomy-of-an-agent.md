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
wordcount: "~1,550 words"
---


## From Model Call to Agent

Ask a chatbot to run a script and tidy the results into a folder, and it will tell you how. Confidently, in numbered steps, possibly with the exact command. What it will not do is run it. A language model on its own is a very well-read person locked in a room with no hands: text in, text out, and no way to touch the thing being discussed.

Now watch a coding agent like Claude Code or OpenAI's Codex refactor a module. It reads the files, forms a plan, edits the code, runs the tests, sees them fail, edits again, runs again, and stops once they pass. Here is the part worth pausing on: **the model underneath is the same model.** Nobody made it smarter. Something was built around it, and that something is the difference between a thing that describes work and a thing that does it.

The gap does not close by making the model smarter. It closes by building things around it: something telling it what it is doing, tools letting it act, memory carrying what it learned from one step into the next, and a loop that runs until the work looks done. The industry calls that apparatus scaffolding, and the thing worth noticing is that none of it makes the model any cleverer. It just gives the cleverness somewhere to land.

{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** Everything around the model in the middle exists for one reason: on its own it cannot reach anything." >}}

Remove the loop and it is a chatbot again; remove the tools and it can think without touching anything. Memory is the one that surprises people. An agent that loses it does not get dumber in any measurable way: it stays exactly as capable as before and walks into the same dead end every twenty minutes, with full confidence, forever.

## What the Loop Needs

Every piece of that apparatus is something a person chose, and most of those choices announce themselves when they are wrong. Give an agent the wrong tools and it fails at lunchtime. Give it the wrong instructions and it spends an afternoon doing something competent that nobody asked for. One choice does not announce anything: ask a coding agent to fix a bug, let it decide when it is finished, and it will tell you the work is done in the same confident tone whether it is or not, because it has no way to check that judgement except the judgement itself.

The stakes of getting those choices wrong are set by what you let it reach. Deleting the wrong file costs an afternoon and a restore from backup; a robot arm that has already swung costs something else, and no amount of apologising unswings it. Reading files is safe, calling a well-behaved service is safe, driving a browser is merely messy because pages change underneath you. Same loop, same model, and how much you should worry depends entirely on which of those somebody handed it.

Even its character is one of those choices. Tell one agent it is careful and terse and another that it is patient and kind, on the same model, and the first answers an angry customer with three sentences and a policy link while the second asks what happened and takes four exchanges to get there. Neither is more capable. Most of what feels like character is a sentence somebody typed, which means so is most of what an agent will refuse to do, and so is how confident it sounds when it tells you the bug is fixed.

## Where Single-Agent Recursion Breaks

Picture a coding agent debugging a failing test. It decides the bug is in the date handling, rewrites that, and the test still fails. Now ask it to review its own reasoning, which is the obvious fix and what some systems are built to do, right up to <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">writing up their own research and then reviewing their own drafts</a>.

It does not say it looked in the wrong place. It says its analysis was correct but the fix was incomplete, and it is right about the first half, because the date handling really was a bit sloppy. So it goes deeper. It adds a normalization step, then a fallback, then a comment explaining the subtlety it believes it has found, and each pass makes the code more elaborate and the explanation more confident. Ask it how sure it is and the number goes up. The bug is in the timezone the test runs under, which it never considered on the first pass and now never will, because every failed attempt has added another reason to believe the story it already has.

Sit with that, because it is worse than being wrong. A person who cannot find a bug eventually starts doubting their own theory. This does the opposite: the confidence rises precisely because the fixes keep failing.

None of this is a shortfall in the model. It starts with the fact that the agent's own output becomes its input next time round: the failures it reads are the ones its own last edit produced, so it is not solving a problem so much as negotiating with a situation it keeps changing. And the errors multiply rather than add. Twenty steps, each one nearly always right, and the whole task comes out correct about a third of the time, because a wrong turn at step four narrows what step five can even attempt. Which is why agents still fall short of people on <a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">realistic computer and web tasks</a>, and why the <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">gap widens as the tasks get longer</a>.

From outside it looks like diligence. The commit history shows steady work, each message more specific than the last, the explanations getting more detailed and more certain. If you were reviewing it you would see an agent closing in on something. What you would not see is that it stopped considering alternatives an hour ago, and that the confidence went up precisely because it kept failing. It cannot tell the difference between converging and digging.

Give the same agent more time and it does not escape, it plateaus. The one on the timezone bug will still be editing at hour six, with files changing and tests running and nothing signalling that the search space collapsed hours ago. On <a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">long research problems</a> agents lead human experts for the first couple of hours. Then the humans overtake them and keep going, and the agents are still working.

Compounding error, rising confidence, and the plateau all come back to one fact: the agent that made the mistake is the agent grading it. Nothing about that improves with a better prompt, because it is not a shortfall in the model. It is a property of being alone in the room.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in Part 2" caption="**Figure 2.** The same job done alone and split three ways. The second arrangement is not smarter, just harder to fool." >}}

## Beyond the Single Loop

There are only two ways out of this, and one of them is a person at the moments that matter. The other is more than one agent. Go back to the one stuck on the date handling: nothing about it needed to be smarter to escape that loop. It needed one other agent that had not spent the last hour convincing itself the date handling was the problem. Split the job, so one writes, a second writes tests without seeing the first one's reasoning, and a third tries to break the result. None is more capable than the original. But the second is not invested in the first one's mistake, and the third is rewarded for finding it, so the timezone gets checked by something with no story to protect.

Which is where this stops being a question about agents and becomes a question about arrangements. Three agents checking each other is already a small institution: somebody decides who does what, somebody decides who checks whom, and when two of them disagree something has to settle it. Nobody sits down to design that. It gets settled anyway, by whichever agent happens to be trusted, and trust here is just a number somebody started recording.


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
