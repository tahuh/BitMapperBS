BitMapperBS: a fast and accurate read aligner for whole-genome bisulfite sequencing
============






### Introduction ###
-------  

Here are the implementations of "BitMapperBS: a fast and accurate read aligner for whole-genome bisulfite sequencing". 
BitMapperBS is an ultra-fast and memory-efficient aligner that is designed for WGBS reads
from directional protocol. 


### Build Requirements ###

(1) G++.

(2) CMake.

(3) CMake-supported build tool.



### Installation ###
(1) Download the source code from Github

    git clone https://github.com/chhylp123/BitMapperBS.git

(2) Build (CPU must support AVX2 instructions)
    
    cd BitMapperBS
    make

(3) If the CPU does not support AVX2 instructions (in this case, BitMapperBS may be slower without AVX2 instructions)

    cd BitMapperBS/for_old_machine_sse4.2
    make

If you have problem with the "make" part of the compile procedure described below, please contact us (chhy@mail.ustc.edu.cn).

    

### Indexing Genome ###
    
    ./bitmapperBS --index <genome file name>


### Quality and Adapter Trimming ###

We recommd users to use Trim_Galore to perform the quality and adapter trimming. Please see https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/. 

### Bisulfite Mapping ###

single-end reads

    ./bitmapperBS --search <genome file name> --seq <read file name> [options]

paired-end reads

    ./bitmapperBS --search <genome file name> --seq1 <read1 file name> --seq2 <read2 file name> --pe [options]


### General Options ###


| Option | Short Option | Type | Default | Brief Description |
| :------: | :---------------: | :-----:|:-----:| :-----|
| --help | -h | String | NULL | Show the help information. |
| --version | -v | String | NULL | Show current version of BitMapperBS. |

### Index Options ###

| Option | Short Option | Type | Default | Brief Description |
| :------: | :---------------: | :-----:|:-----:| :-----|
| --index | -i | String | NULL | Generate an index from the specified fasta file. |


### Mapping Options ###



| Option | Short Option | Type | Default | Brief Description |
| :------: | :---------------: | :-----:|:-----:| :-----|
| --search | NULL| String | NULL | Provide the path to the fasta file when aligning.|
| --fast | NULL| String | NULL | Set bitmapperBS in fast mode (default). Only available for paired-end mode.|
| --sensitive | NULL| String | NULL | Set bitmapperBS in sensitive mode. Only available for paired-end mode.|
| --pe | NULL| NULL | NULL | Searching will be done in paired-end mode. |
| --seq | NULL| String | NULL | Provide the name of single-end read file. |
| --seq1 | NULL| String | NULL | Provide the name of paired-end read_1 file. |
| --seq2 | NULL| String | NULL | Provide the name of paired-end read_2 file. |
| -o | -o | String | output | Provide the name of output file. |
| -e | -e | Double | 0.08 | Set the edit distance rate of read length, which is between 0 and 1. |
| --min | NULL | Int | 0 | Minimum distance between a pair of end sequences. |
| --max | NULL | Int | 0 | Maximum distance between a pair of end sequences. |
| --threads | -t | Int | 1 | Set the number of CPU threads. |
| --pbat | NULL | NULL | NULL | Map the bs-seq from pbat protocol. |






### Examples ###

(1) **Indexing Genome**

For example, to make an index for human genome (GRCH38):

	./bitmapperBS --index human_genome.fa
   
The suffix of the index file should be '.index.*'.

(2) **Quality and Adapter Trimming**

For example, to trim paired-end reads:

	trim_galore --paired read_1.fq read_2.fq
    
(3) **Bisulfite Mapping**

For example, to map reads to human genome (GRCH38) in single-end mode using 6 CPU threads:

	./bitmapperBS --search ../../ssd/human_genome.fa --seq ../../ssd/read.fq -t 6
    
If map the reads from the *_2 reads file or the pbat protocol, the --pbat option should be set:

    	./bitmapperBS --search ../../ssd/human_genome.fa --seq ../../ssd/read_2.fq --pbat -t 6

to map reads to human genome (GRCH38) in paired-end mode using 6 CPU threads:

	./bitmapperBS --search ../../ssd/human_genome.fa --seq1 ../../ssd/read_1.fq --seq2 ../../ssd/read_1.fq --pe -t 6


The default maximum allowed edit distance is 8% of read length (0.08). This option can be changed using -e option. In this example, the maximum allowed edit distance is set to 4% of read length:

    ./bitmapperBS --search ../../ssd/human_genome.fa --seq ../../ssd/read.fq -t 6 -e 0.04



### Contacts ###

Haoyu Cheng: chhy@mail.ustc.edu.cn


### Note ###
-------
* We adopt the pSAscan algorithm [1] to build the suffix array, and build BWT from suffix array.



### References ###
-------


[1] Kärkkäinen J, Kempa D, Puglisi S J. Parallel external memory suffix sorting[C]//Annual Symposium on Combinatorial Pattern Matching. Springer, Cham, 2015: 329-342.
