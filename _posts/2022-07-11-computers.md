---
layout: post
title: "Building computers for remote workshops"
date: 2022-07-11
excerpt: "Blah blah blah"
comments: true
---

My first memories of havings computers at home and school are from the fourth grade, circa 1992. My dad, an engineer, would bring home multiple computers from work, tear them apart, spread all the parts over the living room, and rebuild them. I was not allowed to touch anything. I could play Oregon Trail on a Mac in my school's computer class, and eventually my dad brought home a computer that the family could play games on (unless my phone need to use the home phone :D). 

When I started grad school in 2012, most students had personal laptops, but computing courses were still taught in computer labs. For four years, I organized the [Big Data in Biology Summer School](https://www3.beacon-center.org/blog/2016/03/09/3rd-annual-big-data-in-biology-summer-school/), a week-long event that offered about a dozen courses in bioinformatics and high-performance computing. I had to reserve a computer lab for each course, and when there weren't enough, I rented laptops from a local company. Instructors were particular about whether they wanted Macs or PCs, so that had to be taken into account when assigning rooms. A few weeks before the event, I had to get lists of all the required software and pass that along to a systems administrator for installation, which often required back and forth emails about versions and dependencies. During grad school, I also taught workshops for [Software Carpenty](https://software-carpentry.org/). Their philosophy was that students should bring their laptops and install all the software required so that after the workshop they could continue using those tools after class. Students who needed help with installs could come early for assistance, and they did because installing software is a non-trivial exercise. 

When schools closed for COVID-19, it meant that computing courses had to move online. As a postdoc, I've been teaching [online workshops](https://training.nih-cfde.org/en/latest/) with custom-built computers that run in the cloud. I never dreamed that I would work with computers like my fater did, and while I don't have computer parts strewn across the living room, I do build, break, and rebuild computers on remove servers from the comfort of my home. In this blog post, I'll describe some of the tools we use to create computers in the cloud for remote workshops. 

## [Conda](https://docs.conda.io/en/latest/)

[Conda](https://docs.conda.io/en/latest/) is an open source package management system and environment management system that quickly installs, runs and updates packages and their dependencies. It was developed the Austin-based company, [Anaconda](https://www.anaconda.com/), in 2012, but I didn't start using it till about 2019 when I took a [Conda workshop](https://angus.readthedocs.io/en/2019/conda_tutorial.html). If I had started eariler, I could have saved a lot of time and headache dealing with the often painful process of installing software and dependencies. 

When I need to use Conda, I usually refer to [the docs](https://conda.io/projects/conda/en/latest/index.html) or [this lesson](https://training.nih-cfde.org/en/latest/General-Tools/Introduction-to-Conda/) for instructions. You can create a conda enviornment at the command-line using `conda install PKGNAME`, but I prefer to use a `enviornment.yml` file that lists  all the packages, the [conda channels](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-channels.html) where they are stored, and maybe even a specific package version. A `enviornment.yml` might look something like this. 


```
name: myenv
channels:
    - conda-forge
    - bioconda
    - defaults
dependencies:
    - bwa
    - snakemake-minimal=5.8.1
    - samtools=1.10
    - bcftools
    - r-base=4.0.5
    - r-irkernel=1.1
    - r-ggplot2
    - r-dplyr
    - r-tidyr
```

In the above example, "myenv" is the name I am giving the enviornment. "conda-forge", "bioconda", and "defaults" are the conda channels, and the "dependencies" are the all the software tools to install. I always [search the Anaconda repository](https://anaconda.org/anaconda/repo) to find out what channels to list and how to write the package names. 

This `enviornment.yml` file specifing the conda installion provides a foundation for building compute enviornments with specific specifications locally or on remote servers. 


## MyBinder

[MyBinder](https://mybinder.org/) can turn a GitHub repository with R and shell scripts or python notebooks into an executable environment. I first started using MyBinder to share [the code for a publication](https://github.com/raynamharris/DissociationTest) in a way that was easier to reproduce cloning and executing locally. 

Now I use to create compute enviornments for workshop attendees. For instance, you can click this button [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/nih-cfde/training-rstudio-binder/data?urlpath=rstudio) to launch RStudio in a browser window. The image below shows the first few lines of `GTEx/r4rnaseq-workshop.R` script running after successuflly opening the `GTEx/GTex.Rproj` project and loading the required libraries. This ensures that all attendees have the right version of all the required software and all the relative paths in the lesson will work. 

![](https://i.imgur.com/YCWZj5T.png)


Combining Conda with MyBinder is especially powerfull. Have you ever shared a script with someone and they wrote back with "could not find ..." error message? This happens when they don't have the neccessary packages or libraries installed. You can specify the packages to load at the top of a script, but I rarely remember to do this. Listing all the necessary software in the `enviornment.yml` file makes it easier for current and future user to know what software is necessary and install it successfully. 

If conda installion isn't available, you can use other pacakge managers such as [pip](https://pip.pypa.io/en/stable/) by adding the command to a `postBuild` configuration file. You can add other commands that you would like run before the binder is launcher (such as uncompressing large files or downloading file from the internet that are too large for github). An `postBuild` file might look like this:

  
    #!/bin/bash
    pip install ribbity
    gunzip -k GTEx/data/*gz
  
The `postBuild` and `enviornment.yml`  files should be stored in a directory called `binder` (or `.binder` if you want it hidden) in a **public** [Github](https://github.com/) repsitory. Explore the  [Binder Examples](https://github.com/binder-examples) or the [User Guide](https://mybinder.readthedocs.io/en/latest/index.html) for more information.

We (DIB-lab and CFDE) have used myBinder to teach workshops workshops using R, Unix, [Snakemake](https://training.nih-cfde.org/en/latest/General-Tools/Snakemake/), and many other open source bioinformatics tools _(list and link them maybe)_. Below are screen shots of some binder in recent workshops.

![R](https://github.com/nih-cfde/training-rstudio-binder/blob/data/rstudio-console.png?raw=true)

![Shell](https://training.nih-cfde.org/en/latest/General-Tools/Snakemake/images-snakemake/snakemake_binder_terminal.png)

## Amazon Web Services (AWS) Elastic Compute (EC) 

When you need a lot of computer power,  [AWS ]() is one of the leading providers. 

![](https://i.imgur.com/nL3qxNr.png)


## JupyterHub

links from CFDE portal iHMP AWS JupyterHub demo

https://hackmd.io/rrjnYcZ3QemfuDpt82oomw?view

https://hackmd.io/dW9EoOh3T42eed9q21NLJQ?view


## GitHub +

[GitHub](https://github.com/) is how we for collaborative write code, documentation, and tutorials. 

We use [HackMD](https://hackmd.io/) for drafting documents when we want version control without branches and pull request. We use HackMD to share lesson notes with students. We like it because changes can be made on the fly and the formatting is very nice. These markdown file can also sycnced with GitHub for long-term storage or for use as a template for future workshops.

![](https://i.imgur.com/hU9nkXA.png)

![](https://i.imgur.com/XQ1LeJ0.png)

We use [GitHub Pages](https://pages.github.com/) for building formal lesson websits, typically built with [MkDocs](https://www.mkdocs.org/) templates.



The lesson content is stored in GitHub repostitores and rendered by .

_Not sure if the following should be senetences or paragraphs_

- using mkdocs for formatting
- using github actions for automation
- use gh-pages for rendering??
- read-the docs for preview
- ribbity for issues to webtext


## Challenges

Sometimes it is hard to keep track of all the copies and edits. 



