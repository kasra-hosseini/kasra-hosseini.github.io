---
title: "Global multiple-frequency seismic tomography using teleseismic and core-diffracted body waves"
date: 2016-05-30
author: "Kasra Hosseini"
venue: "PhD Thesis, Ludwig-Maximilians-Universität München"
description: "PhD thesis developing a global multiple-frequency seismic tomography framework that combines the largest collected dataset of Pdiff traveltime measurements with teleseismic P and PP data to image whole-mantle structure, revealing sharp slab geometries, mantle plumes, and subdivisions of Large Low Shear Velocity Provinces."
paper_url: "https://nbn-resolving.de/urn:nbn:de:bvb:19-195973"
paper_label: "PhD Thesis"
paper_categories: ["Natural Sciences", "Computational Science"]
cover:
  image: "/images/papers/phd-thesis/phd-thesis-teaser.png"
  hidden: true
draft: false
bibtex: |
  @phdthesis{ediss19597,
    month = {May},
    year = {2016},
    title = {Global multiple-frequency seismic tomography using teleseismic and core-diffracted body waves},
    author = {Kasra Hosseini},
    publisher = {Ludwig-Maximilians-Universit\"{a}t M\"{u}nchen},
    url = {http://nbn-resolving.de/urn:nbn:de:bvb:19-195973},
    keywords = {Seismic tomography, Inverse theory, Time-series analysis, Numerical solutions, Large seismological data sets, Wave scattering and diffraction, Body waves, Pdiff, Wave propagation, Seismic waveforms, Multiple-frequency seismic tomography, Finite-frequency traveltimes, Imaging, Lowermost mantle, Subduction, Mantle plumes, LLSVP, Python, ObsPy}
  }
---

<div style="text-align: center;">
<img src="/images/papers/phd-thesis/phd-thesis-teaser.png" alt="PhD Thesis Global Tomography" style="max-width: 100%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
The lowermost third of Earth's mantle remains poorly understood because the seismic waves that sample it best, core-diffracted phases (Pdiff), are difficult to model and severely attenuated. This dissertation tackles the challenge by assembling one of the largest body wave datasets for global tomography: 479,559 Pdiff traveltimes combined with millions of teleseismic P and PP measurements, processed automatically from approximately 2000 earthquakes across the broadband frequency range (up to 1 Hz). The resulting tomographic model reveals whole-mantle structures with unprecedented clarity. Vertical cross-sections beneath North America, Afar, Aegean, and Iceland show both downwellings (subducting Farallon, Tethyan, and Aegean slabs) and upwellings (mantle plumes beneath Iceland, Afar, and Tristan da Cunha). An adaptive inversion framework with locally-adjusted regularization captures sharp structural boundaries while avoiding artifacts, revealing subdivisions within the Large Low Shear Velocity Provinces and providing tomographic evidence for continuous plume conduits connecting the core-mantle boundary to surface hotspots.
</div>

## Abstract

Seismic tomography is the pre-eminent tool for imaging the Earth's interior. Since the advent of this method in the 1980's, the internal structure of Earth has been vastly sampled and imaged at a variety of scales, and the resulting models have served as the primary means to investigate the processes driving our planet. Significant recent advances in seismic data acquisition and computing power have drastically progressed tomographic methods. Broad-band seismic waveforms can now be simulated up to the highest naturally occurring frequencies and consequently, measurement techniques can exploit seismic waves in their entire usable spectrum and in multiple frequencies.

This dissertation revolves around aspects of global multiple-frequency seismic tomography, from retrieving and processing of large seismological data sets to explore the multi-scale structure of the earth. The centrepiece of this work is an efficient processing strategy to assemble the largest possible data sets for waveform-based tomographic inversions. Motivated by the complex but loosely-constrained structure of the lowermost mantle, we aim to increase the spatial resolution and coverage of the mantle in all depths by extracting a maximum of information from observed seismograms.

We first present a method that routinely measures finite-frequency traveltimes of Pdiff waves by cross-correlating observed waveforms with synthetic seismograms across the broad-band frequency range. Large volumes of waveform data of ~2000 earthquakes are retrieved and pre-processed using fully automatic software built for this purpose. Synthetic seismograms for these earthquakes are calculated by semi-analytical wave propagation through a spherically symmetric earth model, to 1 Hz dominant frequency. This way, we construct one of the largest core-diffracted P wave traveltime collections so far with a total of 479,559 traveltimes in frequency passbands ranging from 30.0 to 2.7 s dominant period. Projected onto their core-grazing ray segments, the Pdiff observations recover major structural, lower-mantle heterogeneities known from existing global mantle models.

An inversion framework with adaptive parameterisation and locally-adjusted regularisation is developed to accurately map the information of this data set onto the desired model parameters. This broad-band waveform inversion seamlessly incorporates the Pdiff measurements alongside a very large data set of conventional teleseismic P and PP measurements. We obtain structural heterogeneities of considerable detail in all mantle depths. The mapped features confirm several previously imaged structures. At the same time, sharper outlines for several subduction systems (e.g., Tethyan, Aegean and Farallon slabs) and uprising mantle plumes (e.g., Iceland, Afar and Tristan da Cunha) appear in our model. We trace some of these features throughout the mantle to investigate their morphological characteristics in a large (whole-mantle) context. Moreover, we report the structural findings revealed by our model. This ranges from geometries of slab complexes and subdivisions of Large Low Shear Velocity Provinces at the root of the mantle to tomographic evidence to support the existence of deep-mantle plumes beneath Iceland and Tristan da Cunha.

**Keywords:** seismic tomography, inverse theory, time-series analysis, large seismological data sets, wave propagation, Pdiff, multiple-frequency tomography, lowermost mantle, subduction, mantle plumes, LLSVP
