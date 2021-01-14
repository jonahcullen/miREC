# miREC
An error correction tool for miRNA reads at single-base resolution, which proposed a novel 3-layer lattice structure combining kmers, (k-1)mers and (k+1)mers to first time solve the problem of correcting erroneous bases in miRNA sequencing data. 
The novelty of our method is the use of a 3-layer (k-1)mer-kmer-(k+1)mer lattice structure to maintain the frequency differences of the kmers.
The method is particularly effective for the accurate correction of indel errors. 

Our miREC is a user-friendly tool and suits for different research needs. It provides sev- eral parameters for users to specify according to specific tasks. Three most useful settings are: the error types, the frequency threshold τ, and the kmer range [k_1,k_end]. Our miREC has two running modes. One is for the substitution error correction only, the other is Unlike the exist- ing methods which use a fixed threshold, miREC provides options to obtain a good threshold to identify erroneous kmers, thereby avoiding over-correction or insufficient performance when datasets change.  

The miREC program is written in C++11 and tested on Red Hat Enterprise Linux Workstation release 7.7. It is availble under an open-source license.

## Dependancies
KMC3 tool, a kmer counter, is used to obtain kmer frequences. Here is the instruction of KMC3 (http://sun.aei.polsl.pl/REFRESH/kmc).
In the script miREC.sh, it uses kmc and kmc_dump. Please make sure the KMC tool can be ran on your machine and store in the current miREC folder.

## Download & Usage

	git clone https://github.com/XuanrZhang/miREC
	cd miREC
	make
	chmod +x miREC.sh
	chmod +x kmc* [If you don't install kmc tools] 
	
	
Usage: ./miREC.sh -f [File_Name] -s [k_1] -e [k_end] -t [the number of threads] [run_type]

	Required OPTIONS:
	-f [File_Name]: cleaned miRNA sequence fastq dataset （fastq files）

	Optional OPTIONS:
	-t [the number of threads]: default is 8;
	-s [k_1]: 15;
	-e [k_end]: 20;
	-o [Ouput_FileName]: default is correct_read.fastq;
	[run_type]: default is mix, "-u" for substitution errors only;
	
Examples: 
	# test using simulated datasets in github folders.
	./miREC.sh -f ./Data/simulated_data/mix_data/simumD1.fq -s 10 -e 12 -t 26 (correct substitution and indel errors, with threshold_value 5 and k_value from 10 to 12, with 26 threads)
	./miREC.sh -f ./Data/simulated_data/mix_data/simumD1.fq -s 10 -e 12 -t 26 -u -o Correct.fastq(correct substitution errors only, with threshold_value 5 and k_value from 10 to 12, with 26 threads; Setting output file name as Correct.fastq)
	
	# test user's datasets (user_input.fq)
	./miREC.sh -f user_input.fq -t 5 -s 8 -e 20 -t 26 (correct substitution and indel errors, with threshold_value 5 and k_value from 8 to 20,with 26 threads)
	./miREC.sh -f user_input.fq -t 5 -s 8 -e 20 -u  -t 26 (correct substitution errors only, with threshold_value 5 and k_value from 8 to 20,with 26 threads)
	
  
## Data format
Input: A clean miRNA read dataset in fastq format(After adapter cutting)

	- read.fq : store the whole read data needed to be corrected.
	
Output: A corrected read dataset 

	- correct_read.fq :store corrected read data.
	
## Data materials

Simulated data：generated by scprits （gene_simu_mixerr.c / gene_simu_sub.c）

Real salmon data: can be downloaded at the link (https://www.ncbi.nlm.nih.gov//bioproject/PRJNA202973), with the Accession Number PRJNA202973.

We also provide copies in miREC/data file folder.

	
## Citation
Please cite the work "Aberration-corrected ultrafine analysis of miRNA reads at single-base resolution: better data makes better conclusion."

## Citation
If any bugs during your running, please email to xuan.zhang-5@student.uts.edu.au
