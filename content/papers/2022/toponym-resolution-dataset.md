---
title: "A Dataset for Toponym Resolution in Nineteenth-Century English Newspapers"
date: 2022-01-24
author: "Mariona Coll Ardanuy, David Beavan, Kaspar Beelen, Kasra Hosseini, Jon Lawrence, Katherine McDonough, Federico Nanni, Daniel van Strien, Daniel C. S. Wilson"
venue: "Journal of Open Humanities Data"
description: "This paper presents a manually annotated dataset of 3,364 toponyms from 343 nineteenth-century English newspaper articles across four locations, designed as a benchmark for developing and evaluating toponym resolution methods on noisy historical text."
paper_url: "https://doi.org/10.5334/johd.56"
paper_label: "Journal of Open Humanities Data"
paper_categories: ["AI", "NLP"]
cover:
  image: "/images/papers/toponym-resolution-dataset/toponym-resolution-dataset-teaser.png"
  hidden: true
draft: false
bibtex: |
  @article{CollArdanuy2022,
    author = {Coll Ardanuy, Mariona and Beavan, David and Beelen, Kaspar and Hosseini, Kasra and Lawrence, Jon and McDonough, Katherine and Nanni, Federico and van Strien, Daniel and Wilson, Daniel C. S.},
    title = {A Dataset for Toponym Resolution in Nineteenth-Century English Newspapers},
    journal = {Journal of Open Humanities Data},
    volume = {8},
    year = {2022},
    month = {01},
    day = {24},
    doi = {10.5334/johd.56},
    url = {https://doi.org/10.5334/johd.56}
  }
---

<div style="text-align: center;">
<img src="/images/papers/toponym-resolution-dataset/toponym-resolution-dataset-teaser.png" alt="Toponym Resolution Dataset Overview" style="max-width: 100%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Unlocking the geography of nineteenth-century British newspapers requires robust methods for identifying and linking place names - a challenging task complicated by OCR errors, historical spelling variations, and local references that assume regional knowledge. This paper introduces a meticulously annotated dataset of 343 articles from four English locations (Manchester, Ashton-under-Lyne, Poole, and Dorchester) spanning 1780-1870, containing 3,364 manually annotated toponyms. Unlike previous datasets, this resource emphasizes the geographical peculiarities of provincial press, where local place names dominate but vary dramatically by region and decade. The table shows the careful distribution of annotations across time and place, revealing how newspaper geography evolved during industrialization. With high inter-annotator agreement (0.87 for detection, 0.89 for linking), this benchmark dataset enables researchers to develop and test toponym resolution methods specifically designed for noisy historical texts with strong local contexts.
</div>

## Abstract

We present a new dataset for the task of toponym resolution in digitized historical newspapers in English. It consists of 343 annotated articles from newspapers based in four different locations in England (Manchester, Ashton-under-Lyne, Poole and Dorchester), published between 1780 and 1870. The articles have been manually annotated with mentions of places, which are linked - whenever possible - to their corresponding entry on Wikipedia. The dataset consists of 3,364 annotated toponyms, of which 2,784 have been provided with a link to Wikipedia. The dataset is published in the British Library shared research repository, and is especially of interest to researchers working on improving semantic access to historical newspaper content.

**Keywords:** benchmark, dataset, geographic information retrieval, newspapers, nineteenth-century English, toponym resolution
