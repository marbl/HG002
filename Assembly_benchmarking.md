# Benchmarking using HG002 v1.1 and GQC
 
## Input Data (HPRC recipe)
The data is based on the HPRC recipe combining approximately 60x HiFi, 60x ONT UL >100 kbp, and 50x Hi-C.

<details>
<summary><h2>Data Details</h2></summary>
Note that the dataset includes both R9 and R10 datasets. The R9 data is optional but was included in the provided Verkko assembly.

### HiFi Revio Data
Download the [HiFi data](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/polishing/HG002/v1.0/mapping/hifi_revio_pbmay24/hg002v1.0.1_hifi_revio_pbmay24.bam)

### Oxford Nanopore Data
Download the [R10 data](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/polishing/HG002/v1.0/mapping/ont_r10_ul_dorado/hg002v1.0_ont_r10_ul_dorado.bam).
Download the optional [R9 runs](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=NHGRI_UCSC_panel/HG002/nanopore/ultra-long/03_08_22_R941_HG002_rebasecalling-guppy-6.3.7). The assembly only used files `03*_[4-6]*.fq.gz`.

### HiC
Hi-C data is available from the [HPRC](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=working/HPRC_PLUS/HG002/raw_data/hic/downsampled/). The assembly used `HG002.HiC_1_S*fastq.gz` files.
 </details>
 
