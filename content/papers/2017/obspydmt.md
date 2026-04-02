---
title: "ObspyDMT: a Python toolbox for retrieving and processing large seismological data sets"
date: 2017-10-12
author: "Kasra Hosseini, Karin Sigloch"
venue: "Solid Earth"
description: "ObspyDMT is an open-source Python toolbox that unifies query, retrieval, processing, and quality control of large seismological datasets from multiple data centers, usable either as a command-line tool without Python knowledge or as an importable module in automated workflows."
paper_url: "https://se.copernicus.org/articles/8/1047/2017/"
paper_label: "Solid Earth"
github_url: "https://github.com/kasra-hosseini/obspyDMT"
paper_categories: ["Software Engineering", "Computational Science", "Natural Sciences"]
cover:
  image: "/images/papers/obspydmt/obspydmt-teaser.png"
  hidden: true
draft: false
bibtex: |
  @Article{se-8-1047-2017,
    AUTHOR = {Hosseini, K. and Sigloch, K.},
    TITLE = {ObspyDMT: a Python toolbox for retrieving and processing large seismological data sets},
    JOURNAL = {Solid Earth},
    VOLUME = {8},
    YEAR = {2017},
    NUMBER = {5},
    PAGES = {1047--1070},
    URL = {https://se.copernicus.org/articles/8/1047/2017/},
    DOI = {10.5194/se-8-1047-2017}
  }
---

<div style="text-align: center;">
<img src="/images/papers/obspydmt/obspydmt-teaser.png" alt="ObspyDMT Data Growth" style="max-width: 100%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Seismological research increasingly depends on large datasets, but retrieving data from multiple centers with different protocols and formats can consume more time than the actual science. ObspyDMT addresses this by providing a unified Python toolbox that handles the complexities automatically. A single command can query decades of global seismic data and generate summary visualizations. Top panel: The explosive growth of available waveforms since 1990, rising from thousands to millions of seismograms. Bottom panel: Automatic global seismicity map colored by earthquake depth. The tool requires no Python knowledge when used from the command line, yet can be integrated into automated workflows for routine tasks like data archiving, instrument correction, and quality control that are essential but time-consuming.
</div>

## Abstract

We present obspyDMT, a free, open-source software toolbox for the query, retrieval, processing and management of seismological data sets, including very large, heterogeneous and/or dynamically growing ones. ObspyDMT simplifies and speeds up user interaction with data centers, in more versatile ways than existing tools. The user is shielded from the complexities of interacting with different data centers and data exchange protocols and is provided with powerful diagnostic and plotting tools to check the retrieved data and metadata. While primarily a productivity tool for research seismologists and observatories, easy-to-use syntax and plotting functionality also make obspyDMT an effective teaching aid. Written in the Python programming language, it can be used as a stand-alone command-line tool (requiring no knowledge of Python) or can be integrated as a module with other Python codes. It facilitates data archiving, preprocessing, instrument correction and quality control – routine but nontrivial tasks that can consume much user time. We describe obspyDMT's functionality, design and technical implementation, accompanied by an overview of its use cases. As an example of a typical problem encountered in seismogram preprocessing, we show how to check for inconsistencies in response files of two example stations. We also demonstrate the fully automated request, remote computation and retrieval of synthetic seismograms from the Synthetics Engine (Syngine) web service of the Data Management Center (DMC) at the Incorporated Research Institutions for Seismology (IRIS).

**Keywords:** seismological data, Python, data management, open-source software, ObspyDMT, waveform processing
