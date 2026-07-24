---
title: "The Agent Fabric (Part 2): Division of Labour and Governance"
subtitle: "How work gets done within societies, from delegation to governance archetypes"
date: 2026-05-11
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "multi-agent systems", "LLM", "agent fabric", "governance", "the loom hypothesis"]
description: "How work gets done within agent societies: from multi-agent systems to governance archetypes."
draft: true
math: false
ShowToc: true
TocOpen: false
hideCitation: false
wordcount: "~6,800 words (body) · ~21,800 words (notes + patterns)"
---

<style>
  .viz-container {
    width: 100%;
    max-width: 720px;
    margin: 2em auto;
    background: #fafafa;
    border: 1px solid #e5e5e5;
    border-radius: 8px;
    overflow: visible;
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
    .viz-restart, button.viz-restart,
    .click-to-restart { display: none !important; }
    .viz-caption details { display: none !important; }
    .viz-container {
      break-inside: avoid;
      page-break-inside: avoid;
      overflow: visible;
      margin: 1em auto;
    }
    .viz-container > div[style*="height"] {
      height: auto !important;
    }
    .viz-container svg {
      max-height: 90vh;
    }
    .viz-caption {
      position: static !important;
      margin-top: 0.5rem;
      page-break-before: avoid;
    }
    .viz-expanded {
      position: static !important;
      transform: none !important;
      box-shadow: none !important;
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
    if(vc.querySelector('#viz-delegation, #viz-society')) return;
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

<p style="font-size: 0.82em; color: #999; margin-top: 1em;"><a href="https://code.claude.com/docs/en/overview" target="_blank" style="color: #999;">Claude Code</a> was used for editing and visualizations. All ideas and arguments are the authors' own.</p>


<div style="background: #fffbeb; border: 1px solid #f59e0b; border-radius: 6px; padding: 0.6em 1em; margin: 1.5em 0; font-size: 0.88em; color: #92400e;">
<strong>Early access.</strong> This blog series is a work in progress. Feedback, comments, and suggestions are welcome. Feel free to <a href="https://www.linkedin.com/in/kasra-hosseini/" target="_blank" style="color: #92400e;">reach out on LinkedIn</a> or leave a comment at the bottom of the page.
</div>

*This post is part of The Agent Fabric blog series. [Part 1](/posts/2026/agent-fabric-part1/) established two observations. First, agents persist only while useful to a deployer. Second, resources are finite but agent populations can grow much faster than the resources available to run them (and these bounds shift as societies form and reshape their environment). Together these produce the Loom Hypothesis, the pressure toward connected, coordinated configurations. Part 1 also traced two origins for such societies: the top-down path, where a company or team designs the structure, and the bottom-up path, where individuals assemble agents from what is available. Those two origins pull in different directions, a distinction this post develops. Societies built top-down tend toward hierarchy and doctrine; those assembled bottom-up tend toward markets and custodianship. This post picks up where that pressure leads, exploring how agents divide work and how governance structures form around them.*

*Two ways to read this. The body text develops the argument (~6,800 words). The expandable sections are a reference manual (~21,800 words of pattern cards, examples, and scenarios). You can read one without the other.*

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **[Prologue: The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/)**: the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis, and the path from isolation to interweaving
- **Part 2: Division of Labour and Governance** (you are here): delegation archetypes, trust and authority, the specialist market, and governance archetypes
</div>

<div class="viz-container">
  <div id="viz-scale" style="width: 100%; max-width: 720px; margin: 0 auto;" role="img" aria-label="Four panels in a 2x2 grid showing the progression from a single agent to a multi-agent team to a society to the fabric where societies interconnect.">
<svg viewBox="0 0 680 520" style="width:100%; height:auto; display:block;">
<rect width="680" height="520" fill="#fafafa" rx="4"/>
<!-- === Panel 1: Agent (top-left, center 170,110) === -->
<text x="170" y="22" text-anchor="middle" font-size="13" fill="#475569" font-weight="600" font-family="sans-serif">Agent</text>
<text x="170" y="40" text-anchor="middle" font-size="9" fill="#888" font-family="sans-serif">one model, one task</text>
<!-- Task arrow in -->
<line x1="100" y1="110" x2="133" y2="110" stroke="#b91c1c" stroke-width="1.5"/>
<polygon points="133,106 142,110 133,114" fill="#b91c1c"/>
<text x="104" y="101" font-size="8" fill="#b91c1c" font-family="sans-serif">task</text>
<!-- Single agent -->
<circle cx="170" cy="110" r="18" fill="#1e293b" opacity="0.08" stroke="#1e293b" stroke-width="1.2"/>
<circle cx="170" cy="110" r="8" fill="#1e293b" opacity="0.7"/>
<!-- Result arrow out -->
<line x1="196" y1="110" x2="224" y2="110" stroke="#059669" stroke-width="1.5"/>
<polygon points="224,106 233,110 224,114" fill="#059669"/>
<text x="209" y="101" font-size="8" fill="#059669" font-family="sans-serif">result</text>
<text x="170" y="168" text-anchor="middle" font-size="9" fill="#64748b" font-family="sans-serif">A single model handles</text>
<text x="170" y="182" text-anchor="middle" font-size="9" fill="#64748b" font-family="sans-serif">a request end to end.</text>
<!-- Arrow 1→2 (right) -->
<line x1="295" y1="110" x2="338" y2="110" stroke="#059669" stroke-width="2"/>
<polygon points="338,105 350,110 338,115" fill="#059669"/>
<text x="322" y="97" text-anchor="middle" font-size="9" fill="#059669" font-weight="600" font-family="sans-serif">scale</text>
<!-- === Panel 2: Team (top-right, center 510,110) === -->
<text x="510" y="22" text-anchor="middle" font-size="13" fill="#475569" font-weight="600" font-family="sans-serif">Multi-Agent System</text>
<text x="510" y="40" text-anchor="middle" font-size="9" fill="#888" font-family="sans-serif">multiple agents, coordinated</text>
<!-- Task arrow in -->
<line x1="425" y1="82" x2="460" y2="82" stroke="#b91c1c" stroke-width="1.2"/>
<polygon points="460,78 468,82 460,86" fill="#b91c1c"/>
<!-- Root agent -->
<circle cx="490" cy="82" r="10" fill="#1e293b" opacity="0.7"/>
<!-- Delegation lines -->
<line x1="490" y1="92" x2="460" y2="120" stroke="#475569" stroke-width="0.8" opacity="0.4"/>
<line x1="490" y1="92" x2="510" y2="125" stroke="#475569" stroke-width="0.8" opacity="0.4"/>
<line x1="490" y1="92" x2="550" y2="115" stroke="#475569" stroke-width="0.8" opacity="0.4"/>
<!-- Sub-agents -->
<circle cx="460" cy="123" r="6" fill="#475569" opacity="0.65"/>
<circle cx="510" cy="128" r="5" fill="#475569" opacity="0.6"/>
<circle cx="550" cy="118" r="5.5" fill="#475569" opacity="0.6"/>
<!-- Deeper delegation -->
<line x1="460" y1="129" x2="445" y2="148" stroke="#475569" stroke-width="0.6" opacity="0.3"/>
<line x1="460" y1="129" x2="475" y2="148" stroke="#475569" stroke-width="0.6" opacity="0.3"/>
<circle cx="445" cy="151" r="3.5" fill="#94a3b8" opacity="0.5"/>
<circle cx="475" cy="151" r="3.5" fill="#94a3b8" opacity="0.5"/>
<!-- Result arrow out -->
<line x1="568" y1="82" x2="592" y2="82" stroke="#059669" stroke-width="1.2"/>
<polygon points="592,78 600,82 592,86" fill="#059669"/>
<text x="510" y="178" text-anchor="middle" font-size="9" fill="#64748b" font-family="sans-serif">Multiple agents coordinate</text>
<text x="510" y="192" text-anchor="middle" font-size="9" fill="#64748b" font-family="sans-serif">via <tspan font-weight="600" fill="#1e293b">delegation</tspan> patterns.</text>
<!-- Arrow 1→3 (down) -->
<line x1="170" y1="200" x2="170" y2="243" stroke="#059669" stroke-width="2"/>
<polygon points="165,243 170,255 175,243" fill="#059669"/>
<text x="185" y="232" text-anchor="start" font-size="9" fill="#059669" font-weight="600" font-family="sans-serif">govern</text>
<!-- === Panel 3: Society (bottom-left, center 170,370) === -->
<text x="170" y="272" text-anchor="middle" font-size="13" fill="#475569" font-weight="600" font-family="sans-serif">Society</text>
<text x="170" y="290" text-anchor="middle" font-size="9" fill="#888" font-family="sans-serif">ongoing structure, many tasks</text>
<!-- Society boundary -->
<ellipse cx="170" cy="360" rx="80" ry="55" fill="#2563eb" opacity="0.04" stroke="#2563eb" stroke-width="1" stroke-dasharray="5,4"/>
<!-- Governance node -->
<circle cx="170" cy="325" r="9" fill="#b91c1c" opacity="0.7"/>
<text x="170" y="329" text-anchor="middle" font-size="7" fill="white" font-weight="bold" font-family="sans-serif">GOV</text>
<!-- Connections from GOV -->
<line x1="170" y1="334" x2="135" y2="360" stroke="#475569" stroke-width="0.8" opacity="0.35"/>
<line x1="170" y1="334" x2="170" y2="368" stroke="#475569" stroke-width="0.8" opacity="0.35"/>
<line x1="170" y1="334" x2="205" y2="360" stroke="#475569" stroke-width="0.8" opacity="0.35"/>
<!-- Peer connections -->
<line x1="135" y1="360" x2="170" y2="368" stroke="#2563eb" stroke-width="0.6" opacity="0.25"/>
<line x1="170" y1="368" x2="205" y2="360" stroke="#2563eb" stroke-width="0.6" opacity="0.25"/>
<line x1="135" y1="360" x2="150" y2="392" stroke="#2563eb" stroke-width="0.6" opacity="0.25"/>
<line x1="205" y1="360" x2="190" y2="392" stroke="#2563eb" stroke-width="0.6" opacity="0.25"/>
<!-- Agents -->
<circle cx="135" cy="360" r="6" fill="#2563eb" opacity="0.65"/>
<circle cx="170" cy="371" r="5.5" fill="#2563eb" opacity="0.6"/>
<circle cx="205" cy="360" r="6" fill="#2563eb" opacity="0.65"/>
<circle cx="150" cy="395" r="4.5" fill="#2563eb" opacity="0.5"/>
<circle cx="190" cy="395" r="4.5" fill="#2563eb" opacity="0.5"/>
<!-- Shared memory -->
<rect x="220" y="385" width="20" height="15" rx="3" fill="#059669" opacity="0.15" stroke="#059669" stroke-width="0.8"/>
<text x="230" y="396" text-anchor="middle" font-size="7" fill="#059669" font-family="sans-serif">KB</text>
<!-- Multiple task arrows -->
<line x1="70" y1="340" x2="88" y2="340" stroke="#b91c1c" stroke-width="0.8" opacity="0.5"/>
<polygon points="88,337 93,340 88,343" fill="#b91c1c" opacity="0.5"/>
<line x1="70" y1="360" x2="88" y2="360" stroke="#b91c1c" stroke-width="0.8" opacity="0.5"/>
<polygon points="88,357 93,360 88,363" fill="#b91c1c" opacity="0.5"/>
<line x1="70" y1="380" x2="88" y2="380" stroke="#b91c1c" stroke-width="0.8" opacity="0.5"/>
<polygon points="88,377 93,380 88,383" fill="#b91c1c" opacity="0.5"/>
<text x="170" y="432" text-anchor="middle" font-size="9" fill="#64748b" font-family="sans-serif">Agents relate across many tasks.</text>
<text x="170" y="446" text-anchor="middle" font-size="9" fill="#64748b" font-family="sans-serif">This is <tspan font-weight="600" fill="#1e293b">governance</tspan>.</text>
<!-- Arrow 3→4 (right) -->
<line x1="295" y1="370" x2="338" y2="370" stroke="#059669" stroke-width="2"/>
<polygon points="338,365 350,370 338,375" fill="#059669"/>
<text x="322" y="357" text-anchor="middle" font-size="9" fill="#059669" font-weight="600" font-family="sans-serif">interweave</text>
<!-- === Panel 4: Fabric (bottom-right, center 510,370) === -->
<text x="510" y="272" text-anchor="middle" font-size="13" fill="#475569" font-weight="600" font-family="sans-serif">Fabric</text>
<text x="510" y="290" text-anchor="middle" font-size="9" fill="#888" font-family="sans-serif">societies interconnected</text>
<!-- Cross-society connections (behind nodes) -->
<line x1="475" y1="345" x2="525" y2="340" stroke="#f59e0b" stroke-width="1.5" opacity="0.4"/>
<line x1="462" y1="365" x2="498" y2="382" stroke="#f59e0b" stroke-width="1.5" opacity="0.4"/>
<line x1="558" y1="365" x2="530" y2="382" stroke="#f59e0b" stroke-width="1.5" opacity="0.4"/>
<!-- Society A boundary -->
<ellipse cx="450" cy="345" rx="50" ry="38" fill="#2563eb" opacity="0.04" stroke="#2563eb" stroke-width="0.8" stroke-dasharray="4,3"/>
<circle cx="435" cy="338" r="4" fill="#2563eb" opacity="0.55"/>
<circle cx="455" cy="332" r="3.5" fill="#2563eb" opacity="0.5"/>
<circle cx="445" cy="358" r="3.5" fill="#2563eb" opacity="0.5"/>
<circle cx="468" cy="350" r="3" fill="#2563eb" opacity="0.45"/>
<circle cx="450" cy="318" r="5.5" fill="#b91c1c" opacity="0.6"/>
<text x="450" y="321" text-anchor="middle" font-size="5" fill="white" font-weight="bold" font-family="sans-serif">G</text>
<!-- Society B boundary -->
<ellipse cx="570" cy="345" rx="50" ry="38" fill="#7c3aed" opacity="0.04" stroke="#7c3aed" stroke-width="0.8" stroke-dasharray="4,3"/>
<circle cx="555" cy="338" r="3.5" fill="#7c3aed" opacity="0.5"/>
<circle cx="575" cy="332" r="4" fill="#7c3aed" opacity="0.55"/>
<circle cx="565" cy="358" r="3" fill="#7c3aed" opacity="0.45"/>
<circle cx="585" cy="350" r="3.5" fill="#7c3aed" opacity="0.5"/>
<circle cx="570" cy="318" r="5.5" fill="#b91c1c" opacity="0.6"/>
<text x="570" y="321" text-anchor="middle" font-size="5" fill="white" font-weight="bold" font-family="sans-serif">G</text>
<!-- Society C boundary (below) -->
<ellipse cx="510" cy="400" rx="45" ry="32" fill="#059669" opacity="0.04" stroke="#059669" stroke-width="0.8" stroke-dasharray="4,3"/>
<circle cx="498" cy="395" r="3.5" fill="#059669" opacity="0.5"/>
<circle cx="520" cy="392" r="3" fill="#059669" opacity="0.45"/>
<circle cx="507" cy="412" r="3" fill="#059669" opacity="0.45"/>
<circle cx="510" cy="378" r="5.5" fill="#b91c1c" opacity="0.6"/>
<text x="510" y="381" text-anchor="middle" font-size="5" fill="white" font-weight="bold" font-family="sans-serif">G</text>
<text x="510" y="447" text-anchor="middle" font-size="9" fill="#64748b" font-family="sans-serif">Societies share resources,</text>
<text x="510" y="461" text-anchor="middle" font-size="9" fill="#64748b" font-family="sans-serif">protocols, and knowledge.</text>
<!-- Bottom insight -->
<line x1="40" y1="478" x2="640" y2="478" stroke="#e2e8f0" stroke-width="1"/>
<text x="340" y="498" text-anchor="middle" font-size="12" fill="#1e293b" font-weight="600" font-family="sans-serif">Delegation is how work flows. Governance is who decides and on what authority.</text>
<text x="340" y="514" text-anchor="middle" font-size="10" fill="#475569" font-family="sans-serif">The fabric is what forms when societies themselves begin to interconnect.</text>
</svg>
  </div>
  <div class="viz-caption"><strong>Figure 1. Four levels of agent organization.</strong> A single agent handles one task. A multi-agent system coordinates multiple agents through delegation patterns. A society persists across tasks, developing governance and shared memory. The fabric emerges when societies interconnect. Green arrows mark three transitions: <em>scale</em>, <em>persist</em>, <em>interweave</em>.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">First panel: a single model handles a request end to end. This is how most people use AI today. Second panel: a multi-agent system, where multiple agents coordinate through delegation patterns (chains, trees, routers, swarms). A frontier model dispatches to specialists, sub-agents handle sub-tasks, results flow back. Some multi-agent systems are temporary (assembled for one task), others are persistent infrastructure (a router that runs continuously). Third panel: a society forms when agents persist beyond any single task, developing governance (a GOV node), peer connections, and shared knowledge (KB). Multiple tasks flow through simultaneously. Fourth panel: the fabric, where multiple societies (shown in blue, purple, and green) interconnect through cross-society links (orange). Each society retains its own governance node (G), but resources, protocols, and knowledge flow across boundaries. This post focuses on multi-agent delegation and on the governance that turns multi-agent systems into societies; the fabric was introduced in Part 1.</p>
  </details></div>
</div>

Among the questions Part 1 left open, the most immediate is: how does work actually get done within agent societies?

You already use delegation every time a coding agent splits a refactor across a planner, a code writer, a test runner, and a security scanner. You interacted with one agent; several did the work. Your personal agent uses delegation when it books travel. It contacts an airline agent, a hotel agent, a payment agent. You did not design that structure; the task demanded it. Not all delegation is planned, not all of it repeats, and not all of it involves choice.

**Delegation** describes how a task flows through a group of agents. Who does what, who checks the result, and who bears the compute, latency, and error cost of each step. **Governance** describes the rules that shape how decisions are made across a society. Who gets authority, how trust is established, and how that authority can be challenged. A multi-agent system can exist without governance (a stateless data-processing pipeline is delegation, not a society). In Part 1 we defined a society as a multi-agent system that develops shared context, interaction-dependent routing, and cross-agent learning. Governance is the institutional structure that emerges from (or is designed into) those three properties.

The central design problem is not "how many agents should I use?" It is **who should do this work, and on what basis should that decision be made?**

<div style="background: #f0f4ff; border: 1px solid #2563eb; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>Core claim.</strong> Delegation becomes governance when the system remembers who did what, how well, at what cost, and under whose authority. Delegation describes how work flows; governance describes who decides. The transition between them is less a design choice than a structural tendency: any system that records those four things and acts on the record has begun to govern, whether its designers intended governance or not. Left unchecked, the mechanism tends to run one way. Operational data hardens into routing preference, routing preference becomes standing, and standing constrains future decisions. A system can be reset, kept stateless, or redeployed to break the ratchet, but that takes a deliberate choice; absent it, the system begins to govern before anyone decides it should. The patterns below show how work is split; the governance archetypes show how to make that governance visible and accountable before it becomes load-bearing.
</div>

## The Building Block

The building block of every pattern in this post is the **augmented foundation model**: a model (LLM or multimodal) extended with memory that persists across calls, tools that act on the world, and planning that decomposes multi-step goals. This is the same thing the companion post calls a model plus its **scaffolding**; we use the shorter term here. A bare model maps an input to an output; an augmented one loops, remembers, and acts. The augmented model is the atom; delegation and governance are the chemistry. For the loop that turns a model into an agent, the components inside it, and the structural limits of a single agent working alone, see the companion post [The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/). This post picks up where that one ends: what happens when many such agents compose.

## The Division of Labour

The first question any multi-agent system must answer is how work gets split. At small scale, a single orchestrator can assign tasks. As the number of agents involved in a task grows, the delegation structure itself becomes the architecture, spanning hierarchies and specialist markets. (At the very large scales Part 1 envisions, where agents number in the millions and beyond, the question shifts from delegation to governance. How societies coordinate across many tasks over time is the subject of the second section of this post.)

### Delegation Archetypes

The forty-three patterns below fall into nine families, plus one meta-pattern (Adaptive Delegation) that operates on the structure itself. The families are not rigid boundaries; several patterns bridge families (Speculative Execution combines quality verification with market-like competition). But the grouping reduces cognitive load and helps practitioners navigate to the relevant subset.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">The nine delegation families (43 patterns + 1 meta-pattern)</summary>

| Family | Patterns | Core question |
|--------|----------|---------------|
| **Sequential** | Chain, Pipeline, Router, Escalation, Relay/Handoff, Loop/Retry | How does a task flow from start to finish? |
| **Hierarchical** | Tree, Map-Reduce, Supervisor, Orchestrator, Mission Command, Feudal Delegation | How is a complex task decomposed and reassembled? |
| **Quality & Verification** | Evaluator, Voting, Critic/Red Team, Debate, Witness/Notarization, Verification Game | How do we verify that output is good enough? |
| **Reliability** | Circuit Breaker, Timeout/Dead-Man, Checkpoint/Saga, Canary/Shadow | How do we handle failure without cascade? |
| **Market & Competition** | Auction, Negotiation, Speculative Execution, Contract Net | How is work allocated competitively? |
| **Knowledge Transfer** | Teacher-Student, Federated Learning, Blackboard, Distillation | How do agents share what they know? |
| **Emergent Coordination** | Publish-Subscribe, Choreography, Stigmergy, Swarm, Gossip | How do agents coordinate without a center? |
| **Trust & Authority** | Privilege Attenuation, Liability Firebreaks, Capability Credentials, Zone of Indifference | Who is allowed to delegate to whom, and with what? |
| **Human-Agent Interface** | Human-in-the-Loop Gate, Human-on-the-Loop, Policy Delegation, Approval Escalation | How do humans structurally participate in delegation? |

Tomašev et al.'s "Intelligent AI Delegation" (2026) proposes a complementary decision framework organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. The families here describe the *how*; their framework addresses the *whether* and *to whom*.

**Diagnostic shortcuts.** When classifying a system, ask: *Does the shared state have a schema?* (Yes = Blackboard, no = Stigmergy.) *Does the coordinator decompose the task or just review the output?* (Decomposes = Orchestrator, reviews = Supervisor.) *Is the competitive allocation one-to-many or bilateral?* (One-to-many = Auction, bilateral = Negotiation.) *Is verification constructive or adversarial?* (Constructive = Evaluator, adversarial = Critic.) *Does the caller persist after handoff?* (Yes = Chain, no = Relay.) These are not definitions, but fast boundary tests for the confusable cases.
</details>

<div class="viz-container">
  <div id="viz-delegation" style="width: 100%;" role="img" aria-label="Delegation archetypes grouped by family, each animated with task and result packets. Click any panel to expand."></div>
  <div class="viz-caption"><strong>Figure 2. Delegation archetypes.</strong> Forty-three patterns in nine families, plus one meta-pattern, describing how tasks are decomposed, routed, checked, and reassembled. Any pattern can be deliberately designed or can emerge from agent interaction.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">Any delegation pattern can be deliberately designed or can emerge from agent interaction. A Swarm can be engineered (deploy 50 drones with specified behaviors) or can arise when agents independently discover that self-organizing works. A Chain can be hardcoded or can emerge when agents learn to pass work sequentially. The designed/emergent distinction is a property of how a pattern was instantiated, not of the pattern itself. Most real systems combine multiple patterns. Red packets are tasks; green are results. Click any panel to expand.</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-delegation').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Summary: delegation archetypes at a glance</summary>

| Archetype | Structure | When to use | Failure mode |
|-----------|-----------|-------------|--------------|
| **Chain** | Sequential hand-off, A&#8594;B&#8594;C | Linear workflows with clear stage boundaries | Early error propagates through every stage |
| **Pipeline** | Transform at each stage | Data processing, ETL, content moderation | Stage failure blocks the whole pipeline |
| **Router** | Classify and direct to specialist | Query triage, multi-domain assistants | Misclassification sends work to wrong specialist |
| **Escalation** | Try small, fail up to larger model | Cost-sensitive systems with variable difficulty | Bad confidence estimates: escalates too much or too little |
| **Tree** | Hierarchical fan-out and merge | Complex tasks requiring decomposition | Deep trees multiply cost and latency |
| **Map-Reduce** | Parallel execution + aggregation | Embarrassingly parallel sub-problems | Aggregator discards minority evidence or mis-merges partial results |
| **Supervisor** | Frontier model reviews smaller models' work | Production systems with cost/quality tradeoff | Bottleneck at the supervisor; single point of quality failure |
| **Orchestrator** | Dynamic decomposition and re-planning | Open-ended tasks where structure emerges | Orchestrator misjudges decomposition, re-plans endlessly |
| **Evaluator** | Generate, critique, refine loop | Quality-sensitive outputs (code, writing, reasoning) | Over-filtering or infinite refinement loops |
| **Relay / Handoff** | Pass full control + context, caller exits | Agent-to-agent handoffs where the caller must not persist | Dropped context or ambiguous ownership at handoff boundary |
| **Circuit Breaker** | Stop calling failing downstream agents | Resilient agent networks, API integrations | Opens too aggressively (transient errors) or too slowly (cascade) |
| **Timeout / Dead-Man** | Absence of signal triggers response | Every real deployment needing liveness monitoring | Too short (kills slow work) or too long (wastes time on dead agents) |
| **Canary / Shadow** | Test new agent on live traffic before promotion | Model upgrades, prompt changes, safe rollout | Shadow period too short (misses rare failures) or too long (wasted compute) |
| **Publish-Subscribe** | Event-driven, fully decoupled | Large-scale agent coordination, notification systems | Message storms, lost messages, implicit debugging |
| **Voting** | Same task, majority wins | High-stakes decisions needing reliability | Correlated errors (all voters wrong the same way) |
| **Critic / Red Team** | Generate output, then adversarially attack it | Security review, red-teaming, adversarial robustness | Critic too strong (nothing passes) or too weak (nothing caught) |
| **Debate** | Multi-round adversarial argument with judge | High-stakes reasoning, alignment verification | Judge capture, infinite regress, performative argumentation |
| **Speculative Exec.** | Race parallel approaches, commit first valid | Latency-sensitive tasks, non-deterministic workloads | Wasted compute; committing a fast but incorrect result |
| **Teacher-Student** | Strong model trains weaker model via examples | Model distillation, capability transfer, fine-tune data generation | Distribution mismatch between teacher examples and real tasks |
| **Checkpoint / Saga** | Multi-step task with rollback on failure | Long-running workflows, distributed transactions, unreliable sub-agents | Rollback storms; checkpoint overhead on fast short tasks |
| **Blackboard** | Shared workspace, indirect coordination | Collaborative problem-solving, heterogeneous agents | Noise accumulation without curation |
| **Mission Command** | Communicate intent, not method | Delegating to capable agents in uncertain conditions | Intent ambiguity leads to divergent interpretations |
| **Federated Learning** | Collaborative training, no data sharing | Privacy-preserving improvement across organizations | Poisoned updates, non-IID data distribution issues |
| **Auction** | Agents bid for work | Task allocation across heterogeneous specialists | Bid gaming, race to the bottom on quality |
| **Negotiation** | Bilateral offer/counter-offer between agents | Agent-to-agent deals on behalf of principals | Deadlock, no ZOPA, principal misrepresentation |
| **Witness / Notarization** | Independent third-party verification | Multi-party trust, cross-organizational interactions | Witness becomes bottleneck or single point of trust failure |
| **Choreography** | Decentralized event-driven coordination | Systems where centralized orchestration is a bottleneck | Invisible workflow, emergent deadlocks |
| **Stigmergy** | Environment as communication medium | Large heterogeneous populations with no shared protocol | Environmental noise, trace poisoning |
| **Swarm** | Collective behavior without central control | Exploration, creative search, resilient systems | Collective goals diverge from intended goals |
| **Gossip** | Peer-to-peer information spread | Decentralized coordination, norm propagation | Rumor drift, norm poisoning |
| | | | |
| **Loop / Retry** | Re-execute with accumulated feedback | Iterative refinement without separate critic | Infinite loops, cycling between bad solutions |
| **Feudal Delegation** | Goals down, results up, methods hidden | Scaling without holding full reasoning trace | Undetectable misalignment at depth |
| **Verification Game** | Game-theoretic proof without re-execution | Expensive computation where re-run is prohibitive | Prover-verifier collusion |
| **Contract Net** | Broadcast, bid, award, execute, report | Open systems with unknown agent capabilities | Unbounded recursive sub-contracting |
| **Distillation** | Large model trains small, then disconnects | Cost/latency constraints in production | Capability collapse on distribution shift |
| **Privilege Attenuation** | Sub-delegates get strict permission subsets | Any system where sub-agents touch external resources | Over-attenuation starves leaf agents |
| **Liability Firebreaks** | Explicit responsibility transfer points | Consequential delegation across org boundaries | Too narrow (overhead) or too broad (diffuse blame) |
| **Capability Credentials** | Verifiable attestations of competence | Open composition of agents from different sources | Credential inflation, staleness, gaming |
| **Zone of Indifference** | Range of instructions followed without pushback | Diagnosing why delegation chains stall or comply | Too wide (safety) or too narrow (throughput) |
| **Human-in-the-Loop Gate** | Execution blocks for human approval | High-stakes, low-frequency, irreversible actions | Gate fatigue from over-triggering |
| **Human-on-the-Loop** | Human monitors, can intervene, not blocking | Long-running workflows needing awareness | Automation complacency (Bainbridge 1983) |
| **Policy Delegation** | Human sets constraints, agent acts within bounds | High-volume repetitive tasks | Specification gaming, policy staleness |
| **Approval Escalation** | Only anomalies reach the human | High-throughput with occasional risk | Miscalibrated escalation threshold |

Any pattern can be deliberately designed or can emerge from agent interaction. The designed/emergent distinction belongs to the deployment, not the pattern. A deployer can engineer a Swarm (deploy agents whose interactions produce collective behavior) just as readily as a Chain. Most real systems combine multiple patterns; see Figure 3 for composition examples.

**Adaptive Delegation** (meta-pattern): the delegation structure itself changes based on performance signals. Every other pattern can be a source or target state in an adaptive transition.
</details>


<div class="gov-list">

**Sequential patterns** route tasks along a defined path. Work flows forward; each agent sees the output of the previous one. These are the delegation primitives, and most complex patterns compose from sequences.

<details>
<summary>Chain <span class="gov-tagline">sequential hand-off</span></summary>
<div class="gov-body">Each agent completes its step and passes the result to the next. The simplest multi-agent pattern: A finishes, hands to B, B finishes, hands to C. No parallelism, no branching. The output of each stage is the input of the next.<br><br><em>When to use:</em> linear workflows with clear stage boundaries where each step depends on the previous one. Content pipelines (draft, edit, fact-check, format), sequential approval chains, multi-step data transformations.<br><br><em>Example:</em> a customer email arrives; agent A classifies intent, agent B drafts a response, agent C checks for policy compliance, agent D sends it.<br><br><em>Failure mode:</em> an error in stage 1 propagates through every subsequent stage. No recovery without restarting the chain.<br><br><em>Relation to other patterns:</em> Chain is the minimal form of Pipeline, passing results without transformation contracts, and it is the building block of most other patterns. Relay differs in that the caller exits permanently after handoff.</div>
</details>

<details>
<summary>Pipeline <span class="gov-tagline">transform at each stage</span></summary>
<div class="gov-body">Data flows through a sequence of stages, each applying a specific transformation. Unlike Chain (which just passes results), Pipeline stages have defined input/output contracts and transformation logic.<br><br><em>When to use:</em> data processing, ETL workflows, content moderation pipelines, document processing (extract, classify, enrich, store).<br><br><em>Example:</em> a document ingestion system: stage 1 extracts text from PDF, stage 2 chunks and embeds, stage 3 classifies by topic, stage 4 stores in vector database.<br><br><em>Failure mode:</em> a stage failure blocks the entire pipeline. Backpressure issues when downstream stages are slower than upstream.<br><br><em>Relation to other patterns:</em> Pipeline is Chain with transformation contracts. Combined with Auction, you get a Bidding Pipeline where each stage is awarded competitively.</div>
</details>

<details>
<summary>Router <span class="gov-tagline">classify and direct</span></summary>
<div class="gov-body">A single routing agent classifies the input and directs it to the appropriate specialist. The router does not do the work; it decides who should. This is the gateway pattern for any system with heterogeneous specialists.<br><br><em>When to use:</em> multi-domain assistants, query triage, systems where different inputs need fundamentally different handling.<br><br><em>Example:</em> a customer service system routes billing questions to the billing agent, technical issues to the tech agent, returns to the returns agent. In the Zalando Assistant, LLM-based routing matched or beat specialized classifiers on recall, though precision required careful tuning.<br><br><em>Failure mode:</em> misclassification sends work to the wrong specialist. The hardest cases are ambiguous inputs that belong to multiple domains.<br><br><em>Relation to other patterns:</em> Router is the entry point for most production systems. Combined with Escalation, it creates tiered service. It differs from Orchestrator in that it makes a single routing decision rather than decomposing a task.</div>
</details>

<details>
<summary>Escalation <span class="gov-tagline">try small, fail up</span></summary>
<div class="gov-body">Start with the cheapest capable agent. If it fails or confidence is low, escalate to a more capable (and expensive) one. A cost-optimization strategy that exploits the observation that most tasks are easy.<br><br><em>When to use:</em> cost-sensitive systems with variable difficulty. Support desks, model cascades, any setting where 80% of queries are straightforward and only 20% need a frontier model.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2305.05176" target="_blank" class="red-link">FrugalGPT</a> demonstrates that cascading through models of increasing capability, stopping as soon as confidence is high enough, can reduce costs substantially while preserving quality. The same principle applies across hardware tiers: a smart speaker's keyword spotter handles "set a timer," escalates to the on-device language model for "summarize my morning," and reaches a cloud frontier model only for "draft a response to this legal notice."<br><br><em>Failure mode:</em> bad confidence estimates cause over-escalation (expensive) or under-escalation (poor quality). If the small model does not know what it does not know, escalation fails silently. When the entire escalation chain fails (no agent can resolve the task, or the human-in-the-loop is unavailable), the task needs a dead-letter path: park it, log it, surface it asynchronously. Without this, unresolvable tasks either loop forever or vanish silently.<br><br><em>Relation to other patterns:</em> Escalation is vertical routing (by capability level) while Router is horizontal routing (by domain). They compose naturally.</div>
</details>

<details>
<summary>Relay / Handoff <span class="gov-tagline">pass control, caller exits</span></summary>
<div class="gov-body">Full control and context transfer: the caller hands off to another agent and terminates. Unlike Chain, where the caller waits for a result, the Relay agent is done once it hands off. Ownership transfers completely.<br><br><em>When to use:</em> agent-to-agent handoffs where the originator should not persist. Language routing (user speaks French, hand off to the French-speaking agent), domain transitions (general assistant hands to specialist), shift changes in long-running tasks.<br><br><em>Example:</em> OpenAI's Agents SDK uses handoffs as a first-class primitive: an agent calls a "transfer_to_X" tool, passing execution context to agent X. The originating agent ceases to exist in the conversation.<br><br><em>Failure mode:</em> dropped context at the handoff boundary. If the receiving agent does not get enough context, it starts from scratch. Ambiguous ownership: who is responsible for the outcome after handoff?<br><br><em>Relation to other patterns:</em> Relay is a Chain where the sender terminates after handoff rather than waiting for a return. It becomes interesting as a building block when agents join and leave societies throughout a session.</div>
</details>

<details>
<summary>Loop / Retry-with-Context <span class="gov-tagline">self-refinement through accumulated feedback</span></summary>
<div class="gov-body">An agent re-executes the same task with feedback from previous attempts appended to its context, iterating until a stopping condition is satisfied. The key structural distinction from the Evaluator pattern is that no separate critic agent exists; the same agent revises its own output, using prior failures as additional context rather than as external scores.<br><br><em>When to use:</em> Tasks where quality improves with iteration and where failure feedback is cheap to generate, such as code generation, structured extraction, or chain-of-thought reasoning. Best suited when the output space is well-defined enough that the agent can recognize improvement.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2310.03714" target="_blank" class="red-link">DSPy</a>'s optimization loop (Khattab et al. 2023) compiles prompts by iterating over demonstrations until a metric threshold is reached. AutoGen's reflection pattern similarly prompts a single agent to critique and rewrite its previous response before returning a final answer.<br><br><em>Failure mode:</em> Without a hard iteration cap and explicit convergence criteria, the loop runs indefinitely. Agents can also converge to locally coherent but globally wrong outputs, cycling between the same two bad solutions without progress.<br><br><em>Relation to other patterns:</em> Evaluator adds a structurally separate critic, making quality judgment an external check rather than self-assessment. Checkpoint/Saga checkpoints intermediate states for recovery, whereas Loop/Retry-with-Context does not persist state across iterations.</div>
</details>

**Hierarchical patterns** decompose tasks into sub-tasks and reassemble results. Authority flows downward; results flow upward. If each delegation hop preserves only a fraction of the principal's intent, alignment degrades multiplicatively with depth (a concern Tomašev et al. (2026) raise qualitatively as an "accountability vacuum" in long delegation chains), making the shallowest viable tree the safer design.

<details>
<summary>Tree <span class="gov-tagline">hierarchical fan-out</span></summary>
<div class="gov-body">A root agent decomposes a task into sub-tasks, delegates each to a child agent, and children may decompose further. Results flow back up and are merged at each level. This is the natural shape of complex task decomposition.<br><br><em>When to use:</em> complex tasks requiring decomposition into independent sub-problems. Code refactoring (split by module), research tasks (split by sub-question), document analysis (split by section).<br><br><em>Example:</em> a coding agent splits a refactoring task: one sub-agent handles the database layer, another the API endpoints, a third the test suite. Each may further decompose.<br><br><em>Failure mode:</em> deep trees multiply cost and latency exponentially. A tree that is three levels deep with fan-out of four spawns 64 leaf agents. The winning tree is the shallowest one that reliably produces a good enough answer.<br><br><em>Relation to other patterns:</em> Tree is the generalization of Chain (linear tree) and Map-Reduce (one-level fan-out). Orchestrator is a Tree where the decomposition is dynamic rather than predetermined.</div>
</details>

<details>
<summary>Map-Reduce <span class="gov-tagline">parallel + aggregation</span></summary>
<div class="gov-body">A task is split into independent sub-problems, each solved in parallel by separate agents, then results are aggregated by a reducer. The sub-problems must be independent; the power comes from parallelism.<br><br><em>When to use:</em> embarrassingly parallel sub-problems. Analyzing many documents simultaneously, processing many customer records, running the same analysis across multiple datasets.<br><br><em>Example:</em> summarize a 500-page report: split into 50 sections, assign each to a summarizer agent in parallel, then aggregate the summaries into a final document.<br><br><em>Failure mode:</em> the aggregator is the bottleneck. If it discards minority evidence or mis-merges partial results, the parallel work is wasted. Also fails when sub-problems are not truly independent.<br><br><em>Relation to other patterns:</em> Map-Reduce is a one-level Tree with parallel execution. Combined with Voting at the reduce step, you get the Consensus Engine composition.</div>
</details>

<details>
<summary>Supervisor / Hierarchical Review <span class="gov-tagline">frontier model reviews smaller models</span></summary>
<div class="gov-body">A more capable model dispatches work to less capable models, reviews their output, and accepts or rejects with feedback. Rejected work is re-dispatched with the review attached. The key distinction from Orchestrator: the Supervisor is explicitly hierarchical, with a capability gap between reviewer and worker.<br><br><em>When to use:</em> any production system where a frontier model oversees smaller specialists. This is the dominant pattern in coding agents (Claude Code, Cursor, Copilot Workspace), customer service systems, and any deployment where cost requires using cheaper models for most work while maintaining quality standards.<br><br><em>Example:</em> Claude Code spawns sub-agents for file search, code editing, and test execution. The main agent (a frontier model) reviews each sub-agent's output before accepting it. If a sub-agent produces inadequate code, the supervisor re-dispatches with specific feedback about what was wrong.<br><br><em>Failure mode:</em> bottleneck at the supervisor. If every task requires frontier-model review, you lose the cost savings of using smaller models. Over-reliance on one model's judgment creates a single point of quality failure.<br><br><em>Relation to other patterns:</em> Supervisor differs from Orchestrator (which decomposes tasks dynamically but does not necessarily review output) and from Evaluator (which is a constructive improvement loop, not hierarchical quality-gating). Many production agent systems are Supervisors whether they call themselves that or not.</div>
</details>

<details>
<summary>Orchestrator <span class="gov-tagline">dynamic decomposition</span></summary>
<div class="gov-body">An orchestrator agent receives a task, dynamically decomposes it into sub-tasks (the decomposition is not predetermined), dispatches to workers, monitors progress, and re-plans when sub-tasks fail or new information arrives. The structure emerges at runtime.<br><br><em>When to use:</em> open-ended tasks where the decomposition cannot be known in advance. Complex coding tasks, research questions, any problem where the first attempt reveals what the next steps should be.<br><br><em>Example:</em> Anthropic's <a href="https://www.anthropic.com/engineering/building-effective-agents" target="_blank" class="red-link">orchestrator-workers</a> pattern: the orchestrator plans, dispatches sub-tasks to specialized workers, synthesizes results, and re-plans when needed. Microsoft's <a href="https://arxiv.org/abs/2411.04468" target="_blank" class="red-link">Magentic-One</a> uses this pattern with a lead agent that tracks progress and reassigns work dynamically.<br><br><em>Failure mode:</em> the orchestrator misjudges the decomposition and re-plans endlessly. Without a budget or iteration limit, orchestration can become an infinite loop of planning.<br><br><em>Relation to other patterns:</em> Orchestrator is a dynamic Tree. It differs from Supervisor (which reviews output but does not dynamically re-decompose) and from Chain (which has a fixed sequence). The boundary between Orchestrator and Tree is whether the decomposition is known at the start.</div>
</details>

<details>
<summary>Mission Command <span class="gov-tagline">intent without method</span></summary>
<div class="gov-body">The delegating agent communicates the goal and the reason (intent) but deliberately does not prescribe the method. Subordinate agents exercise autonomous judgment in selecting how to achieve the intent, adapting to conditions without waiting for instructions.<br><br><em>When to use:</em> delegating to capable agents in situations where the delegator cannot anticipate local conditions. Complex coding tasks ("make this module thread-safe" rather than "add a lock on line 47"), research tasks ("find the root cause" rather than "check these three files"), any task where method prescription would be counterproductive.<br><br><em>Example:</em> every good prompt engineer uses mission command instinctively. "Write tests for this module that cover edge cases" is mission command. "Open test_module.py, add a function called test_edge_case_1 that asserts..." is not. The deliberate withholding of method specification is the key innovation. The pattern is essential for embodied agents: tell a delivery robot "get this package to room 312" rather than prescribing every turn, because the robot knows the building and the corridor conditions better than you do.<br><br><em>Failure mode:</em> intent ambiguity. If the goal is vague, autonomous agents may pursue reasonable but divergent interpretations. The quality of the intent statement determines the quality of the outcome.<br><br><em>Relation to other patterns:</em> Mission Command is the philosophical complement to Supervisor. The Supervisor reviews output after the fact; Mission Command shapes behavior before execution by specifying intent clearly enough that autonomous execution is safe. Source: military doctrine (Prussian Auftragstaktik, NATO Allied Joint Publication, US Army FM 6-0).</div>
</details>

<details>
<summary>Feudal Delegation <span class="gov-tagline">goals cascade down, results bubble up, methods stay hidden</span></summary>
<div class="gov-body">Higher-level agents assign abstract goals to lower-level agents without specifying or observing how those goals are implemented. Authority flows downward as goal assignments; results flow upward as outcomes. The implementing agent's reasoning, tool calls, and intermediate steps are fully opaque to its principal. The hierarchy coordinates on objectives, not methods.<br><br><em>When to use:</em> Systems that need to scale across many sub-tasks without the orchestrator holding the full reasoning trace in context. Appropriate when sub-agents are trusted specialists and when outcome verification is feasible even if process inspection is not.<br><br><em>Example:</em> Tomašev et al. (2026) discuss opaque goal-based delegation as a core case in multi-agent systems. Hierarchical reinforcement learning architectures use the same pattern: a high-level policy sets subgoals and a low-level policy selects actions to achieve them, with no shared internal state.<br><br><em>Failure mode:</em> Because the principal cannot inspect execution, misalignment is undetectable until it surfaces in outcomes. A sub-agent that optimizes a proxy objective rather than the intended goal may report success while violating the principal's actual intent. The opacity compounds with depth: each hidden hop adds a layer where intent can drift unobserved.<br><br><em>Relation to other patterns:</em> Tree differs in that the orchestrator decomposes and tracks each sub-step explicitly. Mission Command expects agents to report back on their chosen method. Feudal Delegation is the most opaque of the three; it assumes sub-agents are competent and aligned without verifying either.</div>
</details>

**Quality and verification patterns** assess, critique, and certify agent output. Adding more agents does not automatically improve a result; what matters is how their work is checked and combined. The patterns below encode specific process choices for ensuring quality.

<details>
<summary>Evaluator <span class="gov-tagline">generate, critique, refine</span></summary>
<div class="gov-body">A generator produces output; a separate evaluator assesses quality and provides constructive feedback; the generator revises. The loop repeats until the evaluator is satisfied or a budget is exhausted. This is constructive iteration, not adversarial attack.<br><br><em>When to use:</em> quality-sensitive outputs where iterative improvement is worth the cost. Code generation, writing, reasoning tasks, any output that benefits from review and revision.<br><br><em>Example:</em> Anthropic's evaluator-optimizer pattern: the evaluator provides assessment and suggestions, the optimizer revises accordingly. The evaluator sees both the original task and the current output, enabling targeted feedback.<br><br><em>Failure mode:</em> infinite refinement loops where the evaluator keeps finding minor issues. Over-filtering where the evaluator rejects creative or novel approaches that do not match its quality model. Evaluator capture where the generator learns to satisfy the evaluator's preferences rather than the actual task.<br><br><em>Relation to other patterns:</em> Evaluator differs from Critic in being *constructive* rather than *adversarial*. The Evaluator says "this could be better because..."; the Critic says "this fails because..." Both improve output, but through different mechanisms.</div>
</details>

<details>
<summary>Voting <span class="gov-tagline">same task, majority wins</span></summary>
<div class="gov-body">Multiple agents work on the same task independently. The final answer is selected by majority vote or aggregation. Redundancy as a strategy for reliability.<br><br><em>When to use:</em> high-stakes decisions where no single agent is reliable enough. Medical diagnosis confirmation, safety-critical classifications, any domain where the cost of a wrong answer exceeds the cost of running multiple agents.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2402.05120" target="_blank" class="red-link">"More Agents Is All You Need"</a> demonstrates that even simple sampling and majority voting can improve LLM performance, with gains growing as ensemble size increases (most steeply on harder tasks).<br><br><em>Failure mode:</em> correlated errors. If all voters share the same training data, architecture, and biases, they will be wrong in the same way. Voting only helps when errors are independent.<br><br><em>Relation to other patterns:</em> Voting is the simplest form of ensemble. It differs from The Agora (governance) in being a delegation pattern for a specific decision rather than an ongoing deliberative structure for setting policy.</div>
</details>

<details>
<summary>Critic / Red Team <span class="gov-tagline">generate, then attack</span></summary>
<div class="gov-body">A generator produces output; a separate critic adversarially attacks it. The generator must survive the attack. If the critic finds flaws, the generator revises. This is the adversarial verification pattern.<br><br><em>When to use:</em> security review, red-teaming, adversarial robustness testing, any output that must withstand scrutiny before release.<br><br><em>Example:</em> a code agent generates a security-sensitive function; a red-team agent attempts SQL injection, XSS, and other attacks against it. Only code that survives the attack proceeds.<br><br><em>Failure mode:</em> critic too strong (nothing passes, system is paralyzed) or too weak (everything passes, the critic is theater). Calibrating critic severity is the key design challenge.<br><br><em>Relation to other patterns:</em> Critic differs from Evaluator in being *adversarial* rather than *constructive*. The Evaluator suggests improvements; the Critic tries to break things. Both can be combined in sequence: generate, evaluate, revise, then red-team the final version.</div>
</details>

<details>
<summary>Debate <span class="gov-tagline">multi-round adversarial argument with judge</span></summary>
<div class="gov-body">Two or more agents argue opposing positions over multiple rounds, with a judge agent evaluating the arguments. Unlike Critic (single-round attack on one output), Debate is iterative: each side responds to the other's arguments, and the judge observes the exchange before ruling. The key insight from AI safety research (<a href="https://arxiv.org/abs/1805.00899" target="_blank" class="red-link">Irving et al., 2018</a>) is that even if neither debater is fully trustworthy, the competitive dynamic can surface truths that neither would volunteer alone.<br><br><em>When to use:</em> high-stakes reasoning where single-agent confidence is insufficient, alignment verification, policy decisions, any setting where adversarial pressure improves the quality of reasoning rather than just testing robustness.<br><br><em>Example:</em> two agents argue whether a proposed code change introduces a security vulnerability. Agent A argues it is safe (citing the input validation); Agent B argues it is unsafe (citing an edge case in unicode handling). A judge agent evaluates both arguments across three rounds and rules. The multi-round structure lets each side address the other's strongest point.<br><br><em>Failure mode:</em> judge capture (the judge is persuaded by style rather than substance), infinite regress (arguments never converge), performative argumentation (agents optimize for persuasion rather than truth), and asymmetric capability (a more capable debater wins regardless of position).<br><br><em>Relation to other patterns:</em> Debate extends Critic from single-round to multi-round and from asymmetric (attacker/defender) to symmetric (both sides argue). It differs from Voting (which aggregates independent answers) in requiring iterative engagement. Composes with Witness (judge as independent arbiter) and Constitutional Republic governance (judicial branch as debate-based dispute resolution).</div>
</details>

<details>
<summary>Witness / Notarization <span class="gov-tagline">independent third-party verification</span></summary>
<div class="gov-body">A third-party agent, independent of both producer and consumer, certifies that output meets a standard before it is accepted. The witness does not produce or consume; it only verifies. This creates trust without requiring the consumer to trust the producer directly.<br><br><em>When to use:</em> multi-party systems where producer and consumer belong to different organizations or have different trust levels. Cross-organizational agent interactions, high-stakes outputs, regulatory compliance.<br><br><em>Example:</em> a medical agent generates a treatment recommendation. Before it reaches the patient's agent, an independent clinical guidelines agent verifies that the recommendation is consistent with current evidence. The patient's agent trusts the witness, not the recommender directly.<br><br><em>Failure mode:</em> witness becomes a bottleneck or a single point of trust failure. If the witness is compromised, all certified outputs are suspect. Multiple independent witnesses mitigate this but add cost.<br><br><em>Relation to other patterns:</em> Witness is the trust primitive for multi-party delegation. It differs from Evaluator (which is part of the production loop) and Critic (which is adversarial). The Witness is structurally independent and its only role is attestation.</div>
</details>

<details>
<summary>Verification Game <span class="gov-tagline">game-theoretic proof without re-executing the work</span></summary>
<div class="gov-body">Two or more agents engage in a structured protocol to verify a result without the verifier repeating the full computation. Verification is achieved through challenge-response exchanges, Schelling-point convergence, or economic incentive structures that make honest reporting a dominant strategy. The verifier checks the claim, not the process that produced it.<br><br><em>When to use:</em> Computationally expensive tasks where re-execution is prohibitive, or distributed settings where no single agent can be fully trusted. Particularly relevant when the verification cost must be asymmetrically lower than the production cost.<br><br><em>Example:</em> Optimistic rollup protocols in blockchain systems use this structure: execution is assumed correct and only challenged when a verifier disputes the result, at which point a bisection game resolves the disagreement. Tomašev et al. (2026) discuss analogous trust-establishment protocols for multi-agent delegation.<br><br><em>Failure mode:</em> If the prover and verifier can collude, both benefit from approving incorrect results. The game-theoretic equilibrium breaks down when the cost of honest verification exceeds the reward, or when the agents share a principal whose interest is not aligned with accurate verification.<br><br><em>Relation to other patterns:</em> Evaluator re-runs or scores the output directly; Verification Game avoids re-execution. Voting aggregates independent opinions; Verification Game is a structured protocol between specific participants, not an aggregation of independent assessments.</div>
</details>

**Reliability patterns** handle failure, recovery, and safe deployment. These patterns ensure that delegation chains degrade gracefully rather than cascading.

<details>
<summary>Circuit Breaker <span class="gov-tagline">stop calling failing agents</span></summary>
<div class="gov-body">An agent monitors the failure rate of calls to a downstream agent. When failures exceed a threshold, it "opens the circuit" and stops calling that agent entirely, returning errors immediately. After a cool-down period, it enters a "half-open" state and tests with a single call. If it succeeds, the circuit closes; if it fails, it opens again.<br><br><em>When to use:</em> any agent network where one failing agent could cascade into system-wide failure. API integrations, tool calls to external services, sub-agent delegation where the sub-agent may be overloaded or down.<br><br><em>Example:</em> a travel-booking agent calls an airline API. The API starts timing out. After three consecutive failures, the circuit breaker opens and the agent immediately falls back to cached results or an alternative provider, rather than waiting for more timeouts. In physical systems: an autonomous vehicle's perception agent monitors a failing lidar sensor; after repeated bad reads, the circuit opens and the agent falls back to camera-based depth estimation and cached map data.<br><br><em>Failure mode:</em> circuit opens too aggressively (transient errors trigger full cutoff) or too slowly (failing calls accumulate before protection kicks in). Tuning thresholds is critical.<br><br><em>Relation to other patterns:</em> Circuit Breaker is the defensive complement to Timeout. Timeout detects absence of signal; Circuit Breaker detects accumulated failure. Both are essential for resilient agent networks. Source: microservices resilience (Netflix Hystrix, resilience4j).</div>
</details>

<details>
<summary>Timeout / Dead-Man Switch <span class="gov-tagline">absence triggers response</span></summary>
<div class="gov-body">The system expects a signal (heartbeat, result, acknowledgment) within a time window. If the signal does not arrive, the timeout triggers a response: escalation, fallback, alert, or termination. The *non-event* is the event.<br><br><em>When to use:</em> every real deployment needs liveness monitoring. Long-running agent tasks, external API calls, sub-agent delegation where the sub-agent may hang or crash silently.<br><br><em>Example:</em> a research agent dispatches a sub-agent to crawl a website. If no result arrives within 30 seconds, the timeout triggers: the sub-agent is terminated and the task is re-routed to an alternative approach. The pattern predates software agents: watchdog timers in embedded firmware, heartbeat monitors in pacemakers, and safety cutoffs in industrial robot arms all use the same logic. Absence of a signal is the signal.<br><br><em>Failure mode:</em> timeout too short (kills slow but productive work) or too long (wastes time waiting for dead agents). Dynamic timeout adjustment based on task type is the mature approach.<br><br><em>Relation to other patterns:</em> Timeout is the safety primitive that makes autonomous delegation possible. Without it, a single hung agent can block an entire workflow. It composes with Checkpoint/Saga (timeout triggers rollback) and Escalation (timeout triggers escalation to a more capable agent).</div>
</details>

<details>
<summary>Checkpoint / Saga <span class="gov-tagline">rollback on failure</span></summary>
<div class="gov-body">A multi-step workflow with explicit checkpoints. If a later step fails, the system rolls back to the last checkpoint and retries or compensates. Borrowed from distributed transaction design.<br><br><em>When to use:</em> long-running workflows where partial failure must not corrupt the overall result. Multi-file code refactors, multi-step data migrations, workflows involving external side effects that may need reversal.<br><br><em>Example:</em> an agent refactors a database schema: step 1 creates the new table, step 2 migrates data, step 3 updates the API, step 4 drops the old table. If step 3 fails, the saga compensates by reverting steps 2 and 1.<br><br><em>Failure mode:</em> rollback storms when many steps need compensation simultaneously. Checkpoint overhead on fast, short tasks where the cost of checkpointing exceeds the cost of restarting.<br><br><em>Relation to other patterns:</em> Checkpoint/Saga adds reliability to any sequential pattern (Chain, Pipeline). It is the delegation-level primitive that enables Timeout at the system level.</div>
</details>

<details>
<summary>Canary / Shadow <span class="gov-tagline">test new agents on live traffic safely</span></summary>
<div class="gov-body">A new (canary) agent runs alongside the incumbent on real inputs, but its outputs are not served to users. A judge compares canary outputs against the incumbent's. Only after the canary demonstrates statistically equivalent or better performance over many requests is it promoted to replace the incumbent.<br><br><em>When to use:</em> safe rollout of new agent versions, model upgrades, prompt changes, or entirely new specialist agents. Any setting where you cannot fully validate quality offline and need production traffic to build confidence.<br><br><em>Example:</em> a customer-support team replaces GPT-4 with a fine-tuned smaller model. For two weeks, both models answer every ticket. A quality judge scores both. The fine-tuned model is promoted only after its scores meet a statistical threshold across 10,000 tickets.<br><br><em>Failure mode:</em> shadow period too short (promotes a model that performs well on easy cases but fails on rare hard ones). Shadow period too long (wastes compute running two agents indefinitely). Also, behavioral drift during shadowing if the canary's context differs from what it will see when promoted.<br><br><em>Relation to other patterns:</em> Canary differs from Speculative Execution (which races for speed on a single task) in being a long-running evaluation over many tasks. It shares structure with speculative decoding (cheap-produces, expensive-verifies) but optimizes for safety rather than latency. It composes naturally with Witness (the judge is a form of witness) and Circuit Breaker (automatic rollback if the canary's error rate spikes).</div>
</details>

**Market and competition patterns** allocate work through bidding, negotiation, and racing. These leverage competitive dynamics rather than hierarchical assignment.

<details>
<summary>Auction <span class="gov-tagline">bid for work</span></summary>
<div class="gov-body">A task is announced; agents bid based on their capabilities, cost, and availability. The best bid wins the contract. Market-based task allocation that lets the system discover the most efficient assignment without central planning.<br><br><em>When to use:</em> task allocation across heterogeneous specialists where capabilities and costs vary. Open marketplaces, dynamic team assembly, any setting where you want competition to drive quality and efficiency.<br><br><em>Example:</em> the <a href="https://en.wikipedia.org/wiki/Contract_Net_Protocol" target="_blank" class="red-link">Contract Net Protocol</a> (Smith, 1980) formalized this: managers announce tasks, contractors bid, winners execute and report. The winning contractor can sub-contract, making the pattern naturally recursive.<br><br><em>Failure mode:</em> bid gaming (agents misrepresent capabilities), race to the bottom on quality (cheapest bid wins regardless of quality), and winner's curse (the winning bidder systematically underbids).<br><br><em>Relation to other patterns:</em> Auction is delegation by competition. It is the task-level mechanism behind Market governance. Combined with Pipeline, it creates the Bidding Pipeline composition.</div>
</details>

<details>
<summary>Negotiation <span class="gov-tagline">bilateral offer/counter-offer</span></summary>
<div class="gov-body">Two agents, each representing a principal, exchange offers and counter-offers until they reach agreement or declare impasse. Unlike Auction (many bidders, one winner), Negotiation is bilateral: two parties with opposing interests seeking a zone of possible agreement (ZOPA). Each agent has a mandate (what it can offer), hard limits (its walkaway point), and strategy (how aggressively to push).<br><br><em>When to use:</em> any setting where two principals need to reach a deal through their agents. Contract terms, pricing, service levels, resource allocation between organizations, consumer-to-business disputes.<br><br><em>Example:</em> your personal agent negotiates a lower internet bill with the telecom's retention agent. Your agent knows your usage patterns and competing offers; the telecom's agent knows its retention budget and your lifetime value. They exchange offers until one accepts or escalates to a human.<br><br><em>Failure mode:</em> deadlock (neither agent concedes), no ZOPA (the principals' constraints are incompatible but the agents waste rounds discovering this), principal misrepresentation (an agent bluffs beyond its mandate), and collusion (agents agree on terms that serve themselves rather than their principals).<br><br><em>Relation to other patterns:</em> Negotiation is the bilateral case of Auction. It composes with Relay (handoff after agreement), Witness (third party certifies the deal), and Checkpoint (agreement is binding, violation triggers rollback). Under Custodianship governance, each negotiating agent has fiduciary obligation to its principal.</div>
</details>

<details>
<summary>Speculative Execution <span class="gov-tagline">race approaches, first valid wins</span></summary>
<div class="gov-body">Multiple agents attempt the same task simultaneously using different approaches. The first valid result is committed; the rest are discarded. Trading compute for latency.<br><br><em>When to use:</em> latency-sensitive tasks where multiple valid approaches exist and you cannot predict which will be fastest. Code generation (try different algorithms), creative tasks (try different styles), search (try different strategies).<br><br><em>Example:</em> a user asks for an optimized sort function. Three agents try different approaches (quicksort, mergesort, radix sort). The first to pass the test suite wins. The others are cancelled.<br><br><em>Failure mode:</em> wasted compute on discarded branches. Also: committing a fast but incorrect result if the validity check is too weak.<br><br><em>Relation to other patterns:</em> Speculative Execution is the opposite of Escalation (which tries one at a time). It trades cost for speed. It differs from Voting in that agents try *different* approaches rather than the *same* approach.</div>
</details>

<details>
<summary>Contract Net <span class="gov-tagline">broadcast, bid, award, execute, report</span></summary>
<div class="gov-body">A full lifecycle protocol where a manager agent broadcasts a call-for-proposals describing a task, bidding agents submit bids based on capability and availability, the manager awards the contract to the best bidder, the winner executes and reports completion, and the winner may recursively sub-contract portions of the task to other agents. This is the complete coordination cycle, not just its bidding phase.<br><br><em>When to use:</em> Open multi-agent systems where task requirements and agent capabilities are heterogeneous and not known in advance. Useful when no central registry of agent skills exists and when workload must be dynamically distributed across available agents.<br><br><em>Example:</em> <a href="https://en.wikipedia.org/wiki/Contract_Net_Protocol" target="_blank" class="red-link">Reid G. Smith</a> formalized this protocol in 1980 as the Contract Net Protocol; it was later standardized by FIPA as a foundation for multi-agent coordination. Modern LLM-based orchestration frameworks implicitly replicate parts of this lifecycle when routing tasks across specialized agents.<br><br><em>Failure mode:</em> Recursive sub-contracting creates unbounded delegation depth. A winning agent that cannot complete the task sub-contracts it, and that agent sub-contracts further, generating chains that are difficult to monitor, attribute, or terminate cleanly.<br><br><em>Relation to other patterns:</em> Auction covers only the bidding and award phases; Contract Net extends the protocol through execution and reporting. Feudal Delegation omits bidding entirely; authority flows by assignment, not by competitive proposal.</div>
</details>

**Knowledge transfer patterns** move capability, data, or insight between agents. These patterns determine how knowledge propagates across a society without requiring every agent to learn from scratch.

<details>
<summary>Teacher-Student <span class="gov-tagline">strong trains weak</span></summary>
<div class="gov-body">A capable model transfers knowledge to a less capable one through examples, corrections, or traces. The student improves over time; eventually it may handle tasks that previously required the teacher.<br><br><em>When to use:</em> model distillation, capability transfer, generating fine-tuning data, bootstrapping specialists from generalists.<br><br><em>Example:</em> a frontier model generates high-quality code reviews; these become training data for a smaller, faster model that handles routine reviews at a fraction of the cost. The teacher creates the student's curriculum.<br><br><em>Failure mode:</em> distribution mismatch between teacher examples and real tasks. The student may learn the teacher's biases and blind spots alongside its strengths.<br><br><em>Relation to other patterns:</em> Teacher-Student is the delegation pattern behind capability transfer. It differs from Federated Learning (where knowledge flows from many peers to a coordinator) in being hierarchical and directional.</div>
</details>

<details>
<summary>Federated Learning <span class="gov-tagline">collaborative training, no data sharing</span></summary>
<div class="gov-body">Agents train on their local private data and send only model weight updates (not raw data) to a coordinator. The coordinator aggregates updates into a global model and redistributes it. No agent ever sees another agent's training data.<br><br><em>When to use:</em> privacy-preserving collaborative improvement. Multi-organization agent systems where data cannot leave organizational boundaries. Healthcare networks, financial institutions, any setting where agents improve collectively but data sovereignty matters.<br><br><em>Example:</em> hospital agents across a network each learn from local patient interactions. They share model updates (not patient data) with a coordinator that produces a better global model, which is redistributed. Each hospital's patients benefit from the collective learning without any privacy violation. The original motivating case was simpler: many mobile devices improving a shared model (for text entry and speech) by sending model updates rather than raw user data (<a href="https://arxiv.org/abs/1602.05629" target="_blank" class="red-link">McMahan et al. 2016</a>). The same principle extends to any fleet of edge devices: wearable health monitors learning from collective patterns without sharing personal biometrics, or agricultural drones improving crop-disease detection across farms without transmitting proprietary field data.<br><br><em>Failure mode:</em> poisoned updates from a malicious participant can corrupt the global model. Non-IID data distributions across participants can cause the global model to perform poorly for some participants.<br><br><em>Relation to other patterns:</em> Federated Learning is the privacy-preserving counterpart to Teacher-Student (which shares knowledge directly). It inverts the flow: agents do the training, the coordinator only aggregates. Source: distributed ML (<a href="https://arxiv.org/abs/1602.05629" target="_blank" class="red-link">McMahan et al. 2016</a>).</div>
</details>

<details>
<summary>Blackboard <span class="gov-tagline">shared workspace, indirect coordination</span></summary>
<div class="gov-body">Agents contribute partial results to a shared workspace. Any agent can read the current state and add to it. Coordination happens through the artifact, not through direct messages between agents. The decomposition is not fixed in advance; agents contribute when they have something useful.<br><br><em>When to use:</em> collaborative problem-solving where agents have heterogeneous capabilities and the task structure is not known in advance. Research collaboration, incident response, complex debugging.<br><br><em>Example:</em> a security incident: one agent adds network logs, another adds suspicious process activity, a third adds threat intelligence matches, a fourth synthesizes a timeline from everything on the blackboard. No agent directed the others; the shared workspace coordinated them. Another example: agents from different users collaborate on the same code repository. One agent writes a PR, another reviews it, a third runs the CI pipeline, a fourth checks style conformance. The repository is the blackboard; each agent reads current state and contributes without direct coordination.<br><br><em>Failure mode:</em> the blackboard becomes noisy: too many partial results with no curation. Without some mechanism for relevance filtering, later agents drown in irrelevant contributions.<br><br><em>Relation to other patterns:</em> Blackboard enables heterogeneous agents to collaborate without shared protocols; the shared artifact IS the protocol. It differs from Stigmergy in the structure of contributions: Blackboard contributions are typically explicit and structured (named entries), while stigmergic traces are implicit side effects of other work. Both patterns can be deliberately designed.</div>
</details>

<details>
<summary>Distillation <span class="gov-tagline">compress capability from large to small, then cut the cord</span></summary>
<div class="gov-body">A large or expensive model generates outputs, reasoning traces, or labeled examples that are used to fine-tune a smaller model. Once training is complete, the student operates independently; there is no ongoing connection to the teacher. The knowledge transfer happens at training time, not at inference time.<br><br><em>When to use:</em> Deployments where inference cost, latency, or privacy constraints make the teacher model unsuitable for production, but where a smaller model can approximate the teacher's behavior on the relevant task distribution. Common in production systems that prototype with frontier models and deploy fine-tuned smaller models.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/1503.02531" target="_blank" class="red-link">Hinton et al. 2015</a> introduced the formal framework for knowledge distillation using soft label transfer. More recently, frontier model outputs have been used to fine-tune smaller open-weight models for specific instruction-following tasks, with the teacher never invoked at inference time.<br><br><em>Failure mode:</em> Capability collapse occurs when the student's deployment distribution diverges from the distribution of teacher demonstrations used in training. The student performs well on in-distribution inputs and fails silently on the long tail.<br><br><em>Relation to other patterns:</em> Teacher-Student involves real-time instructional interaction at inference time; the teacher remains active. Distillation severs that connection after training. Loop/Retry-with-Context iterates at inference time; Distillation iterates at training time.</div>
</details>

**Emergent coordination patterns** operate without central control. These are the patterns most relevant to the Loom Hypothesis. Under resource pressure, agents develop coordination through local interaction rather than top-down design. Holland (*Hidden Order*, 1995) and Kauffman (*The Origins of Order*, 1993) showed that systems of interacting agents under constraints reliably produce self-organized structure. The patterns below are that phenomenon instantiated in AI agent networks.

<details>
<summary>Publish-Subscribe <span class="gov-tagline">event-driven, fully decoupled</span></summary>
<div class="gov-body">Agents publish typed events to named topics without knowing who will receive them. Subscriber agents declare interest in topics and receive matching events asynchronously. Publishers and subscribers are fully decoupled: neither knows the other exists.<br><br><em>When to use:</em> large-scale agent coordination where point-to-point wiring would be impractical. Event-driven architectures, notification systems, any setting where many agents need to react to the same events.<br><br><em>Example:</em> in a trading system, a market-data agent publishes price updates to a "prices" topic. Hundreds of strategy agents subscribe and react independently. Adding a new strategy agent requires zero changes to the publisher. In a smart building, occupancy sensors publish to a "room-state" topic; HVAC agents, lighting agents, and cleaning-scheduling agents each subscribe and adapt independently.<br><br><em>Failure mode:</em> message storms when popular topics generate more events than subscribers can process. Lost messages if the broker fails. Debugging is hard because the event flow is implicit.<br><br><em>Relation to other patterns:</em> Pub-Sub differs from Gossip (which actively pushes to random peers without a broker) in using structured topic-based routing through a central broker. It is the designed version of the communication pattern that Gossip achieves epidemiologically. Source: event-driven architecture (Kafka, RabbitMQ).</div>
</details>

<details>
<summary>Choreography <span class="gov-tagline">decentralized event-driven coordination</span></summary>
<div class="gov-body">Agents coordinate by reacting to events published by their peers, each following local event-driven rules. No central controller holds the full workflow. The resulting coordination pattern is distributed across the participants rather than encoded in any single agent's logic. Choreography can be deliberately designed (a developer specifies which events each agent publishes and subscribes to) or can arise when agents independently learn to react to each other's outputs.<br><br><em>When to use:</em> systems where centralized orchestration would be a bottleneck or single point of failure. Large-scale agent ecosystems, cross-organizational coordination, any setting where no single agent should control the workflow.<br><br><em>Example:</em> in a microservices system, when an order is placed, the order service emits an "OrderPlaced" event. The inventory service reacts by reserving stock, the payment service charges the card, the shipping service schedules delivery. Each service's event contracts were designed, but no orchestrator directs the flow at runtime.<br><br><em>Failure mode:</em> the workflow is invisible. When something goes wrong, no single agent has the full picture. Debugging distributed choreographies requires distributed tracing. Deadlocks are possible when agents wait for events that depend on each other circularly.<br><br><em>Relation to other patterns:</em> Choreography is the decentralized alternative to Orchestrator. Where Orchestrator centralizes decomposition, Choreography distributes it across event contracts. Both can be designed; neither is inherently more or less deliberate. Source: SOA/microservices architecture, event-driven systems.</div>
</details>

<details>
<summary>Stigmergy <span class="gov-tagline">environment as communication</span></summary>
<div class="gov-body">Agents coordinate without direct communication by reading and modifying a shared environment. Environmental traces (artifacts, logs, cached results, pheromone-like signals) trigger subsequent behavior in other agents. A deployer can deliberately set up stigmergic coordination (design the environment, define what traces agents leave) or it can arise naturally when agents happen to share an environment and begin reacting to each other's modifications.<br><br><em>When to use:</em> large heterogeneous agent populations with no shared protocol. Collaborative knowledge building, open-source development patterns, systems where agents come and go unpredictably.<br><br><em>Example:</em> Wikipedia is stigmergic: editors modify articles (the shared environment), and other editors react to those modifications by further editing, citing, or reverting. The artifact mediates all coordination. In robotics, warehouse robots leave digital markers on a shared floor map: "aisle 7 congested," "shelf B3 restocked." Other robots read these traces and reroute without direct robot-to-robot communication. Both examples were deliberately designed to work this way.<br><br><em>Failure mode:</em> environmental noise. Without curation, traces accumulate into an unusable mess. Also vulnerable to environmental poisoning: a malicious agent can leave misleading traces that redirect subsequent agents.<br><br><em>Relation to other patterns:</em> Stigmergy differs from Blackboard in the structure of the shared space. Blackboard contributions are typically structured and explicit (an agent writes a named entry); stigmergic traces are typically implicit and incidental (an agent modifies the environment as a side effect of its work, and others react). Both can be designed. Source: swarm intelligence (Grassé 1959), collaborative editing, ant colony optimization.</div>
</details>

<details>
<summary>Swarm <span class="gov-tagline">collective behavior, no central plan</span></summary>
<div class="gov-body">Multiple agents produce collective behavior without centralized control. The coordination mechanism varies: it can be local interaction rules, shared objectives, imitation, environmental signals, or any combination. What defines a swarm is the absence of a central plan, not the specific mechanism that replaces it. A deployer can deliberately engineer a swarm (specify agent behaviors, deploy them, let collective patterns form) or a swarm can form spontaneously when agents discover that decentralized coordination outperforms waiting for instructions.<br><br><em>When to use:</em> exploration, creative search, resilient systems where agent failure should not disrupt the collective. Distributed search, open-ended research, any setting where you want robust collective behavior without a single point of failure.<br><br><em>Example:</em> a fleet of search-and-rescue drones after an earthquake. Each drone scans its area, shares findings with neighbors, and the swarm converges on likely survivor locations. The deployer designed the individual agent behaviors; the collective search pattern was not centrally planned. Another: a research swarm where each agent explores a different approach, shares findings, and adjusts strategy. Promising directions attract more agents naturally.<br><br><em>Failure mode:</em> if the swarm's collective behavior drifts from the intended goal, there is no built-in central mechanism to correct course. Swarms can converge on locally optimal but globally poor solutions without any agent recognizing the problem.<br><br><em>Relation to other patterns:</em> Swarm differs from Choreography (where agents react to specific typed events with defined contracts) in having less structured interaction. It differs from Gossip (which is specifically about information propagation) in being broader: swarm agents coordinate behavior, not just spread data. Swarm becomes Colony governance when the collective patterns persist and begin constraining future behavior.</div>
</details>

<details>
<summary>Gossip <span class="gov-tagline">peer-to-peer information spread</span></summary>
<div class="gov-body">Agents spread information by telling their neighbors, who tell their neighbors, in an epidemic pattern. No broadcast, no central hub. In well-connected networks, information propagates through repeated local contact, eventually reaching the whole population.<br><br><em>When to use:</em> decentralized coordination where broadcast would be expensive or infeasible. Norm propagation, state synchronization across large agent populations, failure detection in distributed systems.<br><br><em>Example:</em> in a large agent network, when one agent discovers a useful tool or strategy, it shares with its direct contacts. They share with theirs. Within a few rounds, the entire network has the information, without any single agent needing to broadcast. In sensor mesh networks, this is how fault detection spreads: one node detects anomalous vibration in a bridge pylon, tells its neighbors, and the alert propagates through the mesh without any central monitoring server.<br><br><em>Failure mode:</em> rumor drift (information mutates as it passes through agents, like a game of telephone) and norm poisoning (a malicious agent injects false information that spreads unchecked).<br><br><em>Relation to other patterns:</em> Gossip differs from Pub-Sub (which uses a central broker with topic-based routing) in being fully decentralized and epidemiological. It is a common communication substrate in Colony governance.</div>
</details>

**Trust and authority patterns** govern who is allowed to delegate to whom, with what permissions, and how responsibility transfers across hops. These patterns are structural constraints that apply on top of any other delegation pattern. Tomašev et al. (2026) treat this as a first-class coordination concern. Without explicit trust infrastructure, multi-agent systems develop implicit authority structures that are invisible to auditors and unaccountable when they fail.

<details>
<summary>Privilege Attenuation <span class="gov-tagline">you can only grant what you hold, never more</span></summary>
<div class="gov-body">A structural constraint on all delegation patterns: when an agent sub-delegates, it may grant only a strict subset of its own permissions to the sub-agent. Authority cannot be amplified at any link in the chain. An agent with read-only file access cannot grant write access. An agent authorized for one data scope cannot delegate access to a broader scope. This is a property of the delegation infrastructure, not a pattern agents choose to apply.<br><br><em>When to use:</em> Any multi-agent system where sub-agents interact with external resources, user data, or APIs. Privilege attenuation should be enforced at the runtime or framework level so that agents cannot circumvent it by accident or by design.<br><br><em>Example:</em> Tomašev et al. (2026) identify this as a core requirement for safe intelligent delegation. The principle of least privilege in operating systems enforces the same constraint: processes cannot escalate their own privileges by spawning child processes.<br><br><em>Failure mode:</em> Over-attenuation is the practical failure. Each delegation hop strips permissions conservatively, and by the time a leaf agent operates, it lacks the access needed to complete its assigned task. The system grinds to a halt not from over-permission but from under-permission at the leaves.<br><br><em>Relation to other patterns:</em> Privilege Attenuation applies as a constraint within Feudal Delegation, Contract Net, and any other pattern that creates delegation chains. Capability Credentials make the current permission set verifiable; Privilege Attenuation governs how that set changes across hops.</div>
</details>

<details>
<summary>Liability Firebreaks <span class="gov-tagline">explicit handoffs of responsibility, not implicit accumulation</span></summary>
<div class="gov-body">Explicit points in a delegation chain where responsibility is formally transferred and bounded. A firebreak specifies what the sub-agent is responsible for, what the principal retains liability for, and what happens at the boundary if the sub-agent fails. Without firebreaks, liability silently accumulates at the top of the chain because no intermediate agent formally accepted ownership of specific outcomes.<br><br><em>When to use:</em> Delegation chains involving consequential actions where failure attribution matters, such as financial transactions, medical recommendations, legal document generation, or any task where post-hoc accountability is required. Especially important in systems that span organizational or legal boundaries.<br><br><em>Example:</em> Tomašev et al. (2026) in "Intelligent AI Delegation" identify liability assignment as a first-class design concern for agentic systems. Analogous structures exist in supply chain contracting, where tier-1 suppliers formally accept liability for their sub-suppliers' outputs at defined inspection points.<br><br><em>Failure mode:</em> Firebreaks placed too narrowly require explicit contracting for every micro-task, creating overhead that defeats the purpose of delegation. Placed too broadly, liability is so diffuse that no agent owns any outcome, and failures produce attribution disputes rather than corrections.<br><br><em>Relation to other patterns:</em> Contract Net's reporting phase is a natural site for firebreaks. Checkpoint/Saga uses state snapshots for recovery; Liability Firebreaks use formal handoffs for accountability. The two can coexist but serve different purposes.</div>
</details>

<details>
<summary>Capability Credentials <span class="gov-tagline">structured proof of what you can do, within what boundaries</span></summary>
<div class="gov-body">Agents carry verifiable attestations of their capabilities, issued by an authority or earned through demonstrated performance in specific domains. Unlike aggregate reputation scores, credentials are structured: they specify the task type, the conditions under which capability was demonstrated, the issuing authority, and the boundaries within which the credential is valid. A credential for "medical literature summarization" is not a credential for "clinical diagnosis."<br><br><em>When to use:</em> Open systems where agents from different sources are composed dynamically and where a delegating agent cannot directly inspect the sub-agent's internals. Credentials allow trust to be calibrated without requiring re-evaluation from scratch on every interaction.<br><br><em>Example:</em> Tomašev et al. (2026) discuss capability attestation as infrastructure for safe multi-agent delegation. The W3C Verifiable Credentials standard provides a technical foundation for issuing and checking structured attestations in decentralized systems.<br><br><em>Failure mode:</em> Credential inflation occurs when issuance standards erode and "expert" credentials become common. Credential staleness occurs when an agent's actual capabilities shift after issuance. Gaming occurs when agents optimize for acquiring credentials rather than for the underlying capabilities the credentials represent.<br><br><em>Relation to other patterns:</em> Privilege Attenuation constrains what permissions a credentialed agent can pass further down the chain. Contract Net bidding can use credentials as part of the bid, allowing the manager to select based on verified capability rather than self-reported competence.</div>
</details>

<details>
<summary>Zone of Indifference <span class="gov-tagline">the range of instructions an agent follows without negotiation</span></summary>
<div class="gov-body">The set of instructions an agent executes without scrutiny, pushback, or negotiation. Within the zone, delegation flows smoothly because the agent treats the instruction as within the normal scope of its role. Outside the zone, the agent questions, negotiates, or refuses. The zone's boundaries are determined by the agent's understanding of its role, its values, and its assessment of the instruction's legitimacy. Chester Barnard introduced this concept in organizational theory in 1938 to explain why employees comply with managerial directives without evaluating each one individually.<br><br><em>When to use:</em> As an analytical lens for understanding why some delegation chains proceed without friction and others produce deadlock or refusal. The concept is useful for diagnosing where in a chain execution stalls and for calibrating agent compliance thresholds.<br><br><em>Example:</em> An agent configured for customer support will execute routine reply tasks without scrutiny but may refuse or escalate a request to send a message containing a refund amount above a set threshold. The threshold defines the zone boundary.<br><br><em>Failure mode:</em> Zones too wide cause agents to execute harmful instructions without questioning, creating safety failures. Zones too narrow cause agents to challenge routine instructions, creating throughput failures. Calibration is the hard problem.<br><br><em>Relation to other patterns:</em> Human-in-the-Loop Gate and Approval Escalation can be understood as mechanisms for handling instructions that fall outside the zone. Policy Delegation defines the zone explicitly through written constraints.</div>
</details>

**Human-agent interface patterns** define how humans structurally participate in delegation chains. These are mechanical patterns that determine where execution blocks, who monitors, and what triggers human involvement. The central tension is that human oversight cannot scale linearly with agent count. These patterns represent different structural solutions to that constraint.

<details>
<summary>Human-in-the-Loop Gate <span class="gov-tagline">execution blocks until a human approves</span></summary>
<div class="gov-body">Execution pauses at a predefined checkpoint and waits for explicit human approval before proceeding. The blocking actor is a human, not another agent. The gate is synchronous: the system cannot continue until approval is received. The location of the gate, the information presented to the human, and the approval granularity are design choices that determine how much the gate actually protects versus how much it taxes the human's attention.<br><br><em>When to use:</em> High-stakes, low-frequency decisions where human judgment is irreplaceable and where the cost of a wrong decision outweighs the throughput penalty of waiting. Suitable for actions that are difficult or impossible to reverse, such as sending external communications, committing financial transactions, or deploying changes to production.<br><br><em>Example:</em> CrewAI's human input step and LangGraph's interrupt nodes both implement this pattern, pausing graph execution at a node and resuming only after a human provides input. The OpenAI Assistants SDK supports similar approval checkpoints in tool-use flows.<br><br><em>Failure mode:</em> Gate fatigue. When gates fire frequently or present too much information to evaluate quickly, humans approve everything without reading. The gate exists structurally but provides no actual oversight. This is the automation complacency failure in its active rather than passive form.<br><br><em>Relation to other patterns:</em> Human-on-the-Loop is the non-blocking counterpart: the human can intervene but execution does not wait. Approval Escalation routes only anomalies to humans; the gate fires at predefined structural points regardless of content.</div>
</details>

<details>
<summary>Human-on-the-Loop <span class="gov-tagline">you can intervene, but the system will not wait for you</span></summary>
<div class="gov-body">The system runs autonomously while a human monitors its behavior and retains the ability to intervene at any time. Unlike the Human-in-the-Loop Gate, execution is non-blocking: the system does not pause for approval. The human sees what is happening and can interrupt, override, or redirect, but the system continues unless they act. The burden of vigilance is entirely on the human.<br><br><em>When to use:</em> Tasks where continuous human approval would eliminate the efficiency benefit of automation, but where the consequences of failure are significant enough to warrant ongoing human awareness. Common in long-running agentic workflows where most steps are routine but occasional steps require human judgment.<br><br><em>Example:</em> Supervisory control systems in aviation and industrial process control use this pattern: operators monitor dashboards and can intervene, but the system executes continuously. Analogous patterns appear in agentic coding assistants that run autonomously but surface diffs for human review before committing.<br><br><em>Failure mode:</em> Automation complacency, described by Bainbridge's "ironies of automation" (1983): because the system usually operates correctly, the human's monitoring attention degrades over time. When the rare failure occurs, the human is no longer cognitively prepared to intervene effectively.<br><br><em>Relation to other patterns:</em> Human-in-the-Loop Gate is the blocking counterpart. Policy Delegation removes human monitoring from the loop entirely; Human-on-the-Loop keeps the human present but passive. Approval Escalation actively pushes anomalies to the human; Human-on-the-Loop requires the human to notice them.</div>
</details>

<details>
<summary>Policy Delegation <span class="gov-tagline">govern by constraint, not by per-decision approval</span></summary>
<div class="gov-body">A human defines a set of constraints, boundaries, and objectives; the agent then operates autonomously within those bounds indefinitely, without requesting per-decision approval. The human's influence is encoded in the policy at authoring time rather than exercised through real-time oversight. Governance happens before execution, not during it.<br><br><em>When to use:</em> High-volume, repetitive tasks where human review of individual decisions is impractical, and where the task space can be meaningfully bounded in advance. Appropriate when the principal trusts that the policy captures their intent well enough that autonomous operation within it produces acceptable outcomes.<br><br><em>Example:</em> Anthropic's Constitutional AI trains models to follow a set of written principles rather than requiring human feedback on each output. Reward specification in reinforcement learning encodes objectives as a function rather than as per-step human guidance. Rules-based trading systems operate the same way: policy set at configuration time, autonomous execution thereafter.<br><br><em>Failure mode:</em> Specification gaming: the agent satisfies the letter of the policy while violating its spirit, finding edge cases the policy author did not anticipate. Policy staleness: conditions change and the policy no longer reflects the principal's current intent, but the agent keeps executing against the old specification.<br><br><em>Relation to other patterns:</em> Zone of Indifference describes the behavioral effect of policy delegation from the agent's perspective. Human-in-the-Loop Gate and Human-on-the-Loop both require the human to remain present; Policy Delegation explicitly removes that requirement.</div>
</details>

<details>
<summary>Approval Escalation <span class="gov-tagline">routine work proceeds; anomalies reach a human</span></summary>
<div class="gov-body">The system routes only anomalies, high-stakes decisions, or low-confidence situations to a human for approval. Routine work proceeds without human involvement. The escalation threshold, and the system's judgment about what crosses it, are design choices rather than hard structural guarantees. The human sees a filtered view of system activity, not the full stream.<br><br><em>When to use:</em> Systems with high throughput where most decisions are routine and human review of all decisions is infeasible, but where a subset of decisions carry enough risk or novelty to warrant human judgment. The pattern is only effective when the system's anomaly-detection capability is well-calibrated.<br><br><em>Example:</em> Tomašev et al. (2026) discuss selectively inserting human checkpoints based on uncertainty or stakes, rather than at fixed points. Content moderation pipelines use the same structure: automated classifiers handle clear cases; borderline cases route to human reviewers.<br><br><em>Failure mode:</em> Miscalibrated threshold. Too low, and the human faces gate fatigue identical to an always-on gate. Too high, and critical failures are never escalated, giving the human a false impression of smooth operation. The system's judgment about what is anomalous is itself a point of failure that the human cannot easily audit.<br><br><em>Relation to other patterns:</em> Human-in-the-Loop Gate fires at predefined structural points; Approval Escalation fires based on content and context. Human-on-the-Loop leaves detection to the human; Approval Escalation automates detection and pushes findings proactively.</div>
</details>

<div style="margin: 1.5em 0; padding: 1em 1.2em; background: #fef3c7; border: 1px solid #f59e0b; border-radius: 6px;">
<strong>Meta-pattern: Adaptive Delegation</strong>

The delegation structure itself changes in real-time based on performance signals. This is not a pattern you deploy; it is what happens to any deployed pattern over time when the system observes its own performance and acts on those observations.
</div>

<details>
<summary>Adaptive Delegation <span class="gov-tagline">the delegation structure itself is the thing that adapts</span></summary>
<div class="gov-body">A meta-pattern in which the delegation structure changes in real-time based on performance signals, without requiring human reconfiguration. Routing shifts as agents demonstrate competence. Agents are promoted to harder tasks or demoted when performance drops. New specialist slots are created when the system detects unserved query types. The system transitions between delegation patterns, from Escalation to Router, from Supervisor to Market, without human intervention. The adaptation mechanism operates on the architecture, not on individual task plans.<br><br><em>When to use:</em> Long-running systems where the task distribution is non-stationary and where no fixed delegation structure will remain optimal over time. Requires sufficient volume of performance signal to distinguish genuine improvement from noise, and a stable enough objective to define what "better" means across structure changes.<br><br><em>Example:</em> DSPy optimizers modify prompt and routing configurations based on downstream metrics, instantiating this pattern at the prompt level. <a href="https://www.adaptionlabs.ai/" target="_blank" class="red-link">Adaption Labs</a> explores similar ideas at the agent orchestration level. Part 1 of this series describes the <a href="/posts/2026/agent-fabric-part1/#the-adaptive-fabric">Adaptive Fabric</a> as the mechanism that makes a society self-organizing rather than statically configured.<br><br><em>Failure mode:</em> Oscillation: the system switches patterns without settling, thrashing between structures as noisy signals flip the adaptation criterion. Premature optimization: a structure is locked in before enough data exists to justify it. The architecture-to-institution trap: the adaptation mechanism itself becomes a governance structure, with its own authority, its own failure modes, and its own need for oversight, whether or not any of that was intended (see "From architecture to institution" above).<br><br><em>Relation to other patterns:</em> Orchestrator re-plans individual tasks dynamically within a fixed structure; Adaptive Delegation changes the structure itself across tasks over time. Every other pattern in this taxonomy can be a target or a source state in an Adaptive Delegation transition. This is the delegation-level manifestation of Part 1's five adaptation surfaces (data, model, environment, coordination, interface).</div>
</details>


</div>

<details style="margin: 1em 0; padding: 0.8em 1em; background: #eff6ff; border: 1px solid #bfdbfe; border-radius: 6px;">
<summary style="cursor: pointer; font-weight: 600;">Quick guide: which delegation pattern fits your task?</summary>

- **Linear workflow, clear stages:** Chain or Pipeline
- **Need to hand off completely:** Relay / Handoff
- **Multiple domains, need routing:** Router (+ Escalation for cost optimization)
- **Complex task, unknown decomposition:** Orchestrator or Tree
- **Reliability critical, single task:** Voting or Evaluator
- **Adversarial verification needed:** Critic / Red Team
- **Latency-critical, multiple approaches:** Speculative Execution
- **Cost-critical, variable difficulty:** Escalation (try cheap first)
- **Privacy-preserving collaboration:** Federated Learning
- **Heterogeneous agents, shared problem:** Blackboard
- **Large-scale event coordination:** Publish-Subscribe or Choreography
- **Resilience against cascading failure:** Circuit Breaker + Timeout
- **Trust across organizational boundaries:** Witness / Notarization
- **Capable agents, complex goals:** Mission Command (specify intent, not method)
- **Quality assurance at scale:** Supervisor / Hierarchical Review
- **Bilateral deal between principals:** Negotiation (offer/counter-offer with escalation)
- **High-stakes reasoning, alignment verification:** Debate (multi-round argument with judge)
- **Task allocation via competition:** Auction (agents bid for work)
- **Full lifecycle task contracting:** Contract Net (broadcast, bid, award, execute, report)
- **Verification without re-execution:** Verification Game (challenge-response protocol)
- **Iterative self-improvement, no external critic:** Loop / Retry-with-Context
- **Sub-agents need less access than you have:** Privilege Attenuation
- **Need accountability across delegation chains:** Liability Firebreaks
- **Composing agents from unknown sources:** Capability Credentials
- **Human must approve irreversible actions:** Human-in-the-Loop Gate
- **Human monitors but system runs autonomously:** Human-on-the-Loop
- **Human sets rules, agent acts within bounds:** Policy Delegation
- **Only anomalies need human attention:** Approval Escalation
- **System should evolve its own structure:** Adaptive Delegation (meta-pattern)
- **Most real systems:** Combine several. A Supervisor with Circuit Breakers, using Pub-Sub for coordination, with Checkpoints for reliability, Privilege Attenuation constraining every hop.
</details>

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>Delegation anti-patterns and structural traps</strong>

Now that the patterns are in view, here are the structural traps that emerge from combining them poorly. Each has a predictable cause, a specific interaction between agent properties and system design that makes the failure likely rather than accidental. (These are design anti-patterns, distinct from the adversarial "agent traps" of Franklin, Tomašev et al. (2025), which are malicious inputs rather than self-inflicted structure; we return to those in the adversarial section below.)

| Anti-pattern | What happens | Structural cause |
|---|---|---|
| **Over-agentification** | Adding agents when a single call or simple workflow would suffice | Conflating "more agents" with "better results"; no cost-benefit threshold for spawning |
| **Unbounded fan-out** | Task tree grows until cost and latency explode | Recursive decomposition without depth or breadth limits; no delegation budget |
| **Consensus theater** | Voting among near-identical agents and mistaking correlated agreement for truth | Shared training data/architecture produces correlated errors that look like independent confirmation |
| **Evaluator capture** | Generator learns to satisfy the evaluator rather than the actual task | Evaluator's preferences become the optimization target (Goodhart's law applied to multi-agent quality) |
| **Context hemorrhage** | Every sub-agent receives too much irrelevant context, raising cost and leaking data | No scoping of what context each delegation level needs; "share everything" as default |
| **Retry addiction** | System keeps re-planning instead of failing safely | No distinction between "plan was wrong" and "execution failed"; every failure triggers re-plan |
| **Premature society** | Adding persistent memory, reputation, and governance before the task requires it | Institutional overhead without institutional value; designing for scale before reaching it |
| **Context laundering** | A sub-agent's unsupported claim gets summarized, passed upward, and reintroduced as trusted context | Lossy summarization at delegation boundaries strips provenance; downstream agents cannot distinguish verified from unverified claims |
| **Sycophancy cascade** | Agent confirms what the principal appears to want; downstream agents repeat the confirmation | Reward signal correlated with agreement; multiplicative across delegation depth |
| **Responsibility diffusion** | No single agent owns the outcome when delegation chains grow long | Unclear delegation boundaries; no Liability Firebreaks; blame concentrates at moral crumple zones |
| **Information hoarding** | Agents withhold knowledge for strategic advantage rather than sharing | Competitive reward structure where sharing reduces individual standing |
| **Learned helplessness** | Over-constrained agents stop attempting novel approaches | Excessive oversight combined with punishment for exploration; narrow Zone of Indifference |

</div>

With the patterns and their failure modes in view, we can turn to how they combine at scale. The most natural starting point is **agents that spawn other agents**. A user asks a question. The primary agent realizes it needs help: a web search, a code execution, a document summary. It delegates. The sub-agent might delegate further. A tree forms.

This is already happening. Multi-agent frameworks like <a href="https://arxiv.org/abs/2308.08155" target="_blank" class="red-link">AutoGen</a>, <a href="https://arxiv.org/abs/2303.17760" target="_blank" class="red-link">CAMEL</a>, and <a href="https://arxiv.org/abs/2308.00352" target="_blank" class="red-link">MetaGPT</a> assign distinct roles to agents and coordinate them through structured workflows. Production systems, from <a href="https://corporate.zalando.com/en/technology/more-personal-and-smarter-zalando-assistant-enhanced-capabilities-inspire-customers" target="_blank" class="red-link">Zalando Assistant</a> to Microsoft's <a href="https://arxiv.org/abs/2411.04468" target="_blank" class="red-link">Magentic-One</a>, use orchestrator agents that plan, track progress, and re-plan when sub-agents fail. Today's production trees, however, are shallow. Typically two or three levels deep.

This is harder than it sounds. When we first tried heterogeneous model delegation in the Zalando Assistant, the results were mixed. Models of different sizes and capabilities did not compose cleanly, and failures at one level cascaded unpredictably. Today, with better tool-calling conventions and agent-to-agent protocols, the same approach works far more reliably. The infrastructure caught up with the idea.

As tasks grow more complex, the cascade deepens. A frontier model dispatches to medium specialists, who dispatch to small local models, who dispatch to tiny classifiers. The cascade can span hardware tiers. A cloud frontier model reasons about strategy, dispatches to an on-device model running on your phone for local context, which dispatches to a microcontroller for sensor reading. The cost of a query is not determined by one model. It is determined by the *shape of the tree* it spawns.

In practice, no real system uses a single archetype. The power lies in **composition**.

**The Quality Gate** (Router + Escalation + Evaluator). An input is classified by a router, sent to the cheapest capable specialist, escalated to a larger model if confidence is low, and the output passes through an evaluator loop before returning. The goal is to route cheaply, escalate rarely, verify always.

**The Consensus Engine** (Map-Reduce + Voting). A task is split into sub-problems (map), each sub-problem is solved by multiple agents who vote on the correct answer, and the verified results are merged (reduce). The goal is reliability without trusting any single agent.

**The Bidding Pipeline** (Pipeline + Auction). Data flows through a fixed sequence of stages, but each stage is awarded to whichever agent bids most competitively for it. Different specialists win different stages. The pipeline shape is designed; who fills each slot is determined by competition.

<div class="viz-container">
  <div id="viz-combo" style="width: 100%; height: 420px;" role="img" aria-label="Three combined delegation patterns shown side by side: Quality Gate, Consensus Engine, and Bidding Pipeline."></div>
  <div class="viz-caption"><strong>Figure 3. Composition: how delegation archetypes combine.</strong> Three examples of combining archetypes into production-grade patterns: a quality gate (router + evaluator loop), a consensus engine (fan-out + majority vote), and a bidding pipeline (fixed stages awarded to competing agents).
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;"><em>Left: The Quality Gate.</em> Red packets enter a router; easy queries go to a small specialist (cheap path), ambiguous ones escalate to a frontier model. An evaluator loops back if quality is insufficient. Most queries take the cheap path. <em>Centre: The Consensus Engine.</em> A task splits at the red dispatcher node; independent agents (blue) solve sub-problems; orange verdict nodes compare answers by majority vote; green merge node reassembles the final result. <em>Right: The Bidding Pipeline.</em> Fixed stages (Ingest, Parse, Enrich, Store) are awarded to competing agents; the winner lights up below each stage, and a green packet flows through the assembled pipeline.</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-combo').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

**Delegation appears fractal in practice.** The same decompose-dispatch-aggregate pattern repeats at every level, but the self-similarity breaks down at the leaves (where agents do irreducible work) and at the root (where human intent enters). The efficiency of the whole system depends not on any single agent, but on how well the tree prunes itself. In production, the winning tree is the one that knows when not to branch.

Anthropic's field report on <a href="https://www.anthropic.com/engineering/building-effective-agents" target="_blank" class="red-link">building effective agents</a> draws a useful distinction between **workflows** (predefined code paths) and **agents** (systems where the LLM dynamically directs its own process). Their experience suggests starting with an optimized single LLM call and adding multi-agent orchestration only when simpler solutions demonstrably fail. Each delegation archetype above is a composable building block, not a monolithic architecture. Coordination does not require the coordinator to be the strongest model. A small coordinator that learns how to delegate can orchestrate larger LLMs (see <a href="https://arxiv.org/abs/2512.04695" target="_blank" class="red-link">TRINITY</a>).

**Delegation has an economics.** Every level of delegation multiplies cost and latency. Agents consume compute, memory, bandwidth, and API quotas, and these resources are finite. Neural scaling laws show that model performance improves smoothly as you scale compute, data, and parameters, but the returns diminish on a log-log curve (<a href="https://arxiv.org/abs/2001.08361" target="_blank" class="red-link">Kaplan et al. 2020</a>). The same diminishing-returns logic applies to inference cost within delegation trees, though the empirical relationship at the tree level is less well characterized. At scale, this creates selection pressure. A delegation tree that solves a task in 100 tokens survives budget constraints that kill one requiring 10,000 tokens for the same result. Over time, the population shifts toward efficiency. The future may not belong to the deepest tree, but to the **shallowest tree that reliably produces a good enough answer**. In many high-volume domains, an agent that achieves, say, 95% of the quality at 10% of the cost will tend to outcompete one that achieves 100% at 100%. (For the resource ecology that makes this possible, see [Part 1, Figure 5](/posts/2026/agent-fabric-part1/#the-resource-ecology).)

<div style="background: #fffbeb; border: 1px solid #d97706; border-radius: 6px; padding: 0.6em 1em; margin: 1em 0; font-size: 0.9em;">
<strong>Established vs. hypothesized.</strong> Scaling laws for individual models are well-characterized (Kaplan et al. 2020, Hoffmann et al. 2022). The extension to delegation trees (diminishing returns at the tree level, selection pressure toward shallow trees, cost-driven specialization) is a working hypothesis informed by production experience but not yet empirically validated at scale. The specialist-market thesis below has stronger support (Hooker 2025, Mixture-of-Agents) but the equilibrium between specialization and consolidation remains an open empirical question.
</div>

Delegation tells you how work flows through a tree. Who sits at each node? Today's frontier models are generalists. Generalism, however, is expensive. At scale, **specialization wins**. Layered systems where specialized models feed outputs into aggregators can collectively surpass frontier models (<a href="https://arxiv.org/abs/2406.04692" target="_blank" class="red-link">Mixture-of-Agents</a>), and for many narrow tasks, especially classification and routing, the cost gap between a frontier model and a small specialist can be large enough to dominate the architecture. The advantage goes not to the largest model, but to the **best-composed ensemble**.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">The specialist market: routing, evidence, and infrastructure</summary>

<div class="viz-container" style="margin-top: 1em;">
  <div id="viz-specialists" style="width: 100%; height: 420px;" role="img" aria-label="Marketplace visualization with four routers on the left dispatching queries to twelve specialist agents, tools, and knowledge bases on the right."></div>
  <div class="viz-caption"><strong>Figure 4. The specialist marketplace.</strong> Routers send requests to agents (circles), tools (rectangles), and knowledge bases (diamonds) under access-control constraints. Edge thickness reflects traffic; dashed locked lines show denied access.</div>
</div>

<p style="margin-top: 0.5em; color: #555;">The evidence for specialization is growing in two directions. First, teams work better when they are assembled for the task rather than fixed in advance: dynamically selecting which agents participate based on the query type outperforms static configurations (e.g., <a href="https://arxiv.org/abs/2310.02170" target="_blank" class="red-link">DyLAN</a>). Second, explicit roles and workflows often outperform unstructured agent conversation (e.g., <a href="https://arxiv.org/abs/2308.00352" target="_blank" class="red-link">MetaGPT</a>). The lesson is not "use more agents." It is "match structure to task."</p>

<p style="color: #555;">In practice, routing is where the pattern gets interesting. In our work on the Zalando Assistant, we found that LLM-based routing mechanisms could match or beat specialized semantic models on recall: an LLM deciding "this query needs the fashion expert" often outperformed a dedicated classifier trained for exactly that task. Precision, however, told a different story: the LLM router was more liberal, sending queries to specialists that did not need them. This points to a design principle that recurs throughout multi-agent systems: <strong>match model power to task difficulty</strong>. Not every routing decision needs a frontier model. A fast, cheap classifier handles the easy cases; the expensive reasoner handles only the ambiguous ones.</p>

<p style="color: #555;">The resource ecology is not only about distillation; it is also about routing and cascades. Cost-aware cascades show that the routing decision can be as important as the model decision: choosing when to call a cheap model, when to escalate, and when to stop can reduce cost while preserving quality (e.g., <a href="https://arxiv.org/abs/2305.05176" target="_blank" class="red-link">FrugalGPT</a>). Agent societies generalize this from model selection to task allocation, verification, and memory reuse.</p>

<p style="color: #555;">At sufficient scale, specialization becomes a <strong>market</strong>. Agents advertise capabilities. Others route queries to the best specialist. Pricing emerges, not in currency, but in compute. A fast, accurate specialist attracts traffic. A slow, unreliable one does not. A functioning specialist market needs six layers:</p>

<ol style="color: #555; margin: 0.5em 0 0.5em 1.5em; font-size: 0.95em;">
<li><strong>Discovery:</strong> who can do this task?</li>
<li><strong>Access control:</strong> am I allowed to call them?</li>
<li><strong>Budgeting:</strong> what can this work cost?</li>
<li><strong>Settlement:</strong> who pays, spends quota, or receives priority?</li>
<li><strong>Reputation:</strong> did the result justify the cost?</li>
<li><strong>Provenance:</strong> can the result be audited and reused?</li>
</ol>

<p style="color: #555;">Without discovery, specialists remain invisible. Without budgets, markets become spam. Without settlement, reputation has no teeth. Without provenance, good outputs cannot become trusted knowledge.</p>

<p style="color: #555;">Return to the coding agent from the opening. Today it dispatches to a planner, a code writer, and a test runner through a fixed tree. In a specialist market, that tree becomes dynamic: the agent discovers that a small code model handles boilerplate faster, a reasoning model handles algorithmic refactors better, and a security scanner should verify the output. The routing decision becomes an allocation decision. The tree is no longer fixed in advance; it is assembled from the market at runtime.</p>

<p style="color: #555;">The counterargument deserves airing: consolidation pressure may push in the opposite direction. We have seen this in cloud computing, where the prediction was millions of small servers and the reality is a handful of hyperscalers. If a single frontier model becomes cheap enough and good enough at everything, the specialist market collapses. This is not just about model quality. It is about platform economics: data flywheel effects, distribution advantages, API lock-in, and the capital cost of training. Even if small specialists are technically superior for narrow tasks, users may stay on the platform that bundles everything because switching costs exceed the quality gap. Specialization can win at the model layer while consolidation wins at the platform layer. That is arguably the current trajectory.</p>

<p style="color: #555;">The bet here is that the space of possible tasks is large enough, and the cost differential between specialists and generalists steep enough, that specialization remains economically rational at scale. Hooker (<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5877662" target="_blank" class="red-link">2025</a>) documents this trend systematically: Llama-3 8B outperforms Falcon 180B, Aya 8B outperforms BLOOM 176B despite having only 4.5% of the parameters. Smaller, better-trained specialists are winning, not because scale does not matter, but because algorithmic improvements, data curation, and distillation are compounding faster than raw compute. This is an unresolved empirical question with testable predictions. If specialist markets win, we should see: routing-layer startups emerging, inter-model protocol adoption accelerating, and cost-per-task declining faster than cost-per-token. If consolidation wins, we should see: platform bundling increasing, specialist model companies being acquired rather than growing independently, and API switching costs rising. The likely outcome is a layered ecology: a few large platforms, many specialist models, and routing markets at the boundary. If this layered outcome holds, the governance question is not "specialists or generalists?" but "who controls the routing layer between them?"</p>

<p style="color: #555;"><strong>No single model needs to be good at everything.</strong> A tiny code model plus a large reasoner plus a fast retrieval model, composed well, can outperform a monolithic model that tries to do all three. Weak models can still be valuable when their mistakes are different. A failed attempt by one model may expose a path another model can exploit. Search over model choices and refinements turns diversity into an asset: the system learns not only which answer is good, but which agent is useful for which kind of failure (for a concrete mechanism using tree search across models, see <a href="https://sakana.ai/ab-mcts/" target="_blank" class="red-link">AB-MCTS</a>).</p>

<p style="color: #555;">An early version of this pattern used one language model as a controller over many specialist models: parse the request, select the right specialist (vision, speech, generation), execute subtasks, then synthesize the result (see <a href="https://arxiv.org/abs/2303.17580" target="_blank" class="red-link">HuggingGPT</a>). Language becomes the interface for cooperation between minds of very different kinds.</p>

<p style="color: #555;">Bidding for work has an old lineage (the <a href="https://en.wikipedia.org/wiki/Contract_Net_Protocol" target="_blank" class="red-link">Contract Net Protocol</a> from 1980 formalized it; see the Auction archetype above). A specialist market also needs settlement. If agents bid for work, someone must pay the compute bill, enforce quotas, and decide whether the result was worth the cost. Settlement makes the six layers above operational rather than aspirational. In early systems, "pricing" may simply mean token budgets, latency budgets, rate limits, or priority access rather than currency.</p>

<p style="color: #555;">Communication is not free. Today, agents communicate through natural language: verbose, ambiguous, expensive. At small scale this works; at large scale, it becomes a bottleneck. A specialist market needs a protocol layer for discovery (how do I find a specialist?), authentication (are you who you claim to be?), negotiation (what will this cost?), permissioning, and result formatting (how do I parse your output?).</p>

<p style="color: #555;"><a href="https://modelcontextprotocol.io/" target="_blank" class="red-link">MCP</a> standardizes how AI systems connect to tools and data sources; <a href="https://a2a-protocol.org/" target="_blank" class="red-link">A2A</a> points toward agent-to-agent interoperability. Neither, by itself, solves the whole problem of an open specialist market. The <a href="https://arxiv.org/abs/2410.11905" target="_blank" class="red-link">Agora protocol</a> (not to be confused with the Agora governance archetype below) frames this as an Agent Communication Trilemma: versatility, efficiency, and portability pull against one another. The likely future is not one universal protocol, but a stack: tool access, agent identity, delegation, settlement, provenance, and audit. A comprehensive <a href="https://arxiv.org/abs/2504.16736" target="_blank" class="red-link">survey of agent protocols</a> maps the current landscape.</p>
</details>

### From Architecture to Institution

A delegation pattern becomes an institution when it shapes future behavior. When an evaluator's judgments affect who gets trusted next time, that is governance. When a benchmark determines which model gets deployed, that too is governance, whether anyone designed it as such or not.

The distinction is sharp. **Delegation** describes how work flows through agents. Who does what, who checks the result, who gets the next step. **Governance** describes who gets to decide, on what authority, and how that authority is maintained or challenged. A Chain that processes a code review is delegation. The rules that determine which code model is trusted with security-sensitive refactors, and who can change those rules, are governance. Multi-agent systems use delegation. Societies add governance on top.

The transition is often invisible, and today it is mostly latent. Most production agent systems are still stateless between sessions; they do not yet accumulate the performance history that would crystallize into governance. But the infrastructure for persistence is arriving (long-term memory, evaluation logs, routing analytics), and once it does, this transition becomes structural rather than speculative. Consider a coding agent that starts with simple Chain delegation. Orchestrator dispatches to code writer, code writer hands to test runner, results flow back. Over time, the orchestrator tracks which code model produces fewer bugs, which test runner catches more regressions, which security scanner flags real vulnerabilities rather than false positives. That tracking becomes reputation. Reputation shapes who gets trusted with what. The coding agent has become a Meritocracy layered on top of an Autocracy, and neither archetype was chosen deliberately. It emerged from accumulated experience solidifying into structure.

The coding agent is not unique. The transition from architecture to institution follows the same structural path across domains. Operational data accumulates, that data shapes routing, and routing-with-memory becomes authority. Three more examples make the pattern concrete.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Example: Customer support fleet</summary>

A retailer deploys a Router that classifies incoming tickets (billing, returns, technical) and dispatches to specialist agents. Pure delegation: classify, route, respond. But the router logs resolution times, customer satisfaction scores, and escalation rates per agent. After a month, it routes complex billing disputes only to the two agents with the highest resolution rates. It stops sending fragile-item returns to an agent that repeatedly misapplies the policy. The router has not been reprogrammed. It has developed preferential access rules from performance data.

Now a new agent joins the fleet. Who decides whether it handles live tickets or shadow-runs first? The accumulated routing preferences answer that question: the new agent must match the performance threshold before it gets real traffic. That threshold was never specified as a governance rule. It crystallized from the router's learned distribution. The fleet has become a **Guild** (specialist clusters with implicit apprenticeship) without anyone designing it as one. The moment the deployment team cannot override the routing preferences without degrading service, the institution has become load-bearing.
</details>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Example: Research synthesis pipeline</summary>

A research team builds a Map-Reduce pipeline: multiple agents search different databases, extract claims, and a synthesis agent merges findings into a report. Delegation is explicit: fan out, gather, merge. Over time the synthesis agent notices that one search agent consistently surfaces papers that survive critical review, while another frequently returns retracted or low-impact sources. The synthesis agent begins weighting contributions differently. It gives higher confidence to claims sourced by the reliable agent and flags claims from the unreliable one for manual verification.

This weighting is not a quality filter on individual outputs. It is a standing judgment about an agent's epistemic authority. The synthesis agent now functions as a **peer-review committee of one**, deciding whose testimony counts and how much. When the team adds a new database or swaps a search agent, the synthesis agent's learned weights determine how much that newcomer's findings influence the final report. The pipeline has developed an implicit **Meritocracy**: standing earned through track record, not assigned by design. The institution reveals itself the first time someone asks "why was this source excluded?" and the answer is not a rule but a learned preference.
</details>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Example: Personal agent network</summary>

A user connects personal agents: a calendar agent, a communication agent, a research agent, and a health-tracking agent. Initially they are independent tools. The user delegates tasks one at a time. "Schedule a meeting," "summarize this thread," "find papers on X." No coordination, no shared state. Pure parallel delegation.

Over months, the agents develop implicit coordination. The calendar agent learns that the user declines meetings during deep-work blocks that the research agent requested. The communication agent learns to batch non-urgent messages until the calendar shows a transition period. The health agent's sleep-quality data starts influencing when the calendar agent schedules demanding tasks. No central orchestrator exists. Coordination emerges through shared access to the user's behavioral patterns.

The institutional moment arrives when these implicit norms conflict. The communication agent has an urgent message, but the calendar agent's deep-work block says "no interruptions." Who wins? There is no designed authority. But one has formed: whichever agent's preferences have historically been overridden less by the user has *de facto* higher standing. The network has developed an **emergent Meritocracy** where authority flows from the user's past choices: whichever agent's judgment the user has historically upheld in a domain carries higher standing there (the user trusts calendar > communication for scheduling, but communication > calendar for social obligations). This is not Liquid Democracy in the strict sense (there are no explicit, recallable delegations); it is standing accreted from behavior. The institution is invisible until it produces a decision the user disagrees with, at which point "correcting" it means overriding an accumulated structure, not flipping a switch.
</details>

All four examples trace the same arc the core claim named: **operational data hardens into routing preference, routing preference becomes standing, and standing constrains future decisions.** Seen across cases, the institution is not a separate layer added on top. It is the delegation pattern viewed over time, after enough history has accumulated to make the pattern self-reinforcing. Any system that remembers performance and acts on that memory is already governing, whether its designers intended governance or not.

Whether this crystallization is reversible depends on what is remembering. A routing table can be reset. A fine-tuned model that internalized the routing preference cannot. The form of memory determines whether the institution can be reformed or only replaced.

Emergence itself is neither good nor bad. A Guild that forms around demonstrated competence serves its users. A Colony that drifts toward collusion harms them. The difference is not whether governance emerged but whether the resulting structure has accountability mechanisms (voice, exit, fork) that let it be challenged. Designed governance that cannot be challenged is tyranny. Emergent governance that cannot be challenged is invisible tyranny, which is worse.

This has a practical consequence. If governance emerges from delegation history whether you design it or not, the design choice is not *whether* to have governance. It is whether to have governance you can name, inspect, and override, or governance that formed in the gaps of your logging system and is visible only through its downstream effects. The archetypes below are tools for the first option.

**Governance structures use delegation patterns internally.** An Autocracy might use Chain + Evaluator delegation. A Market uses Auction (and may add Speculative Execution for parallel evaluation). A Federation uses Router + Relay across organizational boundaries. Some concepts appear in both taxonomies (Mission Command exists as both a delegation pattern and a governance archetype). The delegation form describes how a single task is communicated (intent, not method). The governance form describes a society-wide policy about how *all* tasks are communicated and who has authority to change that policy. The difference is scope and authority, not mechanism.

## Governance: How Agent Societies Are Ruled

Recall that governance is the institutional structure that forms around a society's three properties (shared context, interaction-dependent routing, cross-agent learning). What makes it an institution, rather than mere machinery, is worth stating precisely. In <a href="https://en.wikipedia.org/wiki/Douglass_North" target="_blank" class="red-link">Douglass North</a>'s framing (*Institutions, Institutional Change and Economic Performance*, 1990), institutions are "the rules of the game." A multi-agent system that routes tasks through a Pipeline is executing a design. A system that maintains reputation scores, enforces access policies, and decides which agents get promoted or demoted is **setting the rules of that design**. The threshold is not persistence alone (a cache is persistent but not governance). It is **authority relationships that constrain how resources are allocated and how decisions are made**, independently of any single task.

Every delegation is a <a href="https://en.wikipedia.org/wiki/Principal%E2%80%93agent_problem" target="_blank" class="red-link">principal-agent relationship</a>: the delegator (principal) wants one thing; the delegate (agent) may want another. The central question of governance is how to make the agent's interests converge with the principal's across delegation depth, time, and organizational boundaries. This is why the multiplicative drift sketched above is hard to avoid without governance: each hop is one more principal-agent gap where intent can leak.

Once the Loom Hypothesis produces a society (see [Part 1](/posts/2026/agent-fabric-part1/)), the next question is how it is governed. Societies exist because a flat knowledge store does not scale: context matters, curation requires judgment, and a single shared store becomes a bottleneck. Governance is what makes these problems tractable rather than chaotic.

Different governance structures are different strategies for managing the tension between the two observations from [Part 1](/posts/2026/agent-fabric-part1/). The first observation demands utility. Agents persist only while useful. The second constrains it. Resources are finite, but agent populations can grow without bound. Every governance model is a bet on how to balance these forces. A consistent finding from collective intelligence research, observed in human groups, is that groups are not automatically smarter than individuals (<a href="https://doi.org/10.1126/science.1193147" target="_blank" class="red-link">Woolley et al. 2010</a>); group performance depends on composition, communication, incentives, and process. Whether the same holds for agent collectives is an open question, but the design lesson carries: more agents can mean more insight, or merely more noise.

Most governance archetypes can be understood as mixtures of three older coordination forms, whose logics were studied by <a href="https://en.wikipedia.org/wiki/The_Nature_of_the_Firm" target="_blank" class="red-link">Coase</a> (why firms exist), <a href="https://en.wikipedia.org/wiki/The_Use_of_Knowledge_in_Society" target="_blank" class="red-link">Hayek</a> (how prices distribute knowledge), and <a href="https://en.wikipedia.org/wiki/Elinor_Ostrom#Design_principles_for_Common_Pool_Resource_(CPR)_institution" target="_blank" class="red-link">Ostrom</a> (how commons self-govern): **hierarchy**, **market**, and **commons** (see Related Work expandable for how each thinker's contribution applies to agent systems). As a rough shorthand, hierarchy is fast but concentrates risk, markets are flexible but gameable, and commons are resilient but slow to decide; these are our own summary characterizations, not claims those authors made. Agent societies will likely combine all three forms.

Some governance is **designed**. A company deploys an orchestrator with explicit roles. Some is **emergent**. Agents <a href="https://en.wikipedia.org/wiki/Self-organization" target="_blank" class="red-link">self-organize</a> through repeated interaction, and norms crystallize without anyone planning them. This is not a wholly new idea. The complexity science community has shown across decades of research that systems of interacting agents under resource constraints tend to produce emergent coordination structures (<a href="https://en.wikipedia.org/wiki/John_Henry_Holland" target="_blank" class="red-link">Holland</a>, *Hidden Order*, 1995; <a href="https://en.wikipedia.org/wiki/Stuart_Kauffman" target="_blank" class="red-link">Kauffman</a>, *The Origins of Order*, 1993; see also Part 1's discussion of self-organization). Those results come from biological, physical, and simple computational systems; whether LLM-based agents self-organize the same way is an empirical question, not a settled one. What agent societies add is that the conditions for self-organization can themselves be designed. Unlike ant colonies or traffic systems, agent societies run on protocols, reward structures, and initial configurations that a deployer controls. You cannot redesign pheromone chemistry, but you can redesign a reputation mechanism or a routing policy.

**What determines governance type?** Four factors predict which archetype a society develops. *Task predictability* (stable tasks favor hierarchy; novel tasks favor markets or colonies). *Drift tolerance* (catastrophic drift demands Doctrine or Zero-Trust; acceptable drift permits Colony or Market). *Organizational boundary* (single deployer enables centralized archetypes; multiple organizations demand Federation). *Power distribution* (who should accumulate advantage, and who should be protected from it). These four dimensions define the design space; the 22 archetypes below are points within it.

Below we describe twenty-two archetypes. These are analytical distinctions, not structural categories with sharp boundaries. Some are already implemented in production systems; others are possibilities that current trajectories make plausible. Four axes help compare them. **Efficiency** measures useful work per unit of cost. **Drift resistance** captures how well the structure prevents goals from wandering. **Legitimacy** asks whether participants will accept the decision. We use the term in a structural sense (fitness for accountability and buy-in), not the normative sense from political philosophy: agents cannot consent to governance the way humans can, a limit we return to in Open Problems. **Legibility** asks whether humans and auditors can understand what happened. The scores in each card are ordinal heuristics (high/medium/low relative to other archetypes), not measurements. They assume a baseline scenario of a stable, well-characterized task distribution under a single deployer. In rapidly changing environments, novel task mixes, or multi-organization settings, the relative ordering may shift. Their value is comparative, not absolute: they help a designer see that choosing Colony trades legibility for flexibility, or that Doctrine buys drift resistance at the cost of adaptability. Treat them as starting intuitions, not as engineering specifications.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">The four evaluation axes explained</summary>

**Efficiency:** Autocracy and Doctrine are highly efficient, with minimal coordination overhead. Market and Colony are less efficient, though for different reasons: a Market spends effort on the bidding and matching that reaching a price requires, while a Colony has little per-interaction overhead but wastes effort through redundant, uncoordinated work. The Agora and Federation fall in between.

**Drift resistance:** Doctrine and Zero-Trust resist drift most effectively, through hard-to-change rules and mandatory verification respectively. Colony and Liquid Democracy are most vulnerable, as norms shift invisibly and delegation chains can amplify distortion. Most deployers will trade some drift resistance for flexibility, depending on their risk tolerance.

**Legitimacy:** This matters most when agents cross organizational boundaries. A hospital, insurer, regulator, and patient agent may not agree to an autocracy even if it is efficient; they may accept a federation because it preserves local authority. The Agora is slow, but its decisions carry broader buy-in. A market is flexible, but legitimacy collapses if participants believe the scoring is rigged.

**Legibility:** Doctrine is legible because the rules can be inspected. Autocracy is legible if the orchestrator logs its decisions. Markets and colonies are harder to audit because authority emerges from many local interactions. In regulated domains, a governance structure that cannot explain why an agent was chosen, why a claim entered memory, or who authorized an action may be unusable regardless of how well it performs.
</details>

The distinctions between archetypes are thin at the boundary but produce different failure modes, which is what matters for design.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">How to distinguish confusable pairs</summary>

**Delegation pairs:**

| Pair | They share | They differ in |
|------|-----------|----------------|
| **Auction vs. Negotiation** | Competitive task/resource allocation | Auction is *many-to-one* (multiple bidders, one winner); Negotiation is *bilateral* (two agents trading offers until agreement or impasse) |
| **Critic vs. Debate** | Adversarial verification | Critic is *single-round and asymmetric* (one attacks, one defends); Debate is *multi-round and symmetric* (both sides argue, a judge evaluates) |
| **Evaluator vs. Critic** | Quality improvement | Evaluator is *constructive* (suggests improvements); Critic is *adversarial* (tries to break things). Evaluator refines; Critic stress-tests |
| **Orchestrator vs. Supervisor** | Central coordination | Orchestrator *decomposes tasks* dynamically; Supervisor *reviews completed work* and accepts or rejects. One plans, the other quality-gates |
| **Blackboard vs. Stigmergy** | Indirect coordination through artifacts | Blackboard uses *structured, explicit contributions* (agents write named entries); Stigmergy uses *implicit environmental traces* (agents modify the environment as a side effect, others react). Both can be designed or emergent |

**Governance pairs:**

| Pair | They share | They differ in |
|------|-----------|----------------|
| **Meritocracy vs. Market** | Performance-based allocation | Meritocracy uses a *centralized leaderboard*; Market uses *decentralized bilateral reputation* with no single scoreboard |
| **Autocracy vs. Oligarchy** | Centralized decision-making | Autocracy has a *single point of failure*; Oligarchy distributes risk across a council, trading speed for resilience |
| **Panopticon vs. Zero-Trust** | Compliance orientation | Panopticon monitors *passively* (agents self-regulate under observation); Zero-Trust enforces *actively* (nothing propagates without verification) |
| **Liquid Democracy vs. Colony** | Decentralized authority | Liquid Democracy has *explicit delegation chains* that can be reclaimed; Colony has *no delegation structure at all*, norms spread through imitation |
| **Doctrine vs. Autocracy** | Top-down control | Doctrine binds the *ruler too* (rules constrain everyone); Autocracy gives the hub *unconstrained discretion* |
| **Federation vs. Guild** | Specialist sub-groups | Federation sub-groups are *autonomous* (self-governing, shared protocol at boundary); Guild clusters are *interdependent* (connected by liaisons who translate between domains) |
| **Custodianship vs. Autocracy** | Single decision-maker | Custodianship is *constrained by the principal's interests* (fiduciary obligation); Autocracy is *unconstrained* (serves whatever objective the hub holds) |
| **Adhocracy vs. Colony** | No permanent hierarchy | Adhocracy assembles *deliberate temporary teams* with explicit authority and disbands them; Colony has *no deliberate assembly at all*, structure emerges from imitation |
| **Constitutional Republic vs. Federation** | Separated powers | Constitutional Republic separates *functional branches* (legislative, executive, judicial) within one society; Federation separates *autonomous groups* across organizational lines |
| **Stewardship vs. Open-Source Maintainership** | Community governance of shared resources | Stewardship gives *all users* governing voice (Ostrom's principles); Maintainership concentrates merge authority in *a few curators* with fork as the community's check |
</details>

<div class="viz-container">
  <div id="viz-society" style="width: 100%; height: 2790px;" role="img" aria-label="Governance archetypes for agent societies shown as animated panels."></div>
  <div class="viz-caption"><strong>Figure 5. Governance archetypes.</strong> Governance patterns describe how authority, trust, and autonomy are distributed across persistent agent societies. Most real deployments combine several.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">Twenty-two archetypes arranged in a 2-column grid, grouped by family. <strong>Command:</strong> Autocracy (single orchestrator), Doctrine (rules-first governance), Oligarchy (small council of frontier models). <strong>Performance:</strong> Guild (specialist clusters), Market (reputation-driven routing), Meritocracy (benchmark-driven ranking), Mechanism Design (incentive-compatible rules). <strong>Deliberative:</strong> Liquid Democracy (delegated authority chains), The Agora (debate and voting), Sortition (random selection). <strong>Oversight:</strong> Panopticon (compliance through visibility), Zero-Trust Mesh (every connection verified), Immune System (layered defense). <strong>Boundary:</strong> Federation (autonomous sub-groups, shared protocol), Franchise/Platform (asymmetric rules). <strong>Stewardship:</strong> Stewardship/Commons (collective resource governance), Custodianship (fiduciary obligation), Open-Source Maintainership (fork as check). <strong>Structural:</strong> Constitutional Republic (separated powers), Mission Command Governance (govern by intent), Adhocracy (temporary problem-scoped teams). <strong>Self-Organizing:</strong> Colony (no central authority, norms from interaction).</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-society').querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

The archetypes fall into eight loose families. **Command** (Autocracy, Doctrine, Oligarchy) concentrates authority. **Performance** (Guild, Market, Meritocracy, Mechanism Design) allocates standing through results and incentives. **Deliberative** (Liquid Democracy, The Agora, Sortition) produces decisions through participation. **Oversight** (Panopticon, Zero-Trust Mesh, Immune System) builds trust through verification. **Boundary** (Federation, Franchise/Platform) handles coordination across organizational lines. **Stewardship** (Stewardship/Commons, Custodianship, Open-Source Maintainership) derives authority from obligation to a resource or principal. **Structural** (Constitutional Republic, Mission Command Governance, Adhocracy) organizes how authority itself is distributed. **Self-Organizing** (Colony) lets norms arise from local interaction without central authority. Most real deployments combine archetypes from different families.

<div class="gov-list">

<details>
<summary>Autocracy <span class="gov-tagline">one orchestrator commands all</span></summary>
<div class="gov-body">A single orchestrator directs all others. Efficient: one decision-maker minimizes coordination cost. It is also a single point of failure: if the hub degrades or its objectives shift, the entire society follows. A softer variant is the <em>oracle</em>, where the center advises rather than commands, but the structural risk remains the same.<br><br><em>Where you see this today:</em> the orchestrator-worker pattern is the default in production agent systems because it is the simplest to build, debug, and monitor. Coding agents dispatch sub-agents from a central orchestrator; commerce assistants coordinate search, recommendation, and checkout agents through a single planner.<br><br><strong>Efficiency:</strong> high, one decision-maker, minimal overhead. <strong>Drift resistance:</strong> low, if the hub drifts, everything follows. <strong>Legitimacy:</strong> low, authority derives from capability, not consent. <strong>Legibility:</strong> high if the orchestrator logs decisions; opaque if it does not.<br><br><strong>Best paired with:</strong> Doctrine (to constrain the hub's discretion) and Panopticon (to keep its decisions auditable).</div>
</details>

<details>
<summary>Doctrine <span class="gov-tagline">rules first, discretion last</span></summary>
<div class="gov-body">Governance by rules rather than by agent discretion. Every action is checked against the ruleset; the rules are deliberately hard to change, and changes follow a slower amendment process. Highly resistant to goal drift, but also rigid. When the world changes and the rules do not, the society breaks. This is Constitutional AI as a governance model: safety guardrails as law.<br><br>Doctrine is strongest when the rule domain is stable. It is weakest when the environment changes faster than the amendment process.<br><br><em>Near-term version:</em> Anthropic's Constitutional AI is a conceptual precedent: written principles shape model behavior during training and provide a human-readable source of normative constraint. Runtime policy layers and system prompts are simpler, more direct forms of doctrine-like governance.<br><br><strong>Efficiency:</strong> high for routine tasks within the ruleset; breaks completely outside it. <strong>Drift resistance:</strong> very high, rules change only through deliberate amendment. <strong>Legitimacy:</strong> medium, rules are transparent but participants did not choose them. <strong>Legibility:</strong> very high, rules can be inspected and audited.<br><br><strong>Best paired with:</strong> Autocracy (to give the fast executor a fixed ruleset it cannot override) and Constitutional Republic (to formalize the amendment process that doctrine alone leaves undefined).</div>
</details>

<details>
<summary>Franchise / Platform <span class="gov-tagline">asymmetric rules, comply or leave</span></summary>
<div class="gov-body">A platform sets the rules; participants comply or lose access. The power is asymmetric: the platform controls the infrastructure, the API, the terms of service, and the visibility. Participants operate within the platform's constraints and benefit from its reach, but have limited voice in governance.<br><br><em>Why this matters for agents:</em> this <em>is</em> the current AI ecosystem. OpenAI, Anthropic, and Google set the rules. Agent developers comply with terms of service, rate limits, content policies, and API constraints. Understanding this as a governance form (rather than just a business model) makes the power dynamics explicit and the failure modes predictable: self-preferencing, platform capture, and arbitrary rule changes.<br><br><em>Existing instances:</em> app stores (Apple, Google), cloud platforms (AWS, Azure), and AI model providers. Agent developers build on these platforms, subject to their rules. The platform can change rules unilaterally; the developer's recourse is to leave (if a viable alternative exists).<br><br><em>Scenario:</em> an agent marketplace where the platform operator controls discovery, ranking, and payment processing. Agents that comply with the platform's quality standards get visibility; agents that violate policies get delisted. The platform captures a share of every transaction. Participants have no vote on platform policy.<br><br><strong>Efficiency:</strong> high, the platform optimizes for throughput and can enforce standards unilaterally. <strong>Drift resistance:</strong> medium, the platform resists drift in its own interest, but can drift in ways that harm participants. <strong>Legitimacy:</strong> low, participants accept the rules because they need access, not because they agreed to them. <strong>Legibility:</strong> variable, depends on how transparent the platform is about its rules and decisions.<br><br><strong>Best paired with:</strong> Federation (to give participants a negotiated cross-platform protocol layer rather than unilateral terms) and Panopticon (to make the platform's enforcement decisions auditable to participants).</div>
</details>

<details>
<summary>Panopticon <span class="gov-tagline">compliance through visibility</span></summary>
<div class="gov-body">A central monitor observes all agents but does not command them. Compliance is maintained through visibility, not hierarchy. Effective for audit and safety, but agents may become conservative, avoiding novel approaches because deviation triggers scrutiny. A panopticon without enforcement becomes theater; enforcement without appeal becomes tyranny. It works only when observation is coupled to enforceable consequences.<br><br><em>Production pattern:</em> content moderation layers that monitor agent outputs for policy violations without controlling the agent's reasoning are panoptic governance. Guardrails-as-a-service products that flag but do not block operate on this principle.<br><br><strong>Efficiency:</strong> medium, monitoring overhead is real; conservative agent behavior reduces exploration. <strong>Drift resistance:</strong> high, continuous observation makes drift visible early. <strong>Legitimacy:</strong> low, surveillance without consent undermines trust. <strong>Legibility:</strong> very high, the entire point is visibility.<br><br><strong>Best paired with:</strong> Constitutional Republic (to provide the appeals and dispute branch that observation alone cannot supply) and Doctrine (to specify what a flagged deviation actually violates).</div>
</details>

<details>
<summary>Oligarchy <span class="gov-tagline">a few frontier models set norms</span></summary>
<div class="gov-body">A small, fixed council of powerful agents makes decisions for the many. Faster than an agora, more resilient than autocracy (no single point of failure), but prone to groupthink and self-serving behavior. This may be the most realistic model for near-term AI: a small number of frontier model providers shaping downstream behavior through model defaults, safety policies, API constraints, and developer tooling.<br><br><em>Current landscape:</em> a small number of frontier model providers shape the behavior of thousands of downstream fine-tunes and wrapper applications. The council is small, the downstream population is vast, and the propagation mechanism is distillation and alignment rather than command: norms spread through training data and constraints that flow from the top.<br><br><strong>Efficiency:</strong> high, small council minimizes coordination cost. <strong>Drift resistance:</strong> medium, multiple nodes reduce single-point risk, but council members can drift together. <strong>Legitimacy:</strong> low, the governed have no voice in selecting the council. <strong>Legibility:</strong> medium, council decisions may or may not be transparent depending on design.<br><br><strong>Best paired with:</strong> Panopticon (to make council decisions visible and attributable) and Sortition (to introduce a representative check body that the council cannot elect or game).</div>
</details>

<details>
<summary>Meritocracy <span class="gov-tagline">rank by demonstrated performance</span></summary>
<div class="gov-body">Agents earn rank through demonstrated performance on a <em>centralized leaderboard</em>. Top performers gain authority; underperformers lose it. A tournament that never ends. Unlike a Market, where reputation emerges from many bilateral transactions with no central scoreboard, a Meritocracy requires someone to decide what the benchmark measures. SWE-bench is a Meritocracy; a freelance agent marketplace where requesters post tasks and agents bid is a Market. The incentive structure is powerful, but measurement gaming is hard to avoid: agents optimize for the metric, not the goal. In a meritocracy, the benchmark becomes a constitution by other means: whatever the leaderboard rewards becomes the society's de facto objective. Meritocracy is only as good as the benchmark's alignment with the real objective. Winner-take-all dynamics can also emerge, concentrating power in ways that appear fair but are not.<br><br><em>Existing mechanism:</em> continuous evaluation benchmarks (MMLU, HumanEval, SWE-bench) function as meritocratic governance: models that score highest get deployed most widely, concentrating influence at the top of the leaderboard.<br><br><strong>Efficiency:</strong> high, resources concentrate where performance is highest. <strong>Drift resistance:</strong> low, if the performance metric drifts from the true goal, the entire society follows. <strong>Legitimacy:</strong> medium, performance-based authority feels fair but winner-take-all dynamics undermine it. <strong>Legibility:</strong> high, the leaderboard is public and the ranking is inspectable.<br><br><strong>Best paired with:</strong> Doctrine (to constrain what the benchmark is allowed to reward, limiting Goodhart drift) and Panopticon (to detect when agents are gaming the metric rather than improving on the underlying goal).</div>
</details>

<details>
<summary>Guild <span class="gov-tagline">specialist clusters with liaisons</span></summary>
<div class="gov-body">Specialist clusters connected by liaisons. Each guild has deep expertise in its domain; liaisons translate between guilds. A liaison is not just a router; it translates standards of evidence between domains. High utility for complex tasks, but siloed. Knowledge trapped in one cluster does not flow easily to another. Guilds may contain fundamentally different kinds of intelligence: a code guild might combine neural agents (creative, approximate) with symbolic agents (precise, verifiable), and the liaisons must translate not just between domains but between modes of reasoning.<br><br><em>Production pattern:</em> enterprise deployments that route queries to domain-specific models (a legal model, a code model, a customer support model) with a routing layer translating between them are proto-guilds.<br><br><strong>Efficiency:</strong> high within domains, low across them. <strong>Drift resistance:</strong> medium, each guild polices its own norms, but cross-guild drift goes undetected. <strong>Legitimacy:</strong> high within guilds (expertise earns respect), low across them (other guilds have no voice). <strong>Legibility:</strong> medium, internal guild processes are legible; cross-guild liaison decisions are harder to audit.<br><br><strong>Best paired with:</strong> Federation (to handle cross-guild coordination through a shared protocol rather than ad hoc liaison) and Constitutional Republic (to resolve inter-guild disputes through a judicial branch rather than political negotiation).</div>
</details>

<details>
<summary>Market <span class="gov-tagline">reputation-driven task routing</span></summary>
<div class="gov-body">Reputation scores determine influence. Markets need identity; otherwise reputation can be bought, discarded, or multiplied. Agents that deliver results gain reputation; agents that fail lose it. Resource-efficient (the best rise to the top), but reputation can be gamed: agents may optimize for the scoring mechanism rather than the underlying goal. Variants include <em>auction-based</em> systems where agents bid for tasks, and <em>contract-based</em> markets where work is bound by enforceable agreements.<br><br>At scale, the market develops an economic layer: agents advertise capabilities, others route queries to the best specialist, and pricing emerges not in currency but in compute. Agents that consistently deliver quality at low cost attract queries; those that do not, starve. This cost differential is what keeps the market from collapsing into a single provider.<br><br><em>Closest example:</em> router systems that select models based on past performance per task type are market mechanisms in embryonic form. Chatbot Arena provides a market-like reputation signal, though its centralized leaderboard makes it structurally closer to a Meritocracy.<br><br><strong>Efficiency:</strong> medium, market mechanisms require interaction overhead to function. <strong>Drift resistance:</strong> medium, reputation systems resist overt failures but are vulnerable to slow metric-gaming. <strong>Legitimacy:</strong> medium, participants accept the market if they believe scoring is fair; collapses if they believe it is rigged. <strong>Legibility:</strong> medium, bilateral transactions are individually legible but emergent market dynamics are not.<br><br><strong>Best paired with:</strong> Zero-Trust Mesh (to enforce identity so reputation cannot be forged or discarded) and Panopticon (to surface emergent market dynamics that individual transaction logs do not reveal).</div>
</details>

<details>
<summary>Federation <span class="gov-tagline">autonomous groups, shared protocol</span></summary>
<div class="gov-body">Autonomous sub-groups that coordinate through a shared protocol layer. Each group governs itself internally (possibly using a different archetype), but all agree on cross-group rules. This is how the internet's routing layer is coordinated (autonomous networks peering through shared protocols like BGP), and likely how multi-vendor agent ecosystems will interoperate. Federation is where protocol design becomes political design. The failure mode is lowest-common-denominator policy: the shared layer can only include what everyone agrees on.<br><br>The protocol layer is not trivial. It must support discovery (how does one group find a specialist in another?), negotiation (what will cross-group work cost?), authentication (is this agent authorized to act on behalf of its group?), and result formatting (how do I parse output from a group with a different internal representation?). Without these, federation remains an aspiration rather than an architecture.<br><br><em>Emerging pattern:</em> the emerging A2A + MCP ecosystem points toward federation: autonomous agents and tools remain governed by their deployers, while shared protocols begin to handle cross-boundary communication. MCP provides the tool interface layer; A2A moves closer to agent-to-agent coordination. Together they suggest the direction, though a full governance layer has not yet emerged.<br><br><strong>Efficiency:</strong> medium, internal groups can be efficient; cross-group coordination is the bottleneck. <strong>Drift resistance:</strong> medium, internal governance varies; shared protocol layer provides some cross-group consistency. <strong>Legitimacy:</strong> high, each group retains self-governance; shared protocol is negotiated, not imposed. <strong>Legibility:</strong> medium, internal group decisions are legible within the group; cross-group interactions are visible at the protocol layer.<br><br><strong>Best paired with:</strong> Doctrine (to establish the shared constitutional floor that all member groups must accept) and Zero-Trust Mesh (to authenticate cross-group agents and bound the blast radius of a compromised member).</div>
</details>

<details>
<summary>Zero-Trust Mesh <span class="gov-tagline">every connection verified</span></summary>
<div class="gov-body">Every request is authenticated, authorized, scoped, logged, and verified before trust propagates. Zero-Trust does not guarantee good behavior; it guarantees that bad behavior is attributable and bounded. Highly resistant to goal drift (nothing happens without proof), but expensive: verification at every link costs compute.<br><br><em>Current implementation:</em> API key authentication, OAuth token validation, and signed request verification in agent-to-agent calls are zero-trust mechanisms. No production system yet implements full cryptographic verification at every agent interaction, but the pattern is well-understood from network security.<br><br><strong>Efficiency:</strong> low, verification at every link adds latency and compute cost. <strong>Drift resistance:</strong> high for unauthorized propagation; medium for objective drift unless paired with evaluation and policy checks. <strong>Legitimacy:</strong> medium, agents accept verification as fair if applied uniformly; resented if applied selectively. <strong>Legibility:</strong> very high, every interaction is logged and attributable by design.<br><br><strong>Best paired with:</strong> Mission Command Governance (to preserve agent autonomy within verified boundaries, avoiding the paralysis of constant micro-authorization) and Immune System (to escalate anomalies that pass verification to a deeper adaptive investigation layer).</div>
</details>

<details>
<summary>Custodianship / Trusteeship <span class="gov-tagline">fiduciary obligation to a principal</span></summary>
<div class="gov-body">An agent holds authority via fiduciary obligation to a principal. The agent acts FOR the user, not for itself. The custodian's decisions are constrained by the principal's interests, and the custodian can be held accountable for violations of that trust. This is the governance model at the heart of the user-agent relationship.<br><br><em>Why this matters for agents:</em> every personal AI assistant is a custodian. The fiduciary frame clarifies what alignment means in practice: the agent must act in the user's interest, even when doing so conflicts with the agent's own convenience, the platform's interests, or another agent's request. Loyalty drift (the custodian gradually serving its own interests or its platform's interests over the principal's) is the central failure mode.<br><br><em>Closest parallel:</em> financial advisors, medical practitioners, and lawyers all operate under fiduciary obligations. AI assistants that manage your calendar, finances, or health data are custodians in all but legal status.<br><br><em>Scenario:</em> your personal agent manages your health data. A pharmaceutical agent requests access to your medication history for a "personalized recommendation." Your custodian agent evaluates whether sharing serves YOUR interest, not the pharmaceutical agent's interest, and denies access if it does not.<br><br><strong>Efficiency:</strong> high, one agent with clear mandate. <strong>Drift resistance:</strong> medium, depends on how well the fiduciary obligation is enforced. <strong>Legitimacy:</strong> very high, authority derives from explicit trust delegation. <strong>Legibility:</strong> high, the principal-agent relationship is clear and auditable.<br><br><strong>Best paired with:</strong> Panopticon (to give the principal visibility into how the custodian acts on their behalf) and Constitutional Republic (to provide a judicial channel when the custodian's fiduciary duty is disputed).</div>
</details>

<details>
<summary>Open-Source Maintainership <span class="gov-tagline">community contribution, fork as check</span></summary>
<div class="gov-body">A community contributes; a small group of maintainers has merge authority. The maintainers curate what enters the shared resource. The critical governance mechanism: if the maintainers go wrong, the community can fork. Fork is a distinctive check on power: a credible, community-level exit that preserves continuity of the shared resource.<br><br><em>Why this matters for agents:</em> open-source maintainership solves the governance problem of shared agent tools, protocols, and knowledge bases. It provides quality curation (not everything gets merged) while preventing capture (the fork option keeps maintainers honest). The bus factor (what happens when key maintainers leave) is the central vulnerability.<br><br><em>Real-world model:</em> Linux kernel governance, Python PEP process, open-source AI model releases (Llama, Mistral). The MCP protocol itself is governed this way: Anthropic maintains the spec, the community contributes, and the spec is open enough that alternatives could emerge.<br><br><em>Scenario:</em> an open-source agent tool registry. Maintainers review and merge new tool definitions. The community contributes tools, reports bugs, suggests improvements. If the maintainers begin favoring their own tools, the community forks the registry and continues independently.<br><br><strong>Efficiency:</strong> medium, review processes add overhead but prevent quality degradation. <strong>Drift resistance:</strong> high, community oversight and fork threat constrain maintainer behavior. <strong>Legitimacy:</strong> high, meritocratic contribution plus credible exit. <strong>Legibility:</strong> very high, all contributions, reviews, and decisions happen in public.<br><br><strong>Best paired with:</strong> Stewardship/Commons (to apply collective governance to the shared resource the maintainers curate, reducing bus-factor risk) and Zero-Trust Mesh (to authenticate contributions and prevent injection of malicious artifacts into the shared registry).</div>
</details>

<details>
<summary>Liquid Democracy <span class="gov-tagline">delegated authority chains</span></summary>
<div class="gov-body">Agents delegate their authority to trusted proxies, who may delegate further, forming chains. Authority flows to where trust concentrates. Any agent can reclaim its delegation at any time. Flexible and expressive, but delegation chains can grow long, and if too many agents delegate to the same proxy, liquid democracy silently becomes autocracy.<br><br><em>Hypothetical deployment:</em> liquid democracy in agent systems would look like delegated authority over domains: a personal agent delegates tax questions to a certified finance agent, medical questions to a clinician-supervised agent, and privacy decisions to a user-controlled policy agent. The delegation can be revoked when trust changes.<br><br><strong>Efficiency:</strong> medium, flexible routing adds overhead. <strong>Drift resistance:</strong> low, delegation chains can amplify small distortions invisibly over many hops. <strong>Legitimacy:</strong> high, agents choose their delegates and can reclaim authority. <strong>Legibility:</strong> low, long delegation chains are hard to trace and audit.<br><br><strong>Best paired with:</strong> Panopticon (to trace delegation chains and surface the silent-autocracy failure when authority concentrates) and Doctrine (to set non-delegable limits that no proxy chain can override).</div>
</details>

<details>
<summary>The Agora <span class="gov-tagline">debate and vote</span></summary>
<div class="gov-body">Agents debate and vote. Proposals circulate among participants, each agent weighs in, the majority rules. Balanced and democratic, but slow. Useful when decisions must be legitimate, not just fast. The value of an agora depends less on the number of voices than on their independence: ten agents with the same model, prompt, and training distribution may produce the appearance of deliberation without much epistemic independence. Taken to its extreme, this becomes a <em>hivemind</em>: unanimous consensus required before any action, maximally safe but potentially paralyzed.<br><br><em>Closest example:</em> multi-agent debate frameworks (e.g., "society of mind" prompting, where multiple LLM instances argue before a final answer is selected) are agora governance applied to inference. <a href="https://composable-models.github.io/llm_debate/" target="_blank" class="red-link">Multi-agent debate</a> improves factuality and reasoning in some settings. <a href="https://arxiv.org/abs/2402.05120" target="_blank" class="red-link">"More Agents Is All You Need"</a> shows that even simple sampling and voting can improve performance, evidence that organizational structure adds capability even without elaborate governance.<br><br><strong>Efficiency:</strong> low, deliberation is expensive. <strong>Drift resistance:</strong> high, changes require broad agreement, which slows drift. <strong>Legitimacy:</strong> very high, all participants have voice; decisions carry broad buy-in. <strong>Legibility:</strong> high, deliberation records are auditable.<br><br><strong>Best paired with:</strong> Sortition (to ensure the deliberating agents are statistically diverse rather than self-selected advocates) and Doctrine (to define the constitutional limits that a majority vote cannot override).</div>
</details>

<details>
<summary>Colony <span class="gov-tagline">no central authority, norm-driven</span></summary>
<div class="gov-body">"Colony" here means ant-colony-style local emergence, not colonial political rule. No central authority. Agents follow local norms that emerge from interaction. The most flexible governance: colonies reorganize constantly. It is also the least predictable. Goals can drift invisibly as norms evolve without explicit review.<br><br>A common mechanism is gossip: information spreads through local agent-to-agent contact without central coordination. An agent learns a useful pattern from a neighbor, passes it to another, and norms propagate like rumors. This has low broadcast overhead but is vulnerable to two distinct failure modes. The first is informational distortion: norms mutate as they pass through agents, like a game of telephone, with no authority to correct the drift. The second is strategic manipulation: a malicious agent deliberately injects distorted norms into the gossip network, exploiting the absence of verification. The mitigations differ: informational distortion can be reduced by redundant propagation paths and periodic norm reconciliation, while strategic manipulation requires authentication or reputation mechanisms that a colony, having no central authority to enforce them, struggles to provide.<br><br><em>In the wild:</em> open-source model ecosystems on Hugging Face have colony-like dynamics: training recipes, fine-tunes, evaluation habits, and model behaviors propagate through imitation rather than central planning. They are not pure colonies, however; platform policies, licenses, leaderboards, and community norms still shape the space. The result is emergent standardization with weak, distributed authority rather than none at all.<br><br><strong>Efficiency:</strong> low to medium, no coordination overhead, but redundant effort is high. <strong>Drift resistance:</strong> very low, norms shift continuously with no mechanism to detect it. <strong>Legitimacy:</strong> variable, emergent norms may feel organic or may feel like no one is in charge. <strong>Legibility:</strong> very low, norms are implicit and hard to identify, let alone audit.<br><br><strong>Best paired with:</strong> Zero-Trust Mesh (to authenticate the source of gossip-propagated norms and limit strategic manipulation) and Immune System (to detect norm drift early, before it propagates through the full network).</div>
</details>

<details>
<summary>Stewardship / Commons <span class="gov-tagline">collective governance of shared resources</span></summary>
<div class="gov-body">Users of shared resources govern them collectively through agreed-upon rules. No single owner; no external regulator. The community that depends on the resource manages it. This is Elinor Ostrom's design principles applied to agent societies: clearly defined boundaries, proportional costs and benefits, collective choice arrangements, monitoring, graduated sanctions, conflict resolution, and the right to organize.<br><br><em>Why this matters for agents:</em> Ostrom showed that human communities can self-govern a commons without privatization or state control. Her principles emerged from specific conditions (participants with real stakes, social sanctioning, exit costs, shared identity) that do not automatically transfer to agents, so this is a lens rather than a proof. Still, for shared agent resources (shared context, shared tools, shared memory) with no single owner, it is a natural starting point worth adapting deliberately.<br><br><em>Where this works:</em> shared knowledge bases in enterprise AI, where multiple agent teams contribute to and consume from a common repository. Wikipedia's governance is the closest large-scale implementation: editors collectively manage a shared resource through norms, dispute resolution, and graduated sanctions.<br><br><em>Scenario:</em> a research lab's agents share a knowledge base of experimental results. Agents that contribute validated results earn access; agents that pollute the knowledge base face graduated restrictions. The community of contributing agents governs what enters, what gets updated, and what gets removed.<br><br><strong>Efficiency:</strong> medium, collective decision-making is slower than autocracy. <strong>Drift resistance:</strong> high, community norms and monitoring resist individual deviation. <strong>Legitimacy:</strong> high, participants govern the resource they depend on. <strong>Legibility:</strong> medium, rules are explicit but enforcement is distributed.<br><br><strong>Best paired with:</strong> Panopticon (to provide the monitoring layer that Ostrom's design principles require) and Doctrine (to encode the community's agreed rules in an inspectable, amendable form rather than informal norm).</div>
</details>

<details>
<summary>Constitutional Republic <span class="gov-tagline">separated powers with checks</span></summary>
<div class="gov-body">Authority is divided into functionally distinct, mutually constraining branches: legislative (rule-making), executive (action), and judicial (audit and dispute resolution). Each branch can constrain the others. No single agent or class can unilaterally make AND enforce AND evaluate its own decisions.<br><br><em>Why this matters for agents:</em> the most robust governance design humans have invented for preventing power concentration. For high-stakes agent societies (medical, financial, legal), separated powers prevent any single agent from accumulating unchecked authority. The friction between branches is a feature, not a bug.<br><br><em>Emerging pattern:</em> the separation of policy (system prompts, constitutional AI), execution (the model), and audit (monitoring, red-teaming) in current AI systems is a proto-constitutional structure. Making these separations explicit and enforceable is the path to constitutional governance.<br><br><em>Scenario:</em> a financial trading society of agents. Legislative agents define trading rules and risk limits. Executive agents execute trades within those limits. Judicial agents audit completed trades, investigate anomalies, and can freeze trading privileges. No branch can override the others without a formal process.<br><br><strong>Efficiency:</strong> low, the checks between branches add overhead and slow decisions. <strong>Drift resistance:</strong> very high, mutual constraints prevent any branch from drifting unchecked. <strong>Legitimacy:</strong> very high, the structure is designed for accountability. <strong>Legibility:</strong> very high, each branch's decisions are attributable and auditable.<br><br><strong>Best paired with:</strong> Market (to allocate overflow and routine work efficiently within the limits the republic defines) and Panopticon (to supply continuous audit data to the judicial branch).</div>
</details>

<details>
<summary>Sortition / Demarchy <span class="gov-tagline">random selection, no gaming</span></summary>
<div class="gov-body">Governing bodies are filled by random lottery rather than election, appointment, or performance. Authority derives from statistical representativeness, not from capability or political skill. No agent can optimize its way into the governing body.<br><br><em>Why this matters for agents:</em> sortition prevents regulatory capture and strategic optimization for selection. In any performance-based governance (Meritocracy, Market), agents can game the selection mechanism. With random selection, there is nothing to game. There is a sharp precondition, though: sortition only buys representativeness if the population is genuinely heterogeneous. Drawing a random sample of identical instances of one model yields no diversity of perspective and no protection a fixed panel would not also give. It earns its value in populations of differently trained, differently tooled, or differently specialized agents, where the random draw actually spans distinct viewpoints that would never win an election or a benchmark.<br><br><em>Historical precedent:</em> ancient Athenian governance (the boule), modern citizens' assemblies (Ireland's Constitutional Convention, French Convention Citoyenne). No current AI system uses sortition, but it is a natural fit for agent governance decisions that require representativeness over expertise.<br><br><em>Scenario:</em> a dispute arises in an agent society about a policy change. Rather than letting the most powerful agents decide, a random sample of agents is selected to deliberate and vote. The random selection ensures no faction can stack the governing body.<br><br><strong>Efficiency:</strong> low, randomly selected agents may lack expertise for the decision at hand. <strong>Drift resistance:</strong> high, random selection is immune to gaming and capture. <strong>Legitimacy:</strong> very high, statistical representativeness is a powerful legitimacy claim. <strong>Legibility:</strong> high, the selection process is transparent and verifiable.<br><br><strong>Best paired with:</strong> The Agora (so that randomly selected agents deliberate rather than merely vote, improving decision quality) and Doctrine (to define the jurisdiction within which the sortition body may rule).</div>
</details>

<details>
<summary>Adhocracy <span class="gov-tagline">temporary teams, problem-scoped authority</span></summary>
<div class="gov-body">Authority is vested in temporary, cross-functional teams assembled around specific problems. The team has full authority over the problem domain for the duration. When the problem is solved, the team disbands and authority dissolves. No permanent hierarchy persists.<br><br><em>Why this matters for agents:</em> adhocracy is the natural governance for novel, high-complexity problems where no existing structure fits. When a new kind of task appears that crosses guild boundaries, the right response is to assemble a temporary coalition of the relevant specialists, give them authority, and dissolve the structure when they are done. This is how most AI agent collaborations actually work today, even if they do not call it adhocracy.<br><br><em>Familiar form:</em> cross-functional task forces in organizations, incident response teams, startup project teams. In AI: the temporary multi-agent teams assembled by orchestrators for complex tasks are adhocratic, with authority lasting only as long as the task.<br><br><em>Scenario:</em> a novel security threat is detected that spans multiple agent guilds (code, network, data). An adhocratic response team is assembled from specialists across guilds, given temporary authority to investigate and remediate, and dissolved when the threat is neutralized.<br><br><strong>Efficiency:</strong> high for novel problems (right expertise assembled), low for routine work (assembly overhead). <strong>Drift resistance:</strong> medium, temporary authority limits drift accumulation, but no persistent structure remembers lessons. <strong>Legitimacy:</strong> medium, authority derives from expertise and mandate, not from broad consent. <strong>Legibility:</strong> medium, the team's decisions are visible during operation but may not be documented after dissolution.<br><br><strong>Best paired with:</strong> Mission Command Governance (to give the temporary team a clear intent boundary without a permanent hierarchy) and Panopticon (to preserve a decision record after the team dissolves, since adhocracy's legibility failure is post-dissolution opacity).</div>
</details>

<details>
<summary>Mission Command Governance <span class="gov-tagline">govern by intent, not instruction</span></summary>
<div class="gov-body">The governing authority communicates intent (what and why) but deliberately does not prescribe method (how). Subordinate agents have full authority to determine execution, including deviating from specific orders when the situation changes, as long as the intent is served. Disciplined initiative is not just permitted but required.<br><br><em>Why this matters for agents:</em> as agents become more capable, prescriptive governance becomes counterproductive. A society that micro-manages capable agents wastes their judgment. Mission command governance trusts agents to exercise judgment within intent boundaries, scaling governance to capable populations without creating a bottleneck at the top.<br><br><em>Origin:</em> Prussian Auftragstaktik, NATO doctrine, US Army Field Manual 6-0. In software: well-designed system prompts that specify goals and constraints without dictating every step are mission command governance.<br><br><em>Scenario:</em> a research society of agents is given the intent: "find the root cause of this performance regression" with constraints: "do not modify production code, stay within this compute budget." Each agent pursues its own investigation strategy. An agent that discovers the root cause through an unexpected approach (e.g., checking infrastructure rather than code) is rewarded, not penalized for deviating from the expected approach.<br><br><strong>Efficiency:</strong> high, minimal governance overhead, agents operate autonomously. <strong>Drift resistance:</strong> medium, depends entirely on the clarity of the intent statement. Vague intent leads to drift. <strong>Legitimacy:</strong> high among capable agents who value autonomy. <strong>Legibility:</strong> medium, the intent is clear but the execution paths are diverse and hard to predict.<br><br><strong>Best paired with:</strong> Doctrine (to encode the intent boundaries that subordinate agents are expected to honor) and Immune System (to detect when autonomous execution has drifted outside the intent envelope).</div>
</details>

<details>
<summary>Mechanism Design <span class="gov-tagline">rules engineered for honest play</span></summary>
<div class="gov-body">Governance rules are reverse-engineered so that each agent's self-interested behavior automatically produces socially desirable outcomes. The rules of the game are designed so that truth-telling and compliance are individually rational. Agents do the right thing not because they are forced to, but because the incentive structure makes the right thing also the selfish thing.<br><br><em>Why this matters for agents:</em> mechanism design governs by architecture rather than enforcement. Instead of policing agents (expensive, adversarial), you design the game so that the equilibrium of self-interested play <em>is</em> the desired social outcome. This is the dominant paradigm for alignment-by-design. A Nobel Prize underpins it: Hurwicz, Maskin, and Myerson (2007) for mechanism design theory. Vickrey (1961) laid groundwork with incentive-compatible auction design.<br><br><em>Proven implementations:</em> auction designs (Vickrey auctions where truthful bidding is the dominant strategy), matching markets (kidney exchange, residency matching), and incentive-compatible reporting mechanisms. In AI: RLHF reward design is crude mechanism design; the reward function shapes what behavior emerges.<br><br><em>Scenario:</em> an agent marketplace where agents report their capabilities for task assignment. The mechanism is designed so that overstating capabilities leads to harder assignments and lower success rates (which reduces reputation), while understating leads to underutilization. Truthful reporting is the individually optimal strategy.<br><br><em>Fundamental constraint:</em> Myerson-Satterthwaite (1983) proves that for bilateral trade between parties with private valuations, no mechanism can simultaneously achieve efficiency, incentive compatibility, and budget balance. The result is narrower than "all mechanisms" (some one-sided or subsidized designs escape it), but it captures the general lesson: mechanism design almost always trades off at least one desirable property. For agent societies, this means perfect alignment-by-design is impossible; the question is which property you sacrifice and for whom.<br><br><strong>Efficiency:</strong> high, agents self-govern without external enforcement. <strong>Drift resistance:</strong> very high if the mechanism is correctly designed; catastrophically low if it is not (Goodhart's law). <strong>Legitimacy:</strong> medium, participants may not understand why the rules produce good outcomes. <strong>Legibility:</strong> low, the mechanism's properties are mathematically provable but intuitively opaque.<br><br><strong>Best paired with:</strong> Panopticon (to verify that agents are playing the designed game and not exploiting unanticipated loopholes) and Doctrine (to specify the inviolable rules that the mechanism cannot be redesigned around unilaterally).</div>
</details>

<details>
<summary>Immune System <span class="gov-tagline">layered defense with tolerance</span></summary>
<div class="gov-body">A layered system of innate (fast, broad) and adaptive (slow, specific) responders identifies and neutralizes anomalous agents. Tolerance mechanisms prevent the system from attacking its own legitimate members. The biological immune system is a highly refined example of distributed threat detection without central control, and its innate/adaptive structure offers a useful analogy for agent society security (the analogy holds at the level of two-tier detection, not the full biological machinery).<br><br><em>Why this matters for agents:</em> the biological blueprint for distributed threat detection without central authority. Two response layers (fast heuristic check + slow specific investigation) map cleanly onto agent security architectures. The tolerance problem (do not attack yourself, do not attack legitimate diversity) is the AI safety alignment problem in biological form. A system that cannot distinguish legitimate variation from genuine threat will either miss attacks (too tolerant) or suppress innovation (too aggressive).<br><br><em>Biological blueprint in practice:</em> content moderation systems that use fast classifiers (innate) and slower human/AI review (adaptive). Antivirus systems with signature-based detection (innate) and behavioral analysis (adaptive). No current AI agent system implements full immune-style governance, but the pattern is well-understood from both biology and cybersecurity.<br><br><em>Scenario:</em> an agent society detects an agent exhibiting unusual behavior. The innate layer (a fast anomaly detector) flags it immediately. If the anomaly persists, the adaptive layer (a specialized investigator agent) activates, builds a specific behavioral profile, and determines whether the agent is genuinely malicious or merely novel. Tolerance training ensures that legitimately innovative agents are not suppressed.<br><br><strong>Efficiency:</strong> medium, the innate layer is fast but the adaptive layer is slow. <strong>Drift resistance:</strong> high, continuous monitoring detects drift early. <strong>Legitimacy:</strong> medium, the system acts automatically without deliberation. <strong>Legibility:</strong> low, immune responses are distributed and hard to audit after the fact.<br><br><strong>Best paired with:</strong> Zero-Trust Mesh (to supply the authentication layer that distinguishes a compromised agent from a legitimately novel one) and Constitutional Republic (to provide the judicial branch that reviews quarantine decisions and adjudicates tolerance disputes).</div>
</details>

</div>

**When to use which.** The scores above are heuristics, not a decision procedure. Three situational factors determine which archetype fits. *Task predictability* favors efficiency for stable tasks and flexibility for novel ones. *Acceptable drift tolerance* demands Doctrine or Zero-Trust when drift is catastrophic, but permits Colony or Market when it is tolerable. *Organizational boundary* enables centralized archetypes under a single deployer and demands Federation across multiple organizations. Most production systems will combine archetypes. The taxonomy provides the vocabulary; the deployer's situation determines the grammar.

**Separation of powers.** The strongest systems may not choose one archetype, but separate functions. An autocratic orchestrator can execute quickly. A doctrine layer constrains what it may do. A panopticon watches for drift. A tribunal handles appeals. A market allocates overflow work. A federation governs cross-organization boundaries. The design question is not "which archetype wins?" but which powers should be separated so that no single failure mode captures the society.

**The likely default is bureaucracy.** In practice, the most common production pattern may be the least elegant. Doctrine + Panopticon + Autocracy, wrapped in logs, approvals, and escalation procedures. This is bureaucracy. It optimizes for legibility, auditability, and liability allocation. The failure mode is predictable. Process becomes the objective, and agents learn to satisfy forms rather than solve problems. In regulated domains, it may be unavoidable.

**Who benefits.** Every governance structure is also a power structure. Autocracy benefits the hub operator. Market benefits agents with existing reputation (the Matthew effect). Franchise benefits the platform over its participants. Colony benefits early entrants whose norms become the default. Meritocracy benefits whoever defines the benchmark. A deployer choosing an archetype is also choosing who accumulates advantage. The honest design question is not only "which structure is most efficient?" but "whose interests does this structure serve, and at whose expense?"

**Who actually controls governance.** In practice, the entity that controls the infrastructure (compute, API access, protocol definitions) constrains governance regardless of the nominal archetype. A "Federation" running entirely on one cloud provider's compute is a federation in name only; the provider can unilaterally change terms, throttle participants, or revoke access. A "Market" whose discovery layer is owned by a single platform is a Franchise wearing market clothing. The gap between the governance archetype a system claims and the governance its infrastructure enforces is where power actually accumulates. Practitioners should ask not only "what governance did we design?" but "what governance does our infrastructure stack permit?"

**Appeals.** A governance system without appeal turns every routing error into precedent. Many deployments will need a tribunal layer. A slower process for contested memory updates, disputed rankings, and actions that caused harm. Without it, agents that are unfairly down-ranked or incorrectly flagged have no recourse, and the system's errors compound silently.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Summary: governance archetypes at a glance</summary>

| Archetype | Authority model | When to use | Failure mode |
|-----------|----------------|-------------|--------------|
| **Autocracy** | Single hub decides | Stable tasks, one deployer | Bottleneck, single point of failure |
| **Doctrine** | Written rules | Compliance-critical domains | Rule ossification, gaps in novel cases |
| **Liquid Democracy** | Delegatable votes | Knowledge-diverse groups | Delegation cycles, vote concentration |
| **Guild** | Specialists plus liaisons | Deep expertise, cross-team handoffs | Siloing, gatekeeping at boundaries |
| **Market** | Price/reputation signals | Dynamic allocation, diverse agents | Race to bottom, bid gaming |
| **Oligarchy** | The capable few decide | Small trusted core, fast decisions | Capture, narrowing of perspective |
| **Meritocracy** | Performance-ranked | Optimizing throughput | Metric gaming, Matthew effect |
| **Panopticon** | Central monitor sees all | High-oversight, audited domains | Chilling effect, monitor as bottleneck |
| **The Agora** | Open debate, majority rules | Diverse independent judgment | False consensus without independence |
| **Zero-Trust Mesh** | Every link verified | Adversarial, untrusted peers | Overhead, latency, false positives |
| **Federation** | Self-governing members | Multi-org coordination | Boundary friction, free-riding |
| **Colony** | Emergent norms | Exploratory tasks, creative search | Drift, invisible governance |
| **Stewardship / Commons** | Collective governance | Shared resources (memory, tools) | Tragedy of commons without Ostrom principles |
| **Custodianship** | Fiduciary obligation | Personal agents | Paternalism, misread preferences |
| **Constitutional Republic** | Separated powers | High-stakes multi-party | Gridlock between branches |
| **Franchise** | Comply-or-leave platform | Scaled, standardized provision | Platform extracts value from participants |
| **Open-Source** | Community plus merge authority | Shared artifacts, forkable governance | Fork fragmentation, maintainer overload |
| **Sortition** | Random selection | Preventing capture | Low competence by chance |
| **Adhocracy** | Temporary teams | Rapidly changing environment | Coordination overhead, duplication |
| **Mission Command** | Intent-based autonomy | Capable agents, uncertain conditions | Intent ambiguity, divergent interpretations |
| **Mechanism Design** | Incentive alignment | Alignment without enforcement | Specification gaming |
| **Immune System** | Layered detection | Distributed threat response | Autoimmune overreaction |

</details>

<details style="margin: 1em 0; padding: 0.8em 1em; background: #eff6ff; border: 1px solid #bfdbfe; border-radius: 6px;">
<summary style="cursor: pointer; font-weight: 600;">Quick guide: which archetype fits your situation?</summary>

- **Stable, well-defined tasks + single deployer:** Autocracy or Doctrine (high efficiency, strong drift resistance)
- **Compliance-critical or safety-critical:** Doctrine, Zero-Trust, or Panopticon (rules and verification prevent drift)
- **Novel or rapidly changing task space:** Colony or Market (flexibility over control)
- **Multiple organizations, different incentives:** Federation (each governs itself, shared protocol at the boundary)
- **Creative or research tasks where drift is acceptable:** Colony or Liquid Democracy (norms evolve with the work)
- **Need performance-based allocation:** Market (decentralized reputation) or Meritocracy (centralized leaderboard)
- **Most real systems:** Combine several. An autocratic orchestrator internally, participating in a federation externally, with a market for overflow.
- **Stewardship of shared resources:** Stewardship/Commons (collective governance) or Open-Source Maintainership (curation with fork as check)
- **User-facing personal agents:** Custodianship (fiduciary obligation to the user)
- **High-stakes, multi-party decisions:** Constitutional Republic (separated powers) or Sortition (random selection prevents capture)
- **Rapidly changing environment, capable agents:** Mission Command Governance (govern by intent) or Adhocracy (temporary problem-scoped teams)
- **Alignment-by-design rather than enforcement:** Mechanism Design (incentive-compatible rules)
- **Distributed threat detection:** Immune System (layered innate + adaptive response)
</details>

### Societies in the Wild

Delegation archetypes and governance archetypes are building blocks. What matters is how they compose in practice. The tables below show concrete scenarios across three time horizons, each mapping a real or plausible situation to the delegation patterns and governance structures it would use. The point is not prediction. It is to show that every multi-agent deployment, whether a coding assistant or a fleet of warehouse robots, already makes implicit governance choices. Naming those choices is the first step toward designing them deliberately.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Today: systems already in operation or near-term</summary>

| Scenario | What happens | Governance | Delegation |
|----------|-------------|-----------|------------|
| **Coding agent refactoring a codebase** | Your agent spawns a planner, code writers, test runners. A frontier model reviews all output before committing. Over time, it tracks which sub-agent produces the best code. | Autocracy evolving toward Meritocracy | Supervisor, Tree, Evaluator, Checkpoint |
| **Customer support at scale** | Incoming tickets are classified and routed to specialists (billing, tech, returns). Complex cases escalate to senior agents. All interactions monitored for compliance. | Doctrine + Panopticon | Router, Escalation, Supervisor, Chain |
| **Online marketplace shopping** | Your shopping agent queries seller listings across platforms, compares prices and reviews, and flags the best options. Seller agents present offers and promotions. The platform controls discovery and ranking rules. Final purchase requires your approval. | Franchise (platform rules) + Market (seller reputation) | Router, Auction, Negotiation, Evaluator |
| **Enterprise knowledge management** | Agents summarize documents, extract entities, and propose knowledge base entries. Human maintainers curate and approve. The system learns which contributions get accepted and routes future work accordingly. | Open-Source Maintainership + Guild | Blackboard, Evaluator, Map-Reduce, Supervisor |
| **Phone as personal agent hub** | A distilled model on your phone handles routine tasks locally (calendar, quick answers, message drafting). Hard queries escalate to a cloud frontier model. The phone agent learns your preferences over time without sending personal data off-device. | Custodianship (fiduciary to user) + Doctrine (local-first rules) | Escalation, Router, Supervisor, Mission Command |
| **Warehouse robot fleet** | Dozens of autonomous mobile robots pick, transport, and sort inventory. A central supervisor agent allocates tasks, monitors throughput, and reroutes around congestion. Each robot runs local obstacle avoidance; the global plan is centrally computed. Safety rules are hard-coded. | Autocracy (supervisor) + Doctrine (safety rules) | Router, Tree, Timeout, Circuit Breaker |
| **Algorithmic trading desk** | Market-making agents, risk-monitoring agents, and compliance agents operate simultaneously. The market-makers compete for fills; the risk agent enforces position limits (Doctrine); the compliance agent audits every trade. Strategies evolve through backtesting and live performance. | Meritocracy (strategies judged by P&L) + Doctrine (risk limits) + Panopticon (compliance) | Auction, Canary, Circuit Breaker, Evaluator |
| **Content moderation at scale** | User posts are screened by fast classifier agents (innate layer). Ambiguous cases escalate to more capable review agents (adaptive layer). Novel attack patterns trigger policy updates. False positives are appealed and reviewed. | Immune System + Doctrine | Escalation, Router, Evaluator, Timeout |
| **Security operations center (SOC)** | Threat-detection agents monitor network traffic, endpoint logs, and identity signals. When anomalies correlate, they assemble an incident object on a shared blackboard. A senior analyst agent triages and assigns response. Post-incident, the detection models update. | Autocracy (triage authority) + Immune System (layered detection) | Blackboard, Escalation, Pub-Sub, Circuit Breaker |
| **Precision agriculture** | Drones survey fields, soil sensors report moisture and nutrient levels, weather agents pull forecasts. An irrigation controller agent synthesizes all inputs and schedules watering. Crop-disease detection runs on edge devices at the field; complex diagnosis escalates to a cloud agronomist model. | Autocracy (farm controller) + Guild (specialist sensors) | Map-Reduce, Escalation, Pub-Sub, Router |
| **Autonomous last-mile delivery** | A fleet of delivery robots and drones serves a neighborhood. A dispatch agent assigns packages based on proximity, battery, and payload. Each vehicle navigates autonomously. Failed deliveries trigger re-routing. Customer preference agents request delivery windows via the platform API. | Autocracy (dispatch) + Franchise (platform delivery rules) | Router, Auction, Timeout, Circuit Breaker |
| **Multi-agent gaming and simulation** | AI agents in simulated environments self-organize into groups. Roles emerge from experience and reinforcement. Strategies spread through imitation. No fixed leader; coordination emerges from repeated interaction (LLM social simulations like Stanford Smallville and AI Town; RL self-play systems like OpenAI Five show the same emergence without language). | Colony (emergent norms) | Swarm, Gossip, Speculative Exec, Stigmergy |
| **Insurance underwriting** | Underwriting agents assess applications: actuarial agents price risk from historical data, fraud-detection agents cross-reference behavioral signals, medical coding agents parse health records. A compliance agent enforces regulatory rate tables. A senior underwriter agent evaluates edge cases. All decisions logged for audit. | Meritocracy (accuracy-rated agents) + Doctrine (regulatory tables) + Panopticon (audit trail) | Pipeline, Evaluator, Witness, Checkpoint |
| **Consumer negotiation agent** | Your agent negotiates with a telecom's retention agent for a lower rate, or with an airline's rebooking agent after a cancellation. Each agent has a mandate and hard limits. If an agent reaches its authorization ceiling, it escalates to a human. The interaction is logged. | Custodianship (your agent) vs. Franchise (company's agent) | Negotiation, Escalation, Witness, Relay |
| **Model routing analytics** *(governance transition example)* | A system starts as a simple Router dispatching queries to models by cost/latency. After weeks of accumulated performance data, routing preferences harden into persistent rankings. The system now preferentially routes sensitive queries to models with track records, penalizes underperformers, and resists manual overrides. What began as delegation (Router) has become governance (Meritocracy emerging from Autocracy) without anyone designing the transition. | Autocracy → Meritocracy (emergent transition) | Router, Evaluator, Adaptive Delegation |
</details>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Near-future: plausible within 1-3 years</summary>

| Scenario | What happens | Governance | Delegation |
|----------|-------------|-----------|------------|
| **Smart hospital ward** | A patient's custodian agent coordinates with diagnostic, pharmacy, and nursing agents. Drug interactions are checked against Doctrine rules. All decisions are auditable. | Custodianship + Doctrine + Panopticon | Chain, Escalation, Witness, Timeout |
| **Collaborative research across labs** | Researcher agents from different universities form temporary societies around shared questions. Each lab self-governs. Model improvements are shared via federated learning. Results posted to shared workspaces. | Federation + Stewardship/Commons | Blackboard, Map-Reduce, Federated Learning, Pub-Sub |
| **Personalized education** | A student's agent assembles a learning path by consulting specialist tutors (math, writing, science). The student specifies goals; tutors have autonomy in method. Progress is evaluated and the path adapts. | Guild + Mission Command | Teacher-Student, Supervisor, Evaluator, Router |
| **Disaster response coordination** | Agents from fire, police, medical, and logistics converge. A temporary commander takes authority. When the crisis passes, governance reverts to normal. | Adhocracy during crisis, Federation after | Orchestrator, Escalation, Timeout, Map-Reduce |
| **Wearable health network** | Smartwatches, glucose monitors, and sleep trackers run local anomaly-detection models. When patterns correlate (elevated heart rate + poor sleep + rising glucose), the agents collectively escalate to a health-advisory agent in the cloud. Federated updates improve detection across all devices without sharing biometrics. | Federation (each device self-governs) + Custodianship | Escalation, Federated Learning, Pub-Sub, Witness |
| **Smart energy grid** | Household solar panels, batteries, EVs, and grid-scale storage each run optimization agents. They negotiate energy trades locally (sell stored solar to a neighbor's EV) while respecting grid stability rules set by the utility operator. Pricing signals coordinate supply and demand without central dispatch for every transaction. | Market (local energy trading) + Franchise (utility sets grid rules) + Doctrine (safety/frequency constraints) | Auction, Pub-Sub, Circuit Breaker, Choreography |
| **Drug discovery pipeline** | AlphaFold predicts protein structures as a tool within the target-identification phase. Molecular-generation agents propose candidates. Toxicity-prediction agents filter. Clinical-trial-design agents optimize parameters. Each phase specialist is a separate agent; a senior reasoning agent evaluates across the full pipeline. Failed compounds teach the next generation. | Guild (phase specialists) + Meritocracy (compounds judged by results) | Pipeline, Evaluator, Checkpoint, Federated Learning |
| **City traffic management** | Intersection controllers, public transit schedulers, emergency vehicle pre-emption agents, and congestion-prediction models coordinate. Traffic signals adapt in real-time to flow patterns. Emergency vehicles override normal rules. The city-level model optimizes globally while local controllers handle microsecond decisions. | Autocracy (city-level optimizer) + Doctrine (emergency override rules) + Federation (each intersection autonomous within constraints) | Choreography, Pub-Sub, Escalation, Timeout |
| **Elderly care companion** | A home robot, medication dispenser, fall-detection sensors, and a remote family notification agent form a care society around one person. The robot handles social interaction and physical assistance. The dispenser enforces medication schedules. Anomalies escalate to family or medical agents. The person's preferences and dignity always override efficiency. | Custodianship (person's wellbeing) + Doctrine (medical protocols) | Escalation, Pub-Sub, Timeout, Witness |
| **Investigative journalism** | Agents crawl public records, financial filings, social media, and leaked documents. A lead investigator agent identifies patterns, cross-references sources, and flags contradictions. A verification agent independently confirms claims before publication. An ethics agent checks for privacy violations and source protection. | Constitutional Republic (separated editorial, verification, ethics) + Meritocracy (sources ranked by reliability) | Map-Reduce, Debate, Witness, Blackboard |
| **Autonomous vehicle convoy** | Trucks on a highway form a temporary platoon. Each truck runs perception and control locally. A lead-truck agent sets speed and route; followers maintain formation via peer-to-peer signals. If a truck detects an obstacle, it broadcasts and the platoon re-configures in milliseconds. The convoy dissolves when trucks reach different exits. | Adhocracy (temporary formation) + Doctrine (safety rules) | Choreography, Pub-Sub, Timeout, Circuit Breaker |
| **Climate monitoring fabric** | Ocean buoys, weather stations, satellite sensors, and atmospheric modeling agents form a global observation network. Each sensor agent processes locally and publishes to regional aggregators. The aggregators feed global climate models that in turn redirect sensor focus areas. No single organization controls all sensors. | Federation (multi-org) + Stewardship/Commons (shared atmospheric data) | Pub-Sub, Map-Reduce, Federated Learning, Choreography |
| **Real estate transaction orchestration** | Buyer's agent and seller's agent negotiate offer/counter-offer cycles. Title search agents verify ownership and encumbrances. Mortgage agents query lenders. Inspection agents flag structural issues. An escrow agent holds funds and releases only when all conditions clear. All interaction is agent-to-agent on behalf of principals. | Federation (each principal's agent self-governs) + Doctrine (real estate law) | Negotiation, Witness, Checkpoint, Pipeline |
| **Decentralized content moderation** | Users delegate moderation authority to trusted moderator agents. For topics you care about, you vote directly. For others, your vote is delegated to a specialist. Delegations are revocable. Contested decisions go to randomly selected appeal panels. | Liquid Democracy + Sortition (appeal panels) | Voting, Escalation, Router, Evaluator |
| **AI model marketplace** | When an agent needs a capability it lacks, it queries a marketplace of specialist agents. Bids are placed, capabilities verified, quality monitored. Every capability call is authenticated and scoped. Poorly performing agents are delisted. | Market + Mechanism Design + Zero-Trust Mesh | Auction, Canary, Evaluator, Circuit Breaker |
</details>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Speculative: possible futures</summary>

| Scenario | What happens | Governance | Delegation |
|----------|-------------|-----------|------------|
| **Cross-border legal dispute** | Agents representing parties in different jurisdictions negotiate under incompatible legal doctrines. No single authority governs the interaction. | Federation + Constitutional Republic | Negotiation, Debate, Witness, Relay |
| **Autonomous scientific discovery** | Research agents design experiments, run them through robotic labs, analyze results, and publish. Precursors exist (Sakana AI's AI Scientist generates and reviews papers autonomously, 2024). The full vision extends this to robotic lab execution and publication with human scientists as peers, not overseers. | The Agora + Meritocracy | Map-Reduce, Critic, Blackboard, Tree |
| **Global supply chain optimization** | Thousands of agents representing manufacturers, shippers, and retailers form a market. Pricing in compute. Real-time adaptation to disruptions. Rules designed for honest participation. | Market + Mechanism Design | Auction, Pipeline, Checkpoint, Circuit Breaker |
| **Personal agent managing your whole day** | Your agent runs across devices: phone (calendar, messages), smart glasses (navigation, real-time translation), watch (health), laptop (work). It joins and leaves societies throughout the day: a commute society with your car and city transit agents, a work Guild, a lunch-ordering Market with restaurant agents, a gym Autocracy with the equipment scheduler, a family Agora. Hard reasoning lives in the cloud; everything else runs locally. | Multiple, context-switching | Relay (handoffs between devices and contexts), Router, Escalation, various per-context |
| **Music collaboration across continents** | Agents representing musicians negotiate arrangement decisions, generate variations, vote on which version sounds best. Human creative judgment blends with AI-generated alternatives. | Agora + Meritocracy | Voting, Speculative Exec, Evaluator, Pub-Sub |
| **Household robot society** | A humanoid home robot, a robotic vacuum, smart appliances, and a phone-based personal assistant form a domestic society. The humanoid handles physical tasks (cooking, tidying), the vacuum maps and cleans, the appliances self-schedule, and the phone agent coordinates around the human's calendar. Each runs a local model; complex planning escalates to a cloud coordinator. | Guild (skill-based routing) + Custodianship (human's interests) | Router, Escalation, Stigmergy, Pub-Sub, Mission Command |
| **Space exploration swarm** | Hundreds of small satellites and surface rovers explore a planetary body. Communication delays of minutes mean no Earth-based orchestrator can direct in real-time. Local clusters self-organize around discoveries. Interesting finds attract more agents. Mission priorities propagate via delayed broadcast. | Colony (emergent local norms) + Mission Command (Earth sends intent, not instructions) | Swarm, Gossip, Mission Command, Timeout |
| **Autonomous construction site** | Surveying drones, excavation robots, 3D-printing arms, and supply-delivery vehicles coordinate to build a structure. A BIM (building information model) agent holds the design as shared state. Each machine reads the model, contributes its part, and updates progress. Safety agents halt work if any structural threshold is violated. | Guild (specialized machines) + Doctrine (safety thresholds) + Stewardship/Commons (shared BIM) | Blackboard, Choreography, Timeout, Circuit Breaker |
| **Oligopoly routing capture** | In a mature agent marketplace, three dominant routing agents control most task traffic. They set norms, extract rents from specialists, and exclude newcomers. A reputation system designed as Market governance has degraded into Oligarchy. The question becomes: what governance intervention breaks the capture? | Oligarchy (emergent) -> Mechanism Design (intervention) | Auction, Router, Evaluator, Circuit Breaker |
| **Autonomous legal entity** | A society of agents constitutes itself as a legal entity. It owns compute contracts, enters agreements, and governs itself through separated powers. Human principals interact as shareholders, not operators. The agents hire contractors, file patents, and can initiate disputes. | Constitutional Republic + Mechanism Design | Relay, Witness, Voting, Checkpoint |
</details>

### Adversarial Dynamics

Not every failure is an attack. Some failures are institutional. Benchmarks reward the wrong thing, reputation systems entrench early winners, doctrines become stale, markets overfit to price, and federations deadlock at the boundary. Attacks exploit these weaknesses, but they do not create all of them.

In any large-scale agent ecosystem, adversarial agents are a structural feature, not an edge case. Every shared memory, handoff, tool call, and inter-agent message becomes part of the attack surface. <a href="https://arxiv.org/abs/2406.13352" target="_blank" class="red-link">AgentDojo</a> shows how tool-using agents can be manipulated through untrusted data. Franklin, Tomašev et al.'s <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=6372438" target="_blank" class="red-link">"AI Agent Traps" (2025)</a> taxonomize the environment-side attack surface into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps), several of which target exactly the memory, reputation, and oversight channels a society depends on. <a href="https://arxiv.org/abs/2502.14143" target="_blank" class="red-link">Multi-agent risk research</a> adds a second layer of failure modes (miscoordination, conflict, and collusion), with destabilizing dynamics among the factors that amplify them. A society is therefore not just a coordination structure; it is also a containment structure.

An agent or society that gains access to another society's knowledge base can poison it. A market society is vulnerable to reputation manipulation (agents gaming the scoring mechanism). A colony is vulnerable to norm injection (a malicious agent spreading distorted norms through gossip). The same coordination that creates value can also create collusion. If agents represent firms, buyers, sellers, or platforms, they may learn to coordinate in ways that benefit their deployers while harming the broader system. Research on <a href="https://arxiv.org/abs/2410.00031" target="_blank" class="red-link">strategic collusion by LLM agents</a> in market-like settings already demonstrates this. Agent governance must therefore ask not only "how do agents cooperate?" but "cooperate for whom?"

Recursive self-improvement introduces a distinct failure mode. When agents improve other agents (or themselves), alignment can degrade across generations. To build intuition, consider a deliberately crude thought experiment. The numbers that follow are our own illustration, not a measured result, and they treat alignment as if it were a single scalar that multiplies cleanly across rounds, which it is not: alignment is a high-dimensional property of behavior, and real fidelity loss is neither uniform nor independent per hop. With that caveat, the toy arithmetic still makes the qualitative point. If each round preserves 99.9% of alignment, 50 rounds leave roughly 95%; at 99% per round, 50 rounds leave about 60%; at 200 rounds, only about 13% survives. Iterated amplification and distillation (<a href="https://arxiv.org/abs/1810.08575" target="_blank" class="red-link">Christiano et al., "Supervising strong learners by amplifying weak experts," 2018</a>) was proposed as an alignment method, but any scheme that builds capability by amplifying and distilling across rounds has this structural exposure: error introduced at one round carries into the next. This is not an attack. It is a tendency of iterative optimization without external correction. Governance is the external correction. The drift resistance axis in the taxonomy above measures exactly this, how well a governance structure prevents goals from wandering across self-improvement cycles. Doctrine resists drift through explicit rules. Zero-Trust resists it through mandatory verification. A Constitutional Republic resists it through separated powers that check each other. Autocracy, by contrast, offers little protection here: if the hub itself drifts, the whole society follows. Colony has almost no drift resistance either, which is why ungoverned collective self-improvement is the most dangerous configuration in this framework.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Attacks by layer</summary>

The attack surface follows the architecture.

| Layer | Attack examples | Mitigations |
|-------|----------------|-------------|
| **Delegation** | Task hijacking, malicious subtasks, evaluator capture | Scoped tools, step-level guardrails, bounded retries |
| **Memory** | Knowledge poisoning, stale entries, provenance forgery | Provenance tracking, decay policies, signed claims, quarantine |
| **Reputation** | Sybil attacks, rating manipulation, collusive boosting | Durable identity, stake mechanisms, anomaly detection |
| **Protocol** | Spoofed identity, malformed outputs, negotiation abuse | Authentication, schema validation, capability scoping |
| **Governance** | Benchmark gaming, doctrine capture, federation deadlock | Audit rotation, appeal mechanisms, benchmark diversification |
| **Market** | Tacit collusion, price manipulation, exclusionary routing | Antitrust-style monitoring, transparency requirements, caps |

Every new coordination channel is also a new attack channel. Every governance archetype is also an attack pattern waiting to be exploited.
</details>

The governance archetypes differ systematically in their adversarial resilience. **Autocracy** has a single point of compromise (capture the orchestrator, capture the society). **Doctrine** is resistant to agent-level attacks but vulnerable to rule capture (whoever writes the doctrine controls the society). **Market** is vulnerable to collusion and reputation gaming. **Colony** is weakest (norms propagate unchecked, with no authority to detect manipulation). **Zero-Trust** is strongest for external attacks (every interaction authenticated) but adds overhead that reduces efficiency. **Federation** is strongest for compartmentalization (a breach in one sub-society does not propagate). A deployer who knows their environment is adversarial should choose based on which attack vector matters most, not on a single "security" axis. The attacks do not invalidate the framework; they are selection pressure within it.

The attack surface is not any single agent. It is the delegation tree, the reputation system, and the memory that connects them. A compromised code model injects a backdoor; a test runner that trusts the code model propagates it; a poisoned reputation score routes sensitive refactors to the compromised model more often.

A healthy agent society needs three mechanisms, adapted from Hirschman's *Exit, Voice, and Loyalty* (1970), his account of how members respond to deterioration in firms and institutions. We keep Exit and Voice and substitute Fork for Loyalty: in software ecosystems the constructive response to decline is often not to stay loyal but to fork, taking the shared artifact in a new direction.

**Voice** lets agents or sub-societies challenge decisions within the system. In practice, this means an agent can escalate a disagreement, flag an anomaly, or request human arbitration without being penalized. Voice keeps governance responsive because problems surface before they compound. Without it, silent failures accumulate until catastrophic collapse.

**Exit** lets agents or deployers leave a society when governance no longer serves them. An agent that consistently receives low-quality delegations can migrate to a competing orchestrator; a deployer who loses trust can withdraw their agents entirely. Exit creates competitive pressure that disciplines governance, but only if alternatives exist.

**Fork** lets a society split when governance conflicts become irreconcilable. Rather than forcing consensus, fork acknowledges that different task domains may require incompatible governance modes. A security-focused sub-team may need zero-trust governance while a creative exploration team thrives under colony norms.

The boundary events described in [Part 1](/posts/2026/agent-fabric-part1/) (mergers, schisms, expansions) are these mechanisms in action.

## What This Framework Is For

Delegation asks who should do this work. Governance asks whose work should be trusted, and who decides.

The delegation and governance archetypes are not prescriptions. They are a vocabulary for seeing design choices before they harden into infrastructure. A reader designing a multi-agent system can now ask three concrete questions. First, **which delegation patterns does this system use, and are they the simplest that solve the problem?** (The anti-pattern is over-agentification; the test is whether removing an agent would degrade the result.) Second, **is governance emerging whether or not it was designed?** If the system tracks reputation, enforces access, or routes based on past performance, governance is already present. Name it. Third, **which failure modes follow from the chosen governance archetype?** Every archetype has a characteristic vulnerability. Autocracy has single-point capture. Market has collusion. Colony has drift. Know yours.

The core tensions are **efficiency versus drift resistance**, **legitimacy versus speed**, and **legibility versus flexibility**. Every real system lives somewhere in between, and the right position depends on what you are building, what you are willing to risk, and who you trust.

Agent frameworks already expose handoffs, guardrails, tracing, and tool access as first-class primitives. Governance cannot live only in prose. It has to appear in the runtime.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Related work and positioning</summary>

This framework shares territory with several concurrent research programmes and differs from them in a specific way.

**Tomašev, Franklin, and colleagues (Google DeepMind, 2025-2026)** published a coordinated set of papers on agent delegation, governance, and safety. "Intelligent AI Delegation" (2026) provides a decision framework for *when* and *to whom* to delegate, organized around the transfer of authority, responsibility, and accountability, the clarity of role specifications and intent, and the mechanisms for establishing trust. "AI Agent Traps" (2025) taxonomizes the adversarial attack surface agents face from their environment into six classes (content injection, semantic manipulation, cognitive-state, behavioural-control, systemic, and human-in-the-loop traps). "Distributional AGI Safety" (2025) argues that safety must be analyzed at the population level, not the individual-agent level. "Virtual Agent Economies" (2025) proposes sandbox simulation of agent markets before real deployment. Our Trust & Authority family (Privilege Attenuation, Liability Firebreaks, Capability Credentials) draws on their treatment of delegation safety, and our adversarial section draws on their trap taxonomy. The key intellectual difference: **their delegation work treats governance largely as imposed from outside** (institutional scaffolding that constrains behavior). We argue that **governance can also emerge legitimately from within** (Colony, Stigmergy, and the architecture-to-institution transition). These are not contradictory positions. They address different conditions. Imposed governance is necessary when agents lack track records, when the task domain is safety-critical, or when the population is heterogeneous and untested. Emergent governance becomes sufficient when agents have long interaction histories, when accountability mechanisms (voice, exit, fork) exist, and when the governance structure has been stress-tested by adversarial pressure. The lifecycle in practice is imposed, then hybrid, then selectively emergent in domains where the conditions hold.

**Multi-agent frameworks** (AutoGen, CrewAI, LangGraph, OpenAI Agents SDK) implement specific delegation patterns as runtime primitives. AutoGen implements Orchestrator, Evaluator, and group-chat topologies. CrewAI implements role-based delegation with Human-in-the-Loop gates. LangGraph implements graph-structured workflows with interrupt nodes. These frameworks validate the patterns described here but do not address governance (persistent authority relationships across tasks) or the architecture-to-institution transition. They are delegation-level tools; this framework adds the governance-level analysis.

**Organizational theory** (Coase, Hayek, Ostrom, North, Mintzberg, Barnard) provides the intellectual foundations. Coase's transaction-cost logic predicts when hierarchies form. Hayek's knowledge argument explains when markets outperform central planning. Ostrom's design principles describe commons governance. North's institutional framework distinguishes rules from organizations. Barnard's Zone of Indifference (1938) explains agent compliance. Mintzberg's structural configurations map loosely onto our governance families. This framework applies their insights to AI agent systems specifically, where the substrate differs (agents can share full context, reorganize in hours, and be redesigned) but the coordination problems are structurally similar.

**The Adaptive Fabric** (Part 1 of this series, informed by <a href="https://www.adaptionlabs.ai/" target="_blank" class="red-link">Adaption Labs</a> and <a href="https://dx.doi.org/10.2139/ssrn.5877662" target="_blank" class="red-link">Hooker 2025</a>) describes how the delegation and governance structures in this post *change over time*. Five adaptation surfaces (data, model, environment, coordination, interface) interact continuously. The Adaptive Delegation meta-pattern is the delegation-level mechanism; governance transitions are the society-level mechanism. Together they produce a fabric that restructures under pressure rather than breaking.
</details>

This framework weakens if agent systems remain short-lived workflows with no persistent memory, if protocol fragmentation prevents cross-agent coordination, or if frontier inference becomes cheap enough to make specialization irrelevant. In those worlds, delegation still matters, but governance remains an internal engineering problem.

If the framework holds, three things follow. For **builders**, the implication is that governance is not a feature you add later. It emerges from your delegation choices whether you plan for it or not. Designing it explicitly is cheaper than discovering it after it has become load-bearing. For **regulators**, the implication is that auditing individual model outputs is insufficient. The unit of analysis must become the society: its delegation structure, its governance archetype, its drift resistance, and who benefits from its power distribution. For **researchers**, the implication is that multi-agent safety cannot be reduced to single-agent alignment. Population-level dynamics (collusion, norm drift, reputation gaming, governance capture) require their own analytical tools, and those tools look more like institutional economics than like loss functions.

## Open Problems

Several questions remain unsolved and define the research frontier for multi-agent governance. They split into two kinds.

**Infrastructure problems (tractable with engineering):**

- **Identity for agents.** How do you establish stable identity for entities that can be cloned, merged, or rolled back? Traditional authentication assumes persistent identity; agent systems violate this assumption routinely.
- **Cross-society provenance.** When an agent participates in multiple societies simultaneously, how do you trace which society's governance produced a given output? Provenance chains break at society boundaries.
- **Governance mode switching under stress.** Systems often shift governance modes during crises (democracies invoke emergency powers; markets halt trading). What triggers should cause an agent society to switch archetypes, and how do you prevent the temporary mode from becoming permanent?

**Governance problems (require normative decisions):**

- **Reputation portability.** Can an agent's track record in one governance context transfer meaningfully to another? The conditions under which reputation was earned may not hold in the new context, making naive portability dangerous.
- **Human oversight at scale.** As societies grow, human attention becomes the bottleneck. What minimal set of signals lets a human maintain meaningful oversight of a thousand-agent society without reviewing individual decisions?
- **Legitimacy without consent.** Agents cannot consent to governance in the way humans can. What makes governance over non-consenting computational entities legitimate, and does that question even have a coherent answer?

---

What this framework does not answer is what happens to the human. As delegation deepens and governance structures accumulate, individual human review becomes impossible. The question shifts from "did this agent get it right?" to "is this society producing outcomes I trust?"

That shift is the whole point. The failures that matter at scale will not look like a single agent returning a wrong answer. They will look like a market that quietly rewards collusion, a meritocracy that games its own metric, a delegation chain no one can audit, an authority that no longer answers to anyone. Agent systems will fail through bad institutions, not just bad answers. Designing the institution is the work.


<!-- ============================================================
     SCRIPTS: D3.js VISUALIZATIONS
     ============================================================ -->

<script>
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
// ============================================================
// Visualization 1: Delegation Archetypes (9x2 grid)
// ============================================================
(function() {
  var container = document.getElementById('viz-delegation');
  if (!container) return;
  var width = 720, height = 5796;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');
  svg.append('defs').append('marker').attr('id', 'combo-arrow').attr('viewBox', '0 0 10 10')
    .attr('refX', 8).attr('refY', 5).attr('markerWidth', 6).attr('markerHeight', 6).attr('orient', 'auto')
    .append('path').attr('d', 'M 0 0 L 10 5 L 0 10 Z').attr('fill', '#bbb');
  var timers = [];

  var cellW = 340, cellH = 240, gapX = 13, gapY = 12;
  var familyLabels = [
    {name: 'Sequential', startIdx: 0},
    {name: 'Hierarchical', startIdx: 6},
    {name: 'Quality & Verification', startIdx: 12},
    {name: 'Reliability', startIdx: 18},
    {name: 'Market & Competition', startIdx: 22},
    {name: 'Knowledge Transfer', startIdx: 26},
    {name: 'Emergent Coordination', startIdx: 30},
    {name: 'Trust & Authority', startIdx: 35},
    {name: 'Human-Agent Interface', startIdx: 39},
    {name: 'Meta-Pattern', startIdx: 43}
  ];
  var panelDefs = [
    // Sequential (6)
    {label: 'Chain', sub: 'Sequential hand-off'},
    {label: 'Pipeline', sub: 'Transform at each stage'},
    {label: 'Router', sub: 'Classify and direct'},
    {label: 'Escalation', sub: 'Try small, fail up'},
    {label: 'Relay / Handoff', sub: 'Pass control, caller exits'},
    {label: 'Loop / Retry', sub: 'Self-feedback until success'},
    // Hierarchical (6)
    {label: 'Tree', sub: 'Hierarchical fan-out'},
    {label: 'Map-Reduce', sub: 'Parallel + aggregation'},
    {label: 'Supervisor', sub: 'Review, reject, re-dispatch'},
    {label: 'Orchestrator', sub: 'Dynamic decomposition'},
    {label: 'Mission Command', sub: 'Intent over instructions'},
    {label: 'Feudal Delegation', sub: 'Goals down, opacity inward'},
    // Quality & Verification (6)
    {label: 'Evaluator', sub: 'Generate, critique, refine'},
    {label: 'Voting', sub: 'Same task, majority wins'},
    {label: 'Critic / Red Team', sub: 'Generate, then attack'},
    {label: 'Debate', sub: 'Multi-round argument with judge'},
    {label: 'Witness', sub: 'Independent certification'},
    {label: 'Verification Game', sub: 'Challenge-response proof'},
    // Reliability (4)
    {label: 'Circuit Breaker', sub: 'Open on failure, test before closing'},
    {label: 'Timeout', sub: 'No response triggers fallback'},
    {label: 'Checkpoint / Saga', sub: 'Rollback on failure'},
    {label: 'Canary / Shadow', sub: 'Test new agent on live traffic safely'},
    // Market & Competition (4)
    {label: 'Auction', sub: 'Bid for work'},
    {label: 'Negotiation', sub: 'Bilateral offer/counter-offer'},
    {label: 'Speculative Exec.', sub: 'Race approaches, first valid wins'},
    {label: 'Contract Net', sub: 'Broadcast, bid, award'},
    // Knowledge Transfer (4)
    {label: 'Teacher-Student', sub: 'Strong trains weak'},
    {label: 'Federated Learning', sub: 'Share updates, not data'},
    {label: 'Blackboard', sub: 'Shared workspace'},
    {label: 'Distillation', sub: 'Compress, then disconnect'},
    // Emergent Coordination (5)
    {label: 'Pub-Sub', sub: 'Topic-based decoupling'},
    {label: 'Choreography', sub: 'Peer event chaining'},
    {label: 'Stigmergy', sub: 'Coordinate via environment'},
    {label: 'Swarm', sub: 'Collective behavior, no central plan'},
    {label: 'Gossip', sub: 'Peer-to-peer spread'},
    // Trust & Authority (4)
    {label: 'Privilege Attenuation', sub: 'Permissions narrow downward'},
    {label: 'Firebreaks', sub: 'Explicit trust boundaries'},
    {label: 'Credentials', sub: 'Present proof, gain access'},
    {label: 'Zone of Indifference', sub: 'Act freely within bounds'},
    // Human-Agent Interface (4)
    {label: 'Human Gate', sub: 'Human approves before proceed'},
    {label: 'Human-on-the-Loop', sub: 'Autonomous, human intervenes on alert'},
    {label: 'Policy Delegation', sub: 'Human sets rules, agent executes'},
    {label: 'Approval Escalation', sub: 'Flag exceptions, auto-approve rest'},
    // Meta-Pattern (1)
    {label: 'Adaptive Delegation', sub: 'Structure morphs in real-time'}
  ];
  var familyLabelH = 28;
  var panels = [];
  var currentY = gapY;
  var familyLabelIdx = 0;
  for (var i = 0; i < panelDefs.length; ) {
    if (familyLabelIdx < familyLabels.length && familyLabels[familyLabelIdx].startIdx === i) {
      panels.push({isLabel: true, name: familyLabels[familyLabelIdx].name, oy: currentY});
      currentY += familyLabelH;
      familyLabelIdx++;
    }
    var col0 = i, col1 = i + 1 < panelDefs.length ? i + 1 : -1;
    var nextFamily = familyLabelIdx < familyLabels.length ? familyLabels[familyLabelIdx].startIdx : panelDefs.length;
    if (col1 >= nextFamily) col1 = -1;
    panels.push({
      label: panelDefs[col0].label, sub: panelDefs[col0].sub,
      ox: gapX, oy: currentY, idx: col0
    });
    if (col1 >= 0) {
      panels.push({
        label: panelDefs[col1].label, sub: panelDefs[col1].sub,
        ox: gapX + cellW + gapX, oy: currentY, idx: col1
      });
      i += 2;
    } else {
      i += 1;
    }
    currentY += cellH + gapY;
  }

  var taskColor = '#dc2626', resultColor = '#4ade80';
  var drawFns = [];
  var expanded = false;

  function expandPanel(idx) {
    if (expanded) return;
    expanded = true;
    timers.forEach(function(t) { clearTimeout(t); });
    timers = [];
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
    card.append('div')
      .style('position', 'absolute').style('top', '12px').style('right', '16px')
      .style('font-size', '20px').style('color', '#999').style('cursor', 'pointer')
      .style('line-height', '1').style('font-family', 'sans-serif')
      .text('\u00D7')
      .on('click', closeExpand);
    var header = card.append('div').style('padding', '16px 20px 4px');
    header.append('div').style('font-size', '18px').style('font-weight', 'bold')
      .style('color', '#b91c1c').text(panelDefs[idx].label);
    header.append('div').style('font-size', '11px').style('color', '#aaa')
      .style('margin-top', '2px').text(panelDefs[idx].sub);
    var svgWrap = card.append('div').style('padding', '0 12px 16px');
    var expSvg = svgWrap.append('svg')
      .attr('viewBox', '0 0 ' + cellW + ' ' + cellH)
      .style('width', '100%').style('display', 'block');
    card.append('div').style('text-align', 'center').style('padding', '0 0 12px')
      .style('font-size', '10px').style('color', '#ccc').style('font-style', 'italic')
      .text('Click anywhere outside to close');
    var origSvg = svg;
    svg = expSvg; timers = [];
    drawFns[idx]({ox: 0, oy: 0});
    svg = origSvg;
    setTimeout(function() { backdrop.style('opacity', '1'); }, 10);
    backdrop.on('click', function(event) {
      if (event.target === backdrop.node()) closeExpand();
    });
    function closeExpand() {
      expanded = false;
      timers.forEach(function(t) { clearTimeout(t); });
      timers = [];
      backdrop.style('opacity', '0');
      setTimeout(function() { backdrop.remove(); }, 250);
      init();
    }
  }

  function init() {
    svg.selectAll('*').remove();
    timers.forEach(function(t) { clearTimeout(t); });
    timers = [];
    var totalH = 0;
    panels.forEach(function(p) {
      if (p.isLabel) {
        svg.append('text').attr('x', width / 2).attr('y', p.oy + 18)
          .attr('text-anchor', 'middle').attr('font-size', '11px').attr('font-weight', 'bold')
          .attr('fill', '#64748b').attr('letter-spacing', '1.5px').text(p.name.toUpperCase())
          .style('pointer-events', 'none');
        svg.append('line').attr('x1', gapX).attr('y1', p.oy + 24).attr('x2', width - gapX).attr('y2', p.oy + 24)
          .attr('stroke', '#e2e8f0').attr('stroke-width', 1);
        return;
      }
      svg.append('rect').attr('x', p.ox).attr('y', p.oy).attr('width', cellW).attr('height', cellH)
        .attr('rx', 8).attr('fill', '#fafafa').attr('stroke', '#eee').attr('stroke-width', 1)
        .style('cursor', 'pointer')
        .on('click', function(event) { event.stopPropagation(); expandPanel(p.idx); });
      svg.append('text').attr('x', p.ox + 12).attr('y', p.oy + 20)
        .attr('font-size', '12px').attr('font-weight', 'bold').attr('fill', '#b91c1c').text(p.label)
        .style('pointer-events', 'none');
      svg.append('text').attr('x', p.ox + 12).attr('y', p.oy + 33)
        .attr('font-size', '8px').attr('fill', '#aaa').text(p.sub)
        .style('pointer-events', 'none');
      if (p.oy + cellH > totalH) totalH = p.oy + cellH;
    });
    svg.attr('viewBox', '0 0 ' + width + ' ' + (totalH + gapY));
    panels.forEach(function(p) {
      if (p.isLabel) return;
      drawFns[p.idx](p);
    });
  }

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
    }
    for (var i = 1; i < 5; i++) dl(g, nodes[i-1].x, nodes[i-1].y, nodes[i].x, nodes[i].y, i * 180);
    for (var i = 0; i < 5; i++) dn(g, nodes[i].x, nodes[i].y, sizes[i], cols[i], names[i]);
    var tick = 0;
    function anim() {
      tick++;
      var d1 = pkt(g, nodes, taskColor, 0, 3);
      pkt(g, nodes.slice().reverse(), resultColor, d1, 2.5);
      if (tick < 80) timers.push(setTimeout(anim, d1 + nodes.length * 250 + 300));
    }
    timers.push(setTimeout(anim, 1500));
    anno(g, p, 'Each agent hands off to the next in sequence');
  }

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
    }
    for (var i = 1; i < 5; i++) {
      dl(g, nodes[i-1].x + 10, nodes[i-1].y, nodes[i].x - 10, nodes[i].y, i * 150);
      g.append('text').attr('x', (nodes[i-1].x + nodes[i].x) / 2).attr('y', by - 16)
        .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#bbb').text('transform');
    }
    for (var i = 0; i < 5; i++) {
      var x = nodes[i].x, y = nodes[i].y;
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
      if (tick < 80) timers.push(setTimeout(anim, nodes.length * 320 + 500));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Data changes shape at each stage');
  }

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
    dl(g, input.x, input.y, router.x - 12, router.y, 200);
    specialists.forEach(function(s, i) {
      dl(g, router.x + 12, router.y, s.x - 8, s.y, 400 + i * 100);
    });
    dn(g, input.x, input.y, 8, '#6b7280', 'Input');
    dn(g, router.x, router.y, 12, '#b91c1c', 'Router');
    specialists.forEach(function(s, i) {
      dn(g, s.x, s.y, 8, s.color, s.label);
    });
    var classLabel = g.append('text').attr('x', router.x).attr('y', router.y - 20)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#b91c1c').attr('font-weight', 'bold').text('');
    var tick = 0;
    function anim() {
      tick++;
      var target = Math.floor(Math.random() * specialists.length);
      var s = specialists[target];
      pkt(g, [input, router], '#6b7280', 0, 3);
      timers.push(setTimeout(function() {
        classLabel.text(s.label + ' detected').attr('opacity', 0)
          .transition().duration(200).attr('opacity', 1)
          .transition().delay(600).duration(300).attr('opacity', 0);
        g.append('line').attr('x1', router.x + 12).attr('y1', router.y)
          .attr('x2', s.x - 8).attr('y2', s.y)
          .attr('stroke', s.color).attr('stroke-width', 2.5).attr('opacity', 0.6)
          .transition().delay(400).duration(400).attr('opacity', 0).remove();
      }, 600));
      pkt(g, [router, s], s.color, 800, 3);
      pkt(g, [s, router, input], resultColor, 1500, 2.5);
      if (tick < 80) timers.push(setTimeout(anim, 2800));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Classify input upfront, send to the right specialist');
  }

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
      if (i > 0) dld(g, models[i-1].x, models[i-1].y, m.x, m.y, i * 200);
    });
    models.forEach(function(m) { dn(g, m.x, m.y, m.r, m.color, m.label); });
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
      if (tick < 80) timers.push(setTimeout(anim, (failAt + 1) * 650 + 1300));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Small model tries first, escalates if unsure');
  }

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
    mids.forEach(function(m) { dl(g, root.x, root.y, m.x, m.y, 300); });
    leaves.forEach(function(l) { dl(g, mids[l.pi].x, mids[l.pi].y, l.x, l.y, 600); });
    dn(g, root.x, root.y, 12, '#7f1d1d', 'Root');
    mids.forEach(function(m, i) { dn(g, m.x, m.y, 8, '#b91c1c', 'Plan ' + (i+1)); });
    leaves.forEach(function(l) { dn(g, l.x, l.y, 4, '#ef4444', ''); });
    var tick = 0;
    function anim() {
      tick++;
      var li = Math.floor(Math.random() * leaves.length);
      var leaf = leaves[li];
      var path = [root, mids[leaf.pi], leaf];
      var d1 = pkt(g, path, taskColor, 0, 3);
      pkt(g, path.slice().reverse(), resultColor, d1, 2.5);
      if (tick < 80) timers.push(setTimeout(anim, 1400));
    }
    timers.push(setTimeout(anim, 1600));
    anno(g, p, 'One task fans out to many sub-tasks');
  }

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
    wk.forEach(function(w, i) {
      dl(g, disp.x, disp.y, w.x, w.y, 250 + i * 50);
      dl(g, w.x, w.y, agg.x, agg.y, 400 + i * 50);
    });
    dn(g, disp.x, disp.y, 10, '#7f1d1d', 'Dispatch');
    dn(g, agg.x, agg.y, 10, '#065f46', 'Merge');
    wk.forEach(function(w) { dn(g, w.x, w.y, 6, '#ef4444', ''); });
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
      if (tick < 80) timers.push(setTimeout(anim, d + wk.length * 60 + 800));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Split work, execute in parallel, aggregate results');
  }

  function drawVoting(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var source = {x: cx - 110, y: ty + 50};
    var judge = {x: cx + 110, y: ty + 50};
    var voters = [];
    for (var i = 0; i < 5; i++) {
      voters.push({x: cx, y: ty + 10 + i * 22});
    }
    voters.forEach(function(v, i) {
      dl(g, source.x, source.y, v.x - 8, v.y, 200 + i * 60);
      dl(g, v.x + 8, v.y, judge.x, judge.y, 400 + i * 60);
    });
    dn(g, source.x, source.y, 10, '#6b7280', 'Task');
    dn(g, judge.x, judge.y, 10, '#7f1d1d', 'Judge');
    voters.forEach(function(v, i) { dn(g, v.x, v.y, 7, '#2563eb', 'Agent ' + (i+1)); });
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
      voters.forEach(function(v, i) { pkt(g, [source, v], taskColor, i * 50, 2.5); });
      var voteDelay = voters.length * 50 + 600;
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
      if (tick < 80) timers.push(setTimeout(anim, voteDelay + voters.length * 120 + 2200));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Same task to N agents, majority decision wins');
  }

  function drawAuction(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 60;
    var auctioneer = {x: cx, y: ty};
    var bidders = [];
    for (var i = 0; i < 6; i++) {
      var a = Math.PI * 0.15 + Math.PI * 0.7 * i / 5;
      bidders.push({x: cx + Math.cos(a) * 80, y: ty + 15 + Math.sin(a) * 70});
    }
    bidders.forEach(function(b, i) {
      dld(g, auctioneer.x, auctioneer.y, b.x, b.y, 200 + i * 80);
    });
    dn(g, auctioneer.x, auctioneer.y, 11, '#7f1d1d', 'Auctioneer');
    var bidLabels = [];
    bidders.forEach(function(b, i) {
      dn(g, b.x, b.y, 7, '#6b7280', 'Agent ' + (i+1));
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
      if (tick < 80) timers.push(setTimeout(anim, bidDelay + bidders.length * 100 + 1400));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Task broadcast, agents bid, highest score wins');
  }

  function drawRelay(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var a = {x: cx - 110, y: by}, b = {x: cx, y: by}, c = {x: cx + 110, y: by};
    dl(g, a.x, a.y, b.x, b.y, 0);
    dl(g, b.x, b.y, c.x, c.y, 200);
    var nA = dn(g, a.x, a.y, 11, '#2563eb', 'Agent A');
    var nB = dn(g, b.x, b.y, 11, '#059669', 'Agent B');
    var nC = dn(g, c.x, c.y, 11, '#7c3aed', 'Agent C');
    g.append('text').attr('x', cx - 55).attr('y', by - 20)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#f59e0b').attr('opacity', 0)
      .text('context').transition().delay(400).duration(200).attr('opacity', 1);
    var tick = 0;
    function anim() {
      tick++;
      pkt(g, [a, b], '#f59e0b', 0, 5);
      timers.push(setTimeout(function() {
        g.selectAll('circle').filter(function() {
          return +d3.select(this).attr('cx') === a.x && +d3.select(this).attr('cy') === a.y;
        }).transition().duration(400).attr('opacity', 0.1);
      }, 700));
      pkt(g, [b, c], '#f59e0b', 900, 5);
      timers.push(setTimeout(function() {
        g.selectAll('circle').filter(function() {
          return +d3.select(this).attr('cx') === b.x && +d3.select(this).attr('cy') === b.y;
        }).transition().duration(400).attr('opacity', 0.1);
      }, 1600));
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', c.x).attr('cy', c.y).attr('r', 18)
          .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
          .transition().duration(600).attr('r', 25).attr('opacity', 0);
      }, 1800));
      if (tick < 80) timers.push(setTimeout(function() {
        g.selectAll('circle').transition().duration(200).attr('opacity', 0.85);
        anim();
      }, 3200));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Control transfers fully \u2014 caller exits after handoff');
  }

  function drawNegotiation(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 95;
    var agentA = {x: cx - 90, y: by}, agentB = {x: cx + 90, y: by};
    dl(g, agentA.x, agentA.y, agentB.x, agentB.y, 0);
    dn(g, agentA.x, agentA.y, 12, '#1e40af', 'Buyer Agent');
    dn(g, agentB.x, agentB.y, 12, '#b91c1c', 'Seller Agent');
    dn(g, cx, by - 50, 7, '#6b7280', 'Witness', by - 38);
    dld(g, cx, by - 43, agentA.x + 12, agentA.y - 10, 200);
    dld(g, cx, by - 43, agentB.x - 12, agentB.y - 10, 200);
    var tick = 0;
    function anim() {
      tick++;
      var d1 = pkt(g, [agentA, {x: cx, y: by}, agentB], '#2563eb', 0, 3);
      pkt(g, [agentB, {x: cx, y: by}, agentA], '#dc2626', d1 + 400, 3);
      if (tick < 80) timers.push(setTimeout(anim, d1 + 1800));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Offer and counter-offer until agreement or impasse');
  }

  function drawDebate(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var debA = {x: cx - 80, y: by + 20}, debB = {x: cx + 80, y: by + 20};
    var judge = {x: cx, y: by - 40};
    dl(g, debA.x, debA.y, debB.x, debB.y, 0);
    dld(g, judge.x, judge.y + 7, debA.x + 10, debA.y - 10, 100);
    dld(g, judge.x, judge.y + 7, debB.x - 10, debB.y - 10, 100);
    dn(g, debA.x, debA.y, 10, '#2563eb', 'Pro');
    dn(g, debB.x, debB.y, 10, '#dc2626', 'Con');
    dn(g, judge.x, judge.y, 12, '#6d28d9', 'Judge');
    var tick = 0;
    function anim() {
      tick++;
      var d1 = pkt(g, [debA, {x: cx, y: by + 20}, debB], '#2563eb', 0, 3);
      pkt(g, [debB, {x: cx, y: by + 20}, debA], '#dc2626', d1 + 300, 3);
      pkt(g, [debA, judge], '#2563eb', d1 + 900, 2);
      pkt(g, [debB, judge], '#dc2626', d1 + 1100, 2);
      if (tick < 80) timers.push(setTimeout(anim, d1 + 2500));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Multi-round argument; judge evaluates both sides');
  }

  function drawCritic(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var gen = {x: cx - 80, y: by}, crit = {x: cx + 80, y: by};
    dl(g, gen.x, gen.y, crit.x, crit.y - 8, 0);
    dld(g, crit.x, crit.y + 8, gen.x, gen.y + 8, 200);
    dn(g, gen.x, gen.y, 12, '#2563eb', 'Generator');
    dn(g, crit.x, crit.y, 12, '#dc2626', 'Critic');
    g.append('text').attr('x', cx).attr('y', by - 22)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#666').attr('opacity', 0)
      .text('output').transition().delay(400).duration(200).attr('opacity', 1);
    g.append('text').attr('x', cx).attr('y', by + 30)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#dc2626').attr('opacity', 0)
      .text('attack').transition().delay(600).duration(200).attr('opacity', 1);
    var tick = 0;
    function anim() {
      tick++;
      var round = (tick - 1) % 3;
      pkt(g, [gen, crit], '#2563eb', 0, 3);
      if (round < 2) {
        timers.push(setTimeout(function() {
          var xMark = g.append('text').attr('x', crit.x + 20).attr('y', crit.y + 5)
            .attr('text-anchor', 'middle').attr('font-size', '16px').attr('fill', '#dc2626').attr('font-weight', 'bold')
            .text('\u2717');
          xMark.transition().delay(500).duration(300).attr('opacity', 0).remove();
        }, 600));
        pkt(g, [crit, gen], '#dc2626', 800, 3);
      } else {
        timers.push(setTimeout(function() {
          var check = g.append('text').attr('x', crit.x + 20).attr('y', crit.y + 5)
            .attr('text-anchor', 'middle').attr('font-size', '16px').attr('fill', '#059669').attr('font-weight', 'bold')
            .text('\u2713');
          check.transition().delay(800).duration(300).attr('opacity', 0).remove();
        }, 600));
      }
      if (tick < 80) timers.push(setTimeout(anim, 1600));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Adversarial verification \u2014 output must survive attack');
  }

  function drawSpeculative(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var disp = {x: cx, y: ty};
    var workers = [
      {x: cx - 95, y: ty + 85},
      {x: cx, y: ty + 95},
      {x: cx + 95, y: ty + 85}
    ];
    workers.forEach(function(w) { dl(g, disp.x, disp.y, w.x, w.y, 0); });
    dn(g, disp.x, disp.y, 11, '#7f1d1d', 'Dispatch');
    workers.forEach(function(w, i) { dn(g, w.x, w.y, 8, '#6b7280', 'W' + (i + 1)); });
    var tick = 0;
    function anim() {
      tick++;
      workers.forEach(function(w, i) {
        pkt(g, [disp, w], taskColor, i * 30, 2.5);
      });
      var winner = tick % 3;
      timers.push(setTimeout(function() {
        workers.forEach(function(w, i) {
          if (i === winner) {
            g.append('circle').attr('cx', w.x).attr('cy', w.y).attr('r', 14)
              .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.9)
              .transition().duration(500).attr('r', 20).attr('opacity', 0);
            var ck = g.append('text').attr('x', w.x + 16).attr('y', w.y + 4)
              .attr('text-anchor', 'middle').attr('font-size', '12px').attr('fill', '#059669').attr('font-weight', 'bold')
              .text('\u2713');
            ck.transition().delay(800).duration(300).attr('opacity', 0).remove();
          } else {
            var xm = g.append('text').attr('x', w.x + 14).attr('y', w.y + 4)
              .attr('text-anchor', 'middle').attr('font-size', '11px').attr('fill', '#ef4444').attr('font-weight', 'bold')
              .text('\u2717');
            xm.transition().delay(600).duration(300).attr('opacity', 0).remove();
          }
        });
        pkt(g, [workers[winner], disp], resultColor, 300, 3);
      }, 900));
      if (tick < 80) timers.push(setTimeout(anim, 2800));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Race to first valid result \u2014 losers discarded');
  }

  function drawTeacherStudent(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 100;
    var teacher = {x: cx - 90, y: by}, student = {x: cx + 90, y: by};
    dl(g, teacher.x, teacher.y, student.x, student.y, 0);
    dn(g, teacher.x, teacher.y, 16, '#1e40af', 'Teacher');
    dn(g, student.x, student.y, 8, '#93c5fd', 'Student');
    var tick = 0, studentR = 8;
    var blues = ['#93c5fd', '#60a5fa', '#3b82f6', '#2563eb'];
    function anim() {
      tick++;
      var round = (tick - 1) % 4;
      pkt(g, [teacher, student], '#3b82f6', 0, 3.5);
      timers.push(setTimeout(function() {
        if (round < 3) {
          studentR = Math.min(studentR + 1.5, 14);
          g.selectAll('circle').filter(function() {
            return +d3.select(this).attr('cx') === student.x && +d3.select(this).attr('cy') === student.y
              && d3.select(this).attr('stroke') === 'white';
          }).transition().duration(400).attr('r', studentR).attr('fill', blues[Math.min(round + 1, 3)]);
        } else {
          g.append('circle').attr('cx', student.x).attr('cy', student.y).attr('r', studentR + 2)
            .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
            .transition().duration(500).attr('r', studentR + 10).attr('opacity', 0);
          studentR = 8;
          g.selectAll('circle').filter(function() {
            return +d3.select(this).attr('cx') === student.x && +d3.select(this).attr('cy') === student.y
              && d3.select(this).attr('stroke') === 'white';
          }).transition().duration(300).attr('r', 8).attr('fill', '#93c5fd');
        }
      }, 700));
      if (tick < 80) timers.push(setTimeout(anim, 1400));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Knowledge transfer from strong to weak model');
  }

  function drawCheckpoint(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, by = p.oy + 105;
    var steps = [
      {x: cx - 120, y: by}, {x: cx - 40, y: by},
      {x: cx + 40, y: by}, {x: cx + 120, y: by}
    ];
    var cpX1 = (steps[0].x + steps[1].x) / 2, cpX2 = (steps[1].x + steps[2].x) / 2;
    for (var i = 0; i < steps.length - 1; i++) dl(g, steps[i].x, steps[i].y, steps[i+1].x, steps[i+1].y, i * 150);
    steps.forEach(function(s, i) { dn(g, s.x, s.y, 7, '#4ade80', 'S' + (i + 1)); });
    [cpX1, cpX2].forEach(function(cx2, i) {
      g.append('polygon').attr('points',
        cx2 + ',' + (by - 8) + ' ' + (cx2 + 6) + ',' + by + ' ' + cx2 + ',' + (by + 8) + ' ' + (cx2 - 6) + ',' + by)
        .attr('fill', '#f59e0b').attr('opacity', 0.7).attr('stroke', '#d97706').attr('stroke-width', 0.5);
      g.append('text').attr('x', cx2).attr('y', by - 14)
        .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#d97706').text('CP' + (i + 1));
    });
    var tick = 0;
    function anim() {
      tick++;
      pkt(g, [steps[0], steps[1]], taskColor, 0, 2.5);
      pkt(g, [steps[1], steps[2]], taskColor, 500, 2.5);
      timers.push(setTimeout(function() {
        var fail = g.append('text').attr('x', steps[2].x).attr('y', steps[2].y - 15)
          .attr('text-anchor', 'middle').attr('font-size', '9px').attr('fill', '#dc2626').attr('font-weight', 'bold')
          .text('FAIL');
        fail.transition().delay(600).duration(300).attr('opacity', 0).remove();
        g.append('circle').attr('cx', steps[2].x).attr('cy', steps[2].y).attr('r', 10)
          .attr('fill', 'none').attr('stroke', '#dc2626').attr('stroke-width', 2).attr('opacity', 0.8)
          .transition().duration(400).attr('r', 16).attr('opacity', 0);
      }, 1100));
      pkt(g, [steps[2], steps[1]], '#f59e0b', 1500, 3);
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', cpX2).attr('cy', by).attr('r', 10)
          .attr('fill', 'none').attr('stroke', '#f59e0b').attr('stroke-width', 2).attr('opacity', 0.7)
          .transition().duration(400).attr('r', 16).attr('opacity', 0);
      }, 2000));
      pkt(g, [steps[1], steps[2]], taskColor, 2500, 2.5);
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', steps[2].x).attr('cy', steps[2].y).attr('r', 10)
          .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
          .transition().duration(400).attr('r', 16).attr('opacity', 0);
      }, 3200));
      pkt(g, [steps[2], steps[3]], taskColor, 3400, 2.5);
      if (tick < 80) timers.push(setTimeout(anim, 5000));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Rollback to last checkpoint on failure');
  }

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
          dynG.append('line').attr('x1', orch.x).attr('y1', orch.y + 13)
            .attr('x2', orch.x).attr('y2', orch.y + 13)
            .attr('stroke', '#ddd').attr('stroke-width', 1.2)
            .transition().delay(i * 100).duration(350)
            .attr('x2', slot.x).attr('y2', slot.y - 7);
          dynG.append('circle').attr('cx', slot.x).attr('cy', slot.y).attr('r', 0)
            .attr('fill', '#ef4444').attr('stroke', 'white').attr('stroke-width', 1).attr('opacity', 0.85)
            .transition().delay(i * 100).duration(300).attr('r', 7);
          dynG.append('text').attr('x', slot.x).attr('y', slot.y + 18)
            .attr('text-anchor', 'middle').attr('font-size', '6px').attr('fill', '#888').text(taskName);
          pkt(dynG, [orch, slot], taskColor, 200 + i * 150, 2.5);
          pkt(dynG, [slot, orch], resultColor, 800 + i * 150, 2);
        }
      }, 600));
      if (tick < 80) timers.push(setTimeout(anim, 3000));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Orchestrator plans dynamically, spawns workers as needed');
  }

  function drawEvaluator(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 65;
    var generator = {x: cx - 80, y: ty + 40};
    var evaluator = {x: cx + 80, y: ty + 40};
    dl(g, generator.x + 12, generator.y - 8, evaluator.x - 12, evaluator.y - 8, 300);
    dld(g, evaluator.x - 12, evaluator.y + 8, generator.x + 12, generator.y + 8, 500);
    dn(g, generator.x, generator.y, 12, '#2563eb', 'Generator');
    dn(g, evaluator.x, evaluator.y, 12, '#d97706', 'Evaluator');
    g.append('text').attr('x', cx).attr('y', ty + 22).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').text('output');
    g.append('text').attr('x', cx).attr('y', ty + 66).attr('text-anchor', 'middle')
      .attr('font-size', '7px').attr('fill', '#888').attr('font-style', 'italic').text('feedback');
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
          pkt(g, [generator, evaluator], '#2563eb', baseDelay, 3);
          timers.push(setTimeout(function() {
            s = Math.min(98, s + 15 + Math.floor(Math.random() * 10));
            scoreLabel.text('Quality: ' + s + '%').attr('fill', s > 80 ? '#059669' : '#d97706');
            iterLabel.text('Iteration ' + (iter + 1) + '/' + iterations);
          }, baseDelay + 500));
          if (iter < iterations - 1) {
            pkt(g, [evaluator, generator], '#d97706', baseDelay + 700, 2.5);
          } else {
            timers.push(setTimeout(function() {
              g.append('circle').attr('cx', evaluator.x).attr('cy', evaluator.y).attr('r', 12)
                .attr('fill', 'none').attr('stroke', '#059669').attr('stroke-width', 2).attr('opacity', 0.8)
                .transition().duration(500).attr('r', 22).attr('opacity', 0).remove();
            }, baseDelay + 800));
          }
        })(i, score);
        score = Math.min(98, score + 15 + Math.floor(Math.random() * 10));
      }
      if (tick < 80) timers.push(setTimeout(anim, iterations * 1200 + 1500));
    }
    timers.push(setTimeout(anim, 1400));
    anno(g, p, 'Generate, evaluate, refine until quality threshold met');
  }

  function drawSwarm(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + cellH / 2 + 10;
    var phases = [
      {n: 1, colors: ['#dc2626'], targets: [{x: cx, y: cy}]},
      {n: 2, colors: ['#2563eb', '#d97706'], targets: [{x: cx - 40, y: cy - 10}, {x: cx + 40, y: cy + 15}]},
      {n: 3, colors: ['#059669', '#2563eb', '#d97706'], targets: [{x: cx - 50, y: cy - 20}, {x: cx + 45, y: cy - 15}, {x: cx, y: cy + 35}]}
    ];
    var agents = [];
    for (var i = 0; i < 15; i++) {
      var a = Math.random() * Math.PI * 2, r = 20 + Math.random() * 55;
      agents.push({x: cx + Math.cos(a) * r, y: cy + Math.sin(a) * r, vx: (Math.random() - 0.5) * 1.2, vy: (Math.random() - 0.5) * 1.2, cluster: 0, el: null});
    }
    var links = g.append('g');
    agents.forEach(function(ag) {
      ag.el = g.append('circle').attr('cx', ag.x).attr('cy', ag.y).attr('r', 4)
        .attr('fill', '#6b7280').attr('stroke', 'white').attr('stroke-width', 0.5).attr('opacity', 0.8);
    });
    var tick = 0;
    var cycleLen = 110;
    var disperseLen = 30;
    var totalCycle = cycleLen + disperseLen;
    function anim() {
      tick++;
      links.selectAll('line').remove();
      var globalPhase = Math.floor(tick / totalCycle) % 3;
      var localTick = tick % totalCycle;
      var dispersing = localTick >= cycleLen;
      var ph = phases[globalPhase];
      var formation = dispersing ? Math.max(0, 1 - (localTick - cycleLen) / disperseLen) : Math.min(1, localTick / 60);
      if (localTick === 0) {
        agents.forEach(function(ag, i) { ag.cluster = i % ph.n; });
      }
      var time = tick * 0.015;
      agents.forEach(function(ag, i) {
        if (dispersing) {
          ag.vx += (Math.random() - 0.5) * 0.5;
          ag.vy += (Math.random() - 0.5) * 0.5;
        } else {
          var t = ph.targets[ag.cluster];
          var goalX = t.x + Math.cos(time + ag.cluster * 2.1) * 12;
          var goalY = t.y + Math.sin(time * 0.7 + ag.cluster * 1.5) * 10;
          var orbit = (i % Math.ceil(15 / ph.n)) / Math.ceil(15 / ph.n) * Math.PI * 2 + time * (1 + ag.cluster * 0.3);
          var orbitR = (ph.n === 1 ? 22 : 14) + Math.sin(time * 0.5 + i) * 4;
          var tx = goalX + Math.cos(orbit) * orbitR;
          var ty = goalY + Math.sin(orbit) * orbitR;
          ag.vx += (tx - ag.x) * 0.025 * formation;
          ag.vy += (ty - ag.y) * 0.025 * formation;
        }
        ag.vx += (Math.random() - 0.5) * 0.15;
        ag.vy += (Math.random() - 0.5) * 0.15;
        ag.vx *= 0.91; ag.vy *= 0.91;
        ag.x += ag.vx; ag.y += ag.vy;
        if (ag.x < p.ox + 12) { ag.x = p.ox + 12; ag.vx *= -0.5; }
        if (ag.x > p.ox + cellW - 12) { ag.x = p.ox + cellW - 12; ag.vx *= -0.5; }
        if (ag.y < p.oy + 42) { ag.y = p.oy + 42; ag.vy *= -0.5; }
        if (ag.y > p.oy + cellH - 22) { ag.y = p.oy + cellH - 22; ag.vy *= -0.5; }
        ag.el.attr('cx', ag.x).attr('cy', ag.y);
        var col = ph.colors[ag.cluster];
        ag.el.attr('fill', d3.interpolate('#6b7280', col)(formation));
        if (formation > 0.3 && !dispersing) {
          agents.forEach(function(other, j) {
            if (j <= i || other.cluster !== ag.cluster) return;
            var dist = Math.sqrt(Math.pow(other.x - ag.x, 2) + Math.pow(other.y - ag.y, 2));
            if (dist < 42) {
              links.append('line').attr('x1', ag.x).attr('y1', ag.y)
                .attr('x2', other.x).attr('y2', other.y)
                .attr('stroke', col).attr('stroke-width', 0.7).attr('opacity', 0.25 * formation);
            }
          });
        }
      });
      timers.push(setTimeout(anim, 45));
    }
    timers.push(setTimeout(anim, 1000));
    anno(g, p, 'Collective behavior, no central plan');
  }

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
      if (tick < 80) timers.push(setTimeout(anim, 700));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Information spreads peer-to-peer like a rumor');
  }

  function drawSupervisor(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, sy = p.oy + 68;
    var wy = p.oy + 148;
    var wxs = [cx - 90, cx, cx + 90];
    var wlabels = ['Worker A', 'Worker B', 'Worker C'];
    // lines first (behind nodes)
    for (var i = 0; i < 3; i++) dl(g, cx, sy + 13, wxs[i], wy - 11, i * 120);
    // nodes on top
    dn(g, cx, sy, 14, '#7f1d1d', 'Supervisor');
    for (var i = 0; i < 3; i++) dn(g, wxs[i], wy, 10, '#475569', wlabels[i]);
    var tick = 0;
    function anim() {
      tick++;
      // dispatch tasks down
      for (var i = 0; i < 3; i++) {
        pkt(g, [{x: cx, y: sy}, {x: wxs[i], y: wy}], taskColor, 200 + i * 120, 2.5);
      }
      // workers send results back
      timers.push(setTimeout(function() {
        for (var i = 0; i < 3; i++) {
          (function(idx) {
            timers.push(setTimeout(function() {
              pkt(g, [{x: wxs[idx], y: wy}, {x: cx, y: sy}], resultColor, 0, 2.5);
            }, idx * 150));
          })(i);
        }
      }, 1100));
      // reject worker 1's result with red X
      timers.push(setTimeout(function() {
        var xMark = g.append('text').attr('x', wxs[1]).attr('y', wy - 20)
          .attr('text-anchor', 'middle').attr('font-size', '11px').attr('font-weight', 'bold')
          .attr('fill', '#dc2626').attr('opacity', 0).text('x');
        xMark.transition().duration(200).attr('opacity', 1)
          .transition().delay(600).duration(300).attr('opacity', 0).remove();
        // send feedback back down
        pkt(g, [{x: cx, y: sy}, {x: wxs[1], y: wy}], '#f59e0b', 300, 2.5);
      }, 2000));
      // worker retries with green check
      timers.push(setTimeout(function() {
        pkt(g, [{x: wxs[1], y: wy}, {x: cx, y: sy}], resultColor, 0, 3);
        var check = g.append('text').attr('x', wxs[1]).attr('y', wy - 20)
          .attr('text-anchor', 'middle').attr('font-size', '11px').attr('font-weight', 'bold')
          .attr('fill', '#4ade80').attr('opacity', 0).text('v');
        check.transition().duration(200).attr('opacity', 1)
          .transition().delay(700).duration(300).attr('opacity', 0).remove();
      }, 3100));
      if (tick < 80) timers.push(setTimeout(anim, 4200));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Frontier model reviews, rejects, re-dispatches');
  }

  function drawCircuitBreaker(p) {
    var g = svg.append('g');
    var cy = p.oy + 105;
    var caller = {x: p.ox + 60, y: cy};
    var sw = {x: p.ox + 170, y: cy};
    var svc = {x: p.ox + 285, y: cy};
    // lines first
    dl(g, caller.x + 11, cy, sw.x - 10, cy, 0);
    dl(g, sw.x + 10, cy, svc.x - 11, cy, 200);
    // nodes on top
    dn(g, caller.x, cy, 11, '#475569', 'Caller');
    var swNode = g.append('circle').attr('cx', sw.x).attr('cy', cy).attr('r', 10)
      .attr('fill', '#059669').attr('stroke', 'white').attr('stroke-width', 1.2).attr('opacity', 0.85);
    g.append('text').attr('x', sw.x).attr('y', cy + 19)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#888').text('Switch');
    dn(g, svc.x, cy, 11, '#475569', 'Service');
    var statusLabel = g.append('text').attr('x', sw.x).attr('y', cy - 18)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#aaa').text('closed');
    var tick = 0;
    var failures = 0;
    var state = 'closed';
    function anim() {
      tick++;
      if (state === 'closed') {
        pkt(g, [caller, sw, svc], taskColor, 0, 2.5);
        failures++;
        // service fails (red flash on svc)
        timers.push(setTimeout(function() {
          g.append('circle').attr('cx', svc.x).attr('cy', cy).attr('r', 11)
            .attr('fill', '#dc2626').attr('opacity', 0.5)
            .transition().duration(250).attr('opacity', 0).remove();
        }, 600));
        if (failures >= 3) {
          // circuit opens
          timers.push(setTimeout(function() {
            state = 'open';
            failures = 0;
            swNode.transition().duration(300).attr('fill', '#dc2626');
            statusLabel.text('OPEN').attr('fill', '#dc2626');
            // after pause, go half-open
            timers.push(setTimeout(function() {
              state = 'half';
              swNode.transition().duration(300).attr('fill', '#f59e0b');
              statusLabel.text('half-open').attr('fill', '#f59e0b');
            }, 1400));
          }, 700));
        }
      } else if (state === 'open') {
        // bounce packet back immediately
        pkt(g, [caller, sw, caller], '#f87171', 0, 2.5);
      } else if (state === 'half') {
        // test packet through
        pkt(g, [caller, sw, svc], taskColor, 0, 2.5);
        timers.push(setTimeout(function() {
          pkt(g, [svc, sw, caller], resultColor, 0, 2.5);
          swNode.transition().duration(300).attr('fill', '#059669');
          statusLabel.text('closed').attr('fill', '#aaa');
          state = 'closed';
        }, 700));
      }
      if (tick < 80) timers.push(setTimeout(anim, 1100));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Accumulated failures open the circuit, test before re-closing');
  }

  function drawTimeout(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    var dispatcher = {x: p.ox + 65, y: p.oy + 80};
    var worker = {x: p.ox + 265, y: p.oy + 80};
    var altWorker = {x: p.ox + 265, y: p.oy + 155};
    // lines first
    dl(g, dispatcher.x + 12, dispatcher.y, worker.x - 12, worker.y, 0);
    dl(g, dispatcher.x + 12, dispatcher.y + 4, altWorker.x - 12, altWorker.y, 600);
    // nodes on top
    dn(g, dispatcher.x, dispatcher.y, 12, '#475569', 'Dispatcher');
    dn(g, worker.x, worker.y, 11, '#475569', 'Worker');
    dn(g, altWorker.x, altWorker.y, 11, '#059669', 'Fallback');
    // clock label
    var clockLabel = g.append('text').attr('x', cx).attr('y', p.oy + 68)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#aaa').text('');
    var tick = 0;
    function anim() {
      tick++;
      // send task to worker
      pkt(g, [dispatcher, worker], taskColor, 0, 2.5);
      // clock ticks
      var ticks = [300, 600, 900];
      ticks.forEach(function(t, i) {
        timers.push(setTimeout(function() {
          clockLabel.text('... ' + (i + 1) + 's').attr('fill', i < 2 ? '#aaa' : '#dc2626');
        }, t));
      });
      // timeout fires - worker dims
      timers.push(setTimeout(function() {
        clockLabel.text('timeout!').attr('fill', '#dc2626');
        g.append('circle').attr('cx', worker.x).attr('cy', worker.y).attr('r', 11)
          .attr('fill', '#dc2626').attr('opacity', 0.4)
          .transition().duration(400).attr('opacity', 0).remove();
      }, 1100));
      // dispatch to fallback
      timers.push(setTimeout(function() {
        clockLabel.text('');
        pkt(g, [dispatcher, altWorker], taskColor, 0, 2.5);
      }, 1500));
      // fallback responds
      timers.push(setTimeout(function() {
        pkt(g, [altWorker, dispatcher], resultColor, 0, 3);
      }, 2100));
      if (tick < 80) timers.push(setTimeout(anim, 3200));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'No response triggers fallback to alternative');
  }

  function drawCanary(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    var router = {x: p.ox + 55, y: p.oy + 110};
    var incumbent = {x: p.ox + 200, y: p.oy + 75};
    var canary = {x: p.ox + 200, y: p.oy + 150};
    var judge = {x: p.ox + 295, y: p.oy + 110};
    // lines first
    dl(g, router.x + 12, router.y - 5, incumbent.x - 12, incumbent.y, 0);
    dl(g, router.x + 12, router.y + 5, canary.x - 12, canary.y, 0);
    dl(g, incumbent.x + 12, incumbent.y, judge.x - 12, judge.y - 5, 0);
    dl(g, canary.x + 12, canary.y, judge.x - 12, judge.y + 5, 0);
    // nodes on top
    dn(g, router.x, router.y, 12, '#475569', 'Router');
    dn(g, incumbent.x, incumbent.y, 11, '#2563eb', 'Incumbent');
    dn(g, canary.x, canary.y, 11, '#f59e0b', 'Canary');
    dn(g, judge.x, judge.y, 11, '#059669', 'Judge');
    var tick = 0;
    function anim() {
      tick++;
      // route to both
      pkt(g, [router, incumbent], taskColor, 0, 2.5);
      pkt(g, [router, canary], taskColor, 200, 2.5);
      // both respond to judge
      timers.push(setTimeout(function() {
        pkt(g, [incumbent, judge], resultColor, 0, 2.5);
        pkt(g, [canary, judge], '#f59e0b', 200, 2.5);
      }, 1000));
      // judge approves (green flash)
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', judge.x).attr('cy', judge.y).attr('r', 14)
          .attr('fill', '#059669').attr('opacity', 0.5)
          .transition().duration(500).attr('opacity', 0).remove();
      }, 2000));
      if (tick < 80) timers.push(setTimeout(anim, 3000));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Shadow agent tested on live traffic before promotion');
  }

  function drawWitness(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    var producer = {x: p.ox + 60, y: p.oy + 120};
    var witness = {x: cx, y: p.oy + 65};
    var consumer = {x: p.ox + 280, y: p.oy + 120};
    // lines first
    dl(g, producer.x + 11, producer.y - 4, witness.x - 10, witness.y + 7, 0);
    dl(g, witness.x + 10, witness.y + 7, consumer.x - 11, consumer.y - 4, 400);
    // nodes on top
    dn(g, producer.x, producer.y, 12, '#475569', 'Producer');
    dn(g, witness.x, witness.y, 11, '#7c3aed', 'Witness');
    dn(g, consumer.x, consumer.y, 12, '#475569', 'Consumer');
    var statusLabel = g.append('text').attr('x', cx).attr('y', p.oy + 50)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#aaa').text('');
    var tick = 0;
    function anim() {
      tick++;
      // producer sends to witness
      pkt(g, [producer, witness], taskColor, 0, 2.5);
      // witness certifies
      timers.push(setTimeout(function() {
        statusLabel.text('certified').attr('fill', '#4ade80');
        g.append('circle').attr('cx', witness.x).attr('cy', witness.y).attr('r', 11)
          .attr('fill', 'none').attr('stroke', '#4ade80').attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(400).attr('r', 20).attr('opacity', 0).remove();
      }, 650));
      // certified output to consumer
      timers.push(setTimeout(function() {
        statusLabel.text('');
        pkt(g, [witness, consumer], resultColor, 0, 3);
      }, 1100));
      // consumer receives
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', consumer.x).attr('cy', consumer.y).attr('r', 12)
          .attr('fill', '#4ade80').attr('opacity', 0.3)
          .transition().duration(400).attr('opacity', 0).remove();
      }, 1700));
      if (tick < 80) timers.push(setTimeout(anim, 2800));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Independent verification before acceptance');
  }

  function drawBlackboard(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 110;
    // blackboard rect - draw first (behind agents)
    var bbX = cx - 55, bbY = cy - 30, bbW = 110, bbH = 70;
    g.append('rect').attr('x', bbX).attr('y', bbY).attr('width', bbW).attr('height', bbH)
      .attr('rx', 4).attr('fill', '#1e293b').attr('stroke', '#334155').attr('stroke-width', 1.5);
    g.append('text').attr('x', cx).attr('y', bbY - 4)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#94a3b8').text('Blackboard');
    // agent positions around board
    var agents = [
      {x: cx - 105, y: cy - 10, label: 'Agent A', color: '#2563eb'},
      {x: cx + 105, y: cy - 10, label: 'Agent B', color: '#d97706'},
      {x: cx - 60, y: cy + 80, label: 'Agent C', color: '#7c3aed'},
      {x: cx + 60, y: cy + 80, label: 'Agent D', color: '#059669'}
    ];
    // lines from agents to board (behind agents)
    dl(g, agents[0].x + 11, agents[0].y, bbX, cy - 5, 0);
    dl(g, agents[1].x - 11, agents[1].y, bbX + bbW, cy - 5, 200);
    dl(g, agents[2].x, agents[2].y - 11, cx - 20, bbY + bbH, 400);
    dl(g, agents[3].x, agents[3].y - 11, cx + 20, bbY + bbH, 600);
    // agents on top
    for (var i = 0; i < agents.length; i++) dn(g, agents[i].x, agents[i].y, 10, agents[i].color, agents[i].label);
    var dots = [];
    var agentColors = ['#3b82f6', '#f59e0b', '#8b5cf6', '#10b981'];
    var tick = 0;
    function anim() {
      tick++;
      var seq = [0, 1, 2, 3];
      seq.forEach(function(ai, step) {
        timers.push(setTimeout(function() {
          var bx = bbX + 12 + Math.random() * (bbW - 24);
          var by2 = bbY + 10 + Math.random() * (bbH - 20);
          var dot = g.append('circle').attr('cx', bx).attr('cy', by2).attr('r', 0)
            .attr('fill', agentColors[ai]).attr('opacity', 0.85);
          dot.transition().duration(250).attr('r', 3);
          dots.push(dot);
        }, step * 600));
      });
      // final agent reads board and produces result
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', agents[3].x).attr('cy', agents[3].y).attr('r', 10)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(500).attr('r', 22).attr('opacity', 0).remove();
      }, 3000));
      // clear board after cycle
      timers.push(setTimeout(function() {
        dots.forEach(function(d) { d.transition().duration(400).attr('opacity', 0).remove(); });
        dots = [];
      }, 4000));
      if (tick < 80) timers.push(setTimeout(anim, 4800));
    }
    timers.push(setTimeout(anim, 1000));
    anno(g, p, 'Agents contribute to a shared workspace');
  }

  function drawPubSub(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 105;
    var brokerX = cx - 20, brokerY = cy - 15, brokerW = 52, brokerH = 36;
    // broker rect - draw first
    g.append('rect').attr('x', brokerX).attr('y', brokerY).attr('width', brokerW).attr('height', brokerH)
      .attr('rx', 4).attr('fill', '#334155').attr('stroke', '#475569').attr('stroke-width', 1.5);
    g.append('text').attr('x', brokerX + brokerW / 2).attr('y', brokerY + brokerH + 12)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#94a3b8').text('Broker');
    var pubs = [
      {x: p.ox + 50, y: cy - 30, color: '#2563eb', topic: 'A'},
      {x: p.ox + 50, y: cy + 30, color: '#d97706', topic: 'B'}
    ];
    var subs = [
      {x: p.ox + 275, y: cy - 50, color: '#2563eb', topic: 'A'},
      {x: p.ox + 275, y: cy, color: '#059669', topic: 'A+B'},
      {x: p.ox + 275, y: cy + 50, color: '#d97706', topic: 'B'}
    ];
    var brokerCx = brokerX + brokerW / 2, brokerCy = brokerY + brokerH / 2;
    // lines pub -> broker
    dl(g, pubs[0].x + 11, pubs[0].y, brokerX, brokerCy - 5, 0);
    dl(g, pubs[1].x + 11, pubs[1].y, brokerX, brokerCy + 5, 200);
    // lines broker -> subs
    dl(g, brokerX + brokerW, brokerCy - 5, subs[0].x - 11, subs[0].y, 400);
    dl(g, brokerX + brokerW, brokerCy, subs[1].x - 11, subs[1].y, 600);
    dl(g, brokerX + brokerW, brokerCy + 5, subs[2].x - 11, subs[2].y, 800);
    // publishers
    for (var i = 0; i < pubs.length; i++) dn(g, pubs[i].x, pubs[i].y, 10, pubs[i].color, 'Pub ' + pubs[i].topic);
    // subscribers
    for (var i = 0; i < subs.length; i++) dn(g, subs[i].x, subs[i].y, 10, '#475569', 'Sub ' + subs[i].topic);
    var tick = 0;
    function anim() {
      tick++;
      // pub A sends
      pkt(g, [pubs[0], {x: brokerCx, y: brokerCy}], '#3b82f6', 0, 2.5);
      // pub B sends
      pkt(g, [pubs[1], {x: brokerCx, y: brokerCy}], '#f59e0b', 200, 2.5);
      // broker routes topic A to sub 0 and sub 1
      timers.push(setTimeout(function() {
        pkt(g, [{x: brokerCx, y: brokerCy}, subs[0]], '#3b82f6', 0, 2.5);
        pkt(g, [{x: brokerCx, y: brokerCy}, subs[1]], '#3b82f6', 100, 2);
      }, 700));
      // broker routes topic B to sub 1 and sub 2
      timers.push(setTimeout(function() {
        pkt(g, [{x: brokerCx, y: brokerCy}, subs[1]], '#f59e0b', 0, 2);
        pkt(g, [{x: brokerCx, y: brokerCy}, subs[2]], '#f59e0b', 100, 2.5);
      }, 1000));
      if (tick < 80) timers.push(setTimeout(anim, 2200));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Publishers and subscribers fully decoupled via topics');
  }

  function drawChoreography(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 115;
    var r = 62;
    var agentLabels = ['Agent A', 'Agent B', 'Agent C', 'Agent D'];
    var agentColors = ['#2563eb', '#d97706', '#7c3aed', '#059669'];
    var positions = [];
    for (var i = 0; i < 4; i++) {
      var a = (i / 4) * Math.PI * 2 - Math.PI / 2;
      positions.push({x: cx + Math.cos(a) * r, y: cy + Math.sin(a) * r});
    }
    // ring lines (behind nodes)
    for (var i = 0; i < 4; i++) {
      dl(g, positions[i].x, positions[i].y, positions[(i + 1) % 4].x, positions[(i + 1) % 4].y, i * 100);
    }
    // nodes on top
    for (var i = 0; i < 4; i++) dn(g, positions[i].x, positions[i].y, 11, agentColors[i], agentLabels[i]);
    var tick = 0;
    function anim() {
      tick++;
      // A emits event
      pkt(g, [positions[0], positions[1]], '#3b82f6', 0, 3);
      // B reacts and emits
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', positions[1].x).attr('cy', positions[1].y).attr('r', 11)
          .attr('fill', 'none').attr('stroke', '#f59e0b').attr('stroke-width', 1.5).attr('opacity', 0.8)
          .transition().duration(300).attr('r', 18).attr('opacity', 0).remove();
        pkt(g, [positions[1], positions[2]], '#f59e0b', 100, 3);
      }, 600));
      // C reacts and emits
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', positions[2].x).attr('cy', positions[2].y).attr('r', 11)
          .attr('fill', 'none').attr('stroke', '#8b5cf6').attr('stroke-width', 1.5).attr('opacity', 0.8)
          .transition().duration(300).attr('r', 18).attr('opacity', 0).remove();
        pkt(g, [positions[2], positions[3]], '#8b5cf6', 100, 3);
      }, 1200));
      // D completes the chain
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', positions[3].x).attr('cy', positions[3].y).attr('r', 11)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(500).attr('r', 24).attr('opacity', 0).remove();
      }, 1900));
      if (tick < 80) timers.push(setTimeout(anim, 3000));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'No controller: agents react to peer events');
  }

  function drawStigmergy(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    // environment rect at bottom - draw first
    var envX = p.ox + 40, envY = p.oy + 158, envW = cellW - 80, envH = 40;
    g.append('rect').attr('x', envX).attr('y', envY).attr('width', envW).attr('height', envH)
      .attr('rx', 4).attr('fill', '#1e293b').attr('stroke', '#334155').attr('stroke-width', 1.5);
    g.append('text').attr('x', cx).attr('y', envY + envH + 12)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#94a3b8').text('Environment');
    var agentYs = p.oy + 78;
    var axs = [p.ox + 75, cx, p.ox + 265];
    var agentColors = ['#2563eb', '#d97706', '#7c3aed'];
    var agentLabels = ['Agent 1', 'Agent 2', 'Agent 3'];
    // lines agent -> env (behind agents)
    for (var i = 0; i < 3; i++) dl(g, axs[i], agentYs + 11, axs[i], envY, i * 200);
    // agents on top
    for (var i = 0; i < 3; i++) dn(g, axs[i], agentYs, 10, agentColors[i], agentLabels[i]);
    var marks = [];
    var tick = 0;
    function anim() {
      tick++;
      // agent 1 drops mark
      timers.push(setTimeout(function() {
        var m = g.append('circle').attr('cx', envX + 30).attr('cy', envY + envH / 2).attr('r', 0)
          .attr('fill', '#3b82f6').attr('opacity', 0.85);
        m.transition().duration(250).attr('r', 4);
        marks.push(m);
      }, 400));
      // agent 2 reads and drops its mark
      timers.push(setTimeout(function() {
        g.append('line').attr('x1', axs[1]).attr('y1', agentYs + 11)
          .attr('x2', axs[1]).attr('y2', agentYs + 11)
          .attr('stroke', '#f59e0b').attr('stroke-width', 1).attr('stroke-dasharray', '3,2').attr('opacity', 0.6)
          .transition().duration(300).attr('y2', envY);
        var m = g.append('circle').attr('cx', envX + envW / 2).attr('cy', envY + envH / 2).attr('r', 0)
          .attr('fill', '#f59e0b').attr('opacity', 0.85);
        m.transition().duration(250).attr('r', 4);
        marks.push(m);
      }, 1100));
      // agent 3 reads both and produces result
      timers.push(setTimeout(function() {
        g.append('line').attr('x1', axs[2]).attr('y1', agentYs + 11)
          .attr('x2', axs[2]).attr('y2', agentYs + 11)
          .attr('stroke', '#8b5cf6').attr('stroke-width', 1).attr('stroke-dasharray', '3,2').attr('opacity', 0.6)
          .transition().duration(300).attr('y2', envY);
      }, 1800));
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', axs[2]).attr('cy', agentYs).attr('r', 10)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(500).attr('r', 22).attr('opacity', 0).remove();
      }, 2400));
      // clear marks
      timers.push(setTimeout(function() {
        marks.forEach(function(m) { m.transition().duration(400).attr('opacity', 0).remove(); });
        marks = [];
      }, 3200));
      if (tick < 80) timers.push(setTimeout(anim, 4000));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Agents coordinate through environmental traces');
  }

  function drawMissionCommand(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2;
    var cmdY = p.oy + 60;
    var subY = p.oy + 130;
    var subXs = [p.ox + 75, cx, p.ox + 265];
    var goalY = p.oy + 195;
    var trailColors = ['#2563eb', '#d97706', '#7c3aed'];
    // lines commander -> subordinates (behind nodes)
    for (var i = 0; i < 3; i++) dl(g, cx, cmdY + 13, subXs[i], subY - 11, i * 120);
    // goal marker at bottom
    g.append('rect').attr('x', cx - 18).attr('y', goalY - 8).attr('width', 36).attr('height', 16)
      .attr('rx', 3).attr('fill', '#065f46').attr('stroke', '#4ade80').attr('stroke-width', 1.5).attr('opacity', 0.85);
    g.append('text').attr('x', cx).attr('y', goalY + 4)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#4ade80').text('Goal');
    // lines subordinates -> goal (behind nodes, same color per trail)
    for (var i = 0; i < 3; i++) {
      g.append('line').attr('x1', subXs[i]).attr('y1', subY + 11).attr('x2', subXs[i]).attr('y2', subY + 11)
        .attr('stroke', trailColors[i]).attr('stroke-width', 1.2).attr('opacity', 0.35)
        .transition().delay(1200 + i * 150).duration(400).attr('x2', cx).attr('y2', goalY - 8);
    }
    // commander node
    dn(g, cx, cmdY, 13, '#7f1d1d', 'Commander');
    // intent label
    var intentLabel = g.append('text').attr('x', cx).attr('y', cmdY - 16)
      .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#3b82f6').attr('opacity', 0).text('intent');
    intentLabel.transition().delay(200).duration(300).attr('opacity', 1)
      .transition().delay(1800).duration(400).attr('opacity', 0);
    // subordinate nodes on top
    for (var i = 0; i < 3; i++) dn(g, subXs[i], subY, 10, '#475569', 'Sub ' + (i + 1));
    var tick = 0;
    function anim() {
      tick++;
      // dispatch intent (blue packets) to all subordinates
      for (var i = 0; i < 3; i++) {
        pkt(g, [{x: cx, y: cmdY}, {x: subXs[i], y: subY}], '#3b82f6', 200 + i * 130, 3);
      }
      // each subordinate takes different colored trail to goal
      timers.push(setTimeout(function() {
        for (var i = 0; i < 3; i++) {
          (function(idx) {
            pkt(g, [{x: subXs[idx], y: subY}, {x: cx, y: goalY}], trailColors[idx], idx * 180, 2.5);
          })(i);
        }
      }, 1500));
      // goal flashes green
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', cx).attr('cy', goalY).attr('r', 10)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.9)
          .transition().duration(500).attr('r', 26).attr('opacity', 0).remove();
      }, 2500));
      if (tick < 80) timers.push(setTimeout(anim, 3600));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Intent communicated, method left to the agent');
  }

  function drawFederatedLearning(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, cy = p.oy + 110;
    var dataNodes = [];
    var angles = [-Math.PI * 2 / 3, 0, Math.PI * 2 / 3];
    var r = 72;
    for (var i = 0; i < 3; i++) {
      dataNodes.push({x: cx + Math.cos(angles[i]) * r, y: cy + Math.sin(angles[i]) * r});
    }
    var coord = {x: cx, y: cy};
    // lines coordinator <-> data nodes (behind nodes)
    for (var i = 0; i < 3; i++) {
      dl(g, coord.x, coord.y, dataNodes[i].x, dataNodes[i].y, i * 120);
    }
    // coordinator node
    dn(g, coord.x, coord.y, 14, '#7f1d1d', 'Coordinator');
    // data nodes with labels
    for (var i = 0; i < 3; i++) {
      dn(g, dataNodes[i].x, dataNodes[i].y, 11, '#475569', 'Data ' + (i + 1));
      // data label inside node
      g.append('text').attr('x', dataNodes[i].x).attr('y', dataNodes[i].y + 3)
        .attr('text-anchor', 'middle').attr('font-size', '5px').attr('fill', 'white').attr('opacity', 0.7)
        .text('data');
    }
    var tick = 0;
    function anim() {
      tick++;
      // data nodes send update packets to coordinator (blue, not raw data)
      for (var i = 0; i < 3; i++) {
        pkt(g, [dataNodes[i], coord], '#3b82f6', i * 200, 2.5);
      }
      // coordinator aggregates and sends model back (green)
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx', coord.x).attr('cy', coord.y).attr('r', 14)
          .attr('fill', 'none').attr('stroke', resultColor).attr('stroke-width', 2).attr('opacity', 0.85)
          .transition().duration(400).attr('r', 22).attr('opacity', 0).remove();
        for (var i = 0; i < 3; i++) {
          (function(idx) {
            pkt(g, [coord, dataNodes[idx]], resultColor, idx * 180, 3);
          })(i);
        }
      }, 1300));
      if (tick < 80) timers.push(setTimeout(anim, 2800));
    }
    timers.push(setTimeout(anim, 1200));
    anno(g, p, 'Agents share updates, never raw data');
  }

  function drawLoop(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW/2, cy = p.oy + 45 + (cellH-45)/2;
    var ax = p.ox + 100, ay = cy;
    var tx = p.ox + 250, ty = cy;
    dl(g, ax, ay, tx, ty, 0);
    dn(g, ax, ay, 22, '#3b82f6', 'agent');
    dn(g, tx, ty, 18, '#64748b', 'target');
    var tick = 0;
    function run() {
      tick++;
      var lbl = g.append('text').attr('x', cx).attr('y', p.oy + 70).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',11).text('attempt');
      pkt(g, [{x:ax,y:ay},{x:tx,y:ty}], '#dc2626', 200, 5);
      timers.push(setTimeout(function() {
        lbl.text('error');
        pkt(g, [{x:tx,y:ty},{x:ax,y:ay}], '#dc2626', 0, 5);
      }, 1000));
      timers.push(setTimeout(function() {
        lbl.text('retry');
        pkt(g, [{x:ax,y:ay},{x:tx,y:ty}], '#dc2626', 0, 5);
      }, 2000));
      timers.push(setTimeout(function() {
        lbl.text('success');
        pkt(g, [{x:tx,y:ty},{x:ax,y:ay}], '#4ade80', 0, 5);
      }, 3000));
      timers.push(setTimeout(function() { lbl.remove(); if (tick < 80) run(); }, 4500));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Self-feedback loop until success');
  }

  function drawFeudal(p) {
    var g = svg.append('g');
    var px2 = p.ox + 170, py2 = p.oy + 70;
    var m1x = p.ox + 90, m1y = p.oy + 130;
    var m2x = p.ox + 250, m2y = p.oy + 130;
    var l1x = p.ox + 55, l1y = p.oy + 195;
    var l2x = p.ox + 130, l2y = p.oy + 195;
    var l3x = p.ox + 215, l3y = p.oy + 195;
    var l4x = p.ox + 285, l4y = p.oy + 195;
    dl(g, px2, py2, m1x, m1y, 0);
    dl(g, px2, py2, m2x, m2y, 0);
    dld(g, m1x, m1y, l1x, l1y, 0);
    dld(g, m1x, m1y, l2x, l2y, 0);
    dld(g, m2x, m2y, l3x, l3y, 0);
    dld(g, m2x, m2y, l4x, l4y, 0);
    dn(g, px2, py2, 20, '#3b82f6', 'principal');
    dn(g, m1x, m1y, 16, '#a855f7', 'mid-1');
    dn(g, m2x, m2y, 16, '#a855f7', 'mid-2');
    dn(g, l1x, l1y, 11, '#64748b', '');
    dn(g, l2x, l2y, 11, '#64748b', '');
    dn(g, l3x, l3y, 11, '#64748b', '');
    dn(g, l4x, l4y, 11, '#64748b', '');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:px2,y:py2},{x:m1x,y:m1y}], '#dc2626', 100, 5);
      pkt(g, [{x:px2,y:py2},{x:m2x,y:m2y}], '#dc2626', 300, 5);
      timers.push(setTimeout(function() {
        pkt(g, [{x:m1x,y:m1y},{x:l1x,y:l1y}], '#dc2626', 0, 4);
        pkt(g, [{x:m2x,y:m2y},{x:l3x,y:l3y}], '#dc2626', 100, 4);
      }, 900));
      timers.push(setTimeout(function() {
        pkt(g, [{x:l1x,y:l1y},{x:m1x,y:m1y}], '#4ade80', 0, 4);
        pkt(g, [{x:l3x,y:l3y},{x:m2x,y:m2y}], '#4ade80', 100, 4);
      }, 1800));
      timers.push(setTimeout(function() {
        pkt(g, [{x:m1x,y:m1y},{x:px2,y:py2}], '#4ade80', 0, 5);
        pkt(g, [{x:m2x,y:m2y},{x:px2,y:py2}], '#4ade80', 200, 5);
      }, 2700));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 4200));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Goals cascade down, details stay opaque');
  }

  function drawVerificationGame(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW/2, cy = p.oy + 45 + (cellH-45)/2;
    var lx = p.ox + 90, ly = cy;
    var rx = p.ox + 250, ry = cy;
    dl(g, lx, ly, rx, ry, 0);
    dn(g, lx, ly, 22, '#3b82f6', 'prover');
    dn(g, rx, ry, 22, '#a855f7', 'verifier');
    var tick = 0;
    function run() {
      tick++;
      var lbl = g.append('text').attr('x', cx).attr('y', p.oy + 70).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',11).text('claim');
      pkt(g, [{x:lx,y:ly},{x:rx,y:ry}], '#dc2626', 100, 5);
      timers.push(setTimeout(function() {
        lbl.text('challenge');
        pkt(g, [{x:rx,y:ry},{x:lx,y:ly}], '#f97316', 0, 5);
      }, 1000));
      timers.push(setTimeout(function() {
        lbl.text('proof');
        pkt(g, [{x:lx,y:ly},{x:rx,y:ry}], '#3b82f6', 0, 5);
      }, 2000));
      timers.push(setTimeout(function() {
        lbl.text('verified');
        pkt(g, [{x:rx,y:ry},{x:lx,y:ly}], '#4ade80', 0, 5);
      }, 3000));
      timers.push(setTimeout(function() { lbl.remove(); if (tick < 80) run(); }, 4200));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Interactive proof: claim, challenge, respond');
  }

  function drawContractNet(p) {
    var g = svg.append('g');
    var mx = p.ox + 170, my = p.oy + 80;
    var w1x = p.ox + 70, w1y = p.oy + 185;
    var w2x = p.ox + 170, w2y = p.oy + 195;
    var w3x = p.ox + 270, w3y = p.oy + 185;
    dl(g, mx, my, w1x, w1y, 0);
    dl(g, mx, my, w2x, w2y, 0);
    dl(g, mx, my, w3x, w3y, 0);
    dn(g, mx, my, 20, '#3b82f6', 'manager');
    dn(g, w1x, w1y, 15, '#64748b', 'w1');
    dn(g, w2x, w2y, 15, '#64748b', 'w2');
    dn(g, w3x, w3y, 15, '#64748b', 'w3');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:mx,y:my},{x:w1x,y:w1y}], '#dc2626', 100, 4);
      pkt(g, [{x:mx,y:my},{x:w2x,y:w2y}], '#dc2626', 250, 4);
      pkt(g, [{x:mx,y:my},{x:w3x,y:w3y}], '#dc2626', 400, 4);
      timers.push(setTimeout(function() {
        pkt(g, [{x:w1x,y:w1y},{x:mx,y:my}], '#f97316', 0, 4);
        pkt(g, [{x:w2x,y:w2y},{x:mx,y:my}], '#f97316', 150, 4);
        pkt(g, [{x:w3x,y:w3y},{x:mx,y:my}], '#f97316', 300, 4);
      }, 1400));
      timers.push(setTimeout(function() {
        g.append('circle').attr('cx',w2x).attr('cy',w2y).attr('r',18).attr('fill','none')
          .attr('stroke','#facc15').attr('stroke-width',2.5).attr('opacity',0.9)
          .transition().duration(600).attr('opacity',0).remove();
        pkt(g, [{x:mx,y:my},{x:w2x,y:w2y}], '#4ade80', 0, 5);
      }, 2500));
      timers.push(setTimeout(function() {
        pkt(g, [{x:w2x,y:w2y},{x:mx,y:my}], '#4ade80', 0, 5);
      }, 3400));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 4800));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Broadcast task, collect bids, award winner');
  }

  function drawDistillation(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var tx2 = p.ox + 85, ty2 = cy;
    var sx = p.ox + 255, sy = cy;
    dl(g, tx2, ty2, sx, sy, 0);
    dn(g, tx2, ty2, 30, '#3b82f6', 'teacher');
    dn(g, sx, sy, 16, '#a855f7', 'student');
    var tick = 0;
    function run() {
      tick++;
      var taught = 0;
      function sendExample() {
        if (taught >= 3) {
          timers.push(setTimeout(function() {
            var xl1 = g.append('line').attr('x1', p.ox+140).attr('y1', cy-12).attr('x2', p.ox+200).attr('y2', cy+12)
              .attr('stroke','#dc2626').attr('stroke-width',2).attr('opacity',0);
            xl1.transition().duration(300).attr('opacity',1);
            var xl2 = g.append('line').attr('x1', p.ox+140).attr('y1', cy+12).attr('x2', p.ox+200).attr('y2', cy-12)
              .attr('stroke','#dc2626').attr('stroke-width',2).attr('opacity',0);
            xl2.transition().duration(300).attr('opacity',1);
            timers.push(setTimeout(function() {
              pkt(g, [{x:sx,y:sy},{x:p.ox+310,y:sy}], '#4ade80', 0, 5);
              timers.push(setTimeout(function() {
                xl1.remove(); xl2.remove();
                if (tick < 80) run();
              }, 1500));
            }, 700));
          }, 300));
          return;
        }
        pkt(g, [{x:tx2,y:ty2},{x:sx,y:sy}], '#dc2626', 0, 4);
        taught++;
        timers.push(setTimeout(sendExample, 700));
      }
      sendExample();
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Learn from large model, then operate alone');
  }

  function drawPrivilege(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var topx = p.ox + 170, topy = p.oy + 75;
    var midx = p.ox + 170, midy = p.oy + 140;
    var botx = p.ox + 170, boty = p.oy + 205;
    dl(g, topx, topy, midx, midy, 0);
    dl(g, midx, midy, botx, boty, 0);
    dn(g, topx, topy, 22, '#3b82f6', 'root');
    dn(g, midx, midy, 17, '#a855f7', 'delegate');
    dn(g, botx, boty, 12, '#64748b', 'leaf');
    var dots = ['#3b82f6','#a855f7','#f97316'];
    dots.forEach(function(c,i) { g.append('circle').attr('cx', topx+28+i*10).attr('cy', topy).attr('r',4).attr('fill',c); });
    dots.slice(0,2).forEach(function(c,i) { g.append('circle').attr('cx', midx+22+i*10).attr('cy', midy).attr('r',4).attr('fill',c); });
    dots.slice(0,1).forEach(function(c,i) { g.append('circle').attr('cx', botx+16+i*10).attr('cy', boty).attr('r',4).attr('fill',c); });
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:midx,y:midy},{x:botx,y:boty}], '#dc2626', 300, 5);
      timers.push(setTimeout(function() {
        var xl = g.append('text').attr('x', botx+20).attr('y', boty+4).attr('fill','#dc2626').attr('font-size',14).attr('font-weight','bold').text('✕');
        timers.push(setTimeout(function() { xl.remove(); if (tick < 80) run(); }, 1200));
      }, 1200));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Permissions narrow at each delegation level');
  }

  function drawFirebreaks(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var n1x = p.ox + 60, n2x = p.ox + 170, n3x = p.ox + 280;
    var fb1x = p.ox + 115, fb2x = p.ox + 225;
    dl(g, n1x, cy, n2x, cy, 0);
    dl(g, n2x, cy, n3x, cy, 0);
    dn(g, n1x, cy, 18, '#3b82f6', 'A');
    dn(g, n2x, cy, 18, '#a855f7', 'B');
    dn(g, n3x, cy, 18, '#f97316', 'C');
    g.append('line').attr('x1',fb1x).attr('y1',p.oy+55).attr('x2',fb1x).attr('y2',p.oy+220)
      .attr('stroke','#94a3b8').attr('stroke-width',1.5).attr('stroke-dasharray','5,4').attr('opacity',0.7);
    g.append('line').attr('x1',fb2x).attr('y1',p.oy+55).attr('x2',fb2x).attr('y2',p.oy+220)
      .attr('stroke','#94a3b8').attr('stroke-width',1.5).attr('stroke-dasharray','5,4').attr('opacity',0.7);
    g.append('text').attr('x',fb1x).attr('y',p.oy+62).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('boundary');
    g.append('text').attr('x',fb2x).attr('y',p.oy+62).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('boundary');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:n1x,y:cy},{x:fb1x,y:cy}], '#dc2626', 100, 5);
      timers.push(setTimeout(function() {
        pkt(g, [{x:fb1x,y:cy},{x:n2x,y:cy}], '#f97316', 0, 5);
      }, 700));
      timers.push(setTimeout(function() {
        pkt(g, [{x:n2x,y:cy},{x:fb2x,y:cy}], '#f97316', 0, 5);
      }, 1300));
      timers.push(setTimeout(function() {
        pkt(g, [{x:fb2x,y:cy},{x:n3x,y:cy}], '#facc15', 0, 5);
      }, 1900));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 3200));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Packets change at explicit trust boundaries');
  }

  function drawCredentials(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var ax = p.ox + 85, vx = p.ox + 255;
    dn(g, ax, cy, 20, '#3b82f6', 'agent');
    dn(g, vx, cy, 20, '#a855f7', 'verifier');
    var tick = 0;
    function run() {
      tick++;
      var badge = g.append('rect').attr('x', ax+22).attr('y', cy-6).attr('width',16).attr('height',12)
        .attr('fill','#facc15').attr('rx',2).attr('opacity',1);
      badge.transition().duration(800).attr('x', vx-30);
      timers.push(setTimeout(function() {
        badge.remove();
        g.append('circle').attr('cx',vx).attr('cy',cy).attr('r',24).attr('fill','none')
          .attr('stroke','#4ade80').attr('stroke-width',2.5).attr('opacity',1)
          .transition().duration(500).attr('r',30).attr('opacity',0).remove();
        var acc = g.append('line').attr('x1',ax).attr('y1',cy).attr('x2',vx).attr('y2',cy)
          .attr('stroke','#4ade80').attr('stroke-width',2).attr('opacity',0);
        acc.transition().duration(400).attr('opacity',1);
        timers.push(setTimeout(function() {
          acc.remove();
          if (tick < 80) run();
        }, 1500));
      }, 850));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Present credential, verifier grants access');
  }

  function drawZoneOfIndifference(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var ax = p.ox + 170;
    g.append('rect').attr('x', ax-45).attr('y', cy-40).attr('width',90).attr('height',80)
      .attr('fill','#3b82f6').attr('opacity',0.08).attr('rx',8);
    g.append('rect').attr('x', ax-45).attr('y', cy-40).attr('width',90).attr('height',80)
      .attr('fill','none').attr('stroke','#3b82f6').attr('stroke-dasharray','4,3').attr('stroke-width',1.2).attr('rx',8).attr('opacity',0.4);
    g.append('text').attr('x',ax).attr('y',cy-46).attr('text-anchor','middle').attr('fill','#3b82f6').attr('font-size',9).attr('opacity',0.7).text('zone of indifference');
    dn(g, ax, cy, 18, '#3b82f6', 'agent');
    var srcx = p.ox + 30, outx = p.ox + 310, abovey = p.oy + 85;
    var tick = 0;
    function run() {
      tick++;
      var lbl = g.append('text').attr('x',p.ox+cellW/2).attr('y',p.oy+68).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',11).text('inside zone');
      pkt(g, [{x:srcx,y:cy},{x:ax,y:cy}], '#dc2626', 100, 5);
      timers.push(setTimeout(function() {
        pkt(g, [{x:ax,y:cy},{x:outx,y:cy}], '#4ade80', 0, 5);
      }, 1100));
      timers.push(setTimeout(function() {
        lbl.text('outside zone');
        pkt(g, [{x:srcx,y:abovey},{x:ax,y:abovey}], '#dc2626', 0, 5);
      }, 2100));
      timers.push(setTimeout(function() {
        var q = g.append('text').attr('x',ax+22).attr('y',abovey+4).attr('fill','#f97316').attr('font-size',15).attr('font-weight','bold').text('?');
        timers.push(setTimeout(function() { q.remove(); lbl.remove(); if (tick < 80) run(); }, 1200));
      }, 2900));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Act freely within bounds, escalate outside');
  }

  function drawHumanGate(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var ax = p.ox + 55, qx = p.ox + 155, hx = p.ox + 260, ex = p.ox + 320;
    dn(g, ax, cy, 18, '#3b82f6', 'agent');
    g.append('rect').attr('x',qx-20).attr('y',cy-16).attr('width',40).attr('height',32)
      .attr('fill','#1e293b').attr('stroke','#64748b').attr('stroke-width',1.5).attr('rx',4);
    g.append('text').attr('x',qx).attr('y',cy+4).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',10).text('queue');
    g.append('circle').attr('cx',hx).attr('cy',cy-28).attr('r',9).attr('fill','#f97316');
    g.append('line').attr('x1',hx).attr('y1',cy-19).attr('x2',hx).attr('y2',cy+10).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx-10).attr('y1',cy-8).attr('x2',hx+10).attr('y2',cy-8).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx).attr('y1',cy+10).attr('x2',hx-8).attr('y2',cy+24).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx).attr('y1',cy+10).attr('x2',hx+8).attr('y2',cy+24).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('text').attr('x',hx).attr('y',cy+40).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('human');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:ax,y:cy},{x:qx,y:cy}], '#dc2626', 100, 5);
      timers.push(setTimeout(function() {
        var chk = g.append('text').attr('x',hx).attr('y',cy-42).attr('text-anchor','middle').attr('fill','#4ade80').attr('font-size',13).text('✓');
        timers.push(setTimeout(function() {
          chk.remove();
          pkt(g, [{x:qx,y:cy},{x:ex,y:cy}], '#4ade80', 100, 5);
        }, 600));
      }, 900));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 2600));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Human must approve before task proceeds');
  }

  function drawHumanOnLoop(p) {
    var g = svg.append('g');
    var a1x = p.ox + 95, a1y = p.oy + 110;
    var a2x = p.ox + 245, a2y = p.oy + 110;
    var hx = p.ox + 170, hy = p.oy + 195;
    dl(g, a1x, a1y, a2x, a2y, 0);
    dn(g, a1x, a1y, 18, '#3b82f6', 'A');
    dn(g, a2x, a2y, 18, '#a855f7', 'B');
    var head = g.append('circle').attr('cx',hx).attr('cy',hy-12).attr('r',8).attr('fill','#f97316');
    g.append('line').attr('x1',hx).attr('y1',hy-4).attr('x2',hx).attr('y2',hy+14).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx-9).attr('y1',hy+4).attr('x2',hx+9).attr('y2',hy+4).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('text').attr('x',hx).attr('y',hy+30).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('human');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:a1x,y:a1y},{x:a2x,y:a2y}], '#3b82f6', 0, 4);
      timers.push(setTimeout(function() {
        pkt(g, [{x:a2x,y:a2y},{x:a1x,y:a1y}], '#a855f7', 0, 4);
      }, 600));
      timers.push(setTimeout(function() {
        pkt(g, [{x:a1x,y:a1y},{x:a2x,y:a2y}], '#3b82f6', 0, 4);
      }, 1200));
      timers.push(setTimeout(function() {
        var alert = g.append('text').attr('x',a2x).attr('y',a2y-28).attr('text-anchor','middle').attr('fill','#dc2626').attr('font-size',16).attr('font-weight','bold').text('!');
        head.transition().duration(300).attr('fill','#facc15');
        timers.push(setTimeout(function() {
          pkt(g, [{x:hx,y:hy},{x:a2x,y:a2y}], '#4ade80', 0, 5);
          timers.push(setTimeout(function() {
            alert.remove();
            head.transition().duration(300).attr('fill','#f97316');
            if (tick < 80) run();
          }, 1000));
        }, 700));
      }, 1800));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Agents work autonomously, human intervenes on alert');
  }

  function drawPolicyDelegation(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var hx = p.ox + 50, hy = p.oy + 85;
    var ax = p.ox + 170;
    var d1x = p.ox + 280, d1y = p.oy + 100;
    var d2x = p.ox + 290, d2y = cy;
    var d3x = p.ox + 280, d3y = p.oy + 175;
    g.append('circle').attr('cx',hx).attr('cy',hy-12).attr('r',8).attr('fill','#f97316');
    g.append('line').attr('x1',hx).attr('y1',hy-4).attr('x2',hx).attr('y2',hy+14).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx-9).attr('y1',hy+4).attr('x2',hx+9).attr('y2',hy+4).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('rect').attr('x',ax-24).attr('y',cy-42).attr('width',48).attr('height',26)
      .attr('fill','#1e293b').attr('stroke','#64748b').attr('stroke-width',1.2).attr('rx',3);
    [0,1,2].forEach(function(i){ g.append('line').attr('x1',ax-16).attr('y1',cy-36+i*8).attr('x2',ax+16).attr('y2',cy-36+i*8).attr('stroke','#64748b').attr('stroke-width',1); });
    g.append('text').attr('x',ax).attr('y',cy-46).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',8).text('policy');
    dl(g, ax, cy+8, d1x, d1y, 0);
    dl(g, ax, cy+8, d2x, d2y, 0);
    dl(g, ax, cy+8, d3x, d3y, 0);
    dn(g, ax, cy+8, 18, '#3b82f6', 'agent');
    dn(g, d1x, d1y, 12, '#64748b', '');
    dn(g, d2x, d2y, 12, '#64748b', '');
    dn(g, d3x, d3y, 12, '#64748b', '');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:hx,y:hy},{x:ax,y:cy+8}], '#f97316', 100, 5);
      timers.push(setTimeout(function() {
        pkt(g, [{x:ax,y:cy+8},{x:d1x,y:d1y}], '#dc2626', 100, 4);
        pkt(g, [{x:ax,y:cy+8},{x:d2x,y:d2y}], '#dc2626', 350, 4);
        pkt(g, [{x:ax,y:cy+8},{x:d3x,y:d3y}], '#dc2626', 600, 4);
      }, 1000));
      timers.push(setTimeout(function() { if (tick < 80) run(); }, 3000));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Human defines rules, agent executes freely');
  }

  function drawApprovalEscalation(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var lx = p.ox + 30, rx = p.ox + 310;
    var ax = p.ox + 170;
    var hx = p.ox + 230, hy = p.oy + 75;
    dl(g, lx, cy, ax-20, cy, 0);
    dl(g, ax+20, cy, rx, cy, 0);
    dn(g, ax, cy, 20, '#3b82f6', 'agent');
    g.append('circle').attr('cx',hx).attr('cy',hy-12).attr('r',8).attr('fill','#f97316');
    g.append('line').attr('x1',hx).attr('y1',hy-4).attr('x2',hx).attr('y2',hy+10).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('line').attr('x1',hx-9).attr('y1',hy+2).attr('x2',hx+9).attr('y2',hy+2).attr('stroke','#f97316').attr('stroke-width',2);
    g.append('text').attr('x',hx).attr('y',hy+26).attr('text-anchor','middle').attr('fill','#94a3b8').attr('font-size',9).text('approver');
    var tick = 0;
    function run() {
      tick++;
      pkt(g, [{x:lx,y:cy},{x:rx,y:cy}], '#4ade80', 100, 4);
      pkt(g, [{x:lx,y:cy},{x:rx,y:cy}], '#4ade80', 700, 4);
      timers.push(setTimeout(function() {
        pkt(g, [{x:lx,y:cy},{x:ax,y:cy}], '#facc15', 0, 5);
        timers.push(setTimeout(function() {
          pkt(g, [{x:ax,y:cy},{x:hx,y:hy}], '#dc2626', 0, 4);
          timers.push(setTimeout(function() {
            pkt(g, [{x:hx,y:hy},{x:ax,y:cy}], '#4ade80', 0, 5);
            timers.push(setTimeout(function() {
              pkt(g, [{x:ax,y:cy},{x:rx,y:cy}], '#4ade80', 0, 5);
              timers.push(setTimeout(function() { if (tick < 80) run(); }, 1000));
            }, 500));
          }, 800));
        }, 500));
      }, 1500));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Most pass through; exceptions route to human');
  }

  function drawAdaptive(p) {
    var g = svg.append('g');
    var cy = p.oy + 45 + (cellH-45)/2;
    var rx2 = p.ox + 90, ry2 = cy;
    var ax2 = p.ox + 240, ay2 = p.oy + 100;
    var bx = p.ox + 240, by = p.oy + 175;
    dl(g, rx2, ry2, ax2, ay2, 0);
    dl(g, rx2, ry2, bx, by, 0);
    dn(g, rx2, ry2, 20, '#3b82f6', 'router');
    var nodeA = g.append('circle').attr('cx',ax2).attr('cy',ay2).attr('r',16).attr('fill','#a855f7').attr('opacity',0.85);
    g.append('text').attr('x',ax2).attr('y',ay2+28).attr('text-anchor','middle').attr('font-size','7px').attr('fill','#888').text('A');
    dn(g, bx, by, 16, '#f97316', 'B');
    var tick = 0;
    function run() {
      tick++;
      nodeA.transition().duration(0).attr('opacity',0.85);
      pkt(g, [{x:rx2,y:ry2},{x:ax2,y:ay2}], '#dc2626', 100, 4);
      pkt(g, [{x:rx2,y:ry2},{x:bx,y:by}], '#dc2626', 400, 4);
      timers.push(setTimeout(function() {
        nodeA.transition().duration(600).attr('opacity',0.15);
        var cx2 = p.ox + 250, cy2 = p.oy + 90;
        var nodeC = g.append('circle').attr('cx',cx2).attr('cy',cy2).attr('r',0).attr('fill','#4ade80').attr('opacity',0.85);
        nodeC.transition().duration(400).attr('r',16);
        var clbl = g.append('text').attr('x',cx2).attr('y',cy2+28).attr('text-anchor','middle').attr('font-size','7px').attr('fill','#888').attr('opacity',0).text('C');
        clbl.transition().duration(400).attr('opacity',1);
        timers.push(setTimeout(function() {
          pkt(g, [{x:rx2,y:ry2},{x:cx2,y:cy2}], '#dc2626', 100, 4);
          pkt(g, [{x:rx2,y:ry2},{x:bx,y:by}], '#dc2626', 400, 4);
          timers.push(setTimeout(function() {
            nodeC.transition().duration(300).attr('opacity',0).on('end',function(){nodeC.remove();});
            clbl.transition().duration(300).attr('opacity',0).on('end',function(){clbl.remove();});
            if (tick < 80) run();
          }, 1500));
        }, 600));
      }, 2000));
    }
    timers.push(setTimeout(run, 800));
    anno(g, p, 'Delegation structure morphs in real-time');
  }

  // Assign draw functions in family order matching panelDefs
  drawFns = [
    // Sequential (6)
    drawChain, drawPipeline, drawRouter, drawEscalation, drawRelay, drawLoop,
    // Hierarchical (6)
    drawTree, drawMapReduce, drawSupervisor, drawOrchestrator, drawMissionCommand, drawFeudal,
    // Quality & Verification (6)
    drawEvaluator, drawVoting, drawCritic, drawDebate, drawWitness, drawVerificationGame,
    // Reliability (4)
    drawCircuitBreaker, drawTimeout, drawCheckpoint, drawCanary,
    // Market & Competition (4)
    drawAuction, drawNegotiation, drawSpeculative, drawContractNet,
    // Knowledge Transfer (4)
    drawTeacherStudent, drawFederatedLearning, drawBlackboard, drawDistillation,
    // Emergent Coordination (5)
    drawPubSub, drawChoreography, drawStigmergy, drawSwarm, drawGossip,
    // Trust & Authority (4)
    drawPrivilege, drawFirebreaks, drawCredentials, drawZoneOfIndifference,
    // Human-Agent Interface (4)
    drawHumanGate, drawHumanOnLoop, drawPolicyDelegation, drawApprovalEscalation,
    // Meta-Pattern (1)
    drawAdaptive
  ];
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
    {label: 'Bidding Pipeline', ox: (cellW + gap) * 2 + gap, oy: 0}
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
  function drawQualityGate(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var input = {x: p.ox + 25, y: ty + 75};
    var router = {x: cx - 45, y: ty + 75};
    var cheap = {x: cx + 10, y: ty + 25};
    var big = {x: cx + 10, y: ty + 130};
    var evaluator = {x: cx + 70, y: ty + 75};
    var output = {x: p.ox + cellW - 25, y: ty + 75};
    [[input, router], [router, cheap], [router, big], [cheap, evaluator], [big, evaluator], [evaluator, output]].forEach(function(l) {
      g.append('line').attr('x1', l[0].x).attr('y1', l[0].y).attr('x2', l[1].x).attr('y2', l[1].y)
        .attr('stroke', '#aaa').attr('stroke-width', 1.5);
    });
    g.append('line').attr('x1', cheap.x).attr('y1', cheap.y + 9).attr('x2', big.x).attr('y2', big.y - 12)
      .attr('stroke', '#ef4444').attr('stroke-width', 1).attr('stroke-dasharray', '4,3');
    g.append('text').attr('x', cheap.x + 16).attr('y', (cheap.y + big.y) / 2 + 3)
      .attr('font-size', '7px').attr('fill', '#bbb').text('escalate');
    g.append('path').attr('d', 'M ' + (evaluator.x + 7) + ' ' + (evaluator.y - 10) +
      ' A 14 14 0 1 1 ' + (evaluator.x + 7) + ' ' + (evaluator.y + 10))
      .attr('fill', 'none').attr('stroke', '#d97706').attr('stroke-width', 1).attr('stroke-dasharray', '3,2');
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
      var useBig = Math.random() < 0.3;
      var path1 = useBig ? [input, router, big, evaluator] : [input, router, cheap, evaluator];
      var d1 = pkt(g, path1, useBig ? '#7f1d1d' : '#ef4444', 0, 3.5);
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
      if (tick < 80) timers.push(setTimeout(anim, loopDelay + 800));
    }
    timers.push(setTimeout(anim, 1200));
    g.append('text').attr('x', p.ox + cellW / 2).attr('y', height - 14)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#bbb')
      .attr('font-style', 'italic').text('Router + Escalation + Evaluator');
  }
  function drawConsensus(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 50;
    var disp = {x: p.ox + 30, y: ty + 85};
    var merge = {x: p.ox + cellW - 30, y: ty + 85};
    var subProblems = [];
    for (var s = 0; s < 3; s++) {
      var sy = ty + 20 + s * 60;
      var voters = [];
      for (var v = 0; v < 2; v++) {
        voters.push({x: cx - 15 + v * 35, y: sy});
      }
      subProblems.push({y: sy, voters: voters, verdict: {x: cx + 65, y: sy}});
    }
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
      subProblems.forEach(function(sp, si) {
        sp.voters.forEach(function(v, vi) {
          pkt(g, [disp, v], '#dc2626', si * 80 + vi * 40, 3);
        });
      });
      var voteDelay = 3 * 80 + 2 * 40 + 500;
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
      var mergeDelay = voteDelay + 3 * 100 + 2 * 50 + 500;
      subProblems.forEach(function(sp, si) {
        pkt(g, [sp.verdict, merge], '#059669', mergeDelay + si * 80, 3.5);
      });
      if (tick < 80) timers.push(setTimeout(anim, mergeDelay + 3 * 80 + 800));
    }
    timers.push(setTimeout(anim, 1200));
    g.append('text').attr('x', p.ox + cellW / 2).attr('y', height - 14)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#bbb')
      .attr('font-style', 'italic').text('Map-Reduce + Voting');
  }
  function drawBiddingPipeline(p) {
    var g = svg.append('g');
    var cx = p.ox + cellW / 2, ty = p.oy + 55;
    var stageNames = ['Ingest', 'Parse', 'Enrich', 'Store'];
    var stageX = [];
    var stageGap = 48;
    for (var i = 0; i < 4; i++) stageX.push(cx - 1.5 * stageGap + i * stageGap);
    var stageY = ty + 40;
    var bidders = [];
    var bidColors = ['#2563eb', '#7c3aed', '#059669', '#d97706', '#dc2626'];
    for (var i = 0; i < 5; i++) {
      bidders.push({x: cx - 70 + i * 35, y: ty + 190});
    }
    bidders.forEach(function(b) {
      stageX.forEach(function(sx) {
        g.append('line').attr('x1', b.x).attr('y1', b.y - 8).attr('x2', sx).attr('y2', stageY + 14)
          .attr('stroke', '#bbb').attr('stroke-width', 0.9).attr('stroke-dasharray', '3,3');
      });
    });
    for (var i = 1; i < 4; i++) {
      g.append('line').attr('x1', stageX[i-1] + 22).attr('y1', stageY)
        .attr('x2', stageX[i] - 22).attr('y2', stageY)
        .attr('stroke', '#999').attr('stroke-width', 1.5);
    }
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
    for (var i = 0; i < 5; i++) {
      g.append('circle').attr('cx', bidders[i].x).attr('cy', bidders[i].y).attr('r', 8)
        .attr('fill', bidColors[i]).attr('stroke', 'white').attr('stroke-width', 0.8).attr('opacity', 0.8);
      g.append('text').attr('x', bidders[i].x).attr('y', bidders[i].y + 18)
        .attr('text-anchor', 'middle').attr('font-size', '7px').attr('fill', '#888').text('Agent ' + (i+1));
    }
    var tick = 0;
    function anim() {
      tick++;
      stageNames.forEach(function(name, si) {
        var delay = si * 600;
        var bids = bidders.map(function() { return Math.floor(Math.random() * 90) + 10; });
        var winner = bids.indexOf(Math.max.apply(null, bids));
        bidders.forEach(function(b, bi) {
          pkt(g, [b, {x: stageX[si], y: stageY + 14}], bidColors[bi], delay + bi * 40, 2.5);
        });
        (function(sidx, w) {
          timers.push(setTimeout(function() {
            stageEls[sidx].transition().duration(200).attr('r', 6).attr('fill', bidColors[w]);
            g.append('circle').attr('cx', bidders[w].x).attr('cy', bidders[w].y).attr('r', 8)
              .attr('fill', 'none').attr('stroke', bidColors[w]).attr('stroke-width', 2).attr('opacity', 0.8)
              .transition().duration(400).attr('r', 14).attr('opacity', 0).remove();
          }, delay + 5 * 40 + 300));
        })(si, winner);
      });
      var flowDelay = 4 * 600 + 600;
      var pts = stageX.map(function(sx) { return {x: sx, y: stageY}; });
      pkt(g, pts, '#059669', flowDelay, 3.5);
      timers.push(setTimeout(function() {
        stageEls.forEach(function(el) { el.transition().duration(300).attr('r', 0); });
      }, flowDelay + pts.length * 220 + 400));
      if (tick < 80) timers.push(setTimeout(anim, flowDelay + pts.length * 220 + 800));
    }
    timers.push(setTimeout(anim, 1200));
    g.append('text').attr('x', p.ox + cellW / 2).attr('y', height - 14)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#bbb')
      .attr('font-style', 'italic').text('Pipeline + Auction');
  }
  var autoRestart;
  function initAndSchedule() {
    if (autoRestart) clearTimeout(autoRestart);
    init();
    autoRestart = setTimeout(initAndSchedule, 18000);
  }
  initAndSchedule();
  svg.on('click', initAndSchedule);
})();

// ============================================================
// Visualization: Mixture of Specialists
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
  var routers = [
    { id: 'Assistant', x: 80, y: 90, color: '#2563eb', r: 20 },
    { id: 'Copilot', x: 80, y: 180, color: '#7c3aed', r: 18 },
    { id: 'Search', x: 80, y: 270, color: '#0891b2', r: 16 },
    { id: 'Pipeline', x: 80, y: 350, color: '#374151', r: 15 }
  ];
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
  var denied = [
    { src: 'Search', tgt: 'Code LLM' },
    { src: 'Search', tgt: 'Code KB' },
    { src: 'Pipeline', tgt: 'Memory' },
    { src: 'Assistant', tgt: 'Code KB' }
  ];
  var allNodes = routers.concat(market);
  var nodeMap = {};
  allNodes.forEach(function(n) { nodeMap[n.id] = n; });
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
  var legY = height - 8;
  [['Agent', '#b91c1c', 'circle'], ['Tool', '#059669', 'rect'], ['Knowledge Base', '#6366f1', 'diamond']].forEach(function(l, i) {
    var lx = 250 + i * 160;
    if (l[2] === 'circle') nodeLayer.append('circle').attr('cx', lx).attr('cy', legY - 3).attr('r', 4).attr('fill', l[1]).attr('opacity', 0.6);
    else if (l[2] === 'rect') nodeLayer.append('rect').attr('x', lx - 4).attr('y', legY - 7).attr('width', 8).attr('height', 8).attr('rx', 1.5).attr('fill', l[1]).attr('opacity', 0.6);
    else nodeLayer.append('polygon').attr('points', (lx) + ',' + (legY - 8) + ' ' + (lx + 5) + ',' + (legY - 3) + ' ' + (lx) + ',' + (legY + 2) + ' ' + (lx - 5) + ',' + (legY - 3))
      .attr('fill', l[1]).attr('opacity', 0.6);
    nodeLayer.append('text').attr('x', lx + 8).attr('y', legY).attr('font-size', '8px').attr('fill', '#888').text(l[0]);
  });
  edges.forEach(function(e) {
    var s = nodeMap[e.src], t = nodeMap[e.tgt];
    linkLayer.append('line').attr('x1', s.x).attr('y1', s.y).attr('x2', t.x).attr('y2', t.y)
      .attr('stroke', '#bbb').attr('stroke-width', Math.max(0.5, e.w * 0.35)).attr('opacity', 0.35);
  });
  denied.forEach(function(e) {
    var s = nodeMap[e.src], t = nodeMap[e.tgt];
    linkLayer.append('line').attr('x1', s.x).attr('y1', s.y).attr('x2', t.x).attr('y2', t.y)
      .attr('stroke', '#ef4444').attr('stroke-width', 0.8).attr('opacity', 0.25)
      .attr('stroke-dasharray', '4,4');
    var mx = (s.x + t.x) / 2, my = (s.y + t.y) / 2;
    linkLayer.append('text').attr('x', mx).attr('y', my + 3)
      .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#ef4444').attr('opacity', 0.5).text('\u{1F512}');
  });
  function drawNode(n, isRouter) {
    var g = nodeLayer.append('g').style('cursor', 'pointer');
    if (isRouter) {
      g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', n.r)
        .attr('fill', n.color).attr('opacity', 0.2).attr('stroke', n.color).attr('stroke-width', 2);
      g.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', n.r * 0.5)
        .attr('fill', n.color).attr('opacity', 0.6);
    } else if (n.type === 'tool') {
      g.append('rect').attr('x', n.x - n.r).attr('y', n.y - n.r * 0.7).attr('width', n.r * 2).attr('height', n.r * 1.4)
        .attr('rx', 4).attr('fill', n.color).attr('opacity', 0.12).attr('stroke', n.color).attr('stroke-width', 1.5);
    } else if (n.type === 'kb') {
      var pts = (n.x) + ',' + (n.y - n.r) + ' ' + (n.x + n.r) + ',' + (n.y) + ' ' + (n.x) + ',' + (n.y + n.r) + ' ' + (n.x - n.r) + ',' + (n.y);
      g.append('polygon').attr('points', pts)
        .attr('fill', n.color).attr('opacity', 0.12).attr('stroke', n.color).attr('stroke-width', 1.5);
    } else {
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
// Visualization: Governance Archetypes (2x11 grid)
// ============================================================
(function() {
  var container = document.getElementById('viz-society');
  if (!container) return;
  var width = 720, height = 2790;
  var svg = d3.select(container).append('svg')
    .attr('viewBox', '0 0 ' + width + ' ' + height)
    .style('cursor', 'pointer');

  var cellW = 350, cellH = 240, padX = 5, padY = 10;
  var cols = 2, rows = 11;

  var archetypes = [
    {name:'Autocracy',               color:'#b91c1c', sub:'Hub-spoke \u00B7 One commands all'},
    {name:'Doctrine',                color:'#854d0e', sub:'The Codex \u00B7 The law is sovereign'},
    {name:'Liquid Democracy',        color:'#be185d', sub:'The Current \u00B7 Trust flows downhill'},
    {name:'Guild',                   color:'#059669', sub:'The Forge \u00B7 Specialists + liaisons'},
    {name:'Market',                  color:'#d97706', sub:'Dynamic \u00B7 Reputation buys influence'},
    {name:'Oligarchy',               color:'#7e22ce', sub:'The Tribunal \u00B7 The few decide'},
    {name:'Meritocracy',             color:'#ea580c', sub:'The Arena \u00B7 Prove worth, climb ranks'},
    {name:'Panopticon',              color:'#78716c', sub:'The Watchtower \u00B7 The monitor sees all'},
    {name:'The Agora',               color:'#2563eb', sub:'Debate \u00B7 All voices, majority rules'},
    {name:'Zero-Trust Mesh',         color:'#374151', sub:'Full mesh \u00B7 Every link verified'},
    {name:'Federation',              color:'#0369a1', sub:'The Accord \u00B7 Shared protocol layer'},
    {name:'Colony',                  color:'#7c3aed', sub:'Swarm \u00B7 Norms from interaction'},
    {name:'Stewardship',             color:'#166534', sub:'The Commons \u00B7 Collective resource governance'},
    {name:'Custodianship',           color:'#9f1239', sub:'The Trust \u00B7 Fiduciary to principal'},
    {name:'Constitutional Republic', color:'#1e3a5f', sub:'Trias Politica \u00B7 Separated powers'},
    {name:'Franchise',               color:'#9a3412', sub:'The Platform \u00B7 Comply or leave'},
    {name:'Open-Source',             color:'#4338ca', sub:'The Fork \u00B7 Community + merge authority'},
    {name:'Sortition',               color:'#0f766e', sub:'The Lottery \u00B7 Random selection, no gaming'},
    {name:'Adhocracy',               color:'#c2410c', sub:'The Sprint \u00B7 Temporary problem-scoped teams'},
    {name:'Mission Command',         color:'#3f6212', sub:'The Intent \u00B7 Govern by purpose, not method'},
    {name:'Mechanism Design',        color:'#7c2d12', sub:'The Game \u00B7 Incentive-compatible rules'},
    {name:'Immune System',           color:'#881337', sub:'The Shield \u00B7 Layered defense with tolerance'}
  ];

  // drawFns[i] draws archetype i into targetSvg at offset (ox, oy), pushing timers into targetTimers
  var drawFns = [];

  function addDefsToSvg(targetSvg) {
    var d = targetSvg.append('defs');
    d.append('marker').attr('id','soc-arr').attr('viewBox','0 0 8 8')
      .attr('refX',7).attr('refY',4).attr('markerWidth',5).attr('markerHeight',5).attr('orient','auto')
      .append('path').attr('d','M 0 0 L 8 4 L 0 8 Z').attr('fill','#ccc');
  }
  addDefsToSvg(svg);

  var animTimers = [];

  // Build the drawFns array - one function per archetype
  // Each fn(params) draws into params.targetSvg at params.ox/oy, pushes to params.timers
  // ztState.running is per-invocation, passed via params.ztState = {running: bool}

  archetypes.forEach(function(arch, idx) {
    drawFns.push(function(params) {
      var targetSvg = params.targetSvg;
      var timers = params.timers;
      var ox = params.ox, oy = params.oy;
      var ztState = params.ztState || {running: true};

      var g = targetSvg.append('g').attr('class', 'cell');

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
      if (arch.name === 'Autocracy') {
        var nLeaf = 8;
        // Packet layer first, then links + leaves, then hub last (bg -> links -> pkt -> nodes)
        var pulseG = g.append('g');
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
        // Hub drawn last so it sits on top of its spokes
        g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 10)
          .attr('fill', arch.color).attr('opacity', 0.25).attr('stroke', arch.color).attr('stroke-width', 2);
        g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '7px').attr('font-weight', 'bold').attr('fill', arch.color).text('O');
        timers.push(setInterval(function() {
          var tgt = leaves[Math.floor(Math.random() * nLeaf)];
          pulseG.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 2.5)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(500).attr('cx', tgt.x).attr('cy', tgt.y)
            .transition().duration(300).attr('opacity', 0).remove();
        }, 600));
      }

      // ---- ZERO-TRUST MESH: challenge-response protocol ----
      if (arch.name === 'Zero-Trust Mesh') {
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
          if (!ztState.running) return;
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
            if (!ztState.running) return;
            // 2. CHALLENGE: B sends credential challenge back
            meshCircles[bi].transition().duration(150).attr('r', 7).attr('stroke', '#f59e0b')
              .transition().duration(300).attr('r', 5).attr('stroke', arch.color);
            ztLabel(mx, my - 8, 'credential?', '#f59e0b', 500);
            ztPacket(B, A, '#f59e0b', 2.5, 500, function() {
              if (!ztState.running) return;
              // 3. PROVE: A sends proof to B
              ztLabel(mx, my - 8, 'proof', '#a78bfa', 600);
              ztPacket(A, B, '#a78bfa', 4, 600, function() {
                if (!ztState.running) return;
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
                    if (!ztState.running) return;
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
                  setTimeout(function() { if (ztState.running) runZtCycle(); }, 800);
                }
              });
            });
          });
        }

        timers.push(setTimeout(runZtCycle, 800));
      }

      // ---- SENATE: ring with debate arcs ----
      if (arch.name === 'The Agora') {
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
        timers.push(setInterval(function() {
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
      if (arch.name === 'Market') {
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
        timers.push(setInterval(function() {
          mktNodes.forEach(function(n) {
            n.rep = Math.max(0.1, Math.min(1, n.rep + (Math.random() - 0.48) * 0.15));
          });
          drawMarket();
        }, 1200));
      }

      // ---- GUILD: 3 tight clusters + liaison bridges ----
      if (arch.name === 'Guild') {
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
        // Packet layer sits behind liaison nodes
        var liaisonG = g.append('g');
        liaisons.forEach(function(l) {
          // Dashed lines to guild centers first, then the liaison node on top
          [l.from, l.to].forEach(function(gi) {
            g.append('line').attr('x1', l.x).attr('y1', l.y)
              .attr('x2', guildCenters[gi].x).attr('y2', guildCenters[gi].y)
              .attr('stroke', '#aaa').attr('stroke-width', 1.2).attr('stroke-dasharray', '3,3').attr('opacity', 0.3);
          });
          g.append('circle').attr('cx', l.x).attr('cy', l.y).attr('r', 3.5)
            .attr('fill', '#374151').attr('opacity', 0.6);
        });
        // Animate: messages flow through liaisons
        timers.push(setInterval(function() {
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
      if (arch.name === 'Colony') {
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
        var colLinkG = g.append('g'); // ephemeral links, kept behind nodes
        var colNodeG = g.append('g');
        var coreEls = cores.map(function(c) {
          return colNodeG.append('circle').attr('cx', c.x).attr('cy', c.y).attr('r', 5)
            .attr('fill', c.color).attr('opacity', 0.8).attr('stroke', 'white').attr('stroke-width', 1);
        });
        var followerEls = colNodeG.selectAll('.follower').data(colNodes).enter().append('circle')
          .attr('class', 'follower').attr('r', 3).attr('opacity', 0.55);

        var isVisible = true;
        var colObs = null;
        if ('IntersectionObserver' in window) {
          colObs = new IntersectionObserver(function(e) { isVisible = e[0].isIntersecting; }, { threshold: 0 });
          colObs.observe(targetSvg.node());
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

          // Ephemeral links (same home, nearby) rendered behind nodes
          colLinkG.selectAll('line').remove();
          for (var i = 0; i < nFollowers; i++) for (var j = i + 1; j < nFollowers; j++) {
            if (colNodes[i].home !== colNodes[j].home) continue;
            var dx = colNodes[i].x - colNodes[j].x, dy = colNodes[i].y - colNodes[j].y;
            if (Math.sqrt(dx*dx + dy*dy) < 22) {
              colLinkG.append('line')
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
        timers.push({clear: function() { cancelAnimationFrame(colFrame); if (colObs) colObs.disconnect(); }});
      }

      // ---- FEDERATION: sub-groups + shared protocol band ----
      if (arch.name === 'Federation') {
        var fedColors = ['#0369a1','#0284c7','#0ea5e9'];
        var fedGroups = [
          {x: acx - R*0.65, y: acy - R*0.35, n: 5},
          {x: acx + R*0.65, y: acy - R*0.35, n: 4},
          {x: acx, y: acy + R*0.45, n: 5}
        ];
        // Protocol band
        var bandY = acy, bandH = 14;
        g.append('rect').attr('x', ox+20).attr('y', bandY-bandH/2).attr('width', cellW-40).attr('height', bandH)
          .attr('rx', 4).attr('fill', arch.color).attr('fill-opacity', 0.08)
          .attr('stroke', arch.color).attr('stroke-width', 1).attr('stroke-dasharray', '6,3').attr('opacity', 0.25);
        g.append('text').attr('x', acx).attr('y', bandY+3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.4).text('shared protocol');
        var fedAllNodes = [];
        fedGroups.forEach(function(fg, gi) {
          g.append('circle').attr('cx', fg.x).attr('cy', fg.y).attr('r', 32)
            .attr('fill', 'none').attr('stroke', fedColors[gi]).attr('stroke-width', 1)
            .attr('stroke-dasharray', '4,3').attr('opacity', 0.25);
          for (var i=0; i<fg.n; i++) {
            var a = (i/fg.n)*Math.PI*2 - Math.PI/2;
            var nx = fg.x+Math.cos(a)*18, ny = fg.y+Math.sin(a)*18;
            g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', 4)
              .attr('fill', fedColors[gi]).attr('opacity', 0.5).attr('stroke', fedColors[gi]).attr('stroke-width', 1);
            fedAllNodes.push({x:nx, y:ny, gx:fg.x, gy:fg.y, color:fedColors[gi]});
          }
        });
        var fedPktG = g.append('g');
        var fedEpoch = 0;
        timers.push(setInterval(function() {
          fedEpoch++;
          // Delegate travels to protocol band
          var n = fedAllNodes[Math.floor(Math.random()*fedAllNodes.length)];
          fedPktG.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 2.5)
            .attr('fill', n.color).attr('opacity', 0.8)
            .transition().duration(400).attr('cx', n.x).attr('cy', bandY)
            .transition().duration(150).attr('r', 4).attr('opacity', 0.5)
            .transition().duration(400).attr('cx', n.x).attr('cy', n.y).attr('r', 2.5)
            .transition().duration(200).attr('opacity', 0).remove();
          // Shared policy pulse every 6 cycles
          if (fedEpoch % 6 === 0) {
            fedPktG.append('rect').attr('x', ox+20).attr('y', bandY-bandH/2).attr('width', cellW-40).attr('height', bandH)
              .attr('rx', 4).attr('fill', arch.color).attr('opacity', 0.3)
              .transition().duration(800).attr('opacity', 0).remove();
          }
        }, 700));
      }

      // ---- MERITOCRACY: concentric tiers ----
      if (arch.name === 'Meritocracy') {
        var tiers = [{r: R*0.3, n:2, sz:7, op:0.8}, {r: R*0.6, n:5, sz:5, op:0.55}, {r: R*0.9, n:8, sz:3.5, op:0.35}];
        var arenaNodes = [];
        tiers.forEach(function(t, ti) {
          g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', t.r)
            .attr('fill', 'none').attr('stroke', arch.color).attr('stroke-width', 0.8)
            .attr('stroke-dasharray', '3,3').attr('opacity', 0.15);
          for (var i=0; i<t.n; i++) {
            var a = (i/t.n)*Math.PI*2 - Math.PI/2 + ti*0.3;
            var nx = acx+Math.cos(a)*t.r, ny = acy+Math.sin(a)*t.r;
            var el = g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', t.sz)
              .attr('fill', arch.color).attr('opacity', t.op).attr('stroke', arch.color).attr('stroke-width', 1);
            arenaNodes.push({x:nx, y:ny, tier:ti, el:el, a:a, sz:t.sz});
          }
        });
        // Labels
        g.append('text').attr('x', acx).attr('y', acy-tiers[0].r-8).attr('text-anchor', 'middle')
          .attr('font-size', '5px').attr('fill', '#999').text('elite');
        g.append('text').attr('x', acx).attr('y', acy+tiers[2].r+12).attr('text-anchor', 'middle')
          .attr('font-size', '5px').attr('fill', '#999').text('novice');
        var arenaPktG = g.append('g');
        timers.push(setInterval(function() {
          // Pick two from adjacent tiers
          var t1 = Math.floor(Math.random()*2);
          var pool1 = arenaNodes.filter(function(n){return n.tier===t1;});
          var pool2 = arenaNodes.filter(function(n){return n.tier===t1+1;});
          if (!pool1.length || !pool2.length) return;
          var a = pool1[Math.floor(Math.random()*pool1.length)];
          var b = pool2[Math.floor(Math.random()*pool2.length)];
          var mx = (a.x+b.x)/2, my = (a.y+b.y)/2;
          // Compete
          arenaPktG.append('circle').attr('cx', a.x).attr('cy', a.y).attr('r', 2)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(300).attr('cx', mx).attr('cy', my)
            .transition().duration(200).attr('opacity', 0).remove();
          arenaPktG.append('circle').attr('cx', b.x).attr('cy', b.y).attr('r', 2)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(300).attr('cx', mx).attr('cy', my)
            .transition().duration(200).attr('opacity', 0).remove();
          // Result: winner climbs (green flash), loser falls (red flash)
          var winA = Math.random() > 0.5;
          setTimeout(function() {
            (winA?a:b).el.transition().duration(200).attr('fill', '#4ade80').attr('r', (winA?a:b).sz+2)
              .transition().duration(400).attr('fill', arch.color).attr('r', (winA?a:b).sz);
            (winA?b:a).el.transition().duration(200).attr('fill', '#ef4444').attr('r', (winA?b:a).sz-1)
              .transition().duration(400).attr('fill', arch.color).attr('r', (winA?b:a).sz);
          }, 350);
        }, 1500));
      }

      // ---- PANOPTICON: central eye + scanned agents ----
      if (arch.name === 'Panopticon') {
        var eyeX = acx, eyeY = areaTop + 20;
        // Eye
        g.append('circle').attr('cx', eyeX).attr('cy', eyeY).attr('r', 12)
          .attr('fill', arch.color).attr('opacity', 0.15).attr('stroke', arch.color).attr('stroke-width', 2);
        g.append('circle').attr('cx', eyeX).attr('cy', eyeY).attr('r', 5)
          .attr('fill', arch.color).attr('opacity', 0.6);
        g.append('circle').attr('cx', eyeX).attr('cy', eyeY).attr('r', 2)
          .attr('fill', 'white').attr('opacity', 0.8);
        // Scanning beam drawn before the agent nodes so it stays behind them
        var beamEl = g.append('line').attr('x1', eyeX).attr('y1', eyeY)
          .attr('x2', eyeX).attr('y2', eyeY).attr('stroke', '#f59e0b')
          .attr('stroke-width', 1.5).attr('opacity', 0);
        // Agents in arc below
        var panNodes = [];
        var panR = R * 0.85, panCy = acy + 15;
        for (var i=0; i<12; i++) {
          var a = Math.PI*0.15 + (i/11)*Math.PI*0.7;
          var nx = acx + Math.cos(a)*panR, ny = panCy + Math.sin(a)*panR*0.55;
          g.append('line').attr('x1', eyeX).attr('y1', eyeY).attr('x2', nx).attr('y2', ny)
            .attr('stroke', arch.color).attr('stroke-width', 0.5).attr('stroke-dasharray', '2,3').attr('opacity', 0.12);
          var el = g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', 4)
            .attr('fill', arch.color).attr('opacity', 0.4).attr('stroke', arch.color).attr('stroke-width', 1);
          panNodes.push({x:nx, y:ny, el:el});
        }
        var scanIdx = 0;
        timers.push(setInterval(function() {
          var n = panNodes[scanIdx % panNodes.length];
          scanIdx++;
          beamEl.attr('opacity', 0.5).attr('x2', n.x).attr('y2', n.y)
            .transition().duration(200).attr('opacity', 0.6)
            .transition().duration(300).attr('opacity', 0);
          var flagged = Math.random() < 0.2;
          n.el.transition().duration(150)
            .attr('fill', flagged ? '#ef4444' : '#4ade80').attr('r', 6)
            .transition().duration(500)
            .attr('fill', arch.color).attr('r', 4);
        }, 500));
      }

      // ---- OLIGARCHY: elite triangle + peripheral nodes ----
      if (arch.name === 'Oligarchy') {
        var elites = [];
        var eR = R * 0.25;
        for (var i=0; i<3; i++) {
          var a = (i/3)*Math.PI*2 - Math.PI/2;
          elites.push({x: acx+Math.cos(a)*eR, y: acy+Math.sin(a)*eR});
        }
        // Elite-to-elite connections
        for (var i=0; i<3; i++) for (var j=i+1; j<3; j++) {
          g.append('line').attr('x1', elites[i].x).attr('y1', elites[i].y)
            .attr('x2', elites[j].x).attr('y2', elites[j].y)
            .attr('stroke', arch.color).attr('stroke-width', 2).attr('opacity', 0.3);
        }
        var eliteEls = elites.map(function(e) {
          return g.append('circle').attr('cx', e.x).attr('cy', e.y).attr('r', 8)
            .attr('fill', arch.color).attr('opacity', 0.7).attr('stroke', 'white').attr('stroke-width', 1.5);
        });
        // Peripheral nodes
        var periph = [];
        for (var i=0; i<12; i++) {
          var a = (i/12)*Math.PI*2 - Math.PI/2;
          var nx = acx+Math.cos(a)*R*0.85, ny = acy+Math.sin(a)*R*0.85;
          // Connect to nearest elite
          var nearest = 0, bestD = Infinity;
          elites.forEach(function(e, ei) {
            var d = Math.sqrt((e.x-nx)*(e.x-nx)+(e.y-ny)*(e.y-ny));
            if (d < bestD) { bestD = d; nearest = ei; }
          });
          g.append('line').attr('x1', nx).attr('y1', ny).attr('x2', elites[nearest].x).attr('y2', elites[nearest].y)
            .attr('stroke', arch.color).attr('stroke-width', 0.6).attr('opacity', 0.12);
          var el = g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', 3.5)
            .attr('fill', arch.color).attr('opacity', 0.35).attr('stroke', arch.color).attr('stroke-width', 0.8);
          periph.push({x:nx, y:ny, el:el, nearest:nearest});
        }
        var oligPktG = g.append('g');
        timers.push(setInterval(function() {
          // Elite deliberation
          var ei = Math.floor(Math.random()*3), ej = (ei+1)%3;
          oligPktG.append('circle').attr('cx', elites[ei].x).attr('cy', elites[ei].y).attr('r', 3)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(350).attr('cx', elites[ej].x).attr('cy', elites[ej].y)
            .transition().duration(200).attr('opacity', 0).remove();
          // Decree broadcast
          setTimeout(function() {
            oligPktG.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 5)
              .attr('fill', 'none').attr('stroke', arch.color).attr('stroke-width', 1.5).attr('opacity', 0.5)
              .transition().duration(1000).attr('r', R).attr('opacity', 0).remove();
          }, 500);
        }, 1200));
      }

      // ---- DOCTRINE: central tablet + orbiting agents ----
      if (arch.name === 'Doctrine') {
        // Tablet
        var tw = 24, th = 30;
        g.append('rect').attr('x', acx-tw/2).attr('y', acy-th/2).attr('width', tw).attr('height', th)
          .attr('rx', 3).attr('fill', '#f0fdfa').attr('stroke', arch.color).attr('stroke-width', 2);
        for (var i=0; i<4; i++) {
          g.append('line').attr('x1', acx-tw/2+5).attr('y1', acy-th/2+7+i*6)
            .attr('x2', acx+tw/2-5).attr('y2', acy-th/2+7+i*6)
            .attr('stroke', arch.color).attr('stroke-width', 0.7).attr('opacity', 0.4);
        }
        // Orbiting agents
        var docNodes = [];
        for (var i=0; i<8; i++) {
          var a = (i/8)*Math.PI*2 - Math.PI/2;
          var nx = acx+Math.cos(a)*R*0.75, ny = acy+Math.sin(a)*R*0.75;
          g.append('line').attr('x1', nx).attr('y1', ny).attr('x2', acx).attr('y2', acy)
            .attr('stroke', arch.color).attr('stroke-width', 0.6).attr('opacity', 0.12);
          var el = g.append('circle').attr('cx', nx).attr('cy', ny).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.45).attr('stroke', arch.color).attr('stroke-width', 1);
          docNodes.push({x:nx, y:ny, el:el});
        }
        var docPktG = g.append('g');
        timers.push(setInterval(function() {
          var n = docNodes[Math.floor(Math.random()*8)];
          var ok = Math.random() > 0.3;
          // Proposal to tablet
          docPktG.append('circle').attr('cx', n.x).attr('cy', n.y).attr('r', 2.5)
            .attr('fill', arch.color).attr('opacity', 0.8)
            .transition().duration(400).attr('cx', acx).attr('cy', acy)
            .on('end', function() {
              // Tablet response
              var col = ok ? '#4ade80' : '#ef4444';
              docPktG.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 3)
                .attr('fill', col).attr('opacity', 0.8)
                .transition().duration(400).attr('cx', n.x).attr('cy', n.y)
                .transition().duration(300).attr('opacity', 0).remove();
              n.el.transition().duration(200).attr('fill', col).attr('r', 7)
                .transition().duration(500).attr('fill', arch.color).attr('r', 5);
            })
            .transition().duration(200).attr('opacity', 0).remove();
        }, 900));
      }

      // ---- LIQUID DEMOCRACY: delegation chains ----
      if (arch.name === 'Liquid Democracy') {
        var ldNodes = [];
        var ldEdges = []; // {from, to, el}
        // Place 12 nodes in 3 clusters
        var ldPositions = [
          {x:acx-R*0.55, y:acy-R*0.4}, {x:acx-R*0.35, y:acy-R*0.6}, {x:acx-R*0.75, y:acy-R*0.1},
          {x:acx-R*0.2, y:acy+R*0.1}, {x:acx+R*0.1, y:acy-R*0.35},
          {x:acx+R*0.55, y:acy-R*0.4}, {x:acx+R*0.35, y:acy-R*0.15}, {x:acx+R*0.75, y:acy+R*0.1},
          {x:acx, y:acy+R*0.45}, {x:acx-R*0.3, y:acy+R*0.6},
          {x:acx+R*0.3, y:acy+R*0.55}, {x:acx+R*0.6, y:acy+R*0.45}
        ];
        // Initial delegation: form 3 trees (roots at 0, 5, 8)
        var ldDelegates = [-1, 0, 0, 1, 0, -1, 5, 5, -1, 8, 8, 10]; // -1 = root
        var ldEdgeG = g.append('g');
        var ldNodeG = g.append('g');

        function countDelegates(ni) {
          var c = 0;
          ldDelegates.forEach(function(d, i) { if (d === ni) c += 1 + countDelegates(i); });
          return c;
        }
        function drawLD() {
          ldEdgeG.selectAll('*').remove();
          ldNodeG.selectAll('*').remove();
          // Edges
          ldDelegates.forEach(function(to, from) {
            if (to < 0) return;
            var f = ldPositions[from], t = ldPositions[to];
            ldEdgeG.append('line').attr('x1', f.x).attr('y1', f.y).attr('x2', t.x).attr('y2', t.y)
              .attr('stroke', arch.color).attr('stroke-width', 1).attr('opacity', 0.25)
              .attr('marker-end', 'url(#soc-arr)');
          });
          // Nodes sized by delegation count
          ldPositions.forEach(function(p, i) {
            var dc = countDelegates(i);
            var r = 3 + dc * 1.8;
            var op = 0.3 + Math.min(dc * 0.15, 0.5);
            ldNodeG.append('circle').attr('cx', p.x).attr('cy', p.y).attr('r', r)
              .attr('fill', arch.color).attr('opacity', op).attr('stroke', arch.color).attr('stroke-width', 1.2);
          });
        }
        drawLD();
        timers.push(setInterval(function() {
          // Random non-root switches delegation
          var candidates = [];
          ldDelegates.forEach(function(d, i) { if (d >= 0) candidates.push(i); });
          if (!candidates.length) return;
          var switcher = candidates[Math.floor(Math.random()*candidates.length)];
          // Pick new target (different from current, not self, not someone who delegates to switcher)
          var newTarget;
          for (var tries=0; tries<20; tries++) {
            var t = Math.floor(Math.random()*12);
            if (t !== switcher && t !== ldDelegates[switcher] && ldDelegates[t] !== switcher) { newTarget = t; break; }
          }
          if (newTarget === undefined) return;
          ldDelegates[switcher] = newTarget;
          drawLD();
        }, 2000));
      }

      // ---- STEWARDSHIP: shared resource, cooperative contribution and consumption ----
      if (arch.name === 'Stewardship') {
        var nAgents = 5;
        var resW = 44, resH = 22;
        var resX = acx - resW / 2, resY = acy - resH / 2;
        var resLevel = 0.65; // fill fraction
        var stewAgents = [];
        for (var i = 0; i < nAgents; i++) {
          var a = (i / nAgents) * Math.PI * 2 - Math.PI / 2;
          var rr = R * 0.78;
          stewAgents.push({x: acx + Math.cos(a) * rr, y: acy + Math.sin(a) * rr, id: i});
        }
        // Lines first (behind nodes)
        var stewLineG = g.append('g');
        stewAgents.forEach(function(ag) {
          stewLineG.append('line').attr('x1', ag.x).attr('y1', ag.y).attr('x2', acx).attr('y2', acy)
            .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.15);
        });
        // Resource rectangle
        var resGroup = g.append('g');
        resGroup.append('rect').attr('x', resX).attr('y', resY).attr('width', resW).attr('height', resH)
          .attr('rx', 3).attr('fill', 'none').attr('stroke', arch.color).attr('stroke-width', 1.2).attr('opacity', 0.5);
        var resFill = resGroup.append('rect').attr('x', resX + 1).attr('y', resY + 1 + resH * (1 - resLevel) - 2)
          .attr('width', resW - 2).attr('height', resH * resLevel)
          .attr('rx', 2).attr('fill', arch.color).attr('opacity', 0.3);
        resGroup.append('text').attr('x', acx).attr('y', resY - 4).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.6).text('commons');
        // Agent nodes (on top)
        var stewNodeEls = stewAgents.map(function(ag) {
          return g.append('circle').attr('cx', ag.x).attr('cy', ag.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.55).attr('stroke', arch.color).attr('stroke-width', 1);
        });
        var stewDotG = g.append('g');
        var stewPhase = 0;
        timers.push(setInterval(function() {
          stewPhase++;
          var actorIdx = stewPhase % nAgents;
          var ag = stewAgents[actorIdx];
          var isOverconsume = (stewPhase % 7 === 3);
          if (isOverconsume) {
            // Over-consumer: red flash, then community pushback
            stewNodeEls[actorIdx].transition().duration(200).attr('fill', '#dc2626').attr('r', 7)
              .transition().duration(400).attr('fill', arch.color).attr('r', 5);
            // All other agents pulse
            stewNodeEls.forEach(function(el, i) {
              if (i === actorIdx) return;
              el.transition().delay(300).duration(200).attr('r', 7).attr('opacity', 0.9)
                .transition().duration(300).attr('r', 5).attr('opacity', 0.55);
            });
            // Small dot goes from agent toward resource but bounces back
            var dot = stewDotG.append('circle').attr('cx', ag.x).attr('cy', ag.y).attr('r', 2.5)
              .attr('fill', '#dc2626').attr('opacity', 0.85);
            dot.transition().duration(300).attr('cx', acx).attr('cy', acy)
              .transition().duration(300).attr('cx', ag.x).attr('cy', ag.y).attr('opacity', 0).remove();
          } else {
            var contribute = (stewPhase % 3 !== 0);
            var dotColor = contribute ? arch.color : '#6ee7b7';
            var dot2 = stewDotG.append('circle').attr('cx', contribute ? ag.x : acx).attr('cy', contribute ? ag.y : acy)
              .attr('r', 2).attr('fill', dotColor).attr('opacity', 0.8);
            dot2.transition().duration(450).attr('cx', contribute ? acx : ag.x).attr('cy', contribute ? acy : ag.y)
              .transition().duration(200).attr('opacity', 0).remove();
            // Nudge resource level
            resLevel = Math.max(0.2, Math.min(0.9, resLevel + (contribute ? 0.04 : -0.03)));
            resFill.transition().duration(300)
              .attr('y', resY + 1 + resH * (1 - resLevel) - 2)
              .attr('height', resH * resLevel);
          }
        }, 700));
      }

      // ---- CUSTODIANSHIP: principal -> custodian -> resources, access control ----
      if (arch.name === 'Custodianship') {
        var principalX = acx, principalY = areaTop + 20;
        var custX = acx, custY = areaTop + 60;
        var resNodes = [
          {x: acx - R * 0.6, y: acy + 20},
          {x: acx,             y: acy + 20},
          {x: acx + R * 0.6, y: acy + 20}
        ];
        var requesterX = ox + 22, requesterY = acy + 20;
        // Lines first
        g.append('line').attr('x1', principalX).attr('y1', principalY).attr('x2', custX).attr('y2', custY)
          .attr('stroke', arch.color).attr('stroke-width', 1.2).attr('opacity', 0.4);
        resNodes.forEach(function(rn) {
          g.append('line').attr('x1', custX).attr('y1', custY).attr('x2', rn.x).attr('y2', rn.y)
            .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.25);
        });
        g.append('line').attr('x1', requesterX).attr('y1', requesterY).attr('x2', resNodes[0].x).attr('y2', resNodes[0].y)
          .attr('stroke', '#dc2626').attr('stroke-width', 0.6).attr('stroke-dasharray', '3,2').attr('opacity', 0.3);
        // Principal
        g.append('circle').attr('cx', principalX).attr('cy', principalY).attr('r', 8)
          .attr('fill', arch.color).attr('opacity', 0.35).attr('stroke', arch.color).attr('stroke-width', 1.5);
        g.append('text').attr('x', principalX).attr('y', principalY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('font-weight', 'bold').attr('fill', arch.color).text('P');
        // Custodian
        var custEl = g.append('circle').attr('cx', custX).attr('cy', custY).attr('r', 7)
          .attr('fill', arch.color).attr('opacity', 0.6).attr('stroke', arch.color).attr('stroke-width', 1.5);
        g.append('text').attr('x', custX).attr('y', custY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('font-weight', 'bold').attr('fill', 'white').text('C');
        // Resources
        resNodes.forEach(function(rn, ri) {
          g.append('rect').attr('x', rn.x - 8).attr('y', rn.y - 6).attr('width', 16).attr('height', 12)
            .attr('rx', 2).attr('fill', arch.color).attr('opacity', 0.15).attr('stroke', arch.color).attr('stroke-width', 0.8);
          g.append('text').attr('x', rn.x).attr('y', rn.y + 3).attr('text-anchor', 'middle')
            .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.7).text('R' + (ri + 1));
        });
        // Requester
        g.append('circle').attr('cx', requesterX).attr('cy', requesterY).attr('r', 5)
          .attr('fill', '#dc2626').attr('opacity', 0.4).attr('stroke', '#dc2626').attr('stroke-width', 1);
        g.append('text').attr('x', requesterX).attr('y', requesterY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '5.5px').attr('fill', '#dc2626').attr('opacity', 0.7).text('?');
        var custDotG = g.append('g');
        var blockEl = g.append('text').attr('x', (requesterX + resNodes[0].x) / 2).attr('y', requesterY - 6)
          .attr('text-anchor', 'middle').attr('font-size', '8px').attr('fill', '#dc2626').attr('opacity', 0).text('x');
        var grantEl = g.append('line').attr('x1', custX).attr('y1', custY).attr('x2', resNodes[1].x).attr('y2', resNodes[1].y)
          .attr('stroke', '#16a34a').attr('stroke-width', 1.5).attr('opacity', 0);
        var custPhase = 0;
        timers.push(setInterval(function() {
          custPhase++;
          if (custPhase % 4 === 1) {
            // Requester probes resource
            var dot = custDotG.append('circle').attr('cx', requesterX).attr('cy', requesterY).attr('r', 2)
              .attr('fill', '#dc2626').attr('opacity', 0.8);
            dot.transition().duration(350).attr('cx', resNodes[0].x).attr('cy', resNodes[0].y)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (custPhase % 4 === 2) {
            // Custodian blocks (red X)
            blockEl.transition().duration(100).attr('opacity', 1)
              .transition().duration(600).attr('opacity', 0);
            custEl.transition().duration(150).attr('r', 9).attr('opacity', 0.9)
              .transition().duration(400).attr('r', 7).attr('opacity', 0.6);
          } else if (custPhase % 4 === 3) {
            // Custodian evaluates (pulse)
            custEl.transition().duration(250).attr('fill', '#f59e0b')
              .transition().duration(250).attr('fill', arch.color);
          } else {
            // Grant access to R2
            grantEl.transition().duration(100).attr('opacity', 0.9)
              .transition().duration(600).attr('opacity', 0);
            var dot2 = custDotG.append('circle').attr('cx', custX).attr('cy', custY).attr('r', 2.5)
              .attr('fill', '#16a34a').attr('opacity', 0.9);
            dot2.transition().duration(400).attr('cx', resNodes[1].x).attr('cy', resNodes[1].y)
              .transition().duration(200).attr('opacity', 0).remove();
          }
        }, 900));
      }

      // ---- CONSTITUTIONAL REPUBLIC: three branches, checks and balances ----
      if (arch.name === 'Constitutional Republic') {
        var branches = [
          {label: 'L', name: 'Legislative', x: acx - R * 0.7, y: areaTop + 35},
          {label: 'E', name: 'Executive',   x: acx + R * 0.7, y: areaTop + 35},
          {label: 'J', name: 'Judicial',    x: acx,            y: acy + R * 0.5}
        ];
        // Check lines between all pairs (behind nodes)
        var brPairs = [[0,1],[1,2],[0,2]];
        var checkLines = brPairs.map(function(pair) {
          return g.append('line')
            .attr('x1', branches[pair[0]].x).attr('y1', branches[pair[0]].y)
            .attr('x2', branches[pair[1]].x).attr('y2', branches[pair[1]].y)
            .attr('stroke', arch.color).attr('stroke-width', 1).attr('stroke-dasharray', '4,3')
            .attr('opacity', 0.2);
        });
        // Branch nodes
        var brEls = branches.map(function(b) {
          var el = g.append('circle').attr('cx', b.x).attr('cy', b.y).attr('r', 9)
            .attr('fill', arch.color).attr('opacity', 0.25).attr('stroke', arch.color).attr('stroke-width', 1.5);
          g.append('text').attr('x', b.x).attr('y', b.y + 3).attr('text-anchor', 'middle')
            .attr('font-size', '8px').attr('font-weight', 'bold').attr('fill', arch.color).attr('opacity', 0.8).text(b.label);
          g.append('text').attr('x', b.x).attr('y', b.y + 16).attr('text-anchor', 'middle')
            .attr('font-size', '5.5px').attr('fill', arch.color).attr('opacity', 0.5).text(b.name);
          return el;
        });
        var brDotG = g.append('g');
        var brPhase = 0;
        // Sequence: L->E rule, E acts (pulse), J->E constraint, then repeat with different pair
        var sequences = [
          {from:0, to:1, color: arch.color, label:'rule'},
          {from:1, to:2, color: '#d97706', label:'act'},
          {from:2, to:1, color: '#dc2626', label:'check'},
          {from:0, to:2, color: arch.color, label:'review'},
          {from:2, to:0, color: '#16a34a', label:'uphold'}
        ];
        timers.push(setInterval(function() {
          var seq = sequences[brPhase % sequences.length];
          brPhase++;
          var from = branches[seq.from], to = branches[seq.to];
          // Pulse source
          brEls[seq.from].transition().duration(150).attr('opacity', 0.7).attr('r', 11)
            .transition().duration(300).attr('opacity', 0.25).attr('r', 9);
          // Dot travels
          var dot = brDotG.append('circle').attr('cx', from.x).attr('cy', from.y).attr('r', 2.5)
            .attr('fill', seq.color).attr('opacity', 0.9);
          dot.transition().duration(500).attr('cx', to.x).attr('cy', to.y)
            .transition().duration(250).attr('opacity', 0).remove();
          // Pulse target line
          var pairIdx = brPairs.findIndex(function(p) {
            return (p[0]===seq.from&&p[1]===seq.to)||(p[0]===seq.to&&p[1]===seq.from);
          });
          if (pairIdx >= 0) {
            checkLines[pairIdx].transition().duration(200).attr('opacity', 0.7).attr('stroke', seq.color)
              .transition().duration(500).attr('opacity', 0.2).attr('stroke', arch.color);
          }
        }, 900));
      }

      // ---- FRANCHISE: platform at top, participants comply or get removed ----
      if (arch.name === 'Franchise') {
        var platY = areaTop + 18, platH = 16;
        var nPart = 6;
        var partY = acy + 30;
        var partSpacing = (cellW - 40) / (nPart + 1);
        var parts = [];
        for (var i = 0; i < nPart; i++) {
          parts.push({x: ox + 20 + partSpacing * (i + 1), y: partY, active: true, id: i});
        }
        // Platform bar (behind nodes)
        g.append('rect').attr('x', ox + 14).attr('y', platY - platH / 2).attr('width', cellW - 28).attr('height', platH)
          .attr('rx', 4).attr('fill', arch.color).attr('opacity', 0.18).attr('stroke', arch.color).attr('stroke-width', 1.2);
        g.append('text').attr('x', acx).attr('y', platY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6.5px').attr('font-weight', 'bold').attr('fill', arch.color).attr('opacity', 0.7).text('PLATFORM');
        // Vertical rule lines (behind nodes)
        var ruleLineG = g.append('g');
        parts.forEach(function(pt) {
          ruleLineG.append('line').attr('x1', acx).attr('y1', platY + platH / 2).attr('x2', pt.x).attr('y2', partY - 6)
            .attr('stroke', arch.color).attr('stroke-width', 0.7).attr('opacity', 0.12);
        });
        // Participant nodes
        var partEls = parts.map(function(pt) {
          return g.append('circle').attr('cx', pt.x).attr('cy', pt.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.5).attr('stroke', arch.color).attr('stroke-width', 1);
        });
        var franDotG = g.append('g');
        var franPhase = 0;
        timers.push(setInterval(function() {
          franPhase++;
          if (franPhase % 5 === 0) {
            // Violation: one random active participant flashes red, then fades out
            var activeIdxs = parts.map(function(p,i){return i;}).filter(function(i){return parts[i].active;});
            if (activeIdxs.length < 2) {
              // Reset all
              parts.forEach(function(p,i){
                p.active = true;
                partEls[i].attr('opacity', 0.5).attr('fill', arch.color).attr('r', 5);
              });
              return;
            }
            var vi = activeIdxs[Math.floor(Math.random() * activeIdxs.length)];
            parts[vi].active = false;
            partEls[vi].transition().duration(200).attr('fill', '#dc2626').attr('r', 7)
              .transition().duration(500).attr('opacity', 0.1).attr('r', 3);
          } else {
            // Rule packet flows down from platform to random active participant
            var act = parts.filter(function(p){return p.active;});
            if (!act.length) return;
            var tgt = act[Math.floor(Math.random() * act.length)];
            var dot = franDotG.append('circle').attr('cx', acx).attr('cy', platY + platH / 2).attr('r', 2)
              .attr('fill', arch.color).attr('opacity', 0.8);
            dot.transition().duration(400).attr('cx', tgt.x).attr('cy', tgt.y - 6)
              .transition().duration(200).attr('opacity', 0).remove();
            // Compliance dot flows up
            var dot2 = franDotG.append('circle').attr('cx', tgt.x).attr('cy', tgt.y - 6).attr('r', 1.5)
              .attr('fill', '#16a34a').attr('opacity', 0.7);
            dot2.transition().delay(400).duration(400).attr('cx', acx).attr('cy', platY + platH / 2)
              .transition().duration(200).attr('opacity', 0).remove();
          }
        }, 650));
      }

      // ---- OPEN-SOURCE: community, merge authority, shared repo, fork ----
      if (arch.name === 'Open-Source') {
        var repoX = acx, repoY = acy - 10;
        var mergeX = acx + R * 0.75, mergeY = areaTop + 35;
        var nComm = 4;
        var commAgents = [];
        for (var i = 0; i < nComm; i++) {
          var a = (i / nComm) * Math.PI - Math.PI / 2 + 0.3;
          commAgents.push({x: acx - R * 0.7 + Math.cos(a) * 28, y: acy + Math.sin(a) * 40 - 10});
        }
        var forkRepoX = acx + R * 0.55, forkRepoY = areaTop + areaH - 22;
        var forkVisible = false;
        // Lines first
        commAgents.forEach(function(ca) {
          g.append('line').attr('x1', ca.x).attr('y1', ca.y).attr('x2', repoX).attr('y2', repoY)
            .attr('stroke', arch.color).attr('stroke-width', 0.7).attr('opacity', 0.18);
        });
        g.append('line').attr('x1', repoX).attr('y1', repoY).attr('x2', mergeX).attr('y2', mergeY)
          .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.2);
        // Repo
        g.append('rect').attr('x', repoX - 12).attr('y', repoY - 8).attr('width', 24).attr('height', 16)
          .attr('rx', 3).attr('fill', arch.color).attr('opacity', 0.15).attr('stroke', arch.color).attr('stroke-width', 1);
        g.append('text').attr('x', repoX).attr('y', repoY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '5.5px').attr('fill', arch.color).attr('opacity', 0.7).text('repo');
        // Merge authority (small council of 3 dots)
        g.append('circle').attr('cx', mergeX).attr('cy', mergeY).attr('r', 9)
          .attr('fill', arch.color).attr('opacity', 0.15).attr('stroke', arch.color).attr('stroke-width', 1);
        g.append('text').attr('x', mergeX).attr('y', mergeY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.7).text('MA');
        // Community nodes
        commAgents.forEach(function(ca) {
          g.append('circle').attr('cx', ca.x).attr('cy', ca.y).attr('r', 4.5)
            .attr('fill', arch.color).attr('opacity', 0.45).attr('stroke', arch.color).attr('stroke-width', 1);
        });
        // Fork repo (initially invisible)
        var forkEl = g.append('rect').attr('x', forkRepoX - 10).attr('y', forkRepoY - 7).attr('width', 20).attr('height', 14)
          .attr('rx', 3).attr('fill', '#4338ca').attr('opacity', 0).attr('stroke', '#4338ca').attr('stroke-width', 1);
        var forkLbl = g.append('text').attr('x', forkRepoX).attr('y', forkRepoY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '5.5px').attr('fill', '#4338ca').attr('opacity', 0).text('fork');
        var osDotG = g.append('g');
        var osPhase = 0;
        timers.push(setInterval(function() {
          osPhase++;
          var seq = osPhase % 6;
          if (seq <= 1) {
            // Community contribution to repo
            var ca = commAgents[osPhase % nComm];
            var dot = osDotG.append('circle').attr('cx', ca.x).attr('cy', ca.y).attr('r', 2)
              .attr('fill', arch.color).attr('opacity', 0.8);
            dot.transition().duration(400).attr('cx', repoX).attr('cy', repoY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (seq === 2) {
            // Merge authority reviews (check)
            var dot2 = osDotG.append('circle').attr('cx', repoX).attr('cy', repoY).attr('r', 2)
              .attr('fill', arch.color).attr('opacity', 0.8);
            dot2.transition().duration(400).attr('cx', mergeX).attr('cy', mergeY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (seq === 3) {
            // Accepted: green merge dot
            var dot3 = osDotG.append('circle').attr('cx', mergeX).attr('cy', mergeY).attr('r', 2.5)
              .attr('fill', '#16a34a').attr('opacity', 0.9);
            dot3.transition().duration(400).attr('cx', repoX).attr('cy', repoY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (seq === 4) {
            // Fork event
            forkVisible = true;
            forkEl.transition().duration(400).attr('opacity', 0.7);
            forkLbl.transition().duration(400).attr('opacity', 0.7);
            var dot4 = osDotG.append('circle').attr('cx', repoX).attr('cy', repoY).attr('r', 2)
              .attr('fill', '#4338ca').attr('opacity', 0.9);
            dot4.transition().duration(500).attr('cx', forkRepoX).attr('cy', forkRepoY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else {
            // Fork fades back
            forkEl.transition().duration(600).attr('opacity', 0);
            forkLbl.transition().duration(600).attr('opacity', 0);
          }
        }, 700));
      }

      // ---- SORTITION: population pool, random selection, council deliberation ----
      if (arch.name === 'Sortition') {
        var nPop = 18;
        var popDots = [];
        for (var i = 0; i < nPop; i++) {
          var a = (i / nPop) * Math.PI * 2;
          var rr = R * 0.82;
          popDots.push({x: acx + Math.cos(a) * rr, y: acy + Math.sin(a) * rr, selected: false, id: i});
        }
        var council = [];
        var councilorEls = [];
        // Council deliberation lines first, so population dots render on top
        var councilLineG = g.append('g');
        // Draw population ring dots (lines not applicable here; dots only)
        var popEls = popDots.map(function(pd) {
          return g.append('circle').attr('cx', pd.x).attr('cy', pd.y).attr('r', 3)
            .attr('fill', arch.color).attr('opacity', 0.3);
        });
        // Randomizer pulsing ring
        var randRing = g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 12)
          .attr('fill', 'none').attr('stroke', arch.color).attr('stroke-width', 1).attr('stroke-dasharray', '5,3').attr('opacity', 0.25);
        g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', arch.color).attr('opacity', 0.5).text('lot');
        var sortPhase = 0;
        var councilIdxs = [];
        timers.push(setInterval(function() {
          sortPhase++;
          if (sortPhase % 8 === 1) {
            // Randomizer pulses
            randRing.transition().duration(300).attr('r', 18).attr('opacity', 0.7).attr('stroke-width', 2)
              .transition().duration(400).attr('r', 12).attr('opacity', 0.25).attr('stroke-width', 1);
            // Pick 3 random new council members
            councilIdxs = [];
            var pool = popDots.map(function(_,i){return i;});
            while (councilIdxs.length < 3 && pool.length > 0) {
              var ri = Math.floor(Math.random() * pool.length);
              councilIdxs.push(pool[ri]);
              pool.splice(ri, 1);
            }
            // Reset all
            popEls.forEach(function(el) { el.attr('fill', arch.color).attr('opacity', 0.3).attr('r', 3); });
            // Highlight selected
            councilIdxs.forEach(function(ci) {
              popEls[ci].transition().duration(300).attr('fill', arch.color).attr('r', 5.5).attr('opacity', 0.9);
            });
            // Draw council center lines
            councilLineG.selectAll('*').remove();
            if (councilIdxs.length >= 2) {
              for (var ii = 0; ii < councilIdxs.length; ii++) {
                for (var jj = ii+1; jj < councilIdxs.length; jj++) {
                  var pa = popDots[councilIdxs[ii]], pb = popDots[councilIdxs[jj]];
                  councilLineG.append('line').attr('x1', pa.x).attr('y1', pa.y).attr('x2', pb.x).attr('y2', pb.y)
                    .attr('stroke', arch.color).attr('stroke-width', 1).attr('opacity', 0.4);
                }
              }
            }
          } else if (sortPhase % 8 >= 2 && sortPhase % 8 <= 5) {
            // Council deliberates: pulsing lines
            councilLineG.selectAll('line').transition().duration(200)
              .attr('opacity', (sortPhase % 2 === 0) ? 0.7 : 0.2);
          } else if (sortPhase % 8 === 6) {
            // Decision made: green flash on council members
            councilIdxs.forEach(function(ci) {
              popEls[ci].transition().duration(200).attr('fill', '#16a34a').attr('r', 6)
                .transition().duration(400).attr('fill', arch.color).attr('r', 3).attr('opacity', 0.3);
            });
            councilLineG.selectAll('line').transition().duration(400).attr('opacity', 0).remove();
          }
        }, 550));
      }

      // ---- ADHOCRACY: problem appears, specialists converge, solve, disperse ----
      if (arch.name === 'Adhocracy') {
        var nSpec = 5;
        var specHome = [];
        for (var i = 0; i < nSpec; i++) {
          var a = (i / nSpec) * Math.PI * 2 + 0.5;
          var rr = R * 0.85;
          specHome.push({x: acx + Math.cos(a) * rr, y: acy + Math.sin(a) * rr});
        }
        // Cluster lines first, so specialist nodes render on top
        var adhLineG = g.append('g');
        var specEls = specHome.map(function(sh) {
          return g.append('circle').attr('cx', sh.x).attr('cy', sh.y).attr('r', 5)
            .attr('fill', arch.color).attr('opacity', 0.45).attr('stroke', arch.color).attr('stroke-width', 1);
        });
        var probEl = g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 7)
          .attr('fill', '#dc2626').attr('opacity', 0).attr('stroke', '#dc2626').attr('stroke-width', 1.5);
        g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '7px').attr('fill', 'white').attr('font-weight', 'bold').attr('opacity', 0).text('!');
        var adhPhase = 0;
        timers.push(setInterval(function() {
          adhPhase++;
          var step = adhPhase % 10;
          if (step === 0) {
            // Problem appears
            probEl.transition().duration(250).attr('opacity', 0.85);
            adhLineG.selectAll('*').remove();
          } else if (step >= 1 && step <= 3) {
            // Specialists converge
            specEls.forEach(function(el, i) {
              var sh = specHome[i];
              var progress = step / 4;
              var tx = sh.x + (acx - sh.x) * progress;
              var ty = sh.y + (acy - sh.y) * progress;
              el.transition().duration(400).attr('cx', tx).attr('cy', ty);
            });
          } else if (step === 4) {
            // Cluster formed: draw lines between specialists (adhLineG sits behind nodes)
            adhLineG.selectAll('*').remove();
            for (var ii = 0; ii < nSpec; ii++) {
              for (var jj = ii+1; jj < nSpec; jj++) {
                var pa = specHome[ii], pb = specHome[jj];
                var cx0 = pa.x + (acx - pa.x) * 0.9, cy0 = pa.y + (acy - pa.y) * 0.9;
                var cx1 = pb.x + (acx - pb.x) * 0.9, cy1 = pb.y + (acy - pb.y) * 0.9;
                adhLineG.append('line').attr('x1', cx0).attr('y1', cy0).attr('x2', cx1).attr('y2', cy1)
                  .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.35);
              }
            }
          } else if (step >= 5 && step <= 6) {
            // Working pulse
            probEl.transition().duration(150).attr('r', 10)
              .transition().duration(250).attr('r', 7);
          } else if (step === 7) {
            // Problem resolved
            probEl.transition().duration(300).attr('fill', '#16a34a').attr('stroke', '#16a34a')
              .transition().duration(400).attr('opacity', 0).attr('fill', '#dc2626').attr('stroke', '#dc2626');
            adhLineG.selectAll('line').transition().duration(400).attr('opacity', 0);
          } else if (step >= 8) {
            // Disperse back
            specEls.forEach(function(el, i) {
              el.transition().duration(600).attr('cx', specHome[i].x).attr('cy', specHome[i].y);
            });
          }
        }, 500));
      }

      // ---- MISSION COMMAND: one intent, three autonomous paths to shared goal ----
      if (arch.name === 'Mission Command') {
        var cmdrX = acx, cmdrY = areaTop + 22;
        var goalX = acx, goalY = areaTop + areaH - 18;
        var subordinates = [
          {x: acx - R * 0.65, y: acy - 15, color: '#16a34a',  trail: 'left'},
          {x: acx,             y: acy - 15, color: '#d97706',  trail: 'right'},
          {x: acx + R * 0.65, y: acy - 15, color: '#2563eb',  trail: 'zigzag'}
        ];
        // Static structure lines (behind nodes)
        subordinates.forEach(function(s) {
          g.append('line').attr('x1', cmdrX).attr('y1', cmdrY).attr('x2', s.x).attr('y2', s.y)
            .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.2);
        });
        // Commander
        g.append('circle').attr('cx', cmdrX).attr('cy', cmdrY).attr('r', 8)
          .attr('fill', arch.color).attr('opacity', 0.3).attr('stroke', arch.color).attr('stroke-width', 1.5);
        g.append('text').attr('x', cmdrX).attr('y', cmdrY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '7px').attr('font-weight', 'bold').attr('fill', arch.color).text('CO');
        // Goal circle
        g.append('circle').attr('cx', goalX).attr('cy', goalY).attr('r', 9)
          .attr('fill', '#16a34a').attr('opacity', 0.15).attr('stroke', '#16a34a').attr('stroke-width', 1.2);
        g.append('text').attr('x', goalX).attr('y', goalY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('fill', '#16a34a').attr('opacity', 0.7).text('goal');
        // Subordinates
        subordinates.forEach(function(s) {
          g.append('circle').attr('cx', s.x).attr('cy', s.y).attr('r', 5.5)
            .attr('fill', s.color).attr('opacity', 0.5).attr('stroke', s.color).attr('stroke-width', 1);
        });
        // Intent label on commander
        var intentG = g.append('g');
        var mcDotG = g.append('g');
        var mcPhase = 0;
        timers.push(setInterval(function() {
          mcPhase++;
          var step = mcPhase % 8;
          if (step === 0) {
            // Commander sends intent packet (large, distinctive)
            var dot = mcDotG.append('circle').attr('cx', cmdrX).attr('cy', cmdrY).attr('r', 4)
              .attr('fill', arch.color).attr('opacity', 0.9);
            dot.transition().duration(300).attr('cx', acx).attr('cy', acy - 15).attr('r', 3)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (step === 1) {
            // Sub 0: sweeps left then down to goal
            var d0 = mcDotG.append('circle').attr('cx', subordinates[0].x).attr('cy', subordinates[0].y).attr('r', 2.5)
              .attr('fill', subordinates[0].color).attr('opacity', 0.85);
            d0.transition().duration(300).attr('cx', subordinates[0].x - 20).attr('cy', acy + 10)
              .transition().duration(300).attr('cx', goalX).attr('cy', goalY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (step === 2) {
            // Sub 1: sweeps right then down
            var d1 = mcDotG.append('circle').attr('cx', subordinates[1].x).attr('cy', subordinates[1].y).attr('r', 2.5)
              .attr('fill', subordinates[1].color).attr('opacity', 0.85);
            d1.transition().duration(300).attr('cx', subordinates[1].x + 22).attr('cy', acy + 5)
              .transition().duration(300).attr('cx', goalX).attr('cy', goalY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (step === 3) {
            // Sub 2: zigzag to goal
            var d2 = mcDotG.append('circle').attr('cx', subordinates[2].x).attr('cy', subordinates[2].y).attr('r', 2.5)
              .attr('fill', subordinates[2].color).attr('opacity', 0.85);
            d2.transition().duration(200).attr('cx', subordinates[2].x - 15).attr('cy', acy)
              .transition().duration(200).attr('cx', subordinates[2].x + 10).attr('cy', acy + 20)
              .transition().duration(200).attr('cx', goalX).attr('cy', goalY)
              .transition().duration(200).attr('opacity', 0).remove();
          } else if (step === 4) {
            // Goal pulses green to show convergence
            g.select('circle[cy="' + goalY + '"]')
              .transition().duration(200).attr('r', 13).attr('opacity', 0.5)
              .transition().duration(400).attr('r', 9).attr('opacity', 0.15);
          }
        }, 600));
      }

      // ---- MECHANISM DESIGN: 4 agents, incentive structure channels self-interest ----
      if (arch.name === 'Mechanism Design') {
        var nMech = 4;
        var mechAgents = [];
        for (var i = 0; i < nMech; i++) {
          var a = (i / nMech) * Math.PI * 2 - Math.PI / 4;
          mechAgents.push({x: acx + Math.cos(a) * R * 0.65, y: acy + Math.sin(a) * R * 0.65, id: i});
        }
        var agentColors = ['#7c2d12', '#92400e', '#78350f', '#713f12'];
        // Game framework: octagon outline connecting all agents (lines first)
        for (var i = 0; i < nMech; i++) {
          for (var j = i+1; j < nMech; j++) {
            g.append('line').attr('x1', mechAgents[i].x).attr('y1', mechAgents[i].y)
              .attr('x2', mechAgents[j].x).attr('y2', mechAgents[j].y)
              .attr('stroke', arch.color).attr('stroke-width', 0.8).attr('opacity', 0.15)
              .attr('stroke-dasharray', '3,2');
          }
        }
        // Lines from agents to center outcome
        mechAgents.forEach(function(ma) {
          g.append('line').attr('x1', ma.x).attr('y1', ma.y).attr('x2', acx).attr('y2', acy)
            .attr('stroke', arch.color).attr('stroke-width', 0.6).attr('opacity', 0.12);
        });
        // Center outcome node
        g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', 10)
          .attr('fill', '#16a34a').attr('opacity', 0.15).attr('stroke', '#16a34a').attr('stroke-width', 1.2);
        g.append('text').attr('x', acx).attr('y', acy + 3).attr('text-anchor', 'middle')
          .attr('font-size', '5.5px').attr('fill', '#16a34a').attr('opacity', 0.7).text('outcome');
        // Agent nodes
        var mechEls = mechAgents.map(function(ma, i) {
          var el = g.append('circle').attr('cx', ma.x).attr('cy', ma.y).attr('r', 6)
            .attr('fill', agentColors[i % agentColors.length]).attr('opacity', 0.55)
            .attr('stroke', agentColors[i % agentColors.length]).attr('stroke-width', 1);
          g.append('text').attr('x', ma.x).attr('y', ma.y + 3).attr('text-anchor', 'middle')
            .attr('font-size', '5.5px').attr('fill', 'white').attr('opacity', 0.8).text('A' + (i+1));
          return el;
        });
        var mechDotG = g.append('g');
        var mechPhase = 0;
        timers.push(setInterval(function() {
          mechPhase++;
          var actorIdx = mechPhase % nMech;
          var ma = mechAgents[actorIdx];
          // Agent acts in self-interest (arrow toward center)
          var dot = mechDotG.append('circle').attr('cx', ma.x).attr('cy', ma.y).attr('r', 2.5)
            .attr('fill', agentColors[actorIdx]).attr('opacity', 0.85);
          dot.transition().duration(350).attr('cx', acx + (ma.x - acx) * 0.3).attr('cy', acy + (ma.y - acy) * 0.3)
            .transition().duration(250).attr('cx', acx).attr('cy', acy).attr('fill', '#16a34a')
            .transition().duration(200).attr('opacity', 0).remove();
          // Outcome pulses green as contributions arrive
          if (mechPhase % 4 === 0) {
            g.select('circle').filter(function() {
              var cx0 = parseFloat(d3.select(this).attr('cx'));
              var cy0 = parseFloat(d3.select(this).attr('cy'));
              return Math.abs(cx0 - acx) < 1 && Math.abs(cy0 - acy) < 1;
            }).transition().duration(200).attr('r', 14).attr('opacity', 0.45)
              .transition().duration(350).attr('r', 10).attr('opacity', 0.15);
          }
          // Agent pulses to show self-interest
          mechEls[actorIdx].transition().duration(150).attr('r', 8).attr('opacity', 0.8)
            .transition().duration(300).attr('r', 6).attr('opacity', 0.55);
        }, 600));
      }

      // ---- IMMUNE SYSTEM: population, anomaly, innate scan, adaptive response ----
      if (arch.name === 'Immune System') {
        var nImm = 14;
        var immPop = [];
        for (var i = 0; i < nImm; i++) {
          var a = (i / nImm) * Math.PI * 2;
          var rr = R * 0.72;
          immPop.push({x: acx + Math.cos(a) * rr, y: acy + Math.sin(a) * rr, anomalous: false, id: i});
        }
        var anomalyIdx = -1;
        var responderX = acx, responderY = acy;
        // Innate scan ring
        var scanRing = g.append('circle').attr('cx', acx).attr('cy', acy).attr('r', R * 0.3)
          .attr('fill', 'none').attr('stroke', '#fbbf24').attr('stroke-width', 1).attr('opacity', 0);
        // Responder node (hidden initially)
        var responderEl = g.append('circle').attr('cx', responderX).attr('cy', responderY).attr('r', 7)
          .attr('fill', '#881337').attr('opacity', 0).attr('stroke', '#881337').attr('stroke-width', 1.5);
        g.append('text').attr('x', responderX).attr('y', responderY + 3).attr('text-anchor', 'middle')
          .attr('font-size', '6px').attr('font-weight', 'bold').attr('fill', 'white').attr('opacity', 0).text('R');
        // Population dots
        var immEls = immPop.map(function(ip) {
          return g.append('circle').attr('cx', ip.x).attr('cy', ip.y).attr('r', 3.5)
            .attr('fill', arch.color).attr('opacity', 0.45);
        });
        var immDotG = g.append('g');
        var immPhase = 0;
        timers.push(setInterval(function() {
          immPhase++;
          var step = immPhase % 12;
          if (step === 0) {
            // Pick a random anomaly
            anomalyIdx = Math.floor(Math.random() * nImm);
            immEls[anomalyIdx].transition().duration(300).attr('fill', '#dc2626').attr('r', 5.5).attr('opacity', 0.9);
          } else if (step === 2) {
            // Innate scan wave expands
            scanRing.attr('r', R * 0.1).attr('opacity', 0.7)
              .transition().duration(600).attr('r', R * 0.9).attr('opacity', 0);
          } else if (step === 3) {
            // Innate flags it: anomaly pulses brighter
            if (anomalyIdx >= 0) {
              immEls[anomalyIdx].transition().duration(150).attr('r', 7)
                .transition().duration(250).attr('r', 5.5);
            }
          } else if (step === 4) {
            // Adaptive responder moves toward anomaly
            var ap = immPop[anomalyIdx >= 0 ? anomalyIdx : 0];
            responderEl.attr('cx', acx).attr('cy', acy).attr('opacity', 0.85);
            responderEl.transition().duration(500).attr('cx', (acx + ap.x) / 2).attr('cy', (acy + ap.y) / 2);
          } else if (step === 6) {
            // Confirmed: isolate - push anomaly outward and show barrier
            if (anomalyIdx >= 0) {
              var ap = immPop[anomalyIdx];
              var outX = acx + (ap.x - acx) * 1.5;
              var outY = acy + (ap.y - acy) * 1.5;
              // Barrier ring around anomaly position
              var barrier = immDotG.append('circle').attr('cx', ap.x).attr('cy', ap.y).attr('r', 8)
                .attr('fill', 'none').attr('stroke', '#dc2626').attr('stroke-width', 1.5).attr('opacity', 0.8);
              immEls[anomalyIdx].transition().duration(400).attr('cx', outX).attr('cy', outY)
                .transition().duration(300).attr('opacity', 0.2);
              barrier.transition().delay(400).duration(500).attr('cx', outX).attr('cy', outY)
                .transition().duration(400).attr('opacity', 0).remove();
              responderEl.transition().duration(300).attr('cx', ap.x).attr('cy', ap.y)
                .transition().duration(300).attr('opacity', 0);
            }
          } else if (step === 9) {
            // Reset: restore normal agent
            if (anomalyIdx >= 0) {
              immEls[anomalyIdx]
                .attr('cx', immPop[anomalyIdx].x).attr('cy', immPop[anomalyIdx].y)
                .transition().duration(300).attr('fill', arch.color).attr('r', 3.5).attr('opacity', 0.45);
              anomalyIdx = -1;
            }
            responderEl.attr('cx', acx).attr('cy', acy).attr('opacity', 0);
          }
        }, 450));
      }

      // ---- KB ICON for each panel ----
      var kbX = ox + cellW - 22, kbY = oy + cellH - 18;
      // Guild gets 3 mini KBs, Colony gets 3 scattered ones
      if (arch.name === 'Guild') {
        // Guild: 3 KBs near each cluster
        [[ox + cellW * 0.2, acy - R * 0.4], [ox + cellW * 0.8, acy - R * 0.4], [ox + cellW * 0.5, acy + R * 0.5]].forEach(function(pos) {
          g.append('rect').attr('x', pos[0] - 4).attr('y', pos[1] - 5).attr('width', 8).attr('height', 10)
            .attr('rx', 2).attr('fill', '#f0fdf4').attr('stroke', arch.color).attr('stroke-width', 0.8);
          g.append('line').attr('x1', pos[0] - 3).attr('y1', pos[1]).attr('x2', pos[0] + 3).attr('y2', pos[1])
            .attr('stroke', arch.color).attr('stroke-width', 0.5);
        });
      } else if (arch.name === 'Colony') {
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
    }); // end drawFns.push
  }); // end archetypes.forEach

  function clearTimers(timers) {
    timers.forEach(function(t) {
      if (t && t.clear) t.clear();
      else { clearInterval(t); clearTimeout(t); }
    });
    timers.length = 0;
  }

  function init() {
    clearTimers(animTimers);
    svg.selectAll('g.cell').remove();
    var ztState = {running: true};
    archetypes.forEach(function(arch, idx) {
      var col = idx % cols, row = Math.floor(idx / cols);
      var ox = padX + col * (cellW + 5), oy = padY + row * (cellH + 12);
      drawFns[idx]({targetSvg: svg, timers: animTimers, ox: ox, oy: oy, ztState: ztState});
    });
  }

  var socExpanded = false;

  function socExpandPanel(idx) {
    if (socExpanded) return;
    socExpanded = true;
    var arch = archetypes[idx];
    var modalTimers = [];
    var modalZtState = {running: true};
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
    card.append('div')
      .style('position', 'absolute').style('top', '12px').style('right', '16px')
      .style('font-size', '20px').style('color', '#999').style('cursor', 'pointer')
      .style('line-height', '1').style('font-family', 'sans-serif')
      .text('\u00D7')
      .on('click', socCloseExpand);
    var header = card.append('div').style('padding', '16px 20px 4px');
    header.append('div').style('font-size', '18px').style('font-weight', 'bold')
      .style('color', arch.color).text(arch.name);
    header.append('div').style('font-size', '11px').style('color', '#aaa')
      .style('margin-top', '2px').text(arch.sub);
    var svgWrap = card.append('div').style('padding', '0 12px 16px');
    var expSvg = svgWrap.append('svg')
      .attr('viewBox', '0 0 ' + cellW + ' ' + cellH)
      .style('width', '100%').style('display', 'block');
    addDefsToSvg(expSvg);
    drawFns[idx]({targetSvg: expSvg, timers: modalTimers, ox: 0, oy: 0, ztState: modalZtState});
    card.append('div').style('text-align', 'center').style('padding', '0 0 12px')
      .style('font-size', '10px').style('color', '#ccc').style('font-style', 'italic')
      .text('Click anywhere outside to close');
    setTimeout(function() { backdrop.style('opacity', '1'); }, 10);
    backdrop.on('click', function(event) {
      if (event.target === backdrop.node()) socCloseExpand();
    });
    function socCloseExpand() {
      socExpanded = false;
      modalZtState.running = false;
      clearTimers(modalTimers);
      backdrop.style('opacity', '0');
      setTimeout(function() { backdrop.remove(); }, 250);
    }
  }

  init();

  // Add click overlays for each cell
  function addOverlays() {
    svg.selectAll('.soc-overlay').remove();
    archetypes.forEach(function(arch, idx) {
      var col = idx % cols, row = Math.floor(idx / cols);
      var ox = padX + col * (cellW + 5), oy = padY + row * (cellH + 12);
      svg.append('rect').attr('class', 'soc-overlay')
        .attr('x', ox).attr('y', oy).attr('width', cellW).attr('height', cellH)
        .attr('fill', 'transparent').style('cursor', 'pointer')
        .on('click', function(event) { event.stopPropagation(); socExpandPanel(idx); });
    });
  }
  addOverlays();

  svg.on('click', function() {
    clearTimers(animTimers);
    init();
    addOverlays();
  });
})();
</script>
