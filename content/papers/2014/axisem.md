---
title: "AxiSEM: broadband 3-D seismic wavefields in axisymmetric media"
date: 2014-06-04
author: "T. Nissen-Meyer, M. van Driel, S. C. Stähler, Kasra Hosseini, S. Hempel, L. Auer, A. Colombi, A. Fournier"
venue: "Solid Earth"
description: "AxiSEM is an open-source spectral-element method that computes broadband 3-D global seismic wavefields at a fraction of conventional cost by exploiting axisymmetry to reduce the computational domain from 3-D to 2-D while retaining full 3-D wavefield accuracy."
paper_url: "https://doi.org/10.5194/se-5-425-2014"
paper_label: "Solid Earth"
github_url: "https://github.com/geodynamics/axisem"
paper_categories: ["Computational Science", "Software Engineering", "Natural Sciences"]
cover:
  image: "/images/papers/axisem/axisem-teaser.png"
  hidden: true
draft: false
bibtex: |
  @Article{se-5-425-2014,
    AUTHOR = {Nissen-Meyer, T. and van Driel, M. and St\"ahler, S. C. and Hosseini, K. and Hempel, S. and Auer, L. and Colombi, A. and Fournier, A.},
    TITLE = {AxiSEM: broadband 3-D seismic wavefields in axisymmetric media},
    JOURNAL = {Solid Earth},
    VOLUME = {5},
    YEAR = {2014},
    NUMBER = {1},
    PAGES = {425--445},
    URL = {https://se.copernicus.org/articles/5/425/2014/},
    DOI = {10.5194/se-5-425-2014}
  }
---

<div style="text-align: center;">
<img src="/images/papers/axisem/axisem-teaser.png" alt="AxiSEM Teaser" style="max-width: 100%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Computing how seismic waves propagate through Earth's full 3-D structure at high frequencies typically requires supercomputers running for days or weeks. AxiSEM achieves the same accuracy in a fraction of the time by exploiting a key simplification: for spherically symmetric Earth models, the azimuthal dimension can be computed analytically rather than numerically. This reduces the computational domain from 3-D to 2-D while maintaining full 3-D accuracy in the output wavefields. The figure shows a 3-D wavefield simulation from an earthquake in Italy, computed with AxiSEM at frequencies across the observable seismic band. This efficiency breakthrough enables applications previously impractical, from computing millions of synthetic seismograms for tomographic inversions to generating databases for near-instantaneous retrieval of seismograms from any source-receiver combination.
</div>

<div style="text-align: center; margin: 1.5em 0;">
<iframe width="560" height="315" src="https://www.youtube.com/embed/u6-mJ6TCi6o?si=5PVZjqP0qZRnoftM&start=10" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Seismic wave propagation in a spherically symmetric Earth model computed using <a href="https://github.com/geodynamics/axisem" target="_blank" class="red-link">AxiSEM</a>. Warm colors (red/yellow) show <a href="https://en.wikipedia.org/wiki/P-wave" target="_blank" class="red-link">P-waves</a>, while cold colors (blue/green) show <a href="https://en.wikipedia.org/wiki/S-wave" target="_blank" class="red-link">S-waves</a>.
</div>

## Abstract

We present a methodology to compute 3-D global seismic wavefields for realistic earthquake sources in visco-elastic anisotropic media, covering applications across the observable seismic frequency band with moderate computational resources. This is accommodated by mandating axisymmetric background models that allow for a multipole expansion such that only a 2-D computational domain is needed, whereas the azimuthal third dimension is computed analytically on the fly. This dimensional collapse opens doors for storing space–time wavefields on disk that can be used to compute Fréchet sensitivity kernels for waveform tomography. We use the corresponding publicly available AxiSEM (www.axisem.info) open-source spectral-element code, demonstrate its excellent scalability on supercomputers, a diverse range of applications ranging from normal modes to small-scale lowermost mantle structures, tomographic models, and comparison with observed data, and discuss further avenues to pursue with this methodology.

**Keywords:** seismic wavefields, spectral-element method, axisymmetric media, computational seismology, waveform tomography