## Assembly
The assembly consists of two steps, first the ONT UL data is corrected using [Hifiasm](https://github.com/chhylp123/hifiasm) using both ONT R10 UL data and HiFi data. The corrected ONT UL data, raw HiFi data, raw ONT UL, and Hi-C data are the co-assembled with [Verkko](https://github.com/marbl/verkko). This pipeline also requires python 3.8+, winnowmap, mashmap 3+, samtools, bwa, and seqtk.

<details>
<summary><h3>Detailed Assembly Commands</h3></summary>

Define
````
HIFI="hg002v1.0.1_hifi_revio_pbmay24.bam"
ONT_R10="hg002v1.0_ont_r10_ul_dorado.bam"
ONT="03_08_22_R941_HG002_4.fq.gz 03_08_22_R941_HG002_5.fq.gz 03_08_22_R941_HG002_6.fq.gz"
HIC1="HG002.HiC_1_S1_R1_001.fastq.gz HG002.HiC_1_S2_R1_001.fastq.gz HG002.HiC_1_S3_R1_001.fastq.gz"
HIC2="HG002.HiC_1_S1_R2_001.fastq.gz HG002.HiC_1_S2_R2_001.fastq.gz HG002.HiC_1_S3_R2_001.fastq.gz"
````

### Correction with Hifiasm
This used hifiasm v0.25.0r910 or later. This is currently available under the [hybrid_v1 branch](https://github.com/chhylp123/hifiasm/tree/f5078f7b23fa3ba546189255d0242756c25619ca). We only correct reads over 20 kbp below to save compute time/memory due to the deep coverage of this dataset. Typically we recommend 10kb as a filtering threshold. Adjust the -t option to the number of cores available on your system.
````
   (
   for f in $HIFI; do
     if [[ $f == *.bam ]]; then
        samtools fastq "$f"
     elif [[ $f == *.fastq.gz || $f == *.fq.gz ]]; then
        zcat "$f"
     elif [[ $f == *.fastq.bz2 || $f == *.fq.bz2 ]]; then
        bzcat "$f"
     elif [[ $f == *.fastq || $f == *.fq ]]; then
        cat "$f"
     else
        echo "Warning: unrecognized format for hifiInput $f" >&2
     fi
   done
   ) | bgzip -@ $zipCPUs $zipOpt -c -I hifi.input.fastq.gz.gzi - > hifi.input.fastq.gz
   (
   for f in $ONT_R10; do
     if [[ $f == *.bam ]]; then
        samtools fastq "$f"
     elif [[ $f == *.fastq.gz || $f == *.fq.gz ]]; then
        zcat "$f"
     elif [[ $f == *.fastq.bz2 || $f == *.fq.bz2 ]]; then
        bzcat "$f"
     elif [[ $f == *.fastq || $f == *.fq ]]; then
        cat "$f"
     else
        echo "Warning: unrecognized format for hifiInput $f" >&2
     fi
   done
   ) | seqtk seq -L 20000 - | bgzip -@ 8 -l 9 -i -c -I r10-hifiasm-correct.input.fastq.gz.gzi - > r10-hifiasm-correct.input.fastq.gz
   hifiasm -e --write-ec --ont r10-hifiasm-correct.input.fastq.gz \
        --hf hifi.input.fastq.gz --hom-cov 185  \
        -o r10-hifiasm-correct.WORKING -t 64 && \
   bgzip -@ 8 -l 9 -i r10-hifiasm-correct.WORKING.ec.fq && \
   samtools faidx r10-hifiasm-correct.WORKING.ec.fq.gz && \
   cat r10-hifiasm-correct.WORKING.ec.fq.gz.fai | grep     "^m" | awk '{print $1}' > r10-hifiasm-correct.hifi.ids && \
   cat r10-hifiasm-correct.WORKING.ec.fq.gz.fai | grep  -v "^m" | awk '{print $1}' > r10-hifiasm-correct.ont.ids  && \
   seqtk subseq r10-hifiasm-correct.WORKING.ec.fq.gz r10-hifiasm-correct.hifi.ids | bgzip -@ $zipCPUs $zipOpt -c -I r10-hifiasm-correct.ec.hifi.fq.gz.gzi > r10-hifiasm-correct.ec.hifi.fq.gz && \
   seqtk subseq r10-hifiasm-correct.WORKING.ec.fq.gz r10-hifiasm-correct.ont.ids  | bgzip -@ $zipCPUs $zipOpt -c -I r10-hifiasm-correct.ec.ont.fq.gz.gzi  > r10-hifiasm-correct.ec.ont.fq.gz  && \
   mv r10-hifiasm-correct.WORKING.ec.fq.gz r10-hifiasm-correct.ec.fq.gz && \
   mv r10-hifiasm-correct.WORKING.ec.fq.gz.gzi r10-hifiasm-correct.ec.fq.gz.gzi && \
   mv r10-hifiasm-correct.WORKING.ec.fq.gz.fai r10-hifiasm-correct.ec.fq.gz.fai && \
````

The `--hom-cov` parameter is set based on the estimated coverage of ONT and HiFi data (sum of bases divided by 3.1 Gbp). The script include pre-processing various inputs into a format compatible with hifiasm. This requires <800 GB RAM and 3k CPU h on our cluster. Only the `r10-hifiasm-correct.ont.fq.gz` is used after correction so you can remove other corrected outputs to save space. You can download the computed [corrected reads](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/09cd8aa1-726c-4cb3-aeea-36e71bab75ff--HG002_hybrid_benchmark/r10-hifiasm-correct.ec.ont.fq.gz).

### Assembly with verkko
This requires verkko v2.3 or later. The corrected ONT UL data is input as high-quality sequences to verkko while the raw ONT UL is input as resolving ONT data. The supplied command will run verkko on a slurm cluster, assuming one is available and auto-request memory/time for each job. Omit the `--slurm` option if you would like to run verkko on a single node instead, it will auto-detect available CPUs/memory.

````
  verkko --slurm -d verkko-hi-c \
    --ovb-run 8 32 32 \
    --screen-human-contaminants \
    --hifi $HIFI r10-hifiasm-correct.ec.ont.fq.gz \
    --nano $ONT $ONT_R10 \
    --hic1 $HIC1 \
    --hic2 $HIC2 \
    --unitig-abundance 8  
````

The parameter `--unitig-abundance 8` is recommended for high-coverage datasets (the total coverage of ONT UL and HiFi here is 180x). This requires <300 GB RAM and 13K CPU h or 41 hrs walltime on our cluster. 
</details>

Download the [complete assembly](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/09cd8aa1-726c-4cb3-aeea-36e71bab75ff--HG002_hybrid_benchmark/assembly.fasta.gz) or [haplotype1](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/09cd8aa1-726c-4cb3-aeea-36e71bab75ff--HG002_hybrid_benchmark/assembly.haplotype1.fasta.gz) and [haplotype2](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/09cd8aa1-726c-4cb3-aeea-36e71bab75ff--HG002_hybrid_benchmark/assembly.haplotype2.fasta.gz) separately.

## Polishing

The assembly QV (using yak with k=31 mers from Illumina data) is estimated at 56 (Q48 via GQC). Polishing is currently not recommended/necessary.

## Validation

GQC run assumes you've downloaded GQC and the v1.1 benchmark dataset from [here](https://github.com/marbl/GQC). 

<details>
<summary><h3>Detailed QC Commands</h3></summary>

````
CONFIGFILE=<path to GQC>/GQC/benchconfig.txt

ln -s <path to v1.1>/v1.1.fasta.gz     || true
ln -s <path to v1.1>/v1.1.fasta.gz.gzi || true
ln -s <path to v1.1>/v1.1.fasta.gz.fai || true

asm="assembly.fasta"
if [ ! -e "$asm" ]; then
   asm="assembly.fasta.gz"
fi
if [ ! -e "$asm" ]; then
   echo "Error: cannot find either assembly.fasta or assembly.fasta.gz"
   exit 1
fi

GQC -a minimap2 -c $CONFIGFILE -r v1.1.fasta.gz -q $asm -p gqc -A verkko_hg002 -B v1.1
````
</details>

Download the pre-computed [GQC output](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/09cd8aa1-726c-4cb3-aeea-36e71bab75ff--HG002_hybrid_benchmark/gqc.tar.gz).
