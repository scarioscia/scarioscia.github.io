---
layout: post
title: Writing outputs from SLiM simulations to file
---

SLiM is an evolutionary simulation software that allows investigation of a variety of population genetic simulations. Last Fall, my colleague [Andrew Bortvin](https://andrew-bortvin.github.io/) and I designed and taught a course for undergraduates (Population Genetics Simulation and Visualization) at Johns Hopkins. We used SLiM to create these simulations and R to visualize the outputs. 

Mac users can download the SLiM GUI on their [website](https://messerlab.org/slim/). Windows users can refer to pages 64-66 of the SLiM [manual](https://messerlab.org/slim/) for installation guidance. 

In creating our course materials, we leveraged the SLiM [manual](https://messerlab.org/slim/) and added substantial detail regarding features useful for our class on our course [website](https://andrew-bortvin.github.io/slimNotes/slim-guide.html). One of those topics involved running a simulation over multiple combinations of parameters and generating a file for each output. Here is a summary of that pipeline.



In writing each simulation to file, each filename will include the trial number, or some other unique identifier of what the output corresponds to. This trial number is a constant defined in the `initialize()` statement (along with any other parameters necessary for our simulation): 

```
initialize() {
  // Define the variable trialNumber, which we'll use in writing our output file 
  defineConstant('trialNumber', 1);

  // Define other parameters necessary for the simulation 
  initializeMutationRate(1e-7);
  // Create mutation type
  initializeMutationType("m1", 0.5, "f", 0.0);
  initializeMutationType("m2", 1, "f", 0.2);
  m1.convertToSubstitution = F;
  m2.convertToSubstitution = F;
    
  // Create Types of DNA 
  initializeGenomicElementType("g1", m1, 1);   
    
  // Arrange DNA into a genome
  initializeGenomicElement(g1, 0, 3000);
    
  initializeRecombinationRate(1e-4);
  initializeSex("A");
} 
```

We then start our simulation at the first generation, as usual: 

```
1 early() {
  sim.addSubpop("p1", 5000);
}
```

We can then use this variable `trialNumber` to create a name for our file. We define this name and create the file at the first generation we'd like to start recording output. We also create a header for our file, which will therefore show us the column names. This header will be the same across each run of the simulation (i.e., so we can compare outputs across different input parameters) but of course may differ across simulations, depending on the information you are recording. 

In this example, we create the header, define the filename, and initiate the file with the `writeFile()` function at the end of the 300th generation:

```
300 late() {
  // Steps necessary for the simulation: 

  // Initialize generation counter
  sim.setValue("generationCount", 0);
  // Introduce sweep mutation at 5% allele frequency 
  p1_size = size(p1.genomes);
  initial_freq = asInteger(p1_size * 0.05);
  target = sample(p1.genomes, initial_freq);
  target.addNewDrawnMutation(m2, 1000);   
  
  // Steps specifically for writing simulation output to file: 

  // Initialize generation counter
  line = "Generation, Allele.Frequency, Trial";
    
  // Get a file name
  // This is standard syntax for Mac
  defineConstant("fname", paste("~/slim/AF_trial", trialNumber, ".csv", sep =""));
  // for non-Mac systems use a format like:
  // defineConstant("fname", paste("C:/Users/[user]/Downloads/AF_trial", trialNumber, ".csv", sep=""));
    
  // Write to file
  writeFile(fname, line, append=F); 
}
```

If you were to view your file after just 300 generations, it would have one line: the header you defined. You can then add your simulation information from each subsequent generation: 

```
300:1000 late() {
	// Find out number of generations
  gen = sim.getValue("generationCount");
    
  // Count number of m2 mutations in the population
  m2_count = sum(p1.genomes.countOfMutationsOfType(m2));
    
  // Create a line with the information in your header: 
  // generation number, number of m2 mutations, and trial number 
  line = paste(gen, m2_count, trialNumber, sep = ",");

  // Write this line to file each generation 
  writeFile(fname, line, append=T);

  // Update generation counter
  sim.setValue("generationCount", gen + 1);
}
```

So at the end of each generation (300, 301, etc. all the way to the end of the 1000th generation) the values of those three variables are written into your file that you defined and initiated above. 

We then end our simulation with the command:

```
1000 late() { 
  sim.simulationFinished();
}
```

You can view the results of your simulation by checking out that file, or conduct further investigation via plotting (e.g., in R or Python!). Check out an extensive example of creating and writing these output files on our course [website](https://andrew-bortvin.github.io/slimNotes/writing-output.html), and please reach out with any questions or suggestions! 