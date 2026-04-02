---
title: "MapReader: Software and Principles for Computational Map Studies"
date: 2026-01-01
author: "Katherine McDonough, Ruth Ahnert, Kaspar Beelen, Kasra Hosseini, Jon Lawrence, Valeria Vitale, Kalle Westerling, Daniel Wilson, Rosie Wood"
venue: "University of London Press (Early Access)"
description: "This book chapter introduces computational map studies as a field and presents MapReader's patch-based approach to analyzing digitized map collections at scale, arguing that automation and critical humanities interpretation are complementary when software design foregrounds the argumentative nature of maps as historical sources."
paper_url: "https://read.uolpress.co.uk/read/map-reader/section/fbaafbaa-a836-4f13-92a7-e045a7eadbd2"
paper_label: "Book Chapter"
github_url: "https://github.com/maps-as-data/MapReader"
paper_categories: ["AI", "Computer Vision", "Software Engineering"]
cover:
  image: "/images/papers/mapreader-book-chapter/mapreader-book-chapter-teaser.png"
  hidden: true
draft: false
bibtex: |
  @incollection{mcdonough2026mapreader,
    title = {MapReader: Software and Principles for Computational Map Studies},
    author = {McDonough, Katherine and Ahnert, Ruth and Beelen, Kaspar and Hosseini, Kasra and Lawrence, Jon and Vitale, Valeria and Westerling, Kalle and Wilson, Daniel C. S. and Wood, Rosie},
    booktitle = {Living with Machines: Computational Histories of the Age of Industry},
    year = {2026},
    publisher = {University of London Press},
    address = {London},
    isbn = {9781914477652},
    doi = {10.14296/xdyp6338},
    url = {https://read.uolpress.co.uk/read/map-reader/section/fbaafbaa-a836-4f13-92a7-e045a7eadbd2},
    note = {Early Access}
  }
---

<div style="text-align: center;">
<img src="/images/papers/mapreader-book-chapter/mapreader-book-chapter-teaser.png" alt="Ordnance Survey Historical Maps" style="max-width: 100%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Britain's Ordnance Survey created the first comprehensive, detailed picture of Great Britain starting in the early nineteenth century, producing tens of thousands of map sheets across multiple series and editions. Thanks to digitization efforts by the National Library of Scotland, anyone can now browse these collections online, but researchers face a fundamental challenge: how do you analyze thousands of maps simultaneously rather than viewing them sheet by sheet? MapReader addresses this through a radical reimagining of maps as computational data. Rather than manually tracing features into Geographic Information Systems with pixel-level precision, MapReader divides map images into user-defined patches, treating each grid square as a unit for creative labeling and automated classification. This epistemological shift rejects the notion that maps are objective records of landscapes, instead embracing them as historical arguments about space and place. The patch-based approach enables computational map studies at previously impossible scales, revealing spatial patterns across local, regional, and national levels while maintaining the critical interpretive lens essential to humanities inquiry.
</div>

## Summary

MapReader represents an epistemological shift in how historians and humanities scholars engage with digitized map collections at scale. Developed through the Living with Machines project, this chapter introduces computational map studies as a new field that combines scholarly traditions of map interpretation with computational methods designed for analyzing entire collections rather than individual sheets.

At the heart of MapReader's innovation is the "patch" as the unit of analysis. Rather than manually digitizing features with pixel-level precision into Geographic Information Systems (GIS), MapReader imposes a user-defined grid on map images, treating each grid square as a visual frame for classification. This approach fundamentally reframes the research process: patches are labeled iteratively and creatively by researchers during annotation, then classified automatically by trained computer vision models. The method deliberately moves away from reductive approaches that assume maps are objective representations of landscapes, instead embracing maps as historical arguments about space and place.

The software offers two main analytical pathways: patch classification for visual content and text spotting for cartographic text. Unlike traditional GIS databases with rigid relational structures, MapReader outputs data as flat files, providing flexibility for researchers to define their own relationships between map-derived data and other historical sources. This design choice reflects a commitment to experimental, open-ended inquiry rather than predetermined analytical frameworks.

MapReader's development represents several radical departures from existing digital humanities and geospatial tools. First, it maintains a critical approach to maps as data, rejecting the notion that "extracted features" represent objective ground truth about past landscapes. Second, it forgoes building heavyweight database infrastructure in favor of lightweight, flexible outputs. Third, it uses Jupyter notebooks as the interface, exposing the computational process rather than hiding it behind polished user interfaces. This transparency serves both pedagogical goals (teaching computer vision and machine learning through hands-on practice) and research reproducibility goals aligned with open science principles.

The chapter positions MapReader within an emerging "Open Maps" ecosystem, distinguishing it from two prior communities working with maps and computer vision: geographic information scientists treating maps as objective spatial observations, and humanities scholars building Historical GIS or "mirror worlds" of historic cities. MapReader charts a different course by foregrounding the interpretive, argumentative nature of both maps and the datasets derived from them.

Through case studies from the Ordnance Survey digitization project at the National Library of Scotland, the authors demonstrate how MapReader enables asking questions at previously impossible scales. Tens of thousands of map sheets can be analyzed simultaneously, revealing spatial patterns across local, regional, and national levels. The tool has already expanded beyond its initial use case, with applications in plant phenotyping and a growing international community of contributors.

Fundamentally, MapReader offers a new methodological pipeline that reshapes how researchers conceptualize questions, gather evidence, and build arguments from cartographic sources. By defining meaningful abstract units (patches and maptext) rather than attempting to precisely reproduce cartographic features, the tool creates an epistemological space for creative thinking about historical landscapes and cartographic practice. It demonstrates that automation and critical interpretation are not opposites but can work together when software design prioritizes humanities inquiry over technical reductionism.

**Keywords:** computational map studies, computer vision, digital humanities, historical maps, MapReader, spatial humanities, epistemology, open science, Ordnance Survey

**Note:** This chapter is currently available in early access. Full publication details may be updated upon official release.
