---
title: "AxiSEM3D: broad-band seismic wavefields in 3-D global earth models with undulating discontinuities"
date: 2019-02-18
author: "Kuangdai Leng, Tarje Nissen-Meyer, Martin van Driel, Kasra Hosseini, David Al-Attar"
venue: "Geophysical Journal International"
paper_url: "https://doi.org/10.1093/gji/ggz092"
paper_label: "GJI"
paper_categories: ["Computational Science", "Software Engineering", "Natural Sciences"]
draft: false
bibtex: |
  @article{10.1093/gji/ggz092,
    author = {Leng, Kuangdai and Nissen-Meyer, Tarje and van Driel, Martin and Hosseini, Kasra and Al-Attar, David},
    title = {AxiSEM3D: broad-band seismic wavefields in 3-D global earth models with undulating discontinuities},
    journal = {Geophysical Journal International},
    volume = {217},
    number = {3},
    pages = {2125-2146},
    year = {2019},
    month = {02},
    abstract = {We present a novel numerical method to simulate global seismic wave propagation in realistic aspherical 3-D earth models across the observable frequency band of global seismic data. Our method, named AxiSEM3D, is a hybrid of spectral element method (SEM) and pseudospectral method. It describes the azimuthal dimension of global wavefields with a substantially reduced number of degrees of freedom via a global Fourier series parametrization, of which the number of terms can be locally adapted to the inherent azimuthal complexity of the wavefields. AxiSEM3D allows for material heterogeneities, such as velocity, density, anisotropy and attenuation, as well as for finite undulations on radial discontinuities, both solid–solid and solid–fluid, and thereby a variety of aspherical Earth features such as ellipticity, surface topography, variable crustal thickness, undulating transition zone and core–mantle boundary topography. Undulating discontinuities are honoured by means of the 'particle relabelling transformation', so that the spectral element mesh can be kept spherical. The implementation of the particle relabelling transformation is verified by benchmark solutions against a discretized 3-D SEM, considering ellipticity, topography and bathymetry (with the ocean approximated as a hydrodynamic load) and a tomographic mantle model with an undulating transition zone. For the state-of-the-art global tomographic models with aspherical geometry but without a 3-D crust, efficiency comparisons suggest that AxiSEM3D can be two to three orders of magnitude faster than a discretized 3-D method for a seismic period at 5 s or below, with the speed-up increasing with frequency and decreasing with model complexity. We also verify AxiSEM3D for localized small-scale heterogeneities with strong perturbation strength. With reasonable computing resources, we have achieved a corner frequency of up to 1 Hz for 3-D mantle models.},
    issn = {0956-540X},
    doi = {10.1093/gji/ggz092},
    url = {https://doi.org/10.1093/gji/ggz092},
    eprint = {https://academic.oup.com/gji/article-pdf/217/3/2125/28272400/ggz092.pdf}
  }
---

<div style="text-align: center;">
<img src="/images/papers/axisem3d/axisem3d-teaser.png" alt="AxiSEM3D Computational Efficiency" style="max-width: 100%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Earth is not a perfect onion: its internal boundaries undulate, with topography on the core-mantle boundary reaching 10 km and transition zone thickness varying by hundreds of kilometers. Simulating seismic wave propagation through such realistic 3-D complexity at high frequencies pushes traditional methods to their limits. AxiSEM3D solves this through a hybrid approach combining spectral element and pseudospectral methods, parametrizing the azimuthal dimension with a locally adaptive Fourier series that adjusts resolution to match structural complexity. The efficiency gains are dramatic: two to three orders of magnitude faster than full 3-D methods (SPECFEM) at periods of 5 seconds or below, with speedup increasing at higher frequencies. Using particle relabelling transformation to honor undulating discontinuities while keeping the mesh spherical, AxiSEM3D enables 1 Hz simulations of 3-D mantle models with moderate computational resources, making previously inaccessible frequency ranges practical for routine use.
</div>

## Abstract

We present a novel numerical method to simulate global seismic wave propagation in realistic aspherical 3-D earth models across the observable frequency band of global seismic data. Our method, named AxiSEM3D, is a hybrid of spectral element method (SEM) and pseudospectral method. It describes the azimuthal dimension of global wavefields with a substantially reduced number of degrees of freedom via a global Fourier series parametrization, of which the number of terms can be locally adapted to the inherent azimuthal complexity of the wavefields. AxiSEM3D allows for material heterogeneities, such as velocity, density, anisotropy and attenuation, as well as for finite undulations on radial discontinuities, both solid–solid and solid–fluid, and thereby a variety of aspherical Earth features such as ellipticity, surface topography, variable crustal thickness, undulating transition zone and core–mantle boundary topography. Undulating discontinuities are honoured by means of the 'particle relabelling transformation', so that the spectral element mesh can be kept spherical. The implementation of the particle relabelling transformation is verified by benchmark solutions against a discretized 3-D SEM, considering ellipticity, topography and bathymetry (with the ocean approximated as a hydrodynamic load) and a tomographic mantle model with an undulating transition zone. For the state-of-the-art global tomographic models with aspherical geometry but without a 3-D crust, efficiency comparisons suggest that AxiSEM3D can be two to three orders of magnitude faster than a discretized 3-D method for a seismic period at 5 s or below, with the speed-up increasing with frequency and decreasing with model complexity. We also verify AxiSEM3D for localized small-scale heterogeneities with strong perturbation strength. With reasonable computing resources, we have achieved a corner frequency of up to 1 Hz for 3-D mantle models.

**Keywords:** seismic wavefields, spectral element method, 3-D Earth models, undulating discontinuities, computational seismology, AxiSEM3D
