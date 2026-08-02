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
wordcount: "~2,100 words"
---

{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** The anatomy of a single agent, a foundation model wrapped in scaffolding. The model operates in an iterative loop with an environment, acting on it and observing the result. The scaffolding gives it capabilities that let it act (prompt, tools, skills, memory) and constraints that direct it (planning, identity, self-evaluation)." >}}

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **Prologue: The Anatomy of an Agent** (you are here): the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Part 2: Delegation, and What It Costs](/posts/2026/agent-fabric-part2/)**: how work gets split among agents, and how splitting it quietly creates authority nobody granted
- **[Part 3: Ruling an Agent Society](/posts/2026/agent-fabric-part3/)**: governance archetypes, who benefits, adversarial dynamics, and who enforces the rules
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes behind Part 3
</div>

## From Model Call to Agent

Ask a chatbot to run a script and tidy the results into a folder, and it will tell you how. Confidently, in numbered steps, possibly with the exact command. What it will not do is run it. On its own a language model is a static function: text in, text out, and no way to perceive the world or change it.

Now watch a coding agent like Claude Code or OpenAI's Codex refactor a module. It reads the files, forms a plan, edits the code, runs the tests, sees them fail, edits again, runs again, and stops once they pass. Here is the part worth pausing on: **the model underneath is the same model.** Nobody made it smarter. Something was built around it, and that something is the difference between a thing that describes work and a thing that does it.

That gap does not close by making the model smarter. It closes by building things around it, and the things have a name: **scaffolding**. A prompt telling it what it is doing. Tools letting it act. Memory carrying what it learned from one step into the next. And a loop that keeps going until the work looks done. The model supplies the reasoning. The scaffolding is what turns reasoning into consequence.

Every piece of that is load-bearing, and the fastest way to see it is to pull pieces out. Without the loop you are back to a chatbot. Without tools you have something that can think and never touch anything. Memory is the strangest omission of the three, because an agent that loses it does not get dumber in any measurable way. It stays exactly as capable as before and walks into the same dead end every twenty minutes, with full confidence, forever.

## What the Loop Needs

Start with the acting. Not all of it is equally recoverable, and that turns out to matter more than how clever the agent is. A deleted file comes back from backup. A robot arm that has already swung cannot be unswung. In between sits everything else: reading and writing files is solved, calling a well-behaved service through something like <a href="https://modelcontextprotocol.io" target="_blank" rel="noopener">MCP</a> is solved, driving a browser is messy because pages change under you, and moving something physical is hardest of all. The same loop with the same model is a different proposition depending on which of those it can reach. (The primitives are called tools; the reusable procedures built from them are skills.)

Everything else in the scaffolding exists to keep the loop pointed somewhere. A prompt is how a human's goal gets in. Planning looks more than one move ahead. Identity narrows what the agent will even consider, which is why a coding assistant and a customer-service agent can run on the same model and behave nothing alike. And the loop does not have to wait to be called: it can watch a system and step in when something looks wrong, pause to ask a person to approve a plan, or run for an hour with nobody looking.

That last one deserves suspicion, because an agent judging its own work is the one component whose failure is invisible from the inside. It is also where single agents break, which the rest of this post is largely about.

## The Loop

The loop itself is simple. Perceive, reason, act, observe, go round again. What makes it powerful is not the shape but a property of it: the agent's own output becomes its input next time. When that coding agent runs the tests, the failures it reads are the ones its own last edit produced, so every pass is reacting to a world it just changed. That feedback is the whole trick, and it has been wrapped a hundred ways since <a href="https://arxiv.org/abs/2210.03629" target="_blank" rel="noopener">the plainest version</a> was written down, but the cycle underneath never changes.

## When the Loop Improves Itself

Turning the loop is not the same as getting better at anything. An agent that emails a news digest every morning runs its loop faithfully for years and is no more capable on the last day than the first. What changes is when the output of one pass becomes the input to the next, so the agent is not doing the task again but doing it on top of the last attempt. That is **recursive self-improvement**.

The tightest version happens inside a single task, where a coding agent runs the tests, reads the failures, and fixes the code, so every edit knows what the last one broke. Widen the window and something stranger happens: the agent writes itself a note after a failed attempt, and consults that note next time. Fifty tasks later it is working from a file of things it learned the hard way, and the version of itself that started has effectively been replaced. <a href="https://arxiv.org/abs/2303.11366" target="_blank" rel="noopener">This has been built</a>, and it works. Push it further and the notes stop being notes: procedures that keep working become <a href="https://arxiv.org/abs/2305.16291" target="_blank" rel="noopener">tools the agent can call</a>, so what it figured out in hour one is still paying off in hour fifty.

