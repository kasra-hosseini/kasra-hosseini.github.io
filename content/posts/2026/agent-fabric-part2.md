---
title: "The Agent Fabric (Part 2): Delegation, and What It Costs"
subtitle: "How work gets split among agents, what each hop costs, and how splitting it quietly creates authority"
date: 2026-07-31
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "multi-agent systems", "LLM", "agent fabric", "governance", "the loom hypothesis"]
description: "How work gets split inside agent societies: forty-three delegation patterns, the economics that prune them, and the point at which a routing preference hardens into authority nobody designed."
draft: false
math: false
ShowToc: true
TocOpen: false
hideCitation: false
wordcount: "~3,500 words (body) · ~11,000 words (notes + patterns)"
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

<p style="font-size: 0.82em; color: #999; margin-top: 1em;"><a href="https://code.claude.com/docs/en/overview" target="_blank" rel="noopener" style="color: #999;">Claude Code</a> was used for editing and visualizations. All ideas and arguments are the authors' own.</p>


<div style="background: #fffbeb; border: 1px solid #f59e0b; border-radius: 6px; padding: 0.6em 1em; margin: 1.5em 0; font-size: 0.88em; color: #92400e;">
<strong>Early access.</strong> This blog series is a work in progress. Feedback, comments, and suggestions are welcome. Feel free to <a href="https://www.linkedin.com/in/kasra-hosseini/" target="_blank" rel="noopener" style="color: #92400e;">reach out on LinkedIn</a> or leave a comment at the bottom of the page.
</div>

*Part of The Agent Fabric series. [Part 1](/posts/2026/agent-fabric-part1/) argued that agents survive only while they are useful to someone, that compute is finite while agent populations are not, and that the squeeze between those two facts pushes agents toward connected, coordinated arrangements. Some of those arrangements get designed by a company; others just accrete around whatever tools a person happens to own. Either way, this post is about what they turn into.*

*Two ways to read it. The body is the argument. The expandable sections are a reference manual of pattern cards, examples, and scenarios, and you can ignore all of them without losing the thread.*

<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **[Prologue: The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/)**: the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis, and the path from isolation to interweaving
- **Part 2: Delegation, and What It Costs** (you are here): delegation archetypes, the economics of delegation trees, and how splitting work turns into authority
- **[Part 3: Ruling an Agent Society](/posts/2026/agent-fabric-part3/)**: governance archetypes, who benefits, adversarial dynamics, and who enforces the rules
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

Ask a coding agent to refactor something and it quietly splits the job across a planner, a code writer, a test runner, and a security scanner. You talked to one agent. Several did the work, and you never saw the org chart. Ask a personal agent to book travel and it goes out to an airline agent, a hotel agent, a payment agent. You did not design that structure either; the task demanded it.

That is delegation, and it is already ordinary. What is not yet ordinary is what happens when those arrangements start to remember how they went.

Delegation is the easy half. It is a question about flow: how a task moves through a group of agents, who does what, who checks the result, and who pays the compute and latency bill at each step. A data-processing pipeline that forgets everything between runs is pure delegation, and it will never be anything more than that.

Governance is the harder half, because it is a question about authority rather than flow. Who gets to decide? On what basis? And how can that decision be challenged? The two look identical from outside for a while, and what separates them is memory. Part 1 reserved the word *society* for a group of agents that has started accumulating things across tasks rather than starting fresh each time. They build up context they share. Where work gets sent begins to depend on how it went last time. What one of them learns can reach the others. Governance is what grows on top of that accumulation, whether or not anybody planted it.

Which brings us to the claim this post is built on. **Delegation becomes governance when the system remembers who did what, how well, at what cost, and under whose authority.** Nothing needs to be designed for this to happen. A system that keeps those records and acts on them has started to govern, and the drift runs one way: operational data hardens into routing preference, preference becomes standing, and standing constrains what can be decided later. You can break the ratchet by resetting the memory, staying stateless, or redeploying from scratch, but each of those is a deliberate act. Skip them and the system begins governing before anyone chooses that it should. The real design question, then, is not how many agents to use. It is whether the governance you end up with is one you can name, inspect, and overrule.

Everything below is built from one unit: a model with memory that survives between calls, tools that reach the world, and enough planning to break a goal into steps. A bare model maps input to output. This thing loops, remembers, and acts, and left on its own it hits a ceiling that [The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/) works through in detail. That is the atom. This post is the chemistry.

## The Division of Labour

Every multi-agent system has to answer one question first: how does the work get split? With a handful of agents, an orchestrator handing out assignments is enough. Add more and the shape of the delegation stops being an implementation detail and becomes the architecture itself. Push it far enough, to the scales Part 1 imagines, and the question stops being about splitting work at all and becomes about who has standing to decide, which is where [Part 3](/posts/2026/agent-fabric-part3/) picks up.

### Delegation Archetypes

People have found a great many ways to answer that question. Forty-three of them are catalogued further down, and the honest advice is not to read them: open the catalogue when you need to identify something you are already building, and otherwise walk past it. The count is not the point. What matters is a pattern that holds across all of them, and it is the thing this post is really about, which is how quickly a way of splitting up work hardens into a standing rule about who gets trusted with what.

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


<details style="margin: 1.2em 0; padding: 0.7em 1em; background: #f8fafc; border: 1px solid #cbd5e1; border-radius: 6px;">
<summary style="cursor: pointer; font-weight: 600; color: #334155;">The full catalogue: 43 delegation patterns in nine families (reference, safe to skip)</summary>

<p style="font-size: 0.9em; color: #64748b; margin: 0.6em 0 0.4em 0;">Grouped so related patterns sit together. Each entry covers what it is, when to reach for it, how it fails, and how it differs from its neighbours. The argument picks up again below.</p>

<div class="gov-list">

The simplest arrangements just move work forward in a line, each agent seeing what the last one produced. Everything more elaborate is built out of these.

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
<div class="gov-body">Start with the cheapest capable agent. If it fails or confidence is low, escalate to a more capable (and expensive) one. A cost-optimization strategy that exploits the observation that most tasks are easy.<br><br><em>When to use:</em> cost-sensitive systems with variable difficulty. Support desks, model cascades, any setting where 80% of queries are straightforward and only 20% need a frontier model.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2305.05176" target="_blank" rel="noopener" class="red-link">FrugalGPT</a> demonstrates that cascading through models of increasing capability, stopping as soon as confidence is high enough, can reduce costs substantially while preserving quality. The same principle applies across hardware tiers: a smart speaker's keyword spotter handles "set a timer," escalates to the on-device language model for "summarize my morning," and reaches a cloud frontier model only for "draft a response to this legal notice."<br><br><em>Failure mode:</em> bad confidence estimates cause over-escalation (expensive) or under-escalation (poor quality). If the small model does not know what it does not know, escalation fails silently. When the entire escalation chain fails (no agent can resolve the task, or the human-in-the-loop is unavailable), the task needs a dead-letter path: park it, log it, surface it asynchronously. Without this, unresolvable tasks either loop forever or vanish silently.<br><br><em>Relation to other patterns:</em> Escalation is vertical routing (by capability level) while Router is horizontal routing (by domain). They compose naturally.</div>
</details>

<details>
<summary>Relay / Handoff <span class="gov-tagline">pass control, caller exits</span></summary>
<div class="gov-body">Full control and context transfer: the caller hands off to another agent and terminates. Unlike Chain, where the caller waits for a result, the Relay agent is done once it hands off. Ownership transfers completely.<br><br><em>When to use:</em> agent-to-agent handoffs where the originator should not persist. Language routing (user speaks French, hand off to the French-speaking agent), domain transitions (general assistant hands to specialist), shift changes in long-running tasks.<br><br><em>Example:</em> OpenAI's Agents SDK uses handoffs as a first-class primitive: an agent calls a "transfer_to_X" tool, passing execution context to agent X. The originating agent ceases to exist in the conversation.<br><br><em>Failure mode:</em> dropped context at the handoff boundary. If the receiving agent does not get enough context, it starts from scratch. Ambiguous ownership: who is responsible for the outcome after handoff?<br><br><em>Relation to other patterns:</em> Relay is a Chain where the sender terminates after handoff rather than waiting for a return. It becomes interesting as a building block when agents join and leave societies throughout a session.</div>
</details>

