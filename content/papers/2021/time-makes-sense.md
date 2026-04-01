---
title: "When Time Makes Sense: A Historically-Aware Approach to Targeted Sense Disambiguation"
date: 2021-08-01
author: "Kaspar Beelen, Federico Nanni, Mariona Coll Ardanuy, Kasra Hosseini, Giorgia Tolfo, Barbara McGillivray"
venue: "Findings of the Association for Computational Linguistics: ACL-IJCNLP 2021"
paper_url: "https://aclanthology.org/2021.findings-acl.243/"
paper_label: "ACL-IJCNLP 2021"
paper_categories: ["AI", "NLP", "(L)LM"]
draft: false
bibtex: |
  @inproceedings{beelen-etal-2021-time,
    title = "When Time Makes Sense: A Historically-Aware Approach to Targeted Sense Disambiguation",
    author = "Beelen, Kaspar and Nanni, Federico and Coll Ardanuy, Mariona and Hosseini, Kasra and Tolfo, Giorgia and McGillivray, Barbara",
    editor = "Zong, Chengqing and Xia, Fei and Li, Wenjie and Navigli, Roberto",
    booktitle = "Findings of the Association for Computational Linguistics: ACL-IJCNLP 2021",
    month = aug,
    year = "2021",
    address = "Online",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2021.findings-acl.243/",
    doi = "10.18653/v1/2021.findings-acl.243",
    pages = "2751--2761"
  }
---

<div style="text-align: center;">
<img src="/images/papers/time-makes-sense/time-makes-sense-teaser.png" alt="Optimal Date Range for Language Models" style="max-width: 70%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Words change meaning over time, and computational models that ignore this temporal dimension miss crucial context for understanding historical texts. This paper introduces time-sensitive Targeted Sense Disambiguation (TSD), which detects specific word senses in historical documents by accounting for when the text was written. The figure reveals a key insight: optimal date ranges for each language model (measured by F1-score using the sense centroid method) show that matching the model's training period to the target text's era dramatically improves performance. The x-axis represents average points of rolling 100-year quotation date ranges from the Oxford English Dictionary. We train historical BERT models on nineteenth-century English books and create historically evolving sense representations using the OED and its Historical Thesaurus. Results demonstrate that historical language models consistently outperform modern ones, and time-sensitive methods prove especially valuable for older documents - confirming that when it comes to word meaning, time really does make sense.
</div>

## Abstract

As languages evolve historically, making computational approaches sensitive to time can improve performance on specific tasks. In this work, we assess whether applying historical language models and time-aware methods help with determining the correct sense of polysemous words. We outline the task of time-sensitive Targeted Sense Disambiguation (TSD), which aims to detect instances of a sense or set of related senses in historical and time-stamped texts, and address two main goals: 1) we scrutinize the effect of applying historical language models on the performance of several TSD methods and 2) we assess different disambiguation methods that take into account the year in which a text was produced. We train historical BERT models on a corpus of nineteenth-century English books and draw on the Oxford English Dictionary (and its Historical Thesaurus) to create historically evolving sense representations. Our results show that using historical language models consistently improves performance whereas time-sensitive disambiguation helps especially with older documents.

**Keywords:** word sense disambiguation, historical NLP, language evolution, BERT, temporal models, Oxford English Dictionary
