# Introduction-to-Phylogenetics
Introduction to Phylogenetics

## Setting up

Create and activate the conda enviroment
- conda create -n phylogenetics python==3.8 pandas numpy
- conda activate phylogenetics
- conda install -c bioconda figtree
- conda install -c bioconda mafft
- conda install -c bioconda iqtree
- conda install -c bioconda tracer
- pip install beast

Activate the conda enviroment

```
conda activate phylogenetics
```

## Practical 1

Let’s plot and manipulate this tree:

imagen 

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
