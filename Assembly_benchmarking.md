# Benchmarking using HG002 v1.1 and GQC
 
## Input Data (HPRC recipe)
The data is based on the HPRC recipe combining approximately 60x HiFi, 60x ONT UL >100 kbp, and 50x Hi-C.

<details>
<summary><h2>Download</h2></summary>

### HiFi Revio Data
Available from the [HPRC](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/polishing/HG002/v1.0/mapping/hifi_revio_pbmay24/hg002v1.0.1_hifi_revio_pbmay24.bam)

### Oxford Nanopore Data
There is both R9 and R10 data available from HPRC. Download from the [R9 folder](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=NHGRI_UCSC_panel/HG002/nanopore/ultra-long/03_08_22_R941_HG002_rebasecalling-guppy-6.3.7/) includes multiple runs.  The assembly only used files `03*_[4-6]*.fq.gz`. 
Download the [R10 file](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/polishing/HG002/v1.0/mapping/ont_r10_ul_dorado/hg002v1.0_ont_r10_ul_dorado.bam).
 
### HiC
Hi-C data is available from the [HPRC](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=working/HPRC_PLUS/HG002/raw_data/hic/downsampled/). The assembly used `HG002.HiC_1_S*fastq.gz` files.
 </details>
 
## Assembly
The assembly consists of two steps, first the ONT UL data is corrected using [Hifiasm](https://github.com/chhylp123/hifiasm). The corrected ONT UL data, HiFi data, ONT UL raw, and Hi-C data are the co-assembled with [Verkko](https://github.com/marbl/verkko). This pipeline also requires python 3.8+, winnowmap, mashmap 3+, samtools, bwa, and seqtk. We define

<details>
<summary><h3>Commands</h3></summary>

````
HIFI="hg002v1.0.1_hifi_revio_pbmay24.bam"
ONT_R10="hg002v1.0_ont_r10_ul_dorado.bam"
ONT="03_08_22_R941_HG002_4.fq.gz 03_08_22_R941_HG002_5.fq.gz 03_08_22_R941_HG002_6.fq.gz"
HIC1="HG002.HiC_1_S1_R1_001.fastq.gz HG002.HiC_1_S2_R1_001.fastq.gz HG002.HiC_1_S3_R1_001.fastq.gz"
HIC2="HG002.HiC_1_S1_R2_001.fastq.gz HG002.HiC_1_S2_R2_001.fastq.gz HG002.HiC_1_S3_R2_001.fastq.gz"
````

### Correction with Hifiasm
This used hifiasm v0.25.0r726 or later. We only correct reads over 1 kbp below to save compute time/memory
````
   (

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
   ) | seqtk seq -L 1000 - | bgzip -@ 8 -l 9 -i -c -I r10-hifiasm-correct.input.fastq.gz.gzi - > r10-hifiasm-correct.input.fastq.gz 
   hifiasm -e --write-ec --ont r10-hifiasm-correct.input.fastq.gz \
        -o r10-hifiasm-correct.WORKING -t 32 && \
   bgzip -@ 8 -l 9 -i r10-hifiasm-correct.WORKING.ec.fq && \
   samtools faidx r10-hifiasm-correct.WORKING.ec.fq.gz && \
   mv r10-hifiasm-correct.WORKING.ec.fq.gz r10-hifiasm-correct.ec.fq.gz && \
   mv r10-hifiasm-correct.WORKING.ec.fq.gz.gzi r10-hifiasm-correct.ec.fq.gz.gzi && \
   mv r10-hifiasm-correct.WORKING.ec.fq.gz.fai r10-hifiasm-correct.ec.fq.gz.fai && \
````

This include pre-processing various inputs into a format compatible with hifiasm. This takes < 300 GB and 6000 CPU h on our cluster. You can download the computed [corrected reads](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/4919700e-d13b-4031-891b-9da49b3919b8--HG002_test_benchmark/r10-hifiasm-correct.ec.fq.gz).

### Assembly with verkko
This requires verkko v2.3 or later. The corrected ONT UL data is input as high-quality sequences to verkko while the raw ONT UL is input as resolving ONT data:

````
  verkko --slurm -d verkko-hi-c \
    --ovb-run 8 32 32 \
    --screen-human-contaminants \
    --hifi $HIFI r10-hifiasm-correct.ec.fq.gz \
    --nano $ONT $ONT_R10 \
    --hic1 $HIC1 \
    --hic2 $HIC2 \
    --unitig-abundance 8  
````

The parameter `--unitig-abundance 8` is recommended for high-coverage datasets (the total coverage of ONT UL and HiFi here is 180x). This requires approximately <300 GB and 13K CPU h or 41 hrs walltime on our cluster. You can download the [complete assembly](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/4919700e-d13b-4031-891b-9da49b3919b8--HG002_test_benchmark/assembly.fasta.gz) or [haplotype1](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/4919700e-d13b-4031-891b-9da49b3919b8--HG002_test_benchmark/assembly.haplotype1.fasta.gz) and [haplotype2](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/4919700e-d13b-4031-891b-9da49b3919b8--HG002_test_benchmark/assembly.haplotype2.fasta.gz) separately.
</details>


## Polishing

The assembly QV (using yak on Illumina data) is estimated at 49.5. The best polishing strategy is being finalized, stay tuned for more details!

## Validation

GQC run assumes you've downloaded GQC and the v1.1 benchmark dataset from [here](https://github.com/marbl/GQC). 

<details>
<summary><h3>Commands</h3></summary>

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

Download the pre-computed [GQC output](https://s3-us-west-2.amazonaws.com/human-pangenomics/submissions/4919700e-d13b-4031-891b-9da49b3919b8--HG002_test_benchmark/gqc.tar.gz).
