# BioMol-Project-Group2

##### Nico Ares, Pol Besalú, Isaac Soul
------
Download assigned contig from a nematode species.

```bash
# Using npm package manager we're going to install github-files-fetcher
# so we can download particular files from the github repo.
npm install -g github-files-fetcher

# Download contig sequence from repo
fetcher --url="https://github.com/dantekali/BioMol-Project/blob/main/Group11_contig_194888_195063.fa"  --out="~/Desktop/Project"
```

### PROJECT
D. PROJECT DESCRIPTION
The main objective of the project is to annotate the genes included in the contig and to functionally characterize the predicted proteins. For instance, you could use ab-initio tools to obtain a first prediction, identify the putative proteins using blast search and use homology-based tools with the annotations of a closely related species. Later you could functionally characterize your proteins. You should discuss the performance of the different methods. Can you guess to which species does your contig correspond?


###  GitHub report: 
upload to GitHub an .md file describing the pipeline used and the and/or scripts used if any. Each person of each group will upload the report (including the description of the pipeline, a brief discussion of the main results and the scripts -used if any- in a pdf file) to the campus virtual. Name the file as GROUPx_Surname_Name.pdf


  ## AB-INITIO METHODS:
  
  ### GeneID  prediction: (https://genome.crg.cat/software/geneid/geneid.html)

  > For the gene prediction, we used our fasta file ("Group2_contig_15842_16020.fa") and ran it in GeneID. From this we obtained the generation of our gff file ("GeneID.gff") in which we can see that we have a three gene structure with a score of 42.59 and a length of 10560 bps:
  > 
  > 1st gene: 5 exons / 256 aa / score = 7.24.
  > 
  > 2nd gene: 1 exon / 43 aa / score = 2.00
  > 
  > 3rd gene: 5 exons / 428 aa / score = 33.34
  
  ### FGENESH: (http://www.softberry.com/berry.phtml?topic=fgenesh&group=programs&subgroup=gfindYou)

  >From our fasta file ("Group2_contig_15842_16020.fa") using FGENESH, we obtain that we have:
  >
  >Length of sequence 10375
  >
  > 4 genes in +/-chain 2 from the C_elegans genomic DNA.
  > 
  > 16 exons in +chain 6 / -chain 10.
  > 
  > Gene 1: 3 exon (s),   1526  -   2189,   486 bp, 161 aa, chain +
  > 
  > Gene 2: 4 exon (s),   2396  -   3529,   987 bp, 328 aa, chain -
  > 
  > Gene 3: 6 exon (s),   3957  -   6437,  2247 bp, 748 aa, chain -
  > 
  > Gene 4: 6 exon (s),   3957  -   6437,  2247 bp, 155 aa, chain -
  > 
  > The positions of predicted genes and exons have a score = 285.580908 
  > 

### GENESCAN: (http://argonaute.mit.edu/GENSCAN.html)

  > We run our fasta file ("Group2_contig_15842_16020.fa") through GENESCAN and obtained the following results:
  > 
  > 10566 bp : 36.78% C+G : Isochore 1 ( 0 - 43 C+G%)
  > 
  > NO EXONS FOUND AT GIVEN PROBABILITY CUTOFF
  > 
  > Our GENSCAN predicted 1 peptide of 251 aa. 


## HOMOLOGY BASED METHODS:

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

