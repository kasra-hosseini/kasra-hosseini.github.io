---
title: "[ARCHIVE] The Anatomy of an Agent"
subtitle: "The loop, its limits, and why one agent is never enough"
date: 2026-05-18
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "LLM", "agent fabric"]
description: "The loop that turns a model into an agent, the components that make each iteration effective, and where single-agent recursion hits structural limits that point toward the collective capability of many agents."
draft: true
math: false
ShowToc: false
TocOpen: false
hideCitation: false
wordcount: "~1,250 words"
---

<p style="font-size: 0.82em; color: #999; margin-top: 1em;"><a href="https://code.claude.com/docs/en/overview" target="_blank" rel="noopener" style="color: #999;">Claude Code</a> was used for editing and visualizations. All ideas and arguments are the authors' own.</p>

<div style="background: #fffbeb; border: 1px solid #f59e0b; border-radius: 6px; padding: 0.6em 1em; margin: 1.5em 0; font-size: 0.88em; color: #92400e;">
<strong>Early access.</strong> This blog series is a work in progress. Feedback, comments, and suggestions are welcome. Feel free to <a href="https://www.linkedin.com/in/kasra-hosseini/" target="_blank" rel="noopener" style="color: #92400e;">reach out on LinkedIn</a> or leave a comment at the bottom of the page.
</div>

## From Model Call to Agent

A language model asked to run a script and organise the results will describe how to do it, in ordered steps and often with the exact command, but it will not execute anything. This is not a limitation of knowledge. The model can accept text, images, and audio, and can reason over all of it competently. What it lacks is any means of acting on the thing under discussion.

A coding agent built on the same class of model behaves differently. It reads the relevant files, forms a plan, edits the code, runs the tests, observes the failures, edits again, and terminates once the tests pass. The point worth emphasising is that **the underlying model can be identical.** Some models are now post-trained specifically for agentic work, on tool use and long-horizon tasks, and the improvement is measurable. But that is not what separates the two cases. A model that would only describe the work will perform it once the surrounding machinery exists.

That machinery is what distinguishes description from execution: instructions, tools, a memory that persists across steps, and a loop that continues until the work is judged complete. None of it modifies the parameters or the knowledge encoded in them. Whatever the model acquired during training remains exactly what it knows. What scaffolding changes is how much of that knowledge is brought to bear. A retrieved file, a stack trace, or a record of a previous attempt enters the context window, and a model reasoning over the actual error outperforms the same model reasoning from recollection. The capability is already present. Careful prompting, appropriate context, and well-chosen tools determine how much of it appears in the output.

{{< figure src="/images/2026/anatomy-of-an-agent.svg" alt="Anatomy of an AI agent: a foundation model wrapped in scaffolding (tools, skills, memory, planning) acting on an environment through an action-observation loop, shaped by identity and self-evaluation" caption="**Figure 1.** The agent loop. The agent acts on its environment, observes the result, and repeats until it judges the work complete. Below, the scaffolding that wraps the model and determines what each iteration accomplishes, from the prompt and tools to the memory that persists across steps and the self-evaluation that decides whether to continue. At the bottom, the same components arranged along a spectrum from a single model call to a loop that runs unattended." >}}

## Where Single-Agent Recursion Breaks

An agent that determines for itself when a task is complete will report success in the same terms whether or not the work is finished, because the only instrument available to assess that judgement is the judgement itself. The consequences are easiest to see in a concrete case.

Consider a coding agent debugging a failing test. It attributes the failure to the date handling, rewrites that code, and the test continues to fail. The obvious remedy is to have the agent examine its own reasoning, and some systems are constructed entirely around this principle, <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">producing research and then reviewing their own drafts</a>.

The review does not conclude that the diagnosis was wrong. It concludes that the analysis was correct but the fix incomplete, which is partly true, since the date handling was in fact poorly written. The agent then elaborates: a normalization step, a fallback, a comment documenting the subtlety it believes it has identified. Each iteration increases its confidence. The actual defect lies in the timezone under which the test executes, which the agent did not consider initially and will not consider now, because every failed attempt has supplied further reason to retain the explanation it already holds.

Observed from outside, the process resembles diligence. The commit history records steady progress, each message more specific than the last, the explanations increasingly detailed and increasingly assured. What the record does not show is that the agent stopped entertaining alternatives early, and that its confidence rose precisely because it continued to fail. It cannot distinguish convergence from entrenchment.

This is not a failure of reasoning. Every test the agent reads was modified by its own preceding edit, so it is reasoning about a situation it keeps altering, and its initial error constrains every subsequent attempt. The broader evidence is consistent with this. No current agent completes <a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">routine desktop work at anything approaching human throughput</a>, and given the same task twice, <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">an agent will not reliably approach it the same way</a>. On <a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">research problems measured in days</a>, agents lead human experts over the first few hours and then fall behind permanently. The agent on the timezone defect continues editing long after its search space has collapsed, with nothing in the system to signal that it has.

The agent that committed the error is the agent evaluating it. No prompt corrects this, because it is not a defect in the prompt. It is a structural property of deliberating in isolation.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in the delegation patterns field guide" caption="**Figure 2.** The same task performed alone and divided three ways. The second arrangement is not more capable, only harder to mislead." >}}

## Beyond the Single Loop

Two remedies are available. Place a human at the decisions that matter, or stop leaving a single agent alone with them. Returning to the agent committed to the date handling, nothing about it needed greater capability to escape the loop. It needed one other agent that had not spent the preceding hours accumulating reasons to believe the date handling was at fault.

Consider the same task divided three ways, with no more capability in any component than the original possessed. One agent retains the code. A second receives the failing test alone, without the theory, the history, or the accumulated context of the first. A third is instructed to break whatever the second produces.

The second agent performs the step the first never attempted, running the test under two timezones and observing it pass under one. That step was available throughout. The first agent could not see it because it had long since stopped searching for the defect and begun searching for further evidence about the date handling. The division of labour, not any increase in capability, is what recovered it.

The arrangement introduces a cost of its own. The agent that wrote the code maintains the test was unfair; the agent that ran it maintains otherwise. Nothing in the arrangement resolves the disagreement, because nothing was built to resolve it. What resolves it in practice is which of the two has required fewer restarts, an operational metric that was never intended to determine whose account prevails. It does so regardless.

An agent left alone with its own judgement converges on what it already believed, expressed with greater confidence. Extended tool access does not correct this. The well-read model confined to a room could at least be contradicted; an agent with tools acts while mistaken.

The second agent is therefore not a redundancy. It is the least expensive source of the one thing the first cannot produce for itself, namely an assessment it has not already argued itself into. That is the case for assigning more than one agent to a problem, and it is also where the difficulty begins. Once several agents are involved, something must determine which of them to believe, and a restart count is already available to do it.

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a four-post series on why and how AI agents may form societies and what it means for us. The first two are read front to back; the last two are catalogues to search.

- **The Anatomy of an Agent** (you are here): the loop that turns a model into an agent, and where single-agent recursion breaks
- **[The Agent Fabric: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns, from Supervisor to Market
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes, and who ends up enforcing them
</div>
