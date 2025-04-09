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

### Genetic associations with meiotic aneuploidy in day-5 embryos  {#project1}
<div class="research-section">
    <p> Aneuploidy (gain or loss of whole chromosomes) is the leading cause of pregnancy loss and congenital disorders. Only approximately half of all conceptions survive to live birth, primarily due to aneuploidy.</p>
    <p> We conduct a retrospective analysis of preimplantation genetic testing for aneuploidy (PGT-A) data from 139,416 in vitro fertilized embryos (22,850 sets of biological parents), identifying 3,656,198 meiotic crossovers and 92,485 aneuploid chromosomes. We find that aneuploid embryos are depleted of crossovers relative to euploid embryos.</p>
    <p> I identify a genome-wide association with a haplotype spanning several genes, including SMC1B, which encodes a meiotic cohesin protein. Transcriptome-wide association (comparing gene expression data from GTEx to our detected rates of aneuploidy) recapitulated this association, as well as two others, identifying C14orf39 (which encodes a unit of the synaptonemal complex) and NCAPD2 (which encodes a regulatory subunit of the condensin I complex).</p> 
    <p> This work is available as a <a href="https://www.medrxiv.org/content/10.1101/2025.04.02.25325097v1">preprint</a> in medRxiv, where we explore further detail about a putative regulatory mechanism by which this haplotype modulates aneuploidy. I will present this work in a talk at the <a href="https://broadinstitute.swoogo.com/mits2025/5970756">Mutations in Time in Space Conference</a> in Cambridge, MA, later this spring.</p>
    <p>{% include image.html url="../images/research_images/aneuploidy_manhattan.jpg" height="250px" align="center" %}</p>
</div>



### Estimating rates of meiotic and mitotic error in human development  {#project2}
<div class="research-section">
    <p> Estimates of euploid, aneuploid, and mosaic (embryos comprising a mix of euploid and aneuploid cells) of embryos are based on clinical classifications of PGT-A. However, PGT-A relies on a biopsy of just a few cells from the multi-cell day-5 embryo, which may not accurately reflect the true nature of each embryo.</p>
    <p> As a result, these data also do not offer adequate foundation to estimate the rates of meiotic and mitotic error actually occurring in human development. Here, we conduct simulations across a range of each error rate, simulate biopsies, and use approximate Bayesian computation to identify the set of simulated embryos that most matches clinical data. The results offer us ranges of these error rates that best explain the clinical data observed through PGT-A.</p>
    <p> This project began during the rotation of Matthew Isada, a PhD student in the Biology Department at JHU who I mentored in 2022. I then mentored undergraduate Angela Yang as she scaled up our initial 2D and 3D simulations of embryo development and biopsy to investigate a range of errors across numerous replicates.</p>
    <p> This work is available as a <a href="https://www.medrxiv.org/content/10.1101/2025.04.02.25325097v1">preprint</a> in bioRxiv, where we estimate the true incidence of mosaicism at the day-5 stage and investigate a range of misclassification parameters to offer a robust analysis of lcinical data.</p>
    <p>{% include image.html url="../images/research_images/embryo_abc.jpg" height="250px" align="center" %}</p>
</div>



### Testing for transmission distortion in a large sample of human sperm genomes  {#project3}
<div class="research-section">
    <p> Mendel’s Law of Segregation states that the offspring of a diploid, heterozygous parent will inherit either allele with equal probability. While the vast majority of loci adhere to this rule, research in model and non-model organisms has uncovered numerous exceptions whereby “selfish” alleles are disproportionately transmitted to the next generation. Evidence of such transmission distortion (TD) in humans remains equivocal in part due to statistical and sample size limitations of past studies. </p>
    <p>We leveraged single-cell sequencing data from over 40,000 individual sperm across 25 donors (<a href="https://link.springer.com/article/10.1007/s10815-021-02300-3">5 donors</a> with known infertility phenotypes and <a href="https://www.nature.com/articles/s41586-020-2347-0">20 donors</a> of presumed normal fertility). This scan enabled us to look at an earlier developmental time point than had been considered in previous studies, with the possibility of avoiding the effects of downstream selection and other biases. Our results exhibited close concordance with binomial expectations under balanced transmission, in contrast to tenuous signals of TD that were previously reported in pedigree-based studies.</p>
    <p>This work was highlighted in an <a href="https://doi.org/10.7554/eLife.76383">eLife digest</a>! See our <a href="https://doi.org/10.7554/eLife.76383">paper</a> and our <a href="https://github.com/mccoy-lab/transmission-distortion">code</a>, and check Twitter for a <a href="https://twitter.com/saracarioscia/status/1628101974318264321?ref_src=twsrc%5Etfw">summary</a> of our findings!</p>
    <p>{% include image.html url="../images/research_images/td_pipeline_schematic.jpg" height="250px" align="center" %}</p>
</div>



### rhapsodi: an R package to impute sparsely sequenced haploid genomes  {#project4}
<div class="research-section">
    <p>The recent development of high-throughput single-cell genome sequencing of human sperm (termed "Sperm-seq") offers an opportunity to study various aspects of meiosis and inheritance with improved statistical power. However, the low sequencing coverage per cell (0.01x) necessitates the development of tailored statistical methods for recovering gamete genotypes.</p>
    <p>To this end, we developed a method called rhapsodi (R haploid sperm/oocyte data imputation) that uses low-coverage single-cell DNA sequencing data from large samples of gametes to reconstruct phased donor haplotypes, impute gamete genotypes, and map meiotic recombination events. rhapsodi is introduced in our <a href="https://doi.org/10.7554/eLife.76383">paper</a> and is available as a user-friendly and thoroughly benchmarked <a href="https://github.com/mccoy-lab/rhapsodi">R package</a>.</p>
    <p>{% include image.html url="../images/research_images/rhapsodi_schematic.jpg" height="250px" align="center" %}</p>
</div>



### Science Policy  {#project5}
<div class="research-section">
    <p>From 2017 to 2019, I worked as a Science Policy Fellow at the <a href="https://www.ida.org/en/ida-ffrdcs/science-and-technology-policy-institute">Science and Technology Policy Institute</a> (STPI), a federally-funded research and development center in Washington, DC. STPI receives funding from the NSF to conduct research and projects for the White House Office of Science and Technology Policy (OSTP). STPI also contracts with government agencies across the Executive Branch to offer science and technology analysis. These projects generally included research and literature review, interviews, and some project-specific analyses, followed by briefings, conference presentations, and formal publications. Check out some details about my work <a href="https://scarioscia.github.io/2023-01-25/science-policy">here</a>.</p>
    <p>{% include image.html url="../images/research_images/SSA.png" height="250px" align="center" %}</p>
</div>