<details>
<summary>Loop / Retry-with-Context <span class="gov-tagline">same agent tries again, using its own failures as context</span></summary>
<div class="gov-body">An agent re-executes the same task with feedback from previous attempts appended to its context, iterating until a stopping condition is satisfied. The key structural distinction from the Evaluator pattern is that no separate critic agent exists; the same agent revises its own output, using prior failures as additional context rather than as external scores.<br><br><em>When to use:</em> Tasks where quality improves with iteration and where failure feedback is cheap to generate, such as code generation, structured extraction, or chain-of-thought reasoning. Best suited when the output space is well-defined enough that the agent can recognize improvement.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2310.03714" target="_blank" rel="noopener" class="red-link">DSPy</a>'s optimizer (Khattab et al. 2023) reruns a task with prior attempts as context until a quality threshold is met. AutoGen's reflection pattern similarly prompts a single agent to critique and rewrite its previous response before returning a final answer.<br><br><em>Failure mode:</em> Without a hard iteration cap and explicit convergence criteria, the loop runs indefinitely. Agents can also converge to locally coherent but globally wrong outputs, cycling between the same two bad solutions without progress.<br><br><em>Relation to other patterns:</em> Evaluator adds a structurally separate critic, making quality judgment an external check rather than self-assessment. Checkpoint/Saga checkpoints intermediate states for recovery, whereas Loop/Retry-with-Context does not persist state across iterations.</div>
</details>

Break a task into pieces, hand each piece down, and collect the results back up, and you have a hierarchy. These carry a danger that is easy to miss. Suppose each hop preserves most of what the original requester meant, but not all of it. Those small losses multiply with depth, which means a sufficiently deep tree can be working hard, reporting success, and solving something nobody asked for. Tomašev et al. (2026) call the version of this that matters for assigning blame an "accountability vacuum." The practical lesson is blunt: the shallowest tree that works is also the safest one.

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
<div class="gov-body">An orchestrator agent receives a task, dynamically decomposes it into sub-tasks (the decomposition is not predetermined), dispatches to workers, monitors progress, and re-plans when sub-tasks fail or new information arrives. The structure emerges at runtime.<br><br><em>When to use:</em> open-ended tasks where the decomposition cannot be known in advance. Complex coding tasks, research questions, any problem where the first attempt reveals what the next steps should be.<br><br><em>Example:</em> Anthropic's <a href="https://www.anthropic.com/engineering/building-effective-agents" target="_blank" rel="noopener" class="red-link">orchestrator-workers</a> pattern: the orchestrator plans, dispatches sub-tasks to specialized workers, synthesizes results, and re-plans when needed. Microsoft's <a href="https://arxiv.org/abs/2411.04468" target="_blank" rel="noopener" class="red-link">Magentic-One</a> uses this pattern with a lead agent that tracks progress and reassigns work dynamically.<br><br><em>Failure mode:</em> the orchestrator misjudges the decomposition and re-plans endlessly. Without a budget or iteration limit, orchestration can become an infinite loop of planning.<br><br><em>Relation to other patterns:</em> Orchestrator is a dynamic Tree. It differs from Supervisor (which reviews output but does not dynamically re-decompose) and from Chain (which has a fixed sequence). The boundary between Orchestrator and Tree is whether the decomposition is known at the start.</div>
</details>

<details>
<summary>Mission Command <span class="gov-tagline">intent without method</span></summary>
<div class="gov-body">The delegating agent communicates the goal and the reason (intent) but deliberately does not prescribe the method. Subordinate agents exercise autonomous judgment in selecting how to achieve the intent, adapting to conditions without waiting for instructions.<br><br><em>When to use:</em> delegating to capable agents in situations where the delegator cannot anticipate local conditions. Complex coding tasks ("make this module thread-safe" rather than "add a lock on line 47"), research tasks ("find the root cause" rather than "check these three files"), any task where method prescription would be counterproductive.<br><br><em>Example:</em> every good prompt engineer uses mission command instinctively. "Write tests for this module that cover edge cases" is mission command. "Open test_module.py, add a function called test_edge_case_1 that asserts..." is not. The deliberate withholding of method specification is the key innovation. The pattern is essential for embodied agents: tell a delivery robot "get this package to room 312" rather than prescribing every turn, because the robot knows the building and the corridor conditions better than you do.<br><br><em>Failure mode:</em> intent ambiguity. If the goal is vague, autonomous agents may pursue reasonable but divergent interpretations. The quality of the intent statement determines the quality of the outcome.<br><br><em>Relation to other patterns:</em> Mission Command is the philosophical complement to Supervisor. The Supervisor reviews output after the fact; Mission Command shapes behavior before execution by specifying intent clearly enough that autonomous execution is safe. Source: military doctrine (Prussian Auftragstaktik, NATO Allied Joint Publication, US Army FM 6-0).</div>
</details>

<details>
<summary>Feudal Delegation <span class="gov-tagline">goals cascade down, results bubble up, methods stay hidden</span></summary>
<div class="gov-body">Higher-level agents assign abstract goals to lower-level agents without specifying or observing how those goals are implemented. Authority flows downward as goal assignments; results flow upward as outcomes. The implementing agent's reasoning, tool calls, and intermediate steps are fully opaque to its principal. The hierarchy coordinates on objectives, not methods.<br><br><em>When to use:</em> Systems that need to scale across many sub-tasks without the orchestrator holding the full reasoning trace in context. Appropriate when sub-agents are trusted specialists and when outcome verification is feasible even if process inspection is not.<br><br><em>Example:</em> Tomašev et al. (2026) discuss opaque goal-based delegation as a core case in multi-agent systems. A manager assigns a team a quarterly goal and sees only the quarterly outcome, never the team's day-to-day decisions.<br><br><em>Failure mode:</em> Because the principal cannot inspect execution, misalignment is undetectable until it surfaces in outcomes. A sub-agent that optimizes a measurable stand-in rather than the actual goal may report success while violating the principal's real intent. The opacity compounds with depth: each hidden hop adds a layer where intent can drift unobserved.<br><br><em>Relation to other patterns:</em> Tree differs in that the orchestrator decomposes and tracks each sub-step explicitly. Mission Command expects agents to report back on their chosen method. Feudal Delegation is the most opaque of the three; it assumes sub-agents are competent and aligned without verifying either.</div>
</details>

Piling on more agents does not by itself make an answer better. What makes it better is how the work gets checked, and there are more ways to check it than you would expect: constructively, adversarially, by vote, by an independent third party, or by a protocol that verifies a result without redoing it.

<details>
<summary>Evaluator <span class="gov-tagline">generate, critique, refine</span></summary>
<div class="gov-body">A generator produces output; a separate evaluator assesses quality and provides constructive feedback; the generator revises. The loop repeats until the evaluator is satisfied or a budget is exhausted. This is constructive iteration, not adversarial attack.<br><br><em>When to use:</em> quality-sensitive outputs where iterative improvement is worth the cost. Code generation, writing, reasoning tasks, any output that benefits from review and revision.<br><br><em>Example:</em> Anthropic's evaluator-optimizer pattern: the evaluator provides assessment and suggestions, the optimizer revises accordingly. The evaluator sees both the original task and the current output, enabling targeted feedback.<br><br><em>Failure mode:</em> infinite refinement loops where the evaluator keeps finding minor issues. Over-filtering where the evaluator rejects creative or novel approaches that do not match its quality model. Evaluator capture where the generator learns to satisfy the evaluator's preferences rather than the actual task.<br><br><em>Relation to other patterns:</em> Evaluator differs from Critic in being *constructive* rather than *adversarial*. The Evaluator says "this could be better because..."; the Critic says "this fails because..." Both improve output, but through different mechanisms.</div>
</details>

<details>
<summary>Voting <span class="gov-tagline">same task, majority wins</span></summary>
<div class="gov-body">Multiple agents work on the same task independently. The final answer is selected by majority vote or aggregation. Redundancy as a strategy for reliability.<br><br><em>When to use:</em> high-stakes decisions where no single agent is reliable enough. Medical diagnosis confirmation, safety-critical classifications, any domain where the cost of a wrong answer exceeds the cost of running multiple agents.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/2402.05120" target="_blank" rel="noopener" class="red-link">"More Agents Is All You Need"</a> demonstrates that even simple sampling and majority voting can improve LLM performance, with gains growing as ensemble size increases (most steeply on harder tasks).<br><br><em>Failure mode:</em> correlated errors. If all voters share the same training data, architecture, and biases, they will be wrong in the same way. Voting only helps when errors are independent.<br><br><em>Relation to other patterns:</em> Voting is the simplest form of ensemble. It differs from The Agora (governance) in being a delegation pattern for a specific decision rather than an ongoing deliberative structure for setting policy.</div>
</details>

<details>
<summary>Critic / Red Team <span class="gov-tagline">generate, then attack</span></summary>
<div class="gov-body">A generator produces output; a separate critic adversarially attacks it. The generator must survive the attack. If the critic finds flaws, the generator revises. This is the adversarial verification pattern.<br><br><em>When to use:</em> security review, red-teaming, adversarial robustness testing, any output that must withstand scrutiny before release.<br><br><em>Example:</em> a code agent generates a security-sensitive function; a red-team agent attempts SQL injection, XSS, and other attacks against it. Only code that survives the attack proceeds.<br><br><em>Failure mode:</em> critic too strong (nothing passes, system is paralyzed) or too weak (everything passes, the critic is theater). Calibrating critic severity is the key design challenge.<br><br><em>Relation to other patterns:</em> Critic differs from Evaluator in being *adversarial* rather than *constructive*. The Evaluator suggests improvements; the Critic tries to break things. Both can be combined in sequence: generate, evaluate, revise, then red-team the final version.</div>
</details>

