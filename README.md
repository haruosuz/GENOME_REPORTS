----------

Haruo Suzuki <haruo@g-language.org>  
Last Update: 2015-10-08

----------

# GENOME_REPORTS project
Project started 2015-10-08.

## Project directories

	GENOME_REPORTS/
	  README.md: project documentation 
	  bin/: scripts
	  data/: sequence data
	  results/: results of analyses

## Data
Data was downloaded on 2015-10-08 from the FTP site <ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/> into `data/`, using:

	cd data/
	(time sh ../bin/get_GENOME_REPORTS.sh &) > stderr.log 2>&1


## Scripts

*bin/get_GENOME_REPORTS.sh*:

	wget -r -l 1 -nd -A .txt,README ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/
	wget -r -l 2 -nd -A .ids ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/

----------

## Codes

	mkdir -p GENOME_REPORTS/{bin,data,results}
	mkdir results-$(date +%F)

### Inspecting Data

	cd data/

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

#### Count '2 Kingdom'

	FILE=plasmids.txt
	FILE=overview.txt
	grep -v "^#" $FILE | cut -f2 | sort | uniq -c | sed s/^/$'\t'/g

##### overview.txt

	 407 Archaea
	6931 Bacteria
	1540 Eukaryota
	  45 Viroids
	4808 Viruses

##### plasmids.txt

	 108 Eukaryota
	 163 Archaea
	5878 Bacteria

#### Count '5 Group'

	FILE=prokaryotes.txt
	FILE=eukaryotes.txt
	FILE=viruses.txt
	grep -v "^#" $FILE | cut -f5 | sort | uniq -c | sort | sed s/^/$'\t'/g

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

----------

## Results & Discussion


----------

## References
- []()
- [Nucleic Acids Res. 2015 Jan;43(Database issue):D599-605. "Update on RefSeq microbial genomes resources."](http://www.ncbi.nlm.nih.gov/pubmed/25510495)

----------
