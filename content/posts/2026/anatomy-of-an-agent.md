---
title: "The Anatomy of an Agent"
subtitle: "The loop, its limits, and why one agent is never enough"
date: 2026-06-27
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "LLM", "agent fabric"]
description: "The loop at the heart of every AI agent, the components that make each iteration effective, and where single-agent recursion hits structural limits that motivate collective organisation."
draft: true
math: false
ShowToc: false
TocOpen: false
hideCitation: false
wordcount: "~1,300 words"
---

{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** The anatomy of a single agent: a foundation model wrapped in scaffolding. The model operates in a recursive loop with an environment, acting on it and observing the result. Inside: capabilities that let it act (tools, skills, memory) and constraints that direct it (planning, identity, self-evaluation)." >}}

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **Prologue: The Anatomy of an Agent** (you are here): the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis, and the path from isolation to interweaving
- **[Part 2: Division of Labour and Governance](/posts/2026/agent-fabric-part2/)**: delegation archetypes, trust and authority, the specialist market, and governance archetypes
</div>

## From Model Call to Agent

When you ask Claude to answer a question, that is a model call. One input, one output, done. Ask a bare model to "run train.py and put the results in the output folder" and the honest answer is that it cannot: it can only produce text about doing so. A foundation model on its own is a static function. It cannot perceive the external environment or alter it. When you ask Claude Code to refactor a module, it reads files, forms a plan, edits code, runs tests, sees the tests fail, edits again, runs again, and stops when the tests pass. Same model underneath. Completely different behaviour, because now it can act on the world and see the consequences.

The difference is structure. An agent is a foundation model plus the **scaffolding** wrapped around it (sometimes called the harness): the prompt that instructs it, the tools that let it act, and the memory that lets it learn from prior iterations, all run inside a loop with self-evaluation to decide when to stop. The model is the reasoning core; the scaffolding is what turns reasoning into action. Remove any of these and something breaks. Without the loop, a chatbot. Without tools, a reasoner that cannot act. Without memory, every iteration starts from scratch. Without a clear prompt and identity, unpredictable behaviour. Without self-evaluation, a system that never converges.

## What the Loop Needs

These components are not a flat checklist; they overlap by design. Skills are composed from tools and stored as procedural memory. Grounding is where tools act on the world. Planning feeds self-evaluation feeds planning. They form layers around the same loop, not independent boxes.

**Acting on the world.** The agent needs tools (primitives like calling an API or reading a file) and skills (reusable procedures that compose tools with strategy). The <a href="https://agentskills.io" target="_blank">Agent Skills</a> open specification defines a portable format for packaging skills that any agent can load. The grounding stack determines where tools connect, from file systems and <a href="https://modelcontextprotocol.io" target="_blank">MCP</a> servers (mature) to browsers (improving) to physical actuators (hardest). The grounding level determines how cautious each iteration must be.

**Remembering.** Working memory (the context window) holds current state. Episodic memory stores what happened. Procedural memory stores how to do things. Without memory, the agent re-discovers the same failures every iteration.

**Directing.** The prompt is where intent enters the scaffold: the instructions, constraints, and current task that tell the model what it is doing and how. Planning decomposes, predicts, and chooses among alternatives. Identity (trained character plus deployed persona) constrains which actions the agent will consider and how it presents itself; a coding assistant and a customer-service agent can share a model yet behave differently because their prompts and identities scope the action space. Self-evaluation decides whether to iterate, accept, or escalate. The critical finding here, which the failure modes at the end of this post make concrete, is that self-correction only works when grounded in external signals (test results, tool outputs, human feedback). Without that grounding, reflection often makes things worse rather than better.

## The Loop

The structure is perceive, reason, act, observe, repeat. <a href="https://arxiv.org/abs/2210.03629" target="_blank">ReAct</a> (think, act, observe) is the simplest formalisation, but it is one of many. Others include plan-and-execute, <a href="https://arxiv.org/abs/2305.10601" target="_blank">tree-of-thought</a> (explore multiple reasoning paths), <a href="https://arxiv.org/abs/2310.04406" target="_blank">LATS</a> (Monte Carlo search over action sequences), and <a href="https://arxiv.org/abs/2402.01030" target="_blank">code-act</a> (write and execute code as the action space), among others. The implementations vary, but the underlying loop is the same. The agent's own outputs become its inputs on the next iteration. Each Action changes the world; the changed Observation informs the next Action.

What triggers the loop varies. A passive agent loops when called (you ask it to refactor, it does it). A proactive agent loops on its own initiative (it monitors a system and acts when it spots issues, or delivers a daily research briefing on schedule). The loop can involve humans (approve my plan before I execute), other agents (ask the research agent for context), or run fully autonomously.

## When the Loop Becomes Recursive Improvement

The loop runs the same way regardless of goal, but not every loop improves anything. An agent that delivers a daily news summary runs the same loop each morning without accumulating capability. When the goal is to iteratively improve something (code, a design, a research output, the agent's own procedures), the loop enables a specific mode, **recursive improvement**, where each iteration builds on what came before.

Three scales.

- **Within a task.** A coding agent runs tests, fixes errors, re-runs. Each fix is informed by the previous failure.
- **Across tasks.** After success or failure, the agent reflects and carries that critique into the next attempt. <a href="https://arxiv.org/abs/2303.11366" target="_blank">Reflexion</a> (Shinn et al., 2023) stores written self-critique in memory. <a href="https://arxiv.org/abs/2310.03714" target="_blank">DSPy</a> (Khattab et al., 2023) optimises prompt pipelines against a metric.
- **Across sessions.** Successful procedures crystallize into skills. <a href="https://arxiv.org/abs/2305.16291" target="_blank">Voyager</a> builds a skill library where each task builds on all prior tasks. The agent accumulates capability over time.

This generate-filter-validate pattern already operates at high impact, though not every instance is a single agent looping; some are pipelines where a model proposes and automated stages screen. In materials science, DeepMind's GNoME generated 2.2 million candidate crystal structures, of which about 380,000 were predicted stable, and Berkeley's A-Lab then autonomously synthesized 41 new compounds from a set of 58 targets in 17 days. In chemistry, <a href="https://arxiv.org/abs/2304.05376" target="_blank">ChemCrow</a> (2023) autonomously planned and executed chemical syntheses by looping through a reasoning model and eighteen expert-designed chemistry tools for synthesis planning and verification. <a href="https://arxiv.org/abs/2408.06292" target="_blank">The AI Scientist</a> (Sakana AI, 2024) generates research ideas, runs experiments, writes papers, and self-reviews in a loop that costs less than $15 per paper. In each case the pattern is the same. Generate many candidates, reject most through automated evaluation, and surface the few promising routes that warrant real-world validation.

## Where Single-Agent Recursion Breaks

This is where the interesting problems live. Recursive improvement sounds clean in theory, but in practice it has structural failure modes that better prompting alone does not fix.

**Errors compound.** On realistic multi-step web tasks, state-of-the-art agents achieve 14% success where humans reach 78% (<a href="https://arxiv.org/abs/2307.13854" target="_blank">WebArena</a>, 2023). The mechanism is multiplicative. If each step is 90% reliable, five compounding steps yield ~59% cumulative success. Each mistake narrows the recovery options for subsequent iterations.

**Reflection without grounding accelerates failure.** This is the counterintuitive finding. When an agent "reflects" on its performance without external verification signals (test results, tool outputs, human feedback), it does not self-correct. It becomes more confidently wrong. LLMs struggle to self-correct reasoning without external feedback, and their performance can *degrade* after purely introspective correction (<a href="https://arxiv.org/abs/2310.01798" target="_blank">Huang et al., 2023</a>). The agent reinforces its own errors with the conviction that it has addressed them. Self-evaluation needs the world, not just the model's opinion of itself.

**Context saturates.** Agents plateau on long tasks while humans keep improving. On extended research benchmarks, agents lead early but humans pull ahead as the time horizon grows (<a href="https://arxiv.org/abs/2411.15114" target="_blank">RE-Bench</a>, Wijk et al., 2024). The agent exhausts its effective search strategies and enters diminishing-return cycles rather than shifting approach.

**Single agents amplify blind spots.** A homogeneous loop cannot generate the diverse perspectives needed to catch its own systematic errors. It cannot cleanly switch between generating solutions and rigorously checking them. The same cognitive patterns that produced the error are the ones evaluating whether it was an error.

These failures are not bugs to be fixed. They are structural limits of single-agent recursion that point toward two necessities. First, human oversight at key decision points (as an alignment mechanism, not just a safety brake). Second, multi-agent patterns that provide external verification, cognitive diversity, and specialised capabilities.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets) explored in Part 2" caption="**Figure 2.** A single agent can loop through a task alone, or an orchestrator can delegate to specialists. These are just two points in a much larger design space." >}}

## Beyond the Single Loop

These structural limits motivate multi-agent patterns. Orchestrators delegating to specialists, peer networks, voting panels, adversarial verification. The delegation pattern determines what kinds of recursive improvement are possible. These patterns, and the governance that forms around them, are the subject of [The Agent Fabric (Part 2): Division of Labour and Governance](/posts/2026/agent-fabric-part2/).

---

The loop is what distinguishes an agent from a single model call. The components inside (tools, memory, planning, identity, self-evaluation) make each iteration effective. And the structural limits of single-agent recursion are what motivate the collective organisation explored in [The Agent Fabric (Part 1): Why Agents May Form Societies](/posts/2026/agent-fabric-part1/).
