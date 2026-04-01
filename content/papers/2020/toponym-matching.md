---
title: "A Deep Learning Approach to Geographical Candidate Selection through Toponym Matching"
date: 2020-11-13
author: "Mariona Coll Ardanuy, Kasra Hosseini, Katherine McDonough, Amrey Krause, Daniel van Strien, Federico Nanni"
venue: "Proceedings of the 28th International Conference on Advances in Geographic Information Systems (SIGSPATIAL)"
paper_url: "https://doi.org/10.1145/3397536.3422236"
paper_label: "SIGSPATIAL"
paper_categories: ["AI", "NLP"]
draft: false
bibtex: |
  @inproceedings{10.1145/3397536.3422236,
    author = {Ardanuy, Mariona Coll and Hosseini, Kasra and McDonough, Katherine and Krause, Amrey and van Strien, Daniel and Nanni, Federico},
    title = {A Deep Learning Approach to Geographical Candidate Selection through Toponym Matching},
    year = {2020},
    isbn = {9781450380195},
    publisher = {Association for Computing Machinery},
    address = {New York, NY, USA},
    url = {https://doi.org/10.1145/3397536.3422236},
    doi = {10.1145/3397536.3422236},
    booktitle = {Proceedings of the 28th International Conference on Advances in Geographic Information Systems},
    pages = {385--388},
    numpages = {4},
    keywords = {Candidate selection, Deep learning, Fuzzy String Matching, Toponym matching},
    location = {Seattle, WA, USA},
    series = {SIGSPATIAL '20}
  }
---

<div style="text-align: center;">
<img src="/images/papers/toponym-matching/toponym-matching-teaser.png" alt="Toponym Matching Evaluation" style="max-width: 70%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
When a historical newspaper mentions "Manchester," does it refer to Manchester, England, or one of the 30+ other Manchesters worldwide? Candidate selection narrows down which entities a recognized place name could plausibly refer to, a critical but often overlooked step before full entity resolution. This paper applies state-of-the-art neural networks to toponym matching, handling the substantial variation that makes place names so challenging: cross-lingual variations (München vs. Munich), regional differences (neighborhood names that don't appear in gazetteers), and OCR errors that corrupt spellings. The evaluation table shows F1 scores across these challenging scenarios in English and Spanish datasets. By improving candidate selection, the method enables more accurate downstream analysis of where historical texts are actually talking about, unlocking the geographic dimension of digitized archives.
</div>

## Abstract

Recognizing toponyms and resolving them to their real-world referents is required to provide advanced semantic access to textual data. This process is often hindered by the high degree of variation in toponyms. Candidate selection is the task of identifying the potential entities that can be referred to by a previously recognized toponym. While it has traditionally received little attention, candidate selection has a significant impact on downstream tasks (i.e. entity resolution), especially in noisy or non-standard text. In this paper, we introduce a deep learning method for candidate selection through toponym matching, using state-of-the-art neural network architectures. We perform an intrinsic toponym matching evaluation based on several datasets, which cover various challenging scenarios (cross-lingual and regional variations, as well as OCR errors) and assess its performance in the context of geographical candidate selection in English and Spanish.

**Keywords:** candidate selection, deep learning, fuzzy string matching, toponym matching, geographical entity resolution, neural networks
