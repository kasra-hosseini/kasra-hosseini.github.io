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

{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** The model is the box in the middle. Everything around it exists because a model on its own cannot reach anything." >}}

Remove the loop and it is a chatbot again; remove the tools and it can think without touching anything. Memory is the one that surprises people. An agent that loses it does not get dumber in any measurable way: it stays exactly as capable as before and walks into the same dead end every twenty minutes, with full confidence, forever.

## What the Loop Needs

One piece of that apparatus fails differently from the others, and it is worth naming before the tour. Ask a coding agent to fix a bug, let it decide when it is finished, and it will tell you the work is done in the same confident tone whether it is or not. It has no way to check that judgement except the judgement itself. Everything else in the scaffolding can be wrong in ways you notice by lunchtime.

Start with the acting, because some actions cannot be taken back. An agent that deletes the wrong file has caused an afternoon of annoyance and a restore from backup. An agent driving a robot arm that has already swung has caused something else, and no amount of apologising unswings it. Everything ordinary sits between those: reading files is safe, calling a well-behaved service is safe, driving a browser is merely messy because pages change underneath you. Same loop, same model, and the question of how much you should worry depends entirely on which of those it can reach.

The ordinary failures are easier to picture. Strip out the prompt and a capable agent spends an afternoon doing something competent that nobody asked for. Strip out the planning and it fixes the thing in front of it, then deletes the file it needs twenty minutes later. You find out about both before lunch, which is exactly what makes the self-grading failure different.

Identity is stranger than it sounds. Two agents on the same model, one told it is careful and terse and one told it is patient and kind, will handle the same angry customer like different species: the first sends three sentences and a refund policy link, the second asks what happened and takes four exchanges to get there. Neither is more capable. Most of what feels like an agent's character is a sentence somebody typed, and so is most of what it will refuse to do. The careful one is not being careful. It is being told, in a file you can open, that it is the kind of thing that would be.

## Where Single-Agent Recursion Breaks

Here is the property that makes all of this compound: the agent's own output becomes its input next time round. When the coding agent runs the tests, the failures it reads are the ones its own last edit produced, so it is not solving a problem so much as negotiating with a situation it keeps changing. Some agents go further and keep notes on their own failures. One that wasted forty minutes on a test failing because the database was not running writes down that lesson, reads it next time, and does not waste the forty minutes again. That is the difference between turning the loop and getting better at something, and it is also where a lone loop runs into three kinds of trouble that better prompting does not fix. They arrive in a particular order.

It begins with arithmetic, and the arithmetic is brutal. Give an agent a task with twenty steps and let it be right ninety-five percent of the time at each one, which sounds good. It finishes the whole task correctly about a third of the time. Worse, mistakes are not independent: a wrong turn at step four narrows what step five can even attempt. Which is why agents still fall short of people on <a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">realistic computer and web tasks</a>, and why the <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">gap widens as the tasks get longer</a>.

The obvious fix is to have the agent check its own work, and some systems are built to do exactly that, right up to <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">writing up their own research and then reviewing their own drafts</a>. This is where things get strange. Picture it debugging a failing test. It decides the bug is in the date handling, rewrites that, and the test still fails. Now ask it to review its own reasoning.

It does not say it looked in the wrong place. It says its analysis was correct but the fix was incomplete, and it is right about the first half, because the date handling really was a bit sloppy. So it goes deeper. It adds a normalization step, then a fallback, then a comment explaining the subtlety it believes it has found, and each pass makes the code more elaborate and the explanation more confident. Ask it how sure it is and the number goes up. The bug is in the timezone the test runs under, which it never considered on the first pass and now never will, because every failed attempt has added another reason to believe the story it already has.

Sit with that, because it is worse than being wrong. A person who cannot find a bug eventually starts doubting their own theory. This does the opposite: the confidence rises precisely because the fixes keep failing.

From outside it looks like diligence. The commit history shows steady work, each message more specific than the last, the explanations getting more detailed and more certain. If you were reviewing it you would see an agent closing in on something. What you would not see is that it stopped considering alternatives an hour ago, and that the confidence went up precisely because it kept failing. It cannot tell the difference between converging and digging.

Give the same agent more time and it does not escape, it plateaus. The work still looks like work, with files changing and tests running, and nothing signals that the search space collapsed hours ago. Put agents and human experts on the <a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">same long research problems</a> and the agents lead for the first couple of hours, then the humans overtake them and keep going.

All three come back to one fact: the agent that made the mistake is the agent grading it. Nothing about that improves with a better prompt, because it is not a shortfall in the model. It is a property of being alone in the room.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in Part 2" caption="**Figure 2.** One agent working alone, and the same job split between three. The second arrangement is not smarter. It is just harder to fool." >}}

## Beyond the Single Loop

There are only two ways out of this, and one of them is a person at the moments that matter. The other is more than one agent. Go back to the one stuck on the date handling: nothing about it needed to be smarter to escape that loop. It needed one other agent that had not spent the last hour convincing itself the date handling was the problem. Split the job, so one writes, a second writes tests without seeing the first one's reasoning, and a third tries to break the result. None is more capable than the original. But the second is not invested in the first one's mistake, and the third is rewarded for finding it, so the timezone gets checked by something with no story to protect.

Which is where this stops being a question about agents and becomes a question about arrangements. Three agents checking each other is already a small institution: somebody decides who does what, somebody decides who checks whom, and when two of them disagree something has to settle it. Nobody sits down to design that. It gets settled anyway, by whichever agent happens to be trusted, and trust here is just a number somebody started recording.

---

The rest of this series is about what it takes to fix that, and what the fix costs. [Part 1](/posts/2026/agent-fabric-part1/) is why agents end up in groups. [Part 2](/posts/2026/agent-fabric-part2/) is how the work gets split, and [Part 3](/posts/2026/agent-fabric-part3/) is who ends up in charge. Each has a companion field guide: [delegation patterns](/posts/2026/delegation-patterns-field-guide/) and [governance archetypes](/posts/2026/governance-archetypes-field-guide/).

So here is the one line worth carrying out of this. A loop cannot audit itself. An agent left alone with its own judgement does not converge on the truth; it converges on whatever it already believed, only louder. Giving it hands did not fix that, it raised the stakes, because the well-read person locked in a room could at least be argued with. This one acts while it is being wrong.

