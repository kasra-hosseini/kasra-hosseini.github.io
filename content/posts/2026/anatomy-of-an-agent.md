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

Ask a bare language model to "run train.py and put the results in the output folder" and the honest answer is that it cannot. It can only produce text about doing so. On its own a foundation model is a static function: it can neither perceive the world nor change it.

Now watch a coding agent like Claude Code or OpenAI's Codex refactor a module. It reads the files, forms a plan, edits the code, runs the tests, sees them fail, edits again, runs again, and stops once they pass. Here is the part worth pausing on: **the model underneath is the same model.** Nobody made it smarter. Something was built around it, and that something is the difference between a thing that describes work and a thing that does it.

That gap is not closed by a better model. It is closed by architecture, and the architecture has a name: **scaffolding**. A prompt telling the agent what it is doing. Tools letting it act. Memory carrying lessons from one step to the next. A loop that keeps going until the agent's own judgement says the work is done. The model supplies the reasoning; the scaffolding converts reasoning into consequence.

Every piece of that is load-bearing, and the fastest way to see it is to pull pieces out. Without the loop you are back to a chatbot. Without tools you have something that can think and never touch anything. Memory is the strangest omission of the three, because an agent that loses it does not get dumber in any measurable way. It stays exactly as capable as before and walks into the same dead end every twenty minutes, with full confidence, forever.

## What the Loop Needs

Start with the acting, which sounds like the easy part and mostly is, until you notice that not all acting is equally recoverable. Reading and writing files is solved, and so is calling a well-behaved service through something like <a href="https://modelcontextprotocol.io" target="_blank" rel="noopener">MCP</a>. Driving a browser is messier, since pages change under you. Moving a physical actuator is hardest of all. The rule has nothing to do with intelligence: a deleted file comes back from backup, and a robot arm that has already swung cannot be unswung. (The primitives are called tools, and the reusable procedures built from them are skills, which the <a href="https://agentskills.io" target="_blank" rel="noopener">Agent Skills</a> specification gives a portable format.)

Everything else in the scaffolding exists to keep the loop pointed somewhere. A prompt is how a human's goal gets in, planning looks more than one move ahead, and identity narrows what the agent will even consider doing, which is why a coding assistant and a customer-service agent can run on the same model and behave nothing alike. Then there is self-evaluation, which decides when to stop, retry, or escalate to a person.

That last one deserves suspicion, because an agent judging its own work is the one component whose failure is invisible from the inside. It is also where single agents break, which the rest of this post is largely about.

## The Loop

The loop itself is simple. The agent perceives, reasons, acts, observes, and goes round again. What makes it powerful is not the shape but a property of it: the agent's own output becomes its input next time round. Each action changes the world slightly, and the changed world is what it looks at when deciding the next action. That feedback is the whole trick. Research has wrapped it many ways, plainly in <a href="https://arxiv.org/abs/2210.03629" target="_blank" rel="noopener">ReAct</a> or by exploring several lines of reasoning at once, but the cycle underneath does not change.

What sets the loop in motion varies too. A passive agent loops when you call it, as when you ask it to refactor a module and it goes to work. A proactive one loops on its own initiative, watching a system and stepping in when something looks wrong, or delivering a research briefing every morning on schedule. Along the way the loop can pause for a human ("approve my plan before I run it"), reach out to another agent for context, or run entirely on its own.

## When the Loop Improves Itself

Turning the loop is not the same as getting better at anything. An agent that emails a news digest every morning runs its loop faithfully for years and is no more capable on the last day than the first. What changes when the goal is to improve the work itself, whether that is code or a design or the agent's own procedures, is that the output of one pass becomes the input to the next. That is the difference between repetition and **recursive self-improvement**: not doing the task again, but doing it on top of the last attempt.

The tightest version happens inside a single task, where a coding agent runs the tests, reads the failures, and fixes the code, so every edit knows what the last one broke. Widen the window and something stranger happens: the agent writes itself a note after a failed attempt, and consults that note next time. Fifty tasks later it is working from a file of things it learned the hard way, and the version of itself that started has effectively been replaced. That is what <a href="https://arxiv.org/abs/2303.11366" target="_blank" rel="noopener">Reflexion</a> does with self-critiques, what <a href="https://arxiv.org/abs/2310.03714" target="_blank" rel="noopener">DSPy</a> does by tuning prompt pipelines against a score, and what <a href="https://arxiv.org/abs/2305.16291" target="_blank" rel="noopener">Voyager</a> does by turning procedures that keep working into reusable skills.

This is not confined to software either. The same loop has been pointed at chemistry, proposing candidate compounds and reading back what the equipment found (<a href="https://arxiv.org/abs/2304.05376" target="_blank" rel="noopener">ChemCrow</a>), and furthest of all at research itself: <a href="https://arxiv.org/abs/2408.06292" target="_blank" rel="noopener">The AI Scientist</a> generates its own ideas, runs the experiments, writes them up, and then reviews its own drafts. Which should give you pause, and the next section is why.

