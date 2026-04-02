---
title: "Ten simple rules for working with other people's code"
date: 2023-01-01
author: "Charlie Pilgrim, Paul Kent, Kasra Hosseini, Ed Chalstrey"
venue: "PLOS Computational Biology"
description: "This paper presents ten pragmatic rules for researchers who need to use, modify, or build upon existing research software, covering four phases — planning, understanding, making changes, and publishing — while acknowledging the practical constraints of academic research environments."
paper_url: "https://doi.org/10.1371/journal.pcbi.1011031"
paper_label: "PLOS Comp Biol"
paper_categories: ["Software Engineering"]
cover:
  image: "/images/papers/ten-simple-rules/ten-simple-rules-teaser.png"
  hidden: true
draft: false
bibtex: |
  @article{pilgrim2023ten,
    title={Ten simple rules for working with other people's code},
    author={Pilgrim, Charlie and Kent, Paul and Hosseini, Kasra and Chalstrey, Ed},
    journal={PLOS Computational Biology},
    volume={19},
    number={4},
    pages={e1011031},
    year={2023},
    publisher={Public Library of Science San Francisco, CA USA},
    doi={10.1371/journal.pcbi.1011031}
  }
---

<div style="text-align: center;">
<img src="/images/papers/ten-simple-rules/ten-simple-rules-teaser.png" alt="Ten Simple Rules Framework" style="max-width: 50%; display: block; margin: 0 auto;">
</div>

<div style="text-align: center; font-size: 0.9em; color: #666; margin: 0.5em 0 1.5em 0;">
Working with other people's code is a common challenge in research, yet receives less attention than best practices for writing code. This paper presents ten pragmatic rules for researchers at all levels who need to use, modify, or build upon existing research software. The rules are organized into four interconnected phases: planning your approach, understanding the codebase, making changes safely, and publishing your work. Unlike industry software development practices that may introduce unrealistic time burdens, these guidelines acknowledge research realities such as time pressures, niche tools built without formal software engineering practices, and subtle bugs that can be difficult to detect. The framework is iterative rather than linear, recognizing that as understanding of a codebase grows, goals and strategies may need to be reassessed.
</div>

## Abstract

Every time that you use a computer, you are using someone else's code, whether that be an operating system, a word processor, a web application, research tools, or simply code snippets. Almost all code has some bugs and errors. In day to day life, these bugs are usually not too important or at least obvious when they do happen (think of an operating system crashing). However, in research, there is a perfect storm that makes working with other people's code particularly challenging - research needs to be correct and accurate, researchers often use niche and non-commercial tools that are not built with best software practices, bugs can be subtle and hard to detect, and researchers have time pressures to get things done quickly. It is no surprise then that working with other people's code is a common frustration for researchers and is even considered a rite of passage.

There are a wealth of resources addressing how to write better code in academia, including PLOS ONE "Ten Simple Rules" articles on reproducibility, documentation, and writing open-source software. And there are calls for improving research reproducibility by publishing code and ensuring it is reusable. However, the inverse problem - working with other people's code in research - does not receive as much attention. In industry, there are some resources for working with legacy code ("legacy code" essentially translates to "other people's code"). While industry software development practices are useful, they often cannot be applied blindly to research software development and instead need to be adapted. Therefore, our approach is to bring together and integrate lessons from industry, existing literature, and the research experiences of the authors and their colleagues.

The rules in this article will be useful for academic researchers at all levels, from students to professors. Our focus is on pragmatic research efficiency, as opposed to absolute best practices that may introduce unrealistic time burdens. The rules are informed but opinionated, and as such, we encourage readers to use the rules as a starting point to think for themselves and do what works for them. Overall, if the reader can take one useful idea or thought from this article, then we will consider it successful.

**Keywords:** software engineering, research methods, legacy code, code reuse, reproducibility