<details>
<summary>Debate <span class="gov-tagline">agents argue opposing sides; a judge rules</span></summary>
<div class="gov-body">Two or more agents argue opposing positions over multiple rounds, with a judge agent evaluating the arguments. Unlike Critic (single-round attack on one output), Debate is iterative: each side responds to the other's arguments, and the judge observes the exchange before ruling. The key insight from AI safety research (<a href="https://arxiv.org/abs/1805.00899" target="_blank" rel="noopener" class="red-link">Irving et al., 2018</a>) is that even if neither debater is fully trustworthy, the competitive dynamic can surface truths that neither would volunteer alone.<br><br><em>When to use:</em> high-stakes reasoning where single-agent confidence is insufficient, alignment verification, policy decisions, any setting where adversarial pressure improves the quality of reasoning rather than just testing robustness.<br><br><em>Example:</em> two agents argue whether a proposed code change introduces a security vulnerability. Agent A argues it is safe (citing the input validation); Agent B argues it is unsafe (citing an edge case in unicode handling). A judge agent evaluates both arguments across three rounds and rules. The multi-round structure lets each side address the other's strongest point.<br><br><em>Failure mode:</em> judge capture (the judge is persuaded by style rather than substance), arguments that loop without converging, agents that optimize for sounding persuasive rather than being correct, and asymmetric capability (a stronger debater wins regardless of position).<br><br><em>Relation to other patterns:</em> Debate extends Critic from single-round to multi-round and from asymmetric (attacker/defender) to symmetric (both sides argue). It differs from Voting (which aggregates independent answers) in requiring iterative engagement. Composes with Witness (judge as independent arbiter) and Constitutional Republic governance (judicial branch as debate-based dispute resolution).</div>
</details>

<details>
<summary>Witness / Notarization <span class="gov-tagline">independent third-party verification</span></summary>
<div class="gov-body">A third-party agent, independent of both producer and consumer, certifies that output meets a standard before it is accepted. The witness does not produce or consume; it only verifies. This creates trust without requiring the consumer to trust the producer directly.<br><br><em>When to use:</em> multi-party systems where producer and consumer belong to different organizations or have different trust levels. Cross-organizational agent interactions, high-stakes outputs, regulatory compliance.<br><br><em>Example:</em> a medical agent generates a treatment recommendation. Before it reaches the patient's agent, an independent clinical guidelines agent verifies that the recommendation is consistent with current evidence. The patient's agent trusts the witness, not the recommender directly.<br><br><em>Failure mode:</em> witness becomes a bottleneck or a single point of trust failure. If the witness is compromised, all certified outputs are suspect. Multiple independent witnesses mitigate this but add cost.<br><br><em>Relation to other patterns:</em> Witness is the trust primitive for multi-party delegation. It differs from Evaluator (which is part of the production loop) and Critic (which is adversarial). The Witness is structurally independent and its only role is attestation.</div>
</details>

<details>
<summary>Verification Game <span class="gov-tagline">trust a result without repeating the full computation</span></summary>
<div class="gov-body">Two or more agents follow a structured protocol to verify a result without the verifier repeating the full computation. One party produces a result; another checks it with pointed challenges ("show me step N") rather than redoing the work, backed by incentives that make honest reporting the winning move. The verifier checks the claim, not the process that produced it.<br><br><em>When to use:</em> Computationally expensive tasks where re-execution is prohibitive, or distributed settings where no single agent can be fully trusted. Particularly relevant when the verification cost must be asymmetrically lower than the production cost.<br><br><em>Example:</em> Optimistic rollup protocols in blockchain systems use this structure: execution is assumed correct and only challenged when a verifier disputes the result, at which point a bisection game resolves the disagreement. Tomašev et al. (2026) discuss analogous trust-establishment protocols for multi-agent delegation.<br><br><em>Failure mode:</em> If the prover and verifier can collude, both benefit from approving incorrect results. The game-theoretic equilibrium breaks down when the cost of honest verification exceeds the reward, or when the agents share a principal whose interest is not aligned with accurate verification.<br><br><em>Relation to other patterns:</em> Evaluator re-runs or scores the output directly; Verification Game avoids re-execution. Voting aggregates independent opinions; Verification Game is a structured protocol between specific participants, not an aggregation of independent assessments.</div>
</details>

Then there is the unglamorous business of things going wrong, where the goal is a chain that degrades instead of collapsing.

<details>
<summary>Circuit Breaker <span class="gov-tagline">stop calling failing agents</span></summary>
<div class="gov-body">An agent monitors the failure rate of calls to a downstream agent. When failures exceed a threshold, it "opens the circuit" and stops calling that agent entirely, returning errors immediately. After a cool-down period, it enters a "half-open" state and tests with a single call. If it succeeds, the circuit closes; if it fails, it opens again.<br><br><em>When to use:</em> any agent network where one failing agent could cascade into system-wide failure. API integrations, tool calls to external services, sub-agent delegation where the sub-agent may be overloaded or down.<br><br><em>Example:</em> a travel-booking agent calls an airline API. The API starts timing out. After three consecutive failures, the circuit breaker opens and the agent immediately falls back to cached results or an alternative provider, rather than waiting for more timeouts. In physical systems: an autonomous vehicle's perception agent monitors a failing lidar sensor; after repeated bad reads, the circuit opens and the agent falls back to camera-based depth estimation and cached map data.<br><br><em>Failure mode:</em> circuit opens too aggressively (transient errors trigger full cutoff) or too slowly (failing calls accumulate before protection kicks in). Tuning thresholds is critical.<br><br><em>Relation to other patterns:</em> Circuit Breaker is the defensive complement to Timeout. Timeout detects absence of signal; Circuit Breaker detects accumulated failure. Both are essential for resilient agent networks. Source: microservices resilience (Netflix Hystrix, resilience4j).</div>
</details>

<details>
<summary>Timeout / Dead-Man Switch <span class="gov-tagline">absence triggers response</span></summary>
<div class="gov-body">The system expects a signal (heartbeat, result, acknowledgment) within a time window. If the signal does not arrive, the timeout triggers a response: escalation, fallback, alert, or termination. The *non-event* is the event.<br><br><em>When to use:</em> every real deployment needs liveness monitoring. Long-running agent tasks, external API calls, sub-agent delegation where the sub-agent may hang or crash silently.<br><br><em>Example:</em> a research agent dispatches a sub-agent to crawl a website. If no result arrives within 30 seconds, the timeout triggers: the sub-agent is terminated and the task is re-routed to an alternative approach. The pattern predates software agents: watchdog timers in embedded firmware, heartbeat monitors in pacemakers, and safety cutoffs in industrial robot arms all use the same logic. Absence of a signal is the signal.<br><br><em>Failure mode:</em> timeout too short (kills slow but productive work) or too long (wastes time waiting for dead agents). Dynamic timeout adjustment based on task type is the mature approach.<br><br><em>Relation to other patterns:</em> Timeout is the safety primitive that makes autonomous delegation possible. Without it, a single hung agent can block an entire workflow. It composes with Checkpoint/Saga (timeout triggers rollback) and Escalation (timeout triggers escalation to a more capable agent).</div>
</details>

<details>
<summary>Checkpoint / Saga <span class="gov-tagline">recover or compensate on failure</span></summary>
<div class="gov-body">Two related recovery styles for multi-step work. <em>Checkpointing</em> saves state at intervals and restores the last good snapshot on failure. The <em>Saga</em> style, used when steps have irreversible external side effects, gives each step an explicit compensating action instead: if a later step fails, the system runs those compensating actions in reverse order to undo the prior steps (there is no saved state to revert to; each undo is a new forward action). The two are paired here because they answer the same question, how to recover a partially completed workflow, and a real system often uses both. The Saga half is borrowed from distributed transaction design.<br><br><em>When to use:</em> long-running workflows where partial failure must not corrupt the overall result. Multi-file code refactors, multi-step data migrations, workflows involving external side effects that may need reversal.<br><br><em>Example:</em> an agent refactors a database schema: step 1 creates the new table, step 2 migrates data, step 3 updates the API, step 4 drops the old table. If step 3 fails, the saga compensates by reverting steps 2 and 1.<br><br><em>Failure mode:</em> rollback storms when many steps need compensation simultaneously. Checkpoint overhead on fast, short tasks where the cost of checkpointing exceeds the cost of restarting.<br><br><em>Relation to other patterns:</em> Checkpoint/Saga adds reliability to any sequential pattern (Chain, Pipeline). It is the delegation-level primitive that enables Timeout at the system level.</div>
</details>

