---
title: "The Anatomy of an Agent"
subtitle: "The loop, its limits, and why one agent is never enough"
date: 2026-06-15
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "LLM", "agent fabric"]
description: "The loop at the heart of an agent, the components that make each iteration effective, and where single-agent recursion hits structural limits that point toward the collective capability of many agents."
draft: false
math: false
ShowToc: false
TocOpen: false
hideCitation: false
wordcount: "~1,400 words"
---

<p style="font-size: 0.82em; color: #999; margin-top: 1em;"><a href="https://code.claude.com/docs/en/overview" target="_blank" rel="noopener" style="color: #999;">Claude Code</a> was used for editing and visualizations. All ideas and arguments are the authors' own.</p>

<div style="background: #fffbeb; border: 1px solid #f59e0b; border-radius: 6px; padding: 0.6em 1em; margin: 1.5em 0; font-size: 0.88em; color: #92400e;">
<strong>Early access.</strong> This blog series is a work in progress. Feedback, comments, and suggestions are welcome. Feel free to <a href="https://www.linkedin.com/in/kasra-hosseini/" target="_blank" rel="noopener" style="color: #92400e;">reach out on LinkedIn</a> or leave a comment at the bottom of the page.
</div>

## From Model Call to Agent

Ask a chatbot to run a script and tidy the results into a folder, and it will tell you how. Confidently, in numbered steps, possibly with the exact command. What it will not do is run it. A language model on its own is a very well-read person locked in a room with no hands: text in, text out, and no way to touch the thing being discussed.

Now watch a coding agent like Claude Code or OpenAI's Codex refactor a module. It reads the files, forms a plan, edits the code, runs the tests, sees them fail, edits again, runs again, and stops once they pass. Here is the part worth pausing on: **the model underneath is the same model.** Nobody made it smarter.

Something was built around it, and that something is the difference between describing work and doing it: instructions, tools, a memory that carries from one step to the next, and a loop that runs until the work looks done. None of it makes the model better at reasoning. It gives the reasoning a way to reach the world.


{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** Everything around the model in the middle exists for one reason: on its own it cannot reach anything." >}}

Take away the loop and it is a chatbot again. Take away memory and something stranger happens: it stays exactly as capable as it was, and walks into the same dead end every twenty minutes, with full confidence, forever.

## What the Loop Needs

Give an agent a filesystem tool it did not need and by lunchtime it has reorganised a directory nobody asked it to touch. That kind of wrong is loud: somebody notices by the afternoon, the agent loses the tool, and an hour of restoring files ends it.

The quiet one is the decision about when it is allowed to stop. Ask a coding agent to fix a bug, let it decide when it is finished, and it will tell you the work is done in the same confident tone whether it is or not, because it has no way to check that judgement except the judgement itself. Nothing announces the difference. If the worst it can reach is a file, somebody restores the file; if it has already sent the email or moved the money, there is nothing to restore.

## Where Single-Agent Recursion Breaks

Picture a coding agent debugging a failing test. It decides the bug is in the date handling, rewrites that, and the test still fails. So ask it to review its own reasoning. That is the obvious fix, and some systems are built entirely around it, <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">writing up their own research and then reviewing their own drafts</a>.

It does not say it looked in the wrong place. It says its analysis was correct but the fix was incomplete, which is half true, because the date handling really was sloppy. So it goes deeper: a normalization step, then a fallback, then a comment explaining the subtlety it believes it has found. Each pass makes it more certain. The bug is in the timezone the test runs under, which it never considered on the first pass and now never will, because every failed attempt has added another reason to believe the story it already has.


From outside it looks like diligence. The commit history shows steady work, each message more specific than the last, the explanations getting more detailed and more certain. If you were reviewing it you would see an agent closing in on something. What you would not see is that it stopped considering alternatives an hour ago, and that the confidence went up precisely because it kept failing. It cannot tell the difference between converging and digging.

The model is not being stupid. Every test it reads was broken by its own last edit, so it is arguing with a situation it keeps changing, and the wrong turn it took first narrows everything it can attempt after. No agent yet gets through <a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">ordinary desktop work at anything like the rate a person does</a>, and asked the same job twice, <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">it will not reliably do it the same way</a>.

The one on the timezone bug is still editing at hour six, nothing signalling that the search space collapsed before lunch. On a <a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">research problem that takes days</a> it leads a human expert for the first couple of hours, and then never catches up again.

The agent that made the mistake is the agent grading it. No prompt fixes that, because it is not a flaw in the prompt. It is a property of being alone in the room.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in the delegation patterns field guide" caption="**Figure 2.** The same job done alone and split three ways. The second arrangement is not smarter, just harder to fool." >}}

## Beyond the Single Loop

There are two ways out. Put a person at the moments that matter, or stop leaving one agent alone with the decision. Go back to the one stuck on the date handling: nothing about it needed to be smarter to escape that loop. It needed one other agent that had not spent the last hour convincing itself the date handling was the problem. So split the job three ways, with no more capability in any of them than the original had. The writer keeps the code. A second agent gets handed the failing test and nothing else, no theory, no history, none of the writer's six hours. A third is told to break whatever comes out.

That second agent does the obvious thing nobody had done in six hours. It runs the test twice, in two timezones, and watches it pass once.

That is the whole fix. It was available the entire time, and the writer could not see it, because by hour two it was no longer looking for the bug. It was looking for more evidence about the date handling.

Nobody decided the second agent should be the one to catch it. It caught it because it had not spent six hours being wrong.

The arrangement costs something of its own. The writer insists the test was unfair; the agent that ran it insists otherwise. Nothing in the setup can settle that, because nobody built anything to settle it. What settles it is which of the two the system has had to restart less often, because that number happens to exist. Nobody wrote that down as a rule about whose word beats whose. It is one anyway.

Six hours of getting more certain and no closer all looked like work: files changing, tests running, commit messages getting more specific. An agent alone with its own judgement does not converge on the truth. It converges on whatever it already believed, only louder.

Giving it hands did not fix that. It raised the stakes, because the well-read person locked in a room could at least be argued with. This one acts while it is being wrong.

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a four-post series on why and how AI agents may form societies and what it means for us. The first two are read front to back; the last two are catalogues to search.

- **The Anatomy of an Agent** (you are here): the loop at the heart of a single agent, and where single-agent recursion breaks
- **[The Agent Fabric: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns, from Supervisor to Market
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes, and who ends up enforcing them
</div>
