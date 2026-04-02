---
title: "Living Machines: A study of atypical animacy"
date: 2020-12-01
author: "Mariona Coll Ardanuy, Federico Nanni, Kaspar Beelen, Kasra Hosseini, Ruth Ahnert, Jon Lawrence, Katherine McDonough, Giorgia Tolfo, Daniel CS Wilson, Barbara McGillivray"
venue: "Proceedings of the 28th International Conference on Computational Linguistics (COLING)"
description: "This paper introduces the first dataset for atypical animacy detection and a fully unsupervised BERT-based pipeline for identifying when typically inanimate objects—specifically machines in 19th-century English text—are described with animate attributes. The method substantially outperforms baselines and provides evidence of how industrialisation reshaped linguistic representations of machines as active agents."
paper_url: "https://aclanthology.org/2020.coling-main.400/"
paper_label: "COLING"
paper_categories: ["AI", "NLP", "(L)LM"]
cover:
  image: "/images/papers/living-machines/living-machines-teaser.png"
  hidden: true
draft: false
bibtex: |
  @inproceedings{coll-ardanuy-etal-2020-living,
    title = "Living Machines: A study of atypical animacy",
    author = "Coll Ardanuy, Mariona  and
      Nanni, Federico  and
      Beelen, Kaspar  and
      Hosseini, Kasra  and
      Ahnert, Ruth  and
      Lawrence, Jon  and
      McDonough, Katherine  and
      Tolfo, Giorgia  and
      Wilson, Daniel CS  and
      McGillivray, Barbara",
    editor = "Scott, Donia  and
      Bel, Nuria  and
      Zong, Chengqing",
    booktitle = "Proceedings of the 28th International Conference on Computational Linguistics",
    month = dec,
    year = "2020",
    address = "Barcelona, Spain (Online)",
    publisher = "International Committee on Computational Linguistics",
    url = "https://aclanthology.org/2020.coling-main.400/",
    doi = "10.18653/v1/2020.coling-main.400",
    pages = "4534--4545"
  }
---

<div style="text-align: center;">
<img src="/images/papers/living-machines/living-machines-teaser.png" alt="Living Machines Atypical Animacy" style="max-width: 100%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
During the Industrial Revolution, something subtle yet profound happened to English: machines began to "live" in the language. Writers increasingly attributed animate qualities to inanimate objects, describing machines that could work, fail, or behave. This paper tackles the challenge of detecting such atypical animacy using BERT contextualized embeddings trained on nineteenth-century English books. The table demonstrates a striking pattern: when different time-period language models predict tokens for "They were told that the [MASK] stopped working," more recent models increasingly suggest machine-related words (engine, machinery) rather than human-related ones. This fully unsupervised approach captures the gradual linguistic shift as machines became conceptualized as active agents rather than passive tools, providing fine-grained evidence of how technological change reshapes the way we speak.
</div>

## Abstract

This paper proposes a new approach to animacy detection, the task of determining whether an entity is represented as animate in a text. In particular, this work is focused on atypical animacy and examines the scenario in which typically inanimate objects, specifically machines, are given animate attributes. To address it, we have created the first dataset for atypical animacy detection, based on nineteenth-century sentences in English, with machines represented as either animate or inanimate. Our method builds on recent innovations in language modeling, specifically BERT contextualized word embeddings, to better capture fine-grained contextual properties of words. We present a fully unsupervised pipeline, which can be easily adapted to different contexts, and report its performance on an established animacy dataset and our newly introduced resource. We show that our method provides a substantially more accurate characterization of atypical animacy, especially when applied to highly complex forms of language use.

**Keywords:** animacy detection, atypical animacy, BERT, historical text, nineteenth-century English, digital humanities, language models