Worth being precise about what is happening in those cases, though, because not all of them are one agent improving alone. Several are pipelines where a model proposes and some later stage, human or automated, does the screening. The shape underneath is identical either way: generate many candidates, throw nearly all of them out, push the few survivors into the real world, and let what happens there feed the next round.

## Where Single-Agent Recursion Breaks

Recursive self-improvement is clean in theory. In practice a lone loop runs into trouble that better prompting does not fix, and the trouble compounds in a particular order.

It begins with arithmetic. A long task only succeeds if nearly every step does, so a small per-step error rate turns into a large failure over a long chain, and each mistake tends to narrow the options for recovering from it. That is why today's best agents still fall short of human reliability on realistic computer and web tasks, and why the gap widens as tasks lengthen (<a href="https://arxiv.org/abs/2404.07972" target="_blank" rel="noopener">OSWorld</a>, Xie et al., 2024; <a href="https://arxiv.org/abs/2406.12045" target="_blank" rel="noopener">tau-bench</a>, Yao et al., 2024).

The obvious fix is to have the agent check its own work, and this is where things get strange. Picture it debugging a failing test. It decides the bug is in the date handling, rewrites that, and the test still fails. Asked to review its own reasoning, it does not conclude that it looked in the wrong place. It concludes that its fix was correct but incomplete, and goes deeper into the date handling, writing a more elaborate version of the same wrong answer. The bug was in the timezone the test ran under, which it never considered and now never will, because every pass through the loop adds more reasons to believe the story it already has.

This is not a quirk of one system. Language models struggle to self-correct their reasoning without outside feedback and can come out worse after a round of purely introspective revision (<a href="https://arxiv.org/abs/2310.01798" target="_blank" rel="noopener">Huang et al., 2023</a>); a broader survey finds no convincing case of reliable self-correction that does not draw on some external signal (<a href="https://arxiv.org/abs/2406.01297" target="_blank" rel="noopener">Kamoi et al., 2024</a>). The mechanism meant to catch mistakes ends up laundering them into certainty.

Give the same agent more time and it does not escape, it plateaus. On long, open-ended research and engineering work, agents lead early and then fall behind as the horizon stretches, while humans keep making headway (<a href="https://arxiv.org/abs/2411.15114" target="_blank" rel="noopener">RE-Bench</a>, Wijk et al., 2024). The agent exhausts the strategies available to it and settles into diminishing returns instead of stepping back and trying a different angle.

Underneath all three is the same root problem. One loop runs one set of habits, so the very patterns that produced a mistake are the ones deciding whether it was a mistake at all. An agent cannot easily hold a position and attack it at the same time.

None of this is a bug awaiting a patch. These are structural limits of anything improving in isolation, and they point two ways out. One is human oversight at the moments that matter, less a safety brake than a way of keeping the agent pointed at what we actually wanted. The other is more than one agent, so that the check comes from outside the thing being checked.

{{< figure src="/images/2026/anatomy-delegation-styles.svg" alt="Two delegation styles side by side: a single agent looping through a task alone, and an orchestrator dividing work among research, coding, and review specialists, with a panel listing further patterns (peer-to-peer networks, voting panels, specialist markets, hierarchical chains, blackboard systems, debate protocols) explored in Part 2" caption="**Figure 2.** A single agent can loop through a task alone, or an orchestrator can delegate the work to specialists. These are only two points in a much larger design space; Part 2 maps that space in detail, with dozens of delegation patterns grouped into families." >}}

## Beyond the Single Loop

The way out of these limits is to stop relying on a single loop. Once more than one agent is involved, the failure modes above start to lift. Verification can come from an agent other than the one that did the work, different agents can bring genuinely different perspectives, and specialists can handle what a lone generalist cannot. This is where collective capability enters: performance that belongs to the arrangement of agents rather than to any one of them, and, if the arrangement is good enough, perhaps collective intelligence too.

Here is what that buys, concretely. Take a coding task and give it to one capable agent, and it will write something plausible, convince itself the tests it wrote are the right tests, and hand you work that fails in a way it cannot see. Now split the job: one agent writes, a second writes tests without seeing the first one's reasoning, a third tries to break the result. None of them is smarter than the original. But the second one is not invested in the first one's mistake, and the third one is rewarded for finding it, and suddenly the blind spot has somewhere to be caught.

That is the whole idea, and it generalizes uncomfortably far: the thing that could not check itself is now checked by something with no stake in the answer.

Which raises the question the rest of the series is about: once you have several agents, somebody has to decide who does what, who checks whom, and whose judgement wins when two of them disagree. Those are questions about power, and they turn out to answer themselves whether or not anyone is paying attention. [Part 2](/posts/2026/agent-fabric-part2/) is how the work gets split. [Part 3](/posts/2026/agent-fabric-part3/) is who ends up in charge.

---

One line is worth carrying away from all of this. A loop cannot audit itself: an agent left alone with its own judgement does not converge on the truth, it converges on whatever it already believed, only louder. Everything else in this series follows from trying to fix that, and from what the fix costs.
