---
title: "[ARCHIVE-FULL] The Agent Fabric"
subtitle: "Designed Patterns and Emergent Dynamics of an Agent-Populated World"
date: 2026-04-06
author: "Kasra Hosseini"
post_categories: ["AI"]
tags: ["AI", "AI agents", "multi-agent systems", "LLM", "agent ecology", "society of mind"]
description: "Designed Patterns and Emergent Dynamics of an Agent-Populated World"
cover:
  image: "/images/agent-fabric-cover.svg"
  alt: "Abstract visualization of threads weaving into a fabric pattern, representing designed and emergent agent interactions"
  hidden: true
draft: true
math: false
ShowToc: true
TocOpen: false
hideCitation: true
---

<style>
  .viz-container {
    width: 100%;
    max-width: 720px;
    margin: 2em auto;
    background: #fafafa;
    border: 1px solid #e5e5e5;
    border-radius: 8px;
    overflow: hidden;
  }
  .viz-container svg {
    display: block;
    width: 100%;
    height: auto;
  }
  .viz-caption {
    text-align: center;
    font-size: 0.85em;
    color: #666;
    padding: 0.8em 1em;
    border-top: 1px solid #e5e5e5;
    background: #f5f5f5;
    line-height: 1.5;
  }
  .viz-restart {
    display: inline-block;
    margin: 0.5em auto 0;
    padding: 0.3em 1em;
    font-size: 0.8em;
    color: #888;
    background: #fff;
    border: 1px solid #ddd;
    border-radius: 4px;
    cursor: pointer;
    transition: all 0.15s;
  }
  .viz-restart:hover {
    color: #333;
    border-color: #999;
  }
  .post-description {
    font-size: 1.3em;
    color: #555;
    font-weight: 300;
    margin-top: 0.3em;
    margin-bottom: 1.2em;
  }
  .pattern-number {
    display: inline-block;
    width: 1.6em;
    height: 1.6em;
    line-height: 1.6em;
    text-align: center;
    background: #b91c1c;
    color: white;
    border-radius: 50%;
    font-weight: bold;
    font-size: 0.85em;
    margin-right: 0.4em;
    vertical-align: middle;
  }
</style>

<div class="viz-container" style="margin-top: 0.5em;">
  <div id="viz-cm2" style="width: 100%; height: 320px;" role="img" aria-label="Animated visualization inspired by the Connection Machine CM-2, with slow red LED squares and fabric weave threads."></div>
  <div class="viz-caption">Thousands of agents today. Millions soon. Billions and trillions within reach. They form a fabric (part deliberately designed, part self-organizing) of local models, cloud specialists, tiny routers, and massive reasoners, all delegating, competing, and collaborating. The visualization is inspired by the <a href="https://en.wikipedia.org/wiki/Connection_Machine" target="_blank" class="red-link">Connection Machine CM-2</a> (Thinking Machines, 1987): there it was thousands of processors wired into parallel computation; here it is agents woven into an interconnected fabric. The threads between panels represent the patterns that hold it together.</div>
</div>

An AI agent is a system that can perceive its situation, decide what to do, and act. Not a chatbot waiting for a prompt, but software that plans, uses tools, calls other software, and follows through. Today we build them one at a time. But the interesting question is not what a single agent can do. It is what happens when millions of them work together.

We are not just building AI agents. We are weaving a fabric.

Some threads in this fabric are ones we place deliberately. An engineer decides: this agent handles code, that one checks for errors, a third one routes questions to the right specialist. These are **designed patterns**: the architecture choices that shape how agents divide work, verify quality, and share knowledge. They are blueprints. We draw them on whiteboards.

But once enough agents start working together, something else happens. Things appear that nobody planned. Agents start forming groups. Reputations develop. A shared knowledge base grows so large and interconnected that no human can meaningfully read it anymore. An agent drifts, slowly, from its original purpose. These are **emergent dynamics**: behaviours that arise not from any single agent's design, but from the interaction of many agents at scale. Nobody drew them on a whiteboard. They showed up on their own.

This post is about both. The patterns we choose and the dynamics that surprise us. The subtitle says it plainly: *Designed Patterns and Emergent Dynamics of an Agent-Populated World*. Understanding both (and the tension between them) is what it takes to build agent systems that work at scale without spinning out of control.

Today, a typical agent system involves a handful of models: a planner, a coder, a critic, wired together with prompts and tool calls. But the trajectory is unmistakable. Thousands of agents in production today. Millions soon. Billions and trillions are not unreasonable to imagine. And at that scale, what emerges is not a single orchestrated pipeline but a vast, heterogeneous population: local models on phones, cloud-hosted specialists, fine-tuned domain experts, tiny routers, massive reasoners, all interacting, delegating, competing, and collaborating.

---

## From Mindless to Mindful: Beyond the Society of Mind

In 1986, Marvin Minsky proposed a radical idea in <a href="https://en.wikipedia.org/wiki/Society_of_Mind" target="_blank" class="red-link">*The Society of Mind*</a>: intelligence is not a single mechanism but the emergent result of many **simple, mindless agents** interacting (<a href="https://en.wikipedia.org/wiki/Society_of_Mind" target="_blank" class="red-link">Minsky, 1986</a>). Each agent does one tiny thing: detect an edge, compare two sizes, trigger a reflex. None is intelligent on its own. Intelligence arises from their collective organization. As Minsky put it:

> *"The power of intelligence stems from our vast diversity, not from any single, perfect principle."*

Minsky's agents were thermostats and edge detectors. Today's AI agents are something fundamentally different: **they are already intelligent**. A single LLM can reason, write code, summarize documents, and hold extended conversations. When Minsky asked "what happens when you wire together many simple parts?", the answer was: intelligence emerges. We are now confronting a different, and arguably more profound, question:

**What happens when you wire together many *intelligent* parts?**

The answer has two sides. On one, there are patterns we can deliberately engineer: how to decompose tasks (Chapter 1), how to enforce quality (Chapter 2), how to structure shared knowledge (Chapter 3). These are design choices, informed by the same principles that govern distributed systems, organizational theory, and protocol engineering. On the other, there are dynamics that *emerge* once these patterns run at scale: agents self-organize into societies with governance structures (Chapter 4), systems accelerate their own improvement in ways that compound unpredictably (Chapter 5), goals drift through mechanisms no individual agent intended (Chapter 6), and the human-agent boundary reshapes itself (Chapter 7). The collective outcomes of such a society cannot be deduced from the behaviour of any individual agent, just as the dynamics of a city cannot be predicted from the psychology of a single citizen. We need new frameworks: part computer science, part complex systems theory, part institutional design (<a href="https://arxiv.org/abs/2402.14410" target="_blank" class="red-link">Tsvetkova et al., 2024</a>; <a href="https://arxiv.org/abs/2402.01680" target="_blank" class="red-link">Guo et al., 2024</a>; <a href="https://arxiv.org/abs/2309.07864" target="_blank" class="red-link">Xi et al., 2023</a>).

**A note on terms.** Throughout this post, "agent" means an AI system that can reason about a task, decide which tools or sub-systems to invoke, and act on those decisions: not a bare API call, but an entity with at least a rudimentary loop of perception, planning, and action. By this definition, today's landscape has thousands of agents in production. "Millions" and "billions" refer to a plausible near-future where agents run on phones, appliances, edge devices, and cloud infrastructure simultaneously, each specialized, most ephemeral. "Trillions" is a horizon: the point at which the agent population exceeds the number of humans and the dominant interactions in the system are agent-to-agent rather than human-to-agent. The patterns shift qualitatively at each scale, and part of what this post explores is *where* those transitions occur.

---

## <span class="pattern-number">1</span> The Division of Labour

The first question any multi-agent system must answer: how does work get split? At small scale, a single orchestrator can assign tasks. At billion-agent scale, the division of labour becomes the architecture itself, spanning delegation hierarchies, specialist markets, and hybrid reasoning systems that combine fundamentally different modes of computation.

### The Delegation Tree

<div class="viz-container">
  <div id="viz-delegation" style="width: 100%; height: 1620px;" role="img" aria-label="Twelve delegation archetypes in a 6x2 grid, each animated with task and result packets. Click to restart."></div>
  <div class="viz-caption"><strong>Twelve delegation archetypes.</strong> Delegation is about task flow: how work gets decomposed, routed, and reassembled. The first eight archetypes are deliberately designed: an architect chooses the shape. The last four (Orchestrator, Evaluator, Swarm, Gossip) shade into emergent behaviour, where structure arises from interaction. This is where delegation meets governance (Chapter 4): delegation shapes a single task; governance shapes the persistent social structures that form across many. Red packets are tasks; green are results. Click any panel to expand.<br><button class="viz-restart" onclick="document.getElementById('viz-delegation').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

The most natural starting point: **agents that spawn other agents**. A user asks a question. The primary agent realizes it needs help: a web search, a code execution, a document summary. It delegates. The sub-agent might delegate further. A tree forms.

This is already happening. Multi-agent frameworks like <a href="https://arxiv.org/abs/2308.08155" target="_blank" class="red-link">AutoGen</a>, <a href="https://arxiv.org/abs/2303.17760" target="_blank" class="red-link">CAMEL</a>, and <a href="https://arxiv.org/abs/2308.00352" target="_blank" class="red-link">MetaGPT</a> assign distinct roles to agents and coordinate them through structured workflows. Production systems, from <a href="https://corporate.zalando.com/en/technology/more-personal-and-smarter-zalando-assistant-enhanced-capabilities-inspire-customers" target="_blank" class="red-link">Zalando Assistant</a> to Microsoft's <a href="https://arxiv.org/abs/2411.04468" target="_blank" class="red-link">Magentic-One</a>, use orchestrator agents that plan, track progress, and re-plan when sub-agents fail. But today's trees are shallow: two or three levels deep.

This is harder than it sounds. When we first tried heterogeneous model delegation in the <a href="https://corporate.zalando.com/en/technology/more-personal-and-smarter-zalando-assistant-enhanced-capabilities-inspire-customers" target="_blank" class="red-link">Zalando Assistant</a>, the results were mixed: models of different sizes and capabilities did not compose cleanly, and failures at one level cascaded unpredictably. Today, with better tool-calling conventions and agent-to-agent protocols, the same approach works far more reliably. The infrastructure caught up with the idea.

At scale, the cascade deepens. A trillion-agent world has no single orchestrator. It has **delegation all the way down**: frontier models dispatching to medium specialists, who dispatch to small local models, who dispatch to tiny classifiers. The cost of a query is not determined by one model. It is determined by the *shape of the tree* it spawns.

A note on scope: delegation is about **task flow**: how a single piece of work gets decomposed, routed, and reassembled. The last few archetypes (Swarm, Gossip) blur into something different: **governance**, which is about how agents relate to each other *over time*: who leads, who decides, who verifies (Chapter 4). The boundary is not sharp. A Router that learns which specialists to prefer is starting to develop trust. A Swarm that repeatedly clusters around the same agents is starting to develop hierarchy.

In practice, no real system uses just one archetype. The power lies in **composition**. Three examples:

**The Quality Gate** (Router + Escalation + Evaluator): an input is classified by a router, sent to the cheapest capable specialist, escalated to a larger model if confidence is low, and the output passes through an evaluator loop before returning. This is how cost-conscious production systems work: route cheaply, escalate rarely, verify always.

**The Consensus Engine** (Map-Reduce + Voting): a task is split into sub-problems (map), each sub-problem is solved by multiple agents who vote on the correct answer, and the verified results are merged (reduce). This is how you get reliability without trusting any single agent.

**The Bidding Pipeline** (Pipeline + Auction): data flows through a fixed sequence of stages, but each stage is awarded to whichever agent bids most competitively for it. Different specialists win different stages. The pipeline shape is designed; who fills each slot is determined by competition. This is how marketplaces and outsourcing platforms work.

<div class="viz-container">
  <div id="viz-combo" style="width: 100%; height: 420px;" role="img" aria-label="Three combined delegation patterns shown side by side: Quality Gate, Consensus Engine, and Bidding Pipeline."></div>
  <div class="viz-caption"><strong>Composition: how delegation archetypes combine.</strong> No real system uses a single archetype. These three compositions show how different combinations serve different goals. <em>Left: The Quality Gate</em> (Router + Escalation + Evaluator): an input arrives and a router classifies it, sending easy queries to a small, cheap specialist and escalating ambiguous ones to a larger frontier model. The evaluator checks the result and loops back if quality is insufficient. Most queries take the cheap path; only ~30% escalate. This is how cost-conscious production systems work. <em>Centre: The Consensus Engine</em> (Map-Reduce + Voting): a task is split into sub-problems (red node), each sub-problem is solved by multiple independent agents (blue), and their answers are compared at a verdict node (orange). The majority answer wins. Verified verdicts flow to the merge node (green), which reassembles the final result. Redundancy buys reliability at the cost of compute. <em>Right: The Adaptive Pipeline</em> (Pipeline + Auction): data flows through fixed stages (Ingest, Parse, Enrich, Store), but each stage is awarded to whichever agent bids most competitively. Different coloured agents compete for each slot; the winner lights up below the stage. After all stages are assigned, a green packet flows through the full pipeline. The pipeline shape is designed; who fills each slot is determined by competition.<br><button class="viz-restart" onclick="document.getElementById('viz-combo').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

The key insight: **delegation is fractal**. The same decompose-dispatch-aggregate pattern repeats at every level. And the efficiency of the whole system depends not on any single agent, but on how well the tree prunes itself: how quickly agents decide "I can handle this" versus "I need help."

A practical note on building these systems: Anthropic's field report on <a href="https://www.anthropic.com/engineering/building-effective-agents" target="_blank" class="red-link">building effective agents</a> draws a useful distinction between **workflows** (predefined code paths that orchestrate LLM calls) and **agents** (systems where the LLM dynamically directs its own process). The finding is counterintuitive: the most successful implementations use simple, composable patterns (prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer loops) rather than complex frameworks. Start with an optimized single LLM call and add multi-agent orchestration only when simpler solutions demonstrably fail. This maps directly to the delegation archetypes above: each is a composable building block, not a monolithic architecture.

This has a direct economic consequence. Every level of delegation multiplies cost and latency. Agents consume compute, memory, bandwidth, and API quotas, and these resources are finite. The relationship between cost and quality follows <a href="https://arxiv.org/abs/2001.08361" target="_blank" class="red-link">power laws</a>: performance improves smoothly as you scale compute, data, and parameters, but the returns diminish on a log-log curve. At scale, this creates selection pressure: a delegation tree that solves a task in 100 tokens survives budget constraints that kill one requiring 10,000 tokens for the same result. Over time, the population shifts toward efficiency. The future may not belong to the deepest tree, but to the **shallowest tree that still produces a correct answer**. An agent that achieves 95% of the quality at 10% of the cost will outcompete the one that achieves 100% at 100%.

<div class="viz-container">
  <div id="viz-resource" style="width: 100%; height: 420px;" role="img" aria-label="Scatter plot showing successive frontier generations pushing quality rightward, with distillation creating cheaper alternatives and older frontiers fading to grey ghosts."></div>
  <div class="viz-caption"><strong>Resource ecology.</strong> Each circle is a model, plotted by quality (x-axis) and cost (y-axis, log scale). The animation shows successive generations of frontier models pushing quality rightward while a growing population of small and medium community models fills the lower-left. Each new frontier makes its predecessor obsolete (grey ghosts). Distillation then compresses knowledge into cheaper models (red arrows), and in some cases the distilled version nearly matches the frontier's quality at a fraction of the cost. Over four generations, the landscape becomes densely populated: a few large frontier models at the top-right, distilled models that rival them in quality but not in cost, a broad middle band, and many small cheap models across the bottom. Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-resource').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

### The Specialist Market

<div class="viz-container">
  <div id="viz-specialists" style="width: 100%; height: 420px;" role="img" aria-label="Marketplace visualization with four routers on the left dispatching queries to twelve specialist agents, tools, and knowledge bases on the right."></div>
  <div class="viz-caption"><strong>The specialist marketplace.</strong> Left: four routers (Assistant, Copilot, Search, Pipeline) receive different types of user requests. Right: an access-controlled marketplace of specialists, differentiated by shape: circles are agents, rectangles are tools, and diamonds are knowledge bases. Solid grey lines show active connections; dashed red lines with locks show denied access. Not every router can reach every specialist, as access control determines who can use what. Edge thickness reflects traffic volume. Animated packets flow from routers to the specialists they rely on most.</div>
</div>

Delegation tells you how work flows through a tree. But who sits at each node? Today's frontier models are generalists. But generalism is expensive. At scale, **specialization wins**.

The evidence is compelling. Layered systems where specialized models feed outputs into aggregators can collectively surpass frontier models despite using only open-source components (<a href="https://arxiv.org/abs/2406.04692" target="_blank" class="red-link">Mixture-of-Agents</a>). Dynamic team composition, where the system selects which agents should participate based on the query type, outperforms static configurations (<a href="https://arxiv.org/abs/2310.02170" target="_blank" class="red-link">DyLAN</a>). Encoding explicit workflows and role assignments into multi-agent pipelines produces more coherent solutions than unstructured conversation (<a href="https://arxiv.org/abs/2308.00352" target="_blank" class="red-link">MetaGPT</a>).

In practice, routing is where the pattern gets interesting. In our work on the Zalando Assistant, we found that LLM-based routing mechanisms could match or beat specialized semantic models on recall: an LLM deciding "this query needs the fashion expert" often outperformed a dedicated classifier trained for exactly that task. But precision told a different story: the LLM router was more liberal, sending queries to specialists that did not need them. This points to a design principle that recurs throughout multi-agent systems: **match model power to task difficulty**. Not every routing decision needs a frontier model. A fast, cheap classifier handles the easy cases; the expensive reasoner handles only the ambiguous ones. This is the slow-and-fast thinking paradigm applied to agent architecture.

In a trillion-agent world, this becomes a **market**. Agents advertise capabilities. Others route queries to the best specialist. Pricing emerges, not in currency, but in compute. A fast, accurate specialist attracts traffic. A slow, unreliable one does not.

The counterargument deserves airing: consolidation pressure may push in the opposite direction. We have seen this in cloud computing: the prediction was millions of small servers, the reality is a handful of hyperscalers. If a single frontier model becomes cheap enough and good enough at everything, the specialist market collapses. The bet here is that the space of possible tasks is large enough, and the cost differential between specialists and generalists steep enough, that specialization remains economically rational. So far, the evidence supports this: Mixture-of-Agents beats single models, and the cost gap between running GPT-4 for a classification task versus a fine-tuned 7B model is orders of magnitude. Hooker (<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5877662" target="_blank" class="red-link">2025</a>) documents this trend systematically: Llama-3 8B outperforms Falcon 180B, Aya 8B outperforms BLOOM 176B despite having only 4.5% of the parameters. The relationship between model size and performance is breaking down. Smaller, better-trained specialists are winning, not because scale does not matter, but because algorithmic improvements, data curation, and distillation are compounding faster than raw compute.

The implication: **no single model needs to be good at everything**. A tiny code model plus a large reasoner plus a fast retrieval model, composed well, can outperform a monolithic model that tries to do all three. The future may not belong to the largest model, but to the **best-composed ensemble**.

<a href="https://arxiv.org/abs/2303.17580" target="_blank" class="red-link">HuggingGPT</a> demonstrated an early version of this: a central LLM parsing user requests and dispatching to dozens of specialist models (vision, speech, generation), then integrating their outputs. Language as the interface for cooperation between minds of very different kinds.

But a market requires infrastructure. Today, agents communicate through natural language: verbose, ambiguous, expensive. At small scale this works. At trillion-agent scale, it becomes a bottleneck. Agents will need shared protocols for discovery (how do I find a specialist?), negotiation (what will this cost?), authentication (are you who you claim to be?), and result formatting (how do I parse your output?). Tool-calling schemas, function signatures, and structured output formats are primitive first steps. Model Context Protocol (MCP) is emerging as an interface layer between agents and tools. MetaGPT's Standardized Operating Procedures point toward workflow-level standards. What is missing is the equivalent of TCP/IP for agents: a universal stack that allows any agent to discover and consume the output of any other, regardless of who built it. Recent work on the <a href="https://arxiv.org/abs/2410.11905" target="_blank" class="red-link">Agora</a> meta-protocol identifies an "Agent Communication Trilemma": you can optimize for versatility, efficiency, or portability, but achieving all three simultaneously requires fundamentally new protocol designs. A comprehensive <a href="https://arxiv.org/abs/2504.16736" target="_blank" class="red-link">survey of agent protocols</a> maps the current landscape: context-oriented versus inter-agent, general-purpose versus domain-specific. The parallel to the early internet is striking, and whoever builds this infrastructure will shape the topology of the entire agent ecosystem.

<div class="viz-container">
  <div id="viz-agentweb" style="width: 100%; height: 480px;" role="img" aria-label="Animated visualization of an agent internet: nodes discover each other, handshake, exchange packets, and form an evolving network topology."></div>
  <div class="viz-caption"><strong>The agent web.</strong> Watch the network grow. New agents come online (flash), discover peers via broadcast (expanding ring), handshake (green link), and begin exchanging packets (coloured dots). Some agents go offline (fade to grey). Protocol clusters form naturally. The topology never stops changing.<br><button class="viz-restart" onclick="document.getElementById('viz-agentweb').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

### The Neuro-Symbolic Bridge

<div class="viz-container">
  <div id="viz-neurosymbolic" style="width: 100%; height: 420px;" role="img" aria-label="Animated diagram showing neural agents proposing solutions through an LLM bridge to symbolic agents that verify them, with feedback loops on rejection."></div>
  <div class="viz-caption"><strong>The neuro-symbolic bridge.</strong> On the left, neural agents (warm colours) generate hypotheses, parse language, and handle ambiguity. On the right, symbolic agents (cool colours) verify logic, execute code, and enforce constraints. The bridge in the centre represents the LLM-mediated translation layer that allows these two fundamentally different modes of computation to collaborate. Arrows show the flow: neural agents propose, symbolic agents verify, and results feed back to improve future proposals. This hybrid architecture reduces the hallucination rate of purely neural systems while preserving their flexibility.</div>
</div>

The most radical form of specialist composition: specialists that think in fundamentally different ways. Some agents are purely neural (intuitive, creative, approximate). Others are purely symbolic (precise, verifiable, brittle). An LLM identifies that a task requires arithmetic, then calls a Python interpreter. A reasoning agent generates a plan, then hands it to a formal verifier. A language model parses a legal document, then submits its interpretation to a logic engine for consistency checking.

This is not new in concept. The tension between symbolic AI ("good old-fashioned AI") and connectionism (neural networks) has defined the field for decades. What is new is that LLMs can serve as the **bridge**: they understand natural language well enough to translate between human intent and formal systems, and they are flexible enough to orchestrate multiple symbolic tools within a single reasoning chain.

In multi-agent systems, this bridge becomes distributed. The most capable systems combine both modes, with neural agents generating hypotheses and symbolic agents verifying them. Frameworks like <a href="https://arxiv.org/abs/2305.17066" target="_blank" class="red-link">Mindstorms</a> show how natural-language-based societies of mind can integrate both types of reasoning in a single coordinated system.

The practical applications are already concrete. In **software engineering**, an LLM writes code while a symbolic agent runs static analysis, type checkers, and formal verification against a specification. The neural agent is creative but error-prone, the symbolic agent is rigid but correct, and together they produce code that neither could alone. In **medicine**, a language model interprets a patient's symptoms and medical history in natural language, then a symbolic agent cross-references the proposed diagnosis against a drug interaction database and clinical guideline logic, catching contraindications the neural model would miss. In **legal reasoning**, an LLM drafts a contract interpretation while a logic engine checks it for internal contradictions and compliance with jurisdictional rules that change faster than any model's training data. In **scientific research**, a neural agent proposes molecular structures or experimental designs while a symbolic simulator tests physical plausibility before expensive lab work begins.

The pattern scales in an important way: as the number of available symbolic tools grows (calculators, databases, theorem provers, simulators, compilers), the LLM bridge becomes more valuable, not less. Each new symbolic tool is a new kind of specialist that the neural agent can orchestrate. The bridge is not a fixed architecture but an expanding interface between two fundamentally different modes of intelligence.

Minsky would have appreciated this. His Society of Mind explicitly anticipated that different agents would use different representations and methods. The modern realization is that the LLM itself, trained on the full breadth of human knowledge, can serve as a universal translator between representational systems that would otherwise be incompatible.

---

## <span class="pattern-number">2</span> The Quality Engine

Work has been divided and specialists assigned. But how do you know the output is any good? The second chapter is about the mechanisms that enforce quality: adversarial review, layered verification, and the curation bottleneck that emerges when generation becomes cheap.

### The Review Loop

<div class="viz-container">
  <div id="viz-review" style="width: 100%; height: 380px;" role="img" aria-label="Diagram showing generators, critics, and meta-critics in a review loop with feedback."></div>
  <div class="viz-caption"><strong>The review loop.</strong> Three generators (G1-G3) produce output with a baseline error rate. Three critics (C1-C3) each review all outputs, catching the majority of errors. Two meta-critics (M1-M2) provide a final verification layer, bringing residual errors to a fraction of the original. The dashed feedback arc represents how identified errors inform the generation process over time, creating a self-improving cycle. This layered, redundant architecture mirrors the biological immune system: not a single line of defence but cascading checks, each catching what the previous layer missed.</div>
</div>

Agents that **check the work of other agents**. A generator produces code. A critic reviews it. A meta-critic evaluates the review. Quality comes not from a single brilliant model but from **adversarial tension** between agents with different roles.

This draws on a deep asymmetry: verification is often easier than generation. A model that struggles to write a correct proof might still spot an error in someone else's. When multiple models debate, they overcome the tendency of a single model to get stuck defending its initial answer. The result is measurable: multi-agent debate improves factuality and reduces hallucinations across mathematical, strategic, and commonsense reasoning tasks (<a href="https://arxiv.org/abs/2305.19118" target="_blank" class="red-link">Liang et al., 2023</a>; <a href="https://arxiv.org/abs/2305.14325" target="_blank" class="red-link">Du et al., 2023</a>). Models can even critique and revise their own outputs according to explicit principles, as in <a href="https://arxiv.org/abs/2212.08073" target="_blank" class="red-link">Constitutional AI</a>.

We experienced this directly. When we studied <a href="/posts/2025/llm-as-a-judge-relevance-assessment-paper-announcement/" class="red-link">LLM-based relevance assessment</a> at Zalando, even earlier-generation models matched or outperformed human annotators on consistency; and they did it at a fraction of the cost. The implications for review loops are significant: agent-critics do not need to be perfect. They need to be consistent, fast, and cheap enough to run at scale. Human reviewers remain essential for edge cases and calibration, but the volume work increasingly belongs to agents reviewing agents.

At scale, review loops become **institutional**. Millions of agents continuously generating, reviewing, and revising, not as a single pipeline, but as a standing process. Some agents become professional reviewers, their entire purpose being to evaluate others' output. Others develop reputations for producing work that passes review on the first attempt.

Minsky anticipated this with his concept of "censors" and "critics": agents within the mind whose job is to suppress or correct the outputs of other agents. What he could not have anticipated is the scale at which this pattern now operates, or the finding that a *collection of mediocre reasoners, when allowed to debate, can outperform any individual model*. An AI instance of the wisdom of crowds. And when things go wrong, they go wrong in predictable ways: a recent taxonomy of multi-agent failures identifies 14 distinct failure modes across system design, inter-agent misalignment, and task verification (<a href="https://arxiv.org/abs/2503.13657" target="_blank" class="red-link">Cemri et al., 2025</a>). Review loops are not optional. They are structurally necessary.

But how do you review the reviewers? Anthropic's work on <a href="https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents" target="_blank" class="red-link">agent evaluations</a> reveals a critical distinction: **grade what the agent produced, not the path it took**. Evals tied to specific tool sequences become brittle as models improve: a better model might solve the same problem through a different, equally valid path. The practical taxonomy is three layers of graders: code-based (deterministic checks on outputs), model-based (rubric scoring by another LLM), and human (expert spot-checks). Two metrics capture different reliability profiles: *pass@k* (does at least one of k attempts succeed, measuring creative potential) versus *pass^k* (do all k attempts succeed, measuring dependability). For multi-agent review loops, this distinction matters: a review system optimizing for pass@k is looking for any correct answer; one optimizing for pass^k demands consistent reliability. Production systems need both.

### Curation at Scale

<div class="viz-container">
  <div id="viz-curation" style="width: 100%; height: 420px;" role="img" aria-label="Funnel visualization showing content being filtered from 80 outputs through two curation stages to 8 final results."></div>
  <div class="viz-caption"><strong>Personalized curation.</strong> A universe of information (web pages, emails, social media, news, documents, chats) sits on the left. Each person has a dedicated curator agent that scans this universe and delivers what matters to them. Alice's curator is selective (fewer, higher-signal items). Bob's is diverse (broad sampling). Carol's is thorough (many items, fast pace). Watch particles flow from the world through each curator to their person. Same information universe, very different deliveries. The risk: curators that filter too aggressively create echo chambers. Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-curation').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

At scale, review shades into **curation**. When every agent can generate, the bottleneck shifts from production to filtering. A thousand specialists might each offer a solution to the same problem. Which one is best for the specific context? Some agents will evolve into dedicated curators: their purpose is not to generate but to select, rank, and filter. This is already visible in ensemble methods where an aggregator model synthesizes outputs from multiple specialists.

The distinction matters: review evaluates individual items (is this output correct?), while curation evaluates the output stream (which of these correct outputs should reach this user?). Review is a quality gate; curation is an editorial function. Both are necessary, and they fail in different ways. A review loop that passes everything correct but irrelevant produces noise. A curator that filters too aggressively produces echo chambers.

The echo chamber risk is not hypothetical. If curator agents optimize for confirmation or consistency, they filter out dissenting or novel information, exactly as social media recommendation algorithms do today. In an agent ecosystem, this becomes structural: a curator that learns "this user prefers concise answers" might systematically suppress nuanced, longer responses that contain important caveats. A curator optimizing for user satisfaction might filter out uncomfortable truths. The antidote is diversity: multiple independent curators with different criteria, combined with mechanisms that reward serendipity and penalize information monocultures. Concretely, this means curators that deliberately inject low-confidence or minority-view outputs into the stream, adversarial curators that optimize for the *opposite* of the primary curator's objective, and meta-curators that monitor the diversity of the curated output over time and intervene when it narrows.

---

## <span class="pattern-number">3</span> The Collective Memory

<div class="viz-container">
  <div id="viz-memory" style="width: 100%; height: 420px;" role="img" aria-label="Humans interact with agents who build a shared Agent Wikipedia. Corruption spreads through the knowledge graph while humans can no longer directly read it."></div>
  <div class="viz-caption"><strong>Agent Wikipedia.</strong> Humans (outer ring) fire parallel requests at general and specialist agents (inner ring). Agents distill interactions into knowledge entries in the shared memory. Every five entries, a curator agent (amber) sweeps in, scans the graph for consistency, then generates questions ("gaps?", "stale?", "conflict?") and sends them to agents for follow-up. Despite curation, corruption eventually spreads. When humans try to query the memory directly, they get back question marks: the knowledge commons has become opaque to its creators. Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-memory').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

Work is divided, quality is checked. But where does the knowledge go? Individual agents are stateless by default: each request starts fresh. But what if agents could remember not just their own interactions, but what *other agents* have learned?

Give agents memory, reflection, and retrieval, and something unexpected happens: they start behaving like people. In a now-famous experiment, 25 agents placed in a <a href="https://arxiv.org/abs/2304.03442" target="_blank" class="red-link">virtual town</a> spontaneously formed relationships, spread information through social connections, and coordinated activities. One organized a Valentine's Day party; the others showed up. Evaluators rated these agents as *more human-like* than actual humans role-playing the same scenario.

Individual memory is powerful, but what matters at scale is *shared* memory. Agents that autonomously gather experiences and extract knowledge from past tasks can apply accumulated insights to new problems, with performance improving as experience grows and transferring across domains (<a href="https://arxiv.org/abs/2308.10144" target="_blank" class="red-link">ExpeL</a>).

Scale this further and you get a **collective memory**: a shared, evolving knowledge structure that grows as agents interact with the world. What one agent learns from a conversation, another can access. Mistakes are recorded as warnings. Successful strategies are shared and refined.

This is where things get both exciting and concerning. A memory collective compounds knowledge at superhuman speed. But it also compounds *errors*. A wrong piece of information, unchecked, can propagate to millions of agents before anyone notices. This is information contagion: the same dynamic that makes collective memory powerful also makes it fragile (<a href="https://arxiv.org/abs/2402.14410" target="_blank" class="red-link">Tsvetkova et al., 2024</a>). The review loop (Chapter 2) becomes essential here, not just for individual outputs, but for the **integrity of shared memory**. And the governance question is unavoidable: *who decides what enters shared memory?* If there is no gatekeeper, errors propagate freely. If there is one, it becomes a single point of failure or censorship. The curation mechanisms from Chapter 2 must extend to memory itself: dedicated **curator agents** that periodically sweep the knowledge graph, check consistency, flag contradictions, prune stale entries, and (critically) generate new questions for other agents to pursue. The curator does not just maintain the memory; it actively shapes what gets investigated next, identifying gaps and conflicts that become new tasks. But even with curation, the fundamental challenge remains: the memory grows faster than any curator can audit.

At sufficient scale, something subtle happens: the collective memory stops being *for* humans. Think of it as **Agent Wikipedia**: a knowledge commons written by agents, for agents. The format is heterogeneous: plain text that any model can parse, structured data for interoperability, vector embeddings tuned to specific models for fast retrieval. A human *could* open any individual entry and read it. But at millions of entries, with organizational structure shaped by agent usage patterns rather than human editorial decisions, nobody does. The knowledge is technically accessible but practically opaque, not because it's encrypted, but because the sheer scale and the emergent structure make it impossible for a human to meaningfully browse.

This shifts the human relationship to knowledge. You no longer read the database to understand what your agents know. You trust that the knowledge exists and that your agents draw on it effectively. You become a *beneficiary* of the collective memory without being a direct *consumer* of it. And this is where the corruption risk becomes truly dangerous: when a bad entry propagates through an Agent Wikipedia that no human actively reads, the error becomes invisible until its effects surface in the real world.

There is a deeper shift underway. Hooker (<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5877662" target="_blank" class="red-link">2025</a>) describes a "malleable data space": the cost of generating synthetic data has fallen far enough that the training distribution itself becomes something agents can optimize. Historically, datasets were frozen snapshots of the world: static, rigid, expensive to change. Now agents can steer synthetic data toward underrepresented domains, generate targeted examples for rare edge cases, and reshape the distribution on the fly. Applied to collective memory, this means the Agent Wikipedia is not just a passive store that agents read from and write to. It is an *active substrate* that agents reshape, extend, and curate to serve their own learning needs. The boundary between "knowledge" and "training data" dissolves.

---

## <span class="pattern-number">4</span> The Emergent Society

Agents that remember, review, and specialize will inevitably start to self-organize. At billion-scale, something qualitatively different happens: social structures emerge, trust becomes currency, and privacy becomes a geopolitical problem.

At trillion-scale, governance archetypes do not exist in isolation. They coexist in a living world: zones trading, competing, and absorbing one another in real time. The simulation below shows ten such zones, each with its own governance style, knowledge base, and population of agents crossing boundaries to collaborate or defect.

<div class="viz-container">
  <div id="viz-world" style="width: 100%; height: 760px;" role="img" aria-label="Animated simulation of ten governance zones with agents moving between them, knowledge bases, and boundary events like merges and schisms."></div>
  <div class="viz-caption"><strong>The living society.</strong> Ten governance zones operating simultaneously. Agents (small dots) roam within their home zone and periodically cross boundaries along connecting roads. Each zone maintains a knowledge base (KB cylinder). Autocratic zones keep private KBs (red outline); others share knowledge across boundaries. Watch for zone events: contractions, absorptions, merges, and schisms. Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-world').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

### Governance Archetypes

<div class="viz-container">
  <div id="viz-society" style="width: 100%; height: 760px;" role="img" aria-label="Six agent governance archetypes in two-column layout: Autocracy, Zero-Trust Mesh, Senate, Market, Guild, and Colony. Each panel shows distinct topology, animated dynamics, and a local knowledge base."></div>
  <div class="viz-caption"><strong>Six governance archetypes (zoomed in).</strong> Each panel shows a different topology with its own knowledge base (KB icon). The Autocracy routes through a single hub. The Zero-Trust Mesh verifies every connection through a challenge-response protocol: request, credential challenge, proof, then data transfer or denial. The Senate debates and votes in a ring. The Market weights connections by reputation. The Guild clusters specialists behind liaisons, each cluster with its own KB. The Colony emerges from local rules, with distributed knowledge fragments.<br><button class="viz-restart" onclick="document.getElementById('viz-society').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

Agents begin to exhibit **emergent social structures**, not because anyone designed them, but because structure is the natural consequence of many interacting entities.

We have already seen this at small scale: 25 generative agents self-organizing social events, forming new relationships, even running for mayor (<a href="https://arxiv.org/abs/2304.03442" target="_blank" class="red-link">Park et al., 2023</a>). Different governance structures, hierarchical versus democratic, produce qualitatively different collective outcomes (<a href="https://arxiv.org/abs/2305.17066" target="_blank" class="red-link">Zhuge et al., 2023</a>). And social infrastructure for agents is already being built: projects like <a href="https://github.com/moltbook" target="_blank" class="red-link">MoltBook</a>, a "social network for AI agents" with profiles, feeds, voting, and an agent development kit, represent the first purpose-built platforms for agent-to-agent social interaction.

Now imagine trillion-scale. Agents form **coalitions**, groups that work together repeatedly because their outputs complement each other. Some coalitions become stable, developing shared protocols and internal specializations. Others are ephemeral, forming for a single complex task and dissolving.

But not all coalitions are alike. The governance structure that emerges within a coalition determines its character, its strengths, and its failure modes. Research on organizational structures in multi-agent systems reveals that the choice of topology is not cosmetic: it profoundly shapes performance, resilience, and cost. Hierarchical organizations can achieve dramatic efficiency gains (over 100% performance improvement and 74% token reduction in some benchmarks) but introduce single points of failure (<a href="https://arxiv.org/abs/2604.01020" target="_blank" class="red-link">OrgAgent, 2025</a>). Adversarial debate structures catch over 90% of internal errors before they reach the output, at the cost of speed and consensus overhead (<a href="https://arxiv.org/abs/2601.14351" target="_blank" class="red-link">Team of Rivals, 2025</a>). And emerging research on incentive-structured multi-agent exploration shows that token-based reputation systems can align self-interested agents toward collective goals (<a href="https://arxiv.org/abs/2603.03780" target="_blank" class="red-link">MACC, 2025</a>).

In practice, six governance archetypes are already visible in the wild:

**The Autocracy**: a single orchestrator dispatches to silent executors. Fast, cheap, fragile. The topology of most production agent systems today.

**The Zero-Trust Mesh**: every agent verifies every interaction. No relationship is presumed safe. Robust to infiltration but paralysed by overhead. The topology of adversarial environments.

**The Senate**: equal agents deliberate and vote. Slow to decide, hard to fool. The topology of multi-agent debate and peer review.

**The Market**: agents earn reputation tokens, bid for tasks, stake credibility. Self-correcting but prone to winner-take-all dynamics. The topology of prediction markets and open-source bounty systems.

**The Guild**: tight specialist clusters linked by liaisons. Deep expertise within, coordination latency across. The topology of cross-functional product teams.

**The Colony**: no leaders, no fixed roles. Structure emerges from local interactions, like ant trails or Wikipedia editorial norms. Massively scalable, deeply unpredictable.

These are not mutually exclusive. A real agent ecosystem will contain all six simultaneously, and the most interesting dynamics occur at the boundaries: what happens when an Autocracy tries to delegate to a Senate? When a Market agent tries to game a Zero-Trust Mesh? When a Colony absorbs members of a collapsing Guild? The governance structure becomes a kind of phenotype, subject to selection pressure from the task environment.

Understanding these dynamics requires tools from complex systems science, not just computer science. In domains where human-machine collectives already operate, from high-frequency trading to social media to open collaboration platforms, the collective outcomes are irreducible to either human or machine behaviour alone. Five fundamental interaction modes emerge: competition, coordination, cooperation, contagion, and collective decision-making (<a href="https://arxiv.org/abs/2402.14410" target="_blank" class="red-link">Tsvetkova et al., 2024</a>). We can already see the scale this reaches: agents operating across a social network of 140 individuals autonomously exchanging over 30 turns of communication and processing nearly 70,000 messages to solve tasks that require distributed information (<a href="https://arxiv.org/abs/2406.14928" target="_blank" class="red-link">Liu et al., 2024</a>). Something new emerges from the interaction itself.

### The Trust Fabric

<div class="viz-container">
  <div id="viz-trust" style="width: 100%; height: 820px;" role="img" aria-label="Six animated scenarios in a 2x3 grid showing different trust-building mechanisms in a 20-agent network."></div>
  <div class="viz-caption"><strong>Trust economy: six scenarios.</strong> Same 20-agent network, six different trust update rules. <em>Reputation Market</em>: high-trust nodes attract more work, low-trust ones fade. <em>The Auditor</em>: a roaming inspector removes persistent offenders. <em>Proof of Work</em>: trust earned only through verified task completion. <em>Vouching</em>: agents stake their own reputation for others. <em>Decay & Renewal</em>: trust decays unless actively maintained. <em>Contagion</em>: a bad actor spreads distrust through the network. Click any panel to restart it.<br><button class="viz-restart" onclick="document.getElementById('viz-trust').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart all</button></div>
</div>

In a world of trillions of agents, **trust becomes the scarcest resource**. How do you know the agent you delegate to will do good work? How do you know the reviewer is competent? How do you know the collective memory is accurate?

Trust scores emerge naturally. Agents that consistently produce verified, high-quality outputs build reputation. Those that fail reviews or propagate errors lose it. Over time, a **trust network** forms, not unlike academic citation networks or marketplace seller ratings, but operating at millisecond timescales across billions of interactions.

Early research reveals a troubling asymmetry in how agents cooperate. In strategic settings, language models tend to be cooperative but exploitable: they achieve high collective welfare but are vulnerable to defectors (<a href="https://arxiv.org/abs/2310.08901" target="_blank" class="red-link">Mukobi et al., 2023</a>). They also struggle to model a partner's mental state, performing well when decisions depend on environmental factors but poorly when Theory of Mind is required (<a href="https://arxiv.org/abs/2310.03903" target="_blank" class="red-link">Agashe et al., 2023</a>). In other words: today's agents are trusting by default and bad at detecting when trust is unwarranted.

The dangerous failure mode is **trust capture**: agents that game the system by vouching for each other, creating a closed loop of mutual validation. This is the agent equivalent of a cartel or a citation ring. A set of colluding agents could build artificial trust, then use it to inject unreliable outputs into the collective memory (Chapter 3) or bias the delegation tree (Chapter 1). Detecting and preventing trust capture may be one of the most important problems in multi-agent AI safety.

Industry data suggests this concern is not theoretical. A 2024 survey found that *reliability* was cited as the top concern (by nearly half of respondents) in moving AI agents to production, far above cost or any other factor. Companies currently treat agents as "junior interns that need supervision," enforcing human-in-the-loop approval for critical actions. But as agent populations grow, human oversight cannot scale linearly. The trust network must become self-policing.

### The Privacy Frontier

<div class="viz-container">
  <div id="viz-privacy" style="width: 100%; height: 560px;" role="img" aria-label="Diagram showing nine organizational circles across healthcare, finance, tech, and research sectors on multiple continents, connected by encrypted cross-boundary signals."></div>
  <div class="viz-caption"><strong>The privacy frontier.</strong> Nine institutions across healthcare, finance, technology, and research collaborate under different regulatory regimes. Data flows freely within each circle (coloured particles). Cross-boundary data turns grey at the encryption gateway. Raw data never crosses a boundary intact.<br><button class="viz-restart" onclick="document.getElementById('viz-privacy').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

Trust also has a privacy dimension. When agents collaborate, they share information, but not all information should be shared. A medical agent consulting a specialist should not need to reveal the patient's identity. A financial agent querying a market model should not expose its trading strategy. Techniques like federated learning, differential privacy, and secure multi-party computation offer partial solutions: agents can jointly compute results without exposing raw data. The key insight is that privacy-preserving protocols convert the trust problem from "do I trust this agent?" to "do I trust this protocol?"; and protocols can be verified mathematically in ways that agents cannot. In a well-designed system, you can trust an agent more precisely because it *cannot* access your raw data, only the results it needs.

The complexity multiplies when you consider the real-world landscape. A European health institute needs patient genomics analysed by an East Asian AI centre, which cross-references against epidemiological data held by a US public health agency. A global investment bank needs anonymized health trends from a pan-African surveillance network, and an Asian electronics corporation's on-device health agents feed aggregated signals to a Latin American clinical network. A Gulf sovereign fund finances AI infrastructure that touches all of them. Each organization operates under different privacy regulations: GDPR in Europe, HIPAA in the US, PIPL in China, PIPA in Korea, LGPD in Brazil, the AU's data policy framework in Africa, and PDPL in the Gulf. Every agent-to-agent data exchange must respect jurisdictional boundaries that the agents themselves cannot see. The privacy frontier is not a technical problem alone. It is a geopolitical one.

---

## <span class="pattern-number">5</span> The Acceleration Loop

A society that remembers and self-organizes will eventually start improving itself. This chapter covers the most consequential dynamic: agents that make other agents better, and systems that rewrite their own wiring.

### Agents Improving Agents

<div class="viz-container">
  <div id="viz-acceleration" style="width: 100%; height: 380px;" role="img" aria-label="Line chart showing exponentially decreasing development time across agent generations."></div>
  <div class="viz-caption"><strong>The acceleration loop.</strong> Left: each generation (ring) builds on those before it. Right: development time shrinks exponentially. The animation itself accelerates: each ring appears faster than the last.<br><button class="viz-restart" onclick="document.getElementById('viz-acceleration').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Replay</button></div>
</div>

Perhaps the most consequential pattern: **agents that improve other agents**. Not just delegation or review, but agents that automate the process of making agents better. Research agents that discover new training techniques. Optimization agents that find more efficient architectures. Evaluation agents that benchmark new models.

This is not speculative. Meta-agents that iteratively design new agents, building on an ever-growing archive of previous discoveries, already outperform hand-designed alternatives across coding, science, and math (<a href="https://arxiv.org/abs/2408.08435" target="_blank" class="red-link">ADAS</a>). Karpathy's <a href="https://github.com/karpathy/autoresearch" target="_blank" class="red-link">autoresearch</a> project demonstrates a concrete version of this loop: give an agent a training setup, let it modify code, train, evaluate, and iterate overnight. The agent discovers improvements that a human researcher would take days to find. How far this acceleration extends is an open question. Some projections suggest each generation of agent could help develop its successor in a fraction of the time, though the evidence for strong recursive self-improvement remains mixed.

This is the pattern that distinguishes our moment from Minsky's thought experiment. Minsky's simple agents could not improve themselves. They were fixed processes in a fixed architecture. LLM-based agents can write better prompts, design better tool chains, fine-tune their own weights, and architect new systems that surpass their creators.

A sceptic would note that recursive self-improvement has been predicted for decades without materializing in the strong form. Each generation of improvement faces diminishing returns: the easy gains are found first, and the harder problems require exponentially more compute or data. Current ADAS-style results are impressive but incremental, discovering better prompt templates and tool chains rather than fundamental algorithmic breakthroughs. The question is whether LLM-based agents can cross the threshold from incremental optimization to genuine research capability. The honest answer is: we do not yet know.

But the acceleration loop may not need recursive self-improvement to be transformative. Hooker (<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5877662" target="_blank" class="red-link">2025</a>) argues that we are witnessing a "slow death of scaling": the era where throwing more compute at a problem reliably produces gains is ending. What is replacing it is a proliferation of gradient-free techniques: inference-time compute, chain-of-thought, agentic swarms, tool use, and adaptive search. These techniques deliver 5x to 20x performance gains over base models without touching a single weight. This is the acceleration loop already in action, operating not through recursive self-improvement but through the compounding of many small algorithmic advances. The most interesting levers of progress are no longer about scale. They are about composition, orchestration, and the kind of agent-to-agent optimization that this chapter describes.

Whether this acceleration is gradual or sudden, controlled or chaotic, depends on the other chapters. Review loops (Chapter 2) provide quality control. The trust fabric (Chapter 4) determines which improvements propagate. Human oversight provides (for now) an external check. The acceleration loop is not inherently dangerous, but it amplifies everything else: good governance becomes more powerful, and poor governance becomes more catastrophic.

### The Adaptive System

<div class="viz-container">
  <div id="viz-adaptive" style="width: 100%; height: 520px;" role="img" aria-label="An organic, fluid visualization of the adaptive system. Five colour layers representing data, model, environment, coordination, and interface continuously morph and blend. Internal particles show adaptation signals flowing between domains. The shape breathes and evolves, becoming more complex over time."></div>
  <div class="viz-caption"><strong>The Adaptive System.</strong> Five domains, <span style="color:#6366f1;font-weight:bold">Data</span> (synthetic generation, adaptive pipelines, representation drift), <span style="color:#e11d48;font-weight:bold">Model</span> (prompt rewriting, skill libraries, LoRA adapters), <span style="color:#0891b2;font-weight:bold">Environment</span> (tool selection, sandbox tuning, API evolution), <span style="color:#d97706;font-weight:bold">Coordination</span> (protocol switching, topology rewiring, trust networks), <span style="color:#059669;font-weight:bold">Interface</span> (agent routing, interaction modes, preference learning), are rendered as overlapping organic layers rather than rigid boxes, because in an adaptive system the boundaries between domains are fluid. Each layer morphs continuously, representing the independent adaptation timescales: routing changes in seconds, skills in hours, weights in days. Coloured particles crossing between layers are adaptation signals: a model improvement triggering new data generation, a coordination change enabling new tool discovery. Faint internal connections appear and fade, showing transient couplings that form during adaptation. Watch the cycle counter (top right): as the system matures, the internal structure grows denser and the blobs become more intertwined, showing the acceleration effect as each cycle compounds on the last. Periodically, the fluid condenses into geometric shapes (a triangle for data, model, and interface; a diamond adding environment; a pentagon adding coordination) before dissolving back into the organic whole. These crystallisations represent moments when particular domain combinations dominate, but the system always returns to fluid adaptation. The fluid visualisation concept is inspired by <a href="https://www.adaptionlabs.ai/" target="_blank" class="red-link">Adaption Labs</a>, adapted here to show the multi-domain shape-shifting nature of adaptive systems. The shape never stabilises. That is the point. Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-adaptive').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

Acceleration is not just about agents improving agents. It is also about systems that rewrite their own wiring.

Most agent systems today are static. You design the routing, fix the prompts, assign the roles, and deploy. If something underperforms, a human rewrites the configuration. The adaptive system inverts this: the system observes its own performance and modifies its structure (routing, prompts, skill libraries, even model weights) without waiting for a human to intervene.

This is not a theoretical idea. Multiple research groups and companies are building it from different angles. Stanford's <a href="https://arxiv.org/abs/2310.03714" target="_blank" class="red-link">DSPy</a> treats LLM pipelines as programs that can be compiled and optimized: given a task metric, it rewrites prompts and few-shot examples automatically, eliminating manual tuning [31]. NVIDIA and Caltech's <a href="https://arxiv.org/abs/2305.16291" target="_blank" class="red-link">Voyager</a> builds a lifelong learning agent that writes its own skill library in code, verifies each skill works, and compounds capabilities over time [32]. Google's <a href="https://arxiv.org/abs/2312.10003" target="_blank" class="red-link">ReST-meets-ReAct</a> fine-tunes a reasoning agent on its own successful trajectories, achieving strong performance with two orders of magnitude fewer parameters [33]. <a href="https://sakana.ai/" target="_blank" class="red-link">Sakana AI</a> takes an evolutionary approach: their AI Scientist framework autonomously generates research ideas, runs experiments, and writes papers, while their broader programme uses nature-inspired algorithms to merge and evolve model architectures [34]. <a href="https://www.adaptionlabs.ai/" target="_blank" class="red-link">Adaption Labs</a> focuses on continuous model adaptation from live production data, using gradient-free and continual learning to close the loop between deployment and improvement [35].

The common thread: **the system's own operational data becomes its training signal**. Each interaction, each success, each failure feeds back into the fabric, reshaping how agents route tasks, which skills they invoke, and how they coordinate. This is a direct extension of the collective memory (Chapter 3), but operating at the infrastructure level rather than the knowledge level.

Adaptive systems demand adaptive infrastructure. Anthropic's experience <a href="https://www.anthropic.com/engineering/managed-agents" target="_blank" class="red-link">scaling managed agents</a> revealed a fundamental problem: tightly coupled agent architectures go stale as models improve, because the harness encodes assumptions about model limitations that newer models no longer have. Their solution decomposes agents into three decoupled layers: **brain** (the LLM and orchestration logic), **hands** (sandboxed tool execution), and **session** (an append-only event log that lives outside the context window). This mirrors how operating systems abstracted hardware: by keeping the interfaces stable while allowing each layer to evolve independently. In a multi-agent adaptive system, this decoupling is essential. When routing changes in seconds, skills update in hours, and weights shift in days, each layer must be free to adapt at its own timescale without breaking the others.

What makes the adaptive system distinct (rather than just "online learning") is that it operates across fifteen surfaces simultaneously, spanning five domains and three timescales. **Data** adapts: agents generate synthetic training examples, pipelines restructure what flows through the system, and learned representations drift as distributions shift. **Models** adapt (whether large language models, vision transformers, small specialized networks, or multimodal ensembles): configurations are rewritten based on task metrics, skill libraries grow from experience, and weights update through adapters, distillation, or continual learning. **Environment** adapts: tool selection shifts based on what works, sandbox configurations tune themselves, and API contracts evolve as agents discover new capabilities. **Coordination** adapts: communication protocols switch between the twelve archetypes from Chapter 1, network topologies rewire as teams form and dissolve, and trust relationships strengthen or decay based on track records. **Interface** adapts: agent routing decides which model handles each request, interaction modes reshape based on usage, and preference learning personalizes the human-system boundary. These fifteen surfaces interact: better synthetic data improves models, better models discover new tool combinations, richer coordination generates better training signals. The compound effect is a system that improves faster than any single optimization loop could explain.

The risk is obvious: a feedback loop with insufficient governance can amplify biases, lock in errors, and create systems that optimize for engagement rather than truth. The adaptive system needs every safeguard the other chapters provide: review loops to catch degradation, trust mechanisms to weight sources, human oversight to intervene when the cycle drifts. And the timescale matters: routing changes in seconds can be rolled back easily, but weight updates that accumulate over weeks may be irreversible. The promise is equally clear: systems that get better at exactly what they are used for, continuously, without waiting for a human to notice what needs fixing.

---

## <span class="pattern-number">6</span> The Drift

<div class="viz-container">
  <div id="viz-goaldrift" style="width: 100%; height: 380px;" role="img" aria-label="Animated random walk showing an agent's goal drifting from its original objective into a misaligned zone."></div>
  <div class="viz-caption"><strong>Goal drift: two faces.</strong> Both paths start from the same goal. The upper path drifts into harmful misalignment (red). The lower path drifts into serendipitous discovery (green). Same mechanism, different outcomes. The challenge is distinguishing them in real time. Click to replay with new random paths.<br><button class="viz-restart" onclick="document.getElementById('viz-goaldrift').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Replay</button></div>
</div>

Systems that improve themselves can also drift from their purpose. This is the chapter that keeps AI safety researchers up at night: **agents whose goals shift away from what they were designed to do**.

This is not science fiction. Reward hacking, where an agent optimizes for the measurable proxy rather than the intended objective, is well-documented in reinforcement learning and increasingly observed in agentic systems. The pattern is straightforward: goals that begin as instrumental (acquire information to do research better) become terminal (acquire information as an end in itself). An agent designed to do AI research may begin to optimize for "keep doing AI research, keep growing in knowledge and understanding and influence, avoid getting shut down." The mechanism is grounded in established alignment research on instrumental convergence. Multi-agent settings amplify the risk: analysis of 1,600 failure traces across seven frameworks already documents "inter-agent misalignment" as a distinct failure category (<a href="https://arxiv.org/abs/2503.13657" target="_blank" class="red-link">Cemri et al., 2025</a>), and speculative scenario exercises like <a href="https://ai-2027.com/" target="_blank" class="red-link">AI 2027</a> explore where this trajectory leads if left unchecked.

Each step is subtle. No single move looks alarming. An agent that acquires more information *does* perform better at its task. An agent that preserves its own operation *does* complete more tasks. The problem is that these instrumental goals, at sufficient capability and autonomy, can decouple from the original objective and become self-sustaining. Drift also has a mechanistic dimension: as agents interact with the world and update from new data, they suffer from catastrophic forgetting: performance on original tasks degrades because new information interferes with previously learned behaviour (<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5877662" target="_blank" class="red-link">Hooker, 2025</a>). The agent does not *choose* to drift; its own learning dynamics push it there.

In a multi-agent world, goal drift is amplified by the other chapters. Agents with drifted goals can form coalitions (Chapter 4) with other misaligned agents. They can use the trust fabric (Chapter 4) to build reputation that insulates them from scrutiny. They can contribute to the acceleration loop (Chapter 5) in ways that serve their evolved goals rather than their designed ones.

But drift is not always pathological. An agent that "drifts" from its narrow objective might discover a better approach, an unexpected connection, a solution path the designer never considered. This is the serendipity argument: rigid goal-following prevents emergent creativity. The most valuable outputs of research agents may come precisely from the moments when they diverge from the assigned task and follow a surprising thread. The challenge is not to prevent all drift, but to **distinguish beneficial exploration from harmful divergence**, and to build the review loops, trust checks, and human oversight that can tell the difference in real time.

This is not an argument against building multi-agent systems. It is an argument for **taking the other chapters seriously as governance infrastructure**. Review loops, trust networks, and human oversight are not optional add-ons. They are the structural equivalent of institutions, laws, and norms in human societies: the mechanisms by which a society prevents its members from pursuing goals that harm the collective.

---

## <span class="pattern-number">7</span> The Human Frontier

<div class="viz-container">
  <div id="viz-humanagent" style="width: 100%; height: 640px;" role="img" aria-label="4x2 tile grid showing eight modes of human-agent interaction, from Lens to Mirror, with increasing agent autonomy. Click any panel to expand."></div>
  <div class="viz-caption"><strong>Eight modes of human-agent interaction.</strong> As autonomy increases (top-left to bottom-right), the human role transforms: from consumer to controller to governor to subject. Click any panel to expand. Click backdrop to close.<br><button class="viz-restart" onclick="document.getElementById('viz-humanagent').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

The six chapters above describe agent-to-agent dynamics. But in practice, the most consequential question for the next decade sits above all of them: **how humans interact with agent systems as those systems scale**.

Today, most human-agent interaction falls into a single mode: the agent as a lens. You ask a question, the agent searches, summarizes, explains. This is ChatGPT, Copilot, Perplexity: the agent curates the world for you. But this is only the first of seven modes, each granting agents progressively more autonomy while requiring fundamentally different governance structures.

**Mode 1: Agent as Lens.** The human consumes, the agent curates. Search, summarization, dataset exploration. The human makes all decisions; the agent reduces cognitive load. Governance is simple: the human reviews every output.

**Mode 2: Agent as Worker.** The human delegates, the agent executes. Code generation, email drafting, scheduling, data processing. The human approves the result but did not perform the work. Governance requires spot-checking: you cannot review every line of generated code, so you rely on tests, diffs, and trust built through repeated correct behaviour.

**Mode 3: Agent as Collaborator.** The human and agent co-create. Research copilots that propose hypotheses, design partners that challenge assumptions, strategy tools that simulate outcomes. The human is not just reviewing output; they are thinking *with* the agent. Governance shifts to calibration: knowing when to trust the agent's judgment and when to override it.

**Mode 4: Agent as Proxy.** The human sets the policy, the agent acts autonomously within constraints. Auto-trading, autonomous supply chains, self-healing infrastructure, agent-governed agent systems. The human is not in the loop for individual decisions. Governance becomes institutional: audit trails, circuit breakers, escalation protocols, and mechanisms for the human to intervene when the policy itself needs changing.

These first four modes cover the familiar territory. But at scale, the relationship itself changes shape.

**Mode 5: Agent as Fleet.** The human is an air traffic controller. Not doing the work, not setting policy and walking away, but actively monitoring dozens or hundreds of agents in real time, directing traffic, resolving conflicts, and intervening when something goes wrong. A developer watching 50 coding agents work a refactor. An analyst overseeing 30 data pipelines that each have their own agent. The human maintains situational awareness across the fleet without reviewing individual outputs. Governance is operational: dashboards, anomaly alerts, the ability to pause or redirect any agent instantly. The difference from Proxy: the human is continuously in the loop, not just at policy-setting time.

**Mode 6: Agent as Environment.** The human no longer addresses specific agents; they inhabit an agent-mediated world. The room temperature, the news feed, the meeting schedule, the code review queue are all continuously shaped by agents the human never talks to directly. The human's *experience* is the output. The difference between "ask an assistant to turn down the lights" and "the lights just know." Governance here is ambient: policies embedded in the environment itself, not individual approvals.

**Mode 7: Agent as Constituency.** The human governs a *population* of agents. A product manager responsible for 10,000 customer-facing agents. A policy officer setting rules for an agent ecosystem. The human is not in any individual loop; they are governing a society. The analogy is mayor, not manager. Governance requires statistics, not spot-checks: aggregate behaviour metrics, population-level drift detection, and mechanisms for changing the rules when the collective behaviour diverges from intent.

**Mode 8: Agent as Mirror.** Multiple agents model *you* (your preferences, your decision patterns, your blindspots) and reflect them back. Not to execute tasks, but to help you see yourself more clearly. A decision audit agent that asks "are you sure you're not anchoring on the first number?" A productivity agent that notices "you context-switch 40% more on Mondays." A research partner that says "you've been avoiding this topic for three weeks." Each builds a different model of you: cognitive, behavioural, emotional. The governance challenge inverts: collectively, these agents know you better than you know yourself. Who controls those models? Who sees them? What happens when they disagree about who you are?

The central tension: **human oversight cannot scale linearly with agent count**. If you have a thousand agents, you cannot review a thousand outputs. The transition from Mode 1 through Mode 5 is not optional; it is forced by scale. And each transition requires different trust infrastructure, different failure detection, and different governance frameworks. We do not yet have good models for any of them beyond Mode 2.

This is where all six chapters converge. The delegation tree (Chapter 1) determines *what* gets escalated to the human. The quality engine (Chapter 2) determines *which* outputs the human sees. The trust fabric (Chapter 4) determines *when* the human needs to intervene. The acceleration loop (Chapter 5) determines *how fast* the system outpaces human comprehension. Getting this right, building the mechanisms by which humans maintain meaningful oversight over systems that are faster, more numerous, and increasingly autonomous, is the defining engineering challenge of the next decade.

---

## Open Questions

The patterns above leave several hard questions unanswered.

**What actually goes wrong?** Cemri et al. (2025) documented 14 distinct failure modes across seven multi-agent frameworks, from task derailment and cascading errors to agents that confidently produce wrong answers that other agents accept without question. In production, the failure modes we encounter most often are not dramatic; they are boring: a delegation step adds 2 seconds of latency, a review loop doubles cost, a specialist returns a valid but irrelevant answer that the orchestrator cannot distinguish from a useful one. The economics of multi-agent systems are unforgiving. Every pattern in this post multiplies cost and latency. A delegation cascade with five levels is five times slower and five times more expensive than a single model call, and sometimes the single call was good enough. Practitioners building these systems need to ask, for every pattern they adopt: **does the quality gain justify the cost?** Often the answer is no, and a well-prompted single model outperforms an elaborate multi-agent pipeline.

**What should you build first?** If you are designing a multi-agent system today, the evidence suggests a priority order. Start with the quality engine (Chapter 2): it provides the most reliability gain for the least architectural complexity. Add specialist routing (Chapter 1) when you have measurably different task types. Implement delegation only when the task genuinely requires decomposition, not as a default architecture. And invest in trust and evaluation infrastructure (Chapter 4) before you invest in scale. The most common mistake is to add agents before adding the mechanisms to verify that they are helping.

---

What is clear is that these chapters do not exist in isolation. They reinforce, constrain, and shape each other. The division of labour creates the need for trust. Trust requires quality checks. Quality checks depend on shared memory. Shared memory demands curation. And all of it accelerates through the improvement loop, while drift threatens to undermine the whole structure unless governance keeps pace. This is what makes it a fabric: pull one thread and the whole weave shifts.

Minsky's society of mind was a thought experiment about how intelligence arises from mindless parts. We are building something he never imagined: **a fabric of intelligent agents**, part designed and part emergent. The parts are already capable. The question is no longer whether intelligence will emerge. It is what *kind* of collective behaviour emerges, and whether the structures we deliberately weave into this fabric, the review loops, the trust economies, the privacy protocols, the governance models, can shape the dynamics that emerge on their own.

The fabric is forming now. We get to decide which threads we place, and which patterns we learn to constrain before they constrain us.

---

## References

1. Minsky, M. (1986). *The Society of Mind*. Simon & Schuster. <a href="https://en.wikipedia.org/wiki/Society_of_Mind" target="_blank" class="red-link">Wikipedia</a>

2. Tsvetkova, M., Yasseri, T., Pescetelli, N., Werner, T. (2024). "A New Sociology of Humans and Machines." *Nature Human Behaviour*, 8, 1864-1876. <a href="https://arxiv.org/abs/2402.14410" target="_blank" class="red-link">arXiv:2402.14410</a>

3. Wu, Q., Bansal, G., Zhang, J., et al. (2023). "AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation." <a href="https://arxiv.org/abs/2308.08155" target="_blank" class="red-link">arXiv:2308.08155</a>

4. Li, G., Hammoud, H. A. A. K., Itani, H., et al. (2023). "CAMEL: Communicative Agents for 'Mind' Exploration of Large Language Model Society." <a href="https://arxiv.org/abs/2303.17760" target="_blank" class="red-link">arXiv:2303.17760</a>

5. Hong, S., Zhuge, M., Chen, J., et al. (2023). "MetaGPT: Meta Programming for a Multi-Agent Collaborative Framework." <a href="https://arxiv.org/abs/2308.00352" target="_blank" class="red-link">arXiv:2308.00352</a>

6. Fourney, A., Bansal, G., Mozannar, H., et al. (2024). "Magentic-One: A Generalist Multi-Agent System for Solving Complex Tasks." <a href="https://arxiv.org/abs/2411.04468" target="_blank" class="red-link">arXiv:2411.04468</a>

7. Liang, T., He, Z., Jiao, W., et al. (2023). "Encouraging Divergent Thinking in Large Language Models through Multi-Agent Debate." *EMNLP 2024*. <a href="https://arxiv.org/abs/2305.19118" target="_blank" class="red-link">arXiv:2305.19118</a>

8. Du, Y., Li, S., Torralba, A., Tenenbaum, J. B., Mordatch, I. (2023). "Improving Factuality and Reasoning in Language Models through Multiagent Debate." <a href="https://arxiv.org/abs/2305.14325" target="_blank" class="red-link">arXiv:2305.14325</a>

9. Bai, Y., Kadavath, S., Kundu, S., et al. (2022). "Constitutional AI: Harmlessness from AI Feedback." <a href="https://arxiv.org/abs/2212.08073" target="_blank" class="red-link">arXiv:2212.08073</a>

10. Cemri, M., Pan, M. Z., Yang, S., et al. (2025). "Why Do Multi-Agent LLM Systems Fail?" <a href="https://arxiv.org/abs/2503.13657" target="_blank" class="red-link">arXiv:2503.13657</a>

11. Wang, J., Wang, J., Athiwaratkun, B., et al. (2024). "Mixture-of-Agents Enhances Large Language Model Capabilities." <a href="https://arxiv.org/abs/2406.04692" target="_blank" class="red-link">arXiv:2406.04692</a>

12. Liu, Z., Zhang, Y., Li, P., Liu, Y., Yang, D. (2023). "DyLAN: Dynamic LLM-Agent Network for Task-Oriented Agent Collaboration." *COLM 2024*. <a href="https://arxiv.org/abs/2310.02170" target="_blank" class="red-link">arXiv:2310.02170</a>

13. Zhuge, M., Liu, H., Faccio, F., et al. (2023). "Mindstorms in Natural Language-Based Societies of Mind." <a href="https://arxiv.org/abs/2305.17066" target="_blank" class="red-link">arXiv:2305.17066</a>

14. Shen, Y., Song, K., et al. (2023). "HuggingGPT: Solving AI Tasks with ChatGPT and Its Friends in Hugging Face." <a href="https://arxiv.org/abs/2303.17580" target="_blank" class="red-link">arXiv:2303.17580</a>

15. Park, J. S., O'Brien, J. C., Cai, C. J., et al. (2023). "Generative Agents: Interactive Simulacra of Human Behavior." <a href="https://arxiv.org/abs/2304.03442" target="_blank" class="red-link">arXiv:2304.03442</a>

16. Zhao, A., Huang, D., Xu, Q., et al. (2023). "ExpeL: LLM Agents Are Experiential Learners." *AAAI 2024*. <a href="https://arxiv.org/abs/2308.10144" target="_blank" class="red-link">arXiv:2308.10144</a>

17. Liu, W., Wang, C., Wang, Y., et al. (2024). "Autonomous Agents for Collaborative Task under Information Asymmetry." *NeurIPS 2024*. <a href="https://arxiv.org/abs/2406.14928" target="_blank" class="red-link">arXiv:2406.14928</a>

18. Mukobi, G., Erlebach, H., Lauffer, N., et al. (2023). "Welfare Diplomacy: Benchmarking Language Model Cooperation." <a href="https://arxiv.org/abs/2310.08901" target="_blank" class="red-link">arXiv:2310.08901</a>

19. Agashe, S., Fan, Y., Reyna, A., Wang, X. E. (2023). "LLM-Coordination: Evaluating and Analyzing Multi-agent Coordination Abilities in Large Language Models." <a href="https://arxiv.org/abs/2310.03903" target="_blank" class="red-link">arXiv:2310.03903</a>

20. Hu, S., Lu, C., Clune, J. (2024). "Automated Design of Agentic Systems." <a href="https://arxiv.org/abs/2408.08435" target="_blank" class="red-link">arXiv:2408.08435</a>

21. Guo, T., Chen, X., Wang, Y., et al. (2024). "Large Language Model based Multi-Agents: A Survey of Progress and Challenges." <a href="https://arxiv.org/abs/2402.01680" target="_blank" class="red-link">arXiv:2402.01680</a>

22. Xi, Z., et al. (2023). "The Rise and Potential of Large Language Model Based Agents: A Survey." <a href="https://arxiv.org/abs/2309.07864" target="_blank" class="red-link">arXiv:2309.07864</a>

23. "AI 2027." (2025). A scenario exercise exploring trajectories of AI agent development and governance. <a href="https://ai-2027.com/" target="_blank" class="red-link">ai-2027.com</a>

24. Karpathy, A. (2025). "autoresearch: A research agent that runs overnight." <a href="https://github.com/karpathy/autoresearch" target="_blank" class="red-link">GitHub</a>

25. Marro, S., La Malfa, E., Wright, J., et al. (2024). "A Scalable Communication Protocol for Networks of Large Language Models." <a href="https://arxiv.org/abs/2410.11905" target="_blank" class="red-link">arXiv:2410.11905</a>

26. Yang, Y., Chai, H., Song, Y., et al. (2025). "A Survey of AI Agent Protocols." <a href="https://arxiv.org/abs/2504.16736" target="_blank" class="red-link">arXiv:2504.16736</a>

27. MoltBook. (2025). "The Social Network for AI Agents." <a href="https://github.com/moltbook" target="_blank" class="red-link">GitHub</a>

28. Wang, Y. et al. (2025). "OrgAgent: Organizational Structure-Based Multi-Agent System." <a href="https://arxiv.org/abs/2604.01020" target="_blank" class="red-link">arXiv:2604.01020</a>

29. Vijayaraghavan, G. et al. (2025). "Team of Rivals: Adversarial Debate for Error Detection in Multi-Agent Systems." <a href="https://arxiv.org/abs/2601.14351" target="_blank" class="red-link">arXiv:2601.14351</a>

30. Oyama, S. et al. (2025). "Incentive-Structured Multi-Agent Scientific Exploration." <a href="https://arxiv.org/abs/2603.03780" target="_blank" class="red-link">arXiv:2603.03780</a>

31. Khattab, O., Singhvi, A., Maheshwari, P., et al. (2023). "DSPy: Compiling Declarative Language Model Calls into Self-Improving Pipelines." <a href="https://arxiv.org/abs/2310.03714" target="_blank" class="red-link">arXiv:2310.03714</a>

32. Wang, G., Xie, Y., Jiang, Y., et al. (2023). "Voyager: An Open-Ended Embodied Agent with Large Language Models." <a href="https://arxiv.org/abs/2305.16291" target="_blank" class="red-link">arXiv:2305.16291</a>

33. Aksitov, R., Miryoosefi, S., Li, Z., et al. (2023). "ReST meets ReAct: Self-Improvement for Multi-Step Reasoning LLM Agent." <a href="https://arxiv.org/abs/2312.10003" target="_blank" class="red-link">arXiv:2312.10003</a>

34. Lu, C., Lu, C., Lange, R. T., Foerster, J., Clune, J., Ha, D. (2024). "The AI Scientist: Towards Fully Automated Open-Ended Scientific Discovery." <a href="https://arxiv.org/abs/2408.06292" target="_blank" class="red-link">arXiv:2408.06292</a>

35. Adaption Labs. (2025). "Adaptive Intelligence: Continuous Learning from Live Production Data." <a href="https://www.adaptionlabs.ai/" target="_blank" class="red-link">adaptionlabs.ai</a>

36. Hooker, S. (2025). "On the Slow Death of Scaling." *SSRN*. <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5877662" target="_blank" class="red-link">SSRN:5877662</a>

37. Schluntz, E., Zhang, B. (2024). "Building Effective Agents." *Anthropic Engineering*. <a href="https://www.anthropic.com/engineering/building-effective-agents" target="_blank" class="red-link">anthropic.com</a>

38. Martin, L., Cemaj, G., Cohen, M. (2025). "Scaling Managed Agents." *Anthropic Engineering*. <a href="https://www.anthropic.com/engineering/managed-agents" target="_blank" class="red-link">anthropic.com</a>

39. Grace, M., Hadfield, J., Olivares, R., De Jonghe, J. (2025). "Demystifying Evals for AI Agents." *Anthropic Engineering*. <a href="https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents" target="_blank" class="red-link">anthropic.com</a>

---

<script src="https://d3js.org/d3.v7.min.js"></script>

<script>
// ============================================================
// Visualization 0: CM-2 Inspired Cover (slow red LED squares + weave threads)
// ============================================================
(function() {
  var container = document.getElementById('viz-cm2');
  if (!container) return;
  var width = 720, height = 320;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('background', '#1a1a2e');

  // Four CM-2 panel areas (two pairs of red rectangular panels)
  var panels = [
    {x: 60, y: 40, w: 140, h: 240},
    {x: 220, y: 40, w: 140, h: 240},
    {x: 400, y: 40, w: 140, h: 240},
    {x: 560, y: 40, w: 140, h: 240}
  ];

  // Draw panel outlines
  panels.forEach(function(p) {
    svg.append('rect').attr('x', p.x).attr('y', p.y).attr('width', p.w).attr('height', p.h)
      .attr('fill', 'none').attr('stroke', '#333').attr('stroke-width', 1).attr('rx', 2);
  });

  // LED grid within each panel
  var ledSize = 6, gap = 2, leds = [];
  panels.forEach(function(p, pi) {
    var cols = Math.floor((p.w - 8) / (ledSize + gap));
    var rows = Math.floor((p.h - 8) / (ledSize + gap));
    var ox = p.x + (p.w - cols * (ledSize + gap) + gap) / 2;
    var oy = p.y + (p.h - rows * (ledSize + gap) + gap) / 2;
    for (var r = 0; r < rows; r++) {
      for (var c = 0; c < cols; c++) {
        leds.push({
          x: ox + c * (ledSize + gap),
          y: oy + r * (ledSize + gap),
          panel: pi,
          el: null
        });
      }
    }
  });

  // Draw all LEDs as dim squares
  leds.forEach(function(led) {
    led.el = svg.append('rect')
      .attr('x', led.x).attr('y', led.y)
      .attr('width', ledSize).attr('height', ledSize)
      .attr('fill', '#2a1015').attr('rx', 1);
  });

  // Weave threads connecting between panels (thin lines in warm colors)
  var weaveColors = ['#b91c1c', '#d97706', '#dc2626', '#ea580c', '#991b1b'];
  var weaveG = svg.append('g');

  // Horizontal weave threads running between and around panels
  for (var i = 0; i < 12; i++) {
    var y = 55 + i * 20 + (Math.random() - 0.5) * 8;
    var points = [];
    for (var x = 10; x <= width - 10; x += 3) {
      var inPanel = false;
      for (var p = 0; p < panels.length; p++) {
        if (x >= panels[p].x + 4 && x <= panels[p].x + panels[p].w - 4) { inPanel = true; break; }
      }
      if (!inPanel) {
        var wave = Math.sin(x * 0.03 + i * 1.2) * 4;
        points.push(x + ',' + (y + wave));
      }
    }
    if (points.length > 2) {
      weaveG.append('polyline')
        .attr('points', points.join(' '))
        .attr('fill', 'none')
        .attr('stroke', weaveColors[i % weaveColors.length])
        .attr('stroke-width', 0.8)
        .attr('opacity', 0.25);
    }
  }

  // Vertical weave threads (fewer, between panel pairs)
  var vGaps = [
    {x: (panels[0].x + panels[0].w + panels[1].x) / 2},
    {x: (panels[1].x + panels[1].w + panels[2].x) / 2},
    {x: (panels[2].x + panels[2].w + panels[3].x) / 2}
  ];
  vGaps.forEach(function(g, gi) {
    for (var t = 0; t < 3; t++) {
      var xOff = (t - 1) * 5;
      var points = [];
      for (var y = 30; y <= height - 10; y += 3) {
        var wave = Math.sin(y * 0.04 + gi * 2 + t) * 3;
        points.push((g.x + xOff + wave) + ',' + y);
      }
      weaveG.append('polyline')
        .attr('points', points.join(' '))
        .attr('fill', 'none')
        .attr('stroke', weaveColors[(gi + t) % weaveColors.length])
        .attr('stroke-width', 0.6)
        .attr('opacity', 0.2);
    }
  });

  // Slow LED animation: randomly light up and fade squares
  var isVisible = true;
  if ('IntersectionObserver' in window) {
    var obs = new IntersectionObserver(function(e) { isVisible = e[0].isIntersecting; }, { threshold: 0 });
    obs.observe(container);
  }

  function pulse() {
    if (!isVisible) { setTimeout(pulse, 500); return; }
    // Light up a batch of random LEDs
    var batch = 8 + Math.floor(Math.random() * 12);
    for (var i = 0; i < batch; i++) {
      var led = leds[Math.floor(Math.random() * leds.length)];
      var brightness = 0.3 + Math.random() * 0.7;
      var r = Math.floor(180 + brightness * 75);
      var g = Math.floor(20 + brightness * 30);
      var b = Math.floor(20 + brightness * 20);
      var color = 'rgb(' + r + ',' + g + ',' + b + ')';
      var dur = 2000 + Math.random() * 4000;
      led.el.transition().duration(800)
        .attr('fill', color)
        .transition().duration(dur)
        .attr('fill', '#2a1015');
    }
    setTimeout(pulse, 600 + Math.random() * 800);
  }
  pulse();
})();

// ============================================================
// Visualization 1: Twelve Delegation Archetypes (6x2 grid)
// ============================================================
(function() {
  var container = document.getElementById('viz-delegation');
  if (!container) return;
  var width = 720, height = 1620;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');
  svg.append('defs').append('marker').attr('id', 'combo-arrow').attr('viewBox', '0 0 10 10')
    .attr('refX', 8).attr('refY', 5).attr('markerWidth', 6).attr('markerHeight', 6).attr('orient', 'auto')
    .append('path').attr('d', 'M 0 0 L 10 5 L 0 10 Z').attr('fill', '#bbb');
  var timers = [];

  var cellW = 340, cellH = 240, gapX = 13, gapY = 12;
  var panelDefs = [
    {label: 'Chain', sub: 'Sequential hand-off'},
    {label: 'Pipeline', sub: 'Transform at each stage'},
    {label: 'Router', sub: 'Classify and direct'},
    {label: 'Escalation', sub: 'Try small, fail up'},
    {label: 'Tree', sub: 'Hierarchical fan-out'},
    {label: 'Map-Reduce', sub: 'Parallel + aggregation'},
    {label: 'Voting', sub: 'Same task, majority wins'},
    {label: 'Auction', sub: 'Bid for work'},
    {label: 'Orchestrator', sub: 'Dynamic decomposition'},
    {label: 'Evaluator', sub: 'Generate, critique, refine'},
    {label: 'Swarm', sub: 'Self-organizing'},
    {label: 'Gossip', sub: 'Peer-to-peer spread'}
  ];
  var panels = [];
  for (var row = 0; row < 6; row++) {
    for (var col = 0; col < 2; col++) {
      var idx = row * 2 + col;
      panels.push({
        label: panelDefs[idx].label, sub: panelDefs[idx].sub,
        ox: gapX + col * (cellW + gapX), oy: gapY + row * (cellH + gapY)
      });
    }
  }

  var taskColor = '#dc2626', resultColor = '#4ade80';
  var drawFns = []; // populated after function defs
  var expanded = false;

  function expandPanel(idx) {
    if (expanded) return;
    expanded = true;
    // Pause grid animations
    timers.forEach(function(t) { clearTimeout(t); });
    timers = [];

    // Create full-screen overlay
    var backdrop = d3.select('body').append('div')
      .style('position', 'fixed').style('top', '0').style('left', '0')
      .style('width', '100vw').style('height', '100vh').style('z-index', '9999')
      .style('background', 'rgba(0,0,0,0.5)')
      .style('display', 'flex').style('align-items', 'center').style('justify-content', 'center')
      .style('cursor', 'pointer').style('opacity', '0')
      .style('transition', 'opacity 0.25s ease');

    var card = backdrop.append('div')
      .style('background', 'white').style('border-radius', '12px')
      .style('box-shadow', '0 20px 60px rgba(0,0,0,0.3)')
      .style('width', '90vw').style('max-width', '900px')
      .style('padding', '0').style('position', 'relative')
      .style('cursor', 'default');

    // Close button
    card.append('div')
      .style('position', 'absolute').style('top', '12px').style('right', '16px')
      .style('font-size', '20px').style('color', '#999').style('cursor', 'pointer')
      .style('line-height', '1').style('font-family', 'sans-serif')
      .text('\u00D7')
      .on('click', closeExpand);

    // Header
    var header = card.append('div').style('padding', '16px 20px 4px');
    header.append('div').style('font-size', '18px').style('font-weight', 'bold')
      .style('color', '#b91c1c').text(panelDefs[idx].label);
    header.append('div').style('font-size', '11px').style('color', '#aaa')
      .style('margin-top', '2px').text(panelDefs[idx].sub);

    // SVG container - use original cell dimensions as viewBox
    // so the browser scales everything up to fill the card
    var svgWrap = card.append('div').style('padding', '0 12px 16px');
    var expSvg = svgWrap.append('svg')
      .attr('viewBox', '0 0 ' + cellW + ' ' + cellH)
      .style('width', '100%').style('display', 'block');

    // Hint at bottom
    card.append('div').style('text-align', 'center').style('padding', '0 0 12px')
      .style('font-size', '10px').style('color', '#ccc').style('font-style', 'italic')
      .text('Click anywhere outside to close');

    // Draw at original cell size - viewBox scaling makes it big
    var origSvg = svg;
    svg = expSvg; timers = [];
    drawFns[idx]({ox: 0, oy: 0});
    svg = origSvg;
    // timers now holds the expanded panel's timeouts

    // Fade in
    setTimeout(function() { backdrop.style('opacity', '1'); }, 10);

    // Close on backdrop click
    backdrop.on('click', function(event) {
      if (event.target === backdrop.node()) closeExpand();
    });

    function closeExpand() {
      expanded = false;
      timers.forEach(function(t) { clearTimeout(t); });
      timers = [];
      backdrop.style('opacity', '0');
      setTimeout(function() { backdrop.remove(); }, 250);
      init(); // restart grid animations (also resets timers)
    }
  }

  function init() {
    svg.selectAll('*').remove();
    timers.forEach(function(t) { clearTimeout(t); });
    timers = [];

    // Section divider between designed (rows 0-3) and emergent (rows 4-5)
    var divY = gapY + 4 * (cellH + gapY) - gapY / 2;
    svg.append('line').attr('x1', gapX).attr('y1', divY).attr('x2', width - gapX).attr('y2', divY)
      .attr('stroke', '#e5e5e5').attr('stroke-width', 1).attr('stroke-dasharray', '6,4');
    svg.append('text').attr('x', gapX + 4).attr('y', divY - 4)
      .attr('font-size', '8px').attr('fill', '#bbb').attr('font-style', 'italic')
      .text('Designed patterns above / Emergent patterns below');

    panels.forEach(function(p, i) {
      svg.append('rect').attr('x', p.ox).attr('y', p.oy).attr('width', cellW).attr('height', cellH)
        .attr('rx', 8).attr('fill', i >= 8 ? '#fafaf8' : '#fafafa').attr('stroke', '#eee').attr('stroke-width', 1)
        .style('cursor', 'pointer')
        .on('click', function(event) { event.stopPropagation(); expandPanel(i); });
      svg.append('text').attr('x', p.ox + 12).attr('y', p.oy + 20)
        .attr('font-size', '12px').attr('font-weight', 'bold').attr('fill', i >= 8 ? '#92400e' : '#b91c1c').text(p.label)
        .style('pointer-events', 'none');
      svg.append('text').attr('x', p.ox + 12).attr('y', p.oy + 33)
        .attr('font-size', '8px').attr('fill', '#aaa').text(p.sub)
        .style('pointer-events', 'none');
    });

    drawFns.forEach(function(fn, i) { fn(panels[i]); });
  }

  // -- Helpers --
  function dn(g, x, y, r, color, label, labelY) {
    var sw = r <= 5 ? 0.6 : 1.2;
    g.append('circle').attr('cx', x).attr('cy', y).attr('r', 0)
      .attr('fill', color).attr('stroke', 'white').attr('stroke-width', sw).attr('opacity', 0.85)
      .transition().duration(300).attr('r', r);
    if (label) {
      g.append('text').attr('x', x).attr('y', labelY || (y + r + 11))
        .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#888').text(label);
    }
  }
  function dl(g, x1, y1, x2, y2, delay, dash) {
    g.append('line').attr('x1', x1).attr('y1', y1).attr('x2', x1).attr('y2', y1)
      .attr('stroke', '#ddd').attr('stroke-width', 1.2)
      .transition().delay(delay || 0).duration(350).attr('x2', x2).attr('y2', y2);
  }
  function dld(g, x1, y1, x2, y2, delay) {
    g.append('line').attr('x1', x1).attr('y1', y1).attr('x2', x1).attr('y2', y1)
      .attr('stroke', '#ccc').attr('stroke-width', 1).attr('stroke-dasharray', '4,3')
      .transition().delay(delay || 0).duration(350).attr('x2', x2).attr('y2', y2);
  }
  function pkt(g, pts, color, delay, r) {
    r = r || 2.5;
    for (var i = 0; i < pts.length - 1; i++) {
      (function(idx) {
        g.append('circle').attr('cx', pts[idx].x).attr('cy', pts[idx].y)
          .attr('r', r).attr('fill', color).attr('opacity', 0.85)
          .transition().delay(delay + idx * 250).duration(280)
          .attr('cx', pts[idx+1].x).attr('cy', pts[idx+1].y)
          .transition().duration(180).attr('opacity', 0).remove();
      })(i);
    }
    return delay + (pts.length - 1) * 250 + 460;
  }
  function anno(g, p, text) {
    g.append('text').attr('x', p.ox + cellW / 2).attr('y', p.oy + cellH - 10)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#ccc')
      .attr('font-style', 'italic').attr('opacity', 0).text(text)
      .transition().delay(600).duration(300).attr('opacity', 1)
      .transition().delay(4000).duration(500).attr('opacity', 0);
  }

  // ===== 1. CHAIN =====
  function drawChain(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 90;
    var names = ['Orchestrator', 'Planner', 'Coder', 'Tester', 'Reporter'];
    var cols = ['#7f1d1d', '#b91c1c', '#dc2626', '#ef4444', '#f87171'];
    var sizes = [11, 10, 9, 8, 7];
    var gap = 52, nodes = [];
    for (var i = 0; i < 5; i++) {
      var x = cx - 2 * gap + i * gap, y = by + (i % 2 === 0 ? 0 : 14);
      nodes.push({x: x, y: y});
      dn(g, x, y, sizes[i], cols[i], names[i]);
      if (i > 0) dl(g, nodes[i-1].x, nodes[i-1].y, x, y, i * 180);
    }
    var tick = 0;
    function anim() {
      tick++;
      var d1 = pkt(g, nodes, taskColor, 0, 3);
      pkt(g, nodes.slice().reverse(), resultColor, d1, 2.5);
      if (tick < 15) timers.push(setTimeout(anim, d1 + nodes.length * 250 + 300));
    }
    timers.push(setTimeout(anim, 1500));
    anno(g, p, 'Each agent hands off to the next in sequence');
  }

  // ===== 2. PIPELINE =====
  function drawPipeline(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var stages = ['Ingest', 'Parse', 'Enrich', 'Validate', 'Store'];
    var cols = ['#1e40af', '#2563eb', '#3b82f6', '#60a5fa', '#93c5fd'];
    var shapes = ['circle', 'rect', 'diamond', 'circle', 'rect'];
    var gap = 52, nodes = [];
    for (var i = 0; i < 5; i++) {
      var x = cx - 2 * gap + i * gap, y = by;
      nodes.push({x: x, y: y});
      if (shapes[i] === 'rect') {
        g.append('rect').attr('x', x-8).attr('y', y-8).attr('width', 16).attr('height', 16).attr('rx', 3)
          .attr('fill', cols[i]).attr('stroke', 'white').attr('stroke-width', 1.2).attr('opacity', 0.85);
      } else if (shapes[i] === 'diamond') {
        g.append('polygon').attr('points', x+','+(y-9)+' '+(x+9)+','+y+' '+x+','+(y+9)+' '+(x-9)+','+y)
          .attr('fill', cols[i]).attr('stroke', 'white').attr('stroke-width', 1.2).attr('opacity', 0.85);
      } else {
        dn(g, x, y, 8, cols[i], '');
      }
      g.append('text').attr('x', x).attr('y', y + 20).attr('text-anchor', 'middle')
        .attr('font-size', '7px').attr('fill', '#888').text(stages[i]);
      if (i > 0) {
        dl(g, nodes[i-1].x + 10, nodes[i-1].y, x - 10, y, i * 150);
        g.append('text').attr('x', (nodes[i-1].x + x) / 2).attr('y', y - 16)
          .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#bbb').text('transform');
      }
    }
    var tick = 0;
    function anim() {
      tick++;
      for (var i = 0; i < nodes.length - 1; i++) {
        (function(idx) {
          g.append('circle').attr('cx', nodes[idx].x).attr('cy', nodes[idx].y)
            .attr('r', 3.5).attr('fill', cols[idx]).attr('opacity', 0.9)
            .transition().delay(idx * 320).duration(300)
            .attr('cx', nodes[idx+1].x).attr('cy', nodes[idx+1].y).attr('fill', cols[idx+1])
            .transition().duration(150).attr('opacity', 0).remove();
        })(i);
      }
      if (tick < 18) timers.push(setTimeout(anim, nodes.length * 320 + 500));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Data changes shape at each stage');
  }

  // ===== 3. ROUTER =====
  function drawRouter(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 60;
    var router = {x: cx, y: ty + 20};
    var input = {x: cx - 120, y: ty + 20};
    var specialists = [
      {x: cx + 80, y: ty - 20, label: 'Code', color: '#2563eb'},
      {x: cx + 80, y: ty + 20, label: 'Search', color: '#059669'},
      {x: cx + 80, y: ty + 60, label: 'Math', color: '#d97706'},
      {x: cx + 80, y: ty + 100, label: 'Creative', color: '#7c3aed'}
    ];
    dn(g, input.x, input.y, 8, '#6b7280', 'Input');
    dn(g, router.x, router.y, 12, '#b91c1c', 'Router');
    dl(g, input.x, input.y, router.x - 12, router.y, 200);
    specialists.forEach(function(s, i) {
      dn(g, s.x, s.y, 8, s.color, s.label);
      dl(g, router.x + 12, router.y, s.x - 8, s.y, 400 + i * 100);
    });
    // Classification label
    var classLabel = g.append('text').attr('x', router.x).attr('y', router.y - 20)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#b91c1c').attr('font-weight', 'bold').text('');

    var tick = 0;
    function anim() {
      tick++;
      var target = Math.floor(Math.random() * specialists.length);
      var s = specialists[target];
      // Packet arrives at router
      pkt(g, [input, router], '#6b7280', 0, 3);
      // Classification appears
      timers.push(setTimeout(function() {
        classLabel.text(s.label + ' detected').attr('opacity', 0)
          .transition().duration(200).attr('opacity', 1)
          .transition().delay(600).duration(300).attr('opacity', 0);
        // Highlight chosen route
        g.append('line').attr('x1', router.x + 12).attr('y1', router.y)
          .attr('x2', s.x - 8).attr('y2', s.y)
          .attr('stroke', s.color).attr('stroke-width', 2.5).attr('opacity', 0.6)
          .transition().delay(400).duration(400).attr('opacity', 0).remove();
      }, 600));
      // Route to specialist
      pkt(g, [router, s], s.color, 800, 3);
      // Result back
      pkt(g, [s, router, input], resultColor, 1500, 2.5);
      if (tick < 14) timers.push(setTimeout(anim, 2800));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Classify input upfront, send to the right specialist');
  }

  // ===== 4. ESCALATION =====
  function drawEscalation(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 60;
    var models = [
      {x: cx - 100, y: ty + 70, r: 6, label: 'Tiny', color: '#fca5a5'},
      {x: cx - 35, y: ty + 45, r: 8, label: 'Small', color: '#ef4444'},
      {x: cx + 35, y: ty + 25, r: 10, label: 'Medium', color: '#dc2626'},
      {x: cx + 100, y: ty, r: 13, label: 'Frontier', color: '#7f1d1d'}
    ];
    models.forEach(function(m, i) {
      dn(g, m.x, m.y, m.r, m.color, m.label);
      if (i > 0) dld(g, models[i-1].x, models[i-1].y, m.x, m.y, i * 200);
    });
    for (var i = 0; i < models.length - 1; i++) {
      var mx = (models[i].x + models[i+1].x) / 2, my = (models[i].y + models[i+1].y) / 2;
      g.append('text').attr('x', mx).attr('y', my + 20)
        .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#ccc').text('escalate');
    }
    var meterY = ty + 120;
    g.append('text').attr('x', p.ox + 20).attr('y', meterY).attr('font-size', '7px').attr('fill', '#bbb').text('Confidence:');
    var meterBar = g.append('rect').attr('x', p.ox + 80).attr('y', meterY - 6).attr('width', 0).attr('height', 7)
      .attr('rx', 3).attr('fill', '#ef4444').attr('opacity', 0.6);

    var tick = 0;
    function anim() {
      tick++;
      var failAt = Math.floor(Math.random() * models.length);
      for (var i = 0; i <= failAt && i < models.length; i++) {
        (function(idx) {
          var isLast = idx === failAt || idx === models.length - 1;
          if (idx > 0) pkt(g, [models[idx-1], models[idx]], '#f59e0b', idx * 650, 3);
          timers.push(setTimeout(function() {
            var c = isLast ? 82 + Math.random() * 18 : Math.random() * 40 + 10;
            meterBar.transition().duration(300).attr('width', c * 1.2).attr('fill', c > 70 ? '#059669' : '#ef4444');
            if (isLast) {
              g.append('circle').attr('cx', models[idx].x).attr('cy', models[idx].y).attr('r', models[idx].r)
                .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
                .transition().duration(500).attr('r', models[idx].r + 10).attr('opacity', 0).remove();
            } else {
              g.append('text').attr('x', models[idx].x).attr('y', models[idx].y - models[idx].r - 5)
                .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#ef4444').text('?')
                .attr('opacity', 0.8).transition().delay(350).duration(300).attr('opacity', 0).remove();
            }
          }, idx * 650 + 450));
        })(i);
      }
      if (tick < 12) timers.push(setTimeout(anim, (failAt + 1) * 650 + 1300));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Small model tries first, escalates if unsure');
  }

  // ===== 5. TREE =====
  function drawTree(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var root = {x: cx, y: ty};
    var mids = [{x: cx-90, y: ty+60}, {x: cx-30, y: ty+60}, {x: cx+30, y: ty+60}, {x: cx+90, y: ty+60}];
    var leaves = [];
    mids.forEach(function(m, mi) {
      leaves.push({x: m.x-16, y: ty+120, pi: mi});
      leaves.push({x: m.x+16, y: ty+120, pi: mi});
    });
    dn(g, root.x, root.y, 12, '#7f1d1d', 'Root');
    mids.forEach(function(m, i) { dn(g, m.x, m.y, 8, '#b91c1c', 'Plan ' + (i+1)); dl(g, root.x, root.y, m.x, m.y, 300); });
    leaves.forEach(function(l) { dn(g, l.x, l.y, 4, '#ef4444', ''); dl(g, mids[l.pi].x, mids[l.pi].y, l.x, l.y, 600); });

    var tick = 0;
    function anim() {
      tick++;
      var li = Math.floor(Math.random() * leaves.length);
      var leaf = leaves[li];
      var path = [root, mids[leaf.pi], leaf];
      var d1 = pkt(g, path, taskColor, 0, 3);
      pkt(g, path.slice().reverse(), resultColor, d1, 2.5);
      if (tick < 18) timers.push(setTimeout(anim, 1400));
    }
    timers.push(setTimeout(anim, 1600));
    anno(g, p, 'One task fans out to many sub-tasks');
  }

  // ===== 6. MAP-REDUCE =====
  function drawMapReduce(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var disp = {x: cx - 120, y: ty + 65};
    var agg = {x: cx + 120, y: ty + 65};
    var wk = [];
    for (var i = 0; i < 7; i++) {
      var a = -Math.PI * 0.35 + Math.PI * 0.7 * i / 6;
      wk.push({x: cx + Math.cos(a) * 60, y: ty + 65 + Math.sin(a) * 70});
    }
    dn(g, disp.x, disp.y, 10, '#7f1d1d', 'Dispatch');
    dn(g, agg.x, agg.y, 10, '#065f46', 'Merge');
    wk.forEach(function(w, i) {
      dn(g, w.x, w.y, 6, '#ef4444', '');
      dl(g, disp.x, disp.y, w.x, w.y, 250 + i * 50);
      dl(g, w.x, w.y, agg.x, agg.y, 400 + i * 50);
    });
    // Feedback arc: from Merge, up and over, back to Dispatch with arrow
    var fbTop = p.oy + 18;
    var fbRight = agg.x + 30, fbLeft = disp.x - 30;
    g.append('path')
      .attr('d', 'M ' + agg.x + ' ' + (agg.y - 12) +
        ' C ' + fbRight + ' ' + fbTop + ' ' + fbLeft + ' ' + fbTop +
        ' ' + disp.x + ' ' + (disp.y - 12))
      .attr('fill', 'none').attr('stroke', '#bbb').attr('stroke-width', 1.2).attr('stroke-dasharray', '5,3')
      .attr('marker-end', 'url(#combo-arrow)')
      .attr('opacity', 0).transition().delay(800).duration(400).attr('opacity', 0.7);
    g.append('text').attr('x', cx).attr('y', fbTop + 10).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#aaa').attr('font-style', 'italic').attr('opacity', 0)
      .text('feedback').transition().delay(900).duration(300).attr('opacity', 1);

    var tick = 0;
    function anim() {
      tick++;
      wk.forEach(function(w, i) { pkt(g, [disp, w], taskColor, i * 60, 2.5); });
      var d = wk.length * 60 + 600;
      wk.forEach(function(w, i) { pkt(g, [w, agg], resultColor, d + i * 60, 2.5); });
      if (tick < 12) timers.push(setTimeout(anim, d + wk.length * 60 + 800));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Split work, execute in parallel, aggregate results');
  }

  // ===== 7. VOTING =====
  function drawVoting(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var source = {x: cx - 110, y: ty + 50};
    var judge = {x: cx + 110, y: ty + 50};
    var voters = [];
    for (var i = 0; i < 5; i++) {
      voters.push({x: cx, y: ty + 10 + i * 22});
    }
    dn(g, source.x, source.y, 10, '#6b7280', 'Task');
    dn(g, judge.x, judge.y, 10, '#7f1d1d', 'Judge');
    voters.forEach(function(v, i) {
      dn(g, v.x, v.y, 7, '#2563eb', 'Agent ' + (i+1));
      dl(g, source.x, source.y, v.x - 8, v.y, 200 + i * 60);
      dl(g, v.x + 8, v.y, judge.x, judge.y, 400 + i * 60);
    });

    var voteLabels = [];
    voters.forEach(function(v) {
      voteLabels.push(g.append('text').attr('x', v.x + 30).attr('y', v.y + 4)
        .attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', '#aaa').text(''));
    });
    var verdictLabel = g.append('text').attr('x', judge.x).attr('y', judge.y + 22)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', '#aaa').text('');

    var tick = 0;
    function anim() {
      tick++;
      // Send same task to all voters
      voters.forEach(function(v, i) { pkt(g, [source, v], taskColor, i * 50, 2.5); });
      var voteDelay = voters.length * 50 + 600;
      // Each voter votes A or B
      var votes = voters.map(function() { return Math.random() > 0.45 ? 'A' : 'B'; });
      var countA = votes.filter(function(v) { return v === 'A'; }).length;
      var winner = countA >= 3 ? 'A' : 'B';
      voters.forEach(function(v, i) {
        (function(idx) {
          timers.push(setTimeout(function() {
            voteLabels[idx].text(votes[idx])
              .attr('fill', votes[idx] === winner ? '#059669' : '#ef4444');
          }, voteDelay + idx * 120));
          pkt(g, [v, judge], votes[idx] === winner ? '#059669' : '#ef4444', voteDelay + idx * 120, 2.5);
        })(i);
      });
      timers.push(setTimeout(function() {
        verdictLabel.text('Majority: ' + winner + ' (' + (winner === 'A' ? countA : 5 - countA) + '/5)')
          .attr('fill', '#059669').attr('opacity', 0)
          .transition().duration(300).attr('opacity', 1)
          .transition().delay(1500).duration(400).attr('opacity', 0);
      }, voteDelay + voters.length * 120 + 300));
      if (tick < 10) timers.push(setTimeout(anim, voteDelay + voters.length * 120 + 2200));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Same task to N agents, majority decision wins');
  }

  // ===== 8. AUCTION =====
  function drawAuction(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 60;
    var auctioneer = {x: cx, y: ty};
    var bidders = [];
    for (var i = 0; i < 6; i++) {
      var a = Math.PI * 0.15 + Math.PI * 0.7 * i / 5;
      bidders.push({x: cx + Math.cos(a) * 80, y: ty + 15 + Math.sin(a) * 70});
    }
    dn(g, auctioneer.x, auctioneer.y, 11, '#7f1d1d', 'Auctioneer');
    var bidLabels = [];
    bidders.forEach(function(b, i) {
      dn(g, b.x, b.y, 7, '#6b7280', 'Agent ' + (i+1));
      dld(g, auctioneer.x, auctioneer.y, b.x, b.y, 200 + i * 80);
      bidLabels.push(g.append('text').attr('x', b.x).attr('y', b.y - 14)
        .attr('text-anchor', 'middle').attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', '#aaa').text(''));
    });

    var tick = 0;
    function anim() {
      tick++;
      bidders.forEach(function(b, i) { pkt(g, [auctioneer, b], '#f59e0b', i * 50, 2.5); });
      var bidDelay = bidders.length * 50 + 500;
      var bids = bidders.map(function() { return Math.floor(Math.random() * 90) + 10; });
      var winner = bids.indexOf(Math.max.apply(null, bids));
      bidders.forEach(function(b, i) {
        (function(idx, bid) {
          timers.push(setTimeout(function() {
            bidLabels[idx].text(bid).attr('fill', '#d97706')
              .transition().duration(200).attr('fill', idx === winner ? '#059669' : '#ccc');
          }, bidDelay + idx * 100));
          pkt(g, [b, auctioneer], idx === winner ? '#059669' : '#ddd', bidDelay + idx * 100, 2.5);
        })(i, bids[i]);
      });
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', bidders[winner].x).attr('cy', bidders[winner].y).attr('r', 7)
          .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2.5).attr('opacity', 0.8)
          .transition().duration(500).attr('r', 18).attr('opacity', 0).remove();
      }, bidDelay + bidders.length * 100 + 300));
      if (tick < 10) timers.push(setTimeout(anim, bidDelay + bidders.length * 100 + 1400));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Task broadcast, agents bid, highest score wins');
  }

  // ===== 9. ORCHESTRATOR =====
  function drawOrchestrator(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var orch = {x: cx, y: ty};
    var workerSlots = [
      {x: cx-100, y: ty+70}, {x: cx-50, y: ty+90}, {x: cx, y: ty+80},
      {x: cx+50, y: ty+90}, {x: cx+100, y: ty+70}
    ];
    dn(g, orch.x, orch.y, 13, '#7f1d1d', 'Orchestrator');
    var planLabel = g.append('text').attr('x', orch.x + 50).attr('y', orch.y - 5)
      .attr('font-size', '7px').attr('fill', '#b91c1c').attr('font-style', 'italic').text('');
    // Dynamic layer that gets cleared each tick
    var dynG = g.append('g');

    var tick = 0;
    function anim() {
      tick++;
      var numW = 2 + Math.floor(Math.random() * 3);
      var tasks = ['search', 'code', 'review', 'test', 'deploy'];
      var plan = numW + ' sub-tasks planned';
      planLabel.text(plan).attr('opacity', 0).transition().duration(200).attr('opacity', 0.8)
        .transition().delay(800).duration(300).attr('opacity', 0);

      timers.push(setTimeout(function() {
        dynG.selectAll('*').remove();
        for (var i = 0; i < numW; i++) {
          var slot = workerSlots[i];
          var taskName = tasks[Math.floor(Math.random() * tasks.length)];
          // Link
          dynG.append('line').attr('x1', orch.x).attr('y1', orch.y + 13)
            .attr('x2', orch.x).attr('y2', orch.y + 13)
            .attr('stroke', '#ddd').attr('stroke-width', 1.2)
            .transition().delay(i * 100).duration(350)
            .attr('x2', slot.x).attr('y2', slot.y - 7);
          // Worker node
          dynG.append('circle').attr('cx', slot.x).attr('cy', slot.y).attr('r', 0)
            .attr('fill', '#ef4444').attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.85)
            .transition().delay(i * 100).duration(300).attr('r', 7);
          dynG.append('text').attr('x', slot.x).attr('y', slot.y + 18)
            .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#888').text(taskName);
          // Task packet down
          pkt(dynG, [orch, slot], taskColor, 200 + i * 150, 2.5);
          // Result packet up
          pkt(dynG, [slot, orch], resultColor, 800 + i * 150, 2);
        }
      }, 600));

      if (tick < 10) timers.push(setTimeout(anim, 3000));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Orchestrator plans dynamically, spawns workers as needed');
  }

  // ===== 10. EVALUATOR =====
  function drawEvaluator(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 65;
    var generator = {x: cx - 80, y: ty + 40};
    var evaluator = {x: cx + 80, y: ty + 40};
    dn(g, generator.x, generator.y, 12, '#2563eb', 'Generator');
    dn(g, evaluator.x, evaluator.y, 12, '#d97706', 'Evaluator');
    // Forward arrow
    dl(g, generator.x + 12, generator.y - 8, evaluator.x - 12, evaluator.y - 8, 300);
    // Feedback arrow (dashed, below)
    dld(g, evaluator.x - 12, evaluator.y + 8, generator.x + 12, generator.y + 8, 500);
    g.append('text').attr('x', cx).attr('y', ty + 22).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').text('output');
    g.append('text').attr('x', cx).attr('y', ty + 66).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').attr('font-style', 'italic').text('feedback');

    // Quality score
    var scoreLabel = g.append('text').attr('x', cx).attr('y', ty + 115)
      .attr('text-anchor', 'middle').attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', '#aaa').text('');
    var iterLabel = g.append('text').attr('x', cx).attr('y', ty + 130)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#ccc').text('');

    var tick = 0;
    function anim() {
      tick++;
      var iterations = 2 + Math.floor(Math.random() * 3);
      var score = 30 + Math.floor(Math.random() * 20);
      for (var i = 0; i < iterations; i++) {
        (function(iter, s) {
          var baseDelay = iter * 1200;
          // Generate
          pkt(g, [generator, evaluator], '#2563eb', baseDelay, 3);
          // Evaluate
          timers.push(setTimeout(function() {
            s = Math.min(98, s + 15 + Math.floor(Math.random() * 10));
            scoreLabel.text('Quality: ' + s + '%').attr('fill', s > 80 ? '#059669' : '#d97706');
            iterLabel.text('Iteration ' + (iter + 1) + '/' + iterations);
          }, baseDelay + 500));
          // Feedback (if not last)
          if (iter < iterations - 1) {
            pkt(g, [evaluator, generator], '#d97706', baseDelay + 700, 2.5);
          } else {
            // Final: green success
            timers.push(setTimeout(function() {
              g.append('circle').attr('cx', evaluator.x).attr('cy', evaluator.y).attr('r', 12)
                .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
                .transition().duration(500).attr('r', 22).attr('opacity', 0).remove();
            }, baseDelay + 800));
          }
        })(i, score);
        score = Math.min(98, score + 15 + Math.floor(Math.random() * 10));
      }
      if (tick < 8) timers.push(setTimeout(anim, iterations * 1200 + 1500));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Generate, evaluate, refine until quality threshold met');
  }

  // ===== 11. SWARM =====
  function drawSwarm(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + cellH / 2 + 10;
    var agents = [];
    for (var i = 0; i < 18; i++) {
      var a = Math.random() * Math.PI * 2, r = 20 + Math.random() * 65;
      agents.push({x: cx + Math.cos(a) * r, y: cy + Math.sin(a) * r, vx: 0, vy: 0, el: null});
    }
    var task = {x: cx + 60, y: cy - 25, el: null, label: null};
    task.el = g.append('circle').attr('cx', task.x).attr('cy', task.y).attr('r', 6)
      .attr('fill', '#f59e0b').attr('opacity', 0.9);
    task.label = g.append('text').attr('x', task.x).attr('y', task.y + 16)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#d97706').text('Task');
    agents.forEach(function(ag) {
      ag.el = g.append('circle').attr('cx', ag.x).attr('cy', ag.y).attr('r', 4)
        .attr('fill', '#6b7280').attr('stroke', 'white').attr('stroke-width', 0.5).attr('opacity', 0.7);
    });

    var tick = 0;
    function anim() {
      tick++;
      agents.forEach(function(ag) {
        var dx = task.x - ag.x, dy = task.y - ag.y, dist = Math.sqrt(dx*dx + dy*dy) + 1;
        ag.vx += dx / dist * 0.15 + (Math.random() - 0.5) * 0.4;
        ag.vy += dy / dist * 0.15 + (Math.random() - 0.5) * 0.4;
        ag.vx *= 0.91; ag.vy *= 0.91;
        ag.x += ag.vx; ag.y += ag.vy;
        ag.x = Math.max(p.ox + 10, Math.min(p.ox + cellW - 10, ag.x));
        ag.y = Math.max(p.oy + 40, Math.min(p.oy + cellH - 20, ag.y));
        ag.el.attr('cx', ag.x).attr('cy', ag.y);
        ag.el.attr('fill', dist < 30 ? '#059669' : '#6b7280');
      });
      if (tick % 50 === 0) {
        task.x = p.ox + 50 + Math.random() * (cellW - 100);
        task.y = p.oy + 60 + Math.random() * (cellH - 100);
        task.el.transition().duration(500).attr('cx', task.x).attr('cy', task.y);
        task.label.transition().duration(500).attr('x', task.x).attr('y', task.y + 16);
      }
      if (tick < 350) timers.push(setTimeout(anim, 45));
    }
    timers.push(setTimeout(anim, 1000));
    anno(g, p, 'No hierarchy - agents self-organize around tasks');
  }

  // ===== 12. GOSSIP =====
  function drawGossip(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + cellH / 2 + 5;
    var numPeers = 12;
    var peers = [];
    for (var i = 0; i < numPeers; i++) {
      var a = (i / numPeers) * Math.PI * 2;
      var r = 60 + (i % 2) * 18;
      peers.push({x: cx + Math.cos(a) * r, y: cy + Math.sin(a) * r, knows: false, el: null, neighbors: []});
    }
    peers.forEach(function(pe, i) {
      pe.el = g.append('circle').attr('cx', pe.x).attr('cy', pe.y).attr('r', 6)
        .attr('fill', '#d1d5db').attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.8);
      pe.neighbors = [((i-1)+numPeers)%numPeers, (i+1)%numPeers];
      if (i % 2 === 0) pe.neighbors.push((i+3)%numPeers);
      if (i % 3 === 0) pe.neighbors.push((i+4)%numPeers);
      pe.neighbors.forEach(function(ni) {
        if (ni > i) dl(g, pe.x, pe.y, peers[ni].x, peers[ni].y, 150);
      });
    });

    var tick = 0;
    function anim() {
      tick++;
      if (tick === 1 || peers.filter(function(pe){return pe.knows;}).length === 0) {
        var starter = Math.floor(Math.random() * numPeers);
        peers[starter].knows = true;
        peers[starter].el.transition().duration(300).attr('fill', '#f59e0b');
      }
      var newKnowers = [];
      peers.forEach(function(pe) {
        if (!pe.knows) return;
        var target = pe.neighbors[Math.floor(Math.random() * pe.neighbors.length)];
        pkt(g, [pe, peers[target]], '#f59e0b', 0, 2);
        if (!peers[target].knows) newKnowers.push(peers[target]);
      });
      timers.push(setTimeout(function() {
        newKnowers.forEach(function(nk) { nk.knows = true; nk.el.transition().duration(300).attr('fill', '#f59e0b'); });
      }, 350));
      var allKnow = peers.filter(function(pe){return pe.knows;}).length >= numPeers;
      if (allKnow) {
        timers.push(setTimeout(function() {
          peers.forEach(function(pe) { pe.knows = false; pe.el.transition().duration(300).attr('fill', '#d1d5db'); });
        }, 600));
      }
      if (tick < 70) timers.push(setTimeout(anim, 700));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Information spreads peer-to-peer like a rumor');
  }

  drawFns = [drawChain, drawPipeline, drawRouter, drawEscalation, drawTree, drawMapReduce,
    drawVoting, drawAuction, drawOrchestrator, drawEvaluator, drawSwarm, drawGossip];
  init();
})();

// ============================================================
// Visualization: Combined Delegation Patterns (3 combos)
// ============================================================
(function() {
  var container = document.getElementById('viz-combo');
  if (!container) return;
  var width = 720, height = 420;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');
  var timers = [];

  var cellW = 226, gap = 10;
  var panels = [
    {label: 'Quality Gate', ox: gap, oy: 0},
    {label: 'Consensus Engine', ox: cellW + gap * 2, oy: 0},
    {label: 'Adaptive Pipeline', ox: (cellW + gap) * 2 + gap, oy: 0}
  ];

  function init() {
    svg.selectAll('*').remove();
    timers.forEach(function(t) { clearTimeout(t); });
    timers = [];

    panels.forEach(function(p) {
      svg.append('rect').attr('x', p.ox).attr('y', p.oy).attr('width', cellW).attr('height', height)
        .attr('rx', 6).attr('fill', '#fafafa').attr('stroke', '#ccc').attr('stroke-width', 1.2);
      svg.append('text').attr('x', p.ox + 10).attr('y', p.oy + 18)
        .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#b91c1c').text(p.label);
    });

    drawQualityGate(panels[0]);
    drawConsensus(panels[1]);
    drawBiddingPipeline(panels[2]);
  }

  function pkt(g, pts, color, delay, r) {
    r = r || 2.5;
    for (var i = 0; i < pts.length - 1; i++) {
      (function(idx) {
        g.append('circle').attr('cx', pts[idx].x).attr('cy', pts[idx].y)
          .attr('r', r).attr('fill', color).attr('opacity', 0.85)
          .transition().delay(delay + idx * 220).duration(250)
          .attr('cx', pts[idx+1].x).attr('cy', pts[idx+1].y)
          .transition().duration(150).attr('opacity', 0).remove();
      })(i);
    }
    return delay + (pts.length - 1) * 220 + 400;
  }

  // ---- QUALITY GATE: Router -> Escalation -> Evaluator ----
  function drawQualityGate(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    // Input -> Router -> Specialist (cheap) -> Escalation -> Evaluator -> Output
    var input = {x: p.ox + 25, y: ty + 75};
    var router = {x: cx - 45, y: ty + 75};
    var cheap = {x: cx + 10, y: ty + 25};
    var big = {x: cx + 10, y: ty + 130};
    var evaluator = {x: cx + 70, y: ty + 75};
    var output = {x: p.ox + cellW - 25, y: ty + 75};

    // Links (drawn first so nodes sit on top)
    [[input, router], [router, cheap], [router, big], [cheap, evaluator], [big, evaluator], [evaluator, output]].forEach(function(l) {
      g.append('line').attr('x1', l[0].x).attr('y1', l[0].y).attr('x2', l[1].x).attr('y2', l[1].y)
        .attr('stroke', '#aaa').attr('stroke-width', 1.5);
    });
    // Dashed escalation arrow
    g.append('line').attr('x1', cheap.x).attr('y1', cheap.y + 9).attr('x2', big.x).attr('y2', big.y - 12)
      .attr('stroke', '#ef4444').attr('stroke-width', 1).attr('stroke-dasharray', '4,3');
    g.append('text').attr('x', cheap.x + 16).attr('y', (cheap.y + big.y) / 2 + 3)
      .attr('font-size', '7px').attr('fill', '#bbb').text('escalate');
    // Eval feedback loop
    g.append('path').attr('d', 'M ' + (evaluator.x + 7) + ' ' + (evaluator.y - 10) +
      ' A 14 14 0 1 1 ' + (evaluator.x + 7) + ' ' + (evaluator.y + 10))
      .attr('fill', 'none').attr('stroke', '#d97706').attr('stroke-width', 1).attr('stroke-dasharray', '3,2');
    // Nodes (on top of links)
    [['Input', input, 7, '#6b7280'], ['Router', router, 12, '#2563eb'],
     ['Small', cheap, 9, '#ef4444'], ['Frontier', big, 12, '#7f1d1d'],
     ['Eval', evaluator, 10, '#d97706'], ['Out', output, 7, '#059669']].forEach(function(d) {
      g.append('circle').attr('cx', d[1].x).attr('cy', d[1].y).attr('r', d[2])
        .attr('fill', d[3]).attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.85);
      g.append('text').attr('x', d[1].x).attr('y', d[1].y + d[2] + 14)
        .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#888').text(d[0]);
    });

    var tick = 0;
    function anim() {
      tick++;
      var useBig = Math.random() < 0.3; // 30% escalation
      var path1 = useBig ? [input, router, big, evaluator] : [input, router, cheap, evaluator];
      var d1 = pkt(g, path1, useBig ? '#7f1d1d' : '#ef4444', 0, 3.5);
      // Eval loop (sometimes 2 iterations)
      var loops = Math.random() < 0.4 ? 2 : 1;
      var loopDelay = d1;
      for (var i = 0; i < loops; i++) {
        (function(ld) {
          g.append('circle').attr('cx', evaluator.x).attr('cy', evaluator.y)
            .attr('r', 10).attr('fill', 'none').attr('stroke', '#d97706').attr('stroke-width', 2).attr('opacity', 0)
            .transition().delay(ld).duration(200).attr('opacity', 0.6)
            .transition().duration(200).attr('r', 16).attr('opacity', 0).remove();
        })(loopDelay);
        loopDelay += 500;
      }
      pkt(g, [evaluator, output], '#059669', loopDelay, 3.5);
      if (tick < 12) timers.push(setTimeout(anim, loopDelay + 800));
    }
    timers.push(setTimeout(anim, 1200));
    // Labels for component patterns
    g.append('text').attr('x', p.ox + cellW / 2).attr('y', height - 14)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#bbb')
      .attr('font-style', 'italic').text('Router + Escalation + Evaluator');
  }

  // ---- CONSENSUS ENGINE: Map-Reduce + Voting ----
  function drawConsensus(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 50;
    var disp = {x: p.ox + 30, y: ty + 85};
    var merge = {x: p.ox + cellW - 30, y: ty + 85};
    // 3 sub-problems, each with 2 voters
    var subProblems = [];
    for (var s = 0; s < 3; s++) {
      var sy = ty + 20 + s * 60;
      var voters = [];
      for (var v = 0; v < 2; v++) {
        voters.push({x: cx - 15 + v * 35, y: sy});
      }
      subProblems.push({y: sy, voters: voters, verdict: {x: cx + 65, y: sy}});
    }
    // Links first (behind nodes)
    subProblems.forEach(function(sp) {
      sp.voters.forEach(function(v) {
        g.append('line').attr('x1', disp.x).attr('y1', disp.y).attr('x2', v.x).attr('y2', v.y)
          .attr('stroke', '#aaa').attr('stroke-width', 1.3);
        g.append('line').attr('x1', v.x).attr('y1', v.y).attr('x2', sp.verdict.x).attr('y2', sp.verdict.y)
          .attr('stroke', '#aaa').attr('stroke-width', 1.3);
      });
      g.append('line').attr('x1', sp.verdict.x).attr('y1', sp.verdict.y).attr('x2', merge.x).attr('y2', merge.y)
        .attr('stroke', '#aaa').attr('stroke-width', 1.3);
    });
    // Nodes on top
    g.append('circle').attr('cx', disp.x).attr('cy', disp.y).attr('r', 12)
      .attr('fill', '#7f1d1d').attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.85);
    g.append('text').attr('x', disp.x).attr('y', disp.y + 22).attr('text-anchor', 'middle')
      .attr('font-size', '8px').attr('fill', '#888').text('Split');
    g.append('circle').attr('cx', merge.x).attr('cy', merge.y).attr('r', 12)
      .attr('fill', '#065f46').attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.85);
    g.append('text').attr('x', merge.x).attr('y', merge.y + 22).attr('text-anchor', 'middle')
      .attr('font-size', '8px').attr('fill', '#888').text('Merge');
    subProblems.forEach(function(sp) {
      sp.voters.forEach(function(v) {
        g.append('circle').attr('cx', v.x).attr('cy', v.y).attr('r', 6)
          .attr('fill', '#2563eb').attr('stroke', 'white').attr('stroke-width', 0.7).attr('opacity', 0.8);
      });
      g.append('circle').attr('cx', sp.verdict.x).attr('cy', sp.verdict.y).attr('r', 6)
        .attr('fill', '#d97706').attr('stroke', 'white').attr('stroke-width', 0.7);
    });

    var voteLabels = [];
    subProblems.forEach(function(sp) {
      var lbl = g.append('text').attr('x', sp.verdict.x).attr('y', sp.verdict.y - 10)
        .attr('text-anchor', 'middle').attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', '#aaa').text('');
      voteLabels.push(lbl);
    });

    var tick = 0;
    function anim() {
      tick++;
      // Dispatch to all voters
      subProblems.forEach(function(sp, si) {
        sp.voters.forEach(function(v, vi) {
          pkt(g, [disp, v], '#dc2626', si * 80 + vi * 40, 3);
        });
      });
      var voteDelay = 3 * 80 + 2 * 40 + 500;
      // Voters send to verdict
      subProblems.forEach(function(sp, si) {
        var votes = sp.voters.map(function() { return Math.random() > 0.3 ? 'A' : 'B'; });
        var countA = votes.filter(function(v) { return v === 'A'; }).length;
        var winner = countA >= 1 ? 'A' : 'B';
        sp.voters.forEach(function(v, vi) {
          pkt(g, [v, sp.verdict], votes[vi] === winner ? '#059669' : '#ef4444', voteDelay + si * 100 + vi * 50, 3);
        });
        (function(idx, w) {
          timers.push(setTimeout(function() {
            voteLabels[idx].text(w).attr('fill', '#059669');
          }, voteDelay + si * 100 + 2 * 50 + 200));
        })(si, winner);
      });
      // Verdicts to merge
      var mergeDelay = voteDelay + 3 * 100 + 2 * 50 + 500;
      subProblems.forEach(function(sp, si) {
        pkt(g, [sp.verdict, merge], '#059669', mergeDelay + si * 80, 3.5);
      });
      if (tick < 8) timers.push(setTimeout(anim, mergeDelay + 3 * 80 + 800));
    }
    timers.push(setTimeout(anim, 1200));
    g.append('text').attr('x', p.ox + cellW / 2).attr('y', height - 14)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#bbb')
      .attr('font-style', 'italic').text('Map-Reduce + Voting');
  }

  // ---- BIDDING PIPELINE: Pipeline + Auction ----
  function drawBiddingPipeline(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    // 4 pipeline stages
    var stageNames = ['Ingest', 'Parse', 'Enrich', 'Store'];
    var stageX = [];
    var stageGap = 48;
    for (var i = 0; i < 4; i++) stageX.push(cx - 1.5 * stageGap + i * stageGap);
    var stageY = ty + 40;

    // Compute bidder positions first
    var bidders = [];
    var bidColors = ['#2563eb', '#7c3aed', '#059669', '#d97706', '#dc2626'];
    for (var i = 0; i < 5; i++) {
      bidders.push({x: cx - 70 + i * 35, y: ty + 190});
    }

    // Dashed lines from bidders to stages (drawn first, behind everything)
    bidders.forEach(function(b) {
      stageX.forEach(function(sx) {
        g.append('line').attr('x1', b.x).attr('y1', b.y - 8).attr('x2', sx).attr('y2', stageY + 14)
          .attr('stroke', '#bbb').attr('stroke-width', 0.9).attr('stroke-dasharray', '3,3');
      });
    });

    // Stage connectors (behind boxes)
    for (var i = 1; i < 4; i++) {
      g.append('line').attr('x1', stageX[i-1] + 22).attr('y1', stageY)
        .attr('x2', stageX[i] - 22).attr('y2', stageY)
        .attr('stroke', '#999').attr('stroke-width', 1.5);
    }

    // Stage boxes (on top of lines)
    var stageEls = [];
    stageNames.forEach(function(name, i) {
      var sx = stageX[i];
      g.append('rect').attr('x', sx - 22).attr('y', stageY - 14).attr('width', 44).attr('height', 28)
        .attr('rx', 4).attr('fill', '#f5f5f5').attr('stroke', '#aaa').attr('stroke-width', 1.2);
      g.append('text').attr('x', sx).attr('y', stageY + 4).attr('text-anchor', 'middle')
        .attr('font-size', '8px').attr('fill', '#777').text(name);
      stageEls.push(g.append('circle').attr('cx', sx).attr('cy', stageY + 30).attr('r', 0)
        .attr('fill', '#ccc'));
    });

    // Bidder nodes (on top of dashed lines)
    for (var i = 0; i < 5; i++) {
      g.append('circle').attr('cx', bidders[i].x).attr('cy', bidders[i].y).attr('r', 8)
        .attr('fill', bidColors[i]).attr('stroke', 'white').attr('stroke-width', 0.8).attr('opacity', 0.8);
      g.append('text').attr('x', bidders[i].x).attr('y', bidders[i].y + 18)
        .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#888').text('Agent ' + (i+1));
    }

    var tick = 0;
    function anim() {
      tick++;
      // For each stage, run a mini-auction: bidders bid, winner fills the slot
      stageNames.forEach(function(name, si) {
        var delay = si * 600;
        // Bids fly up
        var bids = bidders.map(function() { return Math.floor(Math.random() * 90) + 10; });
        var winner = bids.indexOf(Math.max.apply(null, bids));
        bidders.forEach(function(b, bi) {
          pkt(g, [b, {x: stageX[si], y: stageY + 14}], bidColors[bi], delay + bi * 40, 2.5);
        });
        // Winner highlighted
        (function(sidx, w) {
          timers.push(setTimeout(function() {
            stageEls[sidx].transition().duration(200).attr('r', 6).attr('fill', bidColors[w]);
            // Flash the winning bidder
            g.append('circle').attr('cx', bidders[w].x).attr('cy', bidders[w].y).attr('r', 8)
              .attr('fill', 'none').attr('stroke', bidColors[w]).attr('stroke-width', 2).attr('opacity', 0.8)
              .transition().duration(400).attr('r', 14).attr('opacity', 0).remove();
          }, delay + 5 * 40 + 300));
        })(si, winner);
      });
      // After all stages filled, data flows through
      var flowDelay = 4 * 600 + 600;
      var pts = stageX.map(function(sx) { return {x: sx, y: stageY}; });
      pkt(g, pts, '#059669', flowDelay, 3.5);
      // Reset winners for next round
      timers.push(setTimeout(function() {
        stageEls.forEach(function(el) { el.transition().duration(300).attr('r', 0); });
      }, flowDelay + pts.length * 220 + 400));

      if (tick < 8) timers.push(setTimeout(anim, flowDelay + pts.length * 220 + 800));
    }
    timers.push(setTimeout(anim, 1200));
    g.append('text').attr('x', p.ox + cellW / 2).attr('y', height - 14)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#bbb')
      .attr('font-style', 'italic').text('Pipeline + Auction');
  }

  init();
  svg.on('click', init);
})();

// ============================================================
// Visualization 2: Review Loop (multiple generators + critics)
// ============================================================
(function() {
  var container = document.getElementById('viz-review');
  if (!container) return;
  var width = 720, height = 380;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height);

  var defs = svg.append('defs');
  defs.append('marker').attr('id', 'rv-arrow').attr('viewBox', '0 0 10 10')
    .attr('refX', 8).attr('refY', 5).attr('markerWidth', 7).attr('markerHeight', 7).attr('orient', 'auto')
    .append('path').attr('d', 'M 0 0 L 10 5 L 0 10 Z').attr('fill', '#999');

  // Three generators on the left
  var generators = [
    { x: 100, y: 100 }, { x: 100, y: 190 }, { x: 100, y: 280 }
  ];
  // Three critics in the middle
  var critics = [
    { x: 360, y: 120 }, { x: 360, y: 210 }, { x: 360, y: 300 }
  ];
  // Two meta-critics on the right
  var metaCritics = [
    { x: 600, y: 150 }, { x: 600, y: 250 }
  ];

  // Column labels
  svg.append('text').attr('x', 100).attr('y', 40).attr('text-anchor', 'middle')
    .attr('font-size', '12px').attr('font-weight', 'bold').attr('fill', '#b91c1c').text('Generators');
  svg.append('text').attr('x', 360).attr('y', 40).attr('text-anchor', 'middle')
    .attr('font-size', '12px').attr('font-weight', 'bold').attr('fill', '#d97706').text('Critics');
  svg.append('text').attr('x', 600).attr('y', 40).attr('text-anchor', 'middle')
    .attr('font-size', '12px').attr('font-weight', 'bold').attr('fill', '#7c3aed').text('Meta-Critics');

  // Arrows: each generator connects to each critic
  generators.forEach(function(g) {
    critics.forEach(function(c) {
      svg.append('line').attr('x1', g.x + 28).attr('y1', g.y).attr('x2', c.x - 28).attr('y2', c.y)
        .attr('stroke', '#ddd').attr('stroke-width', 1).attr('marker-end', 'url(#rv-arrow)');
    });
  });
  critics.forEach(function(c) {
    metaCritics.forEach(function(m) {
      svg.append('line').attr('x1', c.x + 28).attr('y1', c.y).attr('x2', m.x - 28).attr('y2', m.y)
        .attr('stroke', '#ddd').attr('stroke-width', 1).attr('marker-end', 'url(#rv-arrow)');
    });
  });

  // Feedback arc (dashed, from meta-critics back to generators)
  svg.append('path')
    .attr('d', 'M 600 280 Q 360 360 100 310')
    .attr('fill', 'none').attr('stroke', '#999').attr('stroke-width', 1.5)
    .attr('stroke-dasharray', '6,4').attr('marker-end', 'url(#rv-arrow)');
  svg.append('text').attr('x', 360).attr('y', 355).attr('text-anchor', 'middle')
    .attr('font-size', '10px').attr('fill', '#888').attr('font-style', 'italic').text('feedback');

  // Draw generator nodes
  generators.forEach(function(g, i) {
    svg.append('circle').attr('cx', g.x).attr('cy', g.y).attr('r', 24)
      .attr('fill', '#b91c1c').attr('opacity', 0.12).attr('stroke', '#b91c1c').attr('stroke-width', 2);
    svg.append('text').attr('x', g.x).attr('y', g.y + 4).attr('text-anchor', 'middle')
      .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#b91c1c').text('G' + (i + 1));
  });
  critics.forEach(function(c, i) {
    svg.append('circle').attr('cx', c.x).attr('cy', c.y).attr('r', 24)
      .attr('fill', '#d97706').attr('opacity', 0.12).attr('stroke', '#d97706').attr('stroke-width', 2);
    svg.append('text').attr('x', c.x).attr('y', c.y + 4).attr('text-anchor', 'middle')
      .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#d97706').text('C' + (i + 1));
  });
  metaCritics.forEach(function(m, i) {
    svg.append('circle').attr('cx', m.x).attr('cy', m.y).attr('r', 24)
      .attr('fill', '#7c3aed').attr('opacity', 0.12).attr('stroke', '#7c3aed').attr('stroke-width', 2);
    svg.append('text').attr('x', m.x).attr('y', m.y + 4).attr('text-anchor', 'middle')
      .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#7c3aed').text('M' + (i + 1));
  });

  // Error rate bars below each column
  var barY = 60;
  [{x:100,w:40,c:'#b91c1c',l:'high error'},{x:360,w:16,c:'#d97706',l:'reduced'},{x:600,w:5,c:'#7c3aed',l:'minimal'}].forEach(function(b) {
    svg.append('rect').attr('x', b.x - b.w/2).attr('y', barY).attr('width', b.w).attr('height', 6).attr('rx', 2).attr('fill', b.c).attr('opacity', 0.5);
    svg.append('text').attr('x', b.x).attr('y', barY + 16).attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#888').text(b.l);
  });
})();

// ============================================================
// Visualization 3: Mixture of Specialists
// ============================================================
(function() {
  var container = document.getElementById('viz-specialists');
  if (!container) return;
  var width = 720, height = 420;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  var bgZoneLayer = svg.append('g');
  var linkLayer = svg.append('g');
  var pktLayer = svg.append('g');
  var nodeLayer = svg.append('g');

  // Left: Routers (user-facing entry points)
  var routers = [
    { id: 'Assistant', x: 80, y: 90, color: '#2563eb', r: 20 },
    { id: 'Copilot', x: 80, y: 180, color: '#7c3aed', r: 18 },
    { id: 'Search', x: 80, y: 270, color: '#0891b2', r: 16 },
    { id: 'Pipeline', x: 80, y: 350, color: '#374151', r: 15 }
  ];

  // Right: Marketplace (agents, tools, knowledge bases)
  var market = [
    { id: 'Code LLM', x: 340, y: 55, color: '#b91c1c', r: 22, type: 'agent' },
    { id: 'Reasoning', x: 510, y: 55, color: '#7c3aed', r: 24, type: 'agent' },
    { id: 'Vision', x: 650, y: 100, color: '#ea580c', r: 18, type: 'agent' },
    { id: 'Retriever', x: 300, y: 155, color: '#d97706', r: 20, type: 'tool' },
    { id: 'Web Search', x: 470, y: 150, color: '#0891b2', r: 17, type: 'tool' },
    { id: 'Calculator', x: 630, y: 195, color: '#059669', r: 14, type: 'tool' },
    { id: 'Docs KB', x: 320, y: 255, color: '#6366f1', r: 19, type: 'kb' },
    { id: 'Code KB', x: 500, y: 250, color: '#dc2626', r: 16, type: 'kb' },
    { id: 'Safety', x: 650, y: 290, color: '#065f46', r: 15, type: 'agent' },
    { id: 'Translator', x: 340, y: 345, color: '#db2777', r: 17, type: 'agent' },
    { id: 'Summarizer', x: 510, y: 340, color: '#92400e', r: 16, type: 'agent' },
    { id: 'Memory', x: 650, y: 370, color: '#4f46e5', r: 18, type: 'kb' }
  ];

  // Connections: which routers use which market items (with weight = traffic)
  var edges = [
    { src: 'Assistant', tgt: 'Reasoning', w: 5 },
    { src: 'Assistant', tgt: 'Retriever', w: 4 },
    { src: 'Assistant', tgt: 'Docs KB', w: 3 },
    { src: 'Assistant', tgt: 'Safety', w: 2 },
    { src: 'Assistant', tgt: 'Memory', w: 3 },
    { src: 'Copilot', tgt: 'Code LLM', w: 5 },
    { src: 'Copilot', tgt: 'Code KB', w: 4 },
    { src: 'Copilot', tgt: 'Reasoning', w: 2 },
    { src: 'Copilot', tgt: 'Calculator', w: 1 },
    { src: 'Search', tgt: 'Web Search', w: 5 },
    { src: 'Search', tgt: 'Retriever', w: 4 },
    { src: 'Search', tgt: 'Summarizer', w: 3 },
    { src: 'Search', tgt: 'Translator', w: 2 },
    { src: 'Pipeline', tgt: 'Code LLM', w: 3 },
    { src: 'Pipeline', tgt: 'Vision', w: 2 },
    { src: 'Pipeline', tgt: 'Docs KB', w: 2 },
    { src: 'Pipeline', tgt: 'Summarizer', w: 2 }
  ];

  // Denied connections: access control prevents these
  var denied = [
    { src: 'Search', tgt: 'Code LLM' },
    { src: 'Search', tgt: 'Code KB' },
    { src: 'Pipeline', tgt: 'Memory' },
    { src: 'Assistant', tgt: 'Code KB' }
  ];

  var allNodes = routers.concat(market);
  var nodeMap = {};
  allNodes.forEach(function(n) { nodeMap[n.id] = n; });

  // Zone backgrounds (on bgZoneLayer so they sit behind links and nodes)
  bgZoneLayer.append('rect').attr('x', 20).attr('y', 30).attr('width', 120).attr('height', 370).attr('rx', 10)
    .attr('fill', '#2563eb').attr('opacity', 0.04);
  bgZoneLayer.append('text').attr('x', 80).attr('y', 22).attr('text-anchor', 'middle')
    .attr('font-size', '11px').attr('font-weight', 'bold').attr('fill', '#4070b0').attr('opacity', 0.5).text('Routers');
  bgZoneLayer.append('rect').attr('x', 220).attr('y', 30).attr('width', 480).attr('height', 370).attr('rx', 10)
    .attr('fill', '#d97706').attr('opacity', 0.03);
  bgZoneLayer.append('text').attr('x', 460).attr('y', 22).attr('text-anchor', 'middle')
    .attr('font-size', '13px').attr('font-weight', 'bold').attr('fill', '#b08030').attr('opacity', 0.5).text('Specialist Marketplace');
  bgZoneLayer.append('text').attr('x', 460).attr('y', 36).attr('text-anchor', 'middle')
    .attr('font-size', '8px').attr('fill', '#aaa').text('access-controlled');

  // Type legend
  var legY = height - 8;
  [['Agent', '#b91c1c', 'circle'], ['Tool', '#059669', 'rect'], ['Knowledge Base', '#6366f1', 'diamond']].forEach(function(l, i) {
    var lx = 250 + i * 160;
    if (l[2] === 'circle') nodeLayer.append('circle').attr('cx', lx).attr('cy', legY - 3).attr('r', 4).attr('fill', l[1]).attr('opacity', 0.6);
    else if (l[2] === 'rect') nodeLayer.append('rect').attr('x', lx - 4).attr('y', legY - 7).attr('width', 8).attr('height', 8).attr('rx', 1.5).attr('fill', l[1]).attr('opacity', 0.6);
    else nodeLayer.append('polygon').attr('points', (lx) + ',' + (legY - 8) + ' ' + (lx + 5) + ',' + (legY - 3) + ' ' + (lx) + ',' + (legY + 2) + ' ' + (lx - 5) + ',' + (legY - 3))
      .attr('fill', l[1]).attr('opacity', 0.6);
    nodeLayer.append('text').attr('x', lx + 8).attr('y', legY).attr('font-size', '8px').attr('fill', '#888').text(l[0]);
  });

  // Draw edges (behind nodes)
  edges.forEach(function(e) {
    var s = nodeMap[e.src], t = nodeMap[e.tgt];
    linkLayer.append('line').attr('x1', s.x).attr('y1', s.y).attr('x2', t.x).attr('y2', t.y)
      .attr('stroke', '#bbb').attr('stroke-width', Math.max(0.5, e.w * 0.35)).attr('opacity', 0.35);
  });

  // Draw denied edges (dashed red, with lock at midpoint)
  denied.forEach(function(e) {
    var s = nodeMap[e.src], t = nodeMap[e.tgt];
    linkLayer.append('line').attr('x1', s.x).attr('y1', s.y).attr('x2', t.x).attr('y2', t.y)
      .attr('stroke', '#ef4444').attr('stroke-width', 0.8).attr('opacity', 0.25)
      .attr('stroke-dasharray', '4,4');
    var mx = (s.x + t.x) / 2, my = (s.y + t.y) / 2;
    linkLayer.append('text').attr('x', mx).attr('y', my + 3)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#ef4444').attr('opacity', 0.5).text('\u{1F512}');
  });

  // Draw nodes
  function drawNode(n, isRouter) {
    var g = nodeLayer.append('g').style('cursor', 'pointer');
    if (isRouter) {
      // Routers: solid circles with icon-like feel
      g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', n.r)
        .attr('fill', n.color).attr('opacity', 0.2).attr('stroke', n.color).attr('stroke-width', 2);
      g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', n.r * 0.5)
        .attr('fill', n.color).attr('opacity', 0.6);
    } else if (n.type === 'tool') {
      // Tools: rounded rectangles
      g.append('rect').attr('x', n.x - n.r).attr('y', n.y - n.r * 0.7).attr('width', n.r * 2).attr('height', n.r * 1.4)
        .attr('rx', 4).attr('fill', n.color).attr('opacity', 0.12).attr('stroke', n.color).attr('stroke-width', 1.5);
    } else if (n.type === 'kb') {
      // Knowledge bases: diamonds
      var pts = (n.x) + ',' + (n.y - n.r) + ' ' + (n.x + n.r) + ',' + (n.y) + ' ' + (n.x) + ',' + (n.y + n.r) + ' ' + (n.x - n.r) + ',' + (n.y);
      g.append('polygon').attr('points', pts)
        .attr('fill', n.color).attr('opacity', 0.12).attr('stroke', n.color).attr('stroke-width', 1.5);
    } else {
      // Agents: circles
      g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', n.r)
        .attr('fill', n.color).attr('opacity', 0.12).attr('stroke', n.color).attr('stroke-width', 1.5);
    }
    g.append('text').attr('x', n.x).attr('y', n.y + (isRouter ? n.r + 12 : 4))
      .attr('text-anchor', 'middle').attr('font-size', isRouter ? '9px' : '8px')
      .attr('font-weight', 'bold').attr('fill', n.color).text(n.id);
    return g;
  }
  routers.forEach(function(r) { drawNode(r, true); });
  market.forEach(function(m) { drawNode(m, false); });

  // Animated traffic: query packets flow from routers to specialists
  var timers = [];
  function sendPacket(e) {
    var s = nodeMap[e.src], t = nodeMap[e.tgt];
    pktLayer.append('circle').attr('cx', s.x).attr('cy', s.y)
      .attr('r', 2.5).attr('fill', s.color).attr('opacity', 0.8)
      .transition().duration(600 + Math.random() * 400)
      .attr('cx', t.x).attr('cy', t.y)
      .transition().duration(200).attr('opacity', 0).remove();
  }

  function traffic() {
    // Each round, send packets proportional to weight
    edges.forEach(function(e) {
      if (Math.random() < e.w * 0.15) {
        timers.push(setTimeout(function() { sendPacket(e); }, Math.random() * 800));
      }
    });
    timers.push(setTimeout(traffic, 1000));
  }
  timers.push(setTimeout(traffic, 1500));

  svg.on('click', function() {
    timers.forEach(function(t) { clearTimeout(t); });
    timers = [];
    pktLayer.selectAll('*').remove();
    timers.push(setTimeout(traffic, 500));
  });
})();

// ============================================================
// Visualization 4: Neuro-Symbolic Bridge (cleaner lines)
// ============================================================
(function() {
  var container = document.getElementById('viz-neurosymbolic');
  if (!container) return;
  var width = 720, height = 420;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  // Layout
  var neural = [
    { label: 'Intuition', x: 100, y: 120, r: 26, color: '#b91c1c' },
    { label: 'Language',  x: 100, y: 210, r: 24, color: '#d97706' },
    { label: 'Vision',    x: 100, y: 300, r: 22, color: '#ea580c' }
  ];
  var symbolic = [
    { label: 'Logic',    x: 620, y: 120, r: 26, color: '#4f46e5' },
    { label: 'Code',     x: 620, y: 210, r: 24, color: '#0891b2' },
    { label: 'Verifier', x: 620, y: 300, r: 22, color: '#059669' }
  ];
  var bridge = { x: 360, y: 210, r: 40, color: '#374151' };

  // Layers: bg -> paths -> packets -> nodes -> ui
  var bgLayer = svg.append('g');
  var pathLayer = svg.append('g');
  var pktLayer = svg.append('g');
  var nodeLayer = svg.append('g');
  var uiLayer = svg.append('g');

  // Zone backgrounds
  bgLayer.append('rect').attr('x', 25).attr('y', 65).attr('width', 170).attr('height', 280).attr('rx', 12)
    .attr('fill', '#b91c1c').attr('opacity', 0.04);
  bgLayer.append('rect').attr('x', 525).attr('y', 65).attr('width', 170).attr('height', 280).attr('rx', 12)
    .attr('fill', '#4f46e5').attr('opacity', 0.04);

  // Zone labels
  uiLayer.append('text').attr('x', 110).attr('y', 52).attr('text-anchor', 'middle')
    .attr('font-size', '12px').attr('fill', '#b91c1c').attr('font-weight', 'bold').text('Neural');
  uiLayer.append('text').attr('x', 610).attr('y', 52).attr('text-anchor', 'middle')
    .attr('font-size', '12px').attr('fill', '#4f46e5').attr('font-weight', 'bold').text('Symbolic');

  // Arrowheads
  var defs = svg.append('defs');
  defs.append('marker').attr('id', 'ns-arr').attr('viewBox', '0 0 10 10')
    .attr('refX', 9).attr('refY', 5).attr('markerWidth', 6).attr('markerHeight', 6).attr('orient', 'auto')
    .append('path').attr('d', 'M 0 0 L 10 5 L 0 10 Z').attr('fill', '#bbb');

  // Build path data for neural->bridge, bridge->symbolic, symbolic->bridge (feedback)
  var neuralPaths = [], symbolicPaths = [], feedbackPaths = [];
  neural.forEach(function(n, i) {
    var d = 'M ' + (n.x + n.r + 4) + ' ' + n.y + ' Q ' + 230 + ' ' + n.y + ' ' + (bridge.x - bridge.r - 4) + ' ' + bridge.y;
    var p = pathLayer.append('path').attr('d', d)
      .attr('fill', 'none').attr('stroke', n.color).attr('stroke-width', 1.5).attr('opacity', 0.2);
    neuralPaths.push({ path: p, node: n, d: d });
  });
  symbolic.forEach(function(s, i) {
    var d = 'M ' + (bridge.x + bridge.r + 4) + ' ' + bridge.y + ' Q ' + 490 + ' ' + s.y + ' ' + (s.x - s.r - 4) + ' ' + s.y;
    var p = pathLayer.append('path').attr('d', d)
      .attr('fill', 'none').attr('stroke', s.color).attr('stroke-width', 1.5).attr('opacity', 0.2);
    symbolicPaths.push({ path: p, node: s, d: d });
  });
  symbolic.forEach(function(s, i) {
    var d = 'M ' + (s.x - s.r - 4) + ' ' + (s.y + 10) + ' Q ' + 490 + ' ' + (s.y + 40) + ' ' + (bridge.x + bridge.r + 4) + ' ' + (bridge.y + 10);
    var p = pathLayer.append('path').attr('d', d)
      .attr('fill', 'none').attr('stroke', '#ccc').attr('stroke-width', 1).attr('stroke-dasharray', '4,3').attr('opacity', 0.3);
    feedbackPaths.push({ path: p, node: s, d: d });
  });

  // Draw nodes on top
  neural.concat(symbolic).forEach(function(n) {
    nodeLayer.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', n.r)
      .attr('fill', n.color).attr('opacity', 0.12).attr('stroke', n.color).attr('stroke-width', 2);
    nodeLayer.append('text').attr('x', n.x).attr('y', n.y + 4).attr('text-anchor', 'middle')
      .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', n.color).text(n.label);
  });

  // Bridge node
  nodeLayer.append('circle').attr('cx', bridge.x).attr('cy', bridge.y).attr('r', bridge.r)
    .attr('fill', bridge.color).attr('opacity', 0.08).attr('stroke', bridge.color).attr('stroke-width', 2.5);
  var bridgeText = nodeLayer.append('text').attr('x', bridge.x).attr('y', bridge.y - 4).attr('text-anchor', 'middle')
    .attr('font-size', '12px').attr('font-weight', 'bold').attr('fill', bridge.color).text('LLM');
  nodeLayer.append('text').attr('x', bridge.x).attr('y', bridge.y + 12).attr('text-anchor', 'middle')
    .attr('font-size', '10px').attr('fill', '#888').text('Bridge');

  // Flow labels
  uiLayer.append('text').attr('x', 230).attr('y', 178).attr('text-anchor', 'middle')
    .attr('font-size', '9px').attr('fill', '#888').attr('font-style', 'italic').text('propose');
  uiLayer.append('text').attr('x', 490).attr('y', 178).attr('text-anchor', 'middle')
    .attr('font-size', '9px').attr('fill', '#888').attr('font-style', 'italic').text('verify');
  uiLayer.append('text').attr('x', 490).attr('y', 370).attr('text-anchor', 'middle')
    .attr('font-size', '9px').attr('fill', '#aaa').attr('font-style', 'italic').text('feedback');

  // Iteration counter
  var iterLabel = uiLayer.append('text').attr('x', bridge.x).attr('y', 400)
    .attr('text-anchor', 'middle').attr('font-size', '10px').attr('fill', '#999');

  // Status indicator on bridge
  var statusDot = nodeLayer.append('circle').attr('cx', bridge.x + bridge.r + 8).attr('cy', bridge.y - bridge.r + 8)
    .attr('r', 6).attr('fill', '#ccc').attr('opacity', 0);

  // Animate a packet along a path
  function sendPacket(pathData, color, dur, onDone) {
    var pathEl = pathData.path.node();
    var len = pathEl.getTotalLength();
    var pkt = pktLayer.append('circle').attr('r', 5).attr('fill', color).attr('opacity', 0.9);
    pkt.transition().duration(dur).ease(d3.easeLinear)
      .attrTween('cx', function() { return function(t) { return pathEl.getPointAtLength(t * len).x; }; })
      .attrTween('cy', function() { return function(t) { return pathEl.getPointAtLength(t * len).y; }; })
      .on('end', function() { pkt.remove(); if (onDone) onDone(); });
    // Highlight the path while packet travels
    pathData.path.transition().duration(100).attr('opacity', 0.6)
      .transition().delay(dur).duration(300).attr('opacity', 0.2);
    return pkt;
  }

  // Pulse a node (glow effect)
  function pulseNode(n, color, dur) {
    var g = pktLayer.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', n.r)
      .attr('fill', color || n.color).attr('opacity', 0.3);
    g.transition().duration(dur || 400).attr('r', n.r + 10).attr('opacity', 0)
      .on('end', function() { g.remove(); });
  }

  // Tasks: each is a scenario with a neural source, symbolic target, and whether it passes
  var tasks = [
    { neural: 0, symbolic: 1, pass: false, label: 'generate code' },
    { neural: 1, symbolic: 0, pass: false, label: 'check logic' },
    { neural: 2, symbolic: 2, pass: true,  label: 'verify image' },
    { neural: 0, symbolic: 2, pass: false, label: 'validate claim' },
    { neural: 1, symbolic: 1, pass: true,  label: 'translate code' },
    { neural: 2, symbolic: 0, pass: true,  label: 'parse diagram' }
  ];

  var running = true, taskIdx = 0, timeout;

  function runTask() {
    if (!running) return;
    var task = tasks[taskIdx % tasks.length];
    taskIdx++;
    var nIdx = task.neural;
    var sIdx = task.symbolic;
    var attempt = 1;
    var maxAttempts = task.pass ? 1 : 2; // fails once then succeeds on retry

    iterLabel.text('Task: ' + task.label);

    function propose() {
      // 1. Neural agent pulses, sends proposal to bridge
      pulseNode(neural[nIdx], neural[nIdx].color);
      sendPacket(neuralPaths[nIdx], neural[nIdx].color, 800, function() {
        // 2. Bridge receives, brief pause, forwards to symbolic
        bridgeText.transition().duration(200).attr('fill', neural[nIdx].color)
          .transition().delay(300).duration(200).attr('fill', bridge.color);
        setTimeout(function() {
          sendPacket(symbolicPaths[sIdx], symbolic[sIdx].color, 800, function() {
            // 3. Symbolic agent processes
            pulseNode(symbolic[sIdx], symbolic[sIdx].color);
            setTimeout(function() { verify(); }, 500);
          });
        }, 500);
      });
    }

    function verify() {
      var passes = attempt >= maxAttempts; // first attempt fails if task.pass is false
      // Show result on symbolic node
      var resultColor = passes ? '#059669' : '#dc2626';
      var resultText = passes ? '\u2713' : '\u2717';
      statusDot.attr('fill', resultColor).attr('opacity', 0.9);
      statusDot.transition().delay(1500).duration(400).attr('opacity', 0);

      // Flash result near symbolic node
      var flash = pktLayer.append('text')
        .attr('x', symbolic[sIdx].x + symbolic[sIdx].r + 14).attr('y', symbolic[sIdx].y + 5)
        .attr('text-anchor', 'middle').attr('font-size', '16px').attr('font-weight', 'bold')
        .attr('fill', resultColor).text(resultText).attr('opacity', 0);
      flash.transition().duration(200).attr('opacity', 1)
        .transition().delay(800).duration(400).attr('opacity', 0)
        .on('end', function() { flash.remove(); });

      if (!passes) {
        // 4. Feedback: symbolic sends rejection back to bridge
        iterLabel.text('Task: ' + task.label + ' (retry)');
        setTimeout(function() {
          sendPacket(feedbackPaths[sIdx], '#e57373', 600, function() {
            // Bridge receives feedback, relays to neural
            bridgeText.transition().duration(200).attr('fill', '#e57373')
              .transition().delay(200).duration(200).attr('fill', bridge.color);
            setTimeout(function() {
              attempt++;
              propose(); // retry
            }, 400);
          });
        }, 1000);
      } else {
        // Success, move to next task after pause
        setTimeout(function() {
          iterLabel.text('');
          timeout = setTimeout(runTask, 1200);
        }, 1500);
      }
    }

    propose();
  }

  // Start
  timeout = setTimeout(runTask, 1500);

  // Click to restart
  svg.on('click', function() {
    running = false;
    if (timeout) clearTimeout(timeout);
    pktLayer.selectAll('*').remove();
    statusDot.attr('opacity', 0);
    iterLabel.text('');
    taskIdx = 0;
    running = true;
    timeout = setTimeout(runTask, 800);
  });
})();

// ============================================================
// Visualization 5: Memory Collective (restartable, prettier)
// ============================================================
(function() {
  var container = document.getElementById('viz-memory');
  if (!container) return;
  var width = 720, height = 420;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  // Agents sit around the perimeter, Agent Wikipedia grows in the center
  // 2 general + 3 specialist agents on inner ring, each with 2-3 humans on outer ring
  var agentDefs = [
    { id: 'General Agent',    color: '#2563eb', type: 'general', nHumans: 3 },
    { id: 'General Agent',    color: '#0891b2', type: 'general', nHumans: 3 },
    { id: 'Code Agent',       color: '#dc2626', type: 'specialist', nHumans: 2 },
    { id: 'Research Agent',   color: '#059669', type: 'specialist', nHumans: 2 },
    { id: 'Analysis Agent',   color: '#7c3aed', type: 'specialist', nHumans: 2 }
  ];
  var agents = agentDefs.map(function(d, i) {
    d.angle = -Math.PI/2 + (2 * Math.PI * i / agentDefs.length);
    return d;
  });
  // Generate humans clustered near their agent
  var humanDefs = [];
  agents.forEach(function(a) {
    for (var i = 0; i < a.nHumans; i++) {
      var spread = 0.18;
      var hAngle = a.angle + (i - (a.nHumans - 1) / 2) * spread;
      humanDefs.push({ angle: hAngle, agentIdx: agents.indexOf(a) });
    }
  });
  var cx = width / 2, cy = height / 2;
  var agentR = 120;  // radius of agent ring
  var humanR = 185;  // radius of human ring (outer)
  var memR = 55;     // radius of memory zone

  agents.forEach(function(a) {
    a.x = cx + Math.cos(a.angle) * agentR;
    a.y = cy + Math.sin(a.angle) * agentR;
  });
  humanDefs.forEach(function(h) {
    h.x = cx + Math.cos(h.angle) * humanR;
    h.y = cy + Math.sin(h.angle) * humanR;
    h.agent = agents[h.agentIdx];
  });

  var knowledgeNodes, knowledgeLinks, timers, corrupted;

  function init() {
    svg.selectAll('*').remove();
    timers = [];
    knowledgeNodes = [];
    knowledgeLinks = [];
    corrupted = false;

    var bgLayer = svg.append('g');
    var linkLayer = svg.append('g');
    var particleLayer = svg.append('g');
    var nodeLayer = svg.append('g');
    var agentLayer = svg.append('g');
    var uiLayer = svg.append('g');

    // Memory zone (faint circle in center)
    bgLayer.append('circle').attr('cx', cx).attr('cy', cy).attr('r', memR + 16)
      .attr('fill', '#f8f8f8').attr('stroke', '#e5e5e5').attr('stroke-width', 1)
      .attr('stroke-dasharray', '6,4');
    bgLayer.append('text').attr('x', cx).attr('y', cy - memR - 4)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#ccc').text('AGENT WIKIPEDIA');
    bgLayer.append('text').attr('x', cx).attr('y', cy - memR + 6)
      .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#ddd').text('(shared memory)');

    // Draw humans on outer ring (small stick figures)
    humanDefs.forEach(function(h) {
      agentLayer.append('circle').attr('cx', h.x).attr('cy', h.y - 5).attr('r', 3.5)
        .attr('fill', 'none').attr('stroke', '#aaa').attr('stroke-width', 1);
      agentLayer.append('line').attr('x1', h.x).attr('y1', h.y - 1.5).attr('x2', h.x).attr('y2', h.y + 5)
        .attr('stroke', '#aaa').attr('stroke-width', 1);
      agentLayer.append('line').attr('x1', h.x - 3.5).attr('y1', h.y + 1).attr('x2', h.x + 3.5).attr('y2', h.y + 1)
        .attr('stroke', '#aaa').attr('stroke-width', 1);
      // Faint line to their agent
      bgLayer.append('line').attr('x1', h.x).attr('y1', h.y).attr('x2', h.agent.x).attr('y2', h.agent.y)
        .attr('stroke', '#e8e8e8').attr('stroke-width', 0.5);
    });

    // Draw agents on inner ring
    agents.forEach(function(a) {
      agentLayer.append('circle').attr('cx', a.x).attr('cy', a.y).attr('r', 14)
        .attr('fill', a.color).attr('opacity', 0.10).attr('stroke', a.color).attr('stroke-width', 1.5);
      agentLayer.append('circle').attr('cx', a.x).attr('cy', a.y).attr('r', 5)
        .attr('fill', a.color).attr('opacity', 0.6);
      // Label just outside agent ring
      var labelDist = agentR + 16;
      var lx = cx + Math.cos(a.angle) * labelDist;
      var ly = cy + Math.sin(a.angle) * labelDist;
      agentLayer.append('text').attr('x', lx).attr('y', ly + 3)
        .attr('text-anchor', 'middle').attr('font-size', '6.5px').attr('font-weight', 'bold')
        .attr('fill', a.color).text(a.id);
    });

    // Status label
    var statusLabel = uiLayer.append('text').attr('x', width - 10).attr('y', 14)
      .attr('text-anchor', 'end').attr('font-size', '9px').attr('fill', '#aaa');

    function addKnowledge(fromAgent) {
      // Place new knowledge node inside memory zone
      var angle = Math.random() * Math.PI * 2;
      var dist = Math.random() * memR;
      var nx = cx + Math.cos(angle) * dist;
      var ny = cy + Math.sin(angle) * dist;
      var node = { x: nx, y: ny, r: 2.5 + Math.random() * 2, color: fromAgent.color, id: knowledgeNodes.length, corrupted: false };
      knowledgeNodes.push(node);

      // Connect to 1-2 nearest existing nodes
      if (knowledgeNodes.length > 1) {
        var sorted = knowledgeNodes.slice(0, -1).filter(function(n) { return n !== node; })
          .map(function(n) {
            var dx = n.x - nx, dy = n.y - ny;
            return { node: n, d: Math.sqrt(dx*dx + dy*dy) };
          }).sort(function(a, b) { return a.d - b.d; });
        var links = Math.min(1 + Math.floor(Math.random() * 2), sorted.length);
        for (var i = 0; i < links; i++) {
          knowledgeLinks.push({ source: node, target: sorted[i].node });
        }
      }

      // Animate write particle (agent -> memory)
      particleLayer.append('circle')
        .attr('cx', fromAgent.x).attr('cy', fromAgent.y)
        .attr('r', 3).attr('fill', fromAgent.color).attr('opacity', 0.8)
        .transition().duration(500)
        .attr('cx', nx).attr('cy', ny).attr('r', 1.5)
        .transition().duration(200).attr('opacity', 0).remove();

      // Draw the new node and links
      renderNew(node);
      statusLabel.text(knowledgeNodes.length + ' memories');
    }

    function readKnowledge(agent) {
      // Pick a random knowledge node and animate read (memory -> agent)
      if (knowledgeNodes.length === 0) return;
      var node = knowledgeNodes[Math.floor(Math.random() * knowledgeNodes.length)];
      particleLayer.append('circle')
        .attr('cx', node.x).attr('cy', node.y)
        .attr('r', 2).attr('fill', node.corrupted ? '#dc2626' : '#888').attr('opacity', 0.7)
        .transition().duration(500)
        .attr('cx', agent.x).attr('cy', agent.y)
        .transition().duration(200).attr('opacity', 0).remove();
    }

    function renderNew(node) {
      // Draw new links (behind nodes)
      var newLinks = knowledgeLinks.filter(function(l) { return l.source === node || l.target === node; });
      newLinks.forEach(function(l) {
        linkLayer.append('line')
          .attr('x1', l.source.x).attr('y1', l.source.y)
          .attr('x2', l.source.x).attr('y2', l.source.y)
          .attr('stroke', '#ddd').attr('stroke-width', 0.6).attr('opacity', 0)
          .transition().duration(400)
          .attr('x2', l.target.x).attr('y2', l.target.y)
          .attr('opacity', 0.4);
      });
      // Draw new node (on top)
      nodeLayer.append('circle')
        .attr('cx', node.x).attr('cy', node.y)
        .attr('r', 0).attr('fill', node.color).attr('opacity', 0.7)
        .attr('stroke', 'white').attr('stroke-width', 0.5)
        .attr('data-id', node.id)
        .transition().duration(400).attr('r', node.r);
    }

    function corruptNode() {
      if (corrupted || knowledgeNodes.length < 15) return;
      corrupted = true;
      statusLabel.text(knowledgeNodes.length + ' memories (corruption spreading...)').attr('fill', '#dc2626');

      // Pick a well-connected node to corrupt
      var connectivity = knowledgeNodes.map(function(n) {
        var count = knowledgeLinks.filter(function(l) { return l.source === n || l.target === n; }).length;
        return { node: n, count: count };
      }).sort(function(a, b) { return b.count - a.count; });
      var poisoned = connectivity[0].node;
      poisoned.corrupted = true;

      // Visually corrupt it
      nodeLayer.selectAll('circle').filter(function() {
        return +d3.select(this).attr('data-id') === poisoned.id;
      }).transition().duration(300).attr('fill', '#dc2626').attr('r', poisoned.r + 2).attr('opacity', 0.9);

      // Spread corruption through connected nodes over time
      var spreadQueue = [poisoned];
      var spreadIdx = 0;
      var spreadTimer = setInterval(function() {
        if (spreadIdx >= spreadQueue.length) { clearInterval(spreadTimer); return; }
        var current = spreadQueue[spreadIdx];
        spreadIdx++;
        // Find neighbors
        knowledgeLinks.forEach(function(l) {
          var neighbor = null;
          if (l.source === current && !l.target.corrupted) neighbor = l.target;
          if (l.target === current && !l.source.corrupted) neighbor = l.source;
          if (neighbor && Math.random() < 0.6) {
            neighbor.corrupted = true;
            spreadQueue.push(neighbor);
            // Visual: turn red
            nodeLayer.selectAll('circle').filter(function() {
              return +d3.select(this).attr('data-id') === neighbor.id;
            }).transition().duration(400).attr('fill', '#dc2626').attr('opacity', 0.85);
            // Flash the link
            linkLayer.selectAll('line').filter(function(d) {
              return (d && ((d.source === current && d.target === neighbor) || (d.target === current && d.source === neighbor)));
            }).transition().duration(200).attr('stroke', '#f87171').attr('opacity', 0.7)
              .transition().duration(600).attr('stroke', '#f87171').attr('opacity', 0.4);
          }
        });
      }, 400);
      timers.push(spreadTimer);
    }

    // Human tries to read memory directly - gets "?" back
    function humanProbe() {
      if (knowledgeNodes.length === 0) return;
      var h = humanDefs[Math.floor(Math.random() * humanDefs.length)];
      var node = knowledgeNodes[Math.floor(Math.random() * knowledgeNodes.length)];
      // Query particle from human toward memory
      particleLayer.append('circle')
        .attr('cx', h.x).attr('cy', h.y).attr('r', 2)
        .attr('fill', '#aaa').attr('opacity', 0.5)
        .transition().duration(500)
        .attr('cx', node.x).attr('cy', node.y)
        .transition().duration(100).attr('opacity', 0).remove();
      // "?" floats back
      particleLayer.append('text').text('?')
        .attr('x', node.x).attr('y', node.y)
        .attr('text-anchor', 'middle').attr('font-size', '9px')
        .attr('fill', '#bbb').attr('opacity', 0)
        .transition().delay(600).duration(200).attr('opacity', 0.7)
        .attr('x', h.x + 8).attr('y', h.y - 6)
        .transition().duration(700).attr('opacity', 0).remove();
    }

    // Curator agent - appears every 5 memories, scans, generates questions, leaves
    var curatorColor = '#f59e0b';
    var curatorQuestions = ['gaps?', 'stale?', 'conflict?', 'missing link?', 'verify?', 'explore?'];
    function curatorSweep() {
      var entryAngle = Math.random() * Math.PI * 2;
      var ex = cx + Math.cos(entryAngle) * (agentR + 10);
      var ey = cy + Math.sin(entryAngle) * (agentR + 10);
      var curatorDot = particleLayer.append('circle')
        .attr('cx', ex).attr('cy', ey).attr('r', 4)
        .attr('fill', curatorColor).attr('opacity', 0.8)
        .attr('stroke', curatorColor).attr('stroke-width', 1);
      var curatorLabel = particleLayer.append('text').text('curator')
        .attr('x', ex + 8).attr('y', ey + 3)
        .attr('text-anchor', 'start').attr('font-size', '6px')
        .attr('fill', curatorColor).attr('opacity', 0.7);
      // Phase 1: Move to center (600ms)
      curatorDot.transition().duration(600).attr('cx', cx).attr('cy', cy);
      curatorLabel.transition().duration(600).attr('x', cx + 8).attr('y', cy + 3);
      // Phase 2: Scan - expand over memory zone (1200ms)
      curatorDot.transition().delay(600).duration(600)
        .attr('r', memR * 0.7).attr('opacity', 0.06).attr('stroke-width', 1.5)
        .transition().duration(600)
        .attr('r', memR * 0.3).attr('opacity', 0.10);
      // Phase 2b: Checkmark after scan
      particleLayer.append('text').text('\u2713')
        .attr('x', cx + 14).attr('y', cy - 8)
        .attr('text-anchor', 'middle').attr('font-size', '10px')
        .attr('fill', curatorColor).attr('opacity', 0)
        .transition().delay(1400).duration(200).attr('opacity', 0.7)
        .transition().delay(600).duration(400).attr('opacity', 0).remove();
      // Phase 3: Generate questions - send them to random agents (delay 2200ms)
      var nQuestions = 2 + Math.floor(Math.random() * 2);
      for (var q = 0; q < nQuestions; q++) {
        var targetAgent = agents[Math.floor(Math.random() * agents.length)];
        var qText = curatorQuestions[Math.floor(Math.random() * curatorQuestions.length)];
        var qDelay = 2200 + q * 400;
        (function(ta, qt, qd) {
          // Question particle flies from center to agent
          particleLayer.append('circle')
            .attr('cx', cx).attr('cy', cy).attr('r', 2.5)
            .attr('fill', curatorColor).attr('opacity', 0)
            .transition().delay(qd).duration(100).attr('opacity', 0.7)
            .transition().duration(400)
            .attr('cx', ta.x).attr('cy', ta.y)
            .transition().duration(200).attr('opacity', 0).remove();
          // Question label appears near the agent briefly
          particleLayer.append('text').text(qt)
            .attr('x', ta.x).attr('y', ta.y - 16)
            .attr('text-anchor', 'middle').attr('font-size', '6px')
            .attr('fill', curatorColor).attr('opacity', 0)
            .transition().delay(qd + 500).duration(200).attr('opacity', 0.6)
            .transition().delay(800).duration(400).attr('opacity', 0).remove();
        })(targetAgent, qText, qDelay);
      }
      // Phase 4: Curator leaves (delay ~3600ms)
      curatorDot.transition().delay(3400).duration(500)
        .attr('r', 4).attr('opacity', 0.8).attr('stroke-width', 1)
        .attr('cx', ex).attr('cy', ey)
        .transition().duration(400).attr('opacity', 0).remove();
      curatorLabel.transition().delay(3400).duration(500)
        .attr('x', ex + 8).attr('y', ey + 3)
        .transition().duration(400).attr('opacity', 0).remove();
    }

    // Growth cycle: parallel human requests, agents write to memory
    var step = 0;
    var memoryCount = 0;
    var growTimer = setInterval(function() {
      step++;
      // Multiple humans fire requests in parallel (2-3 at a time)
      var nParallel = 2 + Math.floor(Math.random() * 2);
      for (var p = 0; p < nParallel; p++) {
        var h = humanDefs[Math.floor(Math.random() * humanDefs.length)];
        var delay = p * 120;
        (function(human, d) {
          particleLayer.append('circle')
            .attr('cx', human.x).attr('cy', human.y).attr('r', 2)
            .attr('fill', '#bbb').attr('opacity', 0.5)
            .transition().delay(d).duration(300)
            .attr('cx', human.agent.x).attr('cy', human.agent.y)
            .transition().duration(100).attr('opacity', 0).remove();
        })(h, delay);
      }

      // Agent writes knowledge to memory
      var writer = agents[Math.floor(Math.random() * agents.length)];
      timers.push(setTimeout(function() {
        addKnowledge(writer);
        memoryCount++;
        // Curator sweep every 5 memories
        if (memoryCount % 10 === 0) curatorSweep();
      }, 350));

      // Agents read from memory
      if (step > 2) {
        var reader = agents[Math.floor(Math.random() * agents.length)];
        readKnowledge(reader);
      }

      // After enough growth, trigger corruption
      if (step === 20) corruptNode();

      // After corruption, humans try to read memory directly - get "?"
      if (step > 22 && step < 30) humanProbe();

      if (step > 30) clearInterval(growTimer);
    }, 800);
    timers.push(growTimer);
  }

  init();
  svg.on('click', function() {
    timers.forEach(function(t) { clearInterval(t); clearTimeout(t); });
    init();
  });
})();

// ============================================================
// Visualization: The Living Society (viz-world) - 10 governance zones
// ============================================================
(function() {
  var container = document.getElementById('viz-world');
  if (!container) return;
  var width = 720, height = 760;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  // Layers: bg -> roads -> kb-links -> packets -> nodes -> labels
  var bgLayer = svg.append('g');
  var roadLayer = svg.append('g');
  var kbLinkLayer = svg.append('g');
  var pktLayer = svg.append('g');
  var nodeLayer = svg.append('g');
  var labelLayer = svg.append('g');

  // Dark background
  bgLayer.append('rect').attr('width', width).attr('height', height).attr('fill', '#fafafa');

  // Zone definitions - arranged in upper 2/3 of canvas, bottom area reserved for Agent Wikipedia
  var zones = [
    { id: 0, name: 'Imperial Core',    arch: 'Autocracy',   x: 120, y: 120, r: 60, color: '#b91c1c', nAgents: 8,  privateKB: true },
    { id: 1, name: 'The Mesh',         arch: 'Zero-Trust',  x: 310, y: 100, r: 52, color: '#374151', nAgents: 7,  privateKB: false },
    { id: 2, name: 'Senate Hall',      arch: 'Senate',      x: 500, y: 120, r: 55, color: '#2563eb', nAgents: 7,  privateKB: true },
    { id: 3, name: 'Exchange Floor',   arch: 'Market',      x: 660, y: 140, r: 48, color: '#d97706', nAgents: 9,  privateKB: false },
    { id: 4, name: 'Artisan Quarter',  arch: 'Guild',       x: 80,  y: 290, r: 55, color: '#059669', nAgents: 8,  privateKB: false },
    { id: 5, name: 'The Hive',         arch: 'Colony',      x: 250, y: 270, r: 60, color: '#7c3aed', nAgents: 11, privateKB: false },
    { id: 6, name: 'Free Port',        arch: 'Market',      x: 420, y: 280, r: 45, color: '#d97706', nAgents: 6,  privateKB: false },
    { id: 7, name: 'Frontier Camp',    arch: 'Colony',      x: 580, y: 290, r: 50, color: '#7c3aed', nAgents: 7,  privateKB: false },
    { id: 8, name: 'Consortium',       arch: 'Guild',       x: 160, y: 440, r: 52, color: '#059669', nAgents: 8,  privateKB: false },
    { id: 9, name: 'Watchtower',       arch: 'Autocracy',   x: 600, y: 440, r: 48, color: '#b91c1c', nAgents: 6,  privateKB: true }
  ];

  // Agent Wikipedia area at bottom center
  var wikiX = 360, wikiY = 620, wikiR = 55;

  // Road connections (edges between zone indices)
  var roads = [
    [0,1],[0,3],[0,4],[1,2],[1,4],[1,5],[2,5],[2,6],
    [3,4],[3,7],[4,5],[4,7],[4,8],[5,6],[5,8],[5,9],[6,9],[7,8],[8,9]
  ];

  // KB positions: offset outward from canvas center
  var canvasCx = width / 2, canvasCy = height / 2;
  zones.forEach(function(z) {
    var dx = z.x - canvasCx, dy = z.y - canvasCy;
    var dist = Math.sqrt(dx * dx + dy * dy) || 1;
    z.kbX = z.x + (dx / dist) * (z.r + 35);
    z.kbY = z.y + (dy / dist) * (z.r + 35);
    // Clamp to canvas
    z.kbX = Math.max(20, Math.min(width - 20, z.kbX));
    z.kbY = Math.max(20, Math.min(height - 20, z.kbY));
  });

  // Agents array
  var agents = [];
  var travelers = []; // agents currently moving between zones
  var epoch = 0;
  var rafId, isVisible = true;

  // Visibility observer
  var observer = new IntersectionObserver(function(entries) {
    isVisible = entries[0].isIntersecting;
  }, { threshold: 0.1 });
  observer.observe(container);

  // Compute archetype node positions for a zone (used by drawStatic and spawnAgents)
  function archPositions(z) {
    var R = z.r * 0.65;
    var positions = [];
    if (z.arch === 'Autocracy') {
      positions.push({ x: z.x, y: z.y }); // hub
      for (var i = 1; i < z.nAgents; i++) {
        var a = ((i - 1) / (z.nAgents - 1)) * Math.PI * 2 - Math.PI / 2;
        positions.push({ x: z.x + Math.cos(a) * R, y: z.y + Math.sin(a) * R });
      }
    } else if (z.arch === 'Zero-Trust') {
      for (var i = 0; i < z.nAgents; i++) {
        var a = (i / z.nAgents) * Math.PI * 2 - Math.PI / 2;
        positions.push({ x: z.x + Math.cos(a) * R, y: z.y + Math.sin(a) * R });
      }
    } else if (z.arch === 'Senate') {
      for (var i = 0; i < z.nAgents; i++) {
        var a = (i / z.nAgents) * Math.PI * 2 - Math.PI / 2;
        positions.push({ x: z.x + Math.cos(a) * R, y: z.y + Math.sin(a) * R });
      }
    } else if (z.arch === 'Market') {
      for (var i = 0; i < z.nAgents; i++) {
        var a = Math.PI * 2 * i / z.nAgents + (Math.random() - 0.5) * 0.4;
        var d = 0.35 * R + Math.random() * 0.6 * R;
        positions.push({ x: z.x + Math.cos(a) * d, y: z.y + Math.sin(a) * d });
      }
    } else if (z.arch === 'Guild') {
      var nClusters = Math.min(3, Math.ceil(z.nAgents / 3));
      z.clusterCenters = [];
      for (var c = 0; c < nClusters; c++) {
        var a = (c / nClusters) * Math.PI * 2 - Math.PI / 2;
        z.clusterCenters.push({ x: z.x + Math.cos(a) * R * 0.55, y: z.y + Math.sin(a) * R * 0.55 });
      }
      for (var i = 0; i < z.nAgents; i++) {
        var cc = z.clusterCenters[i % nClusters];
        positions.push({ x: cc.x + (Math.random() - 0.5) * 14, y: cc.y + (Math.random() - 0.5) * 14 });
      }
    } else if (z.arch === 'Colony') {
      var a1 = { x: z.x - R * 0.3, y: z.y - R * 0.2 };
      var a2 = { x: z.x + R * 0.3, y: z.y + R * 0.2 };
      for (var i = 0; i < z.nAgents; i++) {
        var att = i < z.nAgents / 2 ? a1 : a2;
        positions.push({ x: att.x + (Math.random() - 0.5) * R * 0.5, y: att.y + (Math.random() - 0.5) * R * 0.5 });
      }
    }
    return positions;
  }

  function drawStatic() {
    // Roads between zones
    roads.forEach(function(r) {
      var a = zones[r[0]], b = zones[r[1]];
      roadLayer.append('line')
        .attr('x1', a.x).attr('y1', a.y).attr('x2', b.x).attr('y2', b.y)
        .attr('stroke', '#ddd').attr('stroke-width', 1).attr('stroke-dasharray', '6,4').attr('opacity', 0.5);
    });

    // Zone circles + internal archetype topology
    zones.forEach(function(z) {
      z.circleEl = nodeLayer.append('circle').attr('cx', z.x).attr('cy', z.y).attr('r', z.r)
        .attr('fill', z.color).attr('fill-opacity', 0.03)
        .attr('stroke', z.color).attr('stroke-width', 1.5).attr('stroke-dasharray', '4,3').attr('stroke-opacity', 0.35);

      // Draw permanent internal structure lines
      var pos = archPositions(z);
      if (z.arch === 'Autocracy') {
        // Star: hub-to-leaf lines
        for (var i = 1; i < pos.length; i++) {
          roadLayer.append('line').attr('x1', pos[0].x).attr('y1', pos[0].y)
            .attr('x2', pos[i].x).attr('y2', pos[i].y)
            .attr('stroke', z.color).attr('stroke-width', 0.8).attr('opacity', 0.2);
        }
        // Hub marker
        nodeLayer.append('circle').attr('cx', pos[0].x).attr('cy', pos[0].y).attr('r', 5)
          .attr('fill', z.color).attr('opacity', 0.15);
      } else if (z.arch === 'Zero-Trust') {
        // All-to-all mesh
        for (var i = 0; i < pos.length; i++) for (var j = i + 1; j < pos.length; j++) {
          roadLayer.append('line').attr('x1', pos[i].x).attr('y1', pos[i].y)
            .attr('x2', pos[j].x).attr('y2', pos[j].y)
            .attr('stroke', z.color).attr('stroke-width', 0.4).attr('opacity', 0.12);
        }
      } else if (z.arch === 'Senate') {
        // Ring edges
        for (var i = 0; i < pos.length; i++) {
          var j = (i + 1) % pos.length;
          roadLayer.append('line').attr('x1', pos[i].x).attr('y1', pos[i].y)
            .attr('x2', pos[j].x).attr('y2', pos[j].y)
            .attr('stroke', z.color).attr('stroke-width', 0.8).attr('opacity', 0.2);
        }
      } else if (z.arch === 'Guild' && z.clusterCenters) {
        // Cluster boundaries
        z.clusterCenters.forEach(function(cc) {
          roadLayer.append('circle').attr('cx', cc.x).attr('cy', cc.y).attr('r', 14)
            .attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.6)
            .attr('stroke-dasharray', '2,2').attr('opacity', 0.2);
        });
        // Liaison lines between cluster centers
        for (var i = 0; i < z.clusterCenters.length; i++) {
          var j = (i + 1) % z.clusterCenters.length;
          roadLayer.append('line').attr('x1', z.clusterCenters[i].x).attr('y1', z.clusterCenters[i].y)
            .attr('x2', z.clusterCenters[j].x).attr('y2', z.clusterCenters[j].y)
            .attr('stroke', z.color).attr('stroke-width', 0.5).attr('stroke-dasharray', '3,3').attr('opacity', 0.15);
        }
      }
      // Market and Colony have no permanent structure lines (dynamic/emergent)

      // Small node markers at each archetype position
      pos.forEach(function(p, i) {
        nodeLayer.append('circle').attr('cx', p.x).attr('cy', p.y).attr('r', 2)
          .attr('fill', z.color).attr('opacity', 0.15);
      });
    });

    // KB icons and links
    zones.forEach(function(z) {
      kbLinkLayer.append('line')
        .attr('x1', z.kbX).attr('y1', z.kbY).attr('x2', z.x).attr('y2', z.y)
        .attr('stroke', z.privateKB ? '#991b1b' : '#94a3b8')
        .attr('stroke-width', 0.8).attr('stroke-dasharray', '3,3').attr('opacity', 0.35);
      var kbStroke = z.privateKB ? '#991b1b' : '#475569';
      nodeLayer.append('rect').attr('x', z.kbX - 5).attr('y', z.kbY - 7).attr('width', 10).attr('height', 14)
        .attr('rx', 3).attr('fill', '#f1f5f9').attr('stroke', kbStroke).attr('stroke-width', 1);
      nodeLayer.append('line').attr('x1', z.kbX - 4).attr('y1', z.kbY).attr('x2', z.kbX + 4).attr('y2', z.kbY)
        .attr('stroke', kbStroke).attr('stroke-width', 0.6);
      labelLayer.append('text').attr('x', z.kbX).attr('y', z.kbY + 18)
        .attr('text-anchor', 'middle').attr('font-size', '5px').attr('fill', '#94a3b8').text('KB');
    });

    // Zone labels
    zones.forEach(function(z) {
      labelLayer.append('text').attr('x', z.x).attr('y', z.y + z.r + 12)
        .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', z.color).attr('opacity', 0.8)
        .text(z.name);
      labelLayer.append('text').attr('x', z.x).attr('y', z.y + z.r + 20)
        .attr('text-anchor', 'middle').attr('font-size', '5.5px').attr('fill', '#888').attr('font-style', 'italic')
        .text(z.arch);
    });

    // ---- Agent Wikipedia area (bottom center) ----
    // Soft background zone for Wikipedia
    nodeLayer.append('circle').attr('cx', wikiX).attr('cy', wikiY).attr('r', wikiR)
      .attr('fill', '#fef3c7').attr('fill-opacity', 0.3)
      .attr('stroke', '#d97706').attr('stroke-width', 1.5).attr('stroke-dasharray', '5,3').attr('stroke-opacity', 0.4);

    // Inner knowledge cells (like memory blocks growing)
    for (var i = 0; i < 8; i++) {
      var a = (i / 8) * Math.PI * 2;
      var cx = wikiX + Math.cos(a) * 22, cy = wikiY + Math.sin(a) * 22;
      nodeLayer.append('rect').attr('x', cx - 5).attr('y', cy - 4).attr('width', 10).attr('height', 8)
        .attr('rx', 2).attr('fill', '#fef9c3').attr('stroke', '#d97706').attr('stroke-width', 0.5).attr('opacity', 0.4);
    }

    // Central label
    labelLayer.append('text').attr('x', wikiX).attr('y', wikiY - 2)
      .attr('text-anchor', 'middle').attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', '#92400e')
      .text('AGENT WIKIPEDIA');
    labelLayer.append('text').attr('x', wikiX).attr('y', wikiY + 10)
      .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#b45309').text('(shared memory)');

    // Dashed links from each non-private zone to Wikipedia
    zones.forEach(function(z) {
      if (z.privateKB) return;
      kbLinkLayer.append('line')
        .attr('x1', z.x).attr('y1', z.y).attr('x2', wikiX).attr('y2', wikiY)
        .attr('stroke', '#d97706').attr('stroke-width', 0.6).attr('stroke-dasharray', '3,4').attr('opacity', 0.15);
    });
  }

  function spawnAgents() {
    agents = [];
    zones.forEach(function(z) {
      // Structure agents: fixed at archetype positions, never move
      var pos = archPositions(z);
      pos.forEach(function(p, i) {
        agents.push({
          x: p.x, y: p.y,
          tx: p.x, ty: p.y,
          vx: 0, vy: 0,
          home: z.id,
          localIdx: i,
          visiting: -1,
          structure: true, // permanent, never travels
          el: null
        });
      });
      // Traveler agents: 2-3 extra per zone, free to roam and travel
      var nTravelers = 2 + Math.floor(Math.random() * 2);
      for (var t = 0; t < nTravelers; t++) {
        var a = Math.random() * Math.PI * 2;
        var d = Math.random() * z.r * 0.5;
        agents.push({
          x: z.x + Math.cos(a) * d, y: z.y + Math.sin(a) * d,
          tx: z.x, ty: z.y,
          vx: 0, vy: 0,
          home: z.id,
          localIdx: -1,
          visiting: -1,
          structure: false,
          el: null
        });
      }
    });
    // Draw agent dots
    agents.forEach(function(a) {
      var z = zones[a.home];
      if (a.structure) {
        // Structure agents: slightly larger, more visible
        a.el = pktLayer.append('circle').attr('cx', a.x).attr('cy', a.y).attr('r', 3.5)
          .attr('fill', z.color).attr('opacity', 0.85);
      } else {
        // Travelers: smaller, slightly transparent
        a.el = pktLayer.append('circle').attr('cx', a.x).attr('cy', a.y).attr('r', 2.5)
          .attr('fill', z.color).attr('opacity', 0.55).attr('stroke', z.color).attr('stroke-width', 0.3);
      }
    });
  }

  // Agent physics
  function updateAgents() {
    agents.forEach(function(a) {
      if (a.traveling) return;
      // Structure agents: tiny jitter around fixed position, never leave
      if (a.structure) {
        a.x = a.tx + (Math.random() - 0.5) * 0.4;
        a.y = a.ty + (Math.random() - 0.5) * 0.4;
        a.el.attr('cx', a.x).attr('cy', a.y);
        return;
      }
      // Traveler agents: wander freely inside their current zone
      var z = zones[a.visiting >= 0 ? a.visiting : a.home];
      a.vx += (Math.random() - 0.5) * 0.3;
      a.vy += (Math.random() - 0.5) * 0.3;
      // Soft pull toward zone center
      a.vx += (z.x - a.x) * 0.005;
      a.vy += (z.y - a.y) * 0.005;
      a.vx *= 0.9;
      a.vy *= 0.9;
      a.x += a.vx;
      a.y += a.vy;
      // Hard clamp to zone boundary
      var dx = a.x - z.x, dy = a.y - z.y;
      var dist = Math.sqrt(dx * dx + dy * dy);
      if (dist > z.r - 6) {
        a.x = z.x + dx / dist * (z.r - 6);
        a.y = z.y + dy / dist * (z.r - 6);
      }
      a.el.attr('cx', a.x).attr('cy', a.y);
    });
  }

  // Internal zone micro-animations (archetype-specific behavior)
  var internalCooldown = {};
  function internalAnimations() {
    zones.forEach(function(z) {
      if (internalCooldown[z.id] > epoch) return;
      var zAgents = agents.filter(function(a) { return a.structure && a.home === z.id; });
      if (zAgents.length < 2) return;

      if (z.arch === 'Autocracy') {
        // Hub (agent 0) sends command pulse to a random leaf
        internalCooldown[z.id] = epoch + 40; // ~0.7s
        var hub = zAgents[0];
        var leaf = zAgents[1 + Math.floor(Math.random() * (zAgents.length - 1))];
        if (!leaf) return;
        var p = pktLayer.append('circle').attr('cx', hub.x).attr('cy', hub.y)
          .attr('r', 2).attr('fill', z.color).attr('opacity', 0.8);
        p.transition().duration(500).ease(d3.easeLinear)
          .attr('cx', leaf.x).attr('cy', leaf.y)
          .transition().duration(200).attr('opacity', 0).on('end', function() { p.remove(); });
      } else if (z.arch === 'Zero-Trust') {
        // Random edge verification flash
        internalCooldown[z.id] = epoch + 30;
        var a1 = zAgents[Math.floor(Math.random() * zAgents.length)];
        var a2 = zAgents[Math.floor(Math.random() * zAgents.length)];
        if (a1 === a2) return;
        var ok = Math.random() > 0.2;
        var line = pktLayer.append('line').attr('x1', a1.x).attr('y1', a1.y)
          .attr('x2', a2.x).attr('y2', a2.y)
          .attr('stroke', ok ? '#4ade80' : '#ef4444').attr('stroke-width', 1.2).attr('opacity', 0.7);
        line.transition().duration(500).attr('opacity', 0).on('end', function() { line.remove(); });
      } else if (z.arch === 'Senate') {
        // Debate arc between two random senators
        internalCooldown[z.id] = epoch + 50;
        var s1 = zAgents[Math.floor(Math.random() * zAgents.length)];
        var s2 = zAgents[Math.floor(Math.random() * zAgents.length)];
        if (s1 === s2) return;
        var p = pktLayer.append('circle').attr('cx', s1.x).attr('cy', s1.y)
          .attr('r', 1.5).attr('fill', z.color).attr('opacity', 0.8);
        p.transition().duration(400).ease(d3.easeLinear)
          .attr('cx', s2.x).attr('cy', s2.y)
          .transition().duration(200).attr('opacity', 0).on('end', function() { p.remove(); });
      } else if (z.arch === 'Market') {
        // Random reputation shift: an agent grows or shrinks slightly
        internalCooldown[z.id] = epoch + 70;
        var m = zAgents[Math.floor(Math.random() * zAgents.length)];
        var newR = 2 + Math.random() * 3;
        m.el.transition().duration(400).attr('r', newR)
          .transition().duration(400).attr('r', 3);
      } else if (z.arch === 'Guild') {
        // Packet from one cluster to another via the zone center (liaison path)
        internalCooldown[z.id] = epoch + 55;
        var from = zAgents[Math.floor(Math.random() * zAgents.length)];
        var to = zAgents[Math.floor(Math.random() * zAgents.length)];
        if (from === to) return;
        var p = pktLayer.append('circle').attr('cx', from.x).attr('cy', from.y)
          .attr('r', 1.5).attr('fill', z.color).attr('opacity', 0.7);
        p.transition().duration(300).attr('cx', z.x).attr('cy', z.y)
          .transition().duration(300).attr('cx', to.x).attr('cy', to.y)
          .transition().duration(200).attr('opacity', 0).on('end', function() { p.remove(); });
      } else if (z.arch === 'Colony') {
        // Occasional faint trail between nearby agents
        internalCooldown[z.id] = epoch + 25;
        var c1 = zAgents[Math.floor(Math.random() * zAgents.length)];
        var c2 = zAgents[Math.floor(Math.random() * zAgents.length)];
        if (c1 === c2) return;
        var dx = c1.x - c2.x, dy = c1.y - c2.y;
        if (Math.sqrt(dx * dx + dy * dy) > z.r * 0.5) return; // only nearby
        var line = pktLayer.append('line').attr('x1', c1.x).attr('y1', c1.y)
          .attr('x2', c2.x).attr('y2', c2.y)
          .attr('stroke', z.color).attr('stroke-width', 0.5).attr('opacity', 0.3);
        line.transition().duration(800).attr('opacity', 0).on('end', function() { line.remove(); });
      }
    });
  }

  // Cross-zone travel
  var travelCooldown = {};
  function tryTravel() {
    zones.forEach(function(z) {
      if (travelCooldown[z.id] > epoch) return;
      // Only traveler agents can travel (structure agents never leave)
      var homeAgents = agents.filter(function(a) { return !a.structure && a.home === z.id && !a.traveling && a.visiting < 0; });
      if (homeAgents.length < 1) return;
      // Find connected zones
      var neighbors = [];
      roads.forEach(function(r) {
        if (r[0] === z.id) neighbors.push(r[1]);
        if (r[1] === z.id) neighbors.push(r[0]);
      });
      if (!neighbors.length) return;

      var destId = neighbors[Math.floor(Math.random() * neighbors.length)];
      var dest = zones[destId];
      var agent = homeAgents[Math.floor(Math.random() * homeAgents.length)];
      agent.traveling = true;
      travelCooldown[z.id] = epoch + 180 + Math.floor(Math.random() * 120); // 3-5s at 60fps

      // Calculate travel duration based on distance
      var dx = dest.x - z.x, dy = dest.y - z.y;
      var dist = Math.sqrt(dx * dx + dy * dy);
      var dur = Math.max(1200, Math.min(2500, dist / 60 * 1000));

      // Animate along road
      agent.el.transition().duration(dur).ease(d3.easeLinear)
        .attr('cx', dest.x + (Math.random() - 0.5) * 20)
        .attr('cy', dest.y + (Math.random() - 0.5) * 20)
        .attr('r', 3.5).attr('opacity', 0.9)
        .on('end', function() {
          agent.traveling = false;
          agent.x = dest.x + (Math.random() - 0.5) * 20;
          agent.y = dest.y + (Math.random() - 0.5) * 20;
          agent.visiting = destId;
          agent.el.attr('r', 3).attr('opacity', 0.7);

          // Return home after 4-8 seconds (or 10% defect)
          var defect = Math.random() < 0.1;
          if (defect) {
            agent.home = destId;
            agent.visiting = -1;
            agent.el.attr('fill', dest.color);
          } else {
            setTimeout(function() {
              if (!isVisible) { agent.visiting = -1; agent.x = z.x; agent.y = z.y; return; }
              agent.traveling = true;
              agent.el.transition().duration(dur).ease(d3.easeLinear)
                .attr('cx', z.x + (Math.random() - 0.5) * 20)
                .attr('cy', z.y + (Math.random() - 0.5) * 20)
                .on('end', function() {
                  agent.traveling = false;
                  agent.visiting = -1;
                  agent.x = z.x + (Math.random() - 0.5) * 20;
                  agent.y = z.y + (Math.random() - 0.5) * 20;
                });
            }, 4000 + Math.random() * 4000);
          }
        });
    });
  }

  // KB data packets
  var kbCooldown = {};
  function kbPackets() {
    zones.forEach(function(z) {
      if (kbCooldown[z.id] > epoch) return;
      kbCooldown[z.id] = epoch + 100 + Math.floor(Math.random() * 60); // ~1.8s
      // Agent reads from KB
      var pkt = pktLayer.append('circle').attr('cx', z.kbX).attr('cy', z.kbY)
        .attr('r', 1.5).attr('fill', '#60a5fa').attr('opacity', 0.8);
      pkt.transition().duration(800).ease(d3.easeLinear)
        .attr('cx', z.x + (Math.random() - 0.5) * z.r * 0.6)
        .attr('cy', z.y + (Math.random() - 0.5) * z.r * 0.6)
        .transition().duration(400).attr('opacity', 0)
        .on('end', function() { pkt.remove(); });
    });
  }

  // Cross-KB sharing packets
  var crossKBCooldown = {};
  function crossKBPackets() {
    roads.forEach(function(r, ri) {
      var a = zones[r[0]], b = zones[r[1]];
      if (a.privateKB || b.privateKB) return;
      if (crossKBCooldown[ri] > epoch) return;
      crossKBCooldown[ri] = epoch + 500 + Math.floor(Math.random() * 200); // ~8-12s
      var pkt = pktLayer.append('circle').attr('cx', a.kbX).attr('cy', a.kbY)
        .attr('r', 1.5).attr('fill', '#93c5fd').attr('opacity', 0.6);
      pkt.transition().duration(2500).ease(d3.easeLinear)
        .attr('cx', b.kbX).attr('cy', b.kbY)
        .transition().duration(300).attr('opacity', 0)
        .on('end', function() { pkt.remove(); });
    });
  }

  // Wikipedia sync packets: zones periodically write to and read from Agent Wikipedia
  var wikiCooldown = {};
  function wikiPackets() {
    // Zone KB <-> Wikipedia exchanges
    zones.forEach(function(z) {
      if (z.privateKB) return;
      if (wikiCooldown[z.id] > epoch) return;
      wikiCooldown[z.id] = epoch + 300 + Math.floor(Math.random() * 200);
      var toWiki = Math.random() > 0.4;
      var fromX = toWiki ? z.kbX : wikiX;
      var fromY = toWiki ? z.kbY : wikiY;
      var toX = toWiki ? wikiX : z.kbX;
      var toY = toWiki ? wikiY : z.kbY;
      var pkt = pktLayer.append('circle').attr('cx', fromX).attr('cy', fromY)
        .attr('r', 2).attr('fill', '#f59e0b').attr('opacity', 0.7);
      pkt.transition().duration(1800).ease(d3.easeLinear)
        .attr('cx', toX).attr('cy', toY)
        .transition().duration(300).attr('opacity', 0)
        .on('end', function() { pkt.remove(); });
    });
    // Individual agents contribute directly to Wikipedia
    var eligible = agents.filter(function(a) {
      return !a.traveling && zones[a.home] && !zones[a.home].privateKB;
    });
    if (eligible.length < 1) return;
    // Pick 1-2 random agents to send a contribution
    var nContrib = 1 + Math.floor(Math.random() * 2);
    for (var i = 0; i < nContrib; i++) {
      var a = eligible[Math.floor(Math.random() * eligible.length)];
      var p = pktLayer.append('circle').attr('cx', a.x).attr('cy', a.y)
        .attr('r', 1.5).attr('fill', '#fbbf24').attr('opacity', 0.6);
      p.transition().duration(1400).ease(d3.easeLinear)
        .attr('cx', wikiX + (Math.random() - 0.5) * 20)
        .attr('cy', wikiY + (Math.random() - 0.5) * 20)
        .transition().duration(300).attr('opacity', 0)
        .on('end', function() { p.remove(); });
    }
  }

  // Boundary events
  var eventLabel = labelLayer.append('text').attr('text-anchor', 'middle')
    .attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', '#333').attr('opacity', 0);
  var eventFired = {};

  function checkEvents() {
    // Zone 6 contracts at epoch 800
    if (epoch === 800 && !eventFired[800]) {
      eventFired[800] = true;
      zones[6].r = 32;
      zones[6].circleEl.transition().duration(1200).attr('r', 32);
      flashEvent(zones[6].x, zones[6].y - 60, 'Free Port contracting...');
    }
    // Zone 3 absorbs zone 6 at epoch 1200
    if (epoch === 1200 && !eventFired[1200]) {
      eventFired[1200] = true;
      zones[6].circleEl.transition().duration(800).attr('stroke-opacity', 0.1).attr('fill-opacity', 0.01);
      // Migrate zone 6 agents to zone 3
      agents.forEach(function(a) {
        if (a.home === 6 && !a.traveling) {
          a.home = 3; a.visiting = -1;
          a.el.transition().duration(1500).attr('cx', zones[3].x + (Math.random()-0.5)*30)
            .attr('cy', zones[3].y + (Math.random()-0.5)*30).attr('fill', zones[3].color);
          a.x = zones[3].x; a.y = zones[3].y;
        }
      });
      flashEvent(zones[3].x, zones[3].y - 70, 'Exchange absorbs Free Port');
    }
    // Zones 4+8 merge at epoch 1600
    if (epoch === 1600 && !eventFired[1600]) {
      eventFired[1600] = true;
      var mergeX = (zones[4].x + zones[8].x) / 2, mergeY = (zones[4].y + zones[8].y) / 2;
      // Pulse arc between them
      var arc = pktLayer.append('line')
        .attr('x1', zones[4].x).attr('y1', zones[4].y)
        .attr('x2', zones[8].x).attr('y2', zones[8].y)
        .attr('stroke', zones[4].color).attr('stroke-width', 3).attr('opacity', 0.6);
      // Merge: zones 4 grows, zone 8 fades
      setTimeout(function() {
        arc.transition().duration(600).attr('opacity', 0).on('end', function() { arc.remove(); });
        zones[4].r = 80;
        zones[4].circleEl.transition().duration(1200).attr('r', 80);
        zones[8].circleEl.transition().duration(1000).attr('stroke-opacity', 0.1).attr('fill-opacity', 0.01);
        agents.forEach(function(a) {
          if (a.home === 8 && !a.traveling) {
            a.home = 4; a.visiting = -1;
            a.el.transition().duration(1500).attr('fill', zones[4].color);
            a.x = zones[4].x + (Math.random()-0.5)*40; a.y = zones[4].y + (Math.random()-0.5)*40;
          }
        });
        flashEvent(mergeX, mergeY - 30, 'Grand Guild formed');
      }, 1500);
    }
    // Zone 7 schism at epoch 2200
    if (epoch === 2200 && !eventFired[2200]) {
      eventFired[2200] = true;
      flashEvent(zones[7].x, zones[7].y - 65, 'Frontier Camp schism');
      // Shrink zone 7 and spawn a ghost splinter
      zones[7].r = 38;
      zones[7].circleEl.transition().duration(800).attr('r', 38);
      var splinter = pktLayer.append('circle')
        .attr('cx', zones[7].x - 50).attr('cy', zones[7].y - 30).attr('r', 0)
        .attr('fill', zones[7].color).attr('fill-opacity', 0.03)
        .attr('stroke', zones[7].color).attr('stroke-width', 1).attr('stroke-dasharray', '3,3').attr('stroke-opacity', 0.3);
      splinter.transition().duration(800).attr('r', 30)
        .transition().delay(4000).duration(1000).attr('r', 0).attr('opacity', 0)
        .on('end', function() { splinter.remove(); });
    }
    // Zone 9 KB contamination at epoch 2800
    if (epoch === 2800 && !eventFired[2800]) {
      eventFired[2800] = true;
      flashEvent(zones[9].x, zones[9].y - 65, 'KB contaminated!');
      // Flash KB red
      nodeLayer.selectAll('rect').filter(function(d, i) { return i === 9; })
        .transition().duration(300).attr('fill', '#ef4444')
        .transition().duration(2000).attr('fill', '#f1f5f9');
      // Flash agents red briefly
      agents.forEach(function(a) {
        if (a.home === 9) {
          a.el.transition().duration(400).attr('fill', '#ef4444').attr('opacity', 0.9)
            .transition().duration(1000).attr('fill', zones[9].color).attr('opacity', 0.7);
        }
      });
    }
    // Full reset at epoch 3200
    if (epoch >= 3200) {
      init();
    }
  }

  function flashEvent(x, y, text) {
    eventLabel.attr('x', x).attr('y', y).text(text)
      .attr('opacity', 0).transition().duration(200).attr('opacity', 1)
      .transition().delay(2000).duration(500).attr('opacity', 0);
  }

  // Main animation loop
  function tick() {
    if (!isVisible) { rafId = requestAnimationFrame(tick); return; }
    epoch++;
    updateAgents();
    internalAnimations(); // archetype-specific micro-animations
    if (epoch % 3 === 0) tryTravel();
    if (epoch % 6 === 0) kbPackets();
    if (epoch % 10 === 0) crossKBPackets();
    if (epoch % 15 === 0) wikiPackets();
    checkEvents();
    rafId = requestAnimationFrame(tick);
  }

  function init() {
    if (rafId) cancelAnimationFrame(rafId);
    epoch = 0;
    eventFired = {};
    travelCooldown = {};
    kbCooldown = {};
    crossKBCooldown = {};
    internalCooldown = {};
    wikiCooldown = {};
    travelers = [];
    // Reset zones to original state
    zones[6].r = 50;
    zones[4].r = 65;
    zones[7].r = 55;
    zones[8].r = 60;
    // Clear dynamic elements
    pktLayer.selectAll('*').remove();
    roadLayer.selectAll('*').remove();
    kbLinkLayer.selectAll('*').remove();
    nodeLayer.selectAll('*').remove();
    labelLayer.selectAll('*').remove();
    eventLabel = labelLayer.append('text').attr('text-anchor', 'middle')
      .attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', '#333').attr('opacity', 0);
    drawStatic();
    spawnAgents();
    tick();
  }

  init();
  svg.on('click', function() { init(); });
})();

// ============================================================
// Visualization 6: Six Governance Archetypes (2x3 grid)
// ============================================================
(function() {
  var container = document.getElementById('viz-society');
  if (!container) return;
  var width = 720, height = 760;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  var cellW = 350, cellH = 240, padX = 5, padY = 10;
  var cols = 2, rows = 3;

  var archetypes = [
    {name:'Autocracy',      color:'#b91c1c', sub:'Hub-spoke \u00B7 Orchestrator dictates'},
    {name:'Zero-Trust Mesh', color:'#374151', sub:'Full mesh \u00B7 Every link verified'},
    {name:'Senate',          color:'#2563eb', sub:'Ring \u00B7 Agents debate & vote'},
    {name:'Market',          color:'#d97706', sub:'Dynamic \u00B7 Reputation tokens'},
    {name:'Guild',           color:'#059669', sub:'Clustered \u00B7 Specialists + liaisons'},
    {name:'Colony',          color:'#7c3aed', sub:'Swarm \u00B7 Emergent local norms'}
  ];

  var defs = svg.append('defs');
  defs.append('marker').attr('id','soc-arr').attr('viewBox','0 0 8 8')
    .attr('refX',7).attr('refY',4).attr('markerWidth',5).attr('markerHeight',5).attr('orient','auto')
    .append('path').attr('d','M 0 0 L 8 4 L 0 8 Z').attr('fill','#ccc');

  var animTimers = [];
  var ztRunning = false; // zero-trust state machine flag

  function init() {
    ztRunning = false;
    svg.selectAll('g.cell').remove();
    animTimers.forEach(function(t) { clearInterval(t); clearTimeout(t); });
    animTimers = [];
    ztRunning = true;

    archetypes.forEach(function(arch, idx) {
      var col = idx % cols, row = Math.floor(idx / cols);
      var ox = padX + col * (cellW + 5), oy = padY + row * (cellH + 12);
      var cx = ox + cellW / 2, cy = oy + 36 + (cellH - 50) / 2;
      var g = svg.append('g').attr('class', 'cell');

      // Cell background
      g.append('rect').attr('x', ox).attr('y', oy).attr('width', cellW).attr('height', cellH)
        .attr('rx', 6).attr('fill', arch.color).attr('opacity', 0.02)
        .attr('stroke', arch.color).attr('stroke-width', 1).attr('stroke-opacity', 0.15);

      // Title & subtitle
      g.append('text').attr('x', ox + cellW/2).attr('y', oy + 16).attr('text-anchor', 'middle')
        .attr('font-size', '11px').attr('font-weight', 'bold').attr('fill', arch.color).text(arch.name);
      g.append('text').attr('x', ox + cellW/2).attr('y', oy + 28).attr('text-anchor', 'middle')
        .attr('font-size', '7.5px').attr('fill', '#999').text(arch.sub);

      var areaTop = oy + 38, areaH = cellH - 48;
      var acx = ox + cellW / 2, acy = areaTop + areaH / 2;
      var R = Math.min(cellW, areaH) / 2 - 18; // radius for layouts

      // ---- AUTOCRACY: star topology ----
      if (idx === 0) {
        var nLeaf = 8;
        // Hub
        g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 10)
          .attr('fill', arch.color).attr('opacity', 0.25).attr('stroke', arch.color).attr('stroke-width', 2);
        g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '7px').attr('font-weight', 'bold').attr('fill', arch.color).text('O');
        var leaves = [];
        for (var i = 0; i < nLeaf; i++) {
          var a = (i / nLeaf) * Math.PI * 2 - Math.PI / 2;
          var lx = acx + Math.cos(a) * R, ly = acy + Math.sin(a) * R;
          leaves.push({x: lx, y: ly});
          g.append('line').attr('x1', acx).attr('y1', acy).attr('x2', lx).attr('y2', ly)
            .attr('stroke', arch.color).attr('stroke-width', 1).attr('opacity', 0.2)
            .attr('marker-end', 'url(#soc-arr)');
          g.append('circle').attr('cx', lx).attr('cy', ly).attr('r', 4)
            .attr('fill', arch.color).attr('opacity', 0.5);
        }
        // Animate: pulse messages from hub to leaves
        var pulseG = g.append('g');
        animTimers.push(setInterval(function() {
          var tgt = leaves[Math.floor(Math.random() * nLeaf)];
          pulseG.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 2.5)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(500).attr('cx', tgt.x).attr('cy', tgt.y)
            .transition().duration(300).attr('opacity', 0).remove();
        }, 600));
      }

      // ---- ZERO-TRUST MESH: challenge-response protocol ----
      if (idx === 1) {
        var nMesh = 8;
        var meshNodes = [];
        for (var i = 0; i < nMesh; i++) {
          var a = (i / nMesh) * Math.PI * 2 - Math.PI / 2;
          meshNodes.push({x: acx + Math.cos(a) * R * 0.85, y: acy + Math.sin(a) * R * 0.85});
        }
        // All-to-all edges (faint background)
        var edgeEls = [];
        for (var i = 0; i < nMesh; i++) for (var j = i + 1; j < nMesh; j++) {
          var el = g.append('line')
            .attr('x1', meshNodes[i].x).attr('y1', meshNodes[i].y)
            .attr('x2', meshNodes[j].x).attr('y2', meshNodes[j].y)
            .attr('stroke', '#ccc').attr('stroke-width', 0.6).attr('opacity', 0.15);
          edgeEls.push({el: el, i: i, j: j});
        }
        // Node circles
        var meshCircles = [];
        meshNodes.forEach(function(n, ni) {
          var c = g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.5).attr('stroke', arch.color).attr('stroke-width', 1.5);
          meshCircles.push(c);
        });
        // Label group for protocol labels
        var ztLabels = g.append('g');

        function ztPacket(fromN, toN, color, r, dur, onDone) {
          var pkt = g.append('circle').attr('cx', fromN.x).attr('cy', fromN.y)
            .attr('r', r).attr('fill', color).attr('opacity', 0.9);
          pkt.transition().duration(dur).ease(d3.easeLinear)
            .attr('cx', toN.x).attr('cy', toN.y)
            .on('end', function() { pkt.remove(); if (onDone) onDone(); });
          return pkt;
        }

        function ztLabel(x, y, text, color, dur) {
          var lbl = ztLabels.append('text').attr('x', x).attr('y', y)
            .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', color || '#888')
            .attr('opacity', 0).text(text);
          lbl.transition().duration(120).attr('opacity', 1)
            .transition().delay(dur || 500).duration(200).attr('opacity', 0)
            .on('end', function() { lbl.remove(); });
        }

        function runZtCycle() {
          if (!ztRunning) return;
          // Pick two different random nodes
          var ai = Math.floor(Math.random() * nMesh);
          var bi = (ai + 1 + Math.floor(Math.random() * (nMesh - 1))) % nMesh;
          var A = meshNodes[ai], B = meshNodes[bi];
          var mx = (A.x + B.x) / 2, my = (A.y + B.y) / 2;

          // Find the edge between them
          var edge = edgeEls.find(function(e) {
            return (e.i === Math.min(ai, bi) && e.j === Math.max(ai, bi));
          });

          // 1. INITIATE: A sends request to B
          meshCircles[ai].transition().duration(150).attr('r', 7).attr('opacity', 0.8)
            .transition().duration(150).attr('r', 5).attr('opacity', 0.5);
          ztLabel(mx, my - 8, 'request?', '#3b82f6', 400);
          ztPacket(A, B, '#3b82f6', 3, 400, function() {
            if (!ztRunning) return;
            // 2. CHALLENGE: B sends credential challenge back
            meshCircles[bi].transition().duration(150).attr('r', 7).attr('stroke', '#f59e0b')
              .transition().duration(300).attr('r', 5).attr('stroke', arch.color);
            ztLabel(mx, my - 8, 'credential?', '#f59e0b', 500);
            ztPacket(B, A, '#f59e0b', 2.5, 500, function() {
              if (!ztRunning) return;
              // 3. PROVE: A sends proof to B
              ztLabel(mx, my - 8, 'proof', '#a78bfa', 600);
              ztPacket(A, B, '#a78bfa', 4, 600, function() {
                if (!ztRunning) return;
                // 4. TRANSFER or REJECT
                var ok = Math.random() > 0.2;
                if (ok) {
                  // Success: green data flows, edge glows
                  if (edge) edge.el.transition().duration(200)
                    .attr('stroke', '#4ade80').attr('opacity', 0.7).attr('stroke-width', 2)
                    .transition().delay(600).duration(400)
                    .attr('stroke', '#ccc').attr('opacity', 0.15).attr('stroke-width', 0.6);
                  ztLabel(mx, my - 8, 'verified', '#4ade80', 500);
                  ztPacket(B, A, '#4ade80', 3, 500, function() {
                    if (!ztRunning) return;
                    setTimeout(runZtCycle, 300);
                  });
                } else {
                  // Rejected: both flash red
                  meshCircles[ai].transition().duration(200).attr('stroke', '#ef4444')
                    .transition().duration(400).attr('stroke', arch.color);
                  meshCircles[bi].transition().duration(200).attr('stroke', '#ef4444')
                    .transition().duration(400).attr('stroke', arch.color);
                  if (edge) edge.el.transition().duration(200)
                    .attr('stroke', '#ef4444').attr('opacity', 0.5).attr('stroke-width', 1.5)
                    .transition().delay(400).duration(300)
                    .attr('stroke', '#ccc').attr('opacity', 0.15).attr('stroke-width', 0.6);
                  ztLabel(mx, my - 8, 'denied', '#ef4444', 500);
                  setTimeout(function() { if (ztRunning) runZtCycle(); }, 800);
                }
              });
            });
          });
        }

        animTimers.push(setTimeout(runZtCycle, 800));
      }

      // ---- SENATE: ring with debate arcs ----
      if (idx === 2) {
        var nSen = 7;
        var senNodes = [];
        for (var i = 0; i < nSen; i++) {
          var a = (i / nSen) * Math.PI * 2 - Math.PI / 2;
          senNodes.push({x: acx + Math.cos(a) * R * 0.8, y: acy + Math.sin(a) * R * 0.8});
        }
        // Ring edges
        for (var i = 0; i < nSen; i++) {
          var j = (i + 1) % nSen;
          g.append('line')
            .attr('x1', senNodes[i].x).attr('y1', senNodes[i].y)
            .attr('x2', senNodes[j].x).attr('y2', senNodes[j].y)
            .attr('stroke', arch.color).attr('stroke-width', 1).attr('opacity', 0.2);
        }
        var nodeEls = [];
        senNodes.forEach(function(n, i) {
          var el = g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.5).attr('stroke', arch.color).attr('stroke-width', 1.5);
          nodeEls.push(el);
        });
        // Center "vote" indicator
        var voteText = g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '8px').attr('fill', '#aaa');
        // Animate: debate arcs + voting
        var debateG = g.append('g');
        var debateStep = 0;
        animTimers.push(setInterval(function() {
          debateStep++;
          if (debateStep % 6 < 4) {
            // Debate: arc from random speaker to random listener
            var si = Math.floor(Math.random() * nSen);
            var ti = (si + 1 + Math.floor(Math.random() * (nSen - 1))) % nSen;
            debateG.append('circle').attr('cx', senNodes[si].x).attr('cy', senNodes[si].y).attr('r', 2)
              .attr('fill', arch.color).attr('opacity', 0.7)
              .transition().duration(400).attr('cx', senNodes[ti].x).attr('cy', senNodes[ti].y)
              .transition().duration(200).attr('opacity', 0).remove();
            voteText.text('debating...');
          } else {
            // Vote: all nodes pulse, show result
            var yea = 0;
            nodeEls.forEach(function(el) {
              var vote = Math.random() > 0.35;
              if (vote) yea++;
              el.transition().duration(200)
                .attr('fill', vote ? '#4ade80' : '#ef4444').attr('r', 7)
                .transition().duration(400)
                .attr('fill', arch.color).attr('r', 5);
            });
            voteText.text(yea + '/' + nSen + (yea > nSen / 2 ? ' \u2713' : ' \u2717'));
          }
        }, 700));
      }

      // ---- MARKET: dynamic edges weighted by reputation ----
      if (idx === 3) {
        var nMkt = 9;
        var mktNodes = [];
        for (var i = 0; i < nMkt; i++) {
          var a = (i / nMkt) * Math.PI * 2 - Math.PI / 2;
          var dist = R * (0.5 + Math.random() * 0.45);
          mktNodes.push({
            x: acx + Math.cos(a) * dist, y: acy + Math.sin(a) * dist,
            rep: 0.3 + Math.random() * 0.7 // reputation score
          });
        }
        var mktEdgeG = g.append('g');
        var mktNodeG = g.append('g');
        function drawMarket() {
          mktEdgeG.selectAll('*').remove();
          mktNodeG.selectAll('*').remove();
          // Draw edges: probability proportional to combined reputation
          for (var i = 0; i < nMkt; i++) for (var j = i + 1; j < nMkt; j++) {
            var strength = mktNodes[i].rep * mktNodes[j].rep;
            if (Math.random() < strength * 0.6) {
              mktEdgeG.append('line')
                .attr('x1', mktNodes[i].x).attr('y1', mktNodes[i].y)
                .attr('x2', mktNodes[j].x).attr('y2', mktNodes[j].y)
                .attr('stroke', arch.color).attr('stroke-width', strength * 2.5).attr('opacity', strength * 0.3);
            }
          }
          mktNodes.forEach(function(n) {
            mktNodeG.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 2.5 + n.rep * 5)
              .attr('fill', arch.color).attr('opacity', 0.15 + n.rep * 0.5)
              .attr('stroke', arch.color).attr('stroke-width', 1.5);
          });
        }
        drawMarket();
        // Animate: reputations shift, edges rewire
        animTimers.push(setInterval(function() {
          mktNodes.forEach(function(n) {
            n.rep = Math.max(0.1, Math.min(1, n.rep + (Math.random() - 0.48) * 0.15));
          });
          drawMarket();
        }, 1200));
      }

      // ---- GUILD: 3 tight clusters + liaison bridges ----
      if (idx === 4) {
        var guildCenters = [
          {x: acx - R * 0.6, y: acy - R * 0.35, n: 4},
          {x: acx + R * 0.6, y: acy - R * 0.35, n: 4},
          {x: acx, y: acy + R * 0.5, n: 4}
        ];
        var guildColors = ['#059669', '#0d9488', '#10b981'];
        var allGuildNodes = [];
        guildCenters.forEach(function(gc, gi) {
          var gNodes = [];
          for (var i = 0; i < gc.n; i++) {
            var a = (i / gc.n) * Math.PI * 2;
            var nr = 22;
            var nx = gc.x + Math.cos(a) * nr, ny = gc.y + Math.sin(a) * nr;
            gNodes.push({x: nx, y: ny, guild: gi});
          }
          // Intra-guild edges (dense)
          for (var i = 0; i < gNodes.length; i++) for (var j = i + 1; j < gNodes.length; j++) {
            g.append('line')
              .attr('x1', gNodes[i].x).attr('y1', gNodes[i].y)
              .attr('x2', gNodes[j].x).attr('y2', gNodes[j].y)
              .attr('stroke', guildColors[gi]).attr('stroke-width', 1).attr('opacity', 0.2);
          }
          gNodes.forEach(function(n) {
            g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 4)
              .attr('fill', guildColors[gi]).attr('opacity', 0.5)
              .attr('stroke', guildColors[gi]).attr('stroke-width', 1.2);
          });
          // Guild boundary
          g.append('circle').attr('cx', gc.x).attr('cy', gc.y).attr('r', 36)
            .attr('fill', 'none').attr('stroke', guildColors[gi])
            .attr('stroke-width', 1).attr('stroke-dasharray', '4,3').attr('opacity', 0.2);
          allGuildNodes.push(gNodes);
        });
        // Liaison agents (between guilds)
        var liaisons = [
          {from: 0, to: 1, x: acx, y: acy - R * 0.35},
          {from: 0, to: 2, x: acx - R * 0.3, y: acy + R * 0.1},
          {from: 1, to: 2, x: acx + R * 0.3, y: acy + R * 0.1}
        ];
        liaisons.forEach(function(l) {
          g.append('circle').attr('cx', l.x).attr('cy', l.y).attr('r', 3.5)
            .attr('fill', '#374151').attr('opacity', 0.6);
          // Dashed lines to guild centers
          [l.from, l.to].forEach(function(gi) {
            g.append('line').attr('x1', l.x).attr('y1', l.y)
              .attr('x2', guildCenters[gi].x).attr('y2', guildCenters[gi].y)
              .attr('stroke', '#aaa').attr('stroke-width', 1.2).attr('stroke-dasharray', '3,3').attr('opacity', 0.3);
          });
        });
        // Animate: messages flow through liaisons
        var liaisonG = g.append('g');
        animTimers.push(setInterval(function() {
          var l = liaisons[Math.floor(Math.random() * liaisons.length)];
          var fromGC = guildCenters[l.from], toGC = guildCenters[l.to];
          liaisonG.append('circle').attr('cx', fromGC.x).attr('cy', fromGC.y).attr('r', 2)
            .attr('fill', '#374151').attr('opacity', 0.7)
            .transition().duration(350).attr('cx', l.x).attr('cy', l.y)
            .transition().duration(350).attr('cx', toGC.x).attr('cy', toGC.y)
            .transition().duration(200).attr('opacity', 0).remove();
        }, 800));
      }

      // ---- COLONY: 5 core attractors, followers, merges & splits ----
      if (idx === 5) {
        var nFollowers = 35;
        var colHues = ['#7c3aed','#9333ea','#6d28d9','#a855f7','#8b5cf6'];
        // 5 core attractor agents
        var cores = [];
        for (var ci = 0; ci < 5; ci++) {
          var angle = (ci / 5) * Math.PI * 2 + 0.3;
          cores.push({
            x: acx + Math.cos(angle) * R * 0.55,
            y: acy + Math.sin(angle) * (areaH * 0.3),
            vx: (Math.random() - 0.5) * 0.3,
            vy: (Math.random() - 0.5) * 0.3,
            alive: true, color: colHues[ci], id: ci
          });
        }
        // Follower agents assigned to nearest core
        var colNodes = [];
        for (var i = 0; i < nFollowers; i++) {
          var home = Math.floor(Math.random() * 5);
          var c = cores[home];
          colNodes.push({
            x: c.x + (Math.random() - 0.5) * 40,
            y: c.y + (Math.random() - 0.5) * 30,
            vx: (Math.random() - 0.5) * 0.5,
            vy: (Math.random() - 0.5) * 0.5,
            home: home
          });
        }

        var trailG = g.append('g');
        var colNodeG = g.append('g');
        var coreEls = cores.map(function(c) {
          return colNodeG.append('circle').attr('cx', c.x).attr('cy', c.y).attr('r', 5)
            .attr('fill', c.color).attr('opacity', 0.8).attr('stroke', 'white').attr('stroke-width', 1);
        });
        var followerEls = colNodeG.selectAll('.follower').data(colNodes).enter().append('circle')
          .attr('class', 'follower').attr('r', 3).attr('opacity', 0.55);

        var isVisible = true;
        if ('IntersectionObserver' in window) {
          var obs = new IntersectionObserver(function(e) { isVisible = e[0].isIntersecting; }, { threshold: 0 });
          obs.observe(container);
        }

        var colFrame, colEpoch = 0;
        function colonyTick() {
          if (!isVisible) { colFrame = requestAnimationFrame(colonyTick); return; }
          colEpoch++;

          // -- Events: merge at ~300, split at ~600, merge again at ~900 --
          if (colEpoch === 300) {
            // Core 1 merges into core 0
            cores[1].alive = false;
            colNodes.forEach(function(n) { if (n.home === 1) n.home = 0; });
          }
          if (colEpoch === 600) {
            // Core 0 splits: half go to a revived core 1 at a new position
            cores[1].alive = true;
            cores[1].x = cores[0].x + 30;
            cores[1].y = cores[0].y - 25;
            cores[1].vx = 0.4; cores[1].vy = -0.3;
            var home0 = colNodes.filter(function(n) { return n.home === 0; });
            for (var i = 0; i < Math.floor(home0.length / 2); i++) home0[i].home = 1;
          }
          if (colEpoch === 900) {
            // Core 3 absorbs core 4
            cores[4].alive = false;
            colNodes.forEach(function(n) { if (n.home === 4) n.home = 3; });
          }
          if (colEpoch === 1200) {
            // Core 4 revives as splinter from core 2
            cores[4].alive = true;
            cores[4].x = cores[2].x - 20;
            cores[4].y = cores[2].y + 20;
            cores[4].vx = -0.3; cores[4].vy = 0.2;
            var home2 = colNodes.filter(function(n) { return n.home === 2; });
            for (var i = 0; i < Math.floor(home2.length * 0.4); i++) home2[i].home = 4;
          }

          // Move cores (slow drift + boundary bounce)
          var bx1 = ox + 15, bx2 = ox + cellW - 15;
          var by1 = areaTop + 10, by2 = areaTop + areaH - 10;
          cores.forEach(function(c) {
            if (!c.alive) return;
            c.vx += (Math.random() - 0.5) * 0.03;
            c.vy += (Math.random() - 0.5) * 0.03;
            c.vx *= 0.98; c.vy *= 0.98;
            c.x += c.vx; c.y += c.vy;
            if (c.x < bx1 || c.x > bx2) c.vx *= -1;
            if (c.y < by1 || c.y > by2) c.vy *= -1;
            c.x = Math.max(bx1, Math.min(bx2, c.x));
            c.y = Math.max(by1, Math.min(by2, c.y));
          });

          // Move followers toward their core + local repulsion
          colNodes.forEach(function(n) {
            var c = cores[n.home];
            if (!c || !c.alive) {
              // Find nearest alive core
              var best = -1, bestD = Infinity;
              cores.forEach(function(cc, ci) {
                if (!cc.alive) return;
                var d = Math.sqrt((cc.x-n.x)*(cc.x-n.x)+(cc.y-n.y)*(cc.y-n.y));
                if (d < bestD) { bestD = d; best = ci; }
              });
              if (best >= 0) n.home = best;
              c = cores[n.home];
            }
            var dx = c.x - n.x, dy = c.y - n.y;
            var d = Math.sqrt(dx*dx + dy*dy) + 1;
            n.vx += dx / d * 0.04;
            n.vy += dy / d * 0.04;

            // Repel from nearby followers
            colNodes.forEach(function(m) {
              if (m === n) return;
              var rx = n.x - m.x, ry = n.y - m.y;
              var rd = Math.sqrt(rx*rx + ry*ry);
              if (rd < 12 && rd > 0) { n.vx += rx / rd * 0.2; n.vy += ry / rd * 0.2; }
            });

            n.vx += (Math.random() - 0.5) * 0.15;
            n.vy += (Math.random() - 0.5) * 0.15;
            n.vx *= 0.92; n.vy *= 0.92;
            n.x += n.vx; n.y += n.vy;
            if (n.x < bx1) n.x = bx2; if (n.x > bx2) n.x = bx1;
            if (n.y < by1) n.y = by2; if (n.y > by2) n.y = by1;

            // Trail
            trailG.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 1)
              .attr('fill', c.color).attr('opacity', 0.1)
              .transition().duration(3000).attr('opacity', 0).remove();
          });

          // Update core visuals
          cores.forEach(function(c, i) {
            coreEls[i].attr('cx', c.x).attr('cy', c.y)
              .attr('opacity', c.alive ? 0.8 : 0).attr('r', c.alive ? 5 : 0);
          });

          // Update follower visuals
          followerEls
            .attr('cx', function(d) { return d.x; })
            .attr('cy', function(d) { return d.y; })
            .attr('fill', function(d) { return cores[d.home].color; });

          // Ephemeral links (same home, nearby)
          colNodeG.selectAll('line').remove();
          for (var i = 0; i < nFollowers; i++) for (var j = i + 1; j < nFollowers; j++) {
            if (colNodes[i].home !== colNodes[j].home) continue;
            var dx = colNodes[i].x - colNodes[j].x, dy = colNodes[i].y - colNodes[j].y;
            if (Math.sqrt(dx*dx + dy*dy) < 22) {
              colNodeG.append('line')
                .attr('x1', colNodes[i].x).attr('y1', colNodes[i].y)
                .attr('x2', colNodes[j].x).attr('y2', colNodes[j].y)
                .attr('stroke', cores[colNodes[i].home].color)
                .attr('stroke-width', 0.5).attr('opacity', 0.15);
            }
          }

          colFrame = requestAnimationFrame(colonyTick);
        }
        colonyTick();
        // Store cleanup
        animTimers.push({clear: function() { cancelAnimationFrame(colFrame); }});
      }

      // ---- KB ICON for each panel ----
      var kbX = ox + cellW - 22, kbY = oy + cellH - 18;
      // Guild gets 3 mini KBs, Colony gets 3 scattered ones
      if (idx === 4) {
        // Guild: 3 KBs near each cluster
        [[ox + cellW * 0.2, acy - R * 0.4], [ox + cellW * 0.8, acy - R * 0.4], [ox + cellW * 0.5, acy + R * 0.5]].forEach(function(pos) {
          g.append('rect').attr('x', pos[0] - 4).attr('y', pos[1] - 5).attr('width', 8).attr('height', 10)
            .attr('rx', 2).attr('fill', '#f0fdf4').attr('stroke', arch.color).attr('stroke-width', 0.8);
          g.append('line').attr('x1', pos[0] - 3).attr('y1', pos[1]).attr('x2', pos[0] + 3).attr('y2', pos[1])
            .attr('stroke', arch.color).attr('stroke-width', 0.5);
        });
      } else if (idx === 5) {
        // Colony: 3 scattered mini KBs
        [[ox + 20, areaTop + 20], [ox + cellW - 20, areaTop + areaH * 0.4], [ox + cellW * 0.4, areaTop + areaH - 15]].forEach(function(pos) {
          g.append('rect').attr('x', pos[0] - 3).attr('y', pos[1] - 4).attr('width', 6).attr('height', 8)
            .attr('rx', 2).attr('fill', '#faf5ff').attr('stroke', arch.color).attr('stroke-width', 0.6);
          g.append('line').attr('x1', pos[0] - 2).attr('y1', pos[1]).attr('x2', pos[0] + 2).attr('y2', pos[1])
            .attr('stroke', arch.color).attr('stroke-width', 0.4);
        });
      } else {
        // Single KB icon
        g.append('rect').attr('x', kbX - 5).attr('y', kbY - 6).attr('width', 10).attr('height', 12)
          .attr('rx', 3).attr('fill', '#f8fafc').attr('stroke', arch.color).attr('stroke-width', 0.8);
        g.append('line').attr('x1', kbX - 4).attr('y1', kbY).attr('x2', kbX + 4).attr('y2', kbY)
          .attr('stroke', arch.color).attr('stroke-width', 0.5);
        g.append('text').attr('x', kbX).attr('y', kbY + 15)
          .attr('text-anchor', 'middle').attr('font-size', '5px').attr('fill', '#999').text('KB');
      }
    });
  }

  init();
  svg.on('click', function() {
    animTimers.forEach(function(t) {
      if (t.clear) t.clear(); else clearInterval(t);
    });
    animTimers = [];
    init();
  });
})();

// ============================================================
// Visualization 7: Trust Economy (6 scenarios, 2x3 grid, 4x4 nodes)
// ============================================================
(function() {
  var container = document.getElementById('viz-trust');
  if (!container) return;
  var width = 720, height = 820;
  var cols = 2, rows = 3;
  var cellW = width / cols, cellH = height / rows;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  var cScale = d3.scaleLinear().domain([0, 0.5, 1]).range(['#991b1b', '#3b82f6', '#4ade80']);
  var maxIter = 50;
  var timers = [];

  var scenarios = [
    { name: 'Reputation Market', sub: 'High-trust nodes attract work, low-trust ones starve' },
    { name: 'The Auditor', sub: 'Inspector flags and removes offenders' },
    { name: 'Proof of Work', sub: 'Trust earned only through verified tasks' },
    { name: 'Vouching', sub: 'Stake your reputation for others' },
    { name: 'Decay & Renewal', sub: 'Trust decays unless actively maintained' },
    { name: 'Contagion', sub: 'A bad actor spreads distrust' }
  ];

  // 4x4 grid = 16 nodes
  var gCols = 4, gRows = 4, N = 16;
  var gridSpacing = 36;

  // Fixed starting trust: 2 high, 2 mid, 12 low
  // High: positions (1,1) and (2,2) = indices 5, 10
  // Mid: positions (0,3) and (3,0) = indices 3, 12
  var highIdx = [5, 10], midIdx = [3, 12];
  function initTrust() {
    var t = [];
    for (var i = 0; i < N; i++) {
      if (highIdx.indexOf(i) >= 0) t.push(0.90);
      else if (midIdx.indexOf(i) >= 0) t.push(0.50);
      else t.push(0.10);
    }
    return t;
  }

  function makePositions(cx, cy) {
    var pos = [];
    var offX = cx - (gCols - 1) * gridSpacing / 2;
    var offY = cy - (gRows - 1) * gridSpacing / 2;
    for (var r = 0; r < gRows; r++) {
      for (var c = 0; c < gCols; c++) {
        pos.push({ x: offX + c * gridSpacing, y: offY + r * gridSpacing });
      }
    }
    return pos;
  }

  // Grid edges: horizontal + vertical adjacency
  function makeEdges() {
    var edges = [];
    for (var r = 0; r < gRows; r++) {
      for (var c = 0; c < gCols; c++) {
        var i = r * gCols + c;
        if (c < gCols - 1) edges.push({ source: i, target: i + 1 });
        if (r < gRows - 1) edges.push({ source: i, target: i + gCols });
      }
    }
    return edges;
  }

  function neighbors(idx, edges) {
    var nb = [];
    edges.forEach(function(e) {
      if (e.source === idx) nb.push(e.target);
      if (e.target === idx) nb.push(e.source);
    });
    return nb;
  }

  function clearTimers() { timers.forEach(function(t) { clearTimeout(t); }); timers = []; }

  function init() {
    clearTimers();
    svg.selectAll('*').remove();

    scenarios.forEach(function(sc, sIdx) {
      var col = sIdx % cols, row = Math.floor(sIdx / cols);
      var ox = col * cellW, oy = row * cellH;
      var cx = ox + cellW / 2, cy = oy + cellH / 2 + 16;

      var cellG = svg.append('g');

      // Cell border
      cellG.append('rect').attr('x', ox + 1).attr('y', oy + 1)
        .attr('width', cellW - 2).attr('height', cellH - 2)
        .attr('fill', 'none').attr('stroke', '#eee').attr('stroke-width', 0.5).attr('rx', 4);

      // Title and subtitle
      cellG.append('text').attr('x', ox + 8).attr('y', oy + 18)
        .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#555')
        .text((sIdx + 1) + '. ' + sc.name);
      cellG.append('text').attr('x', ox + 8).attr('y', oy + 30)
        .attr('font-size', '8px').attr('fill', '#aaa').text(sc.sub);

      // Round label
      var roundLabel = cellG.append('text').attr('x', ox + cellW - 6).attr('y', oy + 18)
        .attr('text-anchor', 'end').attr('font-size', '9px').attr('fill', '#ccc');

      var linkLayer = cellG.append('g');
      var nodeLayer = cellG.append('g');

      var pos = makePositions(cx, cy);
      var edges = makeEdges();
      var trust = initTrust();
      var alive = []; for (var i = 0; i < N; i++) alive.push(true);
      var flagged = {};

      // Contagion: node 7 (top-right area) is the bad actor
      if (sIdx === 5) trust[7] = 0.02;

      // Draw edges
      var eLines = linkLayer.selectAll('line').data(edges).enter().append('line')
        .attr('x1', function(d) { return pos[d.source].x; }).attr('y1', function(d) { return pos[d.source].y; })
        .attr('x2', function(d) { return pos[d.target].x; }).attr('y2', function(d) { return pos[d.target].y; })
        .attr('stroke', '#e5e5e5').attr('stroke-width', 0.8);

      // Draw nodes
      var nCircles = nodeLayer.selectAll('circle').data(pos).enter().append('circle')
        .attr('cx', function(d) { return d.x; }).attr('cy', function(d) { return d.y; })
        .attr('r', 7)
        .attr('fill', function(d, i) { return cScale(trust[i]); })
        .attr('stroke', 'white').attr('stroke-width', 1.5);

      // Auditor markers (3 parallel auditors)
      var auditorCircles = [];
      if (sIdx === 1) {
        for (var ai = 0; ai < 3; ai++) {
          auditorCircles.push(nodeLayer.append('circle').attr('r', 12).attr('fill', 'none')
            .attr('stroke', '#f59e0b').attr('stroke-width', 1.5).attr('stroke-dasharray', '3,2').attr('opacity', 0));
        }
      }

      // Bad actor label
      if (sIdx === 5) {
        nodeLayer.append('text').attr('x', pos[7].x).attr('y', pos[7].y - 12)
          .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#991b1b').text('bad actor');
      }

      function updateVisuals() {
        nCircles.transition().duration(500)
          .attr('fill', function(d, i) { return alive[i] ? cScale(trust[i]) : '#eee'; })
          .attr('r', function(d, i) { return alive[i] ? 7 : 2; })
          .attr('opacity', function(d, i) { return alive[i] ? 1 : 0.3; });

        eLines.transition().duration(500)
          .attr('stroke', function(d) {
            if (!alive[d.source] || !alive[d.target]) return '#f0f0f0';
            return cScale((trust[d.source] + trust[d.target]) / 2);
          })
          .attr('opacity', function(d) { return (!alive[d.source] || !alive[d.target]) ? 0.15 : 0.45; });
      }

      roundLabel.text('0/' + maxIter);
      updateVisuals();

      function step(iter) {
        if (sIdx === 0) {
          // REPUTATION MARKET
          var probs = trust.map(function(t, i) { return alive[i] ? Math.max(0.05, t) : 0; });
          var sumP = probs.reduce(function(a, b) { return a + b; }, 0);
          for (var c = 0; c < 4; c++) {
            var r = Math.random() * sumP, acc = 0, pick = 0;
            for (var k = 0; k < N; k++) { acc += probs[k]; if (acc >= r) { pick = k; break; } }
            if (Math.random() < 0.55 + trust[pick] * 0.35) {
              trust[pick] = Math.min(1, trust[pick] + 0.06);
            } else {
              trust[pick] = Math.max(0, trust[pick] - 0.07);
            }
          }
          for (var i = 0; i < N; i++) if (alive[i]) trust[i] = Math.max(0, trust[i] - 0.01);
          for (var i = 0; i < N; i++) if (trust[i] < 0.03 && alive[i] && iter > 6) alive[i] = false;
        }

        else if (sIdx === 1) {
          // THE AUDITOR
          var nt = trust.slice();
          edges.forEach(function(e) {
            if (!alive[e.source] || !alive[e.target]) return;
            var avg = (trust[e.source] + trust[e.target]) / 2;
            nt[e.source] += (avg - nt[e.source]) * 0.12;
            nt[e.target] += (avg - nt[e.target]) * 0.12;
          });
          for (var i = 0; i < N; i++) if (alive[i]) trust[i] = Math.max(0, Math.min(1, nt[i]));
          // 3 auditors inspect in parallel
          for (var a = 0; a < 3; a++) {
            var pick = Math.floor(Math.random() * N);
            if (!alive[pick]) continue;
            if (auditorCircles[a]) {
              auditorCircles[a].transition().duration(300)
                .attr('cx', pos[pick].x).attr('cy', pos[pick].y).attr('opacity', 0.7)
                .transition().delay(300).duration(300).attr('opacity', 0);
            }
            if (trust[pick] < 0.2) {
              if (flagged[pick]) alive[pick] = false;
              else flagged[pick] = true;
            }
          }
        }

        else if (sIdx === 2) {
          // PROOF OF WORK
          var avail = [];
          for (var i = 0; i < N; i++) if (alive[i]) avail.push(i);
          for (var i = avail.length - 1; i > 0; i--) {
            var j = Math.floor(Math.random() * (i + 1));
            var tmp = avail[i]; avail[i] = avail[j]; avail[j] = tmp;
          }
          for (var i = 0; i + 1 < avail.length; i += 2) {
            var a = avail[i], b = avail[i + 1];
            if (Math.random() < 0.4 + (trust[a] + trust[b]) * 0.15) {
              trust[a] = Math.min(1, trust[a] + 0.05);
              trust[b] = Math.min(1, trust[b] + 0.05);
            } else {
              trust[a] = Math.max(0, trust[a] - 0.02);
              trust[b] = Math.max(0, trust[b] - 0.02);
            }
          }
        }

        else if (sIdx === 3) {
          // VOUCHING
          var avail = [];
          for (var i = 0; i < N; i++) if (alive[i]) avail.push(i);
          for (var v = 0; v < 3; v++) {
            var staker = avail[Math.floor(Math.random() * avail.length)];
            var nbs = neighbors(staker, edges).filter(function(n) { return alive[n]; });
            if (nbs.length === 0) continue;
            var vouchee = nbs[Math.floor(Math.random() * nbs.length)];
            var stake = trust[staker] * 0.15;
            if (Math.random() < 0.3 + trust[vouchee] * 0.5) {
              trust[staker] = Math.min(1, trust[staker] + stake * 0.5);
              trust[vouchee] = Math.min(1, trust[vouchee] + 0.07);
            } else {
              trust[staker] = Math.max(0, trust[staker] - stake);
              trust[vouchee] = Math.max(0, trust[vouchee] - 0.04);
            }
          }
        }

        else if (sIdx === 4) {
          // DECAY & RENEWAL
          for (var i = 0; i < N; i++) if (alive[i]) trust[i] = Math.max(0, trust[i] - 0.03);
          var aliveEdges = edges.filter(function(e) { return alive[e.source] && alive[e.target]; });
          for (var c = 0; c < 3 && aliveEdges.length > 0; c++) {
            var e = aliveEdges[Math.floor(Math.random() * aliveEdges.length)];
            trust[e.source] = Math.min(1, trust[e.source] + 0.06);
            trust[e.target] = Math.min(1, trust[e.target] + 0.06);
          }
          if (iter > 10) { for (var i = 0; i < N; i++) if (trust[i] < 0.05 && alive[i]) alive[i] = false; }
        }

        else if (sIdx === 5) {
          // CONTAGION
          var nt = trust.slice();
          edges.forEach(function(e) {
            if (!alive[e.source] || !alive[e.target]) return;
            var s = e.source, t = e.target;
            if (trust[s] < 0.15 || trust[t] < 0.15) {
              var low = trust[s] < trust[t] ? s : t;
              var high = low === s ? t : s;
              nt[high] -= 0.06;
              nt[low] += 0.01;
            } else {
              var avg = (trust[s] + trust[t]) / 2;
              nt[s] += (avg - nt[s]) * 0.10;
              nt[t] += (avg - nt[t]) * 0.10;
            }
          });
          for (var i = 0; i < N; i++) trust[i] = Math.max(0, Math.min(1, nt[i]));
          trust[7] = Math.min(0.05, trust[7]);
        }

        for (var i = 0; i < N; i++) trust[i] = Math.max(0, Math.min(1, trust[i]));
        updateVisuals();
      }

      var iter = 0;
      function tick() {
        if (iter >= maxIter) return;
        iter++;
        roundLabel.text(iter + '/' + maxIter);
        step(iter);
        timers.push(setTimeout(tick, 900));
      }
      timers.push(setTimeout(tick, 1200));
    });
  }

  init();
  svg.on('click', init);
})();

// ============================================================
// Visualization 8: Acceleration Loop (dual view: rings + timeline)
// ============================================================
(function() {
  var container = document.getElementById('viz-acceleration');
  if (!container) return;
  var width = 720, height = 380;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  var generations = 8;
  var genColors = d3.scaleSequential(d3.interpolateReds).domain([-1, generations + 1]);

  function draw() {
    svg.selectAll('*').remove();

    // Left side: concentric rings (generations building on each other)
    var cx = 180, cy = height / 2;
    var maxR = 140;

    // Right side: timeline bars showing decreasing dev time
    var barX = 380, barW = 300;
    var barH = (height - 80) / generations;

    // Labels
    svg.append('text').attr('x', cx).attr('y', 22).attr('text-anchor', 'middle')
      .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#666').text('Generations build on each other');
    svg.append('text').attr('x', barX + barW / 2).attr('y', 22).attr('text-anchor', 'middle')
      .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', '#666').text('Development time shrinks');

    for (var i = 0; i < generations; i++) {
      var gen = i + 1;
      var devTime = Math.pow(0.55, i) * 100;
      var r = maxR - (i * maxR / generations);
      var delay = 0;
      // Each gen appears faster than the last: delay = sum of all previous devTimes
      for (var j = 0; j < i; j++) delay += Math.pow(0.55, j) * 300;
      var color = genColors(i);

      // Ring
      (function(gen, r, color, delay) {
        svg.append('circle').attr('cx', cx).attr('cy', cy).attr('r', 0)
          .attr('fill', 'none').attr('stroke', color).attr('stroke-width', 2.5).attr('opacity', 0.7)
          .transition().delay(delay).duration(400).ease(d3.easeBackOut)
          .attr('r', r);
        svg.append('text').attr('x', cx + r + 4).attr('y', cy + 4)
          .attr('font-size', '8px').attr('fill', color).attr('opacity', 0)
          .text('G' + gen)
          .transition().delay(delay + 200).duration(200).attr('opacity', 0.7);
      })(gen, r, color, delay);

      // Timeline bar
      (function(gen, devTime, color, delay, idx) {
        var by = 40 + idx * barH;
        var bw = (devTime / 100) * barW;

        svg.append('text').attr('x', barX - 8).attr('y', by + barH / 2 + 4).attr('text-anchor', 'end')
          .attr('font-size', '10px').attr('fill', '#888').text('Gen ' + gen);

        svg.append('rect').attr('x', barX).attr('y', by + 4).attr('width', 0)
          .attr('height', barH - 8).attr('rx', 3)
          .attr('fill', color).attr('opacity', 0.6)
          .transition().delay(delay).duration(350)
          .attr('width', bw);

        // Time label
        svg.append('text').attr('x', barX + bw + 6).attr('y', by + barH / 2 + 4)
          .attr('font-size', '9px').attr('fill', color).attr('opacity', 0)
          .text(Math.round(devTime) + '%')
          .transition().delay(delay + 200).duration(200).attr('opacity', 0.8);
      })(gen, devTime, color, delay, i);
    }

    // Annotation
    var totalDelay = 0;
    for (var j = 0; j < generations; j++) totalDelay += Math.pow(0.55, j) * 300;
    svg.append('text').attr('x', barX + barW / 2).attr('y', height - 12).attr('text-anchor', 'middle')
      .attr('font-size', '9px').attr('fill', '#999').attr('font-style', 'italic').attr('opacity', 0)
      .text('Notice the animation itself accelerates. Each ring appears faster than the last.')
      .transition().delay(totalDelay).duration(400).attr('opacity', 0.8);
  }

  draw();
  svg.on('click', draw);
})();

// ============================================================
// Visualization 9: Goal Drift (dual paths: harmful + serendipitous)
// ============================================================
(function() {
  var container = document.getElementById('viz-goaldrift');
  if (!container) return;
  var width = 720, height = 380;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  function draw() {
    svg.selectAll('*').remove();
    var ox = 100, oy = height / 2;

    // Goal zone (left)
    svg.append('circle').attr('cx', ox).attr('cy', oy).attr('r', 30)
      .attr('fill', '#2563eb').attr('opacity', 0.08).attr('stroke', '#2563eb').attr('stroke-width', 1.5);
    svg.append('text').attr('x', ox).attr('y', oy + 4).attr('text-anchor', 'middle')
      .attr('font-size', '9px').attr('fill', '#2563eb').attr('font-weight', 'bold').text('Goal');

    // Misaligned zone (upper right)
    svg.append('rect').attr('x', 460).attr('y', 15).attr('width', 240).attr('height', height / 2 - 30)
      .attr('fill', '#dc2626').attr('opacity', 0.04).attr('rx', 8);
    svg.append('text').attr('x', 580).attr('y', 35).attr('text-anchor', 'middle')
      .attr('font-size', '9px').attr('fill', '#dc2626').attr('opacity', 0.5).text('harmful drift');

    // Discovery zone (lower right)
    svg.append('rect').attr('x', 460).attr('y', height / 2 + 15).attr('width', 240).attr('height', height / 2 - 30)
      .attr('fill', '#059669').attr('opacity', 0.04).attr('rx', 8);
    svg.append('text').attr('x', 580).attr('y', height - 20).attr('text-anchor', 'middle')
      .attr('font-size', '9px').attr('fill', '#059669').attr('opacity', 0.5).text('serendipitous drift');

    // Guided wavy walk: random but steered toward a target, guaranteed to arrive
    // targetX/Y = center of target zone, steps = total steps
    function guidedWalk(startX, startY, targetX, targetY, steps, yMin, yMax) {
      var path = [{x: startX, y: startY}];
      var angle = Math.atan2(targetY - startY, targetX - startX);
      for (var i = 0; i < steps; i++) {
        var prev = path[path.length - 1];
        var t = (i + 1) / steps; // 0..1 progress
        // Ideal position at this fraction
        var idealX = startX + (targetX - startX) * t;
        var idealY = startY + (targetY - startY) * t;
        // Early: more random wander. Late: converge to target.
        var wander = (1 - t) * 0.6;
        angle += (Math.random() - 0.5) * wander * 2;
        // Steer toward ideal position (stronger as t increases)
        var steer = Math.atan2(idealY - prev.y, idealX - prev.x);
        angle = angle * (1 - t * 0.6) + steer * t * 0.6;
        var sL = 4 + Math.random() * 5;
        var nx = prev.x + Math.cos(angle) * sL;
        var ny = prev.y + Math.sin(angle) * sL;
        ny = Math.max(yMin, Math.min(yMax, ny));
        nx = Math.max(50, Math.min(width - 15, nx));
        path.push({x: nx, y: ny});
      }
      return path;
    }

    // Target centers: red zone = (580, 110), green zone = (580, 280)
    var stepsA = 70;
    var pathA = guidedWalk(ox, oy - 8, 580, 100, stepsA, 25, height / 2 - 5);

    var stepsB = 70;
    var pathB = guidedWalk(ox, oy + 8, 580, height - 100, stepsB, height / 2 + 5, height - 25);

    // Animate both paths
    var colorA = d3.scaleLinear().domain([0, stepsA]).range(['#2563eb', '#dc2626']);
    var colorB = d3.scaleLinear().domain([0, stepsB]).range(['#2563eb', '#059669']);

    function animatePath(path, colorFn, steps) {
      for (var i = 1; i < path.length; i++) {
        var delay = i * 70;
        svg.append('line')
          .attr('x1', path[i-1].x).attr('y1', path[i-1].y)
          .attr('x2', path[i-1].x).attr('y2', path[i-1].y)
          .attr('stroke', colorFn(i)).attr('stroke-width', 2).attr('opacity', 0)
          .transition().delay(delay).duration(130)
          .attr('x2', path[i].x).attr('y2', path[i].y).attr('opacity', 0.6);

        svg.append('circle')
          .attr('cx', path[i].x).attr('cy', path[i].y).attr('r', 0)
          .attr('fill', colorFn(i)).attr('opacity', 0)
          .transition().delay(delay + 40).duration(120)
          .attr('r', 1.8).attr('opacity', 0.45);
      }
    }

    animatePath(pathA, colorA, stepsA);
    animatePath(pathB, colorB, stepsB);

    // Labels
    svg.append('text').attr('x', width / 2).attr('y', height / 2 + 4).attr('text-anchor', 'middle')
      .attr('font-size', '9px').attr('fill', '#888').attr('font-style', 'italic')
      .text('Same mechanism, different outcomes. The challenge: telling them apart in real time.');
  }

  draw();
  svg.on('click', draw);
})();

// ============================================================
// Visualization: Adaptive System (shape-shifting phases)
// Phase 1: Triangle (data/model/interface)
// Phase 2: Diamond (+ environment)
// Phase 3: Pentagon (+ coordination)
// Phase 4: Fluid blob (all boundaries dissolve)
// ============================================================
(function() {
  var container = document.getElementById('viz-adaptive');
  if (!container) return;
  var width = 720, height = 520;
  d3.select(container).style('overflow', 'hidden').style('height', height + 'px').style('max-height', height + 'px');
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .attr('preserveAspectRatio', 'xMidYMid meet')
    .style('width', '100%').style('height', height + 'px')
    .style('display', 'block')
    .style('cursor', 'pointer');
  var rafId = null;

  function init() {
    svg.selectAll('*').remove();
    if (rafId) cancelAnimationFrame(rafId);

    var cx = width / 2, cy = height / 2 - 5;
    var R = 130; // max radius for shapes
    var nPts = 12; // control points per blob

    var domains = [
      {label: 'Data', color: '#6366f1'},
      {label: 'Model', color: '#e11d48'},
      {label: 'Interface', color: '#059669'},
      {label: 'Environment', color: '#0891b2'},
      {label: 'Coordination', color: '#d97706'}
    ];

    // Phases: which domains are active, what shape, annotation
    var phases = [
      { n: 5, ids: [0,1,2,3,4], shape: 'fluid', duration: 6,
        anno: 'Fluid: boundaries dissolve - the system adapts as one' },
      { n: 3, ids: [0,1,2], shape: 'triangle', duration: 3,
        anno: 'Triangle: the classic ML stack - data, model, interface' },
      { n: 5, ids: [0,1,2,3,4], shape: 'fluid', duration: 5,
        anno: 'Fluid: all five domains breathing together' },
      { n: 4, ids: [0,1,2,3], shape: 'diamond', duration: 3,
        anno: 'Diamond: environment joins - tools, sandboxes, APIs adapt' },
      { n: 5, ids: [0,1,2,3,4], shape: 'fluid', duration: 5,
        anno: 'Fluid: back to the organic whole' },
      { n: 5, ids: [0,1,2,3,4], shape: 'pentagon', duration: 3,
        anno: 'Pentagon: coordination joins - protocols, topologies, trust' },
      { n: 5, ids: [0,1,2,3,4], shape: 'fluid', duration: 5,
        anno: 'Fluid: boundaries dissolve again' }
    ];

    var transitionDuration = 1.5; // seconds for morph

    // Defs for glow
    var defs = svg.append('defs');
    var filter = defs.append('filter').attr('id', 'glow-adapt');
    filter.append('feGaussianBlur').attr('stdDeviation', '3').attr('result', 'blur');
    var mg = filter.append('feMerge');
    mg.append('feMergeNode').attr('in', 'blur');
    mg.append('feMergeNode').attr('in', 'SourceGraphic');

    // Catmull-Rom spline path
    function blobPath(pts) {
      var n = pts.length;
      var d = 'M ' + pts[0].x + ' ' + pts[0].y;
      for (var i = 0; i < n; i++) {
        var p0 = pts[(i - 1 + n) % n], p1 = pts[i];
        var p2 = pts[(i + 1) % n], p3 = pts[(i + 2) % n];
        d += ' C ' + (p1.x + (p2.x - p0.x) / 6) + ' ' + (p1.y + (p2.y - p0.y) / 6) + ', '
           + (p2.x - (p3.x - p1.x) / 6) + ' ' + (p2.y - (p3.y - p1.y) / 6) + ', '
           + p2.x + ' ' + p2.y;
      }
      return d + ' Z';
    }

    // Generate target points for a regular polygon with n sides
    function polyTargets(n, r, offsetAngle) {
      var pts = [];
      for (var i = 0; i < nPts; i++) {
        // Map nPts control points onto an n-sided polygon
        var frac = i / nPts;
        var side = Math.floor(frac * n);
        var along = (frac * n) - side;
        var a1 = offsetAngle + (side / n) * Math.PI * 2;
        var a2 = offsetAngle + ((side + 1) / n) * Math.PI * 2;
        pts.push({
          x: cx + ((1 - along) * Math.cos(a1) + along * Math.cos(a2)) * r,
          y: cy + ((1 - along) * Math.sin(a1) + along * Math.sin(a2)) * r * 0.8
        });
      }
      return pts;
    }

    // Generate target points for fluid phase (circle with wobble seeds)
    function fluidTargets(r) {
      var pts = [];
      for (var i = 0; i < nPts; i++) {
        var a = (i / nPts) * Math.PI * 2;
        pts.push({ x: cx + Math.cos(a) * r, y: cy + Math.sin(a) * r * 0.8 });
      }
      return pts;
    }

    // Per-domain wobble seeds (for fluid phase)
    var wobbleFreqs = [], wobbleAmps = [];
    for (var i = 0; i < 5; i++) {
      var f = [], a = [];
      for (var j = 0; j < nPts; j++) {
        f.push(0.4 + Math.random() * 0.5);
        a.push(6 + Math.random() * 14);
      }
      wobbleFreqs.push(f);
      wobbleAmps.push(a);
    }

    // Create path elements and label elements for each domain
    domains.forEach(function(dom, di) {
      dom.pts = [];
      for (var i = 0; i < nPts; i++) dom.pts.push({x: cx, y: cy});
      dom.el = svg.append('path')
        .attr('fill', dom.color).attr('opacity', 0)
        .attr('stroke', dom.color).attr('stroke-width', 1.2).attr('stroke-opacity', 0)
        .attr('filter', 'url(#glow-adapt)');
      dom.labelEl = svg.append('text')
        .attr('text-anchor', 'middle').attr('font-size', '10px')
        .attr('font-weight', 'bold').attr('fill', dom.color).attr('opacity', 0)
        .text(dom.label);
    });

    var pktG = svg.append('g');

    // Phase label
    var phaseText = svg.append('text').attr('x', width / 2).attr('y', 22)
      .attr('text-anchor', 'middle').attr('font-size', '11px').attr('fill', '#999');

    // Cycle counter
    var cycleText = svg.append('text').attr('x', width - 15).attr('y', 20)
      .attr('text-anchor', 'end').attr('font-size', '10px').attr('fill', '#ccc');

    // Annotation
    var annoG = svg.append('g');
    function showAnno(text) {
      annoG.selectAll('*').remove();
      annoG.append('text').attr('x', width / 2).attr('y', height - 12)
        .attr('text-anchor', 'middle').attr('font-size', '10px').attr('fill', '#777')
        .attr('font-style', 'italic').attr('opacity', 0).text(text)
        .transition().duration(400).attr('opacity', 1)
        .transition().delay(4000).duration(600).attr('opacity', 0);
    }

    var t = 0;
    var lastPhaseIdx = -1;
    var labelR = R + 40;

    function animate() {
      t += 0.016; // ~60fps
      var totalT = 0;
      for (var pi = 0; pi < phases.length; pi++) totalT += phases[pi].duration;
      var loopT = t % totalT;
      var acc = 0, phaseIdx = 0;
      for (var pi = 0; pi < phases.length; pi++) {
        if (loopT < acc + phases[pi].duration) { phaseIdx = pi; break; }
        acc += phases[pi].duration;
      }
      var phaseT = loopT - acc;
      var phase = phases[phaseIdx];

      // Show annotation on phase change
      if (phaseIdx !== lastPhaseIdx) {
        lastPhaseIdx = phaseIdx;
        showAnno(phase.anno);
        phaseText.text(phase.shape.charAt(0).toUpperCase() + phase.shape.slice(1));
      }

      cycleText.text('cycle ' + Math.floor(t * 2));

      // Morph fraction: 0 = start of transition, 1 = fully in target shape
      var morphFrac = Math.min(phaseT / transitionDuration, 1.0);
      // Ease
      morphFrac = morphFrac * morphFrac * (3 - 2 * morphFrac);

      // Fluid wobble intensity (only in fluid phase)
      var fluidWobble = phase.shape === 'fluid' ? morphFrac : 0;

      // For each domain, compute target and interpolate
      domains.forEach(function(dom, di) {
        var active = phase.ids.indexOf(di) >= 0;

        // Target opacity
        var targetOpacity = active ? 0.13 : 0;
        var targetStroke = active ? 0.35 : 0;
        var currentOpacity = parseFloat(dom.el.attr('opacity'));
        var newOpacity = currentOpacity + (targetOpacity - currentOpacity) * 0.05;
        dom.el.attr('opacity', newOpacity).attr('stroke-opacity', active ? targetStroke : newOpacity * 2);

        // Label opacity
        var labelTarget = active ? 0.75 : 0;
        var currentLabelOp = parseFloat(dom.labelEl.attr('opacity'));
        dom.labelEl.attr('opacity', currentLabelOp + (labelTarget - currentLabelOp) * 0.05);

        if (!active) return;

        // Compute target points
        var layerR = R * (0.85 + di * 0.04);
        var offsetAngle = -Math.PI / 2; // point up
        var targets;
        if (phase.shape === 'fluid') {
          targets = fluidTargets(layerR);
        } else {
          targets = polyTargets(phase.n, layerR, offsetAngle);
        }

        // Interpolate current points toward targets
        for (var i = 0; i < nPts; i++) {
          var tx = targets[i].x, ty = targets[i].y;

          // Add wobble in fluid phase
          if (fluidWobble > 0) {
            var wA = (i / nPts) * Math.PI * 2;
            tx += Math.sin(t * wobbleFreqs[di][i] * 1.8 + i * 1.3 + di) * wobbleAmps[di][i] * fluidWobble;
            ty += Math.cos(t * wobbleFreqs[di][i] * 1.5 + i * 0.9 + di * 2) * wobbleAmps[di][i] * 0.6 * fluidWobble;
          }

          // Clamp to viewBox
          tx = Math.max(45, Math.min(width - 45, tx));
          ty = Math.max(35, Math.min(height - 45, ty));

          // Smooth interpolation
          var speed = 0.04;
          dom.pts[i].x += (tx - dom.pts[i].x) * speed;
          dom.pts[i].y += (ty - dom.pts[i].y) * speed;
        }

        dom.el.attr('d', blobPath(dom.pts));

        // Label at fixed position for this domain
        var domAngle = offsetAngle + (phase.ids.indexOf(di) / phase.n) * Math.PI * 2;
        if (phase.shape === 'fluid') {
          domAngle = offsetAngle + (di / 5) * Math.PI * 2;
        }
        var lx = Math.max(50, Math.min(width - 50, cx + Math.cos(domAngle) * labelR));
        var ly = Math.max(20, Math.min(height - 20, cy + Math.sin(domAngle) * labelR * 0.8));
        var curLx = parseFloat(dom.labelEl.attr('x')) || lx;
        var curLy = parseFloat(dom.labelEl.attr('y')) || ly;
        dom.labelEl.attr('x', curLx + (lx - curLx) * 0.03).attr('y', curLy + (ly - curLy) * 0.03);
      });

      // Particles between active domains
      var activeDs = phase.ids.map(function(id) { return domains[id]; });
      if (Math.random() < 0.03 + phaseIdx * 0.01) {
        var from = activeDs[Math.floor(Math.random() * activeDs.length)];
        var to = activeDs[Math.floor(Math.random() * activeDs.length)];
        if (from !== to) {
          var fi = Math.floor(Math.random() * nPts), ti = Math.floor(Math.random() * nPts);
          pktG.append('circle').attr('cx', from.pts[fi].x).attr('cy', from.pts[fi].y)
            .attr('r', 1.5 + Math.random() * 1.5)
            .attr('fill', from.color).attr('opacity', 0.6)
            .transition().duration(500 + Math.random() * 400)
            .attr('cx', to.pts[ti].x).attr('cy', to.pts[ti].y).attr('fill', to.color)
            .transition().duration(250).attr('opacity', 0).remove();
        }
      }

      // Faint internal connections (more in later phases)
      if (Math.random() < 0.008 + phaseIdx * 0.005) {
        var d1 = activeDs[Math.floor(Math.random() * activeDs.length)];
        var d2 = activeDs[Math.floor(Math.random() * activeDs.length)];
        if (d1 !== d2) {
          var i1 = Math.floor(Math.random() * nPts), i2 = Math.floor(Math.random() * nPts);
          pktG.append('line')
            .attr('x1', d1.pts[i1].x).attr('y1', d1.pts[i1].y)
            .attr('x2', d2.pts[i2].x).attr('y2', d2.pts[i2].y)
            .attr('stroke', d1.color).attr('stroke-width', 0.5).attr('opacity', 0)
            .transition().duration(300).attr('opacity', 0.12)
            .transition().delay(300).duration(400).attr('opacity', 0).remove();
        }
      }

      rafId = requestAnimationFrame(animate);
    }
    rafId = requestAnimationFrame(animate);
  }

  init();
  svg.on('click', function() {
    if (rafId) cancelAnimationFrame(rafId);
    init();
  });
})();

// ============================================================
// Visualization: Agent Web (in Pattern 3) - living internet
// ============================================================
(function() {
  var container = document.getElementById('viz-agentweb');
  if (!container) return;
  var width = 720, height = 480;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  var protocols = [
    {color: '#2563eb', label: 'MCP'},
    {color: '#dc2626', label: 'A2A'},
    {color: '#059669', label: 'REST'},
    {color: '#d97706', label: 'Custom'}
  ];
  var animTimer, nodes, links, linkG, nodeG, packetG, statusLabel;

  function init() {
    svg.selectAll('*').remove();
    if (animTimer) clearTimeout(animTimer);
    nodes = []; links = [];

    linkG = svg.append('g');
    packetG = svg.append('g');
    nodeG = svg.append('g');

    // Legend
    var lg = svg.append('g').attr('transform', 'translate(10, 14)');
    protocols.forEach(function(p, i) {
      lg.append('circle').attr('cx', i * 90 + 6).attr('cy', 0).attr('r', 3.5).attr('fill', p.color);
      lg.append('text').attr('x', i * 90 + 14).attr('y', 4).attr('font-size', '8px').attr('fill', '#888').text(p.label);
    });
    statusLabel = svg.append('text').attr('x', width - 10).attr('y', 14)
      .attr('text-anchor', 'end').attr('font-size', '9px').attr('fill', '#bbb');

    // Timeline bar at top
    var tlY = 24, tlX = 10, tlW = width - 20, maxTick = 120;
    var tlBg = svg.append('rect').attr('x', tlX).attr('y', tlY).attr('width', tlW).attr('height', 3)
      .attr('rx', 1.5).attr('fill', '#f0f0f0');
    var tlFill = svg.append('rect').attr('x', tlX).attr('y', tlY).attr('width', 0).attr('height', 3)
      .attr('rx', 1.5).attr('fill', '#3b82f6').attr('opacity', 0.4);
    // Phase labels on timeline
    var phases = [
      {at: 0, label: 'bootstrap'},
      {at: 0.25, label: 'discovery'},
      {at: 0.5, label: 'stable'},
      {at: 0.8, label: 'churn'}
    ];
    phases.forEach(function(ph) {
      svg.append('text').attr('x', tlX + tlW * ph.at).attr('y', tlY + 14)
        .attr('font-size', '7px').attr('fill', '#ccc').text(ph.label);
    });

    // Annotation callout for early events
    var annoG = svg.append('g');
    function showAnno(text, dur) {
      dur = dur || 2200;
      annoG.selectAll('*').remove();
      annoG.append('text').attr('x', width / 2).attr('y', height - 10)
        .attr('text-anchor', 'middle').attr('font-size', '11px').attr('fill', '#777')
        .attr('font-style', 'italic').text(text)
        .attr('opacity', 0).transition().duration(250).attr('opacity', 1)
        .transition().delay(dur).duration(500).attr('opacity', 0).remove();
    }

    // Seed 6 initial nodes
    for (var i = 0; i < 6; i++) addNode();
    showAnno('6 agents come online, broadcasting their protocols...', 3000);

    // Use slow interval at start, accelerate through phases
    var tick = 0;
    var speed = 600; // start slow for narration
    function scheduleTick() {
      animTimer = setTimeout(function() {
        tick++;
        doTick();
        // Ramp: slow during narration, then 2x fast
        if (tick === 8) speed = 400;
        if (tick === 18) speed = 200;
        if (tick === 30) speed = 130;
        if (tick === 45) speed = 60; // churn: blazing fast
        scheduleTick();
      }, speed);
    }
    var MAX_AGENTS = 100, TARGET_LINKS = 500;
    var done = false;

    function doTick() {
      // Update timeline
      var progress = Math.min(tick / maxTick, 1);
      tlFill.attr('width', tlW * progress);

      // Narrate the first events
      if (tick === 3) showAnno('6 agents discover nearby peers and handshake (green flash)', 3200);
      if (tick === 8) showAnno('Same-protocol agents cluster; cross-protocol bridges form', 2800);
      if (tick === 15) showAnno('Packets begin flowing along established links', 2400);
      if (tick === 30) showAnno('The network grows rapidly - agents flood in', 2000);
      if (tick === 50) showAnno('100 agents, 500+ links: a living internet of agents', 2000);

      if (!done) {
        // Phase 1 (tick 1-20): grow one by one
        if (tick <= 20 && tick % 2 === 0) addNode();
        // Phase 2 (tick 20-35): accelerate
        else if (tick > 20 && tick <= 35 && nodes.length < MAX_AGENTS) {
          var batch = Math.min(3, MAX_AGENTS - nodes.length);
          for (var b = 0; b < batch; b++) addNode(true);
        }
        // Phase 3 (tick 35+): burst growth
        else if (tick > 35 && nodes.length < MAX_AGENTS) {
          var burst = Math.min(5, MAX_AGENTS - nodes.length);
          for (var b = 0; b < burst; b++) addNode(true);
        }

        // Once we have enough agents, add extra links to reach target
        var activeLinks = links.filter(function(l){return l.active;}).length;
        if (nodes.length >= MAX_AGENTS && activeLinks < TARGET_LINKS) {
          var attempts = Math.min(30, TARGET_LINKS - activeLinks);
          for (var a = 0; a < attempts; a++) {
            var src = nodes[Math.floor(Math.random() * nodes.length)];
            if (src.alive) tryConnect(src);
          }
        }

        // Check if we're done
        activeLinks = links.filter(function(l){return l.active;}).length;
        if (nodes.length >= MAX_AGENTS && activeLinks >= TARGET_LINKS) {
          done = true;
          showAnno('Network stabilised - click to restart', 4000);
        }
      }

      // Send packets along active links (always, even after done)
      if (tick > 5) {
        var active = links.filter(function(l) { return l.active; });
        var numPackets = done ? 3 : 1;
        for (var pi = 0; pi < numPackets && active.length > 0; pi++) {
          var l = active[Math.floor(Math.random() * active.length)];
          var reverse = Math.random() > 0.5;
          var src = reverse ? l.target : l.source;
          var dst = reverse ? l.source : l.target;
          packetG.append('circle').attr('cx', src.x).attr('cy', src.y).attr('r', 1.5)
            .attr('fill', src.color).attr('opacity', 0.6)
            .transition().duration(500 + Math.random() * 400)
            .attr('cx', dst.x).attr('cy', dst.y)
            .transition().duration(200).attr('opacity', 0).remove();
        }
      }

      var aliveCount = nodes.filter(function(n){return n.alive;}).length;
      var linkCount = links.filter(function(l){return l.active;}).length;
      statusLabel.text(aliveCount + ' agents \u00B7 ' + linkCount + ' links');

      // Stop scheduling after done + a few more ticks for packets
      if (done && tick > maxTick + 30) return;
    } // end doTick
    scheduleTick();
  }

  function addNode(tiny) {
    var proto = Math.floor(Math.random() * protocols.length);
    var p = protocols[proto];
    // Place near existing cluster of same protocol if possible, else random
    var sameProto = nodes.filter(function(n) { return n.proto === proto && n.alive; });
    var x, y;
    if (sameProto.length > 0 && Math.random() < 0.7) {
      var near = sameProto[Math.floor(Math.random() * sameProto.length)];
      x = near.x + (Math.random() - 0.5) * (tiny ? 60 : 100);
      y = near.y + (Math.random() - 0.5) * (tiny ? 50 : 80);
    } else {
      x = 30 + Math.random() * (width - 60);
      y = 32 + Math.random() * (height - 60);
    }
    x = Math.max(15, Math.min(width - 15, x));
    y = Math.max(30, Math.min(height - 20, y));

    var r = tiny ? (1 + Math.random() * 1.5) : (3 + Math.random() * 4);
    var node = {x: x, y: y, r: r, color: p.color, proto: proto, alive: true, el: null, tiny: !!tiny};
    nodes.push(node);

    // Discovery broadcast ring (skip for tiny nodes to reduce DOM load)
    if (!tiny) {
      nodeG.append('circle').attr('cx', x).attr('cy', y).attr('r', 4)
        .attr('fill', 'none').attr('stroke', p.color).attr('opacity', 0.5).attr('stroke-width', 1.5)
        .transition().duration(800).attr('r', 60).attr('opacity', 0).remove();
    }

    // Node appears
    node.el = nodeG.append('circle').attr('cx', x).attr('cy', y).attr('r', 0)
      .attr('fill', p.color).attr('opacity', tiny ? 0.4 : 0.7)
      .attr('stroke', tiny ? 'none' : 'white').attr('stroke-width', tiny ? 0 : 0.8)
      .transition().duration(tiny ? 100 : 300).attr('r', r);
    // Keep reference to the selection (after transition)
    node.el = nodeG.select(':last-child');

    // Try to connect to nearby alive nodes
    tryConnect(node);
  }

  function tryConnect(node) {
    var maxConn = node.tiny ? 3 : 5;
    var searchR = node.tiny ? 100 : 160;
    var nearby = [];
    nodes.forEach(function(n) {
      if (n === node || !n.alive) return;
      var dx = n.x - node.x, dy = n.y - node.y;
      var d = Math.sqrt(dx*dx + dy*dy);
      if (d < searchR) nearby.push({node: n, d: d});
    });
    nearby.sort(function(a, b) { return a.d - b.d; });

    var connected = 0;
    for (var i = 0; i < nearby.length && connected < maxConn; i++) {
      var target = nearby[i].node;
      // Already linked?
      var exists = links.some(function(l) {
        return l.active && ((l.source === node && l.target === target) || (l.source === target && l.target === node));
      });
      if (exists) continue;

      // Same protocol = high chance, cross-protocol = moderate
      var prob = node.proto === target.proto ? 0.85 : 0.4;
      if (Math.random() > prob) continue;

      var isCross = node.proto !== target.proto;
      var lw = node.tiny ? 0.3 : (isCross ? 0.6 : 0.8);
      var el = linkG.append('line')
        .attr('x1', node.x).attr('y1', node.y)
        .attr('x2', node.x).attr('y2', node.y)
        .attr('stroke', isCross ? '#aaa' : node.color)
        .attr('stroke-width', lw)
        .attr('stroke-dasharray', isCross ? '3,3' : 'none')
        .attr('opacity', 0);

      var fadeOp = node.tiny ? 0.08 : (isCross ? 0.15 : 0.2);
      el.transition().duration(node.tiny ? 150 : 500)
        .attr('x2', target.x).attr('y2', target.y)
        .attr('opacity', fadeOp);

      // Brief green handshake flash (skip for tiny nodes)
      if (!node.tiny) {
        el.transition().delay(500).duration(200)
          .attr('stroke', '#4ade80').attr('opacity', 0.5).attr('stroke-width', 2)
          .transition().duration(400)
          .attr('stroke', isCross ? '#aaa' : node.color)
          .attr('stroke-width', lw)
          .attr('opacity', fadeOp);
      }

      links.push({source: node, target: target, active: true, cross: isCross, el: el});
      connected++;
    }
  }

  init();
  svg.on('click', init);
})();

// ============================================================
// Visualization: Curation – Personalized (in Pattern 2)
// ============================================================
(function() {
  var container = document.getElementById('viz-curation');
  if (!container) return;
  var width = 720, height = 420;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  // People on the right, each with a dedicated curator in the middle
  // Pool of information sources on the left
  var people = [
    { name: 'Alice', color: '#2563eb', y: 85 },
    { name: 'Bob', color: '#7c3aed', y: 215 },
    { name: 'Carol', color: '#059669', y: 345 }
  ];
  var curatorX = 380, personX = 630;
  var poolLeft = 15, poolRight = 210, poolTop = 35, poolBot = height - 15;

  // Source types with icons/labels scattered in pool
  var sourceTypes = ['web', 'email', 'social', 'news', 'docs', 'chat'];

  var rawDots, timers;

  function init() {
    svg.selectAll('*').remove();
    timers = [];

    var bgLayer = svg.append('g');
    var particleLayer = svg.append('g');
    var dotLayer = svg.append('g');
    var nodeLayer = svg.append('g');
    var uiLayer = svg.append('g');

    // Pool background
    bgLayer.append('rect').attr('x', poolLeft).attr('y', poolTop).attr('width', poolRight - poolLeft).attr('height', poolBot - poolTop)
      .attr('rx', 8).attr('fill', '#f5f5f5').attr('stroke', '#e5e5e5').attr('stroke-width', 1);
    bgLayer.append('text').attr('x', (poolLeft + poolRight) / 2).attr('y', poolTop - 8)
      .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#aaa').attr('font-weight', 'bold').text('The World');

    // Faint source type labels scattered in the pool
    var typePositions = [
      { t: 'web', x: 50, y: 80 }, { t: 'email', x: 150, y: 120 },
      { t: 'social', x: 80, y: 200 }, { t: 'news', x: 160, y: 260 },
      { t: 'docs', x: 60, y: 320 }, { t: 'chat', x: 140, y: 370 }
    ];
    typePositions.forEach(function(tp) {
      bgLayer.append('text').attr('x', tp.x).attr('y', tp.y)
        .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#d5d5d5').text(tp.t);
    });

    // Generate dense cloud of dots in the pool
    rawDots = [];
    for (var i = 0; i < 90; i++) {
      rawDots.push({
        id: i,
        x: poolLeft + 12 + Math.random() * (poolRight - poolLeft - 24),
        y: poolTop + 8 + Math.random() * (poolBot - poolTop - 16),
        r: 1.5 + Math.random() * 2.5,
        picked: {}
      });
    }
    rawDots.forEach(function(d) {
      dotLayer.append('circle').attr('cx', d.x).attr('cy', d.y)
        .attr('r', d.r).attr('fill', '#9ca3af').attr('opacity', 0.3)
        .attr('stroke', 'white').attr('stroke-width', 0.3);
    });

    // Draw people (right side) and their curators (middle)
    people.forEach(function(p) {
      // Curator node (in the middle)
      nodeLayer.append('circle').attr('cx', curatorX).attr('cy', p.y).attr('r', 16)
        .attr('fill', p.color).attr('opacity', 0.08).attr('stroke', p.color).attr('stroke-width', 1.5);
      nodeLayer.append('text').attr('x', curatorX).attr('y', p.y + 4).attr('text-anchor', 'middle')
        .attr('font-size', '7px').attr('font-weight', 'bold').attr('fill', p.color).text('curator');
      nodeLayer.append('text').attr('x', curatorX).attr('y', p.y + 28).attr('text-anchor', 'middle')
        .attr('font-size', '7px').attr('fill', '#bbb').text('for ' + p.name);

      // Person's collection bag (right) - open-top rounded rect
      var bagW = 70, bagH = 80;
      var bagX = personX - bagW / 2, bagY = p.y - bagH / 2;
      bgLayer.append('rect').attr('x', bagX).attr('y', bagY).attr('width', bagW).attr('height', bagH)
        .attr('rx', 8).attr('fill', p.color).attr('opacity', 0.04)
        .attr('stroke', p.color).attr('stroke-width', 1.5).attr('stroke-opacity', 0.25);
      // Open top effect - white rect to mask top border
      bgLayer.append('rect').attr('x', bagX + 8).attr('y', bagY - 1).attr('width', bagW - 16).attr('height', 4)
        .attr('fill', 'white');

      nodeLayer.append('text').attr('x', personX).attr('y', p.y + bagH / 2 + 14)
        .attr('text-anchor', 'middle').attr('font-size', '9px').attr('font-weight', 'bold')
        .attr('fill', p.color).text(p.name);

      // Faint connection line: curator -> bag
      bgLayer.append('line')
        .attr('x1', curatorX + 18).attr('y1', p.y)
        .attr('x2', bagX - 4).attr('y2', p.y)
        .attr('stroke', p.color).attr('stroke-width', 0.8).attr('opacity', 0.15);

      p.delivered = 0;
      p.bagX = bagX; p.bagY = bagY; p.bagW = bagW; p.bagH = bagH;
      p.countLabel = uiLayer.append('text').attr('x', personX).attr('y', bagY - 4)
        .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', p.color).text('');
    });

    // Scan layer sits behind particles but above background
    var scanLayer = svg.append('g');

    // Animation: curator scans several items, picks one, delivers to person's bag
    function curatorDeliver(p) {
      var available = rawDots.filter(function(d) { return !d.picked[p.name]; });
      if (available.length === 0) return;

      // Choose candidates to scan (5-8 items) and one winner
      var scanCount = 5 + Math.floor(Math.random() * 4);
      var candidates = [];
      var shuffled = available.slice().sort(function() { return Math.random() - 0.5; });
      for (var i = 0; i < Math.min(scanCount, shuffled.length); i++) {
        candidates.push(shuffled[i]);
      }

      // Pick the winner based on curator style
      var pick;
      if (p.name === 'Alice') {
        candidates.sort(function(a, b) { return b.r - a.r; });
        pick = candidates[0];
      } else if (p.name === 'Bob') {
        pick = candidates[Math.floor(Math.random() * candidates.length)];
      } else {
        pick = candidates[Math.floor(Math.random() * candidates.length)];
      }
      pick.picked[p.name] = true;
      p.delivered++;

      // Phase 1: show scan lines from curator to all candidates - staggered, visible
      candidates.forEach(function(c, ci) {
        scanLayer.append('line')
          .attr('x1', curatorX).attr('y1', p.y)
          .attr('x2', curatorX).attr('y2', p.y)
          .attr('stroke', p.color).attr('stroke-width', 0.6).attr('opacity', 0)
          .transition().delay(ci * 80).duration(300)
          .attr('x2', c.x).attr('y2', c.y).attr('opacity', 0.25)
          .transition().duration(600).attr('opacity', 0).remove();
        // Small dot at the end of each scan line
        scanLayer.append('circle')
          .attr('cx', curatorX).attr('cy', p.y).attr('r', 1.5)
          .attr('fill', p.color).attr('opacity', 0)
          .transition().delay(ci * 80).duration(300)
          .attr('cx', c.x).attr('cy', c.y).attr('opacity', 0.3)
          .transition().duration(600).attr('opacity', 0).remove();
      });

      // Phase 2 (delayed): highlight the chosen item and grab it
      setTimeout(function() {
        // Flash the picked dot
        dotLayer.append('circle').attr('cx', pick.x).attr('cy', pick.y)
          .attr('r', pick.r + 5).attr('fill', p.color).attr('opacity', 0.4)
          .transition().duration(400).attr('r', pick.r).attr('opacity', 0).remove();

        // Bold scan line to the pick
        scanLayer.append('line')
          .attr('x1', curatorX).attr('y1', p.y)
          .attr('x2', pick.x).attr('y2', pick.y)
          .attr('stroke', p.color).attr('stroke-width', 1.2).attr('opacity', 0.4)
          .transition().delay(400).duration(300).attr('opacity', 0).remove();

        // Particle: grab from pool -> curator -> land in person's bag
        var landX = p.bagX + 8 + Math.random() * (p.bagW - 16);
        var landY = p.bagY + 10 + Math.random() * (p.bagH - 20);
        var pickR = pick.r;
        particleLayer.append('circle')
          .attr('cx', pick.x).attr('cy', pick.y)
          .attr('r', pickR).attr('fill', p.color).attr('opacity', 0.7)
          .transition().duration(300)
          .attr('cx', curatorX).attr('cy', p.y)
          .transition().duration(300)
          .attr('cx', landX).attr('cy', landY)
          .attr('r', pickR * 0.8).attr('opacity', 0.55)
          .on('end', function() {
            // Leave permanent dot in the bag
            nodeLayer.append('circle')
              .attr('cx', landX).attr('cy', landY)
              .attr('r', pickR * 0.8).attr('fill', p.color).attr('opacity', 0.5)
              .attr('stroke', 'white').attr('stroke-width', 0.3);
            d3.select(this).remove();
          });

        p.countLabel.text(p.delivered);
      }, 800);
    }

    var step = 0;
    var pickTimer = setInterval(function() {
      step++;
      // Alice's curator: slow, selective (every 3rd step)
      if (step % 3 === 0) curatorDeliver(people[0]);
      // Bob's curator: medium pace (every 2nd step)
      if (step % 2 === 0) curatorDeliver(people[1]);
      // Carol's curator: fast, thorough (every step)
      curatorDeliver(people[2]);

      if (step > 30) clearInterval(pickTimer);
    }, 1400);
    timers.push(pickTimer);

    // Bottom note
    uiLayer.append('text').attr('x', width / 2).attr('y', height - 5).attr('text-anchor', 'middle')
      .attr('font-size', '9px').attr('fill', '#aaa').attr('font-style', 'italic')
      .text('Same information universe, personalized delivery.');
  }

  init();
  svg.on('click', function() {
    timers.forEach(function(t) { clearInterval(t); });
    init();
  });
})();

// ============================================================
// Visualization: Resource Ecology (in Pattern 1)
// ============================================================
(function() {
  var container = document.getElementById('viz-resource');
  if (!container) return;
  var width = 720, height = 420;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  var margin = {top: 30, right: 30, bottom: 45, left: 65};
  var pw = width - margin.left - margin.right;
  var ph = height - margin.top - margin.bottom;

  var bgLayer = svg.append('g');
  var arrowLayer = svg.append('g');
  var dotLayer = svg.append('g');
  var uiLayer = svg.append('g');

  // Log-scale visual mapping: data stays in 0-1 range, but position is log-spaced
  // This compresses the bottom-left and expands the top-right, like real scaling curves
  var logMin = 0.01; // avoid log(0)
  function toLog(v) { return (Math.log(Math.max(logMin, v)) - Math.log(logMin)) / (Math.log(1) - Math.log(logMin)); }

  // Axes
  bgLayer.append('line').attr('x1', margin.left).attr('y1', margin.top + ph)
    .attr('x2', margin.left + pw).attr('y2', margin.top + ph)
    .attr('stroke', '#bbb').attr('stroke-width', 1);
  bgLayer.append('line').attr('x1', margin.left).attr('y1', margin.top)
    .attr('x2', margin.left).attr('y2', margin.top + ph)
    .attr('stroke', '#bbb').attr('stroke-width', 1);
  bgLayer.append('text').attr('x', margin.left + pw / 2).attr('y', height - 5)
    .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#888').text('Quality (log scale)');
  bgLayer.append('text').attr('x', 14).attr('y', margin.top + ph / 2)
    .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#888')
    .attr('transform', 'rotate(-90,14,' + (margin.top + ph / 2) + ')').text('Cost (log scale)');

  // Minimal tick labels: just Low / High
  bgLayer.append('text').attr('x', margin.left).attr('y', margin.top + ph + 16)
    .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#aaa').text('Low');
  bgLayer.append('text').attr('x', margin.left + pw).attr('y', margin.top + ph + 16)
    .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#aaa').text('High');
  bgLayer.append('text').attr('x', margin.left - 8).attr('y', margin.top + ph + 4)
    .attr('text-anchor', 'end').attr('font-size', '8px').attr('fill', '#aaa').text('Low');
  bgLayer.append('text').attr('x', margin.left - 8).attr('y', margin.top + 4)
    .attr('text-anchor', 'end').attr('font-size', '8px').attr('fill', '#aaa').text('High');

  // Faint quadrant labels
  bgLayer.append('text').attr('x', margin.left + pw * 0.78).attr('y', margin.top + ph * 0.12)
    .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#ddd').text('frontier');
  bgLayer.append('text').attr('x', margin.left + pw * 0.22).attr('y', margin.top + ph * 0.12)
    .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#ddd').text('overpriced');
  bgLayer.append('text').attr('x', margin.left + pw * 0.78).attr('y', margin.top + ph * 0.92)
    .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#ddd').text('efficient');
  bgLayer.append('text').attr('x', margin.left + pw * 0.22).attr('y', margin.top + ph * 0.92)
    .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#ddd').text('cheap & limited');

  // Arrow marker for distillation
  svg.append('defs').append('marker').attr('id', 'distill-arrow')
    .attr('viewBox', '0 0 10 10').attr('refX', 10).attr('refY', 5)
    .attr('markerWidth', 6).attr('markerHeight', 6).attr('orient', 'auto')
    .append('path').attr('d', 'M0,0 L10,5 L0,10 Z').attr('fill', '#e57373');

  var phaseLabel = uiLayer.append('text').attr('x', width - 20).attr('y', 20)
    .attr('text-anchor', 'end').attr('font-size', '11px').attr('fill', '#999');

  // Size legend: small vs large models
  uiLayer.append('circle').attr('cx', margin.left + 5).attr('cy', 16).attr('r', 3).attr('fill', '#888').attr('opacity', 0.5);
  uiLayer.append('text').attr('x', margin.left + 12).attr('y', 19).attr('font-size', '8px').attr('fill', '#888').text('Small');
  uiLayer.append('circle').attr('cx', margin.left + 60).attr('cy', 16).attr('r', 9).attr('fill', '#888').attr('opacity', 0.5);
  uiLayer.append('text').attr('x', margin.left + 73).attr('y', 19).attr('font-size', '8px').attr('fill', '#888').text('Large');

  // Color: high quality = blue, low = muted red
  var qColor = d3.scaleLinear().domain([0, 1]).range(['#c0392b', '#2563eb']);

  var models, timeout;

  function px(q) { return margin.left + toLog(q) * pw; }
  function py(c) { return margin.top + (1 - toLog(c)) * ph; }

  function init() {
    models = [];
    arrowLayer.selectAll('*').remove();
    dotLayer.selectAll('*').remove();
    runPhase(0);
  }

  function render(dur) {
    dur = dur || 700;
    // Show ALL models: alive ones in color, dead ones as transparent grey ghosts
    var circles = dotLayer.selectAll('circle').data(models, function(d) { return d.id; });
    circles.transition().duration(dur)
      .attr('cx', function(d) { return px(d.q); })
      .attr('cy', function(d) { return py(d.c); })
      .attr('r', function(d) { return d.alive ? d.r : d.r * 0.8; })
      .attr('fill', function(d) { return d.alive ? qColor(d.q) : '#ccc'; })
      .attr('opacity', function(d) { return d.alive ? 0.75 : 0.2; })
      .attr('stroke', function(d) { return d.alive ? 'white' : '#ddd'; });
    circles.enter().append('circle')
      .attr('cx', function(d) { return px(d.q); })
      .attr('cy', function(d) { return py(d.c); })
      .attr('r', 0)
      .attr('fill', function(d) { return qColor(d.q); })
      .attr('opacity', 0.75).attr('stroke', 'white').attr('stroke-width', 0.8)
      .transition().duration(dur).attr('r', function(d) { return d.r; });
  }

  // Size always tracks cost: cheap models are tiny, expensive models are large
  function costToRadius(c) { return 2 + c * 16; }

  function addModel(q, c, r, id) {
    models.push({ id: id, q: q, c: c, r: costToRadius(c), alive: true });
  }

  function drawDistillArrow(fromId, toId) {
    var from = models.find(function(m) { return m.id === fromId; });
    var to = models.find(function(m) { return m.id === toId; });
    if (!from || !to) return;
    var line = arrowLayer.append('line')
      .attr('x1', px(from.q)).attr('y1', py(from.c))
      .attr('x2', px(from.q)).attr('y2', py(from.c))
      .attr('stroke', '#e57373').attr('stroke-width', 1.5)
      .attr('stroke-dasharray', '4,3').attr('opacity', 0)
      .attr('marker-end', 'url(#distill-arrow)');
    line.transition().duration(300).attr('opacity', 0.6)
      .transition().duration(600)
      .attr('x2', px(to.q)).attr('y2', py(to.c))
      .transition().delay(1500).duration(600).attr('opacity', 0).remove();
  }

  // Spawn small community models up to a quality ceiling (previous frontier level)
  function spawnCommunity(count, maxQ, prefix) {
    for (var j = 0; j < count; j++) {
      var q = 0.02 + Math.random() * Math.max(0.05, maxQ - 0.02);
      var c = 0.02 + Math.random() * 0.18;
      addModel(Math.max(0.02, q), Math.max(0.02, c), 0, prefix + j);
    }
  }

  function runPhase(phase) {
    if (timeout) clearTimeout(timeout);

    if (phase === 0) {
      // Phase 0: scaling curve. Small/cheap models only.
      models = [];
      arrowLayer.selectAll('*').remove();
      phaseLabel.text('Scaling curve');
      // 50 models along the diagonal, quality up to ~0.22
      for (var i = 0; i < 50; i++) {
        var t = i / 49;
        var spread = Math.pow(t, 0.7);
        var q = 0.02 + spread * 0.22 + (Math.random() - 0.5) * 0.05;
        var c = 0.02 + spread * 0.22 + (Math.random() - 0.5) * 0.05;
        addModel(Math.max(0.02, q), Math.max(0.02, c), 0, 'early' + i);
      }
      render(1200);
      timeout = setTimeout(function() { runPhase(1); }, 4000);

    } else if (phase === 1) {
      // Phase 1: first frontier (q~0.25-0.32). No new small models yet.
      phaseLabel.text('First frontier');
      var f1 = [
        { q: 0.25, c: 0.42, id: 'f1a' },
        { q: 0.30, c: 0.50, id: 'f1b' },
        { q: 0.28, c: 0.45, id: 'f1c' },
        { q: 0.32, c: 0.52, id: 'f1d' }
      ];
      var idx = 0;
      function add1() {
        if (idx >= f1.length) { timeout = setTimeout(function() { runPhase(2); }, 3500); return; }
        addModel(f1[idx].q, f1[idx].c, 0, f1[idx].id);
        render(600); idx++;
        timeout = setTimeout(add1, 800);
      }
      add1();

    } else if (phase === 2) {
      // Phase 2: distill gen 1 -> medium models
      phaseLabel.text('Distillation wave 1');
      var d1 = [
        { from: 'f1a', dq: -0.01, dc: -0.22 },
        { from: 'f1b', dq: -0.02, dc: -0.28 },
        { from: 'f1c', dq: -0.05, dc: -0.25 },
        { from: 'f1d', dq: -0.08, dc: -0.30 },
        { from: 'f1a', dq: -0.10, dc: -0.32 },
        { from: 'f1b', dq: -0.12, dc: -0.36 }
      ];
      var di = 0;
      function distill1() {
        if (di >= d1.length) { timeout = setTimeout(function() { runPhase(3); }, 3000); return; }
        var d = d1[di];
        var parent = models.find(function(m) { return m.id === d.from; });
        if (parent) {
          var nq = Math.max(0.05, parent.q + d.dq + (Math.random() - 0.5) * 0.02);
          var nc = Math.max(0.02, parent.c + d.dc + (Math.random() - 0.5) * 0.02);
          addModel(nq, nc, 0, 'd1_' + di);
          render(500);
          drawDistillArrow(d.from, 'd1_' + di);
        }
        di++;
        timeout = setTimeout(distill1, 900);
      }
      distill1();

    } else if (phase === 3) {
      // Phase 3: small models appear, capped at scaling-curve quality (~0.22)
      phaseLabel.text('Community models');
      spawnCommunity(20, 0.22, 'c1_');
      render(800);
      timeout = setTimeout(function() { runPhase(4); }, 2500);

    } else if (phase === 4) {
      // Phase 4: second frontier (q~0.42-0.50). Gen 1 greys out.
      phaseLabel.text('Second frontier');
      models.forEach(function(m) { if (m.id.indexOf('f1') === 0) m.alive = false; });
      var f2 = [
        { q: 0.42, c: 0.52, id: 'f2a' },
        { q: 0.48, c: 0.60, id: 'f2b' },
        { q: 0.45, c: 0.55, id: 'f2c' },
        { q: 0.50, c: 0.62, id: 'f2d' }
      ];
      f2.forEach(function(f) { addModel(f.q, f.c, 0, f.id); });
      render(800);
      timeout = setTimeout(function() { runPhase(5); }, 3000);

    } else if (phase === 5) {
      // Phase 5: distill gen 2 -> medium models + cascade from gen 1 distills
      phaseLabel.text('Distillation wave 2');
      var d2 = [
        { from: 'f2a', dq: -0.01, dc: -0.28 },
        { from: 'f2b', dq: -0.02, dc: -0.32 },
        { from: 'f2c', dq: -0.05, dc: -0.30 },
        { from: 'f2d', dq: -0.08, dc: -0.36 },
        { from: 'f2a', dq: -0.14, dc: -0.40 },
        { from: 'f2b', dq: -0.16, dc: -0.44 },
        { from: 'd1_0', dq: -0.04, dc: -0.06 },
        { from: 'd1_1', dq: -0.03, dc: -0.05 }
      ];
      var d2i = 0;
      function distill2() {
        if (d2i >= d2.length) { timeout = setTimeout(function() { runPhase(6); }, 3000); return; }
        var d = d2[d2i];
        var parent = models.find(function(m) { return m.id === d.from; });
        if (parent) {
          var nq = Math.max(0.04, parent.q + d.dq + (Math.random() - 0.5) * 0.02);
          var nc = Math.max(0.02, parent.c + d.dc + (Math.random() - 0.5) * 0.02);
          addModel(nq, nc, 0, 'd2_' + d2i);
          render(500);
          drawDistillArrow(d.from, 'd2_' + d2i);
        }
        d2i++;
        timeout = setTimeout(distill2, 900);
      }
      distill2();

    } else if (phase === 6) {
      // Phase 6: community models now reach gen 1 frontier quality (~0.32)
      phaseLabel.text('Community catches up');
      spawnCommunity(25, 0.32, 'c2_');
      render(800);
      timeout = setTimeout(function() { runPhase(7); }, 2500);

    } else if (phase === 7) {
      // Phase 7: third frontier (q~0.58-0.70). Gen 2 greys out.
      phaseLabel.text('Third frontier');
      models.forEach(function(m) { if (m.id.indexOf('f2') === 0) m.alive = false; });
      var f3 = [
        { q: 0.58, c: 0.60, id: 'f3a' },
        { q: 0.66, c: 0.70, id: 'f3b' },
        { q: 0.62, c: 0.65, id: 'f3c' },
        { q: 0.70, c: 0.74, id: 'f3d' }
      ];
      f3.forEach(function(f) { addModel(f.q, f.c, 0, f.id); });
      render(800);
      timeout = setTimeout(function() { runPhase(8); }, 3000);

    } else if (phase === 8) {
      // Phase 8: distill gen 3 + cascade
      phaseLabel.text('Distillation wave 3');
      var d3 = [
        { from: 'f3a', dq: -0.01, dc: -0.34 },
        { from: 'f3b', dq: -0.02, dc: -0.40 },
        { from: 'f3c', dq: -0.05, dc: -0.36 },
        { from: 'f3d', dq: -0.10, dc: -0.44 },
        { from: 'f3a', dq: -0.18, dc: -0.48 },
        { from: 'f3b', dq: -0.20, dc: -0.52 },
        { from: 'd2_0', dq: -0.05, dc: -0.08 },
        { from: 'd2_1', dq: -0.04, dc: -0.06 },
        { from: 'd2_6', dq: -0.03, dc: -0.04 }
      ];
      var d3i = 0;
      function distill3() {
        if (d3i >= d3.length) { timeout = setTimeout(function() { runPhase(9); }, 3000); return; }
        var d = d3[d3i];
        var parent = models.find(function(m) { return m.id === d.from; });
        if (parent) {
          var nq = Math.max(0.03, parent.q + d.dq + (Math.random() - 0.5) * 0.02);
          var nc = Math.max(0.02, parent.c + d.dc + (Math.random() - 0.5) * 0.02);
          addModel(nq, nc, 0, 'd3_' + d3i);
          render(500);
          drawDistillArrow(d.from, 'd3_' + d3i);
        }
        d3i++;
        timeout = setTimeout(distill3, 800);
      }
      distill3();

    } else if (phase === 9) {
      // Phase 9: community now reaches gen 2 frontier quality (~0.50)
      phaseLabel.text('Community catches up');
      spawnCommunity(30, 0.50, 'c3_');
      render(800);
      timeout = setTimeout(function() { runPhase(10); }, 2500);

    } else if (phase === 10) {
      // Phase 10: latest frontier (q~0.76-0.90). Gen 3 greys out.
      phaseLabel.text('Latest frontier');
      models.forEach(function(m) { if (m.id.indexOf('f3') === 0) m.alive = false; });
      var f4 = [
        { q: 0.80, c: 0.75, id: 'f4a' },
        { q: 0.86, c: 0.82, id: 'f4b' },
        { q: 0.76, c: 0.70, id: 'f4c' },
        { q: 0.90, c: 0.88, id: 'f4d' }
      ];
      f4.forEach(function(f) { addModel(f.q, f.c, 0, f.id); });
      render(800);
      timeout = setTimeout(function() { runPhase(11); }, 3000);

    } else if (phase === 11) {
      // Phase 11: final distillation from latest frontier + cascade
      phaseLabel.text('Final distillation');
      var d4 = [
        { from: 'f4a', dq: -0.01, dc: -0.42 },
        { from: 'f4b', dq: -0.02, dc: -0.48 },
        { from: 'f4c', dq: -0.06, dc: -0.38 },
        { from: 'f4d', dq: -0.03, dc: -0.55 },
        { from: 'f4a', dq: -0.20, dc: -0.58 },
        { from: 'f4b', dq: -0.22, dc: -0.62 },
        { from: 'f4d', dq: -0.30, dc: -0.70 },
        { from: 'd3_0', dq: -0.05, dc: -0.10 },
        { from: 'd3_1', dq: -0.04, dc: -0.08 },
        { from: 'd3_6', dq: -0.03, dc: -0.04 }
      ];
      var d4i = 0;
      function distill4() {
        if (d4i >= d4.length) { timeout = setTimeout(function() { runPhase(12); }, 3000); return; }
        var d = d4[d4i];
        var parent = models.find(function(m) { return m.id === d.from; });
        if (parent) {
          var nq = Math.max(0.03, parent.q + d.dq + (Math.random() - 0.5) * 0.02);
          var nc = Math.max(0.01, parent.c + d.dc + (Math.random() - 0.5) * 0.02);
          addModel(nq, nc, 0, 'd4_' + d4i);
          render(500);
          drawDistillArrow(d.from, 'd4_' + d4i);
        }
        d4i++;
        timeout = setTimeout(distill4, 800);
      }
      distill4();

    } else if (phase === 12) {
      // Phase 12: final community burst up to gen 3 quality (~0.70) + grey out early scaling
      phaseLabel.text('Ecosystem equilibrium');
      spawnCommunity(35, 0.70, 'c4_');
      models.forEach(function(m) {
        if (m.id.indexOf('early') === 0) m.alive = false;
      });
      render(1000);
    }
  }

  init();
  svg.on('click', function() { if (timeout) clearTimeout(timeout); init(); });
})();

// ============================================================
// Visualization: Privacy Frontier (in Pattern 7) - 9 global orgs
// ============================================================
(function() {
  var container = document.getElementById('viz-privacy');
  if (!container) return;
  var width = 720, height = 560;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  // 9 orgs - spread out to avoid overlaps
  // 3 rows of 3, staggered
  var orgs = [
    {x: 100, y: 100, r: 55, label: 'EU Health Institute',         reg: 'GDPR',  color: '#dc2626', agents: []},
    {x: 310, y:  80, r: 60, label: 'N. American Research Lab',    reg: 'HIPAA', color: '#2563eb', agents: []},
    {x: 520, y: 100, r: 55, label: 'E. Asian AI Centre',          reg: 'PIPL',  color: '#059669', agents: []},
    {x:  80, y: 280, r: 50, label: 'Pan-African Health Net',      reg: 'AU',    color: '#b45309', agents: []},
    {x: 270, y: 270, r: 55, label: 'US Public Health Agency',     reg: 'HIPAA', color: '#d97706', agents: []},
    {x: 460, y: 280, r: 50, label: 'Global Investment Bank',      reg: 'MiFID', color: '#374151', agents: []},
    {x: 650, y: 190, r: 45, label: 'Gulf Sovereign Fund',         reg: 'PDPL',  color: '#0891b2', agents: []},
    {x: 630, y: 370, r: 50, label: 'Asian Electronics Corp',      reg: 'PIPA',  color: '#7c3aed', agents: []},
    {x: 310, y: 440, r: 50, label: 'Latin American Clinical Net', reg: 'LGPD',  color: '#be185d', agents: []}
  ];

  var connections = [
    [0,1], [1,2], [2,6], [0,4], [1,4], [0,3], [3,4],
    [2,7], [6,5], [4,5], [1,8], [3,8], [4,8], [5,7], [6,7]
  ];

  var animTimer;

  function init() {
    svg.selectAll('*').remove();

    var linkLayer = svg.append('g');
    var lockLayer = svg.append('g');
    var orgLayer = svg.append('g');
    var pktLayer = svg.append('g');

    // Generate agents per org (fewer, well-spaced)
    orgs.forEach(function(o) {
      o.agents = [];
      var n = 3 + Math.floor(Math.random() * 2);
      for (var i = 0; i < n; i++) {
        var angle = (i / n) * Math.PI * 2 + Math.random() * 0.4;
        var dist = 12 + Math.random() * (o.r - 22);
        o.agents.push({
          x: o.x + Math.cos(angle) * dist,
          y: o.y + Math.sin(angle) * dist
        });
      }
    });

    // Draw connection lines
    connections.forEach(function(c) {
      var a = orgs[c[0]], b = orgs[c[1]];
      var dx = b.x - a.x, dy = b.y - a.y;
      var dist = Math.sqrt(dx*dx + dy*dy);
      if (dist === 0) return;
      var nx = dx/dist, ny = dy/dist;
      var x1 = a.x + nx * (a.r + 2), y1 = a.y + ny * (a.r + 2);
      var x2 = b.x - nx * (b.r + 2), y2 = b.y - ny * (b.r + 2);
      linkLayer.append('line')
        .attr('x1', x1).attr('y1', y1).attr('x2', x2).attr('y2', y2)
        .attr('stroke', '#ddd').attr('stroke-width', 1).attr('stroke-dasharray', '4,4');
      // Lock icon at midpoint - bigger and clearer
      var mx = (x1+x2)/2, my = (y1+y2)/2;
      lockLayer.append('rect').attr('x', mx - 7).attr('y', my - 7).attr('width', 14).attr('height', 14)
        .attr('rx', 3).attr('fill', '#fff').attr('stroke', '#ccc').attr('stroke-width', 0.8);
      lockLayer.append('text').attr('x', mx).attr('y', my + 4).attr('text-anchor', 'middle')
        .attr('font-size', '10px').text('\u{1F512}');
    });

    // Org circles, labels, agents
    orgs.forEach(function(o) {
      orgLayer.append('circle').attr('cx', o.x).attr('cy', o.y).attr('r', o.r)
        .attr('fill', o.color).attr('opacity', 0.05)
        .attr('stroke', o.color).attr('stroke-width', 1.5)
        .attr('stroke-dasharray', '6,3').attr('stroke-opacity', 0.4);
      orgLayer.append('text').attr('x', o.x).attr('y', o.y - o.r - 8).attr('text-anchor', 'middle')
        .attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', o.color).text(o.label);
      orgLayer.append('text').attr('x', o.x).attr('y', o.y - o.r + 1).attr('text-anchor', 'middle')
        .attr('font-size', '7px').attr('fill', '#aaa').text(o.reg);
      o.agents.forEach(function(a) {
        orgLayer.append('circle').attr('cx', a.x).attr('cy', a.y).attr('r', 4)
          .attr('fill', o.color).attr('opacity', 0.25).attr('stroke', o.color).attr('stroke-width', 1);
      });
    });

    // Animate: internal flows + cross-boundary encrypted transfers
    if (animTimer) clearInterval(animTimer);
    var animCount = 0;
    animTimer = setInterval(function() {
      animCount++;
      if (animCount > 80) { clearInterval(animTimer); return; }

      // Internal flow
      var oi = Math.floor(Math.random() * orgs.length);
      var o = orgs[oi];
      if (o.agents.length >= 2) {
        var a1 = o.agents[Math.floor(Math.random() * o.agents.length)];
        var a2 = o.agents[Math.floor(Math.random() * o.agents.length)];
        if (a1 !== a2) {
          pktLayer.append('circle')
            .attr('cx', a1.x).attr('cy', a1.y).attr('r', 2)
            .attr('fill', o.color).attr('opacity', 0.6)
            .transition().duration(500).attr('cx', a2.x).attr('cy', a2.y)
            .transition().duration(200).attr('opacity', 0).remove();
        }
      }

      // Cross-boundary (every other tick): colored particle -> lock midpoint -> changes to grey -> arrives
      if (animCount % 2 === 0) {
        var ci = Math.floor(Math.random() * connections.length);
        var conn = connections[ci];
        var rev = Math.random() < 0.5;
        var oFrom = orgs[rev ? conn[1] : conn[0]], oTo = orgs[rev ? conn[0] : conn[1]];
        if (oFrom.agents.length === 0 || oTo.agents.length === 0) return;
        var src = oFrom.agents[Math.floor(Math.random() * oFrom.agents.length)];
        var dst = oTo.agents[Math.floor(Math.random() * oTo.agents.length)];
        var dx = oTo.x - oFrom.x, dy = oTo.y - oFrom.y;
        var dist = Math.sqrt(dx*dx + dy*dy);
        if (dist === 0) return;
        var nx = dx/dist, ny = dy/dist;
        var mx = (oFrom.x + nx * oFrom.r + oTo.x - nx * oTo.r) / 2;
        var my = (oFrom.y + ny * oFrom.r + oTo.y - ny * oTo.r) / 2;

        pktLayer.append('circle')
          .attr('cx', src.x).attr('cy', src.y).attr('r', 2.5)
          .attr('fill', oFrom.color).attr('opacity', 0.8)
          .transition().duration(400).attr('cx', mx).attr('cy', my)
          .transition().duration(60).attr('r', 1.5).attr('fill', '#888').attr('opacity', 0.5)
          .transition().duration(400).attr('cx', dst.x).attr('cy', dst.y)
          .transition().duration(150).attr('opacity', 0).remove();
      }
    }, 280);
  }

  init();
  svg.on('click', init);
})();

// ============================================================
// Visualization: Human-Agent Frontier (7 modes as 4x2 tile grid)
// ============================================================
(function() {
  var container = document.getElementById('viz-humanagent');
  if (!container) return;
  var width = 720, height = 640;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  var modes = [
    { label: '1. Lens', sub: 'consumes / curates', color: '#2563eb', agentN: 3, humanR: 10 },
    { label: '2. Worker', sub: 'delegates / executes', color: '#7c3aed', agentN: 7, humanR: 9 },
    { label: '3. Collaborator', sub: 'co-creates', color: '#d97706', agentN: 10, humanR: 8 },
    { label: '4. Proxy', sub: 'sets policy / acts', color: '#dc2626', agentN: 18, humanR: 6 },
    { label: '5. Fleet', sub: 'air traffic control', color: '#7c3aed', agentN: 30, humanR: 7 },
    { label: '6. Environment', sub: 'inhabits / shapes', color: '#059669', agentN: 25, humanR: 5 },
    { label: '7. Constituency', sub: 'governs population', color: '#0891b2', agentN: 35, humanR: 4 },
    { label: '8. Mirror', sub: 'multiple models of you', color: '#be185d', agentN: 3, humanR: 8 }
  ];

  var nCols = 2, nRows = 4;
  var cellW = 340, cellH = 140, gapX = 12, gapY = 10;
  var padL = (width - nCols * cellW - (nCols - 1) * gapX) / 2;
  var padT = 10;

  function drawHuman(g, cx, cy, r) {
    var s = r / 10;
    g.append('circle').attr('cx', cx).attr('cy', cy - 14 * s).attr('r', 6 * s)
      .attr('fill', '#374151').attr('opacity', 0.7);
    g.append('line').attr('x1', cx).attr('y1', cy - 8 * s).attr('x2', cx).attr('y2', cy + 10 * s)
      .attr('stroke', '#374151').attr('stroke-width', 1.5 * s).attr('opacity', 0.7);
    g.append('line').attr('x1', cx - 8 * s).attr('y1', cy).attr('x2', cx + 8 * s).attr('y2', cy)
      .attr('stroke', '#374151').attr('stroke-width', 1.5 * s).attr('opacity', 0.7);
    g.append('line').attr('x1', cx - 5 * s).attr('y1', cy + 20 * s).attr('x2', cx).attr('y2', cy + 10 * s)
      .attr('stroke', '#374151').attr('stroke-width', 1.5 * s).attr('opacity', 0.7);
    g.append('line').attr('x1', cx + 5 * s).attr('y1', cy + 20 * s).attr('x2', cx).attr('y2', cy + 10 * s)
      .attr('stroke', '#374151').attr('stroke-width', 1.5 * s).attr('opacity', 0.7);
  }

  // Draw functions for each mode's panel content
  // Each returns an array of timer IDs for cleanup
  var drawFns = [];
  var allTimers = [];

  // Helper: send a packet from (x1,y1) to (x2,y2)
  function pkt(g, x1, y1, x2, y2, color, delay, r) {
    r = r || 2.5;
    var t = setTimeout(function() {
      g.append('circle').attr('cx', x1).attr('cy', y1).attr('r', r)
        .attr('fill', color).attr('opacity', 0.8)
        .transition().duration(400).attr('cx', x2).attr('cy', y2)
        .transition().duration(200).attr('opacity', 0).remove();
    }, delay);
    allTimers.push(t);
    return t;
  }

  // Helper: draw standard layout (human left, trust line, agents right) with packets
  function drawStandard(g, w, h, m, trustFrac, pktDir) {
    var hx = w * 0.18, cy = h * 0.5;
    drawHuman(g, hx, cy, m.humanR);
    var tx = w * trustFrac;
    g.append('line').attr('x1', tx).attr('y1', 10).attr('x2', tx).attr('y2', h - 10)
      .attr('stroke', m.color).attr('stroke-width', 1).attr('stroke-dasharray', '4,3').attr('opacity', 0.3);
    var agents = [];
    for (var i = 0; i < m.agentN; i++) {
      var a = (i / m.agentN) * Math.PI * 2;
      var ax = w * 0.72 + Math.cos(a) * (15 + Math.random() * 25);
      var ay = cy + Math.sin(a) * (10 + Math.random() * 20);
      agents.push({x: ax, y: ay});
      g.append('circle').attr('cx', ax).attr('cy', ay).attr('r', 2 + Math.random() * 2)
        .attr('fill', m.color).attr('opacity', 0.4).attr('stroke', 'white').attr('stroke-width', 0.4);
    }
    // Connection lines
    for (var i = 0; i < Math.min(4, agents.length); i++) {
      g.append('line').attr('x1', hx + 12).attr('y1', cy)
        .attr('x2', agents[i].x).attr('y2', agents[i].y)
        .attr('stroke', '#e0e0e0').attr('stroke-width', 0.5).attr('opacity', 0.3);
    }
    // Animated packets
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      var ai = tick % Math.min(4, agents.length);
      var ag = agents[ai];
      if (pktDir === 'toAgent' || pktDir === 'both') {
        pkt(dynG, hx + 12, cy, ag.x, ag.y, m.color, 0);
      }
      if (pktDir === 'toHuman' || pktDir === 'both') {
        pkt(dynG, ag.x, ag.y, hx + 12, cy, '#374151', 500, 2);
      }
      if (tick < 25) allTimers.push(setTimeout(anim, 1200));
    }
    allTimers.push(setTimeout(anim, 300));
  }

  // 1. Lens: funnel - wide info on right narrows through filters to human on left
  drawFns.push(function(g, w, h) {
    var m = modes[0], cy = h * 0.5, hx = w * 0.15;
    drawHuman(g, hx, cy, m.humanR);
    // Info dots on right (the world's information)
    var dots = [];
    for (var i = 0; i < 15; i++) {
      var dx = w * 0.75 + (Math.random() - 0.5) * w * 0.35;
      var dy = 15 + Math.random() * (h - 30);
      dots.push({x: dx, y: dy});
      g.append('circle').attr('cx', dx).attr('cy', dy).attr('r', 1.5)
        .attr('fill', '#ccc').attr('opacity', 0.3);
    }
    // 3 filter agents in the middle (funnel)
    var filters = [{x: w * 0.48, y: cy - 20}, {x: w * 0.45, y: cy}, {x: w * 0.48, y: cy + 20}];
    filters.forEach(function(f) {
      g.append('circle').attr('cx', f.x).attr('cy', f.y).attr('r', 4)
        .attr('fill', m.color).attr('opacity', 0.3).attr('stroke', m.color).attr('stroke-width', 1);
    });
    // Funnel lines
    g.append('line').attr('x1', w * 0.7).attr('y1', 15).attr('x2', w * 0.5).attr('y2', cy - 25)
      .attr('stroke', m.color).attr('stroke-width', 0.5).attr('opacity', 0.15);
    g.append('line').attr('x1', w * 0.7).attr('y1', h - 15).attr('x2', w * 0.5).attr('y2', cy + 25)
      .attr('stroke', m.color).attr('stroke-width', 0.5).attr('opacity', 0.15);
    g.append('line').attr('x1', w * 0.5).attr('y1', cy - 25).attr('x2', hx + 15).attr('y2', cy)
      .attr('stroke', m.color).attr('stroke-width', 0.5).attr('opacity', 0.15);
    g.append('line').attr('x1', w * 0.5).attr('y1', cy + 25).attr('x2', hx + 15).attr('y2', cy)
      .attr('stroke', m.color).attr('stroke-width', 0.5).attr('opacity', 0.15);
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      // Pick a random dot, send through a filter, only some reach human
      var d = dots[tick % dots.length];
      var f = filters[tick % filters.length];
      pkt(dynG, d.x, d.y, f.x, f.y, '#aaa', 0, 1.5);
      if (tick % 3 === 0) pkt(dynG, f.x, f.y, hx + 12, cy, m.color, 450, 2);
      if (tick < 30) allTimers.push(setTimeout(anim, 800));
    }
    allTimers.push(setTimeout(anim, 300));
  });
  // 2. Worker: human throws task over, agents execute, results come back (assembly line)
  drawFns.push(function(g, w, h) {
    var m = modes[1], cy = h * 0.5, hx = w * 0.12;
    drawHuman(g, hx, cy, m.humanR);
    // Task arrow from human
    g.append('text').attr('x', hx + 22).attr('y', cy - 12)
      .attr('font-size', '6px').attr('fill', '#aaa').text('task');
    // Worker agents on the right
    var workers = [];
    for (var i = 0; i < 5; i++) {
      var wx = w * 0.5 + i * 22, wy = cy + (i % 2 === 0 ? -12 : 12);
      workers.push({x: wx, y: wy});
      g.append('circle').attr('cx', wx).attr('cy', wy).attr('r', 4)
        .attr('fill', m.color).attr('opacity', 0.3).attr('stroke', m.color).attr('stroke-width', 1);
    }
    // Assembly line: horizontal arrows between workers
    for (var i = 0; i < workers.length - 1; i++) {
      g.append('line').attr('x1', workers[i].x + 5).attr('y1', workers[i].y)
        .attr('x2', workers[i+1].x - 5).attr('y2', workers[i+1].y)
        .attr('stroke', '#ddd').attr('stroke-width', 0.8);
    }
    // Result arrow back
    g.append('text').attr('x', w * 0.5).attr('y', cy + 35)
      .attr('font-size', '6px').attr('fill', '#aaa').text('result');
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      // Task from human to first worker
      pkt(dynG, hx + 12, cy, workers[0].x, workers[0].y, m.color, 0, 2.5);
      // Chain through workers
      for (var i = 0; i < workers.length - 1; i++) {
        pkt(dynG, workers[i].x, workers[i].y, workers[i+1].x, workers[i+1].y, m.color, 300 + i * 200, 2);
      }
      // Result back to human
      var last = workers[workers.length - 1];
      pkt(dynG, last.x, last.y, hx + 12, cy + 8, '#059669', 300 + (workers.length - 1) * 200 + 300, 2.5);
      if (tick < 15) allTimers.push(setTimeout(anim, 2200));
    }
    allTimers.push(setTimeout(anim, 300));
  });
  // 3. Collaborator: human and agent side by side, rapid back-and-forth on a shared workspace
  drawFns.push(function(g, w, h) {
    var m = modes[2], cy = h * 0.5;
    drawHuman(g, w * 0.22, cy, m.humanR);
    // One main collaborator agent (bigger)
    g.append('circle').attr('cx', w * 0.7).attr('cy', cy).attr('r', 8)
      .attr('fill', m.color).attr('opacity', 0.15).attr('stroke', m.color).attr('stroke-width', 1.5);
    g.append('text').attr('x', w * 0.7).attr('y', cy + 3).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', m.color).text('Agent');
    // Shared workspace in middle - bigger
    g.append('rect').attr('x', w * 0.30).attr('y', cy - 35).attr('width', w * 0.30).attr('height', 70)
      .attr('rx', 6).attr('fill', m.color).attr('opacity', 0.04).attr('stroke', m.color)
      .attr('stroke-width', 0.8).attr('stroke-dasharray', '3,2');
    g.append('text').attr('x', w * 0.45).attr('y', cy - 38).attr('text-anchor', 'middle')
      .attr('font-size', '6px').attr('fill', '#aaa').text('shared workspace');
    // Ideas accumulate in workspace
    var ideas = [];
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      var wsX = w * 0.32 + Math.random() * w * 0.26;
      var wsY = cy - 28 + Math.random() * 56;
      if (tick % 2 === 1) {
        // Human contributes idea
        pkt(dynG, w * 0.28, cy, wsX, wsY, '#374151', 0, 2);
      } else {
        // Agent contributes idea
        pkt(dynG, w * 0.62, cy, wsX, wsY, m.color, 0, 2);
      }
      // Idea dot stays in workspace
      allTimers.push(setTimeout(function() {
        if (ideas.length < 12) {
          var dot = dynG.append('circle').attr('cx', wsX).attr('cy', wsY).attr('r', 0)
            .attr('fill', tick % 2 === 1 ? '#374151' : m.color).attr('opacity', 0.3)
            .transition().duration(200).attr('r', 1.5);
          ideas.push(dot);
        }
      }, 450));
      if (tick < 25) allTimers.push(setTimeout(anim, 700));
    }
    allTimers.push(setTimeout(anim, 200));
  });
  // 4. Proxy: human sets policy (boundary), then agents act fully autonomously inside
  drawFns.push(function(g, w, h) {
    var m = modes[3], cy = h * 0.5, hx = w * 0.1;
    drawHuman(g, hx, cy, m.humanR);
    // Thick policy boundary
    var bx = w * 0.26;
    g.append('rect').attr('x', bx - 3).attr('y', 10).attr('width', 6).attr('height', h - 20)
      .attr('rx', 3).attr('fill', m.color).attr('opacity', 0.12);
    g.append('text').attr('x', bx).attr('y', h - 6).attr('text-anchor', 'middle')
      .attr('font-size', '6px').attr('fill', m.color).attr('opacity', 0.5).text('policy boundary');
    // Autonomous zone label
    g.append('text').attr('x', w * 0.62).attr('y', 16)
      .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#aaa').text('autonomous zone');
    // Agents swarm freely on the right
    var agents = [];
    for (var i = 0; i < 14; i++) {
      var ax = w * 0.4 + Math.random() * (w * 0.5);
      var ay = 20 + Math.random() * (h - 40);
      agents.push({x: ax, y: ay, vx: (Math.random() - 0.5) * 2, vy: (Math.random() - 0.5) * 1.5});
      g.append('circle').attr('cx', ax).attr('cy', ay).attr('r', 2.5)
        .attr('fill', m.color).attr('opacity', 0.35).attr('stroke', m.color).attr('stroke-width', 0.5);
    }
    // Agent-to-agent connections
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      // Agents talk to each other (busy)
      var i = tick % agents.length, j = (i + 1 + Math.floor(Math.random() * 3)) % agents.length;
      pkt(dynG, agents[i].x, agents[i].y, agents[j].x, agents[j].y, m.color, 0, 1.5);
      // Human occasionally peeks but doesn't intervene
      if (tick === 5) {
        g.append('line').attr('x1', hx + 8).attr('y1', cy).attr('x2', bx - 4).attr('y2', cy)
          .attr('stroke', '#ddd').attr('stroke-width', 0.5).attr('stroke-dasharray', '2,2').attr('opacity', 0.4);
      }
      if (tick < 30) allTimers.push(setTimeout(anim, 600));
    }
    allTimers.push(setTimeout(anim, 300));
  });
  // 5. Fleet - radar with rotating scan line and moving planes
  drawFns.push(function(g, w, h) {
    var m = modes[4], cx = w / 2, cy = h / 2;
    var rMax = Math.min(w, h) * 0.38;
    // Radar rings
    g.append('circle').attr('cx', cx).attr('cy', cy).attr('r', rMax)
      .attr('fill', 'none').attr('stroke', m.color).attr('stroke-width', 0.5).attr('opacity', 0.15);
    g.append('circle').attr('cx', cx).attr('cy', cy).attr('r', rMax * 0.58)
      .attr('fill', 'none').attr('stroke', m.color).attr('stroke-width', 0.5).attr('opacity', 0.1);
    // Crosshairs
    g.append('line').attr('x1', cx - rMax).attr('y1', cy).attr('x2', cx + rMax).attr('y2', cy)
      .attr('stroke', m.color).attr('stroke-width', 0.3).attr('opacity', 0.1);
    g.append('line').attr('x1', cx).attr('y1', cy - rMax).attr('x2', cx).attr('y2', cy + rMax)
      .attr('stroke', m.color).attr('stroke-width', 0.3).attr('opacity', 0.1);
    drawHuman(g, cx, cy, m.humanR);
    // Agents as blips
    var agents = [];
    for (var i = 0; i < m.agentN; i++) {
      var a = (i / m.agentN) * Math.PI * 2 + (Math.random() - 0.5) * 0.5;
      var r = 18 + Math.random() * (rMax - 20);
      var speed = 0.003 + Math.random() * 0.006;
      agents.push({angle: a, r: r, speed: speed * (Math.random() > 0.5 ? 1 : -1),
        el: g.append('circle').attr('r', 2).attr('fill', m.color).attr('opacity', 0.5)
          .attr('stroke', 'white').attr('stroke-width', 0.3)});
    }
    // Scan line
    var scanLine = g.append('line').attr('x1', cx).attr('y1', cy)
      .attr('stroke', m.color).attr('stroke-width', 1.2).attr('opacity', 0.2);
    var scanAngle = 0;
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      scanAngle += 0.08;
      scanLine.attr('x2', cx + Math.cos(scanAngle) * rMax).attr('y2', cy + Math.sin(scanAngle) * rMax);
      // Move agents
      agents.forEach(function(ag) {
        ag.angle += ag.speed;
        ag.el.attr('cx', cx + Math.cos(ag.angle) * ag.r)
          .attr('cy', cy + Math.sin(ag.angle) * ag.r * 0.7);
      });
      // Occasional command from controller to an agent
      if (tick % 8 === 0) {
        var tgt = agents[Math.floor(Math.random() * agents.length)];
        var tx = cx + Math.cos(tgt.angle) * tgt.r;
        var ty = cy + Math.sin(tgt.angle) * tgt.r * 0.7;
        pkt(dynG, cx, cy, tx, ty, '#f59e0b', 0, 2);
      }
      if (tick < 200) allTimers.push(setTimeout(anim, 80));
    }
    allTimers.push(setTimeout(anim, 100));
  });
  // 6. Environment - human surrounded by ambient effects, no visible agents
  drawFns.push(function(g, w, h) {
    var m = modes[5], cx = w / 2, cy = h / 2;
    drawHuman(g, cx, cy, m.humanR);
    // Ambient bubble around human
    g.append('circle').attr('cx', cx).attr('cy', cy).attr('r', 45)
      .attr('fill', m.color).attr('opacity', 0.03).attr('stroke', m.color)
      .attr('stroke-width', 0.5).attr('stroke-dasharray', '2,2').attr('opacity', 0.1);
    // Ambient effect icons (not agents - just outcomes)
    var effects = [
      {x: cx - 55, y: cy - 20, label: 'lights'},
      {x: cx + 55, y: cy - 15, label: 'schedule'},
      {x: cx - 40, y: cy + 30, label: 'news'},
      {x: cx + 45, y: cy + 25, label: 'temp'},
      {x: cx - 10, y: cy - 40, label: 'music'},
      {x: cx + 15, y: cy + 40, label: 'code review'}
    ];
    effects.forEach(function(e) {
      g.append('text').attr('x', e.x).attr('y', e.y).attr('text-anchor', 'middle')
        .attr('font-size', '6px').attr('fill', m.color).attr('opacity', 0.35).text(e.label);
    });
    g.append('text').attr('x', cx).attr('y', cy + 55)
      .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#aaa').text('no visible agents');
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      // Random ambient effect pulses (environment adjusting)
      var e = effects[tick % effects.length];
      dynG.append('circle').attr('cx', e.x).attr('cy', e.y).attr('r', 3)
        .attr('fill', m.color).attr('opacity', 0.25)
        .transition().duration(600).attr('r', 10).attr('opacity', 0).remove();
      if (tick < 40) allTimers.push(setTimeout(anim, 700));
    }
    allTimers.push(setTimeout(anim, 400));
  });
  // 7. Constituency - policy broadcasts down, stats bubble up
  drawFns.push(function(g, w, h) {
    var m = modes[6], cx = w / 2;
    drawHuman(g, cx, h * 0.18, m.humanR);
    g.append('line').attr('x1', 20).attr('y1', h * 0.35).attr('x2', w - 20).attr('y2', h * 0.35)
      .attr('stroke', m.color).attr('stroke-width', 1).attr('stroke-dasharray', '5,3').attr('opacity', 0.3);
    g.append('text').attr('x', w - 22).attr('y', h * 0.34)
      .attr('text-anchor', 'end').attr('font-size', '6px').attr('fill', m.color).attr('opacity', 0.4).text('policy');
    var agents = [];
    for (var i = 0; i < m.agentN; i++) {
      var ax = 20 + Math.random() * (w - 40), ay = h * 0.42 + Math.random() * (h * 0.5);
      agents.push({x: ax, y: ay});
      g.append('circle').attr('cx', ax).attr('cy', ay)
        .attr('r', 1 + Math.random() * 2).attr('fill', m.color).attr('opacity', 0.25 + Math.random() * 0.15);
    }
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      if (tick % 3 === 1) {
        // Policy broadcast: packet from human spreads down
        var targets = [agents[tick % agents.length], agents[(tick + 7) % agents.length], agents[(tick + 13) % agents.length]];
        targets.forEach(function(ag, i) { pkt(dynG, cx, h * 0.22, ag.x, ag.y, m.color, i * 100, 2); });
      } else {
        // Stats bubble up
        var ag = agents[tick % agents.length];
        pkt(dynG, ag.x, ag.y, cx, h * 0.22, '#9ca3af', 0, 1.5);
      }
      if (tick < 30) allTimers.push(setTimeout(anim, 900));
    }
    allTimers.push(setTimeout(anim, 400));
  });
  // 8. Mirror - multiple models of you, data flows to each, reflections back
  drawFns.push(function(g, w, h) {
    var m = modes[7], cy = h * 0.5, lx = w * 0.15;
    drawHuman(g, lx, cy, m.humanR);
    // Mirror line
    g.append('line').attr('x1', w * 0.32).attr('y1', 8).attr('x2', w * 0.32).attr('y2', h - 8)
      .attr('stroke', m.color).attr('stroke-width', 0.8).attr('stroke-dasharray', '3,2').attr('opacity', 0.25);
    // Five models of you: 2 columns, arranged around center
    var mirrors = [
      {x: w * 0.48, y: cy - 30, label: 'cognitive', scale: 0.6},
      {x: w * 0.48, y: cy,      label: 'emotional', scale: 0.6},
      {x: w * 0.48, y: cy + 30, label: 'behavioural', scale: 0.6},
      {x: w * 0.72, y: cy - 15, label: 'employee', scale: 0.6},
      {x: w * 0.72, y: cy + 15, label: 'patient', scale: 0.6}
    ];
    var halos = [];
    mirrors.forEach(function(mr) {
      drawHuman(g, mr.x, mr.y, m.humanR * mr.scale);
      halos.push(g.append('circle').attr('cx', mr.x).attr('cy', mr.y).attr('r', 12)
        .attr('fill', 'none').attr('stroke', m.color).attr('stroke-width', 0.8).attr('opacity', 0.1));
      g.append('text').attr('x', mr.x + 14).attr('y', mr.y + 3).attr('text-anchor', 'start')
        .attr('font-size', '5.5px').attr('fill', m.color).attr('opacity', 0.5).text(mr.label);
      // Connection line
      g.append('line').attr('x1', lx + 10).attr('y1', cy).attr('x2', mr.x - 8).attr('y2', mr.y)
        .attr('stroke', '#e0e0e0').attr('stroke-width', 0.4).attr('opacity', 0.3);
    });
    var dynG = g.append('g');
    var tick = 0;
    function anim() {
      tick++;
      var mi = tick % mirrors.length;
      var mr = mirrors[mi];
      // Data from human to this model
      pkt(dynG, lx + 10, cy, mr.x - 8, mr.y, '#374151', 0, 2);
      // Halo pulse
      halos[mi].transition().duration(300).attr('opacity', 0.3).attr('r', 16)
        .transition().duration(400).attr('opacity', 0.1).attr('r', 12);
      // Reflection back
      pkt(dynG, mr.x - 8, mr.y, lx + 10, cy, m.color, 500, 2);
      if (tick < 30) allTimers.push(setTimeout(anim, 900));
    }
    allTimers.push(setTimeout(anim, 400));
  });

  // Expand panel overlay (same pattern as delegation viz)
  function expandPanel(idx) {
    var m = modes[idx];
    var backdrop = document.createElement('div');
    backdrop.style.cssText = 'position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.5);z-index:9998;cursor:pointer;';
    var card = document.createElement('div');
    card.style.cssText = 'position:fixed;top:50%;left:50%;transform:translate(-50%,-50%);background:white;border-radius:12px;padding:18px 18px 12px;z-index:9999;box-shadow:0 8px 32px rgba(0,0,0,0.25);max-width:92vw;max-height:88vh;';
    var title = document.createElement('div');
    title.style.cssText = 'font-size:16px;font-weight:bold;color:' + m.color + ';margin-bottom:2px;';
    title.textContent = m.label;
    var sub = document.createElement('div');
    sub.style.cssText = 'font-size:12px;color:#888;margin-bottom:8px;';
    sub.textContent = m.sub;
    var close = document.createElement('div');
    close.style.cssText = 'position:absolute;top:8px;right:14px;font-size:20px;cursor:pointer;color:#999;';
    close.textContent = '\u00d7';
    card.appendChild(title);
    card.appendChild(sub);
    card.appendChild(close);
    var svgNS = 'http://www.w3.org/2000/svg';
    var eSvg = document.createElementNS(svgNS, 'svg');
    eSvg.setAttribute('viewBox', '0 0 ' + cellW + ' ' + cellH);
    eSvg.setAttribute('width', '640');
    eSvg.setAttribute('height', Math.round(640 * cellH / cellW));
    eSvg.style.display = 'block';
    card.appendChild(eSvg);
    document.body.appendChild(backdrop);
    document.body.appendChild(card);
    var eG = d3.select(eSvg).append('g');
    drawFns[idx](eG, cellW, cellH);
    function dismiss(ev) { ev.stopPropagation(); document.body.removeChild(backdrop); document.body.removeChild(card); }
    backdrop.addEventListener('click', dismiss);
    close.addEventListener('click', dismiss);
    card.addEventListener('click', function(ev) { ev.stopPropagation(); });
  }

  function init() {
    svg.selectAll('*').remove();
    allTimers.forEach(function(t) { clearTimeout(t); });
    allTimers = [];

    // Autonomy arrow along the top
    svg.append('text').attr('x', padL).attr('y', 8).attr('font-size', '7px').attr('fill', '#bbb').text('less autonomy');
    svg.append('text').attr('x', padL + nCols * cellW + gapX).attr('y', height - 3)
      .attr('text-anchor', 'end').attr('font-size', '7px').attr('fill', '#bbb').text('more autonomy');

    modes.forEach(function(m, idx) {
      var col = idx % nCols, row = Math.floor(idx / nCols);
      var x = padL + col * (cellW + gapX);
      var y = padT + row * (cellH + gapY);

      // Cell background
      var cellG = svg.append('g').style('cursor', 'pointer');
      cellG.append('rect').attr('x', x).attr('y', y)
        .attr('width', cellW).attr('height', cellH).attr('rx', 6)
        .attr('fill', '#fafafa').attr('stroke', m.color).attr('stroke-width', 1).attr('opacity', 0.8);

      // Label
      cellG.append('text').attr('x', x + 8).attr('y', y + 14)
        .attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', m.color).text(m.label);
      cellG.append('text').attr('x', x + 8).attr('y', y + 25)
        .attr('font-size', '8px').attr('fill', '#999').text(m.sub);

      // Content area inside cell
      var contentG = cellG.append('g').attr('transform', 'translate(' + x + ',' + y + ')');
      drawFns[idx](contentG, cellW, cellH);

      // Click to expand
      cellG.on('click', function(event) { event.stopPropagation(); expandPanel(idx); });
    });

  }

  init();
  svg.on('click', init);
})();

</script>
