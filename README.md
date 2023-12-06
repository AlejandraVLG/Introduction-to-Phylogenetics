# Introduction-to-Phylogenetics
Introduction to Phylogenetics

# Timetable
Subject to change depending on the pace of the class

09:30 - 09:40 Welcome 
09:40 - 10:30 Introduction to molecular phylogenetics
10:30 - 11:15 Multiple sequence alignment
11:15 - 11:30 Break
11:30 - 12:30 Phylogenetic tree inference

12:30 - 13:30 Lunch Break

13:30 - 14:00	Practical: Phylogenetic inference with IQ-TREE
14:00 - 15:00 Assessing tree confidence with the bootstrap
		          Practical: Assessing tree confidence in IQ-TREE
15:00 - 15:15 Break
15:15 - 16:30 Introduction to bayesian phylogenetic inference
		          Practical: Bayesian inference with BEAST
16:30 - 17:30 Phylogenetic applications
		          Practical: Viral Phylogeography with BEAST


## Software setup: 
We will provide access to a computer already setup for the workshop.
(Optional) If you want to follow the workshop on your own computer:
Install: FigTree; MAFFT; IQ-TREE v2; BEAGLE; BEAST v1, Tracer
Data used in the course: data

## Prerequisites:
- Unix command line
- Familiarity with basic concepts in evolution (descent from a common ancestor, sequence divergence due to mutations, natural selection).
- Basic experience of examining DNA/protein sequence data.


## Setting up

Create and activate the conda enviroment
- conda create -n phylogenetics python==3.8 pandas numpy
- conda activate phylogenetics
- conda install -c bioconda figtree
- conda install -c bioconda mafft
- conda install -c bioconda iqtree
- conda install -c bioconda tracer
- conda install -c bioconda beast

Activate the conda enviroment

```
conda activate phylogenetics
```

## Practical 1

Plot and manipulate this tree:

1) Represent this tree in Newick format
2) Save the Newick string in a text file,
3) Open a terminal/promt:
```
figtree
``` 
4) Open it (File->Open) in FigTree
5) Does the tree look the same? Try to make it look similar


## Practical 2

1) Run MAFFT of the primate-mtDNA_unaligned.fasta file in /home/Course_Material/data/primates/ :

```
mafft primate-mtDNA_unaligned.fasta  > primate-mtDNA_mafft-aligned.fasta
```

Compare aligned and unaligned files: what’s the difference?

Run MAFFT also on the SARS-CoV-2 sequences. Why does it take longer?

## Practical 3

1) Run IQ-TREE 2 with GTR substitution model on the alignments we previously generated:

```
iqtree2  -s primate-mtDNA_mafft-aligned.fasta -m GTR --prefix primate-mtDNA_mafft-aligned_iqtreeGTR
```

2) Find best model with IQ-TREE 2 (ModelFinder). Does it include rate variation? Does the tree differ from before (you can use FigTree for visualization)?

```
iqtree2  -s primate-mtDNA_mafft-aligned.fasta --prefix primate-mtDNA_mafft-aligned_iqtreeModelFinder
```


## Practical 4

1) Measure branch support in IQ-TREE 2. First run bootstrap and TBE jointly:

```
iqtree2  -s primate-mtDNA_mafft-aligned.fasta -m GTR -b 100 --tbe --prefix primate-mtDNA_mafft-aligned_iqtreeGTR_tbe
```

2) Then try the Ultra Fast Bootstrap:

```
iqtree2  -s primate-mtDNA_mafft-aligned.fasta -m GTR -B 1000 --prefix primate-mtDNA_mafft-aligned_iqtreeGTR_ufboot
```

3) Finally the aLRT-SH:
```
iqtree2  -s primate-mtDNA_mafft-aligned.fasta -m GTR --alrt 1000 --prefix primate-mtDNA_mafft-aligned_iqtreeGTR_alrt
```

## Practical 5

Run BEAST:

1) Create an input xml file using BEAUti
- Open BEAUti
```
beauti
```
- File->”Import data”; or, drag alignment file onto BEAUti window
- Sites-> select GTR substitution model 
- Trees-> Yule process 
- Click “Generate BEAST File”

2) Run the xml file in BEAST
- Give BEAST the .xml file created by BEAUti; or from the command line: 
```
beast primate-mtDNA_mafft-aligned_BEAST.xml
```
3) Analyse the output in Tracer
- File->”Import trace file”-> pick .log file created by BEAST ; or just drag it on Tracer the window

4) Compare to maximum likelihood branch support
- Small ESS values (<100, in red) mean that the MCMC needs to run longer
- BEAUti->MCMC->Length of chain

5) Process the output in TreeAnnotator
```
treeannotator
```
- Pick as input the .trees file created by BEAST. Choose output name

6) Visualize tree in Fig tree
- Pick as input the tree file created by TreeAnnotator 

7) Compare to maximum likelihood branch support

## Practical 6

Run phylogeography analysis in BEAST

1) Create an input xml file using BEAUti like before from the SARS-CoV-2 genomes:
- Open BEAUti
```
beauti
```
- File->”Import data”;   or, drag alignment file onto BEAUti window

2) Within BEAUti, also include time and geographic information regarding the input sequences:
- Tips->”Import dates”; select given “_dates.txt” file
- Traits->”Import traits”; select given “_locations.txt” file
- Traits->”Create partition from trait”
- Click “Generate BEAST File”


## Resources:
- An example for phylogenomics from whole-genome resequencing: https://www.science.org/doi/epdf/10.1126/science.1258524
- How to estimate the number of MCMC iterations required for convergence: https://journal.r-project.org/articles/RN-2006-002/RN-2006-002.pdf and associated R package CODA - specifically the Raftery-Lewis diagnostic.

