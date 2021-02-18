# BioMol-Project-Group2
### BIOMOL

##### Nico Ares, Pol Besalú and Isaac Soul
------

The main objective of the project is to **annotate the genes** included in the contig and to **functionally characterize** the predicted proteins.

Download assigned contig from a nematode species.

```bash
# Using npm package manager we're going to install github-files-fetcher
# so we can download particular files from the github repo.
npm install -g github-files-fetcher

# Download contig sequence from repo
fetcher --url="https://github.com/dantekali/BioMol-Project/blob/main/Group11_contig_194888_195063.fa"  --out="~/Desktop/Project"
```

#### Genes annotation included in the contig

We will obtain gene predictions using the two types of methods and we will compare the results with Ensembl annotations.

- **ab-initio tools** to obtain a first prediction (select closest species in model)
  <!--Ab-initio methods: they use several elements in the genomic sequence (suchas donor and acceptor splice sites, branch site, initiation and termination codons)and codon usage to obtain a model based on a training set.-->
  
  - GeneID  prediction: https://genome.crg.cat/software/geneid/geneid.html

  > We use the "contig.fa" file to make a gene prediction using GeneID, this will result in the generation of a gff file called "GeneID.gff" containing in this case 4 predicted genes.
  
  - FGENESH: http://www.softberry.com/berry.phtml?topic=fgenesh&group=programs&subgroup=gfindYou

  > We use the "contig.fa" file to make a gene prediction using FGENESH, this will result in the generation of a txt file called "FGENESH.txt" containing the prediction.

  - GENESCAN: http://argonaute.mit.edu/GENSCAN.html

  > We use the "Group11_contig_194888_195063.fa" file to make a gene prediction using GeneID, this will result in the generation of a txt file called "GENESCAN.txt".

- **homology-based tools** with the annotations of  a closely related species (web-server/local)
  <!--Gene predictions are based on alignments from known proteins (usually) from other genomes.-->

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
