---
title: "The Anatomy of an Agent"
subtitle: "The loop, its limits, and why one agent is never enough"
date: 2026-06-27
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "LLM", "agent fabric"]
description: "The loop at the heart of an agent, the components that make each iteration effective, and where single-agent recursion hits structural limits that point toward collective intelligence."
draft: true
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

Before we get to societies of agents, it helps to be clear about what a single agent actually is. That is what this prologue is for. In Part 1 and Part 2 of this series, the agent is the basic unit, the thing that gets connected, delegated to, and governed. Here we open up that unit and look at what makes it tick, so that when later posts talk about many agents working together, we already share a picture of the one.

## From Model Call to Agent

When you ask ChatGPT to answer a question, that is a model call. One input, one output, and you are done. Ask a bare model to "run train.py and put the results in the output folder" and the honest answer is that it cannot. It can only produce text about doing so. A foundation model on its own is a static function; it can neither perceive the world nor change it. Now watch a coding agent like Claude Code or OpenAI's Codex refactor a module. It reads the files, forms a plan, edits the code, runs the tests, sees them fail, edits again, runs again, and stops once they pass. The model underneath is the same. The behaviour is not, because now it can act on the world and see what happens.

What turns the one into the other is architecture. An agent is a foundation model plus the **scaffolding** built around it: the prompt that tells it what it is doing, the tools that let it act, the memory that lets it carry lessons from one step to the next, and the loop that keeps it going until its own self-evaluation says the work is done. The model is the reasoning core; the scaffolding is what turns reasoning into action. Take any piece away and something breaks. Without the loop you are back to a chatbot. Without tools you have a reasoner that can think but not act. Without memory the agent rediscovers the same dead ends on every pass. Without a clear prompt and a stable identity its behaviour drifts. Without planning it acts without looking ahead, and without self-evaluation it never knows when to stop.

## What the Loop Needs

These pieces are not a checklist of separate boxes. They overlap by design and lean on each other. Skills are built out of tools and remembered as procedural memory. Grounding is simply where the tools meet the world. Planning shapes self-evaluation, which in turn reshapes the plan. Together they form layers around a single loop.

The first thing the loop needs is a way to **act on the world**. That means tools, the primitives like reading a file or calling an API, and skills, the reusable procedures that string tools together with a bit of strategy. The <a href="https://agentskills.io" target="_blank">Agent Skills</a> open specification gives skills a portable format that any agent can load. Where those tools connect is what practitioners call the grounding stack, and it ranges from the mature (file systems and <a href="https://modelcontextprotocol.io" target="_blank">MCP</a> servers) through the improving (browsers) to the genuinely hard (physical actuators). The further out on that stack an action reaches, the more careful each step has to be, because the consequences are harder to undo.

The loop also needs a way of **remembering**. Working memory, the context window, holds what is happening right now. Episodic memory keeps a record of what happened before, and procedural memory holds the know-how for getting things done. Strip memory away and the agent starts every iteration from nothing, repeating the failures it just lived through.

Finally, the loop needs to be **directed**. The prompt is where a human's intent enters the system, carrying the instructions, constraints, and task at hand. Planning decomposes the work, weighs alternatives, and looks a few moves ahead. Identity, both the character a model is trained with and the persona it is deployed as, narrows the set of actions the agent will even consider; a coding assistant and a customer-service agent can run on the same model yet behave nothing alike, because their prompts and identities scope what each one is willing to do. Self-evaluation is the part that decides whether to try again, accept the result, or hand the problem to a human. The catch, which the failure modes near the end of this post make concrete, is that self-evaluation only works when it is anchored to something outside the model, like a test result, a tool's output, or human feedback. Left to reflect on itself with nothing external to check against, an agent tends to make things worse rather than better.

## The Loop

The shape of the loop is always the same. The agent perceives, reasons, acts, observes, and goes round again. <a href="https://arxiv.org/abs/2210.03629" target="_blank">ReAct</a>, short for reason and act, is the simplest way to write it down, but it is only one formalisation among many. Others explore multiple reasoning paths at once (<a href="https://arxiv.org/abs/2305.10601" target="_blank">tree-of-thought</a>), search over sequences of actions (<a href="https://arxiv.org/abs/2310.04406" target="_blank">LATS</a>), or make writing and running code the thing the agent does (<a href="https://arxiv.org/abs/2402.01030" target="_blank">code-act</a>). The implementations differ, but the underlying cycle does not. The agent's own output becomes its input on the next turn, each action changes the world a little, and the changed observation shapes the action that follows.

What sets the loop in motion varies too. A passive agent loops when you call it, as when you ask it to refactor a module and it goes to work. A proactive one loops on its own initiative, watching a system and stepping in when something looks wrong, or delivering a research briefing every morning on schedule. Along the way the loop can pause for a human ("approve my plan before I run it"), reach out to another agent for context, or run entirely on its own.

## When the Loop Improves Itself

The loop turns the same way whatever the goal, but turning is not the same as getting better. An agent that emails a daily news digest runs its loop every morning without ever becoming more capable. Something more interesting happens when the goal is to improve the work itself, whether that is code, a design, a research result, or even the agent's own procedures. Then each pass builds on the last, and the loop becomes a form of **recursive self-improvement**. The word recursive matters here, because the agent is not just repeating a task; it is feeding its own results back in to do better next time.

