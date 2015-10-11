----------

Haruo Suzuki (haruo[at]g-language[dot]org)  
Last Update: 2015-10-08  

----------

# GENOME_REPORTS project
Project started 2015-10-08.

## Run environment
Mac OS X 10.9.5

	$uname -a
	Darwin localhost 13.4.0 Darwin Kernel Version 13.4.0: Wed Mar 18 16:20:14 PDT 2015; root:xnu-2422.115.14~1/RELEASE_X86_64 x86_64

	> sessionInfo()
	R version 3.2.2 (2015-08-14)
	Platform: x86_64-apple-darwin13.4.0 (64-bit)
	Running under: OS X 10.9.5 (Mavericks)

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
	mkdir results-$(date +%F)

### Inspecting Data

	cd data/

#### Count '2 Kingdom'

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

#### Count '5 Group'

	FILE=prokaryotes.txt
	FILE=eukaryotes.txt
	FILE=viruses.txt
	grep -v "^#" $FILE | cut -f5 | sort | uniq -c | sort | sed s/^/$'\t'/g

##### eukaryotes.txt

	  13 Other
	 248 Plants
	 311 Protists
	 687 Animals
	1168 Fungi

[![](https://github.com/haruosuz/GENOME_REPORTS/blob/master/images/wordle_eukaryotes.png)]()
[Word clouds](http://www.wordle.net/advanced) representing the abundance of genome projects. The font size of each Group is proportional to its number in <ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/eukaryotes.txt>.

##### viruses.txt

	   1 Deltavirus
	   2 unclassified archaeal viruses
	   4 Avsunviroidae
	   5 unclassified viroids
	   5 unclassified virophages
	   6 Other
	  10 unassigned viruses
	  31 unclassified phages
	  36 Pospiviroidae
	  58 unclassified viruses
	 138 Retro-transcribing viruses
	 217 Satellites
	 223 dsRNA viruses
	 843 ssDNA viruses
	1304 ssRNA viruses
	2067 dsDNA viruses, no RNA stage

##### prokaryotes.txt

	   1 Aquificae
	   1 Caldiserica
	   1 Deinococcus-Thermus
	   1 Gemmatimonadetes
	   1 Parvarchaeota
	   1 Thermotogae
	   2 Nitrospirae
	   2 Planctomycetes
	   3 Aenigmarchaeota
	   3 Diapherotrites
	   3 Nanohaloarchaeota
	   6 Chloroflexi
	   9 Nanoarchaeota
	  10 Tenericutes
	  12 Crenarchaeota
	  15 Fibrobacteres/Acidobacteria group
	  17 Chlamydiae/Verrucomicrobia group
	  19 Euryarchaeota
	  21 Bacteroidetes/Chlorobi group
	  28 Thaumarchaeota
	  34 Spirochaetes
	  44 Cyanobacteria
	 139 unclassified Bacteria
	1789 Actinobacteria
	4039 Proteobacteria
	4785 Firmicutes

#### Count '6 SubGroup' for "Cyanobacteria" in prokaryotes.txt

	FILE=prokaryotes.txt
	grep "Cyanobacteria" $FILE | cut -f6 | sort | uniq -c | sort | sed s/^/$'\t'/g

	   2 Gloeobacteria
	   2 unclassified Cyanobacteria
	   6 Pleurocapsales
	  13 Stigonematales
	  37 Nostocales
	 149 Oscillatoriophycideae
	 156 Prochlorales

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

#### Check '19 Status'

	FILE=eukaryotes.txt
	FILE=prokaryotes.txt
	grep -v "^#" $FILE | cut -f19 | sort | uniq -c | sort | sed s/^/$'\t'/g

##### eukaryotes.txt

	   4 Complete
	  18 Complete Genome
	 334 Chromosome
	 944 Contig
	1127 Scaffold

##### prokaryotes.txt

	 840 Chromosome
	11957 Scaffold
	31941 Contig
	4408 Complete Genome

#### Count '9 Host' in viruses.txt

	FILE=viruses.txt
	grep -v "^#" $FILE | cut -f9 | sort | uniq -c | sort | sed s/^/$'\t'/g

	   3 enviroment
	   5 diatom
	   5 human
	   5 invertebrates, vertebrates
	  15 -
	  36 protozoa
	  39 algae
	  40 vertebrates, invertebrates
	  59 invertebrates, plants
	  63 archaea
	  69 environment
	  82 vertebrates, invertebrates, human
	  99 fungi
	 287 vertebrates, human
	 324 invertebrates
	 888 vertebrates
	1331 plants
	1600 bacteria

#### Check Viruses.ids

	grep "Dengue virus" Viruses.ids | sed s/^/$'\t'/g

	11053	NC_001477	9626685	0	U88536	Dengue virus 1	viral segment Unknown  
	11060	NC_001474	158976983	0	U87411	Dengue virus 2	viral segment Unknown  
	11069	NC_001475	163644368	0	AY099336	Dengue virus 3	viral segment Unknown  
	11070	NC_002640	12084822	0	AF326825	Dengue virus 4	viral segment Unknown  

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
- [Nucleic Acids Res. 2015 Jan;43(Database issue):D599-605. "Update on RefSeq microbial genomes resources."](http://www.ncbi.nlm.nih.gov/pubmed/25510495)
- []()
- []()
- []()
- [ncbi_ftp_download](https://github.com/aleimba/bac-genomics-scripts/tree/master/ncbi_ftp_download) | Scripts to batch download all bacterial genomes of a genus/species from NCBI's FTP site (RefSeq and GenBank) for easy access.

----------