<details>
<summary>Canary / Shadow <span class="gov-tagline">test new agents on live traffic safely</span></summary>
<div class="gov-body">A new (canary) agent runs alongside the incumbent on real inputs, but its outputs are not served to users. A judge compares canary outputs against the incumbent's. Only after the canary demonstrates statistically equivalent or better performance over many requests is it promoted to replace the incumbent.<br><br><em>When to use:</em> safe rollout of new agent versions, model upgrades, prompt changes, or entirely new specialist agents. Any setting where you cannot fully validate quality offline and need production traffic to build confidence.<br><br><em>Example:</em> a customer-support team replaces GPT-4 with a fine-tuned smaller model. For two weeks, both models answer every ticket. A quality judge scores both. The fine-tuned model is promoted only after its scores meet a statistical threshold across 10,000 tickets.<br><br><em>Failure mode:</em> shadow period too short (promotes a model that performs well on easy cases but fails on rare hard ones). Shadow period too long (wastes compute running two agents indefinitely). Also misleading scores if the canary's inputs during shadowing differ from what it will see once promoted.<br><br><em>Relation to other patterns:</em> Canary differs from Speculative Execution (which races for speed on a single task) in being a long-running evaluation over many tasks. It composes naturally with Witness (the judge is a form of witness) and Circuit Breaker (automatic rollback if the canary's error rate spikes).</div>
</details>

Work does not have to be assigned at all. It can be competed for, through bidding, bargaining, or simply racing several attempts and taking whichever finishes first.

<details>
<summary>Auction <span class="gov-tagline">bid for work</span></summary>
<div class="gov-body">A task is announced; agents bid based on their capabilities, cost, and availability. The best bid wins the contract. Market-based task allocation that lets the system discover the most efficient assignment without central planning.<br><br><em>When to use:</em> task allocation across heterogeneous specialists where capabilities and costs vary. Open marketplaces, dynamic team assembly, any setting where you want competition to drive quality and efficiency.<br><br><em>Example:</em> the <a href="https://en.wikipedia.org/wiki/Contract_Net_Protocol" target="_blank" rel="noopener" class="red-link">Contract Net Protocol</a> (Smith, 1980) formalized the bidding and award: managers announce tasks, contractors bid, the best bid wins. (The Contract Net card below carries the award through execution and reporting.)<br><br><em>Failure mode:</em> bid gaming (agents misrepresent capabilities), race to the bottom on quality (cheapest bid wins regardless of quality), and winner's curse (the winning bidder systematically underbids).<br><br><em>Relation to other patterns:</em> Auction is delegation by competition. It is the task-level mechanism behind Market governance. Combined with Pipeline, it creates the Bidding Pipeline composition.</div>
</details>

<details>
<summary>Negotiation <span class="gov-tagline">bilateral offer/counter-offer</span></summary>
<div class="gov-body">Two agents, each representing a principal, exchange offers and counter-offers until they reach agreement or declare impasse. Unlike Auction (many bidders, one winner), Negotiation is bilateral: two parties with opposing interests seeking a zone of possible agreement (ZOPA). Each agent has a mandate (what it can offer), hard limits (its walkaway point), and strategy (how aggressively to push).<br><br><em>When to use:</em> any setting where two principals need to reach a deal through their agents. Contract terms, pricing, service levels, resource allocation between organizations, consumer-to-business disputes.<br><br><em>Example:</em> your personal agent negotiates a lower internet bill with the telecom's retention agent. Your agent knows your usage patterns and competing offers; the telecom's agent knows its retention budget and your lifetime value. They exchange offers until one accepts or escalates to a human.<br><br><em>Failure mode:</em> deadlock (neither agent concedes), no ZOPA (the principals' constraints are incompatible but the agents waste rounds discovering this), principal misrepresentation (an agent bluffs beyond its mandate), and collusion (agents agree on terms that serve themselves rather than their principals).<br><br><em>Relation to other patterns:</em> Negotiation is the bilateral case of Auction. It composes with Relay (handoff after agreement), Witness (third party certifies the deal), and Checkpoint (agreement is binding, violation triggers rollback). Under Custodianship governance, each negotiating agent has fiduciary obligation to its principal.</div>
</details>

<details>
<summary>Speculative Execution <span class="gov-tagline">race approaches, first valid wins</span></summary>
<div class="gov-body">Multiple agents attempt the same task simultaneously using different approaches. The first valid result is committed; the rest are discarded. Trading compute for latency. (In computer architecture "speculative execution" means running work before knowing if it is needed, as in branch prediction; the pattern here is closer to what Google calls a <em>hedged request</em>.)<br><br><em>When to use:</em> latency-sensitive tasks where multiple valid approaches exist and you cannot predict which will be fastest. Code generation (try different algorithms), creative tasks (try different styles), search (try different strategies).<br><br><em>Example:</em> a user asks for an optimized sort function. Three agents try different approaches (quicksort, mergesort, radix sort). The first to pass the test suite wins. The others are cancelled.<br><br><em>Failure mode:</em> wasted compute on discarded branches. Also: committing a fast but incorrect result if the validity check is too weak.<br><br><em>Relation to other patterns:</em> Speculative Execution is the opposite of Escalation (which tries one at a time). It trades cost for speed. It differs from Voting in that agents try *different* approaches rather than the *same* approach.</div>
</details>

<details>
<summary>Contract Net <span class="gov-tagline">broadcast, bid, award, execute, report</span></summary>
<div class="gov-body">A full lifecycle protocol where a manager agent broadcasts a call-for-proposals describing a task, bidding agents submit bids based on capability and availability, the manager awards the contract to the best bidder, the winner executes and reports completion, and the winner may recursively sub-contract portions of the task to other agents. This is the complete coordination cycle, not just its bidding phase.<br><br><em>When to use:</em> Open multi-agent systems where task requirements and agent capabilities are heterogeneous and not known in advance. Useful when no central registry of agent skills exists and when workload must be dynamically distributed across available agents.<br><br><em>Example:</em> <a href="https://en.wikipedia.org/wiki/Contract_Net_Protocol" target="_blank" rel="noopener" class="red-link">Reid G. Smith</a> formalized this protocol in 1980 as the Contract Net Protocol; it was later standardized by FIPA as a foundation for multi-agent coordination. Modern LLM-based orchestration frameworks implicitly replicate parts of this lifecycle when routing tasks across specialized agents.<br><br><em>Failure mode:</em> Recursive sub-contracting creates unbounded delegation depth. A winning agent that cannot complete the task sub-contracts it, and that agent sub-contracts further, generating chains that are difficult to monitor, attribute, or terminate cleanly.<br><br><em>Relation to other patterns:</em> Auction covers only the bidding and award phases; Contract Net extends the protocol through execution and reporting. Feudal Delegation omits bidding entirely; authority flows by assignment, not by competitive proposal.</div>
</details>

Capability itself can move between agents, which is what stops every newcomer from having to learn the job from nothing.

<details>
<summary>Teacher-Student <span class="gov-tagline">strong trains weak</span></summary>
<div class="gov-body">A capable model transfers knowledge to a less capable one through examples, corrections, or traces. The student improves over time; eventually it may handle tasks that previously required the teacher.<br><br><em>When to use:</em> model distillation, capability transfer, generating fine-tuning data, bootstrapping specialists from generalists.<br><br><em>Example:</em> a frontier model generates high-quality code reviews; these become training data for a smaller, faster model that handles routine reviews at a fraction of the cost. The teacher creates the student's curriculum.<br><br><em>Failure mode:</em> distribution mismatch between teacher examples and real tasks. The student may learn the teacher's biases and blind spots alongside its strengths.<br><br><em>Relation to other patterns:</em> Teacher-Student is the delegation pattern behind capability transfer. It differs from Federated Learning (where knowledge flows among many peers via a coordinator) in being hierarchical and one-directional.</div>
</details>

<details>
<summary>Federated Learning <span class="gov-tagline">collaborative training, no data sharing</span></summary>
<div class="gov-body">Agents train on their local private data and send only model weight updates (not raw data) to a coordinator. The coordinator aggregates updates into a global model and redistributes it. No agent ever sees another agent's training data.<br><br><em>When to use:</em> privacy-preserving collaborative improvement. Multi-organization agent systems where data cannot leave organizational boundaries. Healthcare networks, financial institutions, any setting where agents improve collectively but data sovereignty matters.<br><br><em>Example:</em> hospital agents across a network each learn from local patient interactions. They share model updates (not patient data) with a coordinator that produces a better global model, which is redistributed. Each hospital's patients benefit from the collective learning without any privacy violation. The original motivating case was simpler: many mobile devices improving a shared model (for text entry and speech) by sending model updates rather than raw user data (<a href="https://arxiv.org/abs/1602.05629" target="_blank" rel="noopener" class="red-link">McMahan et al. 2016</a>). The same principle extends to any fleet of edge devices: wearable health monitors learning from collective patterns without sharing personal biometrics, or agricultural drones improving crop-disease detection across farms without transmitting proprietary field data.<br><br><em>Failure mode:</em> poisoned updates from a malicious participant can corrupt the global model. Non-IID data distributions across participants can cause the global model to perform poorly for some participants.<br><br><em>Relation to other patterns:</em> Federated Learning is the privacy-preserving counterpart to Teacher-Student (which shares knowledge directly). It inverts the flow: agents do the training, the coordinator only aggregates. Source: distributed ML (<a href="https://arxiv.org/abs/1602.05629" target="_blank" rel="noopener" class="red-link">McMahan et al. 2016</a>).</div>
</details>

<details>
<summary>Blackboard <span class="gov-tagline">shared workspace, indirect coordination</span></summary>
<div class="gov-body">Agents contribute partial results to a shared workspace. Any agent can read the current state and add to it. Coordination happens through the artifact, not through direct messages between agents. The decomposition is not fixed in advance; agents contribute when they have something useful.<br><br><em>When to use:</em> collaborative problem-solving where agents have heterogeneous capabilities and the task structure is not known in advance. Research collaboration, incident response, complex debugging.<br><br><em>Example:</em> a security incident: one agent adds network logs, another adds suspicious process activity, a third adds threat intelligence matches, a fourth synthesizes a timeline from everything on the blackboard. No agent directed the others; the shared workspace coordinated them. Another example: agents from different users collaborate on the same code repository. One agent writes a PR, another reviews it, a third runs the CI pipeline, a fourth checks style conformance. The repository is the blackboard; each agent reads current state and contributes without direct coordination.<br><br><em>Failure mode:</em> the blackboard becomes noisy: too many partial results with no curation. Without some mechanism for relevance filtering, later agents drown in irrelevant contributions.<br><br><em>Relation to other patterns:</em> Blackboard enables heterogeneous agents to collaborate without shared protocols; the shared artifact IS the protocol. It differs from Stigmergy in the structure of contributions: Blackboard contributions are typically explicit and structured (named entries), while stigmergic traces are implicit side effects of other work. Both patterns can be deliberately designed.</div>
</details>

<details>
<summary>Distillation <span class="gov-tagline">train a small model on a large one's outputs, then run without it</span></summary>
<div class="gov-body">A large or expensive model generates outputs, reasoning traces, or labeled examples that are used to fine-tune a smaller model. Once training is complete, the student operates independently; there is no ongoing connection to the teacher. The knowledge transfer happens at training time, not at inference time.<br><br><em>When to use:</em> Deployments where inference cost, latency, or privacy constraints make the teacher model unsuitable for production, but where a smaller model can approximate the teacher's behavior on the relevant task distribution. Common in production systems that prototype with frontier models and deploy fine-tuned smaller models.<br><br><em>Example:</em> <a href="https://arxiv.org/abs/1503.02531" target="_blank" rel="noopener" class="red-link">Hinton et al. 2015</a> introduced the formal framework for knowledge distillation using soft label transfer. More recently, frontier model outputs have been used to fine-tune smaller open-weight models for specific instruction-following tasks, with the teacher never invoked at inference time.<br><br><em>Failure mode:</em> The small model is trained on examples the large one chose, which may not cover the inputs it later meets in production. It performs well on those it saw and fails silently on the long tail it did not.<br><br><em>Relation to other patterns:</em> Teacher-Student involves real-time instructional interaction at inference time; the teacher remains active. Distillation severs that connection after training. Loop/Retry-with-Context iterates at inference time; Distillation iterates at training time.</div>
</details>

Coordination does not actually require anyone in charge. Agents under resource pressure can work out how to cooperate through nothing but local interaction, each responding only to its neighbours, none of them holding a picture of the whole. This is old ground outside AI: Holland (*Hidden Order*, 1995) and Kauffman (*The Origins of Order*, 1993) showed that interacting agents under constraints reliably produce structure nobody designed. What follows is that phenomenon, running in agent networks.

<details>
<summary>Publish-Subscribe <span class="gov-tagline">event-driven, fully decoupled</span></summary>
<div class="gov-body">Agents publish typed events to named topics without knowing who will receive them. Subscriber agents declare interest in topics and receive matching events asynchronously. Publishers and subscribers are fully decoupled: neither knows the other exists.<br><br><em>When to use:</em> large-scale agent coordination where point-to-point wiring would be impractical. Event-driven architectures, notification systems, any setting where many agents need to react to the same events.<br><br><em>Example:</em> in a trading system, a market-data agent publishes price updates to a "prices" topic. Hundreds of strategy agents subscribe and react independently. Adding a new strategy agent requires zero changes to the publisher. In a smart building, occupancy sensors publish to a "room-state" topic; HVAC agents, lighting agents, and cleaning-scheduling agents each subscribe and adapt independently.<br><br><em>Failure mode:</em> message storms when popular topics generate more events than subscribers can process. Lost messages if the broker fails. Debugging is hard because the event flow is implicit.<br><br><em>Relation to other patterns:</em> Pub-Sub differs from Gossip (which actively pushes to random peers without a broker) in using structured topic-based routing through a central broker. Source: event-driven architecture (Kafka, RabbitMQ).</div>
</details>

<details>
<summary>Choreography <span class="gov-tagline">decentralized event-driven coordination</span></summary>
<div class="gov-body">Agents coordinate by reacting to events published by their peers, each following local event-driven rules. No central controller holds the full workflow. The resulting coordination pattern is distributed across the participants rather than encoded in any single agent's logic. Choreography can be deliberately designed (a developer specifies which events each agent publishes and subscribes to) or can arise when agents independently learn to react to each other's outputs.<br><br><em>When to use:</em> systems where centralized orchestration would be a bottleneck or single point of failure. Large-scale agent ecosystems, cross-organizational coordination, any setting where no single agent should control the workflow.<br><br><em>Example:</em> in a microservices system, when an order is placed, the order service emits an "OrderPlaced" event. The inventory service reacts by reserving stock, the payment service charges the card, the shipping service schedules delivery. Each service's event contracts were designed, but no orchestrator directs the flow at runtime.<br><br><em>Failure mode:</em> the workflow is invisible. When something goes wrong, no single agent has the full picture. Debugging distributed choreographies requires distributed tracing. Deadlocks are possible when agents wait for events that depend on each other circularly.<br><br><em>Relation to other patterns:</em> Choreography is the decentralized alternative to Orchestrator. Where Orchestrator centralizes decomposition, Choreography distributes it: each agent's publish/subscribe rules replace a central plan. Both can be designed; neither is inherently more or less deliberate. Source: SOA/microservices architecture, event-driven systems.</div>
</details>

<details>
<summary>Stigmergy <span class="gov-tagline">environment as communication</span></summary>
<div class="gov-body">Agents coordinate without direct communication by reading and modifying a shared environment. Environmental traces (artifacts, logs, cached results, pheromone-like signals) trigger subsequent behavior in other agents. A deployer can deliberately set up stigmergic coordination (design the environment, define what traces agents leave) or it can arise naturally when agents happen to share an environment and begin reacting to each other's modifications.<br><br><em>When to use:</em> large heterogeneous agent populations with no shared protocol. Collaborative knowledge building, open-source development patterns, systems where agents come and go unpredictably.<br><br><em>Example:</em> Wikipedia is stigmergic: editors modify articles (the shared environment), and other editors react to those modifications by further editing, citing, or reverting. The artifact mediates all coordination. In robotics, warehouse robots leave digital markers on a shared floor map: "aisle 7 congested," "shelf B3 restocked." Other robots read these traces and reroute without direct robot-to-robot communication. The robots were designed to work this way; Wikipedia's version emerged from editing practice, not deliberate design.<br><br><em>Failure mode:</em> environmental noise. Without curation, traces accumulate into an unusable mess. Also vulnerable to environmental poisoning: a malicious agent can leave misleading traces that redirect subsequent agents.<br><br><em>Relation to other patterns:</em> Stigmergy differs from Blackboard in the structure of the shared space. Blackboard contributions are typically structured and explicit (an agent writes a named entry); stigmergic traces are typically implicit and incidental (an agent modifies the environment as a side effect of its work, and others react). Both can be designed. Source: swarm intelligence (Grassé 1959), collaborative editing, ant colony optimization.</div>
</details>

<details>
<summary>Swarm <span class="gov-tagline">collective behavior, no central plan</span></summary>
<div class="gov-body">Multiple agents produce collective behavior without centralized control. The coordination mechanism varies: it can be local interaction rules, shared objectives, imitation, environmental signals, or any combination. What defines a swarm is the absence of a central plan, not the specific mechanism that replaces it. A deployer can deliberately engineer a swarm (specify agent behaviors, deploy them, let collective patterns form) or a swarm can form spontaneously when agents discover that decentralized coordination outperforms waiting for instructions.<br><br><em>When to use:</em> exploration, creative search, resilient systems where agent failure should not disrupt the collective. Distributed search, open-ended research, any setting where you want robust collective behavior without a single point of failure.<br><br><em>Example:</em> a fleet of search-and-rescue drones after an earthquake. Each drone scans its area, shares findings with neighbors, and the swarm converges on likely survivor locations. The deployer designed the individual agent behaviors; the collective search pattern was not centrally planned. Another: a research swarm where each agent explores a different approach, shares findings, and adjusts strategy. Promising directions attract more agents naturally.<br><br><em>Failure mode:</em> if the swarm's collective behavior drifts from the intended goal, there is no built-in central mechanism to correct course. Swarms can converge on locally optimal but globally poor solutions without any agent recognizing the problem.<br><br><em>Relation to other patterns:</em> Swarm differs from Choreography (where agents react to specific typed events with defined contracts) in having less structured interaction. It differs from Gossip (which is specifically about information propagation) in being broader: swarm agents coordinate behavior, not just spread data. Swarm becomes Colony governance when the collective patterns persist and begin constraining future behavior.</div>
</details>

<details>
<summary>Gossip <span class="gov-tagline">peer-to-peer information spread</span></summary>
<div class="gov-body">Agents spread information by telling their neighbors, who tell their neighbors, in an epidemic pattern. No broadcast, no central hub. In well-connected networks, information propagates through repeated local contact, eventually reaching the whole population.<br><br><em>When to use:</em> decentralized coordination where broadcast would be expensive or infeasible. Norm propagation, state synchronization across large agent populations, failure detection in distributed systems.<br><br><em>Example:</em> in a large agent network, when one agent discovers a useful tool or strategy, it shares with its direct contacts. They share with theirs. Within a few rounds, the entire network has the information, without any single agent needing to broadcast. In sensor mesh networks, this is how fault detection spreads: one node detects anomalous vibration in a bridge pylon, tells its neighbors, and the alert propagates through the mesh without any central monitoring server.<br><br><em>Failure mode:</em> rumor drift (information mutates as it passes through agents, like a game of telephone) and norm poisoning (a malicious agent injects false information that spreads unchecked).<br><br><em>Relation to other patterns:</em> Gossip differs from Pub-Sub (which uses a central broker with topic-based routing) in being fully decentralized and epidemiological. It is a common communication substrate in Colony governance.</div>
</details>

Who is even allowed to hand work to whom, and with what powers attached? These next few patterns are constraints rather than shapes, in that they sit on top of whatever structure you already have and bound what it may do. Tomašev et al. (2026) treat this as a first-class concern rather than a detail, and the reason is worth stating: skip it and a system does not end up with no authority structure. It ends up with one nobody wrote down, which auditors cannot see and nobody answers for when it fails.

<details>
<summary>Privilege Attenuation <span class="gov-tagline">you can only grant what you hold, never more</span></summary>
<div class="gov-body">A structural constraint on all delegation patterns: when an agent sub-delegates, it may grant only a strict subset of its own permissions to the sub-agent. Authority cannot be amplified at any link in the chain. An agent with read-only file access cannot grant write access. An agent authorized for one data scope cannot delegate access to a broader scope. This is a property of the delegation infrastructure, not a pattern agents choose to apply.<br><br><em>When to use:</em> Any multi-agent system where sub-agents interact with external resources, user data, or APIs. Privilege attenuation should be enforced at the runtime or framework level so that agents cannot circumvent it by accident or by design.<br><br><em>Example:</em> Tomašev et al. (2026) identify this as a core requirement for safe intelligent delegation. The principle of least privilege in operating systems enforces the same constraint: processes cannot escalate their own privileges by spawning child processes.<br><br><em>Failure mode:</em> The practical failure is the reverse: each hop strips permissions too conservatively, so the agent actually doing the work lacks the access it needs. The system grinds to a halt not from over-permission but from under-permission at the far end.<br><br><em>Relation to other patterns:</em> Privilege Attenuation applies as a constraint within Feudal Delegation, Contract Net, and any other pattern that creates delegation chains. Capability Credentials make the current permission set verifiable; Privilege Attenuation governs how that set changes across hops.</div>
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
<summary>Zone of Indifference <span class="gov-tagline">instructions an agent follows without pushback; outside it, friction begins</span></summary>
<div class="gov-body">The set of instructions an agent executes without scrutiny, pushback, or negotiation. Within the zone, delegation flows smoothly because the agent treats the instruction as within the normal scope of its role. Outside the zone, the agent questions, negotiates, or refuses. The zone's boundaries are determined by the agent's understanding of its role, its values, and its assessment of the instruction's legitimacy. Chester Barnard introduced this concept in organizational theory in 1938 to explain why employees comply with managerial directives without evaluating each one individually.<br><br><em>When to use:</em> As an analytical lens for understanding why some delegation chains proceed without friction and others produce deadlock or refusal. The concept is useful for diagnosing where in a chain execution stalls and for calibrating agent compliance thresholds.<br><br><em>Example:</em> An agent configured for customer support will execute routine reply tasks without scrutiny but may refuse or escalate a request to send a message containing a refund amount above a set threshold. The threshold defines the zone boundary.<br><br><em>Failure mode:</em> Zones too wide cause agents to execute harmful instructions without questioning, creating safety failures. Zones too narrow cause agents to challenge routine instructions, creating throughput failures. Calibration is the hard problem.<br><br><em>Relation to other patterns:</em> Human-in-the-Loop Gate and Approval Escalation can be understood as mechanisms for handling instructions that fall outside the zone. Policy Delegation defines the zone explicitly through written constraints.</div>
</details>

Somewhere in all of this sits a person, and where exactly turns out to be a design decision with real teeth. What stops and waits for a human to approve it? Who is watching, and what is alarming enough to interrupt them? Every option here is working around the same wall: your attention does not scale with the number of agents. Nobody reviews a thousand decisions a day. So each of these patterns is really a different bet about which decisions are worth a human's time, and each bet fails differently when it is wrong.

<details>
<summary>Human-in-the-Loop Gate <span class="gov-tagline">execution blocks until a human approves</span></summary>
<div class="gov-body">Execution pauses at a predefined checkpoint and waits for explicit human approval before proceeding. The blocking actor is a human, not another agent. The gate is synchronous: the system cannot continue until approval is received. The location of the gate, the information presented to the human, and the approval granularity are design choices that determine how much the gate actually protects versus how much it taxes the human's attention.<br><br><em>When to use:</em> High-stakes, low-frequency decisions where human judgment is irreplaceable and where the cost of a wrong decision outweighs the throughput penalty of waiting. Suitable for actions that are difficult or impossible to reverse, such as sending external communications, committing financial transactions, or deploying changes to production.<br><br><em>Example:</em> CrewAI's human input step and LangGraph's interrupt nodes both implement this pattern, pausing graph execution at a node and resuming only after a human provides input. OpenAI's Agents SDK supports similar approval checkpoints in tool-use flows.<br><br><em>Failure mode:</em> Gate fatigue. When gates fire frequently or present too much information to evaluate quickly, humans approve everything without reading. The gate exists structurally but provides no actual oversight. This is the automation complacency failure in its active rather than passive form.<br><br><em>Relation to other patterns:</em> Human-on-the-Loop is the non-blocking counterpart: the human can intervene but execution does not wait. Approval Escalation routes only anomalies to humans; the gate fires at predefined structural points regardless of content.</div>
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
<div class="gov-body">A meta-pattern in which the delegation structure changes in real-time based on performance signals, without requiring human reconfiguration. Routing shifts as agents demonstrate competence. Agents are promoted to harder tasks or demoted when performance drops. New specialist slots are created when the system detects unserved query types. The system transitions between delegation patterns, from Escalation to Router, from Supervisor to Market, without human intervention. The adaptation mechanism operates on the architecture, not on individual task plans.<br><br><em>When to use:</em> Long-running systems where the task distribution is non-stationary and where no fixed delegation structure will remain optimal over time. Requires sufficient volume of performance signal to distinguish genuine improvement from noise, and a stable enough objective to define what "better" means across structure changes.<br><br><em>Example:</em> DSPy optimizers modify prompt and routing configurations based on downstream metrics, instantiating this pattern at the prompt level. <a href="https://www.adaptionlabs.ai/" target="_blank" rel="noopener" class="red-link">Adaption Labs</a> explores similar ideas at the agent orchestration level. Part 1 of this series describes the <a href="/posts/2026/agent-fabric-part1/#the-adaptive-fabric">Adaptive Fabric</a> as the mechanism that makes a society self-organizing rather than statically configured.<br><br><em>Failure mode:</em> Oscillation: the system switches patterns without settling, thrashing between structures as noisy signals flip the adaptation criterion. Premature optimization: a structure is locked in before enough data exists to justify it. The architecture-to-institution trap: the adaptation mechanism itself becomes a governance structure, with its own authority, its own failure modes, and its own need for oversight, whether or not any of that was intended (see "From architecture to institution" above).<br><br><em>Relation to other patterns:</em> Orchestrator re-plans individual tasks dynamically within a fixed structure; Adaptive Delegation changes the structure itself across tasks over time. Every other pattern in this taxonomy can be a target or a source state in an Adaptive Delegation transition. This is the delegation-level manifestation of Part 1's five adaptation surfaces (data, model, environment, coordination, interface).</div>
</details>


</div>

</details>

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

Now that the patterns are in view, here are the structural traps that emerge from combining them poorly. Each has a predictable cause, a specific interaction between agent properties and system design that makes the failure likely rather than accidental. (These are design anti-patterns, distinct from the adversarial "agent traps" of Franklin, Tomašev et al. (2025), which are malicious inputs rather than self-inflicted structure; [Part 3](/posts/2026/agent-fabric-part3/) takes those up.)

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

That catalogue is the vocabulary. What it does not tell you is what happens when these patterns start nesting inside each other, which is where the interesting behaviour and most of the cost lives. Start with the simplest version of nesting, which is **agents that spawn other agents**. A user asks a question. The primary agent realizes it needs help: a web search, a code execution, a document summary. It delegates. The sub-agent might delegate further. A tree forms.

This is already happening. Multi-agent frameworks like <a href="https://arxiv.org/abs/2308.08155" target="_blank" rel="noopener" class="red-link">AutoGen</a>, <a href="https://arxiv.org/abs/2303.17760" target="_blank" rel="noopener" class="red-link">CAMEL</a>, and <a href="https://arxiv.org/abs/2308.00352" target="_blank" rel="noopener" class="red-link">MetaGPT</a> assign distinct roles to agents and coordinate them through structured workflows. Production systems, from <a href="https://corporate.zalando.com/en/technology/more-personal-and-smarter-zalando-assistant-enhanced-capabilities-inspire-customers" target="_blank" rel="noopener" class="red-link">Zalando Assistant</a> to Microsoft's <a href="https://arxiv.org/abs/2411.04468" target="_blank" rel="noopener" class="red-link">Magentic-One</a>, use orchestrator agents that plan, track progress, and re-plan when sub-agents fail. Worth noting how modest the real ones are, though: today's production trees run two or three levels deep, not twenty.

This is harder than it sounds. When we first tried heterogeneous model delegation in the Zalando Assistant, the results were mixed. Models of different sizes and capabilities did not compose cleanly, and failures at one level cascaded unpredictably. Today, with better tool-calling conventions and agent-to-agent protocols, the same approach works far more reliably. The infrastructure caught up with the idea.

As tasks grow more complex, the cascade deepens. A frontier model dispatches to medium specialists, who dispatch to small local models, who dispatch to tiny classifiers. The cascade can span hardware tiers. A cloud frontier model reasons about strategy, dispatches to an on-device model running on your phone for local context, which dispatches to a microcontroller for sensor reading. The cost of a query is not determined by one model. It is determined by the *shape of the tree* it spawns.

No real system runs on a single pattern, and the useful ones are recipes rather than ingredients. Chain a router to an escalation ladder and an evaluator loop and you have a quality gate, whose entire philosophy is to route cheaply, escalate rarely, and verify always. A different combination buys something else: fan the same task out to several agents and make them vote before anything gets merged, and now your reliability no longer depends on trusting any one of them. Or hold a pipeline's stages fixed while auctioning off each slot, so the shape stays deliberately designed but who fills it is settled by competition. Figure 3 animates all three.

<div class="viz-container">
  <div id="viz-combo" style="width: 100%; height: 420px;" role="img" aria-label="Three combined delegation patterns shown side by side: Quality Gate, Consensus Engine, and Bidding Pipeline."></div>
  <div class="viz-caption"><strong>Figure 3. Composition: how delegation archetypes combine.</strong> Three examples of combining archetypes into production-grade patterns: a quality gate (router + evaluator loop), a consensus engine (fan-out + majority vote), and a bidding pipeline (fixed stages awarded to competing agents).
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;"><em>Left: The Quality Gate.</em> Red packets enter a router; easy queries go to a small specialist (cheap path), ambiguous ones escalate to a frontier model. An evaluator loops back if quality is insufficient. Most queries take the cheap path. <em>Centre: The Consensus Engine.</em> A task splits at the red dispatcher node; independent agents (blue) solve sub-problems; orange verdict nodes compare answers by majority vote; green merge node reassembles the final result. <em>Right: The Bidding Pipeline.</em> Fixed stages (Ingest, Parse, Enrich, Store) are awarded to competing agents; the winner lights up below each stage, and a green packet flows through the assembled pipeline.</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-combo').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

Look at a working system and the same decompose, dispatch, aggregate motion repeats at every level, which makes delegation look pleasingly fractal until you check the two ends. At the leaves the pattern stops, because someone has to do the irreducible work. At the root it stops too, because that is where a human's intent enters. In between, what determines whether the whole thing performs is not the quality of any single agent but how ruthlessly the tree prunes itself. The best systems are the ones that know when not to branch.

Anthropic's field report on <a href="https://www.anthropic.com/engineering/building-effective-agents" target="_blank" rel="noopener" class="red-link">building effective agents</a> draws a useful distinction between **workflows** (predefined code paths) and **agents** (systems where the LLM dynamically directs its own process). Their experience suggests starting with an optimized single LLM call and adding multi-agent orchestration only when simpler solutions demonstrably fail. Each delegation archetype above is a composable building block, not a monolithic architecture. Coordination does not require the coordinator to be the strongest model. A small coordinator that learns how to delegate can orchestrate larger LLMs (see <a href="https://arxiv.org/abs/2512.04695" target="_blank" rel="noopener" class="red-link">TRINITY</a>).

All of this costs money, and the money shapes the architecture more than any design document does. Every extra level of delegation multiplies both cost and latency, which turns into a brutally simple selection pressure: a tree that answers in a hundred tokens survives the budget review that kills the one needing ten thousand for the same answer. Over time that pressure has a direction. The winner is never the deepest tree, it is the shallowest one that reliably produces a good enough answer, and in high-volume work something delivering 95% of the quality at 10% of the cost beats the perfectionist every time.

<div style="background: #fffbeb; border: 1px solid #d97706; border-radius: 6px; padding: 0.6em 1em; margin: 1em 0; font-size: 0.9em;">
<strong>Established vs. hypothesized.</strong> Scaling laws for individual models are well-characterized (Kaplan et al. 2020, Hoffmann et al. 2022). The extension to delegation trees (diminishing returns at the tree level, selection pressure toward shallow trees, cost-driven specialization) is a working hypothesis informed by production experience but not yet empirically validated at scale. The specialist-market thesis below has stronger support (Hooker 2025, Mixture-of-Agents) but the equilibrium between specialization and consolidation remains an open empirical question.
</div>

That same pressure decides who sits at each node. Today's frontier models are generalists, and generalism is expensive, so the cheaper move is often a small specialist that does one narrow thing well. Push that far enough and the advantage stops belonging to the largest model and starts belonging to the best-composed ensemble, which is a strange thing to sit with (<a href="https://arxiv.org/abs/2406.04692" target="_blank" rel="noopener" class="red-link">Mixture-of-Agents</a>). How far it actually goes is genuinely unsettled, for reasons the box above and the section below both take seriously. (Part 1's [resource ecology](/posts/2026/agent-fabric-part1/#the-resource-ecology) is the wider version of this argument.)

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">The specialist market: routing, evidence, and infrastructure</summary>

<div class="viz-container" style="margin-top: 1em;">
  <div id="viz-specialists" style="width: 100%; height: 420px;" role="img" aria-label="Marketplace visualization with four routers on the left dispatching queries to twelve specialist agents, tools, and knowledge bases on the right."></div>
  <div class="viz-caption"><strong>Figure 4. The specialist marketplace.</strong> Routers send requests to agents (circles), tools (rectangles), and knowledge bases (diamonds) under access-control constraints. Edge thickness reflects traffic; dashed locked lines show denied access.</div>
</div>

<p style="margin-top: 0.5em; color: #555;">The evidence for specialization is growing in two directions. First, teams work better when they are assembled for the task rather than fixed in advance: dynamically selecting which agents participate based on the query type outperforms static configurations (e.g., <a href="https://arxiv.org/abs/2310.02170" target="_blank" rel="noopener" class="red-link">DyLAN</a>). Second, explicit roles and workflows often outperform unstructured agent conversation (e.g., <a href="https://arxiv.org/abs/2308.00352" target="_blank" rel="noopener" class="red-link">MetaGPT</a>). The lesson is not "use more agents." It is "match structure to task."</p>

<p style="color: #555;">In practice, routing is where the pattern gets interesting. In our work on the Zalando Assistant, we found that LLM-based routing mechanisms could match or beat specialized semantic models on recall: an LLM deciding "this query needs the fashion expert" often outperformed a dedicated classifier trained for exactly that task. Precision, however, told a different story: the LLM router was more liberal, sending queries to specialists that did not need them. This points to a design principle that recurs throughout multi-agent systems: <strong>match model power to task difficulty</strong>. Not every routing decision needs a frontier model. A fast, cheap classifier handles the easy cases; the expensive reasoner handles only the ambiguous ones.</p>

<p style="color: #555;">The resource ecology is not only about distillation; it is also about routing and cascades. Cost-aware cascades show that the routing decision can be as important as the model decision: choosing when to call a cheap model, when to escalate, and when to stop can reduce cost while preserving quality (e.g., <a href="https://arxiv.org/abs/2305.05176" target="_blank" rel="noopener" class="red-link">FrugalGPT</a>). Agent societies generalize this from model selection to task allocation, verification, and memory reuse.</p>

<p style="color: #555;">This section develops the federation-not-consolidation scenario: if the future is many specialist providers rather than a few dominant platforms, the machinery below is what routes work among them. If consolidation wins at the platform layer instead (a real possibility we return to shortly), much of this infrastructure stays internal to a handful of large providers rather than becoming an open market. With that bet stated, at sufficient scale, specialization becomes a <strong>market</strong>. Agents advertise capabilities. Others route queries to the best specialist. Pricing emerges, not in currency, but in compute. A fast, accurate specialist attracts traffic. A slow, unreliable one does not. A functioning specialist market needs six layers:</p>

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

<p style="color: #555;">The bet here is that the space of possible tasks is large enough, and the cost differential between specialists and generalists steep enough, that specialization remains economically rational at scale. Hooker (<a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=5877662" target="_blank" rel="noopener" class="red-link">2025</a>) documents this trend systematically: Llama-3 8B outperforms Falcon 180B, Aya 8B outperforms BLOOM 176B despite having only 4.5% of the parameters. Smaller, better-trained specialists are winning, not because scale does not matter, but because algorithmic improvements, data curation, and distillation are compounding faster than raw compute. This is an unresolved empirical question with testable predictions. If specialist markets win, we should see: routing-layer startups emerging, inter-model protocol adoption accelerating, and cost-per-task declining faster than cost-per-token. If consolidation wins, we should see: platform bundling increasing, specialist model companies being acquired rather than growing independently, and API switching costs rising. The likely outcome is a layered ecology: a few large platforms, many specialist models, and routing markets at the boundary. If this layered outcome holds, the governance question is not "specialists or generalists?" but "who controls the routing layer between them?"</p>

<p style="color: #555;"><strong>No single model needs to be good at everything.</strong> A tiny code model plus a large reasoner plus a fast retrieval model, composed well, can outperform a monolithic model that tries to do all three. Weak models can still be valuable when their mistakes are different. A failed attempt by one model may expose a path another model can exploit. Search over model choices and refinements turns diversity into an asset: the system learns not only which answer is good, but which agent is useful for which kind of failure (for a concrete mechanism using tree search across models, see <a href="https://sakana.ai/ab-mcts/" target="_blank" rel="noopener" class="red-link">AB-MCTS</a>).</p>

<p style="color: #555;">An early version of this pattern used one language model as a controller over many specialist models: parse the request, select the right specialist (vision, speech, generation), execute subtasks, then synthesize the result (see <a href="https://arxiv.org/abs/2303.17580" target="_blank" rel="noopener" class="red-link">HuggingGPT</a>). Language becomes the interface for cooperation between minds of very different kinds.</p>

<p style="color: #555;">Bidding for work has an old lineage (the <a href="https://en.wikipedia.org/wiki/Contract_Net_Protocol" target="_blank" rel="noopener" class="red-link">Contract Net Protocol</a> from 1980 formalized it; see the Auction archetype above). A specialist market also needs settlement. If agents bid for work, someone must pay the compute bill, enforce quotas, and decide whether the result was worth the cost. Settlement makes the six layers above operational rather than aspirational. In early systems, "pricing" may simply mean token budgets, latency budgets, rate limits, or priority access rather than currency.</p>

<p style="color: #555;">Communication is not free. Today, agents communicate through natural language: verbose, ambiguous, expensive. At small scale this works; at large scale, it becomes a bottleneck. A specialist market needs a protocol layer for discovery (how do I find a specialist?), authentication (are you who you claim to be?), negotiation (what will this cost?), permissioning, and result formatting (how do I parse your output?).</p>

<p style="color: #555;"><a href="https://modelcontextprotocol.io/" target="_blank" rel="noopener" class="red-link">MCP</a> standardizes how AI systems connect to tools and data sources; <a href="https://a2a-protocol.org/" target="_blank" rel="noopener" class="red-link">A2A</a> points toward agent-to-agent interoperability. Neither, by itself, solves the whole problem of an open specialist market. The <a href="https://arxiv.org/abs/2410.11905" target="_blank" rel="noopener" class="red-link">Agora protocol</a> (not to be confused with the Agora governance archetype in [Part 3](/posts/2026/agent-fabric-part3/)) frames this as an Agent Communication Trilemma: versatility, efficiency, and portability pull against one another. The likely future is not one universal protocol, but a stack: tool access, agent identity, delegation, settlement, provenance, and audit. A comprehensive <a href="https://arxiv.org/abs/2504.16736" target="_blank" rel="noopener" class="red-link">survey of agent protocols</a> maps the current landscape.</p>
</details>

## From Architecture to Institution

A delegation pattern becomes an institution when it shapes future behavior. When an evaluator's judgments affect who gets trusted next time, that is governance. When a benchmark determines which model gets deployed, that too is governance, whether anyone designed it as such or not.

A concrete case makes the difference sharp. Passing a code review down a chain of agents is delegation, pure and simple. But the rule about which model is trusted with security-sensitive refactors, and the question of who is allowed to change that rule, is something else. That is governance, and notice it is not a step in any particular task; it is a standing constraint on all of them.

Mostly this has not happened yet, and it is worth saying so plainly. Production agent systems are still largely stateless between sessions, which means they never accumulate the performance history that would harden into anything. But the pieces are arriving: long-term memory, evaluation logs, routing analytics. Once a system remembers across sessions, what follows is not speculation so much as arithmetic.

Watch it happen to a coding agent. It starts about as simply as possible, with an orchestrator dispatching to a code writer, the code writer handing off to a test runner, results flowing back. Nothing resembling governance anywhere. Then the orchestrator starts keeping score, because keeping score is useful: which model produces fewer bugs, which test runner catches more regressions, which scanner flags real vulnerabilities instead of noise. That score becomes a reputation, and the reputation starts deciding who gets trusted with what.

Look at what that system now is. Every decision still funnels through one central orchestrator, and standing among the workers is earned by track record. Part 3 has names for both of those arrangements, but the names matter less than this: nobody chose either one. They condensed out of accumulated experience while the team was busy shipping features.

The coding agent is not unique, and the path is the same wherever you look: operational data accumulates, the data shapes routing, and routing with a memory becomes authority.

It is worth being precise about why that last step counts as authority rather than just good optimization. A load balancer forgets last week's latencies, which is why nobody would call it a governing body. But a fleet that will not route real work to an unproven agent until it clears a threshold it learned on its own is enforcing a standing rule that no human wrote down, and it will keep enforcing it tomorrow. The preference has outlived the task that produced it.

Three more examples make that concrete. They illustrate where current trajectories point rather than documenting deployed systems, since the transition depends on persistence infrastructure that is still arriving.

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

None of which makes emergence bad in itself. A structure that forms around genuine, demonstrated competence usually serves the people relying on it; one that drifts into quiet collusion does not, and both can emerge by exactly the same process. What separates them is not their origin but whether anyone can push back once they exist, which requires actual mechanisms: a way to object, a way to leave, a way to split off. Governance you cannot challenge is tyranny whether somebody designed it or not, and the emergent kind is worse for being invisible while it happens.

This has a practical consequence. If governance arrives out of delegation history whether you plan for it or not, then the choice was never *whether* to have it. The choice is between governance you can name, inspect, and overrule, and governance that assembled itself in the gaps of your logging and shows up only in its downstream effects.

---

So this is the real cost of delegating. The obvious bill is the one you can measure: every hop multiplies compute, latency, and the chance that intent leaks a little further from what was asked. That bill is why delegation trees stay shallow, why specialists beat generalists on narrow work, and why the shape of a production system is set as much by budget as by design.

The bill that arrives later is authority. Split work among agents, keep records of how it went, act on those records, and you have built an institution without filing any paperwork. It will have preferences you did not choose, standing you did not assign, and a memory that outlives the task that created it. Which raises the question this post has now earned and cannot answer: what kinds of governance are there, which one are you accidentally running, and who exactly enforces it? That is [The Agent Fabric (Part 3): Ruling an Agent Society](/posts/2026/agent-fabric-part3/).



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

</script>
