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

The gap does not close by making the model smarter. It closes by building things around it: something telling it what it is doing, tools letting it act, memory carrying what it learned from one step into the next, and a loop that runs until the work looks done. That apparatus is called **scaffolding**, and none of it is the model.

{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** Everything outside the model in the middle is scaffolding. None of it makes the model smarter, and all of it is what lets the model act." >}}

Remove the loop and it is a chatbot again. Remove the tools and it can think without touching anything. But remove its memory and something stranger happens: it does not get dumber in any measurable way. It stays exactly as capable as before and walks into the same dead end every twenty minutes, with full confidence, forever.

## What the Loop Needs

Watch one piece fail and the rest explains itself. Ask a coding agent to fix a bug, let it decide it is finished, and it will tell you the work is done in the same confident tone whether it is or not. It has no way to check that judgement except the judgement itself. That single component, the agent grading its own work, is where everything in this post goes wrong, and it is worth noticing how ordinary the other pieces are by comparison.

Start with the acting, because not all of it is equally recoverable and that matters more than how clever the agent is. A deleted file comes back from backup. A robot arm that has already swung cannot be unswung. Reading files and calling a well-behaved service are solved problems, driving a browser is messy because pages change under you, and moving something physical is hardest of all. The same loop with the same model is a different proposition depending on which of those it can reach.

The other pieces fail in more ordinary ways. Take away the prompt and a capable agent does something competent that nobody asked for. Take away planning and it solves the step in front of it at the expense of the one after, cheerfully deleting the file it needs in twenty minutes. Those are annoying. The self-grading failure is different in kind, because it is the only one the agent cannot notice from the inside.

Identity does more work than it looks like. Tell one agent it is careful and terse, tell another it is patient and kind, run both on the same model, and put the same angry customer in front of them. They behave like different species. Most of what feels like an agent's character is a sentence somebody typed, which means most of what it will refuse to do is also a sentence somebody typed, and somebody can change it.

## Where Single-Agent Recursion Breaks

Here is the property that makes all of this compound: the agent's own output becomes its input next time round. When the coding agent runs the tests, the failures it reads are the ones its own last edit produced, so it is not solving a problem so much as negotiating with a situation it keeps changing. Some agents go further and keep notes on their own failures. One that wasted forty minutes on a test failing because the database was not running writes down that lesson, reads it next time, and does not waste the forty minutes again. That is the difference between turning the loop and getting better at something, and it is also where a lone loop runs into three kinds of trouble that better prompting does not fix. They arrive in a particular order.

It begins with arithmetic, and the arithmetic is brutal. Give an agent a task with twenty steps and let it be right ninety-five percent of the time at each one, which sounds good. It finishes the whole task correctly about a third of the time. Worse, mistakes are not independent: a wrong turn at step four narrows what step five can even attempt. Which is why agents still fall short of people on <a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">realistic computer and web tasks</a>, and why the <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">gap widens as the tasks get longer</a>.

The obvious fix is to have the agent check its own work, and some systems are built to do exactly that, right up to <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">writing up their own research and then reviewing their own drafts</a>. This is where things get strange. Picture it debugging a failing test. It decides the bug is in the date handling, rewrites that, and the test still fails. Now ask it to review its own reasoning.

It does not say it looked in the wrong place. It says its analysis was correct but the fix was incomplete, and it is right about the first half, because the date handling really was a bit sloppy. So it goes deeper. It adds a normalization step, then a fallback, then a comment explaining the subtlety it believes it has found, and each pass makes the code more elaborate and the explanation more confident. Ask it how sure it is and the number goes up. The bug is in the timezone the test runs under, which it never considered on the first pass and now never will.

Sit with that, because it is worse than being wrong. A person who cannot find a bug eventually starts doubting their own theory. This does the opposite: every failed pass gets absorbed as evidence that the problem is deeper than it thought, and the theory comes out stronger. There is no number of failed attempts that would make it look somewhere else, and this is not one system's quirk: <a href="https://arxiv.org/abs/2310.01798" target="_blank" rel="noopener">models asked to revise their own reasoning</a> with nothing external to check against tend to come out worse, and nobody has <a href="https://arxiv.org/abs/2406.01297" target="_blank" rel="noopener">found an exception</a> that does not lean on an outside signal.

From outside it looks like diligence. The commit history shows steady work, each message more specific than the last, the explanations getting more detailed and more certain. If you were reviewing it you would see an agent closing in on something. What you would not see is that it stopped considering alternatives an hour ago, and that the confidence went up precisely because it kept failing. That is the shape of the problem: not an agent that gives up, but one that cannot tell the difference between converging and digging.

Give the same agent more time and it does not escape, it plateaus. Hour one it tries three approaches; hour eight it is adjusting parameters inside a variation of the one it liked, and the work still looks like work. Files change, tests run, output accumulates, and nothing signals that the search space collapsed six hours ago. Put agents and human experts on the <a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">same long research problems</a> and the agents lead for the first couple of hours, then the humans overtake them and keep going.

All three come back to the same thing. One loop runs one set of habits, so the patterns that produced a mistake are the patterns judging whether it was a mistake. The agent cannot hold a position and attack it at the same time, and no amount of prompting changes that, because it is not a shortfall in the model. It is a property of being alone.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in Part 2" caption="**Figure 2.** One agent working alone, and the same job split between three. The second arrangement is not smarter. It is just harder to fool." >}}

## Beyond the Single Loop

There are only two ways out of this, and one of them is a person at the moments that matter. The other is more than one agent. Go back to the one stuck on the date handling: nothing about it needed to be smarter to escape that loop. It needed one other agent that had not spent the last hour convincing itself the date handling was the problem. Split the job, so one writes, a second writes tests without seeing the first one's reasoning, and a third tries to break the result. None is more capable than the original. But the second is not invested in the first one's mistake, and the third is rewarded for finding it, so the timezone gets checked by something with no story to protect.

Which is where this stops being a question about agents and becomes a question about arrangements. Three agents checking each other is already a small institution: somebody decides who does what, somebody decides who checks whom, and when two of them disagree something has to settle it. Nobody sits down to design that. It gets settled anyway, by whichever agent happens to be trusted, and trust here is just a number somebody started recording.

---

So here is the one line worth carrying out of this. A loop cannot audit itself. An agent left alone with its own judgement does not converge on the truth; it converges on whatever it already believed, only louder. Giving it hands did not fix that, it raised the stakes, because the well-read person locked in a room could at least be argued with. This one acts while it is being wrong.

Everything that follows in this series is an attempt to fix that by putting more than one agent in the room, and every fix charges for it. [Part 1](/posts/2026/agent-fabric-part1/) is why they end up in groups at all. [Part 2](/posts/2026/agent-fabric-part2/) is how the work gets split, with a [field guide](/posts/2026/delegation-patterns-field-guide/) to the arrangements. [Part 3](/posts/2026/agent-fabric-part3/) is who ends up in charge, with [its own](/posts/2026/governance-archetypes-field-guide/).

