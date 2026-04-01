---
title: "MapReader: Open software for the visual analysis of maps"
date: 2024-09-01
author: "Rosie Wood, Kasra Hosseini, Kalle Westerling, Andrew Smith, Kaspar Beelen, Daniel C. S. Wilson, Katherine McDonough"
venue: "Journal of Open Source Software"
paper_url: "https://doi.org/10.21105/joss.06434"
paper_label: "JOSS"
github_url: "https://github.com/maps-as-data/MapReader"
paper_categories: ["AI", "Computer Vision", "Software Engineering"]
draft: false
bibtex: |
  @article{Wood2024,
    doi = {10.21105/joss.06434},
    url = {https://doi.org/10.21105/joss.06434},
    year = {2024},
    publisher = {The Open Journal},
    volume = {9},
    number = {101},
    pages = {6434},
    author = {Wood, Rosie and Hosseini, Kasra and Westerling, Kalle and Smith, Andrew and Beelen, Kaspar and Wilson, Daniel C. S. and McDonough, Katherine},
    title = {MapReader: Open software for the visual analysis of maps},
    journal = {Journal of Open Source Software}
  }
---

<div style="text-align: center;">
<img src="/images/papers/mapreader-joss/mapreader-joss-teaser.png" alt="MapReader Software Architecture" style="max-width: 85%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
MapReader is an open-source software library that transforms how researchers extract information from large image collections, particularly historical maps. This diagram illustrates the modular pipeline architecture and data flow through two core tasks: patch classification (dividing images into small cells and classifying visual features) and text spotting (detecting and recognizing text). Starting from input images (top), users can download maps, annotate patches manually, train computer vision models, and perform inference at scale. The flexible pipeline accommodates both small manually-annotated datasets and large-scale automated analysis, as demonstrated by processing approximately 30.5 million patches in one study. Inspired by biomedical imaging methods and adapted for historians, MapReader has proven its versatility by successfully transferring to plant phenotype research, showcasing the power of open and reproducible research methods. This release, developed through the Living with Machines project, includes extensive documentation and tutorials designed to make large-scale visual map analysis accessible to historians and researchers across disciplines.
</div>

## Summary

MapReader is an interdisciplinary software library for processing digitized maps and other types of images with two tasks: patch classification and text spotting. Patch classification works by 'patching' images into small, custom-sized cells which are then classified according to the user's needs. Text spotting detects and recognizes text. MapReader offers a flexible pipeline which can be used both for manual annotation of small datasets as well as for computer-vision-based inference of large collections. As an example, in one study, we annotated 62,020 patches, trained a suite of computer vision models and performed model inference on approximately 30.5 million patches.

MapReader's approach was inspired by methods in biomedical imaging, which were adapted for use by historians, and it is suitable for a wide range of applications in image analysis: it has, for example, been applied to an image classification problem in plant phenotype research. This cross-pollination between the humanities and the natural sciences was made possible by the open and reproducible research methods at the heart of MapReader.

MapReader pioneers a methodological shift in how historians interact with maps as primary sources. Sustained engagement with big collections of maps rarely moves beyond analysis of cartographic history. To change this, MapReader encourages historians to reflect on the content of maps and is designed to facilitate linking datasets representing visual map content with other historical geospatial data to enable spatial historical research.

In this paper, we present the MapReader release at the conclusion of the Living with Machines project, which supported the initial development of the software and associated historical research. This release represents the culmination of extensive work to improve MapReader's usability among historians, especially through clear documentation and tutorials.

**Keywords:** computer vision, digital humanities, historical maps, image classification, text recognition, open source software, patch classification
