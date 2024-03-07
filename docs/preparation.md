# ![logo](logo_tiny.png "nanoCEM")Data preparation from raw reads

## Raw data from Oxford Nanopore Technologies sequencing

With the introduction of **R10.4.1** flow cell, the diversity of ONT data formats has increased. 
These now include the original single and multiple **fast5** format, the newer **pod5** format,
and the community-driven **slow5/blow5** formats. The relationship between conversion tools 
and these different data formats is as below.

 <center>![data format](data_format.png "data_format") </center>

nanoCEM only supports single-format **fast5** and **blow5**; please transfer the data format before usage.
Since we support `tombo`, `move_table` and `f5c`, while `tombo` only supports single format **fast5** and our `f5c` and `move_table` mode supports **blow5**.
Here are some advice commands if your original data is multi-format **fast5**.

    # pod5 to blow5
    blue-crab p2s file.pod5 -o file.blow5

    # fasts to blow5
    slow5tools f2s </path/to/multi_reads> -d </path/to/blow5_dir>
    slow5tools cat </path/to/blow5_dir> -o file.blow5


## Basecall your raw reads
After obtaining raw reads files, the first step is to basecall them.
Here is an example script to run **Guppy** and **Dorado** basecaller. You can find more details about basecalling at ONT:

    # Guppy basecaller
    guppy_basecaller -i <path/to/fastt> -s <path/to/fastq> --config <config file> --device auto -r
    cat <path/to/fastq> > file.fastq
    # Dorado basecaller (support fast5 & pod5)
    dorado basecaller <model> </path/to/reads> > dorado.bam
    samtools bam2fq dorado.bam > file.fastq

## Choose your reference
The alignment of DNA is relatively simple, but for RNA, it becomes more complex due to the presence of 
alternative splicing and multiple isoforms in eukaryotic organisms. 

The alignment process is already embedded in nanoCEM. For general analysis, if you use RNA mode,
it is advisable to use **transcriptome** as the reference, while for DNA, the reference would be the **genome**.

## Re-squiggle process

nanoCEM supports the re-squiggle results of both `tombo` and `f5c`, 
while f5c has already been integrated into our script, 
`tombo` users will need to install the required environment on their own.
Due to Tombo being an older tool and no longer receiving active updates, 
it presents challenges for seamless integration into our pipeline.

### tombo

**Optional:**
We provide a python script `extract_sub_fast5_from_bam` to assist in sampling reads aligned to your region of interest to reduce re-squiggle time.

    extract_sub_fast5_from_bam -i </path/to/single_reads> -o </path/to/results> -b file.bam --chrom NR_103073.1 --pos 2030 --cpu 32

Tombo is a suite of tools primarily for the identification of modified nucleotides from nanopore sequencing data, and firstly proposed
the generation of re-squiggle.

    tombo preprocess annotate_raw_with_fastqs --fast5-basedir  </path/to/single_reads> --fastq-filenames file.fastq --processes 16 
    tombo resquiggle </path/to/single_reads> reference.fasta --processes 16 --num-most-common-errors 5

**Notes:**  Since the re-squiggle process in tombo can be time-consuming, we recommend using an SSD (Solid State Drive) 
to reduce processing time. 