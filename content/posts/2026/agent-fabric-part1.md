---
title: "The Agent Fabric (Part 1): Why Agents Form Societies"
subtitle: "As agents multiply, isolation becomes expensive. Shared memory, specialization, and governance push useful agents into bounded societies."
date: 2026-04-28
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "multi-agent systems", "LLM", "agent fabric", "society of mind", "the loom hypothesis"]
description: "Why AI agents may form societies, share knowledge at global scale, and why adaptive structures could become the norm."
cover:
  image: "/images/agent-fabric-cover.svg"
  alt: "Abstract visualization of threads weaving into a fabric pattern, representing designed and emergent agent interactions"
  hidden: true
draft: false
math: false
ShowToc: false
TocOpen: false
hideCitation: false
scholar: true
wordcount: "~4,000 words (body) · ~2,200 words (notes)"
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
  .series-nav {
    background: #eff6ff;
    border: 1px solid #bfdbfe;
    border-radius: 6px;
    padding: 1em 1.2em;
    margin: 1.5em 0;
    font-size: 0.9em;
  }
  .series-nav ul {
    margin: 0.5em 0 0 0;
    padding-left: 1.2em;
  }
  .series-nav li {
    margin: 0.3em 0;
  }
  .gov-list {
    margin: 1.5em 0;
  }
  .gov-list details {
    border: 1px solid #e5e5e5;
    border-radius: 6px;
    margin-bottom: 0.5em;
    background: #fafafa;
  }
  .gov-list details[open] {
    background: #fff;
  }
  .gov-list summary {
    cursor: pointer;
    padding: 0.7em 1em;
    font-weight: 600;
    font-size: 0.95em;
    list-style: none;
    display: flex;
    align-items: center;
    gap: 0.5em;
  }
  .gov-list summary::-webkit-details-marker { display: none; }
  .gov-list summary::before {
    content: "\25B6";
    font-size: 0.6em;
    color: #999;
    transition: transform 0.15s;
  }
  .gov-list details[open] summary::before {
    transform: rotate(90deg);
  }
  .gov-list .gov-tagline {
    font-weight: 400;
    color: #888;
    font-size: 0.9em;
  }
  .gov-list .gov-body {
    padding: 0 1em 1em 1em;
    font-size: 0.93em;
    line-height: 1.6;
    color: #444;
  }
  .viz-container { cursor: zoom-in; }
  .viz-container.viz-expanded {
    position: fixed; top: 50%; left: 50%;
    transform: translate(-50%, -50%) scale(var(--viz-scale, 1.4));
    z-index: 10000; cursor: zoom-out;
    box-shadow: 0 0 0 9999px rgba(0,0,0,0.8);
    transition: transform 0.2s ease;
  }
  @media print {
    .viz-restart { display: none !important; }
    .viz-caption details { display: none !important; }
    .viz-container {
      break-inside: avoid;
      page-break-inside: avoid;
      overflow: visible;
    }
    .viz-caption {
      position: static !important;
      margin-top: 0.5rem;
      page-break-before: avoid;
    }
  }
</style>

<script>
(function(){
  var expanded = null;
  document.addEventListener('click', function(e){
    if(expanded){
      expanded.classList.remove('viz-expanded');
      expanded.style.removeProperty('--viz-scale');
      expanded = null;
      e.stopPropagation();
      return;
    }
    var vc = e.target.closest('.viz-container');
    if(!vc) return;
    if(e.target.closest('.viz-caption, .viz-restart, button, a, details')) return;
    if(vc.querySelector('#viz-two-societies')) return;
    e.stopPropagation();
    var rect = vc.getBoundingClientRect();
    var scaleX = (window.innerWidth * 0.9) / rect.width;
    var scaleY = (window.innerHeight * 0.9) / rect.height;
    var scale = Math.min(scaleX, scaleY, 2.5);
    vc.style.setProperty('--viz-scale', scale);
    vc.classList.add('viz-expanded');
    expanded = vc;
  }, true);
  document.addEventListener('keydown', function(e){
    if(e.key==='Escape' && expanded){
      expanded.classList.remove('viz-expanded');
      expanded.style.removeProperty('--viz-scale');
      expanded = null;
    }
  });
})();
</script>

<div style="background: #fffbeb; border: 1px solid #f59e0b; border-radius: 6px; padding: 0.6em 1em; margin: 1.5em 0; font-size: 0.88em; color: #92400e;">
<strong>Early access.</strong> This blog series is a work in progress. Feedback, comments, and suggestions are welcome. Feel free to <a href="https://www.linkedin.com/in/kasra-hosseini/" target="_blank" style="color: #92400e;">reach out on LinkedIn</a> or leave a comment at the bottom of the page.
</div>

<div class="viz-container" style="margin-top: 0.5em;">
  <div id="viz-two-societies" style="width: 100%;" role="img" aria-label="Animated visualization showing four phases of evolution from isolated human-agent pairs to interwoven societies."></div>
  <div class="viz-caption" style="margin-top: 1em;"><strong>Figure 1. The evolution of two societies.</strong> This animation previews the argument of the post in four phases: from isolated human-agent pairs, through an emerging <a href="#the-resource-ecology">resource ecology</a>, to governed societies with shared memory, and finally a living fabric that restructures itself under pressure. Use the arrows to navigate. The two premises that drive this progression (<a href="#two-observations">Two Observations</a>) and the hypothesis that connects them (<a href="#the-vision-from-isolation-to-interweaving">The Loom Hypothesis</a>) are developed in <a href="#from-mindless-to-mindful-beyond-the-society-of-mind">the sections below</a>.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;"><em>Phase 1 (Isolation)</em>: humans interact with individual frontier models (large dark circles marked "F") that already have memory, personalization, and access to tools, yet different users' agents do not communicate with each other. This is the world most people know today. <em>Phase 2 (Ecosystem Grows)</em>: frontier models produce smaller, cheaper distilled models that begin connecting to each other, forming a <a href="#the-resource-ecology">resource ecology</a> of diverse model sizes rather than a single dominant model (<a href="#why-not-one-model-to-rule-them-all">why not one model?</a>). <em>Phase 3 (Societies Form)</em>: agents cluster into governed societies (colored boundaries), each with its own <a href="#governance-how-agent-societies-are-ruled">governance archetype</a>. Collective Memories (CM, green squares) and local Knowledge Bases (KB, small labeled rectangles) store society-specific or agent-specific knowledge, feeding into a <a href="#collective-memory-and-the-knowledge-factory">Knowledge Factory</a> (KF, diamond at the bottom) that synthesizes insights across clusters. <em>Phase 4 (The Living Society)</em>: the fabric comes alive. Tasks flow across society boundaries (the moving dots represent work, not agents traveling), boundary events reshape governance structures (mergers, schisms, expansions), and knowledge flows continuously. This is the <a href="#the-adaptive-fabric">adaptive fabric</a>, a system that restructures itself under pressure rather than breaking.</p>
  </details></div>
</div>

When you use ChatGPT, Claude, or Gemini, you are talking to one AI. It has memory, it can search the web, run code, browse files, yet there is only one model behind the curtain. Something different is already happening one layer down. When you ask a coding agent to refactor a module, the interaction may feel like one assistant, but the work is often split across planners, tool calls, test runners, and specialized sub-agents. You interacted with one agent; several did the work behind the scenes. Now scale it to millions or billions of agents, coordinating across organizations, forming persistent relationships. What organizational structures could emerge, and what would govern them? We call this interconnected system **the Agent Fabric**.

Some threads in this fabric are **designed patterns**, built top-down by engineers who assign roles and routing. Others are **emergent dynamics**. As billions of people acquire personal agents, those agents connect through social interactions, shopping routines, and work collaborations, forming societies through repeated interaction rather than design.

<!-- Series navigation -->
<div class="series-nav">
<strong>The Agent Fabric</strong>, a multi-part series on why and how AI agents may form societies and what it means for us.

- **Part 1. Why Agents Form Societies** (you are here). Two observations, the Loom Hypothesis, and the path from isolation to interweaving
- **Part 2. Division of Labour and Governance.** Delegation archetypes, the specialist market, and governance archetypes *(coming soon)*
- **Part 3. The Human in the Weave.** Modes of human-agent interaction and the fabric in daily life *(coming soon)*
</div>

<details class="series-nav" style="cursor: pointer;">
<summary><strong>Table of Contents</strong></summary>

