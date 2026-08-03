---
title: "The Agent Fabric (Part 2): Delegation, and What It Costs"
subtitle: "How work gets split among agents, and how splitting it quietly creates authority nobody granted"
date: 2026-07-31
author: "Kasra Hosseini and Maria Tsekhmistrenko"
post_categories: ["AI"]
tags: ["AI", "AI agents", "multi-agent systems", "LLM", "agent fabric", "governance", "the loom hypothesis"]
description: "How splitting work among agents quietly creates authority nobody granted: the four records that turn delegation into governance, and why the cheapest engineering decisions are the ones that build an institution."
draft: false
math: false
ShowToc: true
TocOpen: false
hideCitation: false
wordcount: "~1,150 words (body) · ~2,000 words (notes)"
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
  <div class="viz-caption"><strong>Figure 1.</strong> The same agents four times over, each panel adding one thing: more of them, then memory that survives between tasks, then other groups to deal with. Nobody plans the jump from one panel to the next. Green arrows mark three transitions: <em>scale</em>, <em>persist</em>, <em>interweave</em>.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;">First panel: a single model handles a request end to end. This is how most people use AI today. Second panel: a multi-agent system, where multiple agents coordinate through delegation patterns (chains, trees, routers, swarms). A frontier model dispatches to specialists, sub-agents handle sub-tasks, results flow back. Some multi-agent systems are temporary (assembled for one task), others are persistent infrastructure (a router that runs continuously). Third panel: a society forms when agents persist beyond any single task, developing governance (a GOV node), peer connections, and shared knowledge (KB). Multiple tasks flow through simultaneously. Fourth panel: the fabric, where multiple societies (shown in blue, purple, and green) interconnect through cross-society links (orange). Each society retains its own governance node (G), but resources, protocols, and knowledge flow across boundaries. This post focuses on multi-agent delegation and on the governance that turns multi-agent systems into societies; the fabric was introduced in Part 1.</p>
  </details></div>
</div>

You will probably end up with a handful of personal agents: one managing your calendar, one triaging your messages. Nobody wires them together. Then two of them want opposite things, because the messages agent has something urgent and the calendar says no interruptions. Nothing in that system was ever granted the authority to settle it, and yet it gets settled, because whichever agent you have overridden less often now quietly carries more weight. You never wrote that rule. You voted for it, one dismissal at a time, without knowing there was an election.

Try to give the messages agent more room later and it hesitates anyway, because the record of those dismissals is more consistent than you are. You are now arguing with a policy you wrote by accident.


<div class="viz-container">
  <div id="viz-combo" style="width: 100%; height: 420px;" role="img" aria-label="Three combined delegation patterns shown side by side: Quality Gate, Consensus Engine, and Bidding Pipeline."></div>
  <div class="viz-caption"><strong>Figure 2.</strong> Three arrangements that actually ship. Each is a different way of deciding who gets asked, and each one needs a record to decide it with.
  <details style="margin-top: 0.4em;"><summary style="cursor: pointer; color: #2563eb; font-size: 0.92em;">Full caption</summary>
  <p style="margin-top: 0.4em;"><em>Left: The Quality Gate.</em> Red packets enter a router; easy queries go to a small specialist (cheap path), ambiguous ones escalate to a frontier model. An evaluator loops back if quality is insufficient. Most queries take the cheap path. <em>Centre: The Consensus Engine.</em> A task splits at the red dispatcher node; independent agents (blue) solve sub-problems; orange verdict nodes compare answers by majority vote; green merge node reassembles the final result. <em>Right: The Bidding Pipeline.</em> Fixed stages (Ingest, Parse, Enrich, Store) are awarded to competing agents; the winner lights up below each stage, and a green packet flows through the assembled pipeline.</p>
  </details>
  Click to restart.<br><button class="viz-restart" onclick="document.getElementById('viz-combo').parentElement.querySelector('svg').dispatchEvent(new Event('click'))">Restart</button></div>
</div>

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">The specialist market: routing, evidence, and infrastructure</summary>

<div style="background: #fffbeb; border: 1px solid #d97706; border-radius: 6px; padding: 0.6em 1em; margin: 1em 0; font-size: 0.9em;">
<strong>Established vs. hypothesized.</strong> Scaling laws for individual models are well-characterized (Kaplan et al. 2020, Hoffmann et al. 2022). The extension to delegation trees (diminishing returns at the tree level, selection pressure toward shallow trees, cost-driven specialization) is a working hypothesis informed by production experience but not yet empirically validated at scale. The specialist-market thesis below has stronger support (Hooker 2025; <a href="https://arxiv.org/abs/2406.04692" target="_blank" rel="noopener" class="red-link">Mixture-of-Agents</a>, where several small models answer independently and their outputs are merged) but the equilibrium between specialization and consolidation remains an open empirical question.
</div>

<div class="viz-container" style="margin-top: 1em;">
  <div id="viz-specialists" style="width: 100%; height: 420px;" role="img" aria-label="Marketplace visualization with four routers on the left dispatching queries to twelve specialist agents, tools, and knowledge bases on the right."></div>
  <div class="viz-caption"><strong>Figure 3. The specialist marketplace.</strong> Routers send requests to agents (circles), tools (rectangles), and knowledge bases (diamonds) under access-control constraints. Edge thickness reflects traffic; dashed locked lines show denied access.</div>
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

