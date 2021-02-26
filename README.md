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
  
  #### Results:
  
  If we take a closer look to the data, we can see that the prediction for C.Elegans in geneID gives the same exons than the Softberry scan for C.Brenneri.
  GENEID
  > Contig	geneid_v1.2	Internal	7810	7999	 6.60	+	0	Contig_3
  > 
  > Contig	geneid_v1.2	Internal	8056	8293	 4.54	+	2	Contig_3
  > 
  > Contig	geneid_v1.2	Internal	8396	8495	-0.44	+	1	Contig_3
  > 
  > Contig	geneid_v1.2	Terminal	10008	10343	 9.71	+	0	Contig_3
  > 
------
  SOFTBERRY
  > 2 +    2 CDSi      7810 -      7999    6.59      7810 -      7998    189
  > 
  > 2 +    3 CDSi      8056 -      8293   15.23      8058 -      8291    234
  > 
  > 2 +    4 CDSi      8396 -      8495    3.45      8397 -      8495     99
  > 
  > 2 +    5 CDSi     10008 -     10312   20.37     10008 -     10310    303
  > 
  
  Softberry has predicted a protein for this exons, and since two predictors have the same result, it is our more reliable data to continue exploring.So we create        
  a file and copy paste the protein.
  ```bash
  # change directory to path
  cd <path>
  # Create the file in fasta format
  nano Celegansprotein.fa
  # We just have to copy the DNAseq for the exon from the SBCBRENNERI.txt file and paste in the terminal, then we use ctrl+X to save the file.
  ```
  We should get a file like https://github.com/kalicout/BioMol-Project-Group2/blob/main/Celegansprotein.fa

### Homology Based Methods:

Now we will use homology based methods using the Celegansprotein.fa to see if we obtein a good match we can run exonerate with.

We start by running *blastx* which converts from nucleotides to protein of the unspliced sequence against the *nr database*. Since this is a huge database, this time we will run blast on the ncbi server https://blast.ncbi.nlm.nih.gov/Blast.cgi

As a result we get a lot of matches, we can try to make an alignment with all of them but we will chose the most acurate one (The first in the list), we can download it from the web page directly in fasta format as seqdump.fa.

Now we can run exonerate to make an alignment.

### Exonerate pipeline:
Exonerate is a generic tool for pairwise sequence comparison. We will make, in this case, an alignment between our contig and the best ranked protein mach sequence.

```bash
#Now we will select the lines corresponding to exons and we will store them in a .gff file:

exonerate     -m     p2g     --showtargetgff     -q     seqdump.fa     -t Group2_contig_15842_16020.fa -S F| egrep -w exon > Celegans.gff

#We will use bedtools to transform our .gff file into a .fa file:

bedtools getfasta -fi Group2_contig_15842_16020.fa -bed Celegans.gff > Celegans.fa

#Now we use basic bash to concatenate exons and make our file a single line:

sed     -e    '2,$s/>.*//'    Celegans.fa     |grep     -v    '^$'     > Celegans.fa
```

### Functionally characterizating the predicted proteins:

Functional annotation is an essential step in omics data analysis. It is defined as the process of attaching biological information to gene and protein sequences (such as those predicted using Ab-initio and Homology-based methods) and describing functional groups based on similarities, basically it is trying to describe what kind of protein it is and what its function is.

For this we used interproscan (https://www.ebi.ac.uk/interpro/search/sequence/) and we runned the protein stored in seqdump.fa this tool will tell what kind of protein do we have, its binding sites, active sites etc...4





### Discussion of the methods and results:

