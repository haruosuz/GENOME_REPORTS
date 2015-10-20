----------

Haruo Suzuki (haruo[at]g-language[dot]org)  
Last Update: 2015-10-12  

----------

# GENOME_REPORTS project
Project started 2015-10-08.

## Run environment
Mac OS X 10.9.5  
R version 3.2.2 (2015-08-14)  

## Project directories

	GENOME_REPORTS/
	  README.md: project documentation 
	  bin/: scripts
	  data/: sequence data
	  results/: results of analyses

## Data
Data was downloaded on 2015-10-08 from the FTP site into `data/`, using:

	(time sh ../bin/get_GENOME_REPORTS.sh &) > stderr.log 2>&1

## Scripts

*get_GENOME_REPORTS.sh*:

	wget -r -l 1 -nd -A .txt,README ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/
	wget -r -l 2 -nd -A .ids ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/

*blast_parser.pl*:

`blast_parser.pl` was downloaded on 2015-10-09 from <http://kirill-kryukov.com/study/tools/blast-parser/> into `bin/`, using:

	wget http://kirill-kryukov.com/study/tools/blast-parser/blast_parser_1.1.5.zip
	unzip blast_parser_1.1.5.zip

----------

## Steps

	mkdir -p GENOME_REPORTS/{bin,data,results}

### Inspecting Data

	cd GENOME_REPORTS/data/

#### Count 'Kingdom'

	FILE=plasmids.txt
	FILE=overview.txt
	grep -v "^#" $FILE | cut -f2 | sort | uniq -c | awk '{print $2,":",$1}' | sed s/^/$'\t'/g

##### overview.txt

	 407 Archaea
	6931 Bacteria
	1540 Eukaryota
	  45 Viroids
	4808 Viruses

