# miREC
An error correction tool for miRNA reads at single-base resolution, which proposed a novel 3-layer lattice structure combining kmers, (k-1)mers and (k+1)mers to first time solve the problem of correcting erroneous bases in miRNA sequencing data. 
The novelty of our method is the use of a 3-layer (k-1)mer-kmer-(k+1)mer lattice structure to maintain the frequency differences of the kmers.
The method is particularly effective for the accurate correction of indel errors. 

Our miREC is a user-friendly tool and suits for different research needs. It provides sev- eral parameters for users to specify according to specific tasks. Three most useful settings are: the error types, the frequency threshold τ, and the kmer range [k_1,k_end]. Our miREC has two running modes. One is for the substitution error correction only, the other is Unlike the exist- ing methods which use a fixed threshold, miREC provides options to obtain a good threshold to identify erroneous kmers, thereby avoiding over-correction or insufficient performance when datasets change.  

The miREC program is written in C++11 and tested on Red Hat Enterprise Linux Workstation release 7.7. It is availble under an open-source license.

## Dependancies
**KMC3 tool**, a kmer counter, is used to obtain kmer frequences. Here is the instruction of KMC3 (http://sun.aei.polsl.pl/REFRESH/kmc).
In the script miREC.sh, it uses kmc and kmc_dump. Please make sure the KMC tool can be ran on your machine and store in the current miREC folder.

**Cutadapt tool**, Cutadapt finds and removes adapter sequences, primers, poly-A tails and other types of unwanted sequence from your high-throughput sequencing reads.(https://cutadapt.readthedocs.io/en/stable/)
The miREC uses cutadapt. Please make sure the cutadapt tool can be ran on your machine and store in the current miREC folder.


## Download & Usage

Download

	git clone https://github.com/XuanrZhang/miREC
	cd miREC
	make
	chmod +x miREC.sh
	chmod +x kmc* [If you don't install kmc tools] 
	chmod +x cutadapt [If you don't install cutadapt tools]
	
	
Usage: ./miREC.sh -f [Input_File] -s [k_1] -e [k_end] -t [the number of threads] -o [Output_File] -u [run_type]

	Required OPTIONS:
	-f [File_Name]: miRNA sequence fastq dataset（fastq files）

	Optional OPTIONS:
	-t [the number of threads]: default is 8;
	-r [the value of threshold]: default is 5;
	-s [k_1]: 8;
	-e [k_end]: 15;
	-o [Ouput_FileName]: default is correct_read.fastq;
	[run_type]: default is mix, "-u" for substitution errors only;
	[cut_adapter]: " -c" to open cutadatper function, use by -c [adapter sequence] (e.g. -c GCCTTGGCACCCGAGAATTCCA);
	
Examples: 

	# test using simulated datasets in github folders.
	./miREC.sh -f ./Data/simulated_data/mix_data/simumD1.fq -s 8 -e 15 -t 26 (correct substitution and indel errors, with threshold_value 5 and k_value from 8 to 15, with 26 threads)
	./miREC.sh -f ./Data/simulated_data/mix_data/simumD1.fq -s 8 -e 15 -t 26 -r 6 (correct substitution and indel errors, with threshold_value 5 and k_value from 8 to 15, with 26 threads, with kmer frequency threshold as 6)
	./miREC.sh -f ./Data/simulated_data/mix_data/simumD1.fq -s 8 -e 15 -t 26 -c TGGAATTCTCGGGTGCCAAGG (cutadapter with sequence "TGGAATTCTCGGGTGCCAAGG" and then correct substitution and indel errors, with threshold_value 5 and k_value from 8 to 15, with 26 threads)
	./miREC.sh -f ./Data/simulated_data/mix_data/simumD1.fq -s 8 -e 15 -t 26 -u -o Correct.fastq(correct substitution errors only, with threshold_value 5 and k_value from 8 to 15, with 26 threads; Setting output file name as Correct.fastq)
	
	# test user's datasets (user_input.fq)
	./miREC.sh -f user_input.fq -s 8 -e 15 -t 26 (correct substitution and indel errors, with threshold_value 5 and k_value from 8 to 15,with 26 threads)
	./miREC.sh -f user_input.fq -s 8 -e 15 -u  -t 26 (correct substitution errors only, with threshold_value 5 and k_value from 8 to 15,with 26 threads)

**Tips for kvalue selection**:

	**The kmer range parameter is recommanded as [8,15].**
	Every iterative step of miREC with the increasing length of kmer each time by 1 in the range [k1 , kend ] actually corrects different amounts of errors. According to experiments, after five consecutive lengths of k are iterated, about 99.61% of substitution errors, 88.77% of insertion errors and 94.63% of deletion errors can be corrected if k1 is set as 8. With more loops of correction, more erroneous bases are detected and corrected. As each iterative loop consumes the same order of time complexity, users are suggested to narrow the kmer range (by setting kend smaller) to shorten the program running time while correcting almost all of the errors for those miRNA sequencing datasets of huge size.
  
## Data format
Input: A clean miRNA read dataset in fastq format(After adapter cutting)

	- read.fq : store the whole read data needed to be corrected.
	
Output: A corrected read dataset 

	- correct_read.fq :store corrected read data.
	
## Data materials

**Simulated data**：generated by scprits （gene_simu_mixerr.c / gene_simu_sub.c）

**Real salmon data**: can be downloaded at the link (https://www.ncbi.nlm.nih.gov//bioproject/PRJNA202973), with the Accession Number PRJNA202973.

**963 miRXplore Universal Reference miRNAs read data**: can be downloaded at the link (https://github.com/Jappy0/miREC/tree/main/src), as well as related code and results.

We also provide copies in miREC/data file folder.

	
## Citation
Please cite the [work](https://academic.oup.com/nar/advance-article/doi/10.1093/nar/gkab610/6325352).
Zhang, Xuan, et al. "Aberration-corrected ultrafine analysis of miRNA reads at single-base resolution: a k-mer lattice approach." Nucleic Acids Research (2021).

## Question
If any bugs during your running, please email to xuan.zhang-5@student.uts.edu.au
