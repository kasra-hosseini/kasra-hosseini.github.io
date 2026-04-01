---
title: "ObsPyLoad: A Tool for Fully Automated Retrieval of Seismological Waveform Data"
date: 2013-05-01
author: "Chris Scheingraber, Kasra Hosseini, Robert Barsch, Karin Sigloch"
venue: "Seismological Research Letters"
paper_url: "https://doi.org/10.1785/0220120103"
paper_label: "Seismological Research Letters"
github_url: "https://github.com/scheingraber/obspyload"
paper_categories: ["Software Engineering", "Computational Science", "Natural Sciences"]
draft: false
bibtex: |
  @article{10.1785/0220120103,
    author = {Scheingraber, Chris and Hosseini, Kasra and Barsch, Robert and Sigloch, Karin},
    title = {ObsPyLoad: A Tool for Fully Automated Retrieval of Seismological Waveform Data},
    journal = {Seismological Research Letters},
    volume = {84},
    number = {3},
    pages = {525-531},
    year = {2013},
    month = {05},
    issn = {0895-0695},
    doi = {10.1785/0220120103},
    url = {https://doi.org/10.1785/0220120103},
    eprint = {https://pubs.geoscienceworld.org/ssa/srl/article-pdf/84/3/525/2766030/525.pdf}
  }
---

<div style="text-align: center;">
<img src="/images/papers/obspyload/obspyload-teaser.png" alt="ObsPyLoad Program Flow" style="max-width: 50%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
As seismological data centers exploded with new waveform data in the 2010s, researchers faced a growing challenge: downloading, homogenizing, and quality-controlling data from multiple centers with different interfaces and formats consumed more time than the actual science. ObsPyLoad addresses this data avalanche with a fully automated solution that queries metadata holdings and retrieves seismograms from multiple data centers simultaneously - a major advantage over individual center tools. This schematic shows the elegant program flow: from initial configuration through metadata queries to IRIS and ORFEUS data centers, followed by parallel waveform downloads with built-in quality control and retry logic. A simple command-line call without parameters downloads event-based data for all earthquakes from the past 30 days, while extensive options allow customization for geographic regions, time windows, magnitude thresholds, and update modes. By handling the tedious infrastructure work automatically, ObsPyLoad lets seismologists focus on scientific questions rather than data wrangling.
</div>

## Abstract

We confront the data avalanche: the amount of waveform data available from seismological data centers has been growing enormously over the past few years. This is a highly welcome development from a scientific point of view, but the time and effort spent on identification, retrieval, and quality control of subsets of these data may quickly exceed tolerable limits for an individual researcher.

Data from different data centers may not be available through the same interfaces or may arrive in different formats, which have tended to change over time. This often results in time-consuming homogenization efforts. The situation has improved in that certain quasistandards have been adopted for data formats; for example SEED, the Standard for the Exchange of Earthquake Data (IRIS Consortium, 1993), has been in use for nearly 20 years. Also, a few Internet data exchange protocols are in wide use now; for example ArcLink, developed within the WebDC project of GFZ and SZGRF, or DHI, a framework for accessing data and metadata from the IRIS DMC using a DHI-supporting client program.

ObsPyLoad is a software tool that fully automatically queries the metadata holdings of seismological data centers and retrieves all metadata and seismograms of matching attributes via the Internet. Waveforms and metadata can be retrieved from multiple data centers, which is a major advantage over the download tools offered by individual data centers. We currently support downloads from IRIS (via a webservice client), and from ORFEUS (via ArcLink). We expect to add more choices as they become available in the ObsPy framework. Earthquake metadata may currently be retrieved from the Seismic Portal.

ObsPyLoad can either run in standalone mode from a command-line interface, or be integrated into other Python code as a module. A simple call to obspyload.py without any parameters will download event-based data for all events that occurred over the past 30 days. Command-line options allow for extensive customization (e.g., selection of certain geographical source or receiver regions; time windowing; thresholding by event magnitude; retrieval of metadata only; attempts to update an existing data set or reattempt a download for which problems occurred earlier).

**Keywords:** seismological data, waveform retrieval, data center integration, ObsPy, Python, automation
