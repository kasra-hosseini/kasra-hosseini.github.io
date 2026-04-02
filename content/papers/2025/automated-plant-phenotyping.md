---
title: "Automated dynamic phenotyping of whole oilseed rape (Brassica napus) plants from images collected under controlled conditions"
date: 2025-01-01
author: "Evangeline Corcoran, Kasra Hosseini, Laura Siles, Smita Kurup, Sebastian Ahnert"
venue: "Frontiers in Plant Science"
description: "This study adapts the MapReader computer vision pipeline to automatically segment and classify five plant structures in whole oilseed rape images, achieving a weighted F1-score of 97.71 and demonstrating the transferability of humanities-developed image analysis tools to agricultural science."
paper_url: "https://www.frontiersin.org/journals/plant-science/articles/10.3389/fpls.2025.1443882"
paper_label: "Frontiers in Plant Science"
paper_categories: ["AI", "Computer Vision", "Natural Sciences"]
cover:
  image: "/images/papers/automated-plant-phenotyping/automated-plant-phenotyping-teaser.png"
  hidden: true
draft: false
bibtex: |
  @article{10.3389/fpls.2025.1443882,
    author = {Corcoran, Evangeline and Hosseini, Kasra and Siles, Laura and Kurup, Smita and Ahnert, Sebastian},
    title = {Automated dynamic phenotyping of whole oilseed rape (Brassica napus) plants from images collected under controlled conditions},
    journal = {Frontiers in Plant Science},
    volume = {16},
    year = {2025},
    url = {https://www.frontiersin.org/journals/plant-science/articles/10.3389/fpls.2025.1443882},
    doi = {10.3389/fpls.2025.1443882},
    issn = {1664-462X}
  }
---

<div style="text-align: center;">
<img src="/images/papers/automated-plant-phenotyping/automated-plant-phenotyping-teaser.png" alt="Automated Plant Phenotyping Classification" style="max-width: 100%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Predicting crop yields under changing climate conditions requires understanding how individual plant components develop over time, but manually measuring leaves, flowers, and pods across thousands of high-resolution images is impractical. This study adapts MapReader, originally developed for analyzing historical maps, to automatically segment and classify plant structures in whole oilseed rape (Brassica napus) images. Panel A shows the original plant image, while panels B-D compare three modeling approaches: 6-label multi-class classification (B), chain of binary classifiers (C), and the top-performing combined approach (D) that first separates plant from background, then classifies five plant structures (branches in light yellow, pods in orange, leaves in red, buds in magenta, flowers in purple). The combined approach achieved macro-averaged F1-score of 88.50 and weighted F1-score of 97.71, matching MapReader's performance on historical maps. This interdisciplinary transfer demonstrates how computer vision methods can cross domains from digital humanities to agricultural science, enabling automated phenotyping that could help ensure future food security by integrating genetic and environmental factors into crop yield models.
</div>

## Abstract

**Introduction:** Recent advancements in sensor technologies have enabled collection of many large, high-resolution plant images datasets that could be used to non-destructively explore the relationships between genetics, environment and management factors on phenotype or the physical traits exhibited by plants. The phenotype data captured in these datasets could then be integrated into models of plant development and crop yield to more accurately predict how plants may grow as a result of changing management practices and climate conditions, better ensuring future food security. However, automated methods capable of reliably and efficiently extracting meaningful measurements of individual plant components (e.g. leaves, flowers, pods) from imagery of whole plants are currently lacking. In this study, we explore interdisciplinary application of MapReader, a computer vision pipeline for annotating and classifying patches of larger images that was originally developed for semantic exploration of historical maps, to time-series images of whole oilseed rape (Brassica napus) plants.

**Methods:** Models were trained to classify five plant structures in patches derived from whole plant images (branches, leaves, pods, flower buds and flowers), as well as background patches. Three modelling methods are compared: (i) 6-label multi-class classification, (ii) a chain of binary classifiers approach, and (iii) an approach combining binary classification of plant and background patches, followed by 5-label multi-class classification of plant structures.

**Results:** A combined plant/background binarization and 5-label multi-class modelling approach using a 'resnext50d_4s2x40d' model architecture for both the binary classification and multi-class classification components was found to produce the most accurate patch classification for whole B. napus plant images (macro-averaged F1-score = 88.50, weighted average F1-score = 97.71). This combined binary and 5-label multi-class classification approach demonstrate similar performance to the top-performing MapReader 'railspace' classification model.

**Discussion:** This highlights the potential applicability of the MapReader model framework to images data across scientific and humanities domains, and the flexibility it provides in creating pipelines with different modelling approaches. The pipeline for dynamic plant phenotyping from whole plant images developed in this study could potentially be applied to imagery from varied laboratory conditions, and to images datasets of other plants of both agricultural and conservation concern.

**Keywords:** automated phenotyping, computer vision, oilseed rape, Brassica napus, MapReader, plant imaging, crop modeling, food security
