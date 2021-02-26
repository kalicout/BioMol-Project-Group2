# BioMol-Project-Group2

#### Nico Ares, Pol Besalú, Isaac Soul
------

## PROJECT

D. PROJECT DESCRIPTION
The main objective of the project is to annotate the genes included in the contig and to functionally characterize the predicted proteins. For instance, you could use ab-initio tools to obtain a first prediction, identify the putative proteins using blast search and use homology-based tools with the annotations of a closely related species. Later you could functionally characterize your proteins. You should discuss the performance of the different methods. Can you guess to which species does your contig correspond?

------

### Download of our data:

Download assigned contig from an unknown species.

```bash
# Using npm package manager we're going to install github-files-fetcher
# so we can download particular files from the github repo.
npm install -g github-files-fetcher

# If we don't have npm package manager and retry:
sudo apt install npm

# Download contig sequence from repo
fetcher --url= "https://github.com/kalicout/BioMol-Project-Group2/blob/main/Group2_contig_15842_16020.fa"
```

### First aproach to our data

Since our data is a DNA sequence, it is unreadable at first sight so we don't know even what organism does it belong to. To get a first insight at it we can use blast, specifficly blastx(From DNA to AA). Entering the link https://blast.ncbi.nlm.nih.gov/Blast.cgi we click on blastx and select our fasta(.fa) contig.

The blastx beat maches where 
* hypothetical protein CAEBREN_31550 [Caenorhabditis brenneri]
* Uncharacterized protein CELE_R13H4.7 [Caenorhabditis elegans]

knowing this we can start using AB-initio tools specifficly for this two species(they are both small wormlike nematodes).


![CAENORHABDITIS.B](https://github.com/kalicout/BioMol-Project-Group2/blob/main/CB.jpg)C.BRENNERI

------

![CAENORHABDITIS.E](https://github.com/kalicout/BioMol-Project-Group2/blob/main/CE.jpg)C.ELEGANS


### AB-Initio methods:
  
We will use ab initio methods to make predictions about where the exons are in our contig and which proteins are encoded. For this we used three methods GeneID prediction, Softberry and Genescan. For the gene prediction, we used our fasta file ("Group2_contig_15842_16020.fa") and ran it in the said predictors, this were the results.
  
  #### GeneID  prediction: (https://genome.crg.cat/software/geneid/geneid.html)
   We use our contig and select as an option C.elegans:
   * The results are in: https://github.com/kalicout/BioMol-Project-Group2/blob/main/GeneID.gff
   > 3 genes. Score = 42.59 
   > 
   > Gene 1 (Reverse). 5 exons. 256 aa. Score = 7.24 
   > 
   > Gene 2 (Reverse). 1 exons. 43 aa. Score = 2.00 
   > 
   > Gene 3 (Forward). 5 exons. 428 aa. Score = 33.34 
  #### FGENESH: (http://www.softberry.com/berry.phtml?topic=fgenesh&group=programs&subgroup=gfindYou)
   We use our contig and select as an option both C.elegans and C.brenneri:
   * The results for C.elegans are in: https://github.com/kalicout/BioMol-Project-Group2/blob/main/SBCELEGANS.txt
   > Number of predicted genes 4: in +chain 2, in -chain 2.
   > 
   > Number of predicted exons 16: in +chain 6, in -chain 10.
   * The results for C.brenneri are in: https://github.com/kalicout/BioMol-Project-Group2/blob/main/SBCBRENNERI.txt
   > Number of predicted genes 2: in +chain 1, in -chain 1.
   > 
   > Number of predicted exons 8: in +chain 6, in -chain 2.

  #### GENESCAN: (http://argonaute.mit.edu/GENSCAN.html)
   We use our contig and select vertebrae(there isn't an alternate option) as the organism:
   * The results are in: https://github.com/kalicout/BioMol-Project-Group2/blob/main/GENESCAN.txt
   > NO EXONS FOUND AT GIVEN PROBABILITY CUTOF

## HOMOLOGY BASED METHODS:

  > We run our sequence through blast against the nr database

  - Run *blastx* of the unspliced sequence against the *nr database*. Since this is a huge database, this time we will run blast on the ncbi server https://blast.ncbi.nlm.nih.gov/Blast.cgi

  - blast search to identify the putative proteins (more sensitive if protein level instead of nucleotides)
  - https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastp&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome
  
  > Then we transform the translated nucleotide sequence "Group11_contig_194888_195063.fa" to protein with blastx, in this step we generate a "tioblast.fa" containing the putative proteins.

- **sequences comparison** Compare them with the annotated protein in Ensembl database

  - To obtain the multiple alignment we will use the simple MSA option from T-coffee software (http://tcoffee.crg.cat/). Which software do you think is more accurate?

#### Functionally characterization of the predicted proteins

Functional annotation is an essential step in omics data analysis. It is defined as the process of attaching biological information to gene and protein sequences (such as those predicted using Ab-initio and Homology-based methods) and describing functional groups based on similarities.

- Search the Gene Ontology database (http://geneontology.org) for the genesFOXF2, Ds001, Ds002, and Ds003 (gene names).
- Identify conserved protein domains in the peptides encoded by genes (you can find the FASTA file with the sequences -query.fas- here. This python script allows the programmatic access to InterPro Web Services using an API (Application Programming Interface). Use the script  iproscan5.py
- Use   the   script  find_enrichment.py (from the  goatools python library;https://github.com/tanghaibao/goatools) to find if the genes under study (e.g. genes expressed in a specific tissue)  are enriched in a particular (or various) GO terms. For a GO enrichment analysis you need to know i) the names or database identifiers of the genes you want to test(in this example the InterPro domains found for all genes specifically expressed in the tissue analyzed in our study -study.names), ii) the names or database identifiers of the genes in the population(e.g. the InterPro domains of all genes expressed in this organism - i.e., in all tissues - population.names), iii) an association file that maps a gene or database identifier name to a GO category (InterPro2GO.map) and iv) a file with a basic version of the GO database (go-basic.obo)



#### Discuss the performance of the different methods. Can you guess to which species does your contig correspond?