This plays out at three scales. Within a single task, a coding agent runs the tests, reads the failures, and fixes the code, so each edit is informed by the one before. Across tasks, an agent reflects on how an attempt went and carries that critique into the next one; <a href="https://arxiv.org/abs/2303.11366" target="_blank">Reflexion</a> (Shinn et al., 2023) does this by storing written self-critiques in memory, and <a href="https://arxiv.org/abs/2310.03714" target="_blank">DSPy</a> (Khattab et al., 2023) tunes whole prompt pipelines against a metric. Across sessions, the procedures that keep working harden into reusable skills; <a href="https://arxiv.org/abs/2305.16291" target="_blank">Voyager</a> builds up a skill library in which every new task can draw on everything learned before, so capability accumulates over time.

This generate, filter, and validate rhythm is already producing real results, though not every case is one agent looping on its own. Some are pipelines where a model proposes and later stages, human or automated, do the screening. In materials science, DeepMind's GNoME proposed a vast set of candidate crystal structures and predicted a large fraction of them stable, after which Berkeley's autonomous A-Lab synthesised dozens of genuinely new compounds with little human intervention. In chemistry, <a href="https://arxiv.org/abs/2304.05376" target="_blank">ChemCrow</a> (2023) planned and carried out syntheses by looping a reasoning model through a set of expert chemistry tools. <a href="https://arxiv.org/abs/2408.06292" target="_blank">The AI Scientist</a> (Sakana AI, 2024) goes further still, generating ideas, running experiments, writing them up, and reviewing its own drafts, all in a single loop. The shape is the same each time. Generate many candidates, throw most of them out through evaluation that may be automated or done by hand, and put the few survivors forward for real-world testing, which itself feeds back into the next round.

## Where Single-Agent Recursion Breaks

Recursive self-improvement is clean in theory. In practice it runs into a handful of structural failure modes, and better prompting alone does not fix them.

The first is that errors compound. On realistic, multi-step computer and web tasks, today's best agents still fall well short of human reliability (<a href="https://arxiv.org/abs/2404.07972" target="_blank">OSWorld</a>, Xie et al., 2024; <a href="https://arxiv.org/abs/2406.12045" target="_blank">tau-bench</a>, Yao et al., 2024). The reason is simple arithmetic: a task that takes many steps only succeeds if most of them do, so small per-step error rates multiply into large failures over a long chain. Worse, each mistake tends to narrow the options for recovering on the steps that follow.

The second is that reflection without grounding tends to backfire. When an agent reflects on its own performance with nothing external to check against, it often does not correct course; it just grows more confident in the answer it already has. The research bears this out. Language models struggle to self-correct their reasoning without external feedback, and can even get worse after a round of purely introspective revision (<a href="https://arxiv.org/abs/2310.01798" target="_blank">Huang et al., 2023</a>), and a broader survey finds no convincing case of reliable self-correction that does not draw on some outside signal (<a href="https://arxiv.org/abs/2406.01297" target="_blank">Kamoi et al., 2024</a>). This is really the same point as before, seen from the inside: self-evaluation only helps when it is anchored to the world. By "the world" we mean any signal that originates outside the model's own judgement, such as a failing test, an error returned by a tool, or a correction from a person. Absent that, the loop that was supposed to catch mistakes ends up reinforcing them.

The third is that context saturates. On long tasks, agents tend to plateau while humans keep making headway, leading early and then falling behind as the time horizon stretches (<a href="https://arxiv.org/abs/2411.15114" target="_blank">RE-Bench</a>, Wijk et al., 2024). The agent works through the search strategies available to it and settles into diminishing returns, rather than stepping back and trying a different approach.

The fourth is that a single agent amplifies its own blind spots. One loop, running one set of cognitive habits, cannot easily produce the range of perspectives needed to catch its own systematic errors, nor switch cleanly between generating a solution and scrutinising it. The very patterns that produced a mistake are the ones deciding whether it was a mistake at all.

None of these are bugs waiting for a patch. They are structural limits of a single agent improving in isolation, and they point in two directions. One is human oversight at the moments that matter, less a safety brake than a way of keeping the agent aligned with what we actually want. The other is working with more than one agent, so that verification comes from outside, perspectives differ, and specialised capabilities are on hand where a lone generalist would struggle.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in Part 2" caption="**Figure 2.** A single agent can loop through a task alone, or an orchestrator can delegate the work to specialists. These are only two points in a much larger design space; Part 2 maps that space in detail, with dozens of delegation patterns grouped into families." >}}

## Beyond the Single Loop

The way out of these limits is to stop relying on a single loop. Once more than one agent is involved, the failure modes above start to lift. Verification can come from an agent other than the one that did the work, different agents can bring genuinely different perspectives, and specialists can handle what a lone generalist cannot. This is where collective intelligence enters: capability that belongs to the arrangement of agents rather than to any one of them.

There are many ways to arrange them, from an orchestrator delegating to specialists, to peer-to-peer networks, voting panels, hierarchical chains, and adversarial checks between agents. Which arrangement you choose shapes what kind of improvement is even possible. These delegation patterns, and the governance that grows up around them once agents start keeping track of who did what and how well, are the subject of [The Agent Fabric (Part 2): Division of Labour and Governance](/posts/2026/agent-fabric-part2/).

---

The loop is what separates an agent from a single model call, and the parts inside it, the tools, memory, planning, identity, and self-evaluation, are what make each turn of that loop count. But the same loop, run in isolation, has limits it cannot reason its way past. Those limits are exactly why useful agents tend not to stay alone. They are the starting point for [The Agent Fabric (Part 1): Why Agents May Form Societies](/posts/2026/agent-fabric-part1/), where individual agents give way to the collective intelligence of a society.
