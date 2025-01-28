---
layout: post
title: Using a `config.yaml` file in Snakemake pipelines 
---

Snakemake is a workflow management system used in bioinformatics to automate data processing pipelines. Using configuration files offers a flexible way to manage parameters, paths, and variables across your workflow, making it easier to adjust your pipeline without modifying the main workflow script directly. The dreaded message from your advisor, "Hey, have we looked at <12 other things you didn't look at>?" is a lot less scary when your pipeline is built to consider a number of parameters and samples, and all you need to do is add another entry to your `config`.

When working with Snakemake, there are times when you’ll need to reuse the same parameters or file paths across multiple rules. Instead of hardcoding these values directly in the workflow file, using a config.yaml allows you to update input variables without changing your Snakemake workflow, share reproducible workflows that allow collaborators to investigate their own datasets and parameters, and ensure consistency between pipelines for the same project. 

Here I’ll provide a basic introduction to using a `config.yaml` file in Snakemake. We'll look at an example from a genomics project, where we need to configure several variables for a GWAS pipeline. I'll show you how to structure the configuration file and load it in your Snakemake workflow. For more information, check out Snakemake's [introduction to config files](https://snakemake.readthedocs.io/en/stable/snakefiles/configuration.html). I'll also link back here once my current projects that actually use this structure are made public :).

## Example GWAS workflow using a config file 
Here's an example set up for a configuration file that has examples about your static files, input files, and the parameters used for your analysis: 
```
# config.yaml
input_data:
  genotype: "/path/to/genotype_data.bed"
  phenotype: "/path/to/phenotype_data.tsv"

output:
  results_dir: "/path/to/results"
  logs_dir: "/path/to/logs"

parameters:
  snp_subset_size: 100
  significance_threshold: 0.05
  threads: 8

samples:
  - sample1
  - sample2
  - sample3
  - sample4

summary_stats:
  - output: "/path/to/summary_stats_meanCO.csv"
    type: "recombination"
    N: 500
  - output: "/path/to/summary_stats_locationCO.csv"
    type: "recombination"
    N: 600
  - output: "/path/to/summary_stats_aneuploidy.csv"
    type: "aneuploidy"
    N: 450
```
Here we have the input data (genotype and phenotype) which were presumably computed or downloaded via a separate step; defined output directories; input parameters, which can be changed; sample names, which can be expanded; and specific information for each analysis, to which different keys or entries can be added. 

We then have our `snakemake` pipeline that uses the information from the config:
```
# Snakefile

configfile: "config.yaml"

rule process_genotype:
	"""Process genotype input data."""
    input:
        genotype=config["input_data"]["genotype"]
    output:
        "processed_genotype.bed"
    shell:
        "process_genotype {input} > {output}"

rule process_phenotype:
	"""Process phenotype input data."""
    input:
        phenotype=config["input_data"]["phenotype"]
    output:
        "processed_phenotype.tsv"
    shell:
        "process_phenotype {input} > {output}"

rule perform_gwas:
	"""Perform GWAS analysis."""
    input:
        genotype="processed_genotype.bed",
        phenotype="processed_phenotype.tsv"
    output:
        "gwas_results.tsv"
    params:
        snp_subset_size=config["parameters"]["snp_subset_size"],
        significance_threshold=config["parameters"]["significance_threshold"],
        threads=config["parameters"]["threads"],
        samples=config["samples"]  
    shell:
    	"""
    	gwas_analysis --genotype {input.genotype} --phenotype {input.phenotype} \
    	--snp_subset_size {params.snp_subset_size} \
    	--threshold {params.significance_threshold} \
    	--threads {params.threads} \
    	--samples {','.join(params.samples)} > {output}
    	"""

rule analyze_summary_stats:
	"""Analyze summary statistics."""
    input:
        summary_stats_1=config["summary_stats"][0]["output"],
        summary_stats_2=config["summary_stats"][1]["output"],
        summary_stats_3=config["summary_stats"][2]["output"]
    params:
        type1=config["summary_stats"][0]["type"],
        type2=config["summary_stats"][1]["type"],
        type3=config["summary_stats"][2]["type"],
        N1=config["summary_stats"][0]["N"],
        N2=config["summary_stats"][1]["N"],
        N3=config["summary_stats"][2]["N"]
    output:
        "summary_stats_analysis_results.csv"
    shell:
    	"""
    	analyze_stats --type1 {params.type1} --type2 {params.type2} --type3 {params.type3} \
    	--N1 {params.N1} --N2 {params.N2} --N3 {params.N3} \
    	--input1 {input.summary_stats_1} --input2 {input.summary_stats_2} --input3 {input.summary_stats_3} \
    	> {output}
    	"""
```

## Accessing values from the config file
In the Snakemake rules above, you can see that we access values from the config.yaml file using the config object. For example:

`config["input_data"]["genotype"]` provides the file path to the genotype data.
`config["parameters"]["snp_subset_size"]` gives the SNP subset size for the GWAS analysis.
`config["parameters"]["threads"]` specifies the number of threads to use for parallel processing.
For the summary stats analysis, we access the file paths, types, and sample sizes (N) for each summary stat using `config["summary_stats"][0]["output"]`, `config["summary_stats"][0]["type"]`, `config["summary_stats"][0]["N"]`, and so on.

## Using and troubleshooting the config file 
Dry-running your snakemake pipeline (`-n`) will automatically check for values from your config file and flag any missing files or parameters, syntax errors, disagreements between the config and the snakemake file, etc. Like much of the feedback from snakemake (even when using `--verbose`!), the comments can be vague or cryptic. If you get any issues with your config file specifically, you can paste it into a lintr like [this one](https://www.yamllint.com/), which will alert you to any typographical problems. 

Good luck, fellow snakes! 
