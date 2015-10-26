----------

Haruo Suzuki (haruo[at]g-language[dot]org)  
Last Update: 2015-10-21  

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

	echo "tab" | sed s/^/$'\t'/g

### Inspecting Data

	cd GENOME_REPORTS/data/

#### Count 'Kingdom'

	FILE=plasmids.txt
	FILE=overview.txt
	grep -v "^#" $FILE | cut -f2 | sort | uniq -c | awk '{print $2,":",$1}'

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

	grep -v "^#" $FILE | cut -f5 | sort | uniq -c | sort -rn | awk '{print $2,":",$1}'

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

	grep -v "^#" viruses.txt | cut -f15 | sort | uniq -c | sort -rn

	4883 Complete Genome
	  44 Complete
	  12 Chromosome
	  11 Chromosome(s)

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

#### Look at specific viruses

	NAME="ebola"
	NAME="Dengue virus"
	grep -i "$NAME" viruses.txt

	Dengue virus 1	11053	PRJNA15306	15306	ssRNA viruses	Flaviviridae	10.735	46.7	vertebrates, invertebrates, human	1	1	1	1997/02/28	2015/09/15	Complete Genome
	Dengue virus 4	11070	PRJNA15599	15599	ssRNA viruses	Flaviviridae	10.649	47.1	vertebrates, invertebrates, human	1	1	1	2001/01/03	2015/09/15	Complete Genome
	Dengue virus 3	11069	PRJNA15598	15598	ssRNA viruses	Flaviviridae	10.707	46.7	vertebrates, invertebrates, human	1	1	1	1993/08/02	2015/09/14	Complete Genome
	Dengue virus 2	11060	PRJNA20183	20183	ssRNA viruses	Flaviviridae	10.723	45.8	vertebrates, invertebrates, human	1	1	1	1993/08/02	2015/09/15	Complete Genome

	Zaire ebolavirus	186538	PRJNA14703	14703	ssRNA viruses	Filoviridae	18.959	41.1	vertebrates, human	1	7	9	1999/02/10	2015/03/30	Complete Genome
	Reston ebolavirus	186539	PRJNA15006	15006	ssRNA viruses	Filoviridae	18.891	40.6	vertebrates, human	1	8	8	2002/09/04	2014/11/12	Complete Genome
	Sudan ebolavirus	186540	PRJNA15012	15012	ssRNA viruses	Filoviridae	18.875	41.3	vertebrates, human	1	7	8	2004/09/25	2014/11/12	Complete Genome
	Tai Forest ebolavirus	186541	PRJNA51257	51257	ssRNA viruses	Filoviridae	18.935	42.3	vertebrates, human	1	7	9	2008/11/21	2014/11/12	Complete Genome

----------

#### Download RefSeq sequences of Viruses:

	NAME="ebola"
	NAME="Dengue virus"
	NAME=$(echo $NAME | sed 's/ /*/g')
	wget -cm -A "*.faa,*.fna"  "ftp://ftp.ncbi.nlm.nih.gov/genomes/Viruses/*$NAME*"

#### Download RefSeq genomes of Bacteria:

	NAME="Lactobacillus crispatus"
	NAME=$(echo $NAME | sed 's/ /_/g')
	wget -cm -A "*.faa,*.fna"  "ftp://ftp.ncbi.nlm.nih.gov/genomes/Bacteria/*$NAME*"

#### Look at header lines (those that begin with >) in the FASTA files

	grep "^>" ftp.ncbi.nlm.nih.gov/genomes/Viruses/Sudan_ebolavirus_uid15012/NC_006432.faa

	>gi|55770808|ref|YP_138520.1| nucleoprotein [Sudan ebolavirus]
	>gi|55770809|ref|YP_138521.1| polymerase complex protein [Sudan ebolavirus]
	>gi|55770810|ref|YP_138522.1| matrix protein [Sudan ebolavirus]
	>gi|55770812|ref|YP_138523.1| spike glycoprotein [Sudan ebolavirus]
	>gi|55770811|ref|YP_138524.1| small secreted glycoprotein [Sudan ebolavirus]
	>gi|55770813|ref|YP_138525.1| minor nucleoprotein [Sudan ebolavirus]
	>gi|55770814|ref|YP_138526.1| membrane-associated protein [Sudan ebolavirus]
	>gi|55770815|ref|YP_138527.1| RNA-dependent RNA polymerase [Sudan ebolavirus]

	grep "^>" ftp.ncbi.nlm.nih.gov/genomes/Bacteria/*/*.faa | head | sed s/^/$'\t'/g

	>gi|295691863|ref|YP_003600473.1| chromosomal replication initiator protein dnaa [Lactobacillus crispatus ST1]
	>gi|295691864|ref|YP_003600474.1| DNA polymerase iii, beta subunit [Lactobacillus crispatus ST1]
	>gi|295691865|ref|YP_003600475.1| RNA-binding s4 domain protein [Lactobacillus crispatus ST1]
	>gi|295691866|ref|YP_003600476.1| DNA replication and repair protein recf [Lactobacillus crispatus ST1]
	>gi|295691867|ref|YP_003600477.1| DNA gyrase, b subunit [Lactobacillus crispatus ST1]
	>gi|295691868|ref|YP_003600478.1| DNA gyrase, a subunit [Lactobacillus crispatus ST1]
	>gi|295691869|ref|YP_003600479.1| 30S ribosomal protein s6 [Lactobacillus crispatus ST1]
	>gi|295691870|ref|YP_003600480.1| single-stranded DNA-binding protein [Lactobacillus crispatus ST1]
	>gi|295691871|ref|YP_003600481.1| 30S ribosomal protein s18 [Lactobacillus crispatus ST1]
	>gi|295691872|ref|YP_003600482.1| prophage repressor [Lactobacillus crispatus ST1]

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
