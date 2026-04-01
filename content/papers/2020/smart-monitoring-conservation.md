---
title: "Smart Monitoring for Conservation Areas"
date: 2020-06-05
author: "Kasra Hosseini, Mariona Coll Ardanuy, Daniel J. Patterson, Lorena Garcia-Velez, Lucia Castro-Gonzalez, Lena Deecke, et al."
venue: "Data Study Group Final Report: WWF, The Alan Turing Institute"
paper_url: "https://doi.org/10.5281/zenodo.3878457"
paper_label: "Zenodo"
project_url: "https://wwf-sight.org/exploring-ai-with-the-alan-turing-institute/"
paper_categories: ["AI", "NLP"]
draft: false
bibtex: |
  @techreport{hosseini2020smartmonitoring,
    title={Smart Monitoring for Conservation Areas},
    author={Kasra Hosseini and Mariona Coll Ardanuy and Daniel J. Patterson and Lorena Garcia-Velez and Lucia Castro-Gonzalez and Lena Deecke and others},
    year={2020},
    institution={The Alan Turing Institute},
    type={Data Study Group Final Report: WWF},
    doi={10.5281/zenodo.3878457},
    url={https://doi.org/10.5281/zenodo.3878457}
  }
---

<div style="text-align: center;">
<img src="/images/papers/smart-monitoring-conservation/smart-monitoring-conservation-teaser.png" alt="Smart Monitoring for Conservation Areas" style="max-width: 80%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
The pipeline has two phases. During training (top), articles are vectorised, classified as threat-relevant or not, and evaluated; a smart-annotation loop based on active learning then selects the most informative samples for human review, iteratively expanding both training and validation sets. At prediction time (bottom), the trained classifier labels new, unlabelled news articles, then a downstream module extracts place mentions, organisations, facilities, and dates from the positives before reporting results to WWF. The architecture achieved 96% recall and 82% precision on conservation-threat detection, enabling near-real-time monitoring of emerging risks to protected areas worldwide.
</div>

## Abstract

This report documents the outcomes of a Data Study Group held at The Alan Turing Institute in collaboration with WWF Conservation Intelligence. The challenge focused on developing data science techniques to automatically detect news articles reporting emerging threats to protected areas. The project explored approaches ranging from keyword-based filtering to fine-tuned neural language models (BERT) for classifying news articles as relevant conservation threats, particularly infrastructure developments near protected sites. The best-performing model achieved 96% recall and 82% precision, significantly outperforming baseline approaches and demonstrating the feasibility of real-time, automated conservation threat monitoring using NLP.

**Keywords:** Conservation, WWF, Natural Language Processing, BERT, Threat Detection, Protected Areas
