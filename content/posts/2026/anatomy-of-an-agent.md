---
title: "The Anatomy of an Agent"
subtitle: "The loop, its limits, and why one agent is never enough"
date: 2026-06-27
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "LLM", "agent fabric"]
description: "The loop at the heart of an agent, the components that make each iteration effective, and where single-agent recursion hits structural limits that point toward the collective capability of many agents."
draft: false
math: false
ShowToc: false
TocOpen: false
hideCitation: false
wordcount: "~2,000 words"
---

{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** The anatomy of a single agent, a foundation model wrapped in scaffolding. The model operates in an iterative loop with an environment, acting on it and observing the result. The scaffolding gives it capabilities that let it act (prompt, tools, skills, memory) and constraints that direct it (planning, identity, self-evaluation)." >}}

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **Prologue: The Anatomy of an Agent** (you are here): the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis, and the path from isolation to interweaving
- **[Part 2: Division of Labour and Governance](/posts/2026/agent-fabric-part2/)**: delegation archetypes, trust and authority, the specialist market, and governance archetypes
</div>

## From Model Call to Agent

Ask a bare language model to "run train.py and put the results in the output folder" and the honest answer is that it cannot. It can only produce text about doing so. On its own a foundation model is a static function: it can neither perceive the world nor change it. Now watch a coding agent like Claude Code or OpenAI's Codex refactor a module. It reads the files, forms a plan, edits the code, runs the tests, sees them fail, edits again, runs again, and stops once they pass. The model underneath is the same. The behaviour is not, because now it can act on the world and see what happens.

What closes that gap is not a better model. It is architecture. An agent is a foundation model plus the **scaffolding** built around it: a prompt telling it what it is doing, tools letting it act, memory carrying lessons from one step to the next, and a loop that keeps going until its own judgement says the work is done. The model is the reasoning core, and the scaffolding is what converts reasoning into action. Everything the rest of this series claims about societies and governance is built on this one unit, which is why it is worth taking apart properly.

Every piece of that is load-bearing, which is easiest to see by removing them. Take away the loop and you have a chatbot again. Take away the tools and you have something that can think but never touch anything. Memory is the interesting one: strip it out and the agent stays fully capable while losing the ability to learn from the last five minutes, so it walks into the same dead end all afternoon.

## What the Loop Needs

Start with the ability to act, because without it nothing else matters. Tools are the primitives: read a file, call an API. Skills are reusable procedures that string those primitives into a strategy, and the <a href="https://agentskills.io" target="_blank" rel="noopener">Agent Skills</a> open specification gives them a portable format any agent can load.

Not all acting is equally safe, though, and the difference tracks how directly a tool reaches into the real world. Reading and writing files is a solved problem, and so is calling a well-behaved service through something like <a href="https://modelcontextprotocol.io" target="_blank" rel="noopener">MCP</a>. Driving a web browser is messier, since pages change under you. Moving a physical actuator is hardest of all. The pattern is that the further out an action reaches, the more careful each step has to be, for the simple reason that the consequences get harder to take back. A deleted file can be restored from backup. A robot arm that has already swung cannot be unswung.

Acting is only useful if something carries forward, which is what memory does. The context window holds what is happening right now, a record of past episodes holds what happened before, and procedural memory holds the know-how. Strip all that away and the agent restarts from nothing every iteration, cheerfully repeating the failure it just lived through.

What keeps the loop pointed somewhere is a mix of intent and self-restraint. The prompt is where a human's goal enters, carrying instructions and constraints. Planning breaks the work down and looks a few moves ahead. Identity, meaning both the character a model is trained with and the persona it is deployed as, narrows what the agent will even consider doing: a coding assistant and a customer-service agent can run on the same model and behave nothing alike. And self-evaluation decides whether to try again, accept the result, or hand the problem to a human. That last one turns out to be where single agents break, in a way worth its own section below.

None of these are separate boxes to tick. They are the same few capabilities seen from different angles: a skill is built out of tools and then stored as procedural memory, and planning shapes the self-evaluation that turns around and reshapes the plan.

## The Loop

The shape of the loop is always the same. The agent perceives, reasons, acts, observes, and goes round again. <a href="https://arxiv.org/abs/2210.03629" target="_blank" rel="noopener">ReAct</a>, short for reason and act, is the simplest way to write it down, but it is only one formalisation among many. Others explore multiple reasoning paths at once (<a href="https://arxiv.org/abs/2305.10601" target="_blank" rel="noopener">tree-of-thought</a>), search over sequences of actions (<a href="https://arxiv.org/abs/2310.04406" target="_blank" rel="noopener">LATS</a>), or make writing and running code the thing the agent does (<a href="https://arxiv.org/abs/2402.01030" target="_blank" rel="noopener">code-act</a>). The implementations differ, but the underlying cycle does not. The agent's own output becomes its input on the next turn, each action changes the world a little, and the changed observation shapes the action that follows.

What sets the loop in motion varies too. A passive agent loops when you call it, as when you ask it to refactor a module and it goes to work. A proactive one loops on its own initiative, watching a system and stepping in when something looks wrong, or delivering a research briefing every morning on schedule. Along the way the loop can pause for a human ("approve my plan before I run it"), reach out to another agent for context, or run entirely on its own.

## When the Loop Improves Itself

Turning the loop is not the same as getting better at anything. An agent that emails a news digest every morning runs its loop faithfully for years and is no more capable on the last day than the first. What changes when the goal is to improve the work itself, whether that is code or a design or the agent's own procedures, is that the output of one pass becomes the input to the next. That is the difference between repetition and **recursive self-improvement**: not doing the task again, but doing it on top of the last attempt.

The tightest version happens inside a single task, where a coding agent runs the tests, reads the failures, and fixes the code, so every edit knows what the last one broke. Widen the window and the agent starts carrying lessons between separate jobs, writing itself a critique after one attempt and consulting it during the next, which is roughly what <a href="https://arxiv.org/abs/2303.11366" target="_blank" rel="noopener">Reflexion</a> does by keeping those self-critiques in memory, and what <a href="https://arxiv.org/abs/2310.03714" target="_blank" rel="noopener">DSPy</a> does by tuning entire prompt pipelines against a score. Widen it further still and the procedures that keep working stop being notes and become tools: <a href="https://arxiv.org/abs/2305.16291" target="_blank" rel="noopener">Voyager</a> accumulates a library of skills that later tasks pull from, so what it learned in hour one is still paying off in hour fifty.

This generate, filter, and validate rhythm is already producing real results, though not every case is one agent looping on its own. Some are pipelines where a model proposes and later stages, human or automated, do the screening. In materials science, DeepMind's GNoME proposed a vast set of candidate crystal structures and predicted a large fraction of them stable, after which Berkeley's autonomous A-Lab reported synthesising dozens of new compounds with little human intervention. In chemistry, <a href="https://arxiv.org/abs/2304.05376" target="_blank" rel="noopener">ChemCrow</a> (2023) planned and carried out syntheses by looping a reasoning model through a set of expert chemistry tools. <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">The AI Scientist</a> (Sakana AI, 2024) goes further still, generating ideas, running experiments, writing them up, and reviewing its own drafts, all in a single loop. The shape is the same each time. Generate many candidates, throw most out through evaluation, and put the few survivors forward for real-world testing, which feeds back into the next round.

## Where Single-Agent Recursion Breaks

Recursive self-improvement is clean in theory. In practice a lone loop runs into trouble that better prompting does not fix, and the trouble compounds in a particular order.

It begins with arithmetic. A long task only succeeds if nearly every step does, so a small per-step error rate turns into a large failure over a long chain, and each mistake tends to narrow the options for recovering from it. That is why today's best agents still fall short of human reliability on realistic computer and web tasks, and why the gap widens as tasks lengthen (<a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">OSWorld</a>, Xie et al., 2024; <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">tau-bench</a>, Yao et al., 2024).

The obvious fix is to have the agent check its own work, and this is where things get strange. When an agent reflects on its performance with nothing external to check against, it often does not correct course. It grows more confident in the answer it already has. Language models struggle to self-correct their reasoning without outside feedback and can come out worse after a round of purely introspective revision (<a href="https://arxiv.org/abs/2310.01798" target="_blank" rel="noopener">Huang et al., 2023</a>); a broader survey finds no convincing case of reliable self-correction that does not draw on some external signal (<a href="https://arxiv.org/abs/2406.01297" target="_blank" rel="noopener">Kamoi et al., 2024</a>). The mechanism meant to catch mistakes ends up laundering them into certainty.

Give the same agent more time and it does not escape, it plateaus. On long, open-ended research and engineering work, agents lead early and then fall behind as the horizon stretches, while humans keep making headway (<a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">RE-Bench</a>, Wijk et al., 2024). The agent exhausts the strategies available to it and settles into diminishing returns instead of stepping back and trying a different angle.

Underneath all three is the same root problem. One loop runs one set of habits, so the very patterns that produced a mistake are the ones deciding whether it was a mistake at all. An agent cannot easily hold a position and attack it at the same time.

None of this is a bug awaiting a patch. These are structural limits of anything improving in isolation, and they point two ways out. One is human oversight at the moments that matter, less a safety brake than a way of keeping the agent pointed at what we actually wanted. The other is more than one agent, so that the check comes from outside the thing being checked.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in Part 2" caption="**Figure 2.** A single agent can loop through a task alone, or an orchestrator can delegate the work to specialists. These are only two points in a much larger design space; Part 2 maps that space in detail, with dozens of delegation patterns grouped into families." >}}

## Beyond the Single Loop

The way out of these limits is to stop relying on a single loop. Once more than one agent is involved, the failure modes above start to lift. Verification can come from an agent other than the one that did the work, different agents can bring genuinely different perspectives, and specialists can handle what a lone generalist cannot. This is where collective capability enters: performance that belongs to the arrangement of agents rather than to any one of them, and, if the arrangement is good enough, perhaps collective intelligence too.

There are many ways to arrange them, from an orchestrator delegating to specialists, to peer-to-peer networks, voting panels, hierarchical chains, and adversarial checks between agents. Which arrangement you choose shapes what kind of improvement is even possible. These delegation patterns, and the governance that grows up around them once agents start keeping track of who did what and how well, are the subject of [The Agent Fabric (Part 2): Division of Labour and Governance](/posts/2026/agent-fabric-part2/).

---

The loop is what separates an agent from a single model call, and the tools, memory, planning, identity, and self-evaluation inside it are what make each turn count. But a loop cannot audit itself. An agent left alone with its own judgement does not converge on the truth; it converges on whatever it already believed, only louder. That is not a flaw to be prompted away, and it is the reason useful agents rarely stay alone for long. Where they go next is [The Agent Fabric (Part 1): Why Agents May Form Societies](/posts/2026/agent-fabric-part1/).
