---
title: "Instaseis: instant global seismograms based on a broadband waveform database"
date: 2015-06-16
author: "M. van Driel, L. Krischer, S. C. Stähler, Kasra Hosseini, T. Nissen-Meyer"
venue: "Solid Earth"
paper_url: "https://doi.org/10.5194/se-6-701-2015"
paper_label: "Solid Earth"
github_url: "https://github.com/krischer/instaseis"
paper_categories: ["Computational Science", "Software Engineering", "Natural Sciences"]
draft: false
bibtex: |
  @Article{se-6-701-2015,
    AUTHOR = {van Driel, M. and Krischer, L. and St\"ahler, S. C. and Hosseini, K. and Nissen-Meyer, T.},
    TITLE = {Instaseis: instant global seismograms based on a broadband waveform database},
    JOURNAL = {Solid Earth},
    VOLUME = {6},
    YEAR = {2015},
    NUMBER = {2},
    PAGES = {701--717},
    URL = {https://se.copernicus.org/articles/6/701/2015/},
    DOI = {10.5194/se-6-701-2015}
  }
---

<div style="text-align: center;">
<img src="/images/papers/instaseis/instaseis-teaser.png" alt="Instaseis Computational Cost" style="max-width: 70%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Need a seismogram for any earthquake at any station on Earth? Traditionally, each calculation requires running a wave propagation simulation. Instaseis changes this by precomputing and storing Green's functions in a database that enables extraction of arbitrary seismograms in milliseconds. The efficiency is remarkable: generating a complete Instaseis database costs approximately half the computational time of computing seismograms for just a single source using traditional methods. This figure shows CPU hours required to generate full databases with 1-hour seismograms for Earth and Mars using two time integration schemes. By storing basis coefficients of Lagrange polynomials rather than raw waveforms, Instaseis achieves 4th order spatial accuracy while exactly honoring velocity discontinuities like the core-mantle boundary. This transforms workflows that previously required supercomputer access into laptop-scale computations.
</div>

## Abstract

We present a new method and implementation (Instaseis) to store global Green's functions in a database which allows for near-instantaneous (on the order of milliseconds) extraction of arbitrary seismograms. Using the axisymmetric spectral element method (AxiSEM), the generation of these databases, based on reciprocity of the Green's functions, is very efficient and is approximately half as expensive as a single AxiSEM forward run. Thus, this enables the computation of full databases at half the cost of the computation of seismograms for a single source in the previous scheme and allows to compute databases at the highest frequencies globally observed. By storing the basis coefficients of the numerical scheme (Lagrange polynomials), the Green's functions are 4th order accurate in space and the spatial discretization respects discontinuities in the velocity model exactly. High-order temporal interpolation using Lanczos resampling allows to retrieve seismograms at any sampling rate. AxiSEM is easily adaptable to arbitrary spherically symmetric models of Earth as well as other planets. In this paper, we present the basic rationale and details of the method as well as benchmarks and illustrate a variety of applications.

**Keywords:** seismograms, Green's functions, waveform database, spectral element method, computational seismology, AxiSEM
