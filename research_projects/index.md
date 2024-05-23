---
layout: post
title: Research
---

<style>
    /* Style to add space between sections */
    .research-section {
        margin-bottom: 80px; /* Adjust the margin as needed */
    }
</style>

### Quantifying whole-chromosome aneuploidies in human IVF embryos  {#project1}
<div class="research-section">
    <p> Aneuploidy (gain or loss of whole chromosomes) is the leading cause of pregnancy loss and congenital disorders. Only approximately half of all conceptions survive to live birth, primarily due to aneuploidy.</p>
    <p> Using DNA data from a large sample of IVF embryos at day-5 of development, I identified gains and losses of each autosome, as well as the sex chromosomes. We find that each of these error modes is tolerated up to the day 5 stage, and that losses are less frequent on larger, gene-rich chromosomes. Taken together, these findings suggest that the fitness cost of aneuploidy might be due to the cumulative effects of disregulation across numerous genes on a chromosome.</p>
    <p> I shared these initial results in a <a href="https://x.com/saracarioscia/status/1788305960022180242">talk</a> at the Biology of Genomes conference in May 2024, and we continue using this dataset to further investigate aneuploidies in early human development.</p>
    <p>{% include image.html url="../images/research_images/embryo_arrest.jpg" height="250px" align="center" %}</p>
</div>



### Testing for transmission distortion in a large sample of human sperm genomes  {#project2}
<div class="research-section">
    <p> Mendel’s Law of Segregation states that the offspring of a diploid, heterozygous parent will inherit either allele with equal probability. While the vast majority of loci adhere to this rule, research in model and non-model organisms has uncovered numerous exceptions whereby “selfish” alleles are disproportionately transmitted to the next generation. Evidence of such transmission distortion (TD) in humans remains equivocal in part due to statistical and sample size limitations of past studies. </p>
    <p>We leveraged single-cell sequencing data from over 40,000 individual sperm across 25 donors (<a href="https://link.springer.com/article/10.1007/s10815-021-02300-3">5 donors</a> with known infertility phenotypes and <a href="https://www.nature.com/articles/s41586-020-2347-0">20 donors</a> of presumed normal fertility). This scan enabled us to look at an earlier developmental time point than had been considered in previous studies, with the possibility of avoiding the effects of downstream selection and other biases. Our results exhibited close concordance with binomial expectations under balanced transmission, in contrast to tenuous signals of TD that were previously reported in pedigree-based studies.</p>
    <p>This work was highlighted in an <a href="https://doi.org/10.7554/eLife.76383">eLife digest</a>! See our <a href="https://doi.org/10.7554/eLife.76383">paper</a> and our <a href="https://github.com/mccoy-lab/transmission-distortion">code</a>, and check Twitter for a <a href="https://twitter.com/saracarioscia/status/1628101974318264321?ref_src=twsrc%5Etfw">summary</a> of our findings!</p>
    <p>{% include image.html url="../images/research_images/td_pipeline_schematic.jpg" height="250px" align="center" %}</p>
</div>



### rhapsodi: an R package to impute sparsely sequenced haploid genomes  {#project3}
<div class="research-section">
    <p>The recent development of high-throughput single-cell genome sequencing of human sperm (termed "Sperm-seq") offers an opportunity to study various aspects of meiosis and inheritance with improved statistical power. However, the low sequencing coverage per cell (0.01x) necessitates the development of tailored statistical methods for recovering gamete genotypes.</p>
    <p>To this end, we developed a method called rhapsodi (R haploid sperm/oocyte data imputation) that uses low-coverage single-cell DNA sequencing data from large samples of gametes to reconstruct phased donor haplotypes, impute gamete genotypes, and map meiotic recombination events. rhapsodi is introduced in our <a href="https://doi.org/10.7554/eLife.76383">paper</a> and is available as a user-friendly and thoroughly benchmarked <a href="https://github.com/mccoy-lab/rhapsodi">R package</a>.</p>
    <p>{% include image.html url="../images/research_images/rhapsodi_schematic.jpg" height="250px" align="center" %}</p>
</div>



### Science Policy  {#project4}
<div class="research-section">
    <p>From 2017 to 2019, I worked as a Science Policy Fellow at the <a href="https://www.ida.org/en/ida-ffrdcs/science-and-technology-policy-institute">Science and Technology Policy Institute</a> (STPI), a federally-funded research and development center in Washington, DC. STPI receives funding from the NSF to conduct research and projects for the White House Office of Science and Technology Policy (OSTP). STPI also contracts with government agencies across the Executive Branch to offer science and technology analysis. These projects generally included research and literature review, interviews, and some project-specific analyses, followed by briefings, conference presentations, and formal publications. Check out some details about my work <a href="https://scarioscia.github.io/2023-01-25/science-policy">here</a>.</p>
</div>
