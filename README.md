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

*get_GENOME_REPORTS.sh*:

	wget -r -l 1 -nd -A .txt,README ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/
	wget -r -l 2 -nd -A .ids ftp://ftp.ncbi.nih.gov/genomes/GENOME_REPORTS/

----------

## Codes

	mkdir -p GENOME_REPORTS/{bin,data,results}
	mkdir results-$(date +%F)

### Inspecting Data

	cd data/

#### Check columns

	cat prokaryotes.txt | tr '\t' '\n' | less
	# -N # Constantly display line numbers  (press RETURN) # q

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

#### Count Groups

	cut -f5 prokaryotes.txt | sort | uniq -c | sort | sed s/^/$'\t'/g

	   1 Aquificae
	   1 Caldiserica
	   1 Deinococcus-Thermus
	   1 Gemmatimonadetes
	   1 Group
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
