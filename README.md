# Genome2GenomeDistance

Implementation of methods described in Meier-Kolthoff et al 2013. [Article Here](http://www.biomedcentral.com/1471-2105/14/60)

While the authors of that publication rightly believe that the BLAST algorithm gives more sensitivity for these questions , it comes at an extreme cost making application to large datasets computationally intractible. 

These programs provide a minimal implementation of their described methods so that the nucmer program could instead be employed using greedy trimming along with the equation __d<sub>4</sub>__ to calculate a distance between two genome assemblies.

This repo contains a few scripts for executing this program as a standalone or in parallel computing enviroments. 


## G2GCalc

The core program. Currently runs mummer and retrieves MUMs , filters overlapping MUMs and keeping the longest alignment between any two overlapping MUMs, then calculates distance as:

2 * (Total Identical Nucleotides) / (Length of MUMs from Reference) + (Length of MUMs from Query) 


__Usage__:

```bash
  ./G2GCalc <options>
```

__Options__:

--manifest : instead of providing files on a a command line provide a file that contains a list of the files you wish to process. One file per line.

--threads : multi threading is supported. 

--files : files you wish to analyze. Supports wild-cards. (i.e. *.fa)

--out : file to print resulting distance matrix to. Defaults to /dev/stdout

You can mix and match files + --manifest. 

  
## Split_Manifests

Given a large number of files splits the work into many manifests so that analysis can be spread across many computers. 

__Usage__ : 

```bash
./Split__Manifests --files *.fa --chunk 40000
```
__Options__: 

--files : files you wish to be broken up in to sub manifests

--chunk : how big of a chunk you want each manifest to take up. (A chunk of 100 will consist of a sub matrix that is 10 x 10 files,unless it is on the diagonal then it will be 1/2 normal size)


##Array_Submit.slrm
SLURM job file for array jobs. You'll want to know the number of the last manifest in a split to use this.

__Usage__:
```bash
sbatch --array=1-last__manifest Array__Submit.slrm
```

__Options__:
	Anything you can edit in sbatch you can edit here. 


  

