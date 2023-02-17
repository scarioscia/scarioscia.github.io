---
layout: post
title: Using `latexdiff` to track changes in LaTeX via Overleaf 
---

For my first grad school [manuscript](https://elifesciences.org/articles/76383), we used LaTeX via [Overleaf](https://www.overleaf.com). There was a bit of a learning curve, but it proved useful (with Overleaf premium) for version control, tracked changes, and figure integration. After revisions (and revisions, and revisions...) we were ready to resubmit. In addition to the class response to reviewers letter, I thought it would be useful to include a tracked changes version of the document (much like is available via Microsoft Word or Google Docs). Luckily, a package exists specifically for LaTeX.

1. Download `latexdiff`. For Mac, run `brew install latexdiff` on the command line. For non-Mac, follow the detailed [instructions](https://www.overleaf.com/learn/latex/Articles/Using_Latexdiff_For_Marking_Changes_To_Tex_Documents) from Overleaf.
2. Download the `.tex` files from the projects you'd like to compare (for me, that was the version I'd submitted previously and the version I was about to submit).
3. On the command line, run `latexdiff oldfile.tex newfile.tex > diff.tex` (include filepaths and updated filenames as necessary). You can customize this command (and therefore your output) using options outlined [here](https://www.overleaf.com/learn/latex/Articles/Using_Latexdiff_For_Marking_Changes_To_Tex_Documents). 

I think it’s easiest to view this as a new project back in overleaf. First, compress your newly created `diff.tex` to a .zip file (from the command line, `zip -r diff.tex.zip diff.tex`). From here, you have two options: 
1. Duplicate your existing project and replace your main `.tex` file with your new one (e.g., replace `main.tex` with `diff.tex`). This is the better choice if you have numerous dependencies, you want your bibliography or figures to populate, etc.
2. Create a new project on Overleaf and upload your .zip file (the pdf might not compile - that’s ok!). Duplicate the files necessary for your old project into your new project (for example, a `.bib` file if you had a bibliography; mine required a `bmcart-modified.cls` file). This is the better choice if you just need the in-line text differences and know that everything else (figures, etc.) remained the same or are highlighted elsewhere. 

Once you compile the pdf within Overleaf, the pdf of your diff.tex will display. By default, new text is shown in blue and removed text is shown in red; but this will differ based on other options used in your initial `latexdiff` command. 

<figure class="figure">
	<img src="../images/blog_images/latex_diff_TD_abstract.png" alt="">
	<figcaption class="figcaption">New text is shown in blue, while deleted text is shown in red. We hoped this was useful for the reviewers so they could easily see where we'd made in-text changes as they read along in our response to reviewers.</figcaption>
</figure><br>