[![](https://github.com/haruosuz/GENOME_REPORTS/blob/master/images/wordle_overview.png)]()
[Word clouds](http://www.wordle.net/advanced) representing the abundance of genome projects. The font size of each Kingdom is proportional to its number in <ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/overview.txt>.

##### plasmids.txt

	 163 Archaea
	5878 Bacteria
	 108 Eukaryota

#### Count 'Group'

	FILE=eukaryotes.txt
	FILE=viruses.txt
	FILE=prokaryotes.txt
	grep -v "^#" $FILE | cut -f5 | sort | uniq -c | sort -rn | sed s/^/$'\t'/g

##### eukaryotes.txt

	1168 Fungi
	 687 Animals
	 311 Protists
	 248 Plants
	  13 Other

	grep -v "^#" $FILE | cut -f5 | sort | uniq -c | sort -rn | awk '{print $2,":",$1}' | sed s/^/$'\t'/g

[![](https://github.com/haruosuz/GENOME_REPORTS/blob/master/images/wordle_eukaryotes.png)]()
[Word clouds](http://www.wordle.net/advanced) representing the abundance of genome projects. The font size of each Group is proportional to its number in <ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/eukaryotes.txt>.

##### prokaryotes.txt

	22327 Proteobacteria
	16987 Firmicutes
	5067 Actinobacteria
	1160 Bacteroidetes/Chlorobi group
	1048 unclassified Bacteria
	 569 Spirochaetes
	 402 Euryarchaeota
	 365 Cyanobacteria
	 300 Tenericutes
	 259 Chlamydiae/Verrucomicrobia group
	 101 Crenarchaeota
	  95 Fusobacteria
	  64 Deinococcus-Thermus
	  57 Chloroflexi
	  56 Thaumarchaeota
	  48 Fibrobacteres/Acidobacteria group
	  46 Thermotogae
	  40 Planctomycetes
	  22 Aquificae
	  20 Nitrospirae
	  19 Synergistetes
	  17 unclassified Archaea
	  10 Nanoarchaeota
	  10 Gemmatimonadetes
	   8 Thermodesulfobacteria
	   6 environmental samples
	   6 Nitrospinae
	   6 Deferribacteres
	   6 Bathyarchaeota
	   4 Nanohaloarchaeota
	   3 Elusimicrobia
	   3 Diapherotrites
	   3 Armatimonadetes
	   3 Aenigmarchaeota
	   2 Dictyoglomi
	   2 Chrysiogenetes
	   2 Caldiserica
	   1 Parvarchaeota
	   1 Lokiarchaeota
	   1 Korarchaeota

##### viruses.txt

	2067 dsDNA viruses, no RNA stage
	1304 ssRNA viruses
	 843 ssDNA viruses
	 223 dsRNA viruses
	 217 Satellites
	 138 Retro-transcribing viruses
	  58 unclassified viruses
	  36 Pospiviroidae
	  31 unclassified phages
	  10 unassigned viruses
	   6 Other
	   5 unclassified virophages
	   5 unclassified viroids
	   4 Avsunviroidae
	   2 unclassified archaeal viruses
	   1 Deltavirus

#### Count 'SubGroup' for "Cyanobacteria" in prokaryotes.txt

	FILE=prokaryotes.txt
	grep "Cyanobacteria" $FILE | cut -f6 | sort | uniq -c | sort -rn | sed s/^/$'\t'/g

	 156 Prochlorales
	 149 Oscillatoriophycideae
	  37 Nostocales
	  13 Stigonematales
	   6 Pleurocapsales
	   2 unclassified Cyanobacteria
	   2 Gloeobacteria

#### Count Genus for lactic acid bacteria (LAB) in prokaryotes.txt
乳酸菌

	FILE=prokaryotes.txt
	grep "Bifidobacterium\|Enterococcus\|Lactococcus\|Lactobacillus\|Leuconostoc\|Pediococcus" $FILE | cut -f1 | cut -d" " -f1 | sort | uniq -c | sed s/^/$'\t'/g

	 260 Bifidobacterium
	 853 Enterococcus
	 494 Lactobacillus
	  59 Lactococcus
	  37 Leuconostoc
	  13 Pediococcus

#### Check 'Status'

	FILE=prokaryotes.txt
	FILE=eukaryotes.txt
	grep -v "^#" $FILE | cut -f19 | sort | uniq -c | sort -rn | sed s/^/$'\t'/g

##### eukaryotes.txt

	1127 Scaffold
	 944 Contig
	 334 Chromosome
	  18 Complete Genome
	   4 Complete

##### prokaryotes.txt

	31941 Contig
	11957 Scaffold
	4408 Complete Genome
	 840 Chromosome

#### Count 'Host' in viruses.txt

	FILE=viruses.txt
	grep -v "^#" $FILE | cut -f9 | sort | uniq -c | sort -rn | sed s/^/$'\t'/g

	1600 bacteria
	1331 plants
	 888 vertebrates
	 324 invertebrates
	 287 vertebrates, human
	  99 fungi
	  82 vertebrates, invertebrates, human
	  69 environment
	  63 archaea
	  59 invertebrates, plants
	  40 vertebrates, invertebrates
	  39 algae
	  36 protozoa
	  15 -
	   5 invertebrates, vertebrates
	   5 human
	   5 diatom
	   3 enviroment

#### Check Viruses.ids

	grep "Dengue virus" Viruses.ids | sed s/^/$'\t'/g

	11053	NC_001477	9626685	0	U88536	Dengue virus 1	viral segment Unknown  
	11060	NC_001474	158976983	0	U87411	Dengue virus 2	viral segment Unknown  
	11069	NC_001475	163644368	0	AY099336	Dengue virus 3	viral segment Unknown  
	11070	NC_002640	12084822	0	AF326825	Dengue virus 4	viral segment Unknown  

	grep -i "ebola" Viruses.ids | sed s/^/$'\t'/g

	186539	NC_004161	22789222	0	AF522874	Reston ebolavirus	viral segment Unknown  
	186540	NC_006432	55770807	0	AY729654	Sudan ebolavirus	viral segment Unknown  
	186541	NC_014372	302315369	0	FJ217162	Tai Forest ebolavirus	viral segment Unknown  
	186538	NC_002549	10313991	0	AF086833	Zaire ebolavirus	viral segment Unknown  

----------

#### Check columns

	FILE=prokaryotes.txt
	FILE=viruses.txt
	FILE=eukaryotes.txt
	FILE=plasmids.txt
	FILE=overview.txt
	cat $FILE | tr '\t' '\n' | less
	# -N # Constantly display line numbers  (press RETURN) # q

##### overview.txt

      1 #Organism/Name
      2 Kingdom
      3 Group
      4 SubGroup
      5 Size (Mb)
      6 Chrs
      7 Organelles
      8 Plasmids
      9 BioProjects

##### plasmids.txt

      1 #Organism/Name
      2 Kingdom
      3 Group
      4 SubGroup
      5 Plasmid Name
      6 RefSeq
      7 INSDC
      8 Size (Kb)
      9 GC%
     10 Protein
     11 rRNA
     12 tRNA
     13 Other RNA
     14 Gene
     15 Pseudogene

##### viruses.txt

      1 #Organism/Name
      2 TaxID
      3 BioProject Accession
      4 BioProject ID
      5 Group
      6 SubGroup
      7 Size (Kb)
      8 GC%
      9 Host
     10 Segmemts
     11 Genes
     12 Proteins
     13 Release Date
     14 Modify Date
     15 Status

##### eukaryotes.txt

      1 #Organism/Name
      2 TaxID
      3 BioProject Accession
      4 BioProject ID
      5 Group
      6 SubGroup
      7 Size (Mb)
      8 GC%
      9 Assembly Accession
     10 Chromosomes
     11 Organelles
     12 Plasmids
     13 WGS
     14 Scaffolds
     15 Genes
     16 Proteins
     17 Release Date
     18 Modify Date
     19 Status
     20 Center
     21 BioSample Accession

##### prokaryotes.txt

      1 #Organism/Name
      2 TaxID
      3 BioProject Accession
      4 BioProject ID
      5 Group
      6 SubGroup
      7 Size (Mb)
      8 GC%
      9 Chromosomes/RefSeq
     10 Chromosomes/INSDC
     11 Plasmids/RefSeq
     12 Plasmids/INSDC
     13 WGS
     14 Scaffolds
     15 Genes
     16 Proteins
     17 Release Date
     18 Modify Date
     19 Status
     20 Center
     21 BioSample Accession
     22 Assembly Accession
     23 Reference
     24 FTP Path
     25 Pubmed ID

----------

## Results & Discussion

----------

## References
- ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/README
- [Nucleic Acids Res. 2015 Jan;43(Database issue):D599-605. "Update on RefSeq microbial genomes resources."](http://www.ncbi.nlm.nih.gov/pubmed/25510495)
- []()
- [ncbi_ftp_download](https://github.com/aleimba/bac-genomics-scripts/tree/master/ncbi_ftp_download) | Scripts to batch download all bacterial genomes of a genus/species from NCBI's FTP site (RefSeq and GenBank) for easy access.

----------
