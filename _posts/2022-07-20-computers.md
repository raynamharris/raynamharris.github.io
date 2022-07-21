---
layout: post
title: "Building computers for remote teaching"
date: 2022-07-21
excerpt: "When I was young, my dad used our living room to build custom computers for designing off-shore drilling rigs. Now I work from home building computers for scientific discovery and data analysis. How did I get here? "
image: "/images/computer-dad.png"
comments: true
---


<figure>
<center><img src="/images/computer-dad.png" style="width:80%"></center>
<figcaption><center><i>My dad, explaining something to engineers in the Computer Graphics Department, circa 1982.<p></p></i></center></figcaption>
</figure>


When I was very young, my dad would bring home computers from work, tear them apart, spread the parts over the living room, and rebuild them. He was building custom computers that were used to design off-shore drilling rigs, something that [had never been done before](https://magazines.marinelink.com/Magazines/MaritimeReporter/198004/content/marathon-computerdesigned-offshore-206883). I remember wanting to hang out with him while he worked and being told "don't touch anything", so I didn't, for a long time.  



In grade school, I learned how to play the Oregon Trail and use Microsoft Office. In college, I learned how to use special software programs to draw chemical structures and calculate statistical differences, and in grad school, I learned how to use high-performance compute clusters to process large quantities of data. I was good at _using_ computers, but I wasn't interested in customizing them or building them as my dad did. 

That all changed when schools closed for COVID-19 and teaching moved from computer labs (where software could be installed by a systems administrator) to remote classrooms (where students use personal laptops). As a postdoc, I've been teaching online workshops, and rather than asking students to install software on their own, I've been designing custom compute environments in the cloud that can be used in geographically and temporally distributed classrooms. 

In this blog post, I'll describe some of the tools I use to build custom compute environments in the cloud so that you can build them too! 

## [Conda](https://docs.conda.io/en/latest/)

[Conda](https://docs.conda.io/en/latest/) is an open-source package management system and environment management system that quickly installs, runs, and updates packages and their dependencies. Conda installation provides the foundation for building specific compute environments that can be run locally or on remote servers. 


When I need to use Conda, I usually refer to [the docs](https://conda.io/projects/conda/en/latest/index.html) or [this lesson](https://training.nih-cfde.org/en/latest/General-Tools/Introduction-to-Conda/) for instructions. You can create a conda environment at the command-line using `conda install PKGNAME`, but I prefer to use a `environment.yml` file that lists all the packages, the [conda channels](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-channels.html) where they are stored, and maybe even a specific package version. An `environment.yml` might look something like this. 


```
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

In the above example, "conda-forge", "bioconda", and "defaults" are the conda channels where software packages are stored. The "dependencies" are all the software packages that need to be installed. I always [search the Anaconda repository](https://anaconda.org/anaconda/repo) to find out what channels to list and how to write the package names. 

Fun fact: Conda was developed by the Austin-based company, [Anaconda](https://www.anaconda.com/), in 2012. I started using it in 2019 after taking an [Intro to Conda workshop](https://angus.readthedocs.io/en/2019/conda_tutorial.html). Software installation can be a very long and painful process; I wish I had started using conda or equivalent earlier. 


## [MyBinder](https://mybinder.org/)

[GitHub](https://github.com/) is the platform that I and most of my colleagues use to share code and collaboratively write software and documentation. [MyBinder](https://mybinder.org/) can turn a GitHub repository with R and shell scripts or Python notebooks into an executable environment. I first started using MyBinder to share [the code for a publication](https://github.com/raynamharris/DissociationTest) in a way that was easier to reproduce cloning and executing locally.  Since then I've seen MyBinder to teach  workshops on 
[Conda](https://training.nih-cfde.org/en/latest/General-Tools/Introduction-to-Conda/), [R](https://training.nih-cfde.org/en/latest/General-Tools/R-for-RNA-Seq/),
[Python](https://hsf-training.github.io/hsf-training-scikit-hep-webpage/index.html), 
[Snakemake](https://training.nih-cfde.org/en/latest/General-Tools/Snakemake/), and [Unix](https://training.nih-cfde.org/en/latest/General-Tools/UNIX).



<figure>
<center>
<img src="/images/computers-2.png" style="width:90%">
<figcaption><i>A Binder configured to support Python, R, and Bash.<p></p></i></figcaption>
</center>
</figure>


Click this button [![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/nih-cfde/training-rstudio-binder/data?urlpath=rstudio) to launch RStudio in a browser window. The image below shows the first few lines of `GTEx/r4rnaseq-workshop.R` script running after successfully opening the `GTEx/GTex.Rproj` project and loading the required libraries. This ensures that all attendees have the right version of all the required software and that all the relative paths in the lesson will work. 


<figure>
<center>
<img src="/images/computers-1.png" style="width:90%">
<figcaption><i>A Binder running R.<p></p></i></figcaption>
</center>
</figure>


Combining Conda with MyBinder is very powerful. Have you ever shared a script with someone and they wrote back with a "could not find ..." error message? This happens when they don't have the necessary packages or libraries installed. You can specify the packages to load at the top of a script, but I rarely remember to do this. Listing all the necessary software in the `environment.yml` file makes it easier for current and future users to know what software is necessary and install it successfully. 

If Conda installation isn't available, you can use other package managers such as [pip](https://pip.pypa.io/en/stable/) by adding the command to a `postBuild` configuration file. You can add other commands that you would like to run before the binder is launched (such as uncompressing large files or downloading files from the internet that are too large for GitHub). An `postBuild` file might look like this:

  
    #!/bin/bash
    pip install <package>
    gunzip -k <file>
  
The `postBuild` and `environment.yml`  files should be stored in a directory called `binder` (or `.binder` if you want it hidden) in a **public** [Github](https://github.com/) repository. Explore the  [Binder Examples](https://github.com/binder-examples) or the [User Guide](https://mybinder.readthedocs.io/en/latest/index.html) for more information.

Both Binder and Conda are open source and free to use. 

## [Amazon Web Services](https://aws.amazon.com/)

[Amazon Web Services](https://aws.amazon.com/) is another platform I use to build computers for teaching and research for free or low cost. Creating an [EC2](https://aws.amazon.com/ec2/) instance feels a lot like buying a computer online in that you have to check boxes to specify how much memory you need and what features you want, but instead of purchasing a physical computer as you would from Apple or IBM, you get an IP address that connects to via a web browser. My colleagues developed a [3-hour workshop](https://training.nih-cfde.org/en/latest/Cloud-Platforms/Introduction-to-AWS/) to teach people how to use AWS, and a manuscript about lessons learned is forthcoming. The thing that I like the most about AWS and MyBinder is that it allows everyone in the classroom to have similar computing capacity regardless of the operating system.

<figure>
<center>
<img src="/images/computers-aws.png" style="width:90%">
<figcaption><i>Overview of the <a href="https://training.nih-cfde.org/en/latest/Cloud-Platforms/Introduction-to-AWS/">Introduction to AWS course</a> developed by my colleagues.<p></p></i></figcaption>
</center>
</figure>


## TLDR

I used to rely on people like my dad to build computers that could execute specific tasks. Now I build them for myself and trainees. I use GitHub for collaboration, Conda for software installation, and Binder or AWS for custom cloud-based compute environments. 

What tools do you use? Thoughts and comments welcome :)