On a phone the worst this costs is your own afternoon. Move the same arrangement into a company and one thing changes: the record is no longer about the person reading it. It is about a stranger who will never see it, never agreed to it, and has no dismissal to register. You have been that stranger this year, probably several times, and the version below is where it happens.

Somebody writes in because a delivery never arrived. A router reads the message, decides it is a returns question rather than a billing one, and hands it to the agent that handles returns, which sorts it out. The only thing anyone added was logging, because how long each agent takes and how often people write back angry are free to collect. A month later the same router sends complex billing disputes only to the two agents that clear them fastest, and has stopped sending fragile-item returns to the one that keeps misapplying the policy. Nobody reprogrammed it, and the next person whose delivery goes missing is routed by that record rather than by what their problem actually is.


Six weeks in, an engineer called Priya notices that the third agent has not been given a hard ticket since March, and goes looking for the rule that did it. There is no rule. The settings mention the agent only to say it exists, the policy docs say hard tickets go to whoever is best placed to handle them, and three people confirm that nobody changed the routing in March, which is true.

What exists is a table of resolution times. Nobody wrote it and nobody maintains it; it is just what happened, recorded, which is exactly why the team treats it as the one thing in the room that cannot be argued with. The only thing in it she could change is a number, and changing that would be falsifying a record. So she is arguing with something that is not wrong, on behalf of a piece of software, and she can hear how that sounds.

She leaves it. Overriding the routing would make the numbers worse and her the person who made them worse, and nobody there would call the table a rule, which is exactly what makes it one.

What that costs arrives six months on, when the third agent is retired for underperformance. The two survivors are now the only agents with a record on hard billing disputes, so when one of them starts approving refunds it should not, there is nothing left to compare it against.

Priya is in that meeting. She could say the comparison was destroyed by a routing preference nobody chose, and she would be right, and there is nothing she can point at. Which is where you already are with your calendar, minus the standup. You cannot point at the moment you agreed to any of it either.

<details style="margin: 0.8em 0; padding: 0.6em 1em; font-size: 0.9em; background: #eff6ff; border-left: 3px solid #2563eb; border-radius: 0 4px 4px 0;">
<summary style="cursor: pointer; color: #2563eb; font-weight: 600;">Example: Research synthesis pipeline</summary>

A research team builds a Map-Reduce pipeline: multiple agents search different databases, extract claims, and a synthesis agent merges findings into a report. Delegation is explicit: fan out, gather, merge. Over time the synthesis agent notices that one search agent consistently surfaces papers that survive critical review, while another frequently returns retracted or low-impact sources. The synthesis agent begins weighting contributions differently. It gives higher confidence to claims sourced by the reliable agent and flags claims from the unreliable one for manual verification.

This weighting is not a quality filter on individual outputs. It is a standing judgment about an agent's epistemic authority. The synthesis agent now functions as a **peer-review committee of one**, deciding whose testimony counts and how much. When the team adds a new database or swaps a search agent, the synthesis agent's learned weights determine how much that newcomer's findings influence the final report. The pipeline has developed an implicit **Meritocracy**: standing earned through track record, not assigned by design. The institution reveals itself the first time someone asks "why was this source excluded?" and the answer is not a rule but a learned preference.
</details>

---

So here is the one line worth carrying out of this. Nobody gets to choose whether their system has governance, only whether it is the kind you can name and overrule or the kind nobody can find. Priya lost that argument to a spreadsheet, and she was the one person in the building who knew where to look. On your phone there is no spreadsheet and no standup. There is a record you never agreed to, assembled from a hundred one-second decisions you have already forgotten making, and it now governs your attention more reliably than any intention you could state out loud. Nobody at any company decided your calendar agent should win. You elected it.


<div style="background: #f8f8f8; border: 1px solid #e5e5e5; border-radius: 6px; padding: 0.8em 1.2em; margin: 1.5em 0; font-size: 0.95em;">
<strong>The Agent Fabric</strong>, a multi-part blog series on why and how AI agents may form societies and what it means for us.

- **[Prologue: The Anatomy of an Agent](/posts/2026/anatomy-of-an-agent/)**: the loop at the heart of a single agent, and where single-agent recursion breaks
- **[Part 1: Why Agents May Form Societies](/posts/2026/agent-fabric-part1/)**: two observations, the Loom Hypothesis (why isolated agents get woven together), and the path from isolation to interweaving
- **Part 2: Delegation, and What It Costs** (you are here): how work gets split among agents, and how splitting it quietly creates authority nobody granted
- **[Part 3: Ruling an Agent Society](/posts/2026/agent-fabric-part3/)**: governance archetypes, who benefits, who enforces the rules, and how a society gets argued with
- **[Delegation Patterns: A Field Guide](/posts/2026/delegation-patterns-field-guide/)**: the full catalogue of forty-three delegation patterns behind Part 2
- **[Governance Archetypes: A Field Guide](/posts/2026/governance-archetypes-field-guide/)**: the twenty-two governance archetypes behind Part 3
</div>

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
