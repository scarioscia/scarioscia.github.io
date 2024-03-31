---
layout: post
title: rhapsodi
---

### rhapsodi: an R package to impute sparsely sequenced haploid genomes 
The recent development of high-throughput single-cell genome sequencing of human sperm (termed "Sperm-seq") offers an 
opportunity to study various aspects of meiosis and inheritance with improved statistical power. However, the low 
sequencing coverage per cell (0.01x) necessitates the development of tailored statistical methods for recovering gamete 
genotypes.

To this end, we developed a method called rhapsodi (R haploid sperm/oocyte data imputation) that uses low-coverage 
single-cell DNA sequencing data from large samples of gametes to reconstruct phased donor haplotypes, impute gamete 
genotypes, and map meiotic recombination events. rhapsodi is introduced in our 
[paper](https://doi.org/10.7554/eLife.76383); it is available as a user-friendly and thoroughly benchmarked package 
**[here](https://github.com/mccoy-lab/rhapsodi)**. 

{% include image.html url="../images/research_images/rhapsodi_schematic.jpg" height="250px" align="center" %}