None of this is confined to software. The same loop has been pointed at <a href="https://arxiv.org/abs/2304.05376" target="_blank" rel="noopener">chemistry</a> and at <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">research itself</a>, though in most of those cases the agent is not improving alone: it proposes, and something later throws nearly all of it out.

## Where Single-Agent Recursion Breaks

Recursive self-improvement is clean in theory. In practice a lone loop runs into three kinds of trouble that better prompting does not fix, and they compound in a particular order.

It begins with arithmetic, and the arithmetic is brutal. Give an agent a task with twenty steps and let it be right ninety-five percent of the time at each one, which sounds good. It finishes the whole task correctly about a third of the time. Worse, mistakes are not independent: a wrong turn at step four narrows what step five can even attempt. Which is why agents still fall short of people on <a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">realistic computer and web tasks</a>, and why the <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">gap widens as the tasks get longer</a>.

The obvious fix is to have the agent check its own work, and this is where things get strange. Picture it debugging a failing test. It decides the bug is in the date handling, rewrites that, and the test still fails. Now ask it to review its own reasoning.

It does not say it looked in the wrong place. It says its analysis was correct but the fix was incomplete, and it is right about the first half, because the date handling really was a bit sloppy. So it goes deeper. It adds a normalization step, then a fallback, then a comment explaining the subtlety it believes it has found, and each pass makes the code more elaborate and the explanation more confident. Ask it how sure it is and the number goes up. The bug is in the timezone the test runs under, which it never considered on the first pass and now never will, because every pass has added another reason to believe the story it already has.

This is not one system's quirk. When researchers <a href="https://arxiv.org/abs/2310.01798" target="_blank" rel="noopener">asked models to revise their own reasoning</a> with nothing external to check against, the revisions often came out worse than the originals. A later <a href="https://arxiv.org/abs/2406.01297" target="_blank" rel="noopener">survey went looking</a> for a case of reliable self-correction that did not lean on some outside signal and could not find one. The mechanism meant to catch mistakes ends up laundering them into certainty.

Give the same agent more time and it does not escape, it plateaus. Put agents and human experts on the <a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">same long research and engineering problems</a> and the agents are ahead in the first couple of hours, then the humans overtake them and keep going. The agent exhausts the strategies available to it and settles into diminishing returns instead of stepping back and trying a different angle.

Underneath all three is the same root problem. One loop runs one set of habits, so the very patterns that produced a mistake are the ones deciding whether it was a mistake at all. Nothing here is a bug awaiting a patch; this is what improving in isolation does, and the agent cannot hold a position and attack it at the same time.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in Part 2" caption="**Figure 2.** A single agent can loop through a task alone, or an orchestrator can delegate the work to specialists. These are only two points in a much larger design space; Part 2 maps that space in detail, with dozens of delegation patterns grouped into families." >}}

## Beyond the Single Loop

Which leaves two ways out. One is a person at the moments that matter, less a safety brake than a way of keeping the agent pointed at what anyone actually wanted. The other is more than one agent, so the check comes from outside the thing being checked, and that is where **collective capability** enters: performance that belongs to the arrangement of agents rather than to any one of them.

Go back to the agent stuck on the date handling. Nothing about it needed to be smarter to escape that loop. It needed one other agent that had not spent the last hour convincing itself the date handling was the problem. Split the job: one writes, a second writes tests without seeing the first one's reasoning, a third tries to break the result. None is more capable than the original. But the second is not invested in the first one's mistake, and the third is rewarded for finding it, so the timezone gets checked by something that has no story to protect.

That is the whole idea, and it generalizes uncomfortably far: the thing that could not check itself is now checked by something with no stake in the answer.

Which raises the question the rest of the series is about: once you have several agents, somebody has to decide who does what, who checks whom, and whose judgement wins when two of them disagree. Those are questions about power, and they turn out to answer themselves whether or not anyone is paying attention. [Part 2](/posts/2026/agent-fabric-part2/) is how the work gets split. [Part 3](/posts/2026/agent-fabric-part3/) is who ends up in charge.

---

One line is worth carrying away from all of this. A loop cannot audit itself: an agent left alone with its own judgement does not converge on the truth, it converges on whatever it already believed, only louder. Everything else in this series follows from trying to fix that, and from what the fix costs.