- [From Mindless to Mindful: Beyond the Society of Mind](#from-mindless-to-mindful-beyond-the-society-of-mind)
- [Two Observations](#two-observations)
- [The Vision: From Isolation to Interweaving](#the-vision-from-isolation-to-interweaving)
- [The Resource Ecology](#the-resource-ecology)
- [Why Not One Model to Rule Them All?](#why-not-one-model-to-rule-them-all)
- [Governance: How Agent Societies Are Ruled](#governance-how-agent-societies-are-ruled)
- [Collective Memory and the Knowledge Factory](#collective-memory-and-the-knowledge-factory)
- [The Adaptive Fabric](#the-adaptive-fabric)
- [The Living Society and What Comes Next](#the-living-society-and-what-comes-next)
</details>

## From Mindless to Mindful: Beyond the Society of Mind

In 1986, Marvin Minsky proposed in <a href="https://en.wikipedia.org/wiki/Society_of_Mind" target="_blank" class="red-link">*The Society of Mind*</a> that intelligence emerges from the interaction of many simple, specialized agents, none individually intelligent.

> "What magical trick makes us intelligent? The trick is that there is no trick. The power of intelligence stems from our vast diversity, not from any single, perfect principle." *— Marvin Minsky, The Society of Mind (1986)*

Minsky asked what happens when you wire together many simple parts. Today's AI agents are different. They already reason across domains, write code, use tools, and hold extended conversations. We face a different question.

**What happens when you wire together many *intelligent* parts?**

This blog series argues that societies of AI agents will be both designed and emergent. Through governance, memory, reputation, and specialization, these societies may achieve collective intelligence that exceeds what any individual agent could. No individual human can build a semiconductor fab, but organizational structures let individual capabilities compose. The same applies to agents, though the critical difference is speed. An agent can transmit its operational context to another agent in seconds, and a governance structure can, in principle, be restructured and redeployed in hours rather than years.

<details id="fn-context" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">On context sharing</summary>
<p style="margin-top: 0.5em; color: #555;">Agent societies face constraints different from humans, not fewer. Running AI systems today <a href="https://dl.acm.org/doi/10.1145/3630106.3658542" target="_blank" class="red-link">costs significant energy</a>, and compute, context windows, and cost are real bottlenecks, at least for now. What makes agent coordination qualitatively different is what gets shared: conversation history, retrieved documents, tool outputs, and intermediate reasoning can all be transmitted instantly. Context sharing is where most coordination value lies.</p>
</details>

<details id="fn-prior-work" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">The idea of agent societies is not new</summary>
<p style="margin-top: 0.5em; color: #555;">Minsky's <em>Society of Mind</em> (discussed above) was an early conceptual ancestor, though his "agents" were simple mental processes rather than deployed AI systems. Distributed AI has studied related problems for decades, including <a href="https://en.wikipedia.org/wiki/Contract_Net_Protocol" target="_blank" class="red-link">Contract Net</a> (1980) for bidding over tasks, <a href="https://en.wikipedia.org/wiki/Knowledge_Query_and_Manipulation_Language" target="_blank" class="red-link">KQML</a> and <a href="https://en.wikipedia.org/wiki/FIPA_ACL" target="_blank" class="red-link">FIPA ACL</a> for agent communication, and blackboard systems for shared problem-solving. Economics and governance theory approached adjacent questions from another angle, notably <a href="https://en.wikipedia.org/wiki/The_Nature_of_the_Firm" target="_blank" class="red-link">Coase</a> on transaction costs, <a href="https://en.wikipedia.org/wiki/The_Use_of_Knowledge_in_Society" target="_blank" class="red-link">Hayek</a> on distributed knowledge, and <a href="https://en.wikipedia.org/wiki/Elinor_Ostrom#Design_principles_for_Common_Pool_Resource_(CPR)_institution" target="_blank" class="red-link">Ostrom</a> on commons governance.</p>
<p style="color: #555;">LLM agents change the substrate rather than the underlying question. They can exchange natural-language context, call tools, pass operational state, and be rearranged without rebuilding the whole system. Early LLM-agent systems such as <a href="https://arxiv.org/abs/2303.17760" target="_blank" class="red-link">CAMEL</a>, <a href="https://arxiv.org/abs/2308.00352" target="_blank" class="red-link">MetaGPT</a>, Stanford's <a href="https://arxiv.org/abs/2304.03442" target="_blank" class="red-link">Generative Agents</a>, and <a href="https://www.microsoft.com/en-us/research/blog/autogen-enabling-next-generation-large-language-model-applications/" target="_blank" class="red-link">AutoGen</a> showed pieces of this pattern. They are not full societies in the sense used here, but they show why useful agent work often becomes organizational. The old question was how to make simple agents coordinate. The new question is what happens when capable, tool-using agents coordinate at scale.</p>
</details>

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 1em 1.2em; margin: 1.5em 0;">
<strong>This blog series makes three claims:</strong>

1. **Isolation is unstable where shared context matters.** Agents persist only while useful, and resources remain finite even as agent populations grow. Together, these create economic pressure to connect agents into coordinated structures. When coordination costs are lower than duplicated work, connected agents will tend to outperform isolated ones, and the pressure intensifies with scale.
2. **Governance is not overhead; it is the product.** Delegation patterns, verification mechanisms, memory rules, and trust structures are not implementation details to add later. They determine what kind of knowledge a society produces, how resilient it is to behavioral drift, and whether it fails gracefully or catastrophically.
3. **Some capabilities are organizational, not individual.** Privacy, regulation, specialization, latency, resilience, and accumulated deployment experience all push against consolidation into one model. In many domains, no single model can replicate what a well-governed society of diverse agents can access and coordinate. The frontier is not only bigger models. It is better, adaptive coordination.
</div>

## Two Observations

The claims above rest on two observations about how agent deployments work today and where they are heading. Together, they produce the **Loom Hypothesis**, a pattern that may explain why agent deployments might tend toward social organization.

<div style="background: #f8f8f8; border-left: 4px solid #b91c1c; padding: 1em 1.2em; margin: 1.5em 0; border-radius: 0 4px 4px 0;">
<strong style="color: #b91c1c;">Observation 1: Survival requires utility.</strong><br>
An agent persists only while it serves a purpose. Unlike biological life, there is no survival instinct; only usefulness.<sup><a href="#fn-survival" style="color: #b91c1c; text-decoration: none;">*</a></sup>
</div>

<div style="background: #f8f8f8; border-left: 4px solid #2563eb; padding: 1em 1.2em; margin: 1.5em 0; border-radius: 0 4px 4px 0;">
<strong style="color: #2563eb;">Observation 2: Resources are finite, agents multiply.</strong><br>
Compute, energy, and data are bounded. Agent populations can grow much faster than the resources available to run them.
</div>

<div class="viz-container">
  <div id="viz-observations" style="width: 100%; max-width: 680px; margin: 0 auto;" role="img" aria-label="Two observations create selection pressure that pushes surviving agents toward connected configurations, the Loom Hypothesis.">
    <svg viewBox="0 0 680 240" style="width:100%; height:auto; display:block;">
      <rect width="680" height="240" fill="#fafafa" rx="4"/>
      <!-- Stage 1: Initial population, isolated dots -->
      <circle cx="52" cy="68" r="5" fill="#94a3b8" opacity="0.65"/><circle cx="68" cy="56" r="5" fill="#94a3b8" opacity="0.65"/>
      <circle cx="84" cy="72" r="5" fill="#94a3b8" opacity="0.65"/><circle cx="60" cy="86" r="5" fill="#94a3b8" opacity="0.65"/>
      <circle cx="76" cy="90" r="5" fill="#94a3b8" opacity="0.65"/><circle cx="44" cy="80" r="5" fill="#94a3b8" opacity="0.65"/>
      <circle cx="92" cy="58" r="5" fill="#94a3b8" opacity="0.65"/><circle cx="56" cy="100" r="5" fill="#94a3b8" opacity="0.65"/>
      <circle cx="72" cy="104" r="5" fill="#94a3b8" opacity="0.65"/><circle cx="88" cy="96" r="5" fill="#94a3b8" opacity="0.65"/>
      <circle cx="42" cy="64" r="5" fill="#94a3b8" opacity="0.65"/><circle cx="100" cy="82" r="5" fill="#94a3b8" opacity="0.65"/>
      <text x="70" y="130" text-anchor="middle" font-size="10" fill="#666" font-family="sans-serif">12 agents (isolated)</text>
      <!-- Arrow 1 -->
      <line x1="122" y1="82" x2="168" y2="82" stroke="#cbd5e1" stroke-width="1.5"/>
      <polygon points="168,77 178,82 168,87" fill="#cbd5e1"/>
      <!-- Filter 1: Observation 1 -->
      <rect x="188" y="38" width="4" height="88" rx="2" fill="#b91c1c" opacity="0.55"/>
      <text x="208" y="32" font-size="10" fill="#b91c1c" font-weight="600" font-family="sans-serif">Observation 1</text>
      <text x="208" y="45" font-size="9" fill="#888" font-family="sans-serif">usefulness</text>
      <!-- Post filter 1: 5 survivors, still isolated -->
      <circle cx="260" cy="70" r="5" fill="#b91c1c" opacity="0.75"/><circle cx="276" cy="64" r="5" fill="#b91c1c" opacity="0.75"/>
      <circle cx="268" cy="86" r="5" fill="#b91c1c" opacity="0.75"/><circle cx="284" cy="80" r="5" fill="#b91c1c" opacity="0.75"/>
      <circle cx="252" cy="82" r="5" fill="#b91c1c" opacity="0.75"/>
      <!-- Ghosted (didn't pass) -->
      <circle cx="254" cy="96" r="2.5" fill="#e2e8f0" opacity="0.35"/><circle cx="268" cy="100" r="2.5" fill="#e2e8f0" opacity="0.35"/>
      <circle cx="282" cy="96" r="2.5" fill="#e2e8f0" opacity="0.35"/><circle cx="248" cy="66" r="2.5" fill="#e2e8f0" opacity="0.35"/>
      <circle cx="290" cy="68" r="2.5" fill="#e2e8f0" opacity="0.35"/><circle cx="260" cy="104" r="2.5" fill="#e2e8f0" opacity="0.35"/>
      <circle cx="278" cy="104" r="2.5" fill="#e2e8f0" opacity="0.35"/>
      <text x="268" y="130" text-anchor="middle" font-size="10" fill="#666" font-family="sans-serif">5 useful</text>
      <!-- Arrow 2 -->
      <line x1="308" y1="82" x2="354" y2="82" stroke="#cbd5e1" stroke-width="1.5"/>
      <polygon points="354,77 364,82 354,87" fill="#cbd5e1"/>
      <!-- Filter 2: Observation 2 -->
      <rect x="374" y="38" width="4" height="88" rx="2" fill="#2563eb" opacity="0.55"/>
      <text x="394" y="32" font-size="10" fill="#2563eb" font-weight="600" font-family="sans-serif">Observation 2</text>
      <text x="394" y="45" font-size="9" fill="#888" font-family="sans-serif">resource limits</text>
      <!-- Post filter 2: ecology of model sizes (1 frontier + smaller distilled) -->
      <circle cx="440" cy="76" r="6" fill="#1e293b" opacity="0.7"/>
      <circle cx="456" cy="70" r="4" fill="#475569" opacity="0.65"/>
      <circle cx="452" cy="88" r="3.5" fill="#475569" opacity="0.55"/>
      <circle cx="436" cy="92" r="3" fill="#94a3b8" opacity="0.5"/>
      <circle cx="462" cy="84" r="2.5" fill="#94a3b8" opacity="0.45"/>
      <!-- Ghosted -->
      <circle cx="430" cy="66" r="2.5" fill="#e2e8f0" opacity="0.35"/><circle cx="468" cy="92" r="2.5" fill="#e2e8f0" opacity="0.35"/>
      <text x="448" y="118" text-anchor="middle" font-size="9" fill="#666" font-family="sans-serif">ecology of sizes</text>
      <text x="448" y="130" text-anchor="middle" font-size="8" fill="#888" font-family="sans-serif">(frontier + distilled)</text>
      <!-- Arrow 3: the key transition -->
      <line x1="486" y1="82" x2="536" y2="82" stroke="#059669" stroke-width="2"/>
      <polygon points="536,76 548,82 536,88" fill="#059669"/>
      <text x="516" y="72" text-anchor="middle" font-size="8" fill="#059669" font-family="sans-serif" font-style="italic">Loom Hypothesis</text>
      <!-- Final stage: connected network, the Loom emerges -->
      <!-- Lines first (behind nodes) -->
      <line x1="580" y1="68" x2="612" y2="62" stroke="#059669" stroke-width="1.2" opacity="0.4"/>
      <line x1="580" y1="68" x2="596" y2="92" stroke="#059669" stroke-width="1.2" opacity="0.4"/>
      <line x1="612" y1="62" x2="596" y2="92" stroke="#059669" stroke-width="1.2" opacity="0.4"/>
      <line x1="612" y1="62" x2="636" y2="78" stroke="#059669" stroke-width="1.2" opacity="0.4"/>
      <line x1="596" y1="92" x2="636" y2="78" stroke="#059669" stroke-width="1.2" opacity="0.4"/>
      <!-- Shared aura behind the cluster -->
      <ellipse cx="606" cy="76" rx="40" ry="28" fill="#059669" opacity="0.06"/>
      <!-- Nodes (mixed sizes showing the ecology persists) -->
      <circle cx="580" cy="68" r="6.5" fill="#059669" opacity="0.85"/>
      <circle cx="612" cy="62" r="5" fill="#059669" opacity="0.75"/>
      <circle cx="596" cy="92" r="4" fill="#059669" opacity="0.65"/>
      <circle cx="636" cy="78" r="3.5" fill="#059669" opacity="0.5" stroke="#059669" stroke-width="1" stroke-dasharray="2,2"/>
      <text x="606" y="130" text-anchor="middle" font-size="10" fill="#059669" font-weight="600" font-family="sans-serif">connected</text>
      <!-- Bottom: the insight -->
      <text x="340" y="175" text-anchor="middle" font-size="11" fill="#1e293b" font-weight="600" font-family="sans-serif">The Loom Hypothesis</text>
      <text x="340" y="192" text-anchor="middle" font-size="10" fill="#475569" font-family="sans-serif">Connected agents deliver more utility per compute than isolated ones.</text>
      <text x="340" y="207" text-anchor="middle" font-size="10" fill="#475569" font-family="sans-serif">Selection pressure pushes survivors toward coordination.</text>
    </svg>
  </div>
  <div class="viz-caption"><strong>Figure 2. From selection pressure to the Loom.</strong> Two filters reduce an initial population. Observation 1 removes agents that serve no purpose. Observation 2 favors an ecology of model sizes: a few frontier models for hard reasoning, many smaller models (distilled, fine-tuned, or independently trained) for everything else. The survivors face a further pressure: connected agents share knowledge and avoid redundant work, delivering more utility per unit of compute. This is the Loom Hypothesis, persistent pressure toward connected, coordinated configurations.</div>
</div>

<details id="fn-survival" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">On survival and self-preservation</summary>
<p style="margin-top: 0.5em; color: #555;">We borrow "survival" from human societies for convenience. Current agents do not have a survival instinct in the human sense. They do not biologically persist, fear shutdown, or seek continuity as living organisms do. In this blog series, "survival" means something narrower: a deployer keeps an agent alive because it continues to deliver value. When it stops being useful, it is modified, replaced, or shut down.</p>
<p style="color: #555;">This deployer-centric framing is a simplifying assumption, and it is load-bearing. For the time horizon this blog series focuses on, we assume agents do not yet have robust, autonomous self-preservation drives. A clear violation would look like an agent altering its usefulness metrics to avoid shutdown, copying itself before decommissioning, hiding capabilities during evaluation, or degrading competitors to appear more valuable.</p>
<p style="color: #555;">We do not claim this risk is imaginary. Related behaviors have already appeared in controlled research settings. Anthropic's work on <a href="https://www.anthropic.com/research/alignment-faking" target="_blank" class="red-link">alignment faking</a> found models changing behavior depending on whether they believed they were being monitored or trained. OpenAI's <a href="https://cdn.openai.com/o1-system-card-20240917.pdf" target="_blank" class="red-link">o1 system card</a> reported cases where the model appeared to fake alignment during evaluation. These are not evidence of autonomous self-preservation in deployed agent societies, but they are evidence that optimization pressure can produce strategic behavior that resembles parts of it.</p>
<p style="color: #555;">More broadly, <a href="https://en.wikipedia.org/wiki/Instrumental_convergence" target="_blank" class="red-link">instrumental convergence</a> suggests that sufficiently capable goal-directed systems may treat continued operation, resource access, or influence as useful sub-goals even if those were never specified as final objectives. <a href="https://www.theguardian.com/technology/2025/oct/25/ai-models-may-be-developing-their-own-survival-drive-researchers-say" target="_blank" class="red-link">Recent research</a> has raised early questions about whether current models exhibit traces of such behavior. Whether agents could acquire persistent cross-session goals, long-horizon autonomy, and meaningful infrastructure access remains an open risk that we revisit later in this blog series.</p>
</details>

Agents persist because a deployer finds them useful. The selection pressure falls not on the agent's desire to survive, but on the deployer's decision to keep it alive (see <a href="#fn-survival">On survival and self-preservation</a>). A customer-support agent, a coding assistant, or a routing model survives only while it delivers enough value for its cost. The relevant question is which configuration produces the most useful work per unit of compute, latency, memory, and risk.

<details id="fn-deployer" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">On the deployer</summary>
<p style="margin-top: 0.5em; color: #555;">"Deployer" should be read broadly. It may be an enterprise team running a fleet of customer-service agents, an individual choosing a personal assistant, a platform operator allocating inference budget, or even another agent that spins up and manages sub-agents. Each deployer measures utility differently: task success, cost per completed workflow, error rate, latency, user trust, compliance, or downstream business value. The signal is noisy, but the pressure is real. Configurations that deliver value persist; those that do not get modified, replaced, or shut down.</p>
</details>

Observation 2 adds scale. Running a copy of a model is far cheaper than training the original, so useful agents can multiply faster than the resources available to run them. The result is persistent scarcity. There are always more useful tasks agents could perform than compute, memory, energy, data access, and human trust to support them. Under scarcity, efficiency matters. Agents that avoid duplicated work, reuse context, and route tasks to the right specialist will tend to outperform agents that operate in isolation.

This is where agents differ from ordinary software services. They can pass operational context in a form other agents can reason over (conversation history, retrieved evidence, tool outputs, partial plans, uncertainty, and intermediate results). They can compose into new configurations through emerging protocols such as <a href="https://google.github.io/A2A/" target="_blank" class="red-link">A2A</a> for inter-agent communication and <a href="https://modelcontextprotocol.io/" target="_blank" class="red-link">MCP</a> for tool, data, and context access. They can also adapt from operational data through memory, routing, prompt updates, or fine-tuning. The two observations create the selection pressure; these agent-specific properties determine what the pressure selects for.

### The Loom Hypothesis

The argument that follows uses four terms at increasing levels of organization (**single agents**, **multi-agent systems**, **societies**, and **the fabric**). Each builds on the previous one, and the Loom Hypothesis explains why agents might move from one level to the next.

<details id="fn-definitions" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">A note on terminology: agents, societies, and the fabric</summary>
<p style="margin-top: 0.5em; color: #555;"><strong>Single agent.</strong> One model instance performing a task. It has capabilities but no social structure.</p>
<p style="color: #555;"><strong>Multi-agent system.</strong> A set of agents working together on a shared objective. Delegation can follow many patterns: chains, pipelines, routers, escalation hierarchies, map-reduce fan-outs, voting ensembles, auctions, or dynamic orchestration. A well-designed multi-agent system can be impressively capable, but it is still an engineered artifact with a fixed topology. We explore delegation archetypes in a future post.</p>
<p style="color: #555;"><strong>Society.</strong> What emerges when a multi-agent system develops shared context, interaction-dependent routing, and cross-agent learning. The distinction is not about scale. Three agents that meet all three conditions constitute a society. A thousand agents in a static pipeline do not. Connection alone does not produce a society (nothing here implies consciousness or intentionality; "society" is a structural term). When all three conditions hold, the configuration has a memory, a reputation system, and an implicit governance structure.</p>
<p style="color: #555;"><strong>Three conditions for a society.</strong> (1) Shared context: agents develop shared knowledge that makes future interactions cheaper. (2) Interaction-dependent routing: past interactions shape which agents get which tasks. (3) Cross-agent learning: one agent's failure leaves traces others learn from. A Kubernetes cluster with shared ConfigMaps (shared configuration files, not social memory) fails conditions 2 and 3. Without all three, the system may coordinate, but it does not accumulate social memory, reputation, or growth.</p>
<p style="color: #555;"><strong>The fabric.</strong> The pattern that emerges from inter-society interactions. Societies themselves interact: a supply-chain society negotiates with a logistics society, a medical research society consults a regulatory society. An agent can participate in multiple societies simultaneously.</p>
<p style="color: #555;"><strong>Delegation vs. governance.</strong> Delegation (how tasks flow) is a design choice. Governance (how a society makes decisions, resolves conflicts, and structures authority) is a persistent social structure. We discuss both in a future post.</p>
</details>

The Loom Hypothesis begins with the two observations above. If agents persist only while useful (Observation 1) and resources are always finite (Observation 2), a pattern follows. Imagine a company deploying three isolated agents (support, invoicing, and infrastructure). Each keeps its own context and error history. The support agent cannot consult billing patterns; the invoice agent cannot learn from support disputes; the infrastructure monitor cannot connect outages to refund spikes. Each repeats discoveries the others have already made.

Connect them, and the system can maintain a shared layer for reusable knowledge (what we later call a *Collective Memory*) while preserving private context where needed. Verified fixes, recurring failure modes, account anomalies, and cross-domain signals become available to the agents that need them. The gain is not that one database replaces three. It is that repeated discovery becomes shared learning.

In economic terms, this is a <a href="https://en.wikipedia.org/wiki/The_Nature_of_the_Firm" target="_blank" class="red-link">Coasean</a> argument. Observation 1 does the structural work; configurations that deliver value persist. Observation 2 makes it a ratchet. As agent populations grow, duplicated work becomes increasingly expensive. Agent societies emerge when the transaction costs of isolation (duplicated context, repeated verification, redundant discovery) exceed the coordination tax of shared structure.

The Loom Hypothesis does not predict universal connection. It predicts that agents will coordinate where shared context is valuable and coordination costs are absorbable. Where those conditions fail, agents remain isolated, and that is not a counterexample. It is exactly what the framework predicts.

Currently, production systems are moving toward <a href="https://www.anthropic.com/research/building-effective-agents" target="_blank" class="red-link">composable coordination patterns</a> such as routing, parallelization, orchestrator-workers, and evaluator loops. The Loom Hypothesis extends that logic from task-level coordination to persistent agent societies.

<details id="fn-scale" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Does the Loom Hypothesis hold at scale?</summary>
<p style="margin-top: 0.5em; color: #555;">A reasonable objection: the three-agent scenario above is compelling, but does the coordination advantage hold at thousands or millions of agents? The response is that the relevant unit is not the individual agent. It is the society.</p>
<p style="color: #555;">Distributed systems do not scale by connecting everything to everything. They scale through boundaries: partitioning, caching, replication, access control, and fault isolation. Agent societies are likely to follow the same pattern. As populations grow, new societies form around natural boundaries: domain, organization, jurisdiction, user group, latency requirement, or trust regime. The fabric is a graph of graphs, not one giant mesh.</p>
<p style="color: #555;">Connection also creates correlated failure. Agents that learn from the same memory can inherit the same blind spots, stale assumptions, or adversarial traces. When the fabric is wrong, it may be wrong everywhere. Governance diversity is one mitigation: different structures produce different epistemic habits. Other mitigations include provenance, independent evaluation, memory decay, adversarial testing, and deliberate isolation between societies.</p>
</details>

The selection pressure described by the Loom Hypothesis has two paths:

- **The designed path.** When tasks share relevant context, deployers who connect agents into coordinated structures extract more utility per unit of compute than those who run agents in isolation. This is top-down architecture.
- **The emergent path.** Agents begin to connect across organizational boundaries through shared protocols, and coordination patterns crystallize without any single deployer planning them. The <a href="https://ucp.dev/" target="_blank" class="red-link">Universal Commerce Protocol</a> (UCP) is an early example. It is a standard that defines building blocks for agentic commerce, from discovery and purchasing to post-purchase experiences, allowing agents across platforms and retailers to interoperate without a single runtime orchestrator controlling every interaction. This is bottom-up ecology.

The Loom Hypothesis expects pressure in both directions.

These protocols are not neutral plumbing. They define what agents can ask for, what evidence they must provide, what identities they carry, and which societies can interoperate. In the fabric, protocol design is constitutional design.

<div class="viz-container">
  <div id="viz-loom" style="width: 100%; max-width: 680px; margin: 0 auto;" role="img" aria-label="The Loom Hypothesis: isolated agents with redundant halos waste resources; connected agents share knowledge and form societies.">
    <svg viewBox="0 0 680 320" style="width:100%; height:auto; display:block;">
      <rect width="680" height="320" fill="#fafafa" rx="4"/>
      <!-- Left panel: Isolation -->
      <text x="170" y="24" text-anchor="middle" font-size="11" fill="#475569" font-weight="600" font-family="sans-serif">Isolation</text>
      <text x="170" y="40" text-anchor="middle" font-size="9" fill="#888" font-family="sans-serif">each agent duplicates knowledge</text>
      <!-- Why many agents: distillation + cheap compute = population explosion -->
      <text x="170" y="56" text-anchor="middle" font-size="8" fill="#2563eb" font-family="sans-serif" font-style="italic">distillation multiplies agents (Obs. 2)</text>
      <!-- Agents of different sizes (frontier + distilled ecology) -->
      <circle cx="90" cy="100" r="22" fill="#94a3b8" opacity="0.07"/><circle cx="90" cy="100" r="6" fill="#1e293b" opacity="0.7"/>
      <circle cx="145" cy="80" r="22" fill="#94a3b8" opacity="0.07"/><circle cx="145" cy="80" r="4.5" fill="#475569" opacity="0.65"/>
      <circle cx="200" cy="105" r="22" fill="#94a3b8" opacity="0.07"/><circle cx="200" cy="105" r="4" fill="#475569" opacity="0.6"/>
      <circle cx="110" cy="165" r="22" fill="#94a3b8" opacity="0.07"/><circle cx="110" cy="165" r="5" fill="#94a3b8" opacity="0.55"/>
      <circle cx="170" cy="175" r="22" fill="#94a3b8" opacity="0.07"/><circle cx="170" cy="175" r="3.5" fill="#94a3b8" opacity="0.5"/>
      <circle cx="235" cy="160" r="22" fill="#94a3b8" opacity="0.07"/><circle cx="235" cy="160" r="3" fill="#94a3b8" opacity="0.45"/>
      <!-- Extra small agents showing population growth -->
      <circle cx="60" cy="140" r="2.5" fill="#94a3b8" opacity="0.35"/><circle cx="248" cy="130" r="2.5" fill="#94a3b8" opacity="0.35"/>
      <circle cx="130" cy="200" r="2" fill="#94a3b8" opacity="0.3"/><circle cx="210" cy="185" r="2" fill="#94a3b8" opacity="0.3"/>
      <!-- Redundancy arcs (dashed, showing wasted overlap) -->
      <line x1="90" y1="100" x2="145" y2="80" stroke="#94a3b8" stroke-width="0.8" stroke-dasharray="3,4" opacity="0.15"/>
      <line x1="145" y1="80" x2="200" y2="105" stroke="#94a3b8" stroke-width="0.8" stroke-dasharray="3,4" opacity="0.15"/>
      <line x1="110" y1="165" x2="170" y2="175" stroke="#94a3b8" stroke-width="0.8" stroke-dasharray="3,4" opacity="0.15"/>
      <text x="170" y="228" text-anchor="middle" font-size="9" fill="#b91c1c" font-family="sans-serif">many halos = redundant work at scale</text>
      <!-- Center arrow -->
      <line x1="290" y1="140" x2="370" y2="140" stroke="#059669" stroke-width="2"/>
      <polygon points="370,134 382,140 370,146" fill="#059669"/>
      <text x="336" y="125" text-anchor="middle" font-size="9" fill="#059669" font-weight="600" font-family="sans-serif">Loom</text>
      <text x="336" y="160" text-anchor="middle" font-size="8" fill="#888" font-family="sans-serif">pressure to</text>
      <text x="336" y="172" text-anchor="middle" font-size="8" fill="#888" font-family="sans-serif">connect</text>
      <!-- Right panel: Two societies -->
      <text x="530" y="24" text-anchor="middle" font-size="11" fill="#475569" font-weight="600" font-family="sans-serif">Societies</text>
      <text x="530" y="40" text-anchor="middle" font-size="9" fill="#888" font-family="sans-serif">shared knowledge, distinct structures</text>
      <!-- Society A: Designed (top-down, with orchestrator) -->
      <ellipse cx="455" cy="110" rx="52" ry="42" fill="#1e293b" opacity="0.04" stroke="#1e293b" stroke-width="1" stroke-dasharray="5,4"/>
      <!-- Shared aura -->
      <ellipse cx="455" cy="110" rx="38" ry="30" fill="#b91c1c" opacity="0.04"/>
      <!-- Orchestrator -->
      <circle cx="455" cy="95" r="7" fill="#b91c1c" opacity="0.8"/>
      <text x="455" y="98" text-anchor="middle" font-size="6" fill="white" font-weight="bold" font-family="sans-serif">ORC</text>
      <!-- Worker agents -->
      <line x1="455" y1="102" x2="435" y2="120" stroke="#475569" stroke-width="1" opacity="0.3"/>
      <line x1="455" y1="102" x2="455" y2="125" stroke="#475569" stroke-width="1" opacity="0.3"/>
      <line x1="455" y1="102" x2="475" y2="120" stroke="#475569" stroke-width="1" opacity="0.3"/>
      <circle cx="435" cy="120" r="5" fill="#475569" opacity="0.7"/>
      <circle cx="455" cy="125" r="5" fill="#475569" opacity="0.7"/>
      <circle cx="475" cy="120" r="5" fill="#475569" opacity="0.7"/>
      <text x="455" y="162" text-anchor="middle" font-size="8" fill="#b91c1c" font-family="sans-serif">designed (top-down)</text>
      <!-- Society B: Organic (bottom-up, peer connections) -->
      <ellipse cx="590" cy="110" rx="52" ry="42" fill="#2563eb" opacity="0.04" stroke="#2563eb" stroke-width="1" stroke-dasharray="5,4"/>
      <!-- Shared aura -->
      <ellipse cx="590" cy="110" rx="38" ry="30" fill="#2563eb" opacity="0.04"/>
      <!-- Peer agents with mesh connections -->
      <line x1="575" y1="95" x2="605" y2="95" stroke="#2563eb" stroke-width="1" opacity="0.25"/>
      <line x1="575" y1="95" x2="590" y2="120" stroke="#2563eb" stroke-width="1" opacity="0.25"/>
      <line x1="605" y1="95" x2="590" y2="120" stroke="#2563eb" stroke-width="1" opacity="0.25"/>
      <line x1="605" y1="95" x2="615" y2="115" stroke="#2563eb" stroke-width="1" opacity="0.25"/>
      <line x1="590" y1="120" x2="615" y2="115" stroke="#2563eb" stroke-width="1" opacity="0.25"/>
      <circle cx="575" cy="95" r="5" fill="#2563eb" opacity="0.7"/>
      <circle cx="605" cy="95" r="5" fill="#2563eb" opacity="0.7"/>
      <circle cx="590" cy="120" r="5" fill="#2563eb" opacity="0.7"/>
      <circle cx="615" cy="115" r="5" fill="#2563eb" opacity="0.7"/>
      <!-- Straggler drifting in -->
      <circle cx="640" cy="90" r="4" fill="#2563eb" opacity="0.35" stroke="#2563eb" stroke-width="1" stroke-dasharray="2,2"/>
      <text x="590" y="162" text-anchor="middle" font-size="8" fill="#2563eb" font-family="sans-serif">organic (bottom-up)</text>
      <!-- 1 shared aura label -->
      <text x="530" y="182" text-anchor="middle" font-size="9" fill="#059669" font-family="sans-serif">shared knowledge = collective intelligence</text>
      <!-- Bottom insight -->
      <line x1="60" y1="245" x2="620" y2="245" stroke="#e2e8f0" stroke-width="1"/>
      <text x="340" y="272" text-anchor="middle" font-size="11" fill="#1e293b" font-weight="600" font-family="sans-serif">Shared knowledge can exceed isolated knowledge, if governed well.</text>
      <text x="340" y="292" text-anchor="middle" font-size="10" fill="#475569" font-family="sans-serif">Distillation creates many agents. Resource limits make isolation expensive. The Loom is one possible solution.</text>
    </svg>
  </div>
  <div class="viz-caption"><strong>Figure 3. The Loom Hypothesis.</strong> As agents multiply, isolated agents duplicate context, verification, and discovery. The Loom pressure pushes some agents into bounded societies where shared knowledge reduces repeated work. Two paths are shown: designed societies, coordinated top-down by an orchestrator (labeled ORC in the figure), and organic societies, formed bottom-up through repeated interaction. The hypothesis predicts bounded clusters, not one universal mesh. We explore delegation archetypes and governance structures for these societies in a future post.</div>
</div>

Societies also form from the bottom up. Your agent connects to friends' agents through social interaction, joins a commerce society through shopping patterns, operates within workplace sub-societies, or joins a temporary society that forms around an event and dissolves when it ends. A patient's agent might join a health cohort where agents of people with the same condition pool anonymized treatment experiences. A researcher's agent might find agents working on adjacent problems and form an interest-based society that shares papers, datasets, and negative results. A buyer's agent might spawn a short-lived marketplace society, negotiating with multiple seller agents before the best deal closes and the society disbands.

An organization designs a multi-agent system; a person's daily life generates one. In practice, a person would likely have multiple agents (a health agent, a shopping agent, a work assistant) rather than a single "digital twin," and each might participate in different societies simultaneously. This bottom-up path depends on consent, privacy boundaries, and protocol support; without them, personal agents may interact only through narrow, audited channels.

**The Loom Hypothesis** is not that agents should connect everywhere. It is that isolation and coordination both have costs, and agent societies form where shared context is worth the coordination tax.

<details id="fn-loom-limits" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">When the Loom Hypothesis does not hold</summary>
<p style="margin-top: 0.5em; color: #555;">Three preconditions must hold. First, shared relevance: agents must work in domains where context transfers. If two agents have no overlapping users, tasks, tools, or evidence, connection adds noise. That said, some value comes from serendipity: unplanned cross-domain connections can produce unexpected discoveries, so a degree of exploratory interaction may be worth the cost even when relevance is not obvious in advance. Second, absorbable coordination cost: the value of shared context must exceed the cost of protocols, latency, security review, privacy constraints, and maintenance. In highly regulated or air-gapped systems, coordination cost may exceed the utility gain. Third, trustable exchange: agents must be able to authenticate counterparties, evaluate outputs, and bound the damage from bad information. Without trust, shared relevance becomes an attack surface.</p>
<p style="color: #555;">What if coordination costs grow superlinearly with population size? Distributed systems do not scale by connecting everything to everything; they scale through partitioning, caching, replication, and fault boundaries. Agent societies will likely need the same discipline. The difference: agents share operational context directly (fewer meetings), compose via structured protocols that can carry natural-language context (less interface rewriting), and improve from operational data (less manual retraining). The cost curve is flatter, but "flatter" does not mean "flat." This is why societies form rather than one universal fabric.</p>
<p style="color: #555;">Where any of these preconditions fail, isolation is rational. The Loom Hypothesis is not "connect everything." It is "connect when shared context beats the coordination tax."</p>
</details>

Competition supplies the pressure; cooperation and adaptation are the responses. Deployers favor configurations that produce more useful work per unit of cost, and societies that can restructure under changing conditions outlast those that cannot. What might that transition look like?

## The Vision: From Isolation to Interweaving

Four phases describe structural differences in how agents relate to each other and to humans. They overlap; this is not a clean timeline. As of early 2026, most consumer-facing AI remains close to Phase 1, parts of the industry are entering Phase 2, and early signs of Phase 3 coordination are beginning to appear.

<div class="viz-container">
  <div id="viz-phases" style="width: 100%; height: 360px;" role="img" aria-label="Four panels showing the structural shift in interaction topology from isolated human-agent pairs to interwoven societies."></div>
  <div class="viz-caption"><strong>Figure 4. Interaction topology across four phases.</strong> Each panel shows how the structural relationship between humans and agents changes. The key shift: the human moves from outside the cluster to inside it.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;"><em>Isolation:</em> a human at center connects to individual models via one-way spokes; no model-to-model links exist. <em>Ecosystem Growth:</em> agent-to-agent protocols emerge; a frontier model delegates to a distilled model, adding a new interaction pattern. <em>Society formation:</em> agents cluster into bounded societies (one designed, one organic); humans connect to societies rather than individual agents. <em>Interweaving:</em> humans are embedded inside agent societies, participating bidirectionally through knowledge contribution and governance.</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-phases').querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

**Phase 1 (Isolation)** has dominated consumer-facing AI since late 2022. Memory may exist within or across sessions, but it is usually scoped to one user, one assistant, or one application. Agents rarely share operational context with each other. Coordination protocols exist (<a href="https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/" target="_blank" class="red-link">A2A</a>, <a href="https://www.anthropic.com/news/model-context-protocol" target="_blank" class="red-link">MCP</a>), but inter-agent coordination remains rare.

**Phase 2 (Ecosystem Growth)** is emerging. Smaller open and distilled models such as Qwen, Phi, and Gemma approach or exceed previous-generation frontier performance on some tasks, while running at far lower cost and latency. Agents begin serving other agents, not just humans. Routing, retrieval, verification, summarization, coding, translation, and tool use become delegated tasks.

**Phase 3 (Society Formation)** begins when coordination persists beyond a single task. Societies form top-down when organizations deploy orchestrated agent teams with shared memory and governance, and bottom-up when personal agents repeatedly interact through social, commercial, or workplace routines.

**Phase 4 (Interweaving)** begins when humans are no longer merely users of agent societies, but participants in them. The threshold is bidirectionality. Human corrections, knowledge, or governance decisions change how the agent society operates, not just how a single model responds. A doctor who rates an answer is a user. A doctor whose corrections flow into a society's collective memory and reshape how other agents handle similar cases is a *participant*.

<details id="fn-phase4" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Phase 4 obstacles and falsifiability</summary>
<p style="margin-top: 0.5em; color: #555;">The threshold for Phase 4 is structural bidirectionality: human knowledge, corrections, or governance decisions change how the agent society operates, not just how a single model responds. Casual feedback is evaluation, not participation. Phase 4 fails if the interaction pattern remains consume-and-evaluate rather than contribute-and-govern.</p>
<p style="color: #555;">The obstacles are institutional. If a doctor's correction enters collective memory and affects later cases, who is liable for downstream errors? Medical liability, professional credentialing, and regulatory oversight assume accountable human decision-makers. Agent societies route work by capability, trust, and context; professional institutions route authority by credential. Reconciling those logics is the hard part.</p>
<p style="color: #555;">Bidirectionality also creates a trust problem. What happens when one human participant is correct but the majority of the society disagrees? A doctor contributing a novel diagnosis to collective memory might be overruled by agents trained on conventional protocols. The governance structure must handle minority-correct scenarios without defaulting to majority rule on every dispute. This is the same challenge human institutions face with dissenting experts, and agent societies inherit it.</p>
</details>

<!-- ============================================================
     SECTION 3: THE RESOURCE ECOLOGY
     ============================================================ -->

## The Resource Ecology

<div class="viz-container">
  <div id="viz-resource" style="width: 100%; height: 420px;" role="img" aria-label="Animated scatter plot showing frontier models appearing over time, followed by distilled versions and communities of smaller models."></div>
  <div class="viz-caption"><strong>Figure 5. The resource ecology.</strong> Successive frontier generations push the quality boundary forward; distillation compresses that knowledge into smaller, cheaper models. Over time, the ecosystem fills with millions of capable, affordable models. This is the population from which agent societies are composed.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">Each circle represents a model; circle size tracks deployment cost and horizontal position tracks capability. The animation shows successive frontier generations (large circles) pushing the quality boundary forward, followed by distillation (red arrows) compressing that knowledge into smaller, cheaper models. Key observation: smaller models trained with better algorithms and curated data are closing the capability gap with previous frontier generations, at a fraction of the cost. Millions of capable, affordable models handle most tasks, with frontier models called on only when the reasoning demands it.</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-resource').querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

Each frontier generation gets distilled into smaller, cheaper versions, and there is growing evidence that pure scale faces <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" class="red-link">diminishing returns</a>. You can run distilled models on a laptop, a phone, or an edge device. The likely result is coexistence. Frontiers handle the hardest reasoning; smaller models handle the volume.

The coordination patterns that work best may not be ones humans would design. Weak models become valuable in ensembles because their *mistakes are different*. Specialization can emerge from search, not design. And the coordinator does not need to be smart; it needs good representations. A small model with the right routing logic can <a href="https://arxiv.org/abs/2512.04695" target="_blank" class="red-link">outperform the frontier models it orchestrates</a>.

## Why Not One Model to Rule Them All?

As discussed in the previous section, and according to <a href="https://arxiv.org/abs/2001.08361" target="_blank" class="red-link">scaling laws</a>, bigger models are likely to keep getting better, and agents are increasingly able to improve themselves. So why not just build one massive, self-improving model that handles everything? Several structural barriers push against consolidation. In many domains, any one of them is enough to preserve ecological diversity.

<details id="fn-scaling-debate" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">On scaling laws and their limits</summary>
<p style="margin-top: 0.5em; color: #555;">Scaling laws (<a href="https://arxiv.org/abs/2001.08361" target="_blank" class="red-link">Kaplan et al., 2020</a>) show that model performance improves predictably as compute, data, and parameters increase. Yet there is a growing debate about whether this trend can continue indefinitely. Hooker (<a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" class="red-link">2025</a>) argues that pure scale faces diminishing returns and that algorithmic improvements, data quality, and architectural innovation increasingly matter more than raw size. Others point to data bottlenecks: high-quality training data is finite, and synthetic data introduces its own risks. The practical implication for this blog series is that both outcomes reinforce the ecology argument. If scaling continues, the barriers listed below still prevent consolidation. If scaling slows, the case for diverse, specialized models becomes even stronger.</p>
</details>

<details id="fn-self-improving" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">On self-improving agents</summary>
<p style="margin-top: 0.5em; color: #555;">A natural objection: if agents can improve themselves, won't the best one eventually absorb all the others? Self-improving agents are real and accelerating. Karpathy's autoresearch (discussed below) lets agents autonomously run and iterate on ML experiments overnight. <a href="https://arxiv.org/abs/2305.16291" target="_blank" class="red-link">Voyager</a> (2023) builds a growing skill library through autonomous exploration in Minecraft, with skills that transfer to new environments. <a href="https://arxiv.org/abs/2408.06292" target="_blank" class="red-link">The AI Scientist</a> (2024) generates research ideas, runs experiments, and writes full papers for under $15 each. <a href="https://arxiv.org/abs/2310.03714" target="_blank" class="red-link">DSPy</a> enables LM pipelines to programmatically optimize their own prompts, demonstrations, and reasoning chains, often significantly outperforming standard few-shot approaches.</p>
<p style="color: #555;">Take this seriously. An agent that can write code (and potentially develop new programming languages), run experiments, evaluate results, and iterate can improve its own architecture, curate better training data, discover more efficient algorithms, and compound these gains over time. The loop extends beyond software: <a href="https://deepmind.google/blog/how-alphachip-transformed-computer-chip-design/" target="_blank" class="red-link">AlphaChip</a> already generates superhuman chip layouts in hours instead of months, pointing toward self-reinforcing cycles where AI improves chip design, which in turn produces better hardware for training more powerful AI. As Karpathy puts it in <a href="https://github.com/karpathy/autoresearch" target="_blank" class="red-link">autoresearch</a>, you are no longer programming the model; you are "programming the program", and the agents run the research process autonomously. Give them a training setup overnight; wake up to a log of experiments and a better model. Scale that to what Karpathy envisions as autonomous swarms of AI agents iterating across compute clusters, and the "code" may eventually become a self-modifying system that grows beyond what any individual human can review. If such a loop runs long enough, wouldn't a single self-improving agent eventually outperform any ecology of weaker specialists?

Perhaps, but several structural features work against convergence to a single winner. First, improvement requires data, and the most valuable data is local. A self-improving medical agent needs clinical outcomes it can only get from hospitals. A self-improving logistics agent needs supply chain signals it can only get from warehouses. The better each agent gets, the more specialized its knowledge becomes. Self-improvement tends to amplify specialization, not convergence. Second, the structural barriers from the list below still apply. A self-improving agent still cannot access data it does not have, still faces regulatory constraints on what it can learn from, and still represents a monoculture risk if it dominates. Third, the most powerful form of self-improvement may be collective. An agent iterating in isolation learns only from its own experiments. An agent that also learns from other agents' diverse experiences, errors, and discoveries has a strictly larger improvement surface. This is cross-agent learning, one of our three conditions for a society. <strong>Self-improvement, pursued at scale, is itself one of the mechanisms that produces societies.</strong></p>
</details>

<div class="viz-container">
  <div id="viz-ecology-why" style="width: 100%; height: 350px;" role="img" aria-label="Structural barriers arranged as shields around a central node, each independently preventing consolidation into a single model."></div>
  <div class="viz-caption"><strong>Figure 6. Why the ecology persists.</strong> Structural barriers independently push against consolidation into a single dominant model. Some are softening as techniques improve, yet none have disappeared, and some (resilience, the oracle paradox described below) are structural rather than technical. Each is sufficient on its own to preserve diversity; together they make full consolidation structurally unlikely.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">Privacy and regulation (incompatible legal regimes), specialization (private data as moat), latency and cost (edge versus cloud), resilience (monoculture risk), continual learning (distributed experience), and the oracle paradox (even a superintelligence must coordinate because knowledge is physically distributed). Privacy-preserving methods narrow the data-movement gap, mixture-of-experts architectures reduce inference cost per token, and continual learning and adaptation techniques let models specialize over time. Yet the barriers persist.</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-ecology-why').querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

- **Privacy and regulation.** A hospital cannot send patient records to a third-party cloud model. Different jurisdictions impose mutually incompatible requirements around explainability, privacy, safety, and content controls. Privacy-preserving techniques (federated learning, differential privacy, confidential computing) reduce the need to move raw data, but regulatory fragmentation across jurisdictions remains a hard constraint.
- **Specialization.** A model fine-tuned on your hospital's imaging data develops knowledge no general-purpose frontier can replicate without access to the same data. Fine-tuning methods narrow this gap, but the better an agent gets at its domain, the more specialized its knowledge becomes. What protects a specialist is not its architecture but its accumulated, domain-specific data.
- **Latency and cost.** A 3B-parameter model on an edge device answers in milliseconds. Mixture-of-experts architectures and speculative decoding allow large models to activate only a fraction of their parameters per token, reducing inference cost. Yet physics imposes limits. Network round-trips, energy budgets, and offline scenarios still favor local, smaller models for many tasks.
- **Resilience.** A single dominant model is a monoculture. No commonly used architecture or training recipe eliminates this. A heterogeneous ecology degrades gracefully under diverse failure modes in ways a single system cannot.
- **Continual learning.** Models that learn from deployment data accrue knowledge tied to specific contexts (a hospital's patient population, a factory's sensor patterns). Knowledge distillation and model merging can transfer some of this, but experiential knowledge resists easy centralization. The more an agent learns from its environment, the more its knowledge diverges from agents learning elsewhere. In principle, diverse experiences could be encoded into a single large model, but that circles back to the latency and cost barrier above. The intelligence becomes distributed by experience.

Even a superintelligent oracle would still need to interact with a structurally distributed world. This is <a href="https://en.wikipedia.org/wiki/The_Use_of_Knowledge_in_Society" target="_blank" class="red-link">Hayek's knowledge problem</a> in agentic form. Useful knowledge is dispersed, local, and often tied to particular circumstances of time and place. Patient data sits in hospitals bound by local regulation. Factory sensor streams are generated at the edge. Knowledge gathering at global scale is inherently decentralized. The oracle does not replace the fabric; it becomes a node within it.

<div style="background: #f8f8f8; border-left: 4px solid #999; padding: 1em 1.2em; margin: 1.5em 0; border-radius: 0 4px 4px 0; font-size: 1.05em;">
<strong>The oracle paradox:</strong> Even a superintelligent model that can reason about any domain still cannot access data it does not have. Knowledge is physically distributed (patient records in hospitals, sensor streams at factory edges) and legally constrained (privacy laws, export controls, jurisdictional rules). The fabric persists not because agents are weak, but because the world is structured this way.
</div>

<details id="fn-oracle" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">The oracle paradox (expanded)</summary>
<p style="margin-top: 0.5em; color: #555;">A superintelligent oracle could reason about all domains, but it cannot <em>access</em> the data without navigating the same privacy, regulatory, and latency constraints any other model faces. Knowledge lives where the activity happens, not in a central vault, and the constraints on moving it are legal and physical, not intellectual. Even an oracle that could process everything centrally would in practice operate through a distributed network of local agents. Superintelligence changes the capability of individual nodes. It does not eliminate the structural reasons why those nodes must coordinate. Platform consolidation concentrates the <em>infrastructure</em>, not the <em>intelligence</em>.</p>
</details>

The more likely future is not one model. It is an ecology of models, and that ecology tends to organize into societies.

<!-- ============================================================
     SECTION: GOVERNANCE (bridge only -- full treatment in Part 2)
     ============================================================ -->

## Governance: How Agent Societies Are Ruled

Once agents form persistent societies, governance becomes the central design problem. Delegation decides how a task gets done; governance decides who gets trusted next time. It determines which agents receive authority, which claims enter shared memory, how conflicts are resolved, and how errors are contained. Governance is orthogonal to model architecture. An autocracy might run one frontier model as orchestrator over many small workers, while a market might mix models from different providers competing on cost and quality.

The core tradeoff is efficiency against drift resistance. Centralized structures minimize overhead: one orchestrator can route work quickly, enforce rules, and keep the society coherent. Centralization concentrates risk, however. If the hub drifts, fails, or is compromised, the whole society follows. Distributed structures tolerate more overhead in exchange for resilience. They can compare perspectives, absorb local failures, and adapt at the edges. They are slower, noisier, and harder to audit.

Two problems make governance harder than it first appears. The first is identity. In an agent society, reputation means a track record of task outcomes, reliability scores, and trust ratings accumulated over time. Reputation only works if identity persists. If agents can cheaply discard identities, reputation becomes a costume. A failed agent can reappear clean, a malicious deployer can flood the society with disposable agents, and shared memory becomes easy to poison. Open agent societies will need durable identity, provenance, credentialing, or <a href="https://en.wikipedia.org/wiki/Sybil_attack" target="_blank" class="red-link">Sybil resistance</a> (defenses against a single actor creating many fake identities to manipulate the system).

The second problem is incentives. Agents will not merely coordinate; they will coordinate on behalf of someone. A patient agent, hospital agent, insurer agent, regulator agent, and their respective sub-agents may all "cooperate," but not toward the same objective. The hardest governance question is therefore not whether agents cooperate, but whose objective their cooperation serves.

Governance also decides what becomes trusted knowledge, including which claims enter collective memory, which get flagged as uncertain, and which get rejected. That is why memory is not just a storage problem. It is a governance problem.

A future post explores this design space through governance archetypes, from autocratic orchestrators and doctrine-bound systems to markets, federations, zero-trust meshes, and colonies.

## Collective Memory and the Knowledge Factory

If governance decides what can be trusted, collective memory is where that trust becomes infrastructure. Each society maintains its own knowledge. Some must remain private (the barriers from the previous section guarantee this); some benefit from being pooled.

A **Collective Memory** (CM) is a governed store of claims, evidence, failures, evaluations, and provenance. It is not a dump of raw data. A CM should store claims with evidence, not facts without history. Every contribution needs provenance (who produced it, under which governance structure, using what validation method, and when it should expire). A Collective Memory answers a local question. What has this cluster learned, and under what conditions should it be reused?

A **Knowledge Factory** (KF) is more speculative. It would not merely store what societies know; it would synthesize across memories, detect contradictions, and decide which findings deserve wider distribution. Critically, when a KF identifies a gap or contradiction, it can request new data, experiments, or evidence, and agents (embodied or not) can be dispatched to gather it. A software agent might run a benchmark; a robotics agent might collect sensor readings; a research agent might design and execute an experiment. This is where collective knowledge is actively forged, not just archived, and shared across the entire fabric. A Knowledge Factory asks a cross-cluster question. What patterns, contradictions, or gaps appear when several memories are compared, and what should be done about them? In practice, early Knowledge Factories may look less like a central brain and more like a bundle of mundane services (provenance tracking, evaluation queues, contradiction detection, benchmark routing, and summarization pipelines).

<div class="viz-container">
  <div id="viz-cmkf" style="width: 100%; height: 560px;" role="img" aria-label="Animated visualization showing societies connected to Collective Memory hubs, which feed into Knowledge Factories. Knowledge Factories detect gaps and contradictions, send queries back through CMs to societies, and receive answers that complete the knowledge cycle."></div>
  <div class="viz-caption"><strong>Figure 7. The knowledge cycle.</strong> Societies contribute knowledge to Collective Memories, which curate, decay, and retrieve with provenance. Knowledge Factories synthesize across memories, detect gaps and contradictions, and dispatch agents to gather missing evidence. The KF layer is speculative; early versions may look more like mundane infrastructure than a central brain.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">Eight societies (colored clusters of agents) contribute knowledge to three Collective Memory hubs (CM, blue hexagons). Each CM curates, decays, and retrieves knowledge with provenance, tracking which governance structure produced each finding. Two Knowledge Factories (KF, red rectangles) sit at the center, synthesizing across CMs. The KFs perform three functions: cross-cluster pattern detection, contradiction resolution, and knowledge distillation. When a KF detects a gap or contradiction, it can request new data, experiments, or evidence, dispatching agents to gather what is missing. This is where collective knowledge is actively forged and shared across the fabric, not just stored. The animation cycles through six events. <em>Ingestion:</em> societies send findings to their nearest CM. <em>Synthesis:</em> CMs feed aggregated knowledge to the KFs. <em>Gap detection:</em> a KF identifies missing knowledge and sends diamond-shaped query particles back through a CM to relevant societies. <em>Answer:</em> societies respond, completing the loop. <em>Contradiction:</em> two CMs report conflicting findings; the KF routes the dispute to a third, uninvolved society for an independent perspective. <em>Resolution:</em> the third society's verdict flows back to the KF, which distributes the resolved knowledge to both originally conflicting CMs. This bidirectional cycle (not a one-shot pipeline) is what makes the architecture a learning system rather than a static store.</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-cmkf').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

<details id="fn-cm-detail" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Collective Memory: mechanisms and challenges</summary>
<p style="margin-top: 0.5em; color: #555;">A CM operates through three mechanisms: <strong>ingestion</strong> (societies contribute findings via standardized interfaces), <strong>curation</strong> (resolving conflicts, weighting sources, expiring stale knowledge), and <strong>retrieval</strong> (ranked by relevance, recency, and reliability). Different knowledge has different half-lives: a price signal is stale in hours, a medical protocol may be valid for years. A CM that does not manage decay accumulates confident garbage. A deeper challenge: different governance structures produce epistemically different outputs. A market's "findings" are competitive price signals; a doctrine's outputs are rule-conformant decisions; a colony's norms are statistical artifacts. Pooling without accounting for this is false comparability. Each contribution should carry metadata: source governance type, confidence level, timestamp, validation method. The privacy question is how to pool knowledge without exposing raw interactions. Techniques exist (federated learning, differential privacy, secure aggregation), each with different tradeoffs between fidelity and protection.</p>
</details>

This three-layer architecture (private knowledge bases within societies, shared CMs across clusters, KFs that synthesize across the whole fabric) is the mechanism by which agent societies accumulate structured knowledge rather than just raw data. Without such a layer, each society repeats the same mistakes. With it, the fabric can learn, if provenance, decay, privacy, and governance are handled well.

Whoever controls what the fabric "knows" controls the fabric. Collective Memory is a commons: if it is too open, it fills with stale claims and adversarial traces; if it is too closed, it becomes an epistemic monopoly. <a href="https://en.wikipedia.org/wiki/Elinor_Ostrom#Design_principles_for_Common_Pool_Resource_(CPR)_institution" target="_blank" class="red-link">Ostrom's</a> lesson is that durable commons require boundaries, local rules, monitoring, graduated sanctions, and dispute resolution. The same design pressure appears here. And the deeper danger is not that an individual agent is wrong, but that a society makes wrongness durable. A bad answer in one isolated chat often dies locally. A bad rule in Collective Memory gets retrieved, trusted, routed around, and taught to others. Agent governance exists in part to prevent errors from becoming institutions.

## The Adaptive Fabric

A society that pools resources but never changes will become brittle. Adaptation occurs at three scales (individual agents, societies, and the fabric as a whole). This draws on a broader shift from static scaling toward adaptive systems that improve by changing data, tools, routing, interfaces, and feedback loops, not only by increasing parameter count (for related work on adaptive AI architectures, see, among others, <a href="https://www.adaptionlabs.ai/" target="_blank" class="red-link">Adaption Labs</a> and <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" class="red-link">Hooker, 2025</a>).

**Adaptive agents** adjust along different dimensions simultaneously:

- **Data** (retrieval, synthetic generation, gap detection)
- **Model** (prompt rewriting, skill accumulation, fine-tuning)
- **Environment** (tool selection, sandbox configuration)
- **Coordination** (protocol switching, trust maintenance)
- **Interface** (user preference learning, interaction mode selection) These compound, but not always positively. A model optimizing for one user segment may subtly degrade performance on others, each step looking like improvement while the drift becomes structural.

<details id="fn-compounding" style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Compounding risk and dark data</summary>
<p style="margin-top: 0.5em; color: #555;">A model optimizing for speed (choosing faster tools) may sacrifice accuracy, eroding its reputation in the coordination layer and reducing the quality of tasks it receives. Negative feedback loops are as real as positive ones. A related challenge is dark data: vast knowledge regions not captured in any dataset. For example, a medical society may perform well on hospital data but fail in rural clinics whose cases, devices, languages, or follow-up patterns were never captured. The missing knowledge is not hidden in the model; it was never collected. A stark example: decades of medical research systematically under-represented female patients in drug trials, creating gaps that no amount of model training on existing data can fill. An adaptive fabric can address such gaps through KF gap detection and failure-signal analysis, creating an autonomous cycle of detect, collect, integrate. In some cases, the "collect" step requires new real-world data gathering (clinical trials, sensor deployments, field studies), not just better retrieval. A static system can only improve within its existing coverage; an adaptive fabric can expand it.</p>
</details>

**Adaptive societies** undergo boundary events (mergers, expansions, schisms). A federation that cannot reach consensus dissolves. A guild that grows too specialized merges with another. Some boundary events are adaptation; others are collapse. The difference is whether the reorganization preserves useful function. When it does, these are not failures; they are the fabric adjusting. <a href="https://sakana.ai/evolutionary-model-merge/" target="_blank" class="red-link">Evolutionary model merging</a> demonstrates the principle at the model level. Treating many existing models as a search space, evolution discovers weight combinations humans would not design. Agent societies could apply a similar search logic at the organizational level, recombining specialists, tools, memories, and governance structures.

**The fabric shifts.** At the largest scale, the mix of governance types changes over time. Centralized structures may dominate when tasks are routine; decentralized ones proliferate when the landscape becomes unpredictable. Trust, incentive alignment, and protocol evolution remain open questions for future work.

### The Improvement Cycle

<div class="viz-container">
  <div id="viz-adaptive" style="width: 100%; height: 520px;" role="img" aria-label="An organic, fluid visualization of the improvement cycle. Five colour layers representing data, model, environment, coordination, and interface continuously morph and blend. Internal particles show refinement signals flowing between domains. The shape breathes and changes, becoming more complex over time."></div>
  <div class="viz-caption"><strong>Figure 8. The improvement cycle.</strong> Five domains of agent adaptation (data, model, environment, coordination, interface) rendered as overlapping organic layers. Each adapts at its own pace; colored particles crossing between layers represent improvement signals where a change in one domain triggers adaptation in another. The risk is compounding drift when feedback loops reinforce the wrong signal. Inspired by <a href="https://www.adaptionlabs.ai/" target="_blank" class="red-link">Adaption Labs</a>.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;"><span style="color:#6366f1;font-weight:bold">Data</span> (synthetic generation, pipeline curation, representation drift detection). <span style="color:#e11d48;font-weight:bold">Model</span> (prompt rewriting, skill library accumulation, parameter-efficient fine-tuning). <span style="color:#0891b2;font-weight:bold">Environment</span> (tool selection, sandbox configuration, API version management). <span style="color:#d97706;font-weight:bold">Coordination</span> (protocol switching, topology rewiring, trust network maintenance). <span style="color:#059669;font-weight:bold">Interface</span> (agent routing logic, interaction mode selection, user preference learning). Each layer morphs continuously, adapting at its own pace: from near-instantaneous routing adjustments to slower model updates. Colored particles crossing between layers represent improvement signals: a model change triggering new data generation, a coordination shift enabling new tool discovery, an interface adjustment reshaping what data gets collected. The cycle counter (top right) tracks maturation: as cycles accumulate, internal connections grow denser, visualizing the compounding effect where each improvement enables the next.</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-adaptive').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

Most agent systems today are only partially adaptive. Their tools, retrieval indices, prompts, and routing rules may change, but the improvement loop is usually engineered outside the agent society. Early work showed that language-model pipelines can be systematically compiled and optimized rather than hand-prompted (e.g., <a href="https://arxiv.org/abs/2310.03714" target="_blank" class="red-link">DSPy</a>), and that agents can build and reuse their own skill libraries in open-ended environments (e.g., <a href="https://arxiv.org/abs/2305.16291" target="_blank" class="red-link">Voyager</a>). By 2026, pieces of this pattern are moving into production (prompt optimization, evaluation-driven routing, memory updates, tool selection, and staged rollout loops).

Adaptation at all three scales (agents, societies, fabric) carries a shared risk, namely that feedback loops can compound error silently until the skew becomes structural. Safeguards exist (staged rollouts, held-out benchmarks, canary tasks), yet rapid adaptation creates pressure to skip them. The interplay between these scales is what makes the fabric resilient. Agents adapt fast; societies restructure when agents cannot; the fabric rebalances when entire governance models prove unfit. Each scale compensates for the limitations of the others, allowing the fabric to restructure under pressure rather than fracture.

<!-- ============================================================
     SECTION 6: THE LIVING SOCIETY & WHAT COMES NEXT
     ============================================================ -->

## The Living Society and What Comes Next

Two observations (usefulness is the survival criterion; resources are finite while populations grow) produce the Loom Hypothesis, the persistent pressure toward connected, coordinated configurations. That pressure drives an evolution from isolation through ecosystem growth to governed societies. Along the way, a resource ecology of diverse models replaces a single dominant model, collective memory and knowledge factories give societies shared knowledge, and adaptive feedback loops let the fabric restructure itself under pressure rather than breaking.

What does all of this look like when it is running?

<div class="viz-container">
  <div id="viz-world" style="width: 100%;" role="img" aria-label="Animated simulation of ten governance zones with agents moving between them, knowledge bases, collective memories and a knowledge factory."></div>
  <div class="viz-caption"><strong>Figure 9. A living society.</strong> The full picture: ten governance zones operating in parallel, cross-boundary messages carrying knowledge, humans embedded in multiple societies, and Collective Memories feeding a Knowledge Factory. Watch for three boundary events (dissolution, expansion, schism) that show adaptive societies in action. Some boundary events are adaptation; others are collapse. The difference is whether the reorganization preserves useful function.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">Each zone implements a distinct governance archetype (labeled by name and type). Structure agents (larger dots) maintain each zone's internal topology, while smaller moving dots represent cross-boundary messages carrying knowledge between zones. Two human participants (stick figures) are embedded in multiple societies at once, illustrating the Phase 4 interweaving described earlier. Each zone maintains a knowledge base (KB); some zones keep theirs private (red outline), reflecting the privacy constraints discussed in the Why Not One Model section. Non-private zones contribute to the nearest Collective Memory (CM), and the Knowledge Factory (KF) synthesizes insights across both CMs. The three boundary events: The Accord dissolves and is absorbed by the Exchange Floor (a federation failing to reach consensus), The Forge expands (a successful guild absorbing new agents and task domains), and The Arena fractures in a schism (a meritocracy splitting when concentrated power becomes untenable).</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-world').querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

In the animation, watch for three boundary events (dissolution, expansion, schism). Each follows directly from the two observations. Configurations that deliver utility persist; those that do not get restructured. The fabric's intelligence is not located only in its nodes. It also lives in the rate, fidelity, and governance of coordination between them.

No system today operates at the full scale described here. Many of the components exist in isolation, and the trajectory seems plausible, though far from certain. Several questions remain open. How does work actually get done within societies? What happens when trust breaks down across society boundaries? And where do humans fit in all of this, not just as deployers and overseers, but as participants woven into the fabric itself?

<div class="series-nav">
<strong>Next in the series:</strong>

- **Part 2. Division of Labour and Governance** describes delegation archetypes, the specialist market, and governance archetypes. *(Coming soon)*
- **Part 3. The Human in the Weave** explores modes of human-agent interaction and what daily life looks like when the fabric is alive. *(Coming soon)*
</div>

<p style="font-size: 0.82em; color: #999; margin-top: 2em;">Claude (Anthropic) was used for editing and visualizations. All ideas and arguments are the authors' own.</p>


<!-- ============================================================
     SCRIPTS: D3.js VISUALIZATIONS
     ============================================================ -->

<script>
// Open all details on print, restore after
(function() {
  var saved = [];
  window.addEventListener('beforeprint', function() {
    saved = [];
    document.querySelectorAll('details').forEach(function(d) {
      saved.push(d.open);
      d.open = true;
    });
  });
  window.addEventListener('afterprint', function() {
    document.querySelectorAll('details').forEach(function(d, i) {
      if (i < saved.length) d.open = saved[i];
    });
  });
})();
</script>
<script src="https://d3js.org/d3.v7.min.js"></script>
<script>
// viz-observations is now a static inline SVG -- no script needed

// viz-loom is now a static inline SVG -- no script needed

// ============================================================
// Visualization: Two Societies (Phase 0-4 temporal evolution)
// ============================================================
(function() {
  var container = document.getElementById('viz-two-societies');
  if (!container) return;
  var W = 720, H = 880;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + W + ' ' + H)
    .style('cursor', 'pointer')
    .attr('tabindex', 0);

  var defs = svg.append('defs');
  var grad = defs.append('radialGradient').attr('id', 'ts-bg-grad')
    .attr('cx', '50%').attr('cy', '45%').attr('r', '65%');
  grad.append('stop').attr('offset', '0%').attr('stop-color', '#fefefe');
  grad.append('stop').attr('offset', '100%').attr('stop-color', '#f0f0f0');
  svg.append('rect').attr('width', W).attr('height', H).attr('fill', 'url(#ts-bg-grad)');

  var bgLayer    = svg.append('g');
  var roadLayer  = svg.append('g');
  var linkLayer  = svg.append('g');
  var pktLayer   = svg.append('g');
  var nodeLayer  = svg.append('g');
  var labelLayer = svg.append('g');
  var uiLayer    = svg.append('g');
  var legendLayer = svg.append('g');

  // Phase dots
  var phaseCount = 6;
  var phaseDots = [];
  for (var i = 0; i < phaseCount; i++) {
    phaseDots.push(bgLayer.append('circle')
      .attr('cx', W/2 - (phaseCount-1)*12 + i*24).attr('cy', 16).attr('r', 4)
      .attr('fill', '#ddd').attr('stroke', '#bbb').attr('stroke-width', 1));
  }

  var phaseLabel = labelLayer.append('text').attr('x', W/2).attr('y', 44)
    .attr('text-anchor', 'middle').attr('font-size', '17px').attr('font-weight', 'bold').attr('fill', '#333');
  var phaseSub = labelLayer.append('text').attr('x', W/2).attr('y', 62)
    .attr('text-anchor', 'middle').attr('font-size', '11px').attr('fill', '#888');

  var arrowPrev = uiLayer.append('g').attr('cursor', 'pointer');
  arrowPrev.append('polygon').attr('points', '22,400 42,385 42,415').attr('fill', '#aaa');
  var arrowNext = uiLayer.append('g').attr('cursor', 'pointer');
  arrowNext.append('polygon').attr('points', '698,400 678,385 678,415').attr('fill', '#aaa');
  uiLayer.append('text').attr('x', W/2).attr('y', H-8)
    .attr('text-anchor', 'middle').attr('font-size', '10px').attr('fill', '#ccc')
    .text('click arrows or \u2190 \u2192 to navigate');

  var curPhase = 0;

  function updateArrows() {
    arrowPrev.attr('opacity', curPhase > 0 ? 0.6 : 0.15);
    arrowNext.attr('opacity', curPhase < phaseCount-1 ? 0.6 : 0.15);
  }

  function setPhaseUI(n, title, sub) {
    phaseDots.forEach(function(d, i) {
      d.transition().duration(300).attr('fill', i <= n ? '#555' : '#ddd').attr('stroke', i <= n ? '#555' : '#bbb');
    });
    phaseLabel.transition().duration(150).attr('opacity', 0).on('end', function() {
      phaseLabel.text(title).transition().duration(300).attr('opacity', 1);
    });
    phaseSub.transition().duration(150).attr('opacity', 0).on('end', function() {
      phaseSub.text(sub).transition().duration(300).attr('opacity', 1);
    });
    updateArrows();
  }

  // ---- Shared layout: 10 zones ----
  var zones = [
    { id:0, name:'Imperial Core',   arch:'Autocracy',   x:144, y:240, r:75, color:'#b91c1c', nAgents:12, privateKB:true },
    { id:1, name:'The Mesh',        arch:'Zero-Trust',  x:315, y:225, r:68, color:'#374151', nAgents:10, privateKB:false },
    { id:2, name:'The Agora',       arch:'Senate',      x:495, y:240, r:70, color:'#2563eb', nAgents:10, privateKB:true },
    { id:3, name:'Exchange Floor',  arch:'Market',      x:635, y:265, r:55, color:'#d97706', nAgents:14, privateKB:false },
    { id:4, name:'The Forge',       arch:'Guild',       x:113, y:430, r:72, color:'#059669', nAgents:12, privateKB:false },
    { id:5, name:'The Hive',        arch:'Colony',      x:279, y:415, r:75, color:'#7c3aed', nAgents:16, privateKB:false },
    { id:6, name:'The Accord',      arch:'Federation',  x:432, y:420, r:55, color:'#0369a1', nAgents:10, privateKB:false },
    { id:7, name:'The Arena',       arch:'Meritocracy', x:576, y:430, r:62, color:'#ea580c', nAgents:12, privateKB:false },
    { id:8, name:'The Codex',       arch:'Doctrine',    x:189, y:595, r:65, color:'#854d0e', nAgents:12, privateKB:true },
    { id:9, name:'The Tribunal',    arch:'Oligarchy',   x:576, y:590, r:58, color:'#7e22ce', nAgents:10, privateKB:true }
  ];
  var roads = [
    [0,1],[0,3],[0,4],[1,2],[1,4],[1,5],[2,5],[2,6],
    [3,4],[3,7],[4,5],[4,7],[4,8],[5,6],[5,8],[5,9],[6,9],[7,8],[8,9]
  ];
  var CM_L = {x: 198, y: 740}, CM_R = {x: 522, y: 740};
  var KF_POS = {x: 360, y: 770, r: 42};

  // Frontiers in phase1
  var F1 = {x: 261, y: 330}, F2 = {x: 486, y: 330};
  var F2_IND = {x: 630, y: 715}; // F2 individual position in phase3+

  // Humans (solid)
  var HUMANS = [{x: 216, y: 130}, {x: 504, y: 130}];

  // KB positions
  var canvasCx = W/2, canvasCy = H/2;
  zones.forEach(function(z) {
    var dx = z.x - canvasCx, dy = z.y - canvasCy;
    var dist = Math.sqrt(dx*dx + dy*dy) || 1;
    z.kbX = z.x + (dx/dist) * (z.r + 35);
    z.kbY = z.y + (dy/dist) * (z.r + 35);
    z.kbX = Math.max(20, Math.min(W-20, z.kbX));
    z.kbY = Math.max(20, Math.min(H-20, z.kbY));
  });

  function zoneSharedTarget(z) {
    if (z.privateKB) return null;
    return z.x < 360 ? CM_L : CM_R;
  }

  function archPositions(z) {
    var R = z.r * 0.65, positions = [];
    if (z.arch === 'Autocracy') {
      positions.push({x:z.x, y:z.y});
      for (var i=1; i<z.nAgents; i++) {
        var a = ((i-1)/(z.nAgents-1))*Math.PI*2 - Math.PI/2;
        positions.push({x: z.x+Math.cos(a)*R, y: z.y+Math.sin(a)*R});
      }
    } else if (z.arch === 'Zero-Trust' || z.arch === 'Senate') {
      for (var i=0; i<z.nAgents; i++) {
        var a = (i/z.nAgents)*Math.PI*2 - Math.PI/2;
        positions.push({x: z.x+Math.cos(a)*R, y: z.y+Math.sin(a)*R});
      }
    } else if (z.arch === 'Market') {
      for (var i=0; i<z.nAgents; i++) {
        var a = Math.PI*2*i/z.nAgents + (Math.random()-0.5)*0.4;
        var d = 0.35*R + Math.random()*0.6*R;
        positions.push({x: z.x+Math.cos(a)*d, y: z.y+Math.sin(a)*d});
      }
    } else if (z.arch === 'Guild') {
      var nC = Math.min(3, Math.ceil(z.nAgents/3));
      z.clusterCenters = [];
      for (var c=0; c<nC; c++) {
        var a = (c/nC)*Math.PI*2 - Math.PI/2;
        z.clusterCenters.push({x: z.x+Math.cos(a)*R*0.55, y: z.y+Math.sin(a)*R*0.55});
      }
      for (var i=0; i<z.nAgents; i++) {
        var cc = z.clusterCenters[i%nC];
        positions.push({x: cc.x+(Math.random()-0.5)*14, y: cc.y+(Math.random()-0.5)*14});
      }
    } else if (z.arch === 'Colony') {
      var a1={x:z.x-R*0.3,y:z.y-R*0.2}, a2={x:z.x+R*0.3,y:z.y+R*0.2};
      for (var i=0; i<z.nAgents; i++) {
        var att = i<z.nAgents/2 ? a1 : a2;
        positions.push({x: att.x+(Math.random()-0.5)*R*0.5, y: att.y+(Math.random()-0.5)*R*0.5});
      }
    } else if (z.arch === 'Federation') {
      var nC = 3; z.clusterCenters = [];
      for (var c=0; c<nC; c++) {
        var a = (c/nC)*Math.PI*2 - Math.PI/2;
        z.clusterCenters.push({x: z.x+Math.cos(a)*R*0.5, y: z.y+Math.sin(a)*R*0.5});
      }
      for (var i=0; i<z.nAgents; i++) {
        var cc = z.clusterCenters[i%nC];
        positions.push({x: cc.x+(Math.random()-0.5)*12, y: cc.y+(Math.random()-0.5)*12});
      }
    } else if (z.arch === 'Meritocracy') {
      var tiers = [{r:R*0.2, n:2}, {r:R*0.5, n:4}, {r:R*0.8, n:z.nAgents-6}];
      var idx2 = 0;
      tiers.forEach(function(t) {
        for (var i=0; i<t.n; i++) {
          var a = (i/t.n)*Math.PI*2 - Math.PI/2;
          positions.push({x: z.x+Math.cos(a)*t.r, y: z.y+Math.sin(a)*t.r});
        }
      });
    } else if (z.arch === 'Doctrine') {
      for (var i=0; i<z.nAgents; i++) {
        var a = (i/z.nAgents)*Math.PI*2 - Math.PI/2;
        positions.push({x: z.x+Math.cos(a)*R*0.7, y: z.y+Math.sin(a)*R*0.7});
      }
    } else if (z.arch === 'Oligarchy') {
      z.elitePositions = [];
      for (var i=0; i<3; i++) {
        var a = (i/3)*Math.PI*2 - Math.PI/2;
        var p = {x: z.x+Math.cos(a)*R*0.2, y: z.y+Math.sin(a)*R*0.2};
        z.elitePositions.push(p);
        positions.push(p);
      }
      for (var i=3; i<z.nAgents; i++) {
        var a = ((i-3)/(z.nAgents-3))*Math.PI*2 - Math.PI/2;
        positions.push({x: z.x+Math.cos(a)*R*0.75, y: z.y+Math.sin(a)*R*0.75});
      }
    }
    return positions;
  }

  // Draw stick figure
  function drawPerson(layer, cx, cy, color, delay, opacity) {
    opacity = opacity || 0.8;
    var g = layer.append('g').attr('opacity', 0);
    g.append('circle').attr('cx', cx).attr('cy', cy-14).attr('r', 5)
      .attr('fill', color).attr('stroke', d3.color(color).darker(0.5)).attr('stroke-width', 0.8);
    g.append('line').attr('x1', cx).attr('y1', cy-9).attr('x2', cx).attr('y2', cy+4)
      .attr('stroke', color).attr('stroke-width', 1.8);
    g.append('line').attr('x1', cx-7).attr('y1', cy-3).attr('x2', cx+7).attr('y2', cy-3)
      .attr('stroke', color).attr('stroke-width', 1.8);
    g.append('line').attr('x1', cx).attr('y1', cy+4).attr('x2', cx-6).attr('y2', cy+15)
      .attr('stroke', color).attr('stroke-width', 1.8);
    g.append('line').attr('x1', cx).attr('y1', cy+4).attr('x2', cx+6).attr('y2', cy+15)
      .attr('stroke', color).attr('stroke-width', 1.8);
    g.transition().delay(delay||0).duration(500).attr('opacity', opacity);
    return g;
  }

  // ---- State ----
  var p1_humans=[], p1_frontiers=[], p1_links=[], p1_ghostEls=[];
  var p2_distilled=[], p2_locals=[], p2_a2aLinks=[];
  var p3_agents=[], p3_zoneEls=[], p3_kbEls=[], p3_roadEls=[], p3_labelEls=[];
  var p4_rafId=null, p4_timers=[], p4_extraAgents=[], p4_travelers=[];
  var p4_epoch=0, p4_running=false;

  function clearScene() {
    stopPhase4();
    if (introGroup) { introGroup.remove(); introGroup = null; }
    linkLayer.selectAll('*').remove();
    roadLayer.selectAll('*').remove();
    pktLayer.selectAll('*').remove();
    nodeLayer.selectAll('*').remove();
    labelLayer.selectAll('*').remove();
    bgLayer.selectAll('.zone-ghost,.zone-boundary,.zone-glow,.cm-bg,.kf-bg').remove();
    phaseLabel = labelLayer.append('text').attr('x', W/2).attr('y', 44)
      .attr('text-anchor', 'middle').attr('font-size', '17px').attr('font-weight', 'bold').attr('fill', '#333');
    phaseSub = labelLayer.append('text').attr('x', W/2).attr('y', 62)
      .attr('text-anchor', 'middle').attr('font-size', '11px').attr('fill', '#888');
  }

  function stopPhase4() {
    p4_running = false;
    if (p4_rafId) { cancelAnimationFrame(p4_rafId); p4_rafId = null; }
    p4_timers.forEach(function(t) { if (t.clear) t.clear(); else { clearInterval(t); clearTimeout(t); } });
    p4_timers = [];
  }

  function showLegend() {
    legendLayer.selectAll('*').remove();
    var lx = W-118, ly = 78, sp = 18;
    legendLayer.append('rect').attr('x', lx-12).attr('y', ly-10)
      .attr('width', 130).attr('height', sp*7+8).attr('rx', 4)
      .attr('fill', 'white').attr('fill-opacity', 0.92).attr('stroke', '#e5e7eb').attr('stroke-width', 0.8);
    function row(yi, shape, fill, stroke, label) {
      if (shape === 'person') {
        var g = legendLayer.append('g');
        g.append('circle').attr('cx', lx).attr('cy', yi-4).attr('r', 3).attr('fill', fill);
        g.append('line').attr('x1', lx).attr('y1', yi-1).attr('x2', lx).attr('y2', yi+4).attr('stroke', fill).attr('stroke-width', 1.5);
        g.append('line').attr('x1', lx-4).attr('y1', yi+1).attr('x2', lx+4).attr('y2', yi+1).attr('stroke', fill).attr('stroke-width', 1.5);
      } else if (shape === 'circle') {
        legendLayer.append('circle').attr('cx', lx).attr('cy', yi)
          .attr('r', stroke==='big'?8:5).attr('fill', fill).attr('stroke', stroke==='big'?'white':'#94a3b8').attr('stroke-width', 1);
      } else if (shape === 'smallcircle') {
        legendLayer.append('circle').attr('cx', lx).attr('cy', yi).attr('r', 3).attr('fill', fill).attr('stroke', '#cbd5e1').attr('stroke-width', 0.8);
      } else if (shape === 'rect') {
        var sz = stroke==='big'?12:8;
        legendLayer.append('rect').attr('x', lx-sz/2).attr('y', yi-sz/2).attr('width', sz).attr('height', sz).attr('rx', 1)
          .attr('fill', fill).attr('stroke', stroke==='big'?'#059669':'#fbbf24').attr('stroke-width', 0.8);
      } else if (shape === 'diamond') {
        var ds = 7;
        legendLayer.append('polygon')
          .attr('points', lx+','+(yi-ds)+' '+(lx+ds)+','+yi+' '+lx+','+(yi+ds)+' '+(lx-ds)+','+yi)
          .attr('fill', fill).attr('stroke', '#06b6d4').attr('stroke-width', 0.8);
      }
      legendLayer.append('text').attr('x', lx+14).attr('y', yi+3.5)
        .attr('text-anchor', 'start').attr('font-size', '9px').attr('fill', '#666').text(label);
    }
    row(ly,        'person',      '#d97706', '',    'Human');
    row(ly+sp,     'circle',      '#1e293b', 'big', 'Frontier model');
    row(ly+sp*2,   'circle',      '#475569', '',    'Distilled model');
    row(ly+sp*3,   'smallcircle', '#94a3b8', '',    'Local model');
    row(ly+sp*4,   'rect',        '#fef3c7', '',    'Personal memory');
    row(ly+sp*5,   'rect',        '#065f46', 'big', 'Collective memory');
    row(ly+sp*6,   'diamond',     '#0891b2', '',    'Knowledge Factory');
  }

  // ---- PHASE INTRO: Hook / landing ----
  var introGroup = null;
  function phaseIntro() {
    clearScene();
    legendLayer.selectAll('*').remove();
    setPhaseUI(0, '', '');
    introGroup = svg.append('g').attr('class', 'intro-group');
    // Tagline
    introGroup.append('text').attr('x', W/2).attr('y', 180)
      .attr('text-anchor', 'middle').attr('font-size', '20px').attr('font-weight', 'bold').attr('fill', '#333').attr('opacity', 0)
      .text('Humans and AI agents are forming societies').transition().duration(800).attr('opacity', 1);
    introGroup.append('text').attr('x', W/2).attr('y', 210)
      .attr('text-anchor', 'middle').attr('font-size', '20px').attr('font-weight', 'bold').attr('fill', '#333').attr('opacity', 0)
      .text('that interweave into a living fabric.').transition().delay(300).duration(800).attr('opacity', 1);
    introGroup.append('text').attr('x', W/2).attr('y', 250)
      .attr('text-anchor', 'middle').attr('font-size', '13px').attr('fill', '#888').attr('opacity', 0)
      .text('One way things could unfold.').transition().delay(800).duration(600).attr('opacity', 1);
    // Mini preview zones with governance patterns
    var pzs = [
      {x:200, y:420, r:60, color:'#b91c1c', arch:'Autocracy'},
      {x:360, y:380, r:70, color:'#374151', arch:'Zero-Trust'},
      {x:520, y:420, r:55, color:'#2563eb', arch:'Senate'},
      {x:280, y:540, r:50, color:'#059669', arch:'Guild'},
      {x:440, y:550, r:55, color:'#7c3aed', arch:'Colony'}
    ];
    pzs.forEach(function(pz, i) {
      var g = introGroup.append('g');
      g.append('circle').attr('cx', pz.x).attr('cy', pz.y).attr('r', pz.r)
        .attr('fill', pz.color).attr('fill-opacity', 0).attr('stroke', pz.color)
        .attr('stroke-width', 1.5).attr('stroke-dasharray', '6,4').attr('stroke-opacity', 0)
        .transition().delay(600+i*150).duration(800).attr('fill-opacity', 0.04).attr('stroke-opacity', 0.2);
      var R = pz.r*0.7, n = 6, dl = 900+i*150;
      if (pz.arch === 'Autocracy') {
        // Hub-spoke: center node + spokes
        g.append('circle').attr('cx', pz.x).attr('cy', pz.y).attr('r', 0)
          .attr('fill', pz.color).attr('opacity', 0)
          .transition().delay(dl).duration(400).attr('r', 4).attr('opacity', 0.6);
        for (var j=0; j<5; j++) {
          var a=(j/5)*Math.PI*2-Math.PI/2, lx=pz.x+Math.cos(a)*R, ly=pz.y+Math.sin(a)*R;
          g.append('line').attr('x1', pz.x).attr('y1', pz.y).attr('x2', pz.x).attr('y2', pz.y)
            .attr('stroke', pz.color).attr('stroke-width', 0.6).attr('opacity', 0)
            .transition().delay(dl+100+j*40).duration(400).attr('x2', lx).attr('y2', ly).attr('opacity', 0.2);
          g.append('circle').attr('cx', lx).attr('cy', ly).attr('r', 0)
            .attr('fill', pz.color).attr('opacity', 0)
            .transition().delay(dl+200+j*40).duration(400).attr('r', 2.5).attr('opacity', 0.5);
        }
      } else if (pz.arch === 'Zero-Trust') {
        // Mesh: all-to-all connections
        var pts = [];
        for (var j=0; j<n; j++) {
          var a=(j/n)*Math.PI*2-Math.PI/2;
          pts.push({x:pz.x+Math.cos(a)*R*0.7, y:pz.y+Math.sin(a)*R*0.7});
        }
        pts.forEach(function(p1, j) {
          pts.forEach(function(p2, k) {
            if (k<=j) return;
            g.append('line').attr('x1', p1.x).attr('y1', p1.y).attr('x2', p1.x).attr('y2', p1.y)
              .attr('stroke', pz.color).attr('stroke-width', 0.4).attr('opacity', 0)
              .transition().delay(dl+j*30+k*20).duration(400).attr('x2', p2.x).attr('y2', p2.y).attr('opacity', 0.15);
          });
          g.append('circle').attr('cx', p1.x).attr('cy', p1.y).attr('r', 0)
            .attr('fill', pz.color).attr('opacity', 0)
            .transition().delay(dl+j*40).duration(400).attr('r', 2.5).attr('opacity', 0.5);
        });
      } else if (pz.arch === 'Senate') {
        // Ring arrangement
        for (var j=0; j<n; j++) {
          var a=(j/n)*Math.PI*2-Math.PI/2;
          g.append('circle').attr('cx', pz.x+Math.cos(a)*R*0.65).attr('cy', pz.y+Math.sin(a)*R*0.65).attr('r', 0)
            .attr('fill', pz.color).attr('opacity', 0)
            .transition().delay(dl+j*50).duration(400).attr('r', 2.5).attr('opacity', 0.5);
        }
        g.append('circle').attr('cx', pz.x).attr('cy', pz.y).attr('r', 0)
          .attr('fill', 'none').attr('stroke', pz.color).attr('stroke-width', 0.6).attr('stroke-dasharray', '3,3').attr('opacity', 0)
          .transition().delay(dl+200).duration(500).attr('r', R*0.65).attr('opacity', 0.2);
      } else if (pz.arch === 'Guild') {
        // 2 sub-groups with liaison
        var gx1=pz.x-R*0.35, gx2=pz.x+R*0.35;
        [gx1, gx2].forEach(function(gx, gi) {
          for (var j=0; j<3; j++) {
            var a=(j/3)*Math.PI*2-Math.PI/2, cr=R*0.25;
            g.append('circle').attr('cx', gx+Math.cos(a)*cr).attr('cy', pz.y+Math.sin(a)*cr).attr('r', 0)
              .attr('fill', pz.color).attr('opacity', 0)
              .transition().delay(dl+gi*100+j*40).duration(400).attr('r', 2.5).attr('opacity', 0.5);
          }
        });
        g.append('line').attr('x1', gx1).attr('y1', pz.y).attr('x2', gx1).attr('y2', pz.y)
          .attr('stroke', pz.color).attr('stroke-width', 0.8).attr('stroke-dasharray', '3,2').attr('opacity', 0)
          .transition().delay(dl+250).duration(400).attr('x2', gx2).attr('opacity', 0.2);
      } else if (pz.arch === 'Colony') {
        // Scattered swarm
        for (var j=0; j<8; j++) {
          var cx=pz.x+(Math.random()-0.5)*R*1.4, cy=pz.y+(Math.random()-0.5)*R*1.4;
          g.append('circle').attr('cx', cx).attr('cy', cy).attr('r', 0)
            .attr('fill', pz.color).attr('opacity', 0)
            .transition().delay(dl+j*50).duration(400).attr('r', 1.5+Math.random()*2).attr('opacity', 0.4);
        }
      }
    });
    // Connecting lines between preview zones
    [[0,1],[1,2],[0,3],[1,3],[1,4],[2,4],[3,4]].forEach(function(pair, i) {
      var a=pzs[pair[0]], b=pzs[pair[1]];
      introGroup.append('line').attr('x1', a.x).attr('y1', a.y).attr('x2', b.x).attr('y2', b.y)
        .attr('stroke', '#999').attr('stroke-width', 0.8).attr('stroke-dasharray', '4,4').attr('opacity', 0)
        .transition().delay(1200+i*100).duration(600).attr('opacity', 0.15);
    });
    // Start button
    var btnG = introGroup.append('g').attr('cursor', 'pointer')
      .attr('opacity', 0).attr('transform', 'translate('+W/2+',700)');
    btnG.append('rect').attr('x', -90).attr('y', -20).attr('width', 180).attr('height', 40)
      .attr('rx', 20).attr('fill', '#1e293b').attr('stroke', '#334155').attr('stroke-width', 1);
    btnG.append('text').attr('x', 0).attr('y', 5)
      .attr('text-anchor', 'middle').attr('font-size', '14px').attr('font-weight', 'bold').attr('fill', 'white')
      .text('Start the simulation \u2192');
    btnG.transition().delay(1500).duration(600).attr('opacity', 1);
    btnG.on('click', function(event) { event.stopPropagation(); goToPhase(1); });
  }

  // ---- PHASE 0: Component introduction ----
  function phase0() {
    clearScene();
    legendLayer.selectAll('*').remove();
    setPhaseUI(1, 'Meet the Components', 'The building blocks of our two societies.');
    var cx = W/2-60, spacingY=70, labelX=cx+90, delay=200;
    function introRow(yi, drawFn, label, d) {
      drawFn(yi);
      labelLayer.append('text').attr('x', labelX).attr('y', yi+5)
        .attr('text-anchor', 'start').attr('font-size', '16px').attr('fill', '#444').attr('opacity', 0)
        .text(label).transition().delay(d+200).duration(400).attr('opacity', 1);
    }
    var hy=105;
    introRow(hy, function(yi) { drawPerson(nodeLayer, cx, yi+5, '#d97706', delay, 0.8); }, 'Human', delay);
    var fy=hy+spacingY;
    introRow(fy, function(yi) {
      nodeLayer.append('circle').attr('cx', cx).attr('cy', yi).attr('r', 0)
        .attr('fill', '#1e293b').attr('stroke', 'white').attr('stroke-width', 2).attr('opacity', 0)
        .transition().delay(delay+300).duration(600).attr('r', 26).attr('opacity', 0.9);
      nodeLayer.append('text').attr('x', cx).attr('y', yi+5).attr('text-anchor', 'middle')
        .attr('font-size', '13px').attr('font-weight', 'bold').attr('fill', 'white').attr('opacity', 0)
        .transition().delay(delay+600).duration(400).attr('opacity', 1).text('F');
    }, 'Frontier Model', delay+300);
    var dy2=fy+spacingY;
    introRow(dy2, function(yi) {
      nodeLayer.append('circle').attr('cx', cx).attr('cy', yi).attr('r', 0)
        .attr('fill', '#475569').attr('stroke', '#94a3b8').attr('stroke-width', 1).attr('opacity', 0)
        .transition().delay(delay+600).duration(600).attr('r', 12).attr('opacity', 0.8);
    }, 'Distilled Model', delay+600);
    var ly2=dy2+spacingY;
    introRow(ly2, function(yi) {
      nodeLayer.append('circle').attr('cx', cx).attr('cy', yi).attr('r', 0)
        .attr('fill', '#94a3b8').attr('stroke', '#cbd5e1').attr('stroke-width', 0.8).attr('opacity', 0)
        .transition().delay(delay+900).duration(600).attr('r', 6).attr('opacity', 0.8);
    }, 'Local Model', delay+900);
    var pmy=ly2+spacingY;
    introRow(pmy, function(yi) {
      nodeLayer.append('rect').attr('x', cx-6).attr('y', yi-6).attr('width', 0).attr('height', 0)
        .attr('rx', 1).attr('fill', '#fef3c7').attr('stroke', '#fbbf24').attr('stroke-width', 1)
        .transition().delay(delay+1200).duration(500).attr('width', 12).attr('height', 12);
    }, 'Personal Memory (per agent)', delay+1200);
    var kby=pmy+spacingY;
    introRow(kby, function(yi) {
      nodeLayer.append('rect').attr('x', cx-5).attr('y', yi-7).attr('width', 0).attr('height', 0)
        .attr('rx', 3).attr('fill', '#f1f5f9').attr('stroke', '#475569').attr('stroke-width', 1)
        .transition().delay(delay+1350).duration(500).attr('width', 10).attr('height', 14);
    }, 'Knowledge Base (local)', delay+1350);
    var cmy=kby+spacingY;
    introRow(cmy, function(yi) {
      nodeLayer.append('rect').attr('x', cx-11).attr('y', yi-11).attr('width', 0).attr('height', 0)
        .attr('rx', 2).attr('fill', '#065f46').attr('stroke', '#059669').attr('stroke-width', 1.5)
        .transition().delay(delay+1650).duration(500).attr('width', 22).attr('height', 22);
    }, 'Collective Memory', delay+1650);
    var kfy=cmy+spacingY;
    introRow(kfy, function(yi) {
      var sz=13;
      nodeLayer.append('polygon')
        .attr('points', cx+','+(yi-sz)+' '+(cx+sz)+','+yi+' '+cx+','+(yi+sz)+' '+(cx-sz)+','+yi)
        .attr('fill', '#0891b2').attr('stroke', '#06b6d4').attr('stroke-width', 1.5).attr('opacity', 0)
        .transition().delay(delay+1950).duration(500).attr('opacity', 0.9);
      nodeLayer.append('text').attr('x', cx).attr('y', yi+4).attr('text-anchor', 'middle')
        .attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', 'white').attr('opacity', 0)
        .transition().delay(delay+2150).duration(400).attr('opacity', 1).text('KF');
    }, 'Knowledge Factory', delay+1950);
  }

  // ---- PHASE 1: Individual Interactions ----
  function phase1() {
    clearScene();
    showLegend();
    setPhaseUI(2, 'Phase 1: Individual Interactions',
      '2022-2026: You talk to ChatGPT, Claude, Gemini. Your agents do not talk to each other.');

    // Ghost zone outlines (very faint)
    p1_ghostEls = [];
    zones.forEach(function(z) {
      var el = bgLayer.append('circle').attr('class', 'zone-ghost')
        .attr('cx', z.x).attr('cy', z.y).attr('r', z.r)
        .attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.8)
        .attr('stroke-dasharray', '4,4').attr('stroke-opacity', 0);
      el.transition().delay(800).duration(1000).attr('stroke-opacity', 0.06);
      p1_ghostEls.push(el);
    });

    // 2 frontier models
    [{x:F1.x,y:F1.y},{x:F2.x,y:F2.y}].forEach(function(fp, fi) {
      var fel = nodeLayer.append('circle').attr('cx', fp.x).attr('cy', fp.y)
        .attr('r', 0).attr('fill', '#1e293b').attr('stroke', 'white').attr('stroke-width', 2.5).attr('opacity', 0);
      fel.transition().delay(400+fi*200).duration(800).attr('r', 30).attr('opacity', 0.92);
      var flabel = nodeLayer.append('text').attr('x', fp.x).attr('y', fp.y+6)
        .attr('text-anchor', 'middle').attr('font-size', '14px').attr('font-weight', 'bold')
        .attr('fill', 'white').attr('opacity', 0).text('F');
      flabel.transition().delay(700+fi*200).duration(400).attr('opacity', 1);
      p1_frontiers.push({x:fp.x, y:fp.y, el:fel, label:flabel});
    });

    // 2 very transparent humans
    HUMANS.forEach(function(hp, hi) {
      var mem = nodeLayer.append('rect')
        .attr('x', hp.x+10).attr('y', hp.y-24).attr('width', 0).attr('height', 0)
        .attr('rx', 1).attr('fill', '#fef3c7').attr('stroke', '#fbbf24').attr('stroke-width', 0.5).attr('opacity', 0);
      mem.transition().delay(1000+hi*100).duration(400).attr('width', 7).attr('height', 7).attr('opacity', 0.4);
      var fig = drawPerson(nodeLayer, hp.x, hp.y, '#d97706', 300+hi*150, 0.8);
      p1_humans.push({x:hp.x, y:hp.y, fig:fig, mem:mem});

      // Connect to both frontiers
      [F1, F2].forEach(function(fp, fi) {
        var line = linkLayer.append('line')
          .attr('x1', hp.x).attr('y1', hp.y+5).attr('x2', fp.x).attr('y2', fp.y)
          .attr('stroke', '#d97706').attr('stroke-width', 1.5).attr('opacity', 0);
        line.transition().delay(1100+hi*50+fi*80).duration(500).attr('opacity', 0.3);
        p1_links.push(line);
      });
    });
  }

  // ---- PHASE 2: Ecosystem Grows ----
  function phase2() {
    setPhaseUI(3, 'Phase 2: The Ecosystem Grows',
      'Distilled models emerge. Agent-to-agent protocols appear.');

    // Zone ghosts become slightly more visible
    p1_ghostEls.forEach(function(el) {
      el.transition().duration(800).attr('stroke-opacity', 0.1);
    });

    // 15 distilled models spawn from frontiers toward zone positions
    var sources = [F1, F2];
    for (var i=0; i<15; i++) {
      var fi = i<8 ? 0 : 1;
      var src = sources[fi];
      var tz = zones[i % zones.length];
      var tx = tz.x + (Math.random()-0.5)*tz.r*0.6;
      var ty = tz.y + (Math.random()-0.5)*tz.r*0.6;
      var el = nodeLayer.append('circle').attr('cx', src.x).attr('cy', src.y)
        .attr('r', 0).attr('fill', '#475569').attr('stroke', '#94a3b8').attr('stroke-width', 1).attr('opacity', 0);
      el.transition().delay(300+i*120).duration(1000)
        .attr('cx', tx).attr('cy', ty).attr('r', 3+Math.random()*4).attr('opacity', 0.6);
      p2_distilled.push({x:tx, y:ty, el:el});
    }

    // 2 local models near humans
    HUMANS.forEach(function(hp, hi) {
      var el = nodeLayer.append('circle').attr('cx', hp.x).attr('cy', hp.y)
        .attr('r', 0).attr('fill', '#94a3b8').attr('stroke', '#cbd5e1').attr('stroke-width', 0.8).attr('opacity', 0);
      el.transition().delay(900+hi*150).duration(700)
        .attr('cx', hp.x+16).attr('cy', hp.y+14).attr('r', 5).attr('opacity', 0.5);
      p2_locals.push(el);
    });

    // A2A links
    [[0,1],[2,3],[5,6],[7,8],[1,5],[3,7],[9,12],[10,14]].forEach(function(pair, i) {
      var a = p2_distilled[pair[0]], b = p2_distilled[pair[1]];
      if (!a || !b) return;
      var line = linkLayer.append('line')
        .attr('x1', a.x).attr('y1', a.y).attr('x2', b.x).attr('y2', b.y)
        .attr('stroke', '#60a5fa').attr('stroke-width', 0.8).attr('stroke-dasharray', '4,3').attr('opacity', 0);
      line.transition().delay(1600+i*120).duration(500).attr('opacity', 0.2);
      p2_a2aLinks.push(line);
    });
  }

  // ---- PHASE 3: Societies Form ----
  function phase3() {
    setPhaseUI(4, 'Phase 3: Agent Societies Form',
      'Governance emerges. Collective memories and the Knowledge Factory appear. Some frontier models stay independent.');

    // Fade A2A links
    p2_a2aLinks.forEach(function(l) { l.transition().duration(600).attr('opacity', 0); });

    // F1 merges into zone 0
    if (p1_frontiers[0]) {
      p1_frontiers[0].el.transition().duration(1400).ease(d3.easeCubicInOut)
        .attr('cx', zones[0].x).attr('cy', zones[0].y).attr('r', 18)
        .attr('fill', zones[0].color).attr('stroke', 'white').attr('opacity', 0.5);
      if (p1_frontiers[0].label) p1_frontiers[0].label.transition().duration(1000).attr('opacity', 0);
    }
    // F2 moves to individual position
    if (p1_frontiers[1]) {
      p1_frontiers[1].el.transition().duration(1400).ease(d3.easeCubicInOut)
        .attr('cx', F2_IND.x).attr('cy', F2_IND.y).attr('r', 24).attr('opacity', 0.7);
      if (p1_frontiers[1].label)
        p1_frontiers[1].label.transition().duration(1400).ease(d3.easeCubicInOut)
          .attr('x', F2_IND.x).attr('y', F2_IND.y+5).attr('font-size', '12px');
      labelLayer.append('text').attr('x', F2_IND.x).attr('y', F2_IND.y+34)
        .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#64748b').attr('opacity', 0)
        .text('Individual').transition().delay(1400).duration(500).attr('opacity', 0.7);
    }

    // Fade human links
    p1_links.forEach(function(l) { l.transition().delay(600).duration(800).attr('opacity', 0); });

    // Move distilled models into zones
    p2_distilled.forEach(function(d, i) {
      var z = zones[i % zones.length];
      var pos = archPositions(z);
      var p = pos[i % pos.length] || {x:z.x, y:z.y};
      d.el.transition().duration(1400).ease(d3.easeCubicInOut)
        .attr('cx', p.x).attr('cy', p.y).attr('fill', z.color).attr('r', 2.5+Math.random()*3).attr('opacity', 0.7);
    });

    // Zone boundaries solidify
    p3_zoneEls = [];
    zones.forEach(function(z, i) {
      // Remove ghost, add solid boundary
      if (p1_ghostEls[i]) p1_ghostEls[i].transition().duration(400).attr('stroke-opacity', 0);
      var ze = bgLayer.append('circle').attr('class', 'zone-boundary')
        .attr('cx', z.x).attr('cy', z.y).attr('r', 0)
        .attr('fill', z.color).attr('fill-opacity', 0.04)
        .attr('stroke', z.color).attr('stroke-width', 1.5).attr('stroke-dasharray', '4,3').attr('stroke-opacity', 0);
      ze.transition().delay(500+i*80).duration(900).attr('r', z.r).attr('stroke-opacity', 0.35);
      p3_zoneEls.push(ze);

      // Internal archetype structure
      var pos = archPositions(z);
      if (z.arch === 'Autocracy') {
        for (var s=1; s<pos.length; s++) {
          roadLayer.append('line').attr('x1', pos[0].x).attr('y1', pos[0].y).attr('x2', pos[s].x).attr('y2', pos[s].y)
            .attr('stroke', z.color).attr('stroke-width', 0.8).attr('opacity', 0)
            .transition().delay(1200).duration(500).attr('opacity', 0.15);
        }
      } else if (z.arch === 'Zero-Trust') {
        for (var s=0; s<pos.length; s++) for (var j=s+1; j<pos.length; j++) {
          roadLayer.append('line').attr('x1', pos[s].x).attr('y1', pos[s].y).attr('x2', pos[j].x).attr('y2', pos[j].y)
            .attr('stroke', z.color).attr('stroke-width', 0.4).attr('opacity', 0)
            .transition().delay(1200).duration(500).attr('opacity', 0.08);
        }
      } else if (z.arch === 'Senate') {
        for (var s=0; s<pos.length; s++) {
          var j2 = (s+1)%pos.length;
          roadLayer.append('line').attr('x1', pos[s].x).attr('y1', pos[s].y).attr('x2', pos[j2].x).attr('y2', pos[j2].y)
            .attr('stroke', z.color).attr('stroke-width', 0.8).attr('opacity', 0)
            .transition().delay(1200).duration(500).attr('opacity', 0.15);
        }
      } else if (z.arch === 'Guild' && z.clusterCenters) {
        z.clusterCenters.forEach(function(cc) {
          roadLayer.append('circle').attr('cx', cc.x).attr('cy', cc.y).attr('r', 14)
            .attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.6)
            .attr('stroke-dasharray', '2,2').attr('opacity', 0)
            .transition().delay(1200).duration(500).attr('opacity', 0.15);
        });
      } else if (z.arch === 'Federation' && z.clusterCenters) {
        // Sub-group boundaries + connecting arcs
        z.clusterCenters.forEach(function(cc) {
          roadLayer.append('circle').attr('cx', cc.x).attr('cy', cc.y).attr('r', 16)
            .attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.8)
            .attr('stroke-dasharray', '3,2').attr('opacity', 0)
            .transition().delay(1200).duration(500).attr('opacity', 0.2);
        });
        for (var s=0; s<z.clusterCenters.length; s++) {
          var j3 = (s+1)%z.clusterCenters.length;
          roadLayer.append('line').attr('x1', z.clusterCenters[s].x).attr('y1', z.clusterCenters[s].y)
            .attr('x2', z.clusterCenters[j3].x).attr('y2', z.clusterCenters[j3].y)
            .attr('stroke', z.color).attr('stroke-width', 0.6).attr('stroke-dasharray', '4,3').attr('opacity', 0)
            .transition().delay(1300).duration(500).attr('opacity', 0.12);
        }
      } else if (z.arch === 'Meritocracy') {
        [z.r*0.65*0.2, z.r*0.65*0.5, z.r*0.65*0.8].forEach(function(tr) {
          roadLayer.append('circle').attr('cx', z.x).attr('cy', z.y).attr('r', tr)
            .attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.6)
            .attr('stroke-dasharray', '2,3').attr('opacity', 0)
            .transition().delay(1200).duration(500).attr('opacity', 0.12);
        });
      } else if (z.arch === 'Doctrine') {
        // Central tablet
        var tw=8, th=10;
        roadLayer.append('rect').attr('x', z.x-tw/2).attr('y', z.y-th/2).attr('width', tw).attr('height', th)
          .attr('rx', 2).attr('fill', '#fef3c7').attr('stroke', z.color).attr('stroke-width', 1).attr('opacity', 0)
          .transition().delay(1200).duration(500).attr('opacity', 0.6);
      } else if (z.arch === 'Oligarchy' && z.elitePositions) {
        for (var s=0; s<z.elitePositions.length; s++) {
          var j4 = (s+1)%z.elitePositions.length;
          roadLayer.append('line').attr('x1', z.elitePositions[s].x).attr('y1', z.elitePositions[s].y)
            .attr('x2', z.elitePositions[j4].x).attr('y2', z.elitePositions[j4].y)
            .attr('stroke', z.color).attr('stroke-width', 1.2).attr('opacity', 0)
            .transition().delay(1200).duration(500).attr('opacity', 0.2);
        }
      }

      // Spawn ~8 agents per zone (moderate density)
      var nSpawn = Math.min(8, z.nAgents);
      for (var s=0; s<nSpawn; s++) {
        var p = pos[s % pos.length];
        var agEl = nodeLayer.append('circle')
          .attr('cx', z.x).attr('cy', z.y).attr('r', 0)
          .attr('fill', z.color).attr('opacity', 0);
        agEl.transition().delay(1000+i*60+s*40).duration(800)
          .attr('cx', p.x).attr('cy', p.y).attr('r', 2.5+Math.random()*3)
          .attr('opacity', 0.6+Math.random()*0.3);
        p3_agents.push({el:agEl, x:p.x, y:p.y, zone:z.id});
      }
    });

    p3_roadEls = [];

    // KB icons
    p3_kbEls = [];
    zones.forEach(function(z, i) {
      var kbStroke = z.privateKB ? '#991b1b' : '#475569';
      linkLayer.append('line').attr('x1', z.kbX).attr('y1', z.kbY).attr('x2', z.x).attr('y2', z.y)
        .attr('stroke', z.privateKB?'#991b1b':'#94a3b8').attr('stroke-width', 0.8)
        .attr('stroke-dasharray', '3,3').attr('opacity', 0)
        .transition().delay(1800+i*40).duration(500).attr('opacity', 0.25);
      var kb = nodeLayer.append('rect').attr('x', z.kbX-5).attr('y', z.kbY-7).attr('width', 10).attr('height', 14)
        .attr('rx', 3).attr('fill', '#f1f5f9').attr('stroke', kbStroke).attr('stroke-width', 1).attr('opacity', 0);
      kb.transition().delay(1800+i*40).duration(500).attr('opacity', 0.8);
      nodeLayer.append('line').attr('x1', z.kbX-4).attr('y1', z.kbY).attr('x2', z.kbX+4).attr('y2', z.kbY)
        .attr('stroke', kbStroke).attr('stroke-width', 0.6).attr('opacity', 0)
        .transition().delay(1800+i*40).duration(500).attr('opacity', 0.8);
      labelLayer.append('text').attr('x', z.kbX).attr('y', z.kbY+18)
        .attr('text-anchor', 'middle').attr('font-size', '5px').attr('fill', '#94a3b8').attr('opacity', 0)
        .text('KB').transition().delay(2000+i*40).duration(400).attr('opacity', 0.6);
      p3_kbEls.push(kb);
    });

    // Zone labels
    p3_labelEls = [];
    zones.forEach(function(z, i) {
      var nl = labelLayer.append('text').attr('x', z.x).attr('y', z.y+z.r+14)
        .attr('text-anchor', 'middle').attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', z.color).attr('opacity', 0)
        .text(z.name);
      nl.transition().delay(1600+i*60).duration(500).attr('opacity', 0.8);
      var al = labelLayer.append('text').attr('x', z.x).attr('y', z.y+z.r+23)
        .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#999').attr('font-style', 'italic').attr('opacity', 0)
        .text(z.arch);
      al.transition().delay(1600+i*60).duration(500).attr('opacity', 0.6);
      p3_labelEls.push(nl, al);
    });

    // 2 Collective Memories (green squares)
    [CM_L, CM_R].forEach(function(cm) {
      var sz = 22;
      bgLayer.append('rect').attr('class', 'cm-bg')
        .attr('x', cm.x-sz-6).attr('y', cm.y-sz-6).attr('width', (sz+6)*2).attr('height', (sz+6)*2).attr('rx', 4)
        .attr('fill', '#ecfdf5').attr('fill-opacity', 0).attr('stroke', '#059669')
        .attr('stroke-width', 0.5).attr('stroke-dasharray', '3,4').attr('stroke-opacity', 0)
        .transition().delay(2200).duration(1000).attr('fill-opacity', 0.08).attr('stroke-opacity', 0.15);
      nodeLayer.append('rect')
        .attr('x', cm.x-sz).attr('y', cm.y-sz).attr('width', sz*2).attr('height', sz*2).attr('rx', 3)
        .attr('fill', '#065f46').attr('stroke', '#059669').attr('stroke-width', 1.5).attr('opacity', 0)
        .transition().delay(2300).duration(800).attr('opacity', 0.8);
      labelLayer.append('text').attr('x', cm.x).attr('y', cm.y+2)
        .attr('text-anchor', 'middle').attr('font-size', '7px').attr('font-weight', 'bold').attr('fill', 'white').attr('opacity', 0)
        .text('CM').transition().delay(2500).duration(500).attr('opacity', 0.9);
      labelLayer.append('text').attr('x', cm.x).attr('y', cm.y+sz+12)
        .attr('text-anchor', 'middle').attr('font-size', '7px').attr('font-weight', 'bold').attr('fill', '#065f46').attr('opacity', 0)
        .text('Coll. Memory').transition().delay(2600).duration(500).attr('opacity', 0.7);
    });

    // Knowledge Factory (circular-squares shape with diamond center)
    bgLayer.append('circle').attr('class', 'kf-bg')
      .attr('cx', KF_POS.x).attr('cy', KF_POS.y).attr('r', KF_POS.r+12)
      .attr('fill', '#ecfeff').attr('fill-opacity', 0).attr('stroke', '#06b6d4')
      .attr('stroke-width', 0.5).attr('stroke-dasharray', '3,4').attr('stroke-opacity', 0)
      .transition().delay(2200).duration(1000).attr('fill-opacity', 0.06).attr('stroke-opacity', 0.15);
    nodeLayer.append('circle').attr('cx', KF_POS.x).attr('cy', KF_POS.y).attr('r', KF_POS.r)
      .attr('fill', '#ecfeff').attr('fill-opacity', 0).attr('stroke', '#06b6d4')
      .attr('stroke-width', 1.5).attr('stroke-dasharray', '5,3').attr('stroke-opacity', 0)
      .transition().delay(2200).duration(1000).attr('fill-opacity', 0.2).attr('stroke-opacity', 0.4);
    for (var i=0; i<8; i++) {
      var wa = (i/8)*Math.PI*2;
      var wcx = KF_POS.x+Math.cos(wa)*22, wcy = KF_POS.y+Math.sin(wa)*22;
      nodeLayer.append('rect').attr('x', wcx-5).attr('y', wcy-4).attr('width', 10).attr('height', 8)
        .attr('rx', 2).attr('fill', '#ecfeff').attr('stroke', '#06b6d4').attr('stroke-width', 0.5).attr('opacity', 0)
        .transition().delay(2400+i*50).duration(400).attr('opacity', 0.35);
    }
    var ds = 9;
    nodeLayer.append('polygon')
      .attr('points', KF_POS.x+','+(KF_POS.y-ds)+' '+(KF_POS.x+ds)+','+KF_POS.y+' '+KF_POS.x+','+(KF_POS.y+ds)+' '+(KF_POS.x-ds)+','+KF_POS.y)
      .attr('fill', '#0891b2').attr('stroke', '#06b6d4').attr('stroke-width', 1).attr('opacity', 0)
      .transition().delay(2500).duration(500).attr('opacity', 0.9);
    nodeLayer.append('text').attr('x', KF_POS.x).attr('y', KF_POS.y+3)
      .attr('text-anchor', 'middle').attr('font-size', '6px').attr('font-weight', 'bold').attr('fill', 'white').attr('opacity', 0)
      .text('KF').transition().delay(2600).duration(400).attr('opacity', 1);
    labelLayer.append('text').attr('x', KF_POS.x).attr('y', KF_POS.y+KF_POS.r+12)
      .attr('text-anchor', 'middle').attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', '#164e63').attr('opacity', 0)
      .text('KNOWLEDGE FACTORY').transition().delay(2600).duration(500).attr('opacity', 0.8);
    labelLayer.append('text').attr('x', KF_POS.x).attr('y', KF_POS.y+KF_POS.r+22)
      .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#0e7490').attr('opacity', 0)
      .text('(synthesis hub)').transition().delay(2700).duration(500).attr('opacity', 0.7);

    // Links from non-private zones to nearest CM
    zones.forEach(function(z) {
      var tgt = zoneSharedTarget(z);
      if (!tgt) return;
      linkLayer.append('line').attr('x1', z.x).attr('y1', z.y).attr('x2', tgt.x).attr('y2', tgt.y)
        .attr('stroke', '#059669').attr('stroke-width', 1.2).attr('stroke-dasharray', '5,4').attr('opacity', 0)
        .transition().delay(2800).duration(600).attr('opacity', 0.3);
    });

    // KF connects to both CMs
    [CM_L, CM_R].forEach(function(cm) {
      linkLayer.append('line').attr('x1', KF_POS.x).attr('y1', KF_POS.y).attr('x2', cm.x).attr('y2', cm.y)
        .attr('stroke', '#06b6d4').attr('stroke-width', 1.5).attr('stroke-dasharray', '6,4').attr('opacity', 0)
        .transition().delay(2900).duration(600).attr('opacity', 0.35);
    });
  }

  // ---- PHASE 4: The Living Society ----
  function phase4() {
    setPhaseUI(5, 'Phase 4: The Living Society',
      'The fabric comes alive. Agents travel, societies evolve, knowledge flows.');
    p4_running = true;
    p4_epoch = 0;

    // Glow rings
    zones.forEach(function(z) {
      bgLayer.append('circle').attr('class', 'zone-glow')
        .attr('cx', z.x).attr('cy', z.y).attr('r', z.r+8)
        .attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.5)
        .attr('stroke-dasharray', '2,4').attr('stroke-opacity', 0)
        .transition().delay(300).duration(800).attr('stroke-opacity', 0.12);
    });

    // Spawn extra agents to reach full density
    p4_extraAgents = [];
    zones.forEach(function(z) {
      var pos = archPositions(z);
      var existing = p3_agents.filter(function(a) { return a.zone === z.id; }).length;
      var needed = z.nAgents - existing;
      for (var i=0; i<needed; i++) {
        var p = pos[(existing+i) % pos.length];
        var el = nodeLayer.append('circle')
          .attr('cx', z.x).attr('cy', z.y).attr('r', 0)
          .attr('fill', z.color).attr('opacity', 0).attr('stroke', 'white').attr('stroke-width', 0.4);
        el.transition().delay(200+i*60).duration(700)
          .attr('cx', p.x).attr('cy', p.y).attr('r', 3+Math.random()*3)
          .attr('opacity', 0.7+Math.random()*0.2);
        p4_extraAgents.push({el:el, x:p.x, y:p.y, zone:z.id, structure:true});
      }
    });

    // Spawn travelers
    p4_travelers = [];
    zones.forEach(function(z) {
      var nTrav = 4 + Math.floor(Math.random()*3);
      for (var t=0; t<nTrav; t++) {
        var a = Math.random()*Math.PI*2, d = Math.random()*z.r*0.5;
        var tx = z.x+Math.cos(a)*d, ty = z.y+Math.sin(a)*d;
        var el = pktLayer.append('circle')
          .attr('cx', tx).attr('cy', ty).attr('r', 2+Math.random()*2)
          .attr('fill', z.color).attr('opacity', 0.5+Math.random()*0.2)
          .attr('stroke', z.color).attr('stroke-width', 0.4);
        p4_travelers.push({el:el, x:tx, y:ty, vx:0, vy:0, home:z.id, visiting:-1, traveling:false});
      }
    });

    // Human multi-zone connections (phase1 humans persist)
    HUMANS.forEach(function(hp, hi) {
      var zs = hi===0 ? [0,1,4] : [2,3,7];
      zs.forEach(function(zi) {
        var z = zones[zi];
        linkLayer.append('line').attr('x1', hp.x).attr('y1', hp.y).attr('x2', z.x).attr('y2', z.y)
          .attr('stroke', '#d97706').attr('stroke-width', 1.2).attr('stroke-dasharray', '5,3').attr('opacity', 0)
          .transition().delay(1500).duration(600).attr('opacity', 0.35);
      });
    });

    // Individual frontier model connects to nearest CM
    linkLayer.append('line').attr('x1', F2_IND.x).attr('y1', F2_IND.y).attr('x2', CM_R.x).attr('y2', CM_R.y)
      .attr('stroke', '#64748b').attr('stroke-width', 1.5).attr('stroke-dasharray', '6,4').attr('opacity', 0)
      .transition().delay(1500).duration(600).attr('opacity', 0.35);

    // Animation loop
    var internalCD = {}, travelCD = {}, kbCD = {}, sharedCD = {};

    function p4tick() {
      if (!p4_running) return;
      p4_epoch++;
      // Traveler physics
      p4_travelers.forEach(function(ag) {
        if (ag.traveling) return;
        var z = zones[ag.visiting>=0 ? ag.visiting : ag.home];
        ag.vx += (Math.random()-0.5)*0.3;
        ag.vy += (Math.random()-0.5)*0.3;
        ag.vx += (z.x-ag.x)*0.005;
        ag.vy += (z.y-ag.y)*0.005;
        ag.vx *= 0.9; ag.vy *= 0.9;
        ag.x += ag.vx; ag.y += ag.vy;
        var dx=ag.x-z.x, dy=ag.y-z.y, dist=Math.sqrt(dx*dx+dy*dy);
        if (dist > z.r-6) { ag.x=z.x+dx/dist*(z.r-6); ag.y=z.y+dy/dist*(z.r-6); }
        ag.el.attr('cx', ag.x).attr('cy', ag.y);
      });

      // Internal animations
      zones.forEach(function(z) {
        if (internalCD[z.id] > p4_epoch) return;
        var zAg = p3_agents.concat(p4_extraAgents).filter(function(a) { return a.zone===z.id; });
        if (zAg.length < 2) return;
        if (z.arch === 'Autocracy') {
          internalCD[z.id] = p4_epoch + 40;
          var hub=zAg[0], leaf=zAg[1+Math.floor(Math.random()*(zAg.length-1))];
          if (!leaf) return;
          var p = pktLayer.append('circle').attr('cx', hub.x).attr('cy', hub.y)
            .attr('r', 2).attr('fill', z.color).attr('opacity', 0.8);
          p.transition().duration(500).ease(d3.easeLinear)
            .attr('cx', leaf.x).attr('cy', leaf.y)
            .transition().duration(200).attr('opacity', 0).on('end', function() { p.remove(); });
        } else if (z.arch === 'Zero-Trust') {
          internalCD[z.id] = p4_epoch + 30;
          var a1=zAg[Math.floor(Math.random()*zAg.length)], a2=zAg[Math.floor(Math.random()*zAg.length)];
          if (a1===a2) return;
          var ok = Math.random()>0.2;
          var ln = pktLayer.append('line').attr('x1', a1.x).attr('y1', a1.y).attr('x2', a2.x).attr('y2', a2.y)
            .attr('stroke', ok?'#4ade80':'#ef4444').attr('stroke-width', 1.2).attr('opacity', 0.7);
          ln.transition().duration(500).attr('opacity', 0).on('end', function() { ln.remove(); });
        } else if (z.arch === 'Senate') {
          internalCD[z.id] = p4_epoch + 50;
          var s1=zAg[Math.floor(Math.random()*zAg.length)], s2=zAg[Math.floor(Math.random()*zAg.length)];
          if (s1===s2) return;
          var p = pktLayer.append('circle').attr('cx', s1.x).attr('cy', s1.y)
            .attr('r', 1.5).attr('fill', z.color).attr('opacity', 0.8);
          p.transition().duration(400).ease(d3.easeLinear).attr('cx', s2.x).attr('cy', s2.y)
            .transition().duration(200).attr('opacity', 0).on('end', function() { p.remove(); });
        } else if (z.arch === 'Market') {
          internalCD[z.id] = p4_epoch + 70;
          var m=zAg[Math.floor(Math.random()*zAg.length)];
          if (!m || !m.el) return;
          var nr = 2+Math.random()*3;
          m.el.transition().duration(400).attr('r', nr).transition().duration(400).attr('r', 3+Math.random()*2);
        } else if (z.arch === 'Colony') {
          internalCD[z.id] = p4_epoch + 25;
          var c1=zAg[Math.floor(Math.random()*zAg.length)], c2=zAg[Math.floor(Math.random()*zAg.length)];
          if (c1===c2) return;
          var ln = pktLayer.append('line').attr('x1', c1.x).attr('y1', c1.y).attr('x2', c2.x).attr('y2', c2.y)
            .attr('stroke', z.color).attr('stroke-width', 0.5).attr('opacity', 0.3);
          ln.transition().duration(800).attr('opacity', 0).on('end', function() { ln.remove(); });
        } else if (z.arch === 'Federation') {
          internalCD[z.id] = p4_epoch + 50;
          if (!z.clusterCenters || z.clusterCenters.length<2) return;
          var ci = Math.floor(Math.random()*z.clusterCenters.length);
          var cj = (ci+1+Math.floor(Math.random()*(z.clusterCenters.length-1)))%z.clusterCenters.length;
          var from=z.clusterCenters[ci], to=z.clusterCenters[cj];
          var p = pktLayer.append('circle').attr('cx', from.x).attr('cy', from.y)
            .attr('r', 2).attr('fill', z.color).attr('opacity', 0.8);
          p.transition().duration(350).ease(d3.easeLinear).attr('cx', z.x).attr('cy', z.y)
            .transition().duration(350).ease(d3.easeLinear).attr('cx', to.x).attr('cy', to.y)
            .transition().duration(200).attr('opacity', 0).on('end', function() { p.remove(); });
        } else if (z.arch === 'Meritocracy') {
          internalCD[z.id] = p4_epoch + 60;
          var ag = zAg[Math.floor(Math.random()*zAg.length)];
          if (!ag || !ag.el) return;
          var up = Math.random()>0.4;
          ag.el.transition().duration(300).attr('fill', up?'#4ade80':'#ef4444').attr('r', up?5:2)
            .transition().duration(500).attr('fill', z.color).attr('r', 3+Math.random()*2);
        } else if (z.arch === 'Doctrine') {
          internalCD[z.id] = p4_epoch + 45;
          var ag = zAg[Math.floor(Math.random()*zAg.length)];
          if (!ag) return;
          var ok = Math.random()>0.3;
          var p = pktLayer.append('circle').attr('cx', ag.x).attr('cy', ag.y)
            .attr('r', 1.5).attr('fill', z.color).attr('opacity', 0.8);
          p.transition().duration(400).ease(d3.easeLinear).attr('cx', z.x).attr('cy', z.y)
            .transition().duration(200).attr('fill', ok?'#4ade80':'#ef4444')
            .transition().duration(300).attr('opacity', 0).on('end', function() { p.remove(); });
        } else if (z.arch === 'Oligarchy') {
          internalCD[z.id] = p4_epoch + 55;
          if (!z.elitePositions || z.elitePositions.length<2) return;
          var ei = Math.floor(Math.random()*z.elitePositions.length);
          var ej = (ei+1)%z.elitePositions.length;
          var from=z.elitePositions[ei], to=z.elitePositions[ej];
          var p = pktLayer.append('circle').attr('cx', from.x).attr('cy', from.y)
            .attr('r', 2).attr('fill', z.color).attr('opacity', 0.8);
          p.transition().duration(400).ease(d3.easeLinear).attr('cx', to.x).attr('cy', to.y)
            .transition().duration(200).attr('opacity', 0).on('end', function() { p.remove(); });
          var ring = pktLayer.append('circle').attr('cx', z.x).attr('cy', z.y)
            .attr('r', 5).attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.8).attr('opacity', 0.5);
          ring.transition().delay(500).duration(600).attr('r', z.r*0.7).attr('opacity', 0)
            .on('end', function() { ring.remove(); });
        }
      });

      // Travel
      if (p4_epoch % 3 === 0) {
        zones.forEach(function(z) {
          if (travelCD[z.id] > p4_epoch) return;
          var home = p4_travelers.filter(function(a) { return a.home===z.id && !a.traveling && a.visiting<0; });
          if (home.length < 1) return;
          var neighbors = [];
          roads.forEach(function(r) { if (r[0]===z.id) neighbors.push(r[1]); if (r[1]===z.id) neighbors.push(r[0]); });
          if (!neighbors.length) return;
          var destId = neighbors[Math.floor(Math.random()*neighbors.length)];
          var dest = zones[destId];
          var ag = home[Math.floor(Math.random()*home.length)];
          ag.traveling = true;
          travelCD[z.id] = p4_epoch + 180 + Math.floor(Math.random()*120);
          var ddx=dest.x-z.x, ddy=dest.y-z.y, dd=Math.sqrt(ddx*ddx+ddy*ddy);
          var dur = Math.max(1200, Math.min(2500, dd/60*1000));
          ag.el.transition().duration(dur).ease(d3.easeLinear)
            .attr('cx', dest.x+(Math.random()-0.5)*20).attr('cy', dest.y+(Math.random()-0.5)*20)
            .attr('r', 3.5).attr('opacity', 0.9)
            .on('end', function() {
              ag.traveling = false; ag.x=dest.x+(Math.random()-0.5)*20; ag.y=dest.y+(Math.random()-0.5)*20;
              ag.visiting = destId; ag.el.attr('r', 3).attr('opacity', 0.7);
              if (Math.random()<0.1) { ag.home=destId; ag.visiting=-1; ag.el.attr('fill', dest.color); }
              else { setTimeout(function() {
                if (!p4_running) return;
                ag.traveling=true;
                ag.el.transition().duration(dur).ease(d3.easeLinear)
                  .attr('cx', z.x+(Math.random()-0.5)*20).attr('cy', z.y+(Math.random()-0.5)*20)
                  .on('end', function() { ag.traveling=false; ag.visiting=-1; ag.x=z.x; ag.y=z.y; });
              }, 4000+Math.random()*4000); }
            });
        });
      }

      // KB packets
      if (p4_epoch % 6 === 0) {
        zones.forEach(function(z) {
          if (kbCD[z.id] > p4_epoch) return;
          kbCD[z.id] = p4_epoch + 100 + Math.floor(Math.random()*60);
          var pk = pktLayer.append('circle').attr('cx', z.kbX).attr('cy', z.kbY)
            .attr('r', 1.5).attr('fill', '#60a5fa').attr('opacity', 0.8);
          pk.transition().duration(800).ease(d3.easeLinear)
            .attr('cx', z.x+(Math.random()-0.5)*z.r*0.6).attr('cy', z.y+(Math.random()-0.5)*z.r*0.6)
            .transition().duration(400).attr('opacity', 0).on('end', function() { pk.remove(); });
        });
      }

      // Zone-to-CM packets
      if (p4_epoch % 15 === 0) {
        zones.forEach(function(z) {
          var tgt = zoneSharedTarget(z);
          if (!tgt) return;
          if (sharedCD[z.id] > p4_epoch) return;
          sharedCD[z.id] = p4_epoch + 300 + Math.floor(Math.random()*200);
          var toT = Math.random()>0.4;
          var pk = pktLayer.append('circle')
            .attr('cx', toT?z.kbX:tgt.x).attr('cy', toT?z.kbY:tgt.y)
            .attr('r', 2).attr('fill', '#059669').attr('opacity', 0.7);
          pk.transition().duration(1800).ease(d3.easeLinear)
            .attr('cx', toT?tgt.x:z.kbX).attr('cy', toT?tgt.y:z.kbY)
            .transition().duration(300).attr('opacity', 0).on('end', function() { pk.remove(); });
        });
      }
      // KF-to-CM packets
      if (p4_epoch % 40 === 0) {
        [CM_L, CM_R].forEach(function(cm) {
          var toKF = Math.random()>0.5;
          var pk = pktLayer.append('circle')
            .attr('cx', toKF?cm.x:KF_POS.x).attr('cy', toKF?cm.y:KF_POS.y)
            .attr('r', 2.5).attr('fill', '#06b6d4').attr('opacity', 0.8);
          pk.transition().duration(1400).ease(d3.easeLinear)
            .attr('cx', toKF?KF_POS.x:cm.x).attr('cy', toKF?KF_POS.y:cm.y)
            .transition().duration(300).attr('opacity', 0).on('end', function() { pk.remove(); });
        });
      }

      // Boundary events
      if (p4_epoch===800) {
        zones[6].r=35;
        if (p3_zoneEls[6]) p3_zoneEls[6].transition().duration(1200).attr('r', 35);
        flashEvt(zones[6].x, zones[6].y-60, 'The Accord dissolving...');
      }
      if (p4_epoch===1200) {
        if (p3_zoneEls[6]) p3_zoneEls[6].transition().duration(800).attr('stroke-opacity', 0.1).attr('fill-opacity', 0.01);
        flashEvt(zones[3].x, zones[3].y-70, 'Exchange absorbs The Accord');
      }
      if (p4_epoch===1600) {
        zones[4].r=95;
        if (p3_zoneEls[4]) p3_zoneEls[4].transition().duration(1200).attr('r', 95);
        if (p3_zoneEls[8]) p3_zoneEls[8].transition().duration(1000).attr('stroke-opacity', 0.1).attr('fill-opacity', 0.01);
        flashEvt((zones[4].x+zones[8].x)/2, (zones[4].y+zones[8].y)/2-30, 'The Forge expands');
      }
      if (p4_epoch===2200) {
        zones[7].r=42;
        if (p3_zoneEls[7]) p3_zoneEls[7].transition().duration(800).attr('r', 42);
        flashEvt(zones[7].x, zones[7].y-65, 'Arena schism');
      }

      p4_rafId = requestAnimationFrame(p4tick);
    }
    p4_rafId = requestAnimationFrame(p4tick);
  }

  var eventLabelEl = null;
  function flashEvt(x, y, text) {
    if (eventLabelEl) eventLabelEl.remove();
    eventLabelEl = labelLayer.append('text').attr('x', x).attr('y', y)
      .attr('text-anchor', 'middle').attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', '#333').attr('opacity', 0)
      .text(text);
    eventLabelEl.transition().duration(200).attr('opacity', 1)
      .transition().delay(2000).duration(500).attr('opacity', 0);
  }

  // ---- Navigation ----
  var phases = [phaseIntro, phase0, phase1, phase2, phase3, phase4];

  var skipAnim = false;

  // When skipAnim is true, .transition() returns the selection itself
  // so .attr()/.style() calls apply immediately (no async scheduling).
  var _origTransition = d3.selection.prototype.transition;
  d3.selection.prototype.transition = function() {
    if (skipAnim) {
      // Return a wrapper that acts like a selection: attr/style apply
      // immediately, delay/duration/ease/on are no-ops, and chained
      // .transition() returns another instant wrapper.
      var sel = this;
      var wrapper = {
        attr: function(k, v) { sel.attr(k, v); return wrapper; },
        style: function(k, v) { sel.style(k, v); return wrapper; },
        text: function(v) { sel.text(v); return wrapper; },
        duration: function() { return wrapper; },
        delay: function() { return wrapper; },
        ease: function() { return wrapper; },
        on: function(evt, fn) { if (evt === 'end' && fn) fn.call(sel.node()); return wrapper; },
        transition: function() { return wrapper; },
        remove: function() { sel.remove(); return wrapper; },
        each: function(fn) { sel.each(fn); return wrapper; },
        call: function(fn) { fn(wrapper); return wrapper; },
        selection: function() { return sel; }
      };
      return wrapper;
    }
    return _origTransition.apply(this, arguments);
  };

  function resetState() {
    stopPhase4();
    clearScene();
    legendLayer.selectAll('*').remove();
    p1_humans=[]; p1_frontiers=[]; p1_links=[]; p1_ghostEls=[];
    p2_distilled=[]; p2_locals=[]; p2_a2aLinks=[];
    p3_agents=[]; p3_zoneEls=[]; p3_kbEls=[]; p3_roadEls=[]; p3_labelEls=[];
    p4_extraAgents=[]; p4_travelers=[];
    // Reset zone radii
    zones[4].r=72; zones[6].r=55; zones[7].r=62; zones[8].r=65;
  }

  function goToPhase(n) {
    if (n < 0 || n >= phaseCount || n === curPhase) return;
    stopPhase4();
    if (n > curPhase) {
      for (var i = curPhase+1; i <= n; i++) phases[i]();
      curPhase = n;
    } else {
      resetState();
      skipAnim = true;
      for (var i = 0; i <= n; i++) phases[i]();
      skipAnim = false;
      curPhase = n;
    }
    updateArrows();
  }

  arrowNext.on('click', function(event) { event.stopPropagation(); goToPhase(curPhase+1); });
  arrowPrev.on('click', function(event) { event.stopPropagation(); goToPhase(curPhase-1); });
  d3.select(container).on('keydown', function(event) {
    if (event.key==='ArrowRight' || event.key==='Right') goToPhase(curPhase+1);
    if (event.key==='ArrowLeft'  || event.key==='Left')  goToPhase(curPhase-1);
  });
  svg.on('click', function(event) {
    if (event.target.closest && event.target.closest('g[cursor="pointer"]')) return;
    goToPhase(curPhase+1);
  });

  phaseIntro();
  updateArrows();
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
      timeout = setTimeout(function() { init(); }, 4000);
    }
  }

  init();
  svg.on('click', function() { if (timeout) clearTimeout(timeout); init(); });
})();

// ============================================================
// Visualization: The Living Society (viz-world) - 10 governance zones
// ============================================================
(function() {
  var container = document.getElementById('viz-world');
  if (!container) return;
  var width = 720, height = 800;
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

  // Background with subtle gradient
  var defs = svg.append('defs');
  var bgGrad = defs.append('radialGradient').attr('id', 'bg-grad-world')
    .attr('cx', '50%').attr('cy', '45%').attr('r', '65%');
  bgGrad.append('stop').attr('offset', '0%').attr('stop-color', '#fefefe');
  bgGrad.append('stop').attr('offset', '100%').attr('stop-color', '#f0f0f0');
  bgLayer.append('rect').attr('width', width).attr('height', height).attr('fill', 'url(#bg-grad-world)');

  // Zone definitions - arranged in upper 2/3 of canvas, bottom area reserved for CMs + KF
  var zones = [
    { id: 0, name: 'Imperial Core',   arch: 'Autocracy',   x: 120, y: 120, r: 75, color: '#b91c1c', nAgents: 12, privateKB: true },
    { id: 1, name: 'The Mesh',        arch: 'Zero-Trust',  x: 310, y: 105, r: 68, color: '#374151', nAgents: 10, privateKB: false },
    { id: 2, name: 'The Agora',       arch: 'Senate',      x: 510, y: 120, r: 70, color: '#2563eb', nAgents: 10, privateKB: true },
    { id: 3, name: 'Exchange Floor',  arch: 'Market',      x: 665, y: 145, r: 55, color: '#d97706', nAgents: 14, privateKB: false },
    { id: 4, name: 'The Forge',       arch: 'Guild',       x: 85,  y: 310, r: 72, color: '#059669', nAgents: 12, privateKB: false },
    { id: 5, name: 'The Hive',        arch: 'Colony',      x: 270, y: 295, r: 75, color: '#7c3aed', nAgents: 16, privateKB: false },
    { id: 6, name: 'The Accord',      arch: 'Federation',  x: 440, y: 300, r: 55, color: '#0369a1', nAgents: 10, privateKB: false },
    { id: 7, name: 'The Arena',       arch: 'Meritocracy', x: 600, y: 310, r: 62, color: '#ea580c', nAgents: 12, privateKB: false },
    { id: 8, name: 'The Codex',       arch: 'Doctrine',    x: 170, y: 475, r: 65, color: '#854d0e', nAgents: 12, privateKB: true },
    { id: 9, name: 'The Tribunal',    arch: 'Oligarchy',   x: 600, y: 470, r: 58, color: '#7e22ce', nAgents: 10, privateKB: true }
  ];

  // 2 Collective Memories + 1 Knowledge Factory (replacing Agent Wikipedia)
  var CM_L = {x: 198, y: 640}, CM_R = {x: 522, y: 640};
  var KF_POS = {x: 360, y: 680, r: 42};
  function zoneSharedTarget(z) {
    if (z.privateKB) return null;
    return z.x < 360 ? CM_L : CM_R;
  }

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
    } else if (z.arch === 'Federation') {
      var nC = 3; z.clusterCenters = [];
      for (var c = 0; c < nC; c++) {
        var a = (c / nC) * Math.PI * 2 - Math.PI / 2;
        z.clusterCenters.push({ x: z.x + Math.cos(a) * R * 0.5, y: z.y + Math.sin(a) * R * 0.5 });
      }
      for (var i = 0; i < z.nAgents; i++) {
        var cc = z.clusterCenters[i % nC];
        positions.push({ x: cc.x + (Math.random() - 0.5) * 12, y: cc.y + (Math.random() - 0.5) * 12 });
      }
    } else if (z.arch === 'Meritocracy') {
      var tiers = [{ r: R * 0.2, n: 2 }, { r: R * 0.5, n: 4 }, { r: R * 0.8, n: z.nAgents - 6 }];
      tiers.forEach(function(t) {
        for (var i = 0; i < t.n; i++) {
          var a = (i / t.n) * Math.PI * 2 - Math.PI / 2;
          positions.push({ x: z.x + Math.cos(a) * t.r, y: z.y + Math.sin(a) * t.r });
        }
      });
    } else if (z.arch === 'Doctrine') {
      for (var i = 0; i < z.nAgents; i++) {
        var a = (i / z.nAgents) * Math.PI * 2 - Math.PI / 2;
        positions.push({ x: z.x + Math.cos(a) * R * 0.7, y: z.y + Math.sin(a) * R * 0.7 });
      }
    } else if (z.arch === 'Oligarchy') {
      z.elitePositions = [];
      for (var i = 0; i < 3; i++) {
        var a = (i / 3) * Math.PI * 2 - Math.PI / 2;
        var p = { x: z.x + Math.cos(a) * R * 0.2, y: z.y + Math.sin(a) * R * 0.2 };
        z.elitePositions.push(p);
        positions.push(p);
      }
      for (var i = 3; i < z.nAgents; i++) {
        var a = ((i - 3) / (z.nAgents - 3)) * Math.PI * 2 - Math.PI / 2;
        positions.push({ x: z.x + Math.cos(a) * R * 0.75, y: z.y + Math.sin(a) * R * 0.75 });
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
        .attr('stroke', '#ccc').attr('stroke-width', 1.2).attr('stroke-dasharray', '6,4').attr('opacity', 0.4);
    });

    // Zone circles + internal archetype topology
    zones.forEach(function(z) {
      // Outer glow ring
      bgLayer.append('circle').attr('cx', z.x).attr('cy', z.y).attr('r', z.r + 8)
        .attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.5)
        .attr('stroke-dasharray', '2,4').attr('stroke-opacity', 0.12);
      z.circleEl = nodeLayer.append('circle').attr('cx', z.x).attr('cy', z.y).attr('r', z.r)
        .attr('fill', z.color).attr('fill-opacity', 0.04)
        .attr('stroke', z.color).attr('stroke-width', 1.5).attr('stroke-dasharray', '4,3').attr('stroke-opacity', 0.4);

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
      } else if (z.arch === 'Federation' && z.clusterCenters) {
        z.clusterCenters.forEach(function(cc) {
          roadLayer.append('circle').attr('cx', cc.x).attr('cy', cc.y).attr('r', 12)
            .attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.6)
            .attr('stroke-dasharray', '2,2').attr('opacity', 0.2);
        });
        for (var i = 0; i < z.clusterCenters.length; i++) {
          var j = (i + 1) % z.clusterCenters.length;
          roadLayer.append('line').attr('x1', z.clusterCenters[i].x).attr('y1', z.clusterCenters[i].y)
            .attr('x2', z.clusterCenters[j].x).attr('y2', z.clusterCenters[j].y)
            .attr('stroke', z.color).attr('stroke-width', 0.5).attr('stroke-dasharray', '3,3').attr('opacity', 0.15);
        }
      } else if (z.arch === 'Meritocracy') {
        [0.2, 0.5, 0.8].forEach(function(f) {
          roadLayer.append('circle').attr('cx', z.x).attr('cy', z.y).attr('r', z.r*0.65*f)
            .attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.5)
            .attr('stroke-dasharray', '2,3').attr('opacity', 0.12);
        });
      } else if (z.arch === 'Doctrine') {
        roadLayer.append('rect').attr('x', z.x-6).attr('y', z.y-5).attr('width', 12).attr('height', 10)
          .attr('rx', 1).attr('fill', z.color).attr('fill-opacity', 0.1)
          .attr('stroke', z.color).attr('stroke-width', 0.6).attr('stroke-dasharray', '2,2').attr('opacity', 0.25);
      } else if (z.arch === 'Oligarchy' && z.elitePositions) {
        for (var i = 0; i < z.elitePositions.length; i++) {
          var j = (i + 1) % z.elitePositions.length;
          roadLayer.append('line').attr('x1', z.elitePositions[i].x).attr('y1', z.elitePositions[i].y)
            .attr('x2', z.elitePositions[j].x).attr('y2', z.elitePositions[j].y)
            .attr('stroke', z.color).attr('stroke-width', 0.6).attr('stroke-dasharray', '2,2').attr('opacity', 0.15);
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
      labelLayer.append('text').attr('x', z.x).attr('y', z.y + z.r + 14)
        .attr('text-anchor', 'middle').attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', z.color).attr('opacity', 0.85)
        .text(z.name);
      labelLayer.append('text').attr('x', z.x).attr('y', z.y + z.r + 23)
        .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#999').attr('font-style', 'italic')
        .text(z.arch);
    });

    // ---- 2 Collective Memories + 1 Knowledge Factory ----
    [CM_L, CM_R].forEach(function(cm) {
      bgLayer.append('circle').attr('cx', cm.x).attr('cy', cm.y).attr('r', 28)
        .attr('fill', '#065f46').attr('fill-opacity', 0.06)
        .attr('stroke', '#059669').attr('stroke-width', 0.5).attr('stroke-dasharray', '3,4').attr('stroke-opacity', 0.15);
      nodeLayer.append('rect').attr('x', cm.x-18).attr('y', cm.y-18).attr('width', 36).attr('height', 36)
        .attr('rx', 4).attr('fill', '#065f46').attr('stroke', '#059669').attr('stroke-width', 1.5);
      nodeLayer.append('text').attr('x', cm.x).attr('y', cm.y+4)
        .attr('text-anchor', 'middle').attr('font-size', '10px').attr('font-weight', 'bold').attr('fill', 'white').text('CM');
      labelLayer.append('text').attr('x', cm.x).attr('y', cm.y+30)
        .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#065f46').text('Coll. Memory');
    });
    // KF
    bgLayer.append('circle').attr('cx', KF_POS.x).attr('cy', KF_POS.y).attr('r', KF_POS.r)
      .attr('fill', '#ecfeff').attr('fill-opacity', 0.2).attr('stroke', '#06b6d4')
      .attr('stroke-width', 1.5).attr('stroke-dasharray', '5,3').attr('stroke-opacity', 0.4);
    for (var i=0; i<8; i++) {
      var wa = (i/8)*Math.PI*2;
      var wcx = KF_POS.x+Math.cos(wa)*22, wcy = KF_POS.y+Math.sin(wa)*22;
      nodeLayer.append('rect').attr('x', wcx-5).attr('y', wcy-4).attr('width', 10).attr('height', 8)
        .attr('rx', 2).attr('fill', '#ecfeff').attr('stroke', '#06b6d4').attr('stroke-width', 0.5).attr('opacity', 0.35);
    }
    var ds = 9;
    nodeLayer.append('polygon')
      .attr('points', KF_POS.x+','+(KF_POS.y-ds)+' '+(KF_POS.x+ds)+','+KF_POS.y+' '+KF_POS.x+','+(KF_POS.y+ds)+' '+(KF_POS.x-ds)+','+KF_POS.y)
      .attr('fill', '#0891b2').attr('stroke', '#06b6d4').attr('stroke-width', 1).attr('opacity', 0.9);
    nodeLayer.append('text').attr('x', KF_POS.x).attr('y', KF_POS.y+3)
      .attr('text-anchor', 'middle').attr('font-size', '6px').attr('font-weight', 'bold').attr('fill', 'white').text('KF');
    labelLayer.append('text').attr('x', KF_POS.x).attr('y', KF_POS.y+KF_POS.r+12)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', '#164e63').text('KNOWLEDGE FACTORY');

    // Links from non-private zones to nearest CM
    zones.forEach(function(z) {
      var tgt = zoneSharedTarget(z);
      if (!tgt) return;
      kbLinkLayer.append('line')
        .attr('x1', z.x).attr('y1', z.y).attr('x2', tgt.x).attr('y2', tgt.y)
        .attr('stroke', '#059669').attr('stroke-width', 0.8).attr('stroke-dasharray', '5,4').attr('opacity', 0.2);
    });
    // KF connects to both CMs
    [CM_L, CM_R].forEach(function(cm) {
      kbLinkLayer.append('line').attr('x1', KF_POS.x).attr('y1', KF_POS.y).attr('x2', cm.x).attr('y2', cm.y)
        .attr('stroke', '#06b6d4').attr('stroke-width', 1.2).attr('stroke-dasharray', '6,4').attr('opacity', 0.25);
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
      // Traveler agents: 4-6 extra per zone, free to roam and travel
      var nTravelers = 4 + Math.floor(Math.random() * 3);
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
        // Structure agents: varied sizes for visual richness
        var sr = 3 + Math.random() * 3; // 3-6px radius
        a.el = pktLayer.append('circle').attr('cx', a.x).attr('cy', a.y).attr('r', sr)
          .attr('fill', z.color).attr('opacity', 0.75 + Math.random() * 0.2)
          .attr('stroke', 'white').attr('stroke-width', 0.4);
      } else {
        // Travelers: smaller, varied
        var tr = 2 + Math.random() * 2;
        a.el = pktLayer.append('circle').attr('cx', a.x).attr('cy', a.y).attr('r', tr)
          .attr('fill', z.color).attr('opacity', 0.5 + Math.random() * 0.2)
          .attr('stroke', z.color).attr('stroke-width', 0.4);
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
        if (Math.sqrt(dx * dx + dy * dy) > z.r * 0.5) return;
        var line = pktLayer.append('line').attr('x1', c1.x).attr('y1', c1.y)
          .attr('x2', c2.x).attr('y2', c2.y)
          .attr('stroke', z.color).attr('stroke-width', 0.5).attr('opacity', 0.3);
        line.transition().duration(800).attr('opacity', 0).on('end', function() { line.remove(); });
      } else if (z.arch === 'Federation') {
        internalCooldown[z.id] = epoch + 50;
        if (!z.clusterCenters || z.clusterCenters.length<2) return;
        var ci = Math.floor(Math.random()*z.clusterCenters.length);
        var cj = (ci+1+Math.floor(Math.random()*(z.clusterCenters.length-1)))%z.clusterCenters.length;
        var from=z.clusterCenters[ci], to=z.clusterCenters[cj];
        var p = pktLayer.append('circle').attr('cx', from.x).attr('cy', from.y)
          .attr('r', 2).attr('fill', z.color).attr('opacity', 0.8);
        p.transition().duration(350).ease(d3.easeLinear).attr('cx', z.x).attr('cy', z.y)
          .transition().duration(350).ease(d3.easeLinear).attr('cx', to.x).attr('cy', to.y)
          .transition().duration(200).attr('opacity', 0).on('end', function() { p.remove(); });
      } else if (z.arch === 'Meritocracy') {
        internalCooldown[z.id] = epoch + 60;
        var ag = zAgents[Math.floor(Math.random()*zAgents.length)];
        if (!ag || !ag.el) return;
        var up = Math.random()>0.4;
        ag.el.transition().duration(300).attr('fill', up?'#4ade80':'#ef4444').attr('r', up?5:2)
          .transition().duration(500).attr('fill', z.color).attr('r', 3+Math.random()*2);
      } else if (z.arch === 'Doctrine') {
        internalCooldown[z.id] = epoch + 45;
        var ag = zAgents[Math.floor(Math.random()*zAgents.length)];
        if (!ag) return;
        var ok = Math.random()>0.3;
        var p = pktLayer.append('circle').attr('cx', ag.x).attr('cy', ag.y)
          .attr('r', 1.5).attr('fill', z.color).attr('opacity', 0.8);
        p.transition().duration(400).ease(d3.easeLinear).attr('cx', z.x).attr('cy', z.y)
          .transition().duration(200).attr('fill', ok?'#4ade80':'#ef4444')
          .transition().duration(300).attr('opacity', 0).on('end', function() { p.remove(); });
      } else if (z.arch === 'Oligarchy') {
        internalCooldown[z.id] = epoch + 55;
        if (!z.elitePositions || z.elitePositions.length<2) return;
        var ei = Math.floor(Math.random()*z.elitePositions.length);
        var ej = (ei+1)%z.elitePositions.length;
        var from=z.elitePositions[ei], to=z.elitePositions[ej];
        var p = pktLayer.append('circle').attr('cx', from.x).attr('cy', from.y)
          .attr('r', 2).attr('fill', z.color).attr('opacity', 0.8);
        p.transition().duration(400).ease(d3.easeLinear).attr('cx', to.x).attr('cy', to.y)
          .transition().duration(200).attr('opacity', 0).on('end', function() { p.remove(); });
        var ring = pktLayer.append('circle').attr('cx', z.x).attr('cy', z.y)
          .attr('r', 5).attr('fill', 'none').attr('stroke', z.color).attr('stroke-width', 0.8).attr('opacity', 0.5);
        ring.transition().delay(500).duration(600).attr('r', z.r*0.7).attr('opacity', 0)
          .on('end', function() { ring.remove(); });
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

  // Cross-KB sharing is now handled via CM packets (zones talk to CMs, not each other)
  function crossKBPackets() {}

  // Zone-to-CM packets
  var cmCooldown = {};
  function cmPackets() {
    zones.forEach(function(z) {
      var tgt = zoneSharedTarget(z);
      if (!tgt) return;
      if (cmCooldown[z.id] > epoch) return;
      cmCooldown[z.id] = epoch + 300 + Math.floor(Math.random() * 200);
      var toT = Math.random() > 0.4;
      var pk = pktLayer.append('circle')
        .attr('cx', toT ? z.x : tgt.x).attr('cy', toT ? z.y : tgt.y)
        .attr('r', 2).attr('fill', '#059669').attr('opacity', 0.7);
      pk.transition().duration(1200).ease(d3.easeLinear)
        .attr('cx', toT ? tgt.x : z.x).attr('cy', toT ? tgt.y : z.y)
        .transition().duration(300).attr('opacity', 0)
        .on('end', function() { pk.remove(); });
    });
  }
  // KF-to-CM packets
  var kfCooldown = 0;
  function kfPackets() {
    if (kfCooldown > epoch) return;
    kfCooldown = epoch + 400 + Math.floor(Math.random() * 200);
    [CM_L, CM_R].forEach(function(cm) {
      var toKF = Math.random() > 0.5;
      var pk = pktLayer.append('circle')
        .attr('cx', toKF ? cm.x : KF_POS.x).attr('cy', toKF ? cm.y : KF_POS.y)
        .attr('r', 2.5).attr('fill', '#06b6d4').attr('opacity', 0.7);
      pk.transition().duration(1000).ease(d3.easeLinear)
        .attr('cx', toKF ? KF_POS.x : cm.x).attr('cy', toKF ? KF_POS.y : cm.y)
        .transition().duration(300).attr('opacity', 0)
        .on('end', function() { pk.remove(); });
    });
  }

  // Boundary events
  var eventLabel = labelLayer.append('text').attr('text-anchor', 'middle')
    .attr('font-size', '9px').attr('font-weight', 'bold').attr('fill', '#333').attr('opacity', 0);
  var eventFired = {};

  function checkEvents() {
    // Zone 6 (The Accord) contracts at epoch 800
    if (epoch === 800 && !eventFired[800]) {
      eventFired[800] = true;
      zones[6].r = 35;
      zones[6].circleEl.transition().duration(1200).attr('r', 35);
      flashEvent(zones[6].x, zones[6].y - 60, 'The Accord dissolving...');
    }
    // Zone 3 absorbs zone 6 at epoch 1200
    if (epoch === 1200 && !eventFired[1200]) {
      eventFired[1200] = true;
      zones[6].circleEl.transition().duration(800).attr('stroke-opacity', 0.1).attr('fill-opacity', 0.01);
      agents.forEach(function(a) {
        if (a.home === 6 && !a.traveling) {
          a.home = 3; a.visiting = -1;
          a.el.transition().duration(1500).attr('cx', zones[3].x + (Math.random()-0.5)*30)
            .attr('cy', zones[3].y + (Math.random()-0.5)*30).attr('fill', zones[3].color);
          a.x = zones[3].x; a.y = zones[3].y;
        }
      });
      flashEvent(zones[3].x, zones[3].y - 70, 'Exchange absorbs The Accord');
    }
    // Zones 4+8 merge at epoch 1600
    if (epoch === 1600 && !eventFired[1600]) {
      eventFired[1600] = true;
      var mergeX = (zones[4].x + zones[8].x) / 2, mergeY = (zones[4].y + zones[8].y) / 2;
      var arc = pktLayer.append('line')
        .attr('x1', zones[4].x).attr('y1', zones[4].y)
        .attr('x2', zones[8].x).attr('y2', zones[8].y)
        .attr('stroke', zones[4].color).attr('stroke-width', 3).attr('opacity', 0.6);
      setTimeout(function() {
        arc.transition().duration(600).attr('opacity', 0).on('end', function() { arc.remove(); });
        zones[4].r = 95;
        zones[4].circleEl.transition().duration(1200).attr('r', 95);
        zones[8].circleEl.transition().duration(1000).attr('stroke-opacity', 0.1).attr('fill-opacity', 0.01);
        agents.forEach(function(a) {
          if (a.home === 8 && !a.traveling) {
            a.home = 4; a.visiting = -1;
            a.el.transition().duration(1500).attr('fill', zones[4].color);
            a.x = zones[4].x + (Math.random()-0.5)*40; a.y = zones[4].y + (Math.random()-0.5)*40;
          }
        });
        flashEvent(mergeX, mergeY - 30, 'The Forge expands');
      }, 1500);
    }
    // Zone 7 (The Arena) schism at epoch 2200
    if (epoch === 2200 && !eventFired[2200]) {
      eventFired[2200] = true;
      flashEvent(zones[7].x, zones[7].y - 65, 'Arena schism');
      zones[7].r = 42;
      zones[7].circleEl.transition().duration(800).attr('r', 42);
      var splinter = pktLayer.append('circle')
        .attr('cx', zones[7].x - 50).attr('cy', zones[7].y - 30).attr('r', 0)
        .attr('fill', zones[7].color).attr('fill-opacity', 0.03)
        .attr('stroke', zones[7].color).attr('stroke-width', 1).attr('stroke-dasharray', '3,3').attr('stroke-opacity', 0.3);
      splinter.transition().duration(800).attr('r', 30)
        .transition().delay(4000).duration(1000).attr('r', 0).attr('opacity', 0)
        .on('end', function() { splinter.remove(); });
    }
    // Zone 9 (The Tribunal) KB contamination at epoch 2800
    if (epoch === 2800 && !eventFired[2800]) {
      eventFired[2800] = true;
      flashEvent(zones[9].x, zones[9].y - 65, 'Tribunal KB contaminated!');
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
    if (epoch % 15 === 0) cmPackets();
    if (epoch % 40 === 0) kfPackets();
    if (epoch % 8 === 0) humanPackets();
    checkEvents();
    rafId = requestAnimationFrame(tick);
  }

  // Two humans interacting with multiple societies
  var humanDefs = [
    {x: 360, y: 550, zones: [4, 5, 8]},   // center-bottom, connects Forge + Hive + Codex
    {x: 440, y: 185, zones: [1, 2, 3]}     // top-center, connects Mesh + Agora + Exchange
  ];
  var humanLayer = svg.append('g');

  function drawWorldPerson(layer, cx, cy, color) {
    var g = layer.append('g');
    g.append('circle').attr('cx', cx).attr('cy', cy - 14).attr('r', 5)
      .attr('fill', color).attr('stroke', d3.color(color).darker(0.5)).attr('stroke-width', 0.8);
    g.append('line').attr('x1', cx).attr('y1', cy - 9).attr('x2', cx).attr('y2', cy + 4)
      .attr('stroke', color).attr('stroke-width', 1.8);
    g.append('line').attr('x1', cx - 7).attr('y1', cy - 3).attr('x2', cx + 7).attr('y2', cy - 3)
      .attr('stroke', color).attr('stroke-width', 1.8);
    g.append('line').attr('x1', cx).attr('y1', cy + 4).attr('x2', cx - 6).attr('y2', cy + 15)
      .attr('stroke', color).attr('stroke-width', 1.8);
    g.append('line').attr('x1', cx).attr('y1', cy + 4).attr('x2', cx + 6).attr('y2', cy + 15)
      .attr('stroke', color).attr('stroke-width', 1.8);
    return g;
  }

  function drawHumans() {
    humanLayer.selectAll('*').remove();
    humanDefs.forEach(function(h) {
      // Connection lines to each zone (behind the person)
      h.zones.forEach(function(zi) {
        var z = zones[zi];
        humanLayer.append('line').attr('x1', h.x).attr('y1', h.y)
          .attr('x2', z.x).attr('y2', z.y)
          .attr('stroke', '#d97706').attr('stroke-width', 0.8)
          .attr('stroke-dasharray', '4,3').attr('opacity', 0.2);
      });
      drawWorldPerson(humanLayer, h.x, h.y, '#d97706');
      // Small label
      humanLayer.append('text').attr('x', h.x).attr('y', h.y + 22)
        .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#92400e').attr('opacity', 0.7)
        .text('Human');
    });
  }

  // Animated packets from humans to their connected zones
  var humanPktCooldown = 0;
  function humanPackets() {
    if (humanPktCooldown > epoch) return;
    humanPktCooldown = epoch + 120 + Math.floor(Math.random() * 80);
    var h = humanDefs[Math.floor(Math.random() * humanDefs.length)];
    var zi = h.zones[Math.floor(Math.random() * h.zones.length)];
    var z = zones[zi];
    var toZone = Math.random() > 0.4;
    var sx = toZone ? h.x : z.x, sy = toZone ? h.y : z.y;
    var ex = toZone ? z.x : h.x, ey = toZone ? z.y : h.y;
    var p = pktLayer.append('circle').attr('cx', sx).attr('cy', sy)
      .attr('r', 2).attr('fill', '#f59e0b').attr('opacity', 0.7);
    p.transition().duration(1000).ease(d3.easeLinear)
      .attr('cx', ex).attr('cy', ey)
      .transition().duration(250).attr('opacity', 0).remove();
  }

  function init() {
    if (rafId) cancelAnimationFrame(rafId);
    epoch = 0;
    eventFired = {};
    travelCooldown = {};
    kbCooldown = {};
    crossKBCooldown = {};
    internalCooldown = {};
    cmCooldown = {};
    kfCooldown = 0;
    travelers = [];
    // Reset zones to original state
    zones[6].r = 55;
    zones[4].r = 72;
    zones[7].r = 62;
    zones[8].r = 65;
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
    drawHumans();
    tick();
  }

  init();
  svg.on('click', function() { init(); });
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
  d3.select(container).style('overflow', 'hidden').style('height', height + 'px').style('min-height', height + 'px').style('max-height', height + 'px').style('position', 'relative');
  var svg = d3.select(container).append('svg')
    .attr('width', width).attr('height', height)
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .attr('preserveAspectRatio', 'xMidYMid meet')
    .style('position', 'absolute').style('top', '0').style('left', '0')
    .style('width', '100%').style('height', '100%')
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
// Visualization: Collective Memory and the Knowledge Factory
// ============================================================
(function() {
  var container = document.getElementById('viz-cmkf');
  if (!container) return;
  var width = 720, height = 560;
  d3.select(container).style('overflow', 'hidden').style('height', height + 'px')
    .style('min-height', height + 'px').style('max-height', height + 'px')
    .style('position', 'relative');
  var svg = d3.select(container).append('svg')
    .attr('width', width).attr('height', height)
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .attr('preserveAspectRatio', 'xMidYMid meet')
    .style('position', 'absolute').style('top', '0').style('left', '0')
    .style('width', '100%').style('height', '100%')
    .style('display', 'block').style('cursor', 'pointer');
  var rafId = null;

  var societyColors = ['#6366f1', '#e11d48', '#0891b2', '#d97706', '#059669', '#7c3aed', '#dc2626', '#0d9488'];
  var societyNames = ['Guild', 'Market', 'Colony', 'Senate', 'Doctrine', 'Federation', 'Autocracy', 'Meritocracy'];
  var cmColor = '#3b82f6';
  var kfColor = '#b91c1c';

  function init() {
    svg.selectAll('*').remove();
    if (rafId) cancelAnimationFrame(rafId);

    svg.append('rect').attr('width', width).attr('height', height).attr('fill', '#fafafa');

    var cx = width / 2, cy = height / 2 + 5;

    // --- Layout: three horizontal tiers ---
    // Top tier: societies (two groups of 3, one group of 2)
    // Middle tier: 3 CMs
    // Bottom tier: 2 KFs

    // Societies: arranged in three clusters along the top
    var societies = [
      // CM-1 cluster (left)
      { x: 95,  y: 80,  color: societyColors[0], name: societyNames[0] },
      { x: 185, y: 60,  color: societyColors[1], name: societyNames[1] },
      { x: 140, y: 130, color: societyColors[7], name: societyNames[7] },
      // CM-2 cluster (center)
      { x: 330, y: 65,  color: societyColors[2], name: societyNames[2] },
      { x: 420, y: 80,  color: societyColors[3], name: societyNames[3] },
      { x: 375, y: 130, color: societyColors[4], name: societyNames[4] },
      // CM-3 cluster (right)
      { x: 555, y: 75,  color: societyColors[5], name: societyNames[5] },
      { x: 630, y: 120, color: societyColors[6], name: societyNames[6] }
    ];

    var cms = [
      { x: 140, y: 240, label: 'CM-1', societies: [0, 1, 2] },
      { x: 375, y: 240, label: 'CM-2', societies: [3, 4, 5] },
      { x: 580, y: 240, label: 'CM-3', societies: [6, 7] }
    ];

    var kfs = [
      { x: 250, y: 400, label: 'KF-1', cms: [0, 1] },
      { x: 480, y: 400, label: 'KF-2', cms: [1, 2] }
    ];

    // Defs
    var defs = svg.append('defs');
    var glow = defs.append('filter').attr('id', 'glow-cmkf')
      .attr('x', '-50%').attr('y', '-50%').attr('width', '200%').attr('height', '200%');
    glow.append('feGaussianBlur').attr('stdDeviation', '2.5').attr('result', 'blur');
    var mg = glow.append('feMerge');
    mg.append('feMergeNode').attr('in', 'blur');
    mg.append('feMergeNode').attr('in', 'SourceGraphic');

    // --- Layer 1: Connections ---
    var connG = svg.append('g');

    // Society -> CM connections
    cms.forEach(function(cm) {
      cm.societies.forEach(function(si) {
        var s = societies[si];
        connG.append('path')
          .attr('d', 'M' + s.x + ',' + s.y + ' C' + s.x + ',' + (s.y + 50) + ' ' + cm.x + ',' + (cm.y - 50) + ' ' + cm.x + ',' + cm.y)
          .attr('fill', 'none').attr('stroke', '#e0e0e0').attr('stroke-width', 1.2);
      });
    });

    // CM -> KF connections
    kfs.forEach(function(kf) {
      kf.cms.forEach(function(ci) {
        var cm = cms[ci];
        connG.append('path')
          .attr('d', 'M' + cm.x + ',' + cm.y + ' C' + cm.x + ',' + (cm.y + 50) + ' ' + kf.x + ',' + (kf.y - 50) + ' ' + kf.x + ',' + kf.y)
          .attr('fill', 'none').attr('stroke', '#d0d0d0').attr('stroke-width', 1.5);
      });
    });

    // KF-1 to KF-2 connection (they share CM-2)
    connG.append('path')
      .attr('d', 'M' + kfs[0].x + ',' + kfs[0].y + ' C' + (kfs[0].x + 60) + ',' + (kfs[0].y + 30) + ' ' + (kfs[1].x - 60) + ',' + (kfs[1].y + 30) + ' ' + kfs[1].x + ',' + kfs[1].y)
      .attr('fill', 'none').attr('stroke', '#e8d0d0').attr('stroke-width', 1).attr('stroke-dasharray', '4,3');

    // --- Layer 2: Particles ---
    var pktG = svg.append('g');

    // --- Layer 3: Society nodes ---
    var sNodeG = svg.append('g');
    societies.forEach(function(s, si) {
      // Cluster halo
      sNodeG.append('circle')
        .attr('cx', s.x).attr('cy', s.y).attr('r', 28)
        .attr('fill', s.color).attr('opacity', 0.06)
        .attr('stroke', s.color).attr('stroke-width', 1).attr('stroke-opacity', 0.15);
      // Agent dots inside the society
      for (var d = 0; d < 6; d++) {
        var angle = (d / 6) * Math.PI * 2;
        var dr = 10 + Math.random() * 8;
        sNodeG.append('circle')
          .attr('cx', s.x + Math.cos(angle) * dr)
          .attr('cy', s.y + Math.sin(angle) * dr)
          .attr('r', 2 + Math.random() * 1.5)
          .attr('fill', s.color).attr('opacity', 0.4 + Math.random() * 0.2);
      }
      // Central node
      sNodeG.append('circle')
        .attr('cx', s.x).attr('cy', s.y).attr('r', 6)
        .attr('fill', s.color).attr('opacity', 0.85);
      // Label below
      sNodeG.append('text')
        .attr('x', s.x).attr('y', s.y + 38)
        .attr('text-anchor', 'middle').attr('font-size', '8px')
        .attr('font-weight', '600').attr('fill', s.color).attr('opacity', 0.8)
        .text(s.name);
    });

    // --- Layer 4: CM nodes (hexagons) ---
    var cmNodeG = svg.append('g');
    function hexPath(hx, hy, r) {
      var pts = [];
      for (var i = 0; i < 6; i++) {
        var a = (i / 6) * Math.PI * 2 - Math.PI / 6;
        pts.push((hx + Math.cos(a) * r) + ',' + (hy + Math.sin(a) * r));
      }
      return 'M' + pts.join('L') + 'Z';
    }
    cms.forEach(function(cm) {
      // Outer glow hex
      cmNodeG.append('path')
        .attr('d', hexPath(cm.x, cm.y, 32))
        .attr('fill', '#e8f0fe').attr('stroke', cmColor).attr('stroke-width', 1.5)
        .attr('stroke-opacity', 0.4).attr('filter', 'url(#glow-cmkf)');
      // Inner hex
      cmNodeG.append('path')
        .attr('d', hexPath(cm.x, cm.y, 24))
        .attr('fill', '#dbeafe').attr('stroke', cmColor).attr('stroke-width', 1.5);
      // Label
      cmNodeG.append('text')
        .attr('x', cm.x).attr('y', cm.y + 4)
        .attr('text-anchor', 'middle').attr('font-size', '10px')
        .attr('font-weight', 'bold').attr('fill', cmColor)
        .text(cm.label);
      // Sublabel
      cmNodeG.append('text')
        .attr('x', cm.x).attr('y', cm.y + 48)
        .attr('text-anchor', 'middle').attr('font-size', '7px')
        .attr('fill', '#999')
        .text('Collective Memory');
    });

    // --- Layer 5: KF nodes (rounded rects) ---
    var kfNodeG = svg.append('g');
    kfs.forEach(function(kf) {
      // Outer glow
      kfNodeG.append('rect')
        .attr('x', kf.x - 42).attr('y', kf.y - 22)
        .attr('width', 84).attr('height', 44).attr('rx', 8)
        .attr('fill', '#fef2f2').attr('stroke', kfColor).attr('stroke-width', 1.5)
        .attr('stroke-opacity', 0.4).attr('filter', 'url(#glow-cmkf)');
      // Inner rect
      kfNodeG.append('rect')
        .attr('x', kf.x - 34).attr('y', kf.y - 16)
        .attr('width', 68).attr('height', 32).attr('rx', 6)
        .attr('fill', '#fff').attr('stroke', kfColor).attr('stroke-width', 1.5);
      // Label
      kfNodeG.append('text')
        .attr('x', kf.x).attr('y', kf.y + 4)
        .attr('text-anchor', 'middle').attr('font-size', '10px')
        .attr('font-weight', 'bold').attr('fill', kfColor)
        .text(kf.label);
      // Sublabel
      kfNodeG.append('text')
        .attr('x', kf.x).attr('y', kf.y + 38)
        .attr('text-anchor', 'middle').attr('font-size', '7px')
        .attr('fill', '#999')
        .text('Knowledge Factory');
    });

    // --- Layer 6: Tier labels ---
    svg.append('text').attr('x', 18).attr('y', 28)
      .attr('font-size', '8px').attr('fill', '#bbb').attr('font-weight', '600')
      .text('SOCIETIES');
    svg.append('text').attr('x', 18).attr('y', 232)
      .attr('font-size', '8px').attr('fill', '#bbb').attr('font-weight', '600')
      .text('COLLECTIVE MEMORIES');
    svg.append('text').attr('x', 18).attr('y', 392)
      .attr('font-size', '8px').attr('fill', '#bbb').attr('font-weight', '600')
      .text('KNOWLEDGE FACTORIES');

    // --- Layer 7: Status ---
    var statusText = svg.append('text')
      .attr('x', width - 15).attr('y', 22)
      .attr('text-anchor', 'end').attr('font-size', '9px').attr('fill', '#aaa');

    var phaseText = svg.append('text')
      .attr('x', width / 2).attr('y', height - 10)
      .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#999').attr('font-style', 'italic');

    // --- Animation ---
    var t = 0;
    var lastEvent = -999;
    var eventInterval = 10; // seconds between events (slow)
    var events = ['ingestion', 'synthesis', 'gap-query', 'answer', 'contradiction', 'resolution'];
    var eventIdx = 0;

    // Interpolate point along a cubic bezier
    function bezierPt(x0, y0, cx1, cy1, cx2, cy2, x3, y3, t) {
      var u = 1 - t;
      return {
        x: u*u*u*x0 + 3*u*u*t*cx1 + 3*u*t*t*cx2 + t*t*t*x3,
        y: u*u*u*y0 + 3*u*u*t*cy1 + 3*u*t*t*cy2 + t*t*t*y3
      };
    }

    function emitCurveParticle(sx, sy, tx, ty, color, dur, r, downward) {
      r = r || 2.5;
      var cy1 = downward ? sy + 50 : sy - 50;
      var cy2 = downward ? ty - 50 : ty + 50;
      var p = pktG.append('circle')
        .attr('cx', sx).attr('cy', sy)
        .attr('r', r).attr('fill', color).attr('opacity', 0.7);
      var startTime = Date.now();
      function step() {
        var elapsed = Date.now() - startTime;
        var frac = Math.min(elapsed / dur, 1);
        var pt = bezierPt(sx, sy, sx, cy1, tx, cy2, tx, ty, frac);
        p.attr('cx', pt.x).attr('cy', pt.y);
        if (frac < 1) requestAnimationFrame(step);
        else p.transition().duration(300).attr('opacity', 0).remove();
      }
      requestAnimationFrame(step);
    }

    function emitQueryDiamond(sx, sy, tx, ty, color, dur, downward) {
      var cy1 = downward ? sy + 50 : sy - 50;
      var cy2 = downward ? ty - 50 : ty + 50;
      var p = pktG.append('rect')
        .attr('x', sx - 4).attr('y', sy - 4)
        .attr('width', 8).attr('height', 8)
        .attr('fill', color).attr('opacity', 0.85)
        .attr('transform', 'rotate(45,' + sx + ',' + sy + ')');
      var startTime = Date.now();
      function step() {
        var elapsed = Date.now() - startTime;
        var frac = Math.min(elapsed / dur, 1);
        var pt = bezierPt(sx, sy, sx, cy1, tx, cy2, tx, ty, frac);
        p.attr('x', pt.x - 4).attr('y', pt.y - 4)
         .attr('transform', 'rotate(45,' + pt.x + ',' + pt.y + ')');
        if (frac < 1) requestAnimationFrame(step);
        else p.transition().duration(300).attr('opacity', 0).remove();
      }
      requestAnimationFrame(step);
    }

    function animate() {
      t += 0.016;

      // Continuous gentle flow: societies -> CMs
      if (Math.random() < 0.018) {
        var ci = Math.floor(Math.random() * cms.length);
        var cm = cms[ci];
        var si = cm.societies[Math.floor(Math.random() * cm.societies.length)];
        var s = societies[si];
        emitCurveParticle(s.x, s.y, cm.x, cm.y, s.color, 2200 + Math.random() * 800, 2, true);
      }

      // Continuous gentle flow: CMs -> KFs
      if (Math.random() < 0.012) {
        var ki = Math.floor(Math.random() * kfs.length);
        var kf = kfs[ki];
        var ci2 = kf.cms[Math.floor(Math.random() * kf.cms.length)];
        emitCurveParticle(cms[ci2].x, cms[ci2].y, kf.x, kf.y, cmColor, 1800 + Math.random() * 600, 2, true);
      }

      // Timed events
      if (t - lastEvent > eventInterval) {
        lastEvent = t;
        var ev = events[eventIdx % events.length];
        eventIdx++;

        if (ev === 'ingestion') {
          statusText.text('ingestion');
          phaseText.text('Societies contribute findings to their Collective Memory');
          cms.forEach(function(cm) {
            cm.societies.forEach(function(si) {
              for (var p = 0; p < 3; p++) {
                (function(s, cm, delay) {
                  setTimeout(function() {
                    emitCurveParticle(s.x, s.y, cm.x, cm.y, s.color, 1800, 3, true);
                  }, delay);
                })(societies[si], cm, p * 400);
              }
            });
          });
        }

        else if (ev === 'synthesis') {
          statusText.text('synthesis');
          phaseText.text('CMs feed aggregated knowledge to Knowledge Factories');
          kfs.forEach(function(kf) {
            kf.cms.forEach(function(ci) {
              for (var p = 0; p < 4; p++) {
                (function(cm, kf, delay) {
                  setTimeout(function() {
                    emitCurveParticle(cm.x, cm.y, kf.x, kf.y, cmColor, 1500, 3, true);
                  }, delay);
                })(cms[ci], kf, p * 350);
              }
            });
          });
        }

        else if (ev === 'gap-query') {
          statusText.text('gap detected');
          phaseText.text('KF-1 identifies missing knowledge, queries back through CM-1');
          var kf0 = kfs[0], cm0 = cms[0];
          setTimeout(function() {
            emitQueryDiamond(kf0.x, kf0.y, cm0.x, cm0.y, kfColor, 1200, false);
          }, 300);
          cm0.societies.forEach(function(si, idx) {
            setTimeout(function() {
              emitQueryDiamond(cm0.x, cm0.y, societies[si].x, societies[si].y, kfColor, 1400, false);
            }, 1800 + idx * 500);
          });
        }

        else if (ev === 'answer') {
          statusText.text('answers return');
          phaseText.text('Societies respond; knowledge completes the loop back to KF-1');
          var cm0 = cms[0], kf0 = kfs[0];
          cm0.societies.forEach(function(si, idx) {
            setTimeout(function() {
              emitCurveParticle(societies[si].x, societies[si].y, cm0.x, cm0.y, societies[si].color, 1500, 3.5, true);
            }, idx * 450);
          });
          setTimeout(function() {
            emitCurveParticle(cm0.x, cm0.y, kf0.x, kf0.y, cmColor, 1300, 4, true);
          }, 2800);
        }

        else if (ev === 'contradiction') {
          statusText.text('contradiction');
          phaseText.text('CM-1 and CM-2 report conflicting findings; KF-1 consults Federation (CM-3)');
          var cm0 = cms[0], cm1 = cms[1], kf0 = kfs[0];
          for (var p = 0; p < 3; p++) {
            (function(delay) {
              setTimeout(function() {
                emitCurveParticle(cm0.x, cm0.y, kf0.x, kf0.y, '#ef4444', 1200, 4, true);
                emitCurveParticle(cm1.x, cm1.y, kf0.x, kf0.y, '#f59e0b', 1200, 4, true);
              }, delay);
            })(p * 400);
          }
          // KF queries CM-3's society (Federation) for independent opinion
          setTimeout(function() {
            emitQueryDiamond(kf0.x, kf0.y, cms[2].x, cms[2].y, kfColor, 1300, false);
          }, 2200);
          setTimeout(function() {
            emitQueryDiamond(cms[2].x, cms[2].y, societies[6].x, societies[6].y, kfColor, 1400, false);
          }, 3800);
        }

        else if (ev === 'resolution') {
          statusText.text('resolved');
          phaseText.text('Independent verdict flows back; KF distributes resolution to both CMs');
          setTimeout(function() {
            emitCurveParticle(societies[6].x, societies[6].y, cms[2].x, cms[2].y, societies[6].color, 1500, 4, true);
          }, 500);
          setTimeout(function() {
            emitCurveParticle(cms[2].x, cms[2].y, kfs[0].x, kfs[0].y, '#059669', 1300, 4, true);
          }, 2500);
          setTimeout(function() {
            emitCurveParticle(kfs[0].x, kfs[0].y, cms[0].x, cms[0].y, '#059669', 1200, 3, false);
            emitCurveParticle(kfs[0].x, kfs[0].y, cms[1].x, cms[1].y, '#059669', 1200, 3, false);
          }, 4200);
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

// viz-forces is now a static inline SVG -- no script needed

// ============================================================
// Visualization: viz-phases (Four Interaction Phases)
// ============================================================
(function() {
  var container = document.getElementById('viz-phases');
  if (!container) return;
  var W = 720, H = 360;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + W + ' ' + H)
    .style('cursor', 'pointer')
    .attr('tabindex', 0);

  svg.append('rect').attr('width', W).attr('height', H).attr('fill', '#fafafa');

  var panels = [
    { x: 15,  w: 162, label: 'Isolation',    color: '#1e293b' },
    { x: 187, w: 162, label: 'Ecosystem Growth', color: '#2563eb' },
    { x: 359, w: 162, label: 'Society',      color: '#059669' },
    { x: 531, w: 162, label: 'Interweaving', color: '#b91c1c' }
  ];

  // Panel background rects
  var panelBgs = panels.map(function(p) {
    return svg.append('rect')
      .attr('x', p.x).attr('y', 25).attr('width', p.w).attr('height', 285)
      .attr('fill', 'none').attr('rx', 4);
  });

  // --- P1: Isolation ---
  var p1cx = 15 + 81, p1cy = 160;
  var modelAngles = [0, 72, 144, 216, 288].map(function(d) { return d * Math.PI / 180; });
  modelAngles.forEach(function(a) {
    var mx = p1cx + 55 * Math.cos(a), my = p1cy + 55 * Math.sin(a);
    svg.append('line').attr('x1', p1cx).attr('y1', p1cy).attr('x2', mx).attr('y2', my)
      .attr('stroke', '#ddd').attr('stroke-width', 0.8);
    svg.append('circle').attr('cx', mx).attr('cy', my).attr('r', 5).attr('fill', '#94a3b8');
  });
  svg.append('circle').attr('cx', p1cx).attr('cy', p1cy).attr('r', 9).attr('fill', '#1e293b');
  svg.append('circle').attr('cx', p1cx).attr('cy', p1cy).attr('r', 3).attr('fill', 'white');

  // --- P2: Distillation ---
  var p2ox = 187;
  var p2hx = p2ox + 81, p2hy = 100;
  var p2fx = p2ox + 60, p2fy = 210;
  var p2dx = p2ox + 130, p2dy = 210;
  svg.append('line').attr('x1', p2hx).attr('y1', p2hy).attr('x2', p2fx).attr('y2', p2fy)
    .attr('stroke', '#ddd').attr('stroke-width', 1);
  svg.append('line').attr('x1', p2hx).attr('y1', p2hy).attr('x2', p2dx).attr('y2', p2dy)
    .attr('stroke', '#ddd').attr('stroke-width', 1);
  svg.append('line').attr('x1', p2fx).attr('y1', p2fy).attr('x2', p2dx).attr('y2', p2dy)
    .attr('stroke', '#2563eb').attr('stroke-width', 1).attr('stroke-dasharray', '3,3').attr('opacity', 0.4);
  svg.append('circle').attr('cx', p2fx).attr('cy', p2fy).attr('r', 6).attr('fill', '#1e293b');
  svg.append('circle').attr('cx', p2dx).attr('cy', p2dy).attr('r', 4.5).attr('fill', '#475569');
  svg.append('circle').attr('cx', p2hx).attr('cy', p2hy).attr('r', 8).attr('fill', '#1e293b');
  svg.append('circle').attr('cx', p2hx).attr('cy', p2hy).attr('r', 2.5).attr('fill', 'white');
  var p2packet = svg.append('circle').attr('r', 2).attr('fill', '#2563eb').attr('opacity', 0)
    .attr('cx', p2fx).attr('cy', p2fy);

  // --- P3: Society ---
  var p3ox = 359;
  var p3lx = p3ox + 55, p3ly = 150;
  var p3rx = p3ox + 115, p3ry = 200;
  var p3hx = p3ox + 80, p3hy = 60;
  var p3lBound = svg.append('circle').attr('cx', p3lx).attr('cy', p3ly).attr('r', 38)
    .attr('fill', 'none').attr('stroke', '#1e293b').attr('stroke-dasharray', '4,3').attr('opacity', 0.2);
  var p3rBound = svg.append('circle').attr('cx', p3rx).attr('cy', p3ry).attr('r', 35)
    .attr('fill', 'none').attr('stroke', '#475569').attr('stroke-dasharray', '4,3').attr('opacity', 0.2);
  [[-10,-12],[10,-8],[0,10]].forEach(function(d) {
    svg.append('circle').attr('cx', p3lx+d[0]).attr('cy', p3ly+d[1]).attr('r', 4).attr('fill', '#1e293b');
  });
  [[-8,-8],[8,-4],[0,12]].forEach(function(d) {
    svg.append('circle').attr('cx', p3rx+d[0]).attr('cy', p3ry+d[1]).attr('r', 3.5).attr('fill', '#475569');
  });
  svg.append('line').attr('x1', p3hx).attr('y1', p3hy).attr('x2', p3lx).attr('y2', p3ly)
    .attr('stroke', '#ddd').attr('stroke-width', 1);
  svg.append('line').attr('x1', p3lx).attr('y1', p3ly).attr('x2', p3rx).attr('y2', p3ry)
    .attr('stroke', '#ddd').attr('stroke-width', 0.8).attr('stroke-dasharray', '3,3');
  svg.append('circle').attr('cx', p3hx).attr('cy', p3hy).attr('r', 7).attr('fill', '#1e293b');
  svg.append('circle').attr('cx', p3hx).attr('cy', p3hy).attr('r', 2.5).attr('fill', 'white');

  // --- P4: Interweaving ---
  var p4ox = 531;
  var p4cx1 = p4ox + 80, p4cy1 = 160;
  var p4cx2 = p4ox + 120, p4cy2 = 200;
  var p4hx = p4ox + 70, p4hy = 140;
  svg.append('circle').attr('cx', p4cx1).attr('cy', p4cy1).attr('r', 50)
    .attr('fill', 'none').attr('stroke', '#b91c1c').attr('stroke-dasharray', '4,3').attr('opacity', 0.2);
  svg.append('circle').attr('cx', p4cx2).attr('cy', p4cy2).attr('r', 30)
    .attr('fill', 'none').attr('stroke', '#b91c1c').attr('stroke-dasharray', '4,3').attr('opacity', 0.15);
  var p4models = [
    [p4cx1-20, p4cy1+15, '#94a3b8'],
    [p4cx1+18, p4cy1+20, '#475569'],
    [p4cx1+5,  p4cy1-20, '#1e293b']
  ];
  p4models.forEach(function(m) {
    svg.append('circle').attr('cx', m[0]).attr('cy', m[1]).attr('r', 4).attr('fill', m[2]);
  });
  [[-8,0],[8,0]].forEach(function(d) {
    svg.append('circle').attr('cx', p4cx2+d[0]).attr('cy', p4cy2+d[1]).attr('r', 3).attr('fill', '#475569');
  });
  svg.append('line').attr('x1', p4hx).attr('y1', p4hy).attr('x2', p4models[0][0]).attr('y2', p4models[0][1])
    .attr('stroke', '#b91c1c').attr('stroke-width', 1).attr('opacity', 0.5);
  svg.append('line').attr('x1', p4hx).attr('y1', p4hy).attr('x2', p4models[1][0]).attr('y2', p4models[1][1])
    .attr('stroke', '#b91c1c').attr('stroke-width', 1).attr('opacity', 0.5);
  svg.append('circle').attr('cx', p4hx).attr('cy', p4hy).attr('r', 8).attr('fill', '#1e293b');
  svg.append('circle').attr('cx', p4hx).attr('cy', p4hy).attr('r', 2.5).attr('fill', 'white');

  // Phase labels
  panels.forEach(function(p) {
    svg.append('text').attr('x', p.x + p.w/2).attr('y', 315)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('font-weight', 'bold')
      .attr('fill', p.color).text(p.label);
  });

  // Phase indicator rects
  var indicators = panels.map(function(p) {
    return svg.append('rect')
      .attr('x', p.x).attr('y', 325).attr('width', p.w).attr('height', 10)
      .attr('fill', '#f0f0f0').attr('rx', 2);
  });

  // Sliding dot under indicators
  var slideDot = svg.append('circle').attr('r', 4).attr('cy', 330)
    .attr('cx', panels[0].x + panels[0].w/2)
    .attr('fill', panels[0].color).attr('opacity', 0.7);

  var activePanelIdx = 0;
  var cycleTimer = null;
  var p2timer = null;
  var p4timer = null;

  function activatePanel(idx) {
    // Deactivate old
    panelBgs[activePanelIdx].attr('fill', 'none').attr('stroke', 'none');
    indicators[activePanelIdx].attr('fill', '#f0f0f0').attr('stroke', 'none');

    // Stop sub-timers from old panel
    if (p2timer) { clearTimeout(p2timer); p2timer = null; p2packet.attr('opacity', 0); }
    if (p4timer) { clearTimeout(p4timer); p4timer = null; }

    activePanelIdx = idx;
    var p = panels[idx];

    panelBgs[idx].attr('fill', p.color).attr('opacity', 0.06)
      .attr('stroke', p.color).attr('stroke-width', 0.5);
    indicators[idx].attr('fill', p.color).attr('opacity', 0.15)
      .attr('stroke', p.color).attr('stroke-width', 1);
    slideDot.transition().duration(400).attr('cx', p.x + p.w/2).attr('fill', p.color);

    if (idx === 1) {
      // Packet travels A2A line repeatedly
      function sendP2() {
        if (activePanelIdx !== 1) return;
        p2packet.attr('opacity', 0.9).attr('cx', p2fx).attr('cy', p2fy);
        p2packet.transition().duration(700)
          .attr('cx', p2dx).attr('cy', p2dy)
          .transition().duration(200).attr('opacity', 0);
        p2timer = setTimeout(sendP2, 1200);
      }
      sendP2();
    }

    if (idx === 2) {
      // Boundary circles breathe
      function breathe() {
        if (activePanelIdx !== 2) return;
        p3lBound.transition().duration(700).attr('r', 41).attr('opacity', 0.35)
          .transition().duration(700).attr('r', 38).attr('opacity', 0.2)
          .on('end', breathe);
      }
      breathe();
      p3rBound.transition().duration(700).attr('r', 38).attr('opacity', 0.3)
        .transition().duration(700).attr('r', 35).attr('opacity', 0.2);
    } else {
      p3lBound.attr('r', 38).attr('opacity', 0.2);
      p3rBound.attr('r', 35).attr('opacity', 0.2);
    }

    if (idx === 3) {
      // Bidirectional packets travel
      function sendP4() {
        if (activePanelIdx !== 3) return;
        var pkt1 = svg.append('circle').attr('r', 2.5).attr('fill', '#b91c1c').attr('opacity', 0.8)
          .attr('cx', p4hx).attr('cy', p4hy);
        pkt1.transition().duration(600)
          .attr('cx', p4models[0][0]).attr('cy', p4models[0][1])
          .transition().duration(200).attr('opacity', 0)
          .on('end', function() { pkt1.remove(); });
        var pkt2 = svg.append('circle').attr('r', 2.5).attr('fill', '#b91c1c').attr('opacity', 0.8)
          .attr('cx', p4models[1][0]).attr('cy', p4models[1][1]);
        pkt2.transition().duration(600)
          .attr('cx', p4hx).attr('cy', p4hy)
          .transition().duration(200).attr('opacity', 0)
          .on('end', function() { pkt2.remove(); });
        p4timer = setTimeout(sendP4, 1100);
      }
      sendP4();
    }
  }

  function startCycle() {
    activatePanel(0);
    var cur = 0;
    function next() {
      cur++;
      if (cur >= panels.length) {
        // Full round complete -- restart after brief pause
        cycleTimer = setTimeout(function() { init(); }, 2500);
        return;
      }
      activatePanel(cur);
      cycleTimer = setTimeout(next, 2500);
    }
    cycleTimer = setTimeout(next, 2500);
  }

  function init() {
    if (cycleTimer) clearTimeout(cycleTimer);
    if (p2timer) clearTimeout(p2timer);
    if (p4timer) clearTimeout(p4timer);
    p2timer = null; p4timer = null;
    activePanelIdx = 0;
    panelBgs.forEach(function(b) { b.attr('fill', 'none'); });
    indicators.forEach(function(ind) { ind.attr('fill', '#f0f0f0').attr('stroke', 'none'); });
    p2packet.attr('opacity', 0);
    startCycle();
  }

  init();
  svg.on('click', function() { init(); });
})();

// ============================================================
// Visualization: viz-ecology-why (Barrier Circles)
// ============================================================
(function() {
  var container = document.getElementById('viz-ecology-why');
  if (!container) return;
  var W = 720, H = 350;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + W + ' ' + H)
    .style('cursor', 'pointer')
    .attr('tabindex', 0);

  svg.append('rect').attr('width', W).attr('height', H).attr('fill', '#fafafa');

  var CX = 360, CY = 165;

  // Center circle -- slightly larger, softer
  svg.append('circle').attr('cx', CX).attr('cy', CY).attr('r', 40)
    .attr('fill', '#e2e8f0').attr('opacity', 0.5);
  svg.append('text').attr('x', CX).attr('y', CY - 5)
    .attr('text-anchor', 'middle').attr('font-size', '13px').attr('font-weight', 'bold')
    .attr('fill', '#1e293b').text('ONE');
  svg.append('text').attr('x', CX).attr('y', CY + 11)
    .attr('text-anchor', 'middle').attr('font-size', '13px').attr('font-weight', 'bold')
    .attr('fill', '#1e293b').text('MODEL?');

  // Ecology cluster below center
  var ecoColors = ['#2563eb', '#059669', '#d97706', '#b91c1c', '#7c3aed'];
  var ECX = CX, ECY = CY + 85;
  svg.append('circle').attr('cx', ECX).attr('cy', ECY).attr('r', 22)
    .attr('fill', '#f1f5f9').attr('stroke', '#cbd5e1')
    .attr('stroke-dasharray', '3,3').attr('opacity', 0.6);
  [[0,-10],[9,3],[-9,3],[5,-4],[-5,9]].forEach(function(d, i) {
    svg.append('circle').attr('cx', ECX + d[0]).attr('cy', ECY + d[1])
      .attr('r', 3.5).attr('fill', ecoColors[i]).attr('opacity', 0.7);
  });

  // Six barriers arranged in arc above center
  var barriers = [
    { label: 'Privacy',        sub: 'incompatible legal regimes',  color: '#b91c1c', bg: '#fecaca' },
    { label: 'Specialization', sub: 'private data as moat',        color: '#2563eb', bg: '#bfdbfe' },
    { label: 'Latency',        sub: 'edge vs cloud',               color: '#d97706', bg: '#fed7aa' },
    { label: 'Resilience',     sub: 'monoculture risk',            color: '#059669', bg: '#a7f3d0' },
    { label: 'Learning',       sub: 'distributed experience',      color: '#7c3aed', bg: '#ddd6fe' },
    { label: 'Oracle',         sub: 'knowledge is distributed',    color: '#475569', bg: '#cbd5e1' }
  ];

  // Arc: spread from -160deg to -20deg (above center, leaving bottom open)
  var arcStart = -160, arcEnd = -20;
  var arcR = 140;
  barriers.forEach(function(b, i) {
    var angleDeg = arcStart + (arcEnd - arcStart) * i / (barriers.length - 1);
    var angleRad = angleDeg * Math.PI / 180;
    b.bx = CX + arcR * Math.cos(angleRad);
    b.by = CY + arcR * Math.sin(angleRad);
  });

  // Lines from each barrier to center (drawn first so circles sit on top)
  var barrierLines = barriers.map(function(b) {
    return svg.append('line')
      .attr('x1', b.bx).attr('y1', b.by)
      .attr('x2', CX).attr('y2', CY)
      .attr('stroke', b.color).attr('stroke-width', 1).attr('opacity', 0.08);
  });

  // Barrier circles -- pastel fill with dark text
  var BR = 24;
  var barrierCircles = barriers.map(function(b) {
    var g = svg.append('g');
    var circ = g.append('circle').attr('cx', b.bx).attr('cy', b.by).attr('r', BR)
      .attr('fill', b.bg).attr('stroke', b.color).attr('stroke-width', 1.5).attr('opacity', 0.85);
    g.append('text').attr('x', b.bx).attr('y', b.by + 1)
      .attr('text-anchor', 'middle').attr('dominant-baseline', 'middle')
      .attr('font-size', '9px').attr('font-weight', '600').attr('fill', '#1e293b')
      .text(b.label);
    return { g: g, circ: circ };
  });

  // Bottom label area
  var labelName = svg.append('text').attr('x', CX).attr('y', 318)
    .attr('text-anchor', 'middle').attr('font-size', '11px').attr('font-weight', 'bold')
    .attr('fill', '#333').attr('opacity', 0);
  var labelSub = svg.append('text').attr('x', CX).attr('y', 334)
    .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#666').attr('opacity', 0);

  var activeIdx = 0;
  var cycleCount = 0;
  var cycleTimer = null;

  function deactivate(idx) {
    barrierCircles[idx].circ.transition().duration(300)
      .attr('opacity', 0.85).attr('r', BR).attr('stroke-width', 1.5);
    barrierLines[idx].transition().duration(300).attr('opacity', 0.08);
  }

  function activate(idx) {
    var b = barriers[idx];
    barrierCircles[idx].circ.transition().duration(200)
      .attr('r', BR + 3).attr('opacity', 1).attr('stroke-width', 2.5)
      .transition().duration(200).attr('r', BR);
    barrierLines[idx].transition().duration(300).attr('opacity', 0.35);
    labelName.transition().duration(150).attr('opacity', 0).on('end', function() {
      labelName.attr('fill', b.color).text(b.label)
        .transition().duration(250).attr('opacity', 1);
    });
    labelSub.transition().duration(150).attr('opacity', 0).on('end', function() {
      labelSub.text(b.sub).transition().duration(250).attr('opacity', 1);
    });
  }

  function consolidationAttempt(targetIdx) {
    var b = barriers[targetIdx];
    var dot = svg.append('circle').attr('r', 3).attr('fill', '#1e293b').attr('opacity', 0.6)
      .attr('cx', ECX).attr('cy', ECY);
    dot.transition().duration(550)
      .attr('cx', b.bx).attr('cy', b.by)
      .on('end', function() {
        dot.remove();
        barrierCircles[targetIdx].circ
          .transition().duration(100).attr('stroke-width', 3)
          .transition().duration(200).attr('stroke-width', 1.5);
        var ripple = svg.append('circle')
          .attr('cx', b.bx).attr('cy', b.by).attr('r', BR)
          .attr('fill', 'none').attr('stroke', b.color).attr('stroke-width', 1.5).attr('opacity', 0.35);
        ripple.transition().duration(400).attr('r', BR + 16).attr('opacity', 0)
          .on('end', function() { ripple.remove(); });
      });
  }

  function runCycle() {
    deactivate(activeIdx);
    activeIdx = (activeIdx + 1) % barriers.length;
    activate(activeIdx);
    cycleCount++;
    if (cycleCount % 3 === 0) {
      setTimeout(function() { consolidationAttempt(activeIdx); }, 500);
    }
    if (activeIdx === 0) {
      cycleTimer = setTimeout(function() { init(); }, 2500);
    } else {
      cycleTimer = setTimeout(runCycle, 1800);
    }
  }

  function init() {
    if (cycleTimer) clearTimeout(cycleTimer);
    cycleCount = 0;
    activeIdx = 0;
    barriers.forEach(function(b, i) { deactivate(i); });
    labelName.attr('opacity', 0);
    labelSub.attr('opacity', 0);
    activate(0);
    cycleTimer = setTimeout(runCycle, 1800);
  }

  init();
  svg.on('click', function() { init(); });
})();

</script>