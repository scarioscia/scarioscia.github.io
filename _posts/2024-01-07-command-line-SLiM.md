---
layout: post
title: Executing SLiM from the command line  
---

Earlier this week I wrote a [post](https://scarioscia.github.io/2024-01-05/writing-SLiM-output) about outputting the results of various SLiM simulations to files. In that example, you would need to change the parameters for each simulation within your `.slim` file, and update some indication of the simulation number (e.g., `trialNumber`). 

Obviously, opening the `.slim` file and changing these variables manually is not ideal. Instead, you can define the variables on the **command line** when you run the script. To do this, we'll use the command line version of SLiM, which should be automatically installed when you install the SLiM graphical user interface (GUI); installation is outlined on the website and in the SLiM [manual](https://messerlab.org/slim/). 

Let's start by creating a SLiM script with a neutral simulation, called `scratch.slim`:

```
// set up a simple neutral simulation
initialize() {
	initializeMutationRate(1e-7);
	
	// neutral m1 mutation type
	initializeMutationType("m1", 0.5, "f", 0.0);
	
	// g1 genomic element type, use m1 for all mutations
	initializeGenomicElementType("g1", m1, 1.0);
	
	// uniform chromosome of length 100 kb with uniform recombination
	initializeGenomicElement(g1, 0, 99999);
	initializeRecombinationRate(1e-8);
}
```

We can then initialize a population in the first generation: 

```
// create a population of 500 individuals
1 early() {
	sim.addSubpop("p1", 500);
}
```

We let that population just exist for some amount of time; then at the 2000th generation we write our output to file. You can add to your simulation any other population dynamics of interest (either from my earlier week's [post](https://scarioscia.github.io/2024-01-05/writing-SLiM-output) or from a page on our course [website](https://andrew-bortvin.github.io/slimNotes/slim-guide.html)).

```
2000 late() { 
	// define a filename with filepath 
	// this is the syntax for Mac users
	fname = paste("~/slim/slimTest.txt", sep = "");
	
	// for non-Mac systems use a format like:
  // fname = paste("C:/Users/[user]/Downloads/slimTest.txt", sep=""));
	
  // write the fixed mutations to the file you just named 
	sim.outputFixedMutations(filePath = fname, append = F); 
}
```

Instead of typing in the specific filepath and name (also called "hard-coding"), let's try defining `trialNumber` externally and passing it to our script via the command line.

To run your script from the command line, open any interface (`Terminal` on Mac is installed by default; for Windows, consider [`Cygwin`](https://www.cygwin.com/)). Navigate to the folder containing your script, and then execute it by typing `slim scratch.slim`. This will run your script with parameter values as defined within the file. 

Let's update our script now to use inputs from the command line. We'll replace the `fname` definition in the `2000 late()` block with the below:  

```
2000 late() { 
	fname = paste("~/slim/slimTest", trialNumber, ".txt", sep = "");
	sim.outputFixedMutations(filePath = fname, append = F); 
}
```

As you can see, `trialNumber` is not yet defined in our simulation. Instead, we'll pass that variable via the command line by typing: `slim -d trialNumber=19 scratch.slim`. 

Let's break that statement down. 
- First, `slim` is the command used to run the SLiM program; it's the executable that starts the simulation. 
- The `-d` flag means "define" and lets SLiM know you're about to define a variable with a value; in this case we're defining the variable `trialNumber` to be 19. (You can view other possible flags, or "options", by typing `slim -h`, for help, or even just `slim`.) 
- Finally, we're writing the name of the SLiM script file we want to execute. 

You can then expand upon this format to execute multiple simulations at once and write them to file with different parameters or a different file name. In this example, we create an external shell script that will change the `trialNumber` variable for us. In a file named `runSLiM.sh` write: 

```
#!/bin/bash

for TRIAL in {1..4}
do
	slim -d trialNumber=${TRIAL} commandline.slim
done
```

To excute your script, write on the command line `bash ./runSLiM.sh`. In this script, the first line is the "shebang", which lets the file know to use the bash executable. The next line is a `for` loop structure, which walks through each of the trial numbers and passes those as a variable within your `slim` command. The `slim` line is the same, but instead of hard-coding or typing a number (e.g., 15) to be used as the `trialNumber` within the script. 

You can update your SLiM script to include other variables that are not defined in your file but are also accepted from the command line; this can include the population model, genetic events, and other simulation details. 

This article (and our entire [course](https://andrew-bortvin.github.io/slimNotes/slim-guide.html) using SLiM for population genetic models at Johns Hopkins!) was co-written with my colleague, [Andrew Bortvin](https://andrew-bortvin.github.io/). Let us know if you have any questions or suggestions! Check out the third of these three posts, on a simple visualization of the outputs [in R](https://scarioscia.github.io/2024-01-09/plot-SLiM-R).

