---
layout: post
title: Research
---

### Testing for transmission distortion in a large sample of human sperm genomes  {#project1}
Mendel’s Law of Segregation states that the offspring of a diploid, heterozygous parent will inherit either allele with equal probability. While the vast majority of loci adhere to this rule, research in model and non-model organisms has uncovered numerous exceptions whereby “selfish” alleles are disproportionately transmitted to the next generation. Evidence of such transmission distortion (TD) in humans remains equivocal in part due to statistical and sample size limitations of past studies.

We leveraged [recently published](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7351608/) single-cell sequencing data from over 40,000 individual sperm across 25 donors. This scan enabled us to look at an earlier developmental time point than had been considered in previous studies, with the possibility of avoiding the effects of downstream selection and other biases. Our results exhibited close concordance with binomial expectations under balanced transmission, in contrast to tenuous signals of TD that were previously reported in pedigree-based studies. 

See our paper **[here](https://doi.org/10.7554/eLife.76383)** and our code **[here](https://github.com/mccoy-lab/transmission-distortion)**, and check Twitter for a [summary](https://twitter.com/saracarioscia/status/1628101974318264321?ref_src=twsrc%5Etfw) of our findings! 

{% include image.html url="../images/research_images/td_pipeline_schematic.jpg" height="250px" align="center" %}


<br>
<br>

### rhapsodi: an R package to impute sparsely sequenced haploid genomes 
The recent development of high-throughput single-cell genome sequencing of human sperm (termed "Sperm-seq") offers an opportunity to study various aspects of meiosis and inheritance with improved statistical power. However, the low sequencing coverage per cell (0.01x) necessitates the development of tailored statistical methods for recovering gamete genotypes.

To this end, we developed a method called rhapsodi (R haploid sperm/oocyte data imputation) that uses low-coverage single-cell DNA sequencing data from large samples of gametes to reconstruct phased donor haplotypes, impute gamete genotypes, and map meiotic recombination events. rhapsodi is introduced in our [paper](https://doi.org/10.7554/eLife.76383); it is available as a user-friendly and thoroughly benchmarked package **[here](https://github.com/mccoy-lab/rhapsodi)**. 

{% include image.html url="../images/research_images/rhapsodi_schematic.jpg" height="250px" align="center" %}


<br>
<br>

### Science Policy

From 2017 to 2019, I worked as a Science Policy Fellow at the **[Science and Technology Policy Institute](https://www.ida.org/en/ida-ffrdcs/science-and-technology-policy-institute)** (STPI), a federally-funded research and development center in Washington, DC. STPI receives funding from the NSF to conduct research and projects for the White House Office of Science and Technology Policy (OSTP). STPI also contracts with government agencies across the Executive Branch to offer science and technology analysis. These projects generally included research and literature review, interviews, and some project-specific analyses, followed by briefings, conference presentations, and formal publications. Check out some details about my work **[here](https://scarioscia.github.io/2023-01-25/science-policy)**.

<br />