# Telomere-to-telomere consortium HG002 "Q100" project
 
In collaboration with the [Human Pangenome Reference Consortium](https://humanpangenome.org) and the [Genome in a Bottle Consortium](https://www.nist.gov/programs-projects/genome-bottle) we have sequenced, assembled and polished the [HG002](https://www.coriell.org/0/Sections/Search/Sample_Detail.aspx?Ref=GM24385) (also known as GM24385 and huAA53E0) cell line. 
The ultimate goal of this effort is to create a reference assembly for the HG002 reference material which is perfectly accurate. This is the meaning of the "Q100" project nickname, which refers to a Phred quality score of 1 error per 10 billion bases.

The initial assembly used for this project was performed using HiFi data available from the HPRC, as well as ONT data available from the HPRC and GIAB. The [verkko](https://github.com/marbl/verkko) assembler was used, followed by manual assignment of nodes to chromosomes, ONT-based patching to resolve HiFi coverage gaps, manual resolution of tangles, and Strand-Seq and Hi-C based assignment of acrocentric short arms to chromosomes.

The first released assembly (release "v0.7") then underwent two rounds of extensive polishing, patching, and validation to produce our latest assembly release, v1.0. Although this latest assembly release does contain gaps (scaffolded stretches of N's) for nine out of ten of the rDNA arrays, it is otherwise T2T ("telomere to telomere"), and is estimated to have very high consensus quality (exceeding phred-scaled error rate Q70) and near perfect haplotype phasing. Work on the annotation and characterization of any remaining errors in the v1.0 assembly is underway.
 
## Data reuse and license
All assembly data is released to the public domain ([CC0](https://creativecommons.org/publicdomain/zero/1.0/)) and we encourage its reuse. We would appreciate if you would acknowledge and cite the "Telomere-to-Telomere" (T2T) Consortium for the creation of this data. More information about our consortium can be found on the [T2T homepage](https://sites.google.com/ucsc.edu/t2tworkinggroup/) and a list of related citations is available below:
 
### Relevant citations:
1. Rautiainen M, et al. [Verkko: telomere-to-telomere assembly of diploid chromosomes](https://doi.org/10.1101/2022.06.24.497523). BioRxiv, 2022.
2. Wang T. [The Human Pangenome Project: a global resource to map genomic diversity](https://www.nature.com/articles/s41586-022-04601-8). Nature, 2022.
3. Zook J. et al. [A robust benchmark for detection of germline large deletions and insertions](https://doi.org/10.1038/s41587-020-0538-8). Nature Biotech, 2022.
 
# Assembly releases
### v1.0 (October, 2023)
<a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/hg002v1.0.fasta">T2T reconstruction of all 46 chromosomes of HG002 with fixes and improvements to the centromeres, telomeres, and rDNA arrays</a>. This assembly was created by applying patches to the v0.9 assembly (see below) contained in the VCF file [hg002v0.9_to_hg002v1.0.vcf.gz](https://github.com/marbl/HG002-issues/blob/main/patches/already_applied/hg002v0.9_to_hg002v1.0.vcf.gz). In addition to correcting 4,482 small errors found using high-accuracy Element and Onso short read data, 19 corrections were made to the centromeres of seven chromosomes, small corrections were made to the telomeres, and the consensus sequence at the edges of the rDNA arrays was improved and extended inward. Corrections were also made to the rDNA array on chr13_PATERNAL, which has no gap due to its smaller size. Newer ultra-long ONT reads led to a correction of the number of rDNA units in this unit from two to six. The v1.0 assembly has a merqury-estimated QV of 75.1 (75.5 on the maternal chromosomes, 74.7 on the paternal chromosomes) using a hybrid database as described in McCartney et al., [Chasing perfection: validation and polishing strategies for telomere-to-telomere genome assemblies](https://doi.org/10.1038/s41592-022-01440-3). Nat Methods, 2022.

### v0.9 (July, 2023)
<a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/hg002v0.9.fasta">T2T reconstruction of all 46 chromosomes of HG002 with initial round of polishing and patching</a>. This assembly was created by applying patches to the v0.7 assembly contained in the [hg002v0.7_to_hg002v0.9.vcf.gz VCF file available from our HG002-issues github repository](https://github.com/marbl/HG002-issues/blob/main/patches/already_applied/hg002v0.7_to_hg002v0.9.vcf.gz). Proposed errors in the v0.7 assembly of various types were validated by a team of curators and applied if they were found to correct actual errors. In addition, larger "patches" were made to problematic regions (e.g., regions with low consensus accuracy due to low read coverage or regions where there were false duplications or collapsed repeats). These patches were created by examining the corresponding region in a newer assembly (v0.8, see below), and replacing v0.7 with v0.8 if the v0.8 region was more consistent with read data. The v0.9 assembly has a merqury-estimated QV of 71.8 (72.1 on the maternal chromosomes, 71.5 on the paternal chromosomes) using a hybrid database of k-mers found in HiFi and Illumina reads.

### v0.8 (July, 2023)
<a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/hg002v0.8.fasta">Verkko assembly of HG002 using improved HiFi and ONT data</a>. This release consists of a Verkko assembly run on three cells of HiFi Revio sequencing reads and ONT R10 ultralong data. While the assembly is not T2T, its scaffolds have been labeled with their human chromosome names (in the form "chr#_MAT", "chr#_PAT", etc.) and its consensus quality was found to be more accurate than the v0.7 assembly was prior to polishing. For this reason, it was used as a source of consensus patches in regions of v0.7 that showed evidence of larger errors when creating version v0.9.

### v0.7 (November, 2022)
<a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/scratch/HG002/assemblies/drafts/assembly.v0.7.fasta">T2T reconstruction of all 46 chromosomes of HG002</a> along with mitochondria and EBV sequences. Maternal and paternal haplotypes are denoted by contig name. Unassigned contigs are from unresolved rDNA arrays. The pipeline used two versions of verkko combined with manual curation and resolution. Assemblies using both versions are available below along with the final traversals used for each chromosome and haplotype. Negative gaps indicate there was an overlap between the verkko contigs, supported by ONT-only assembly and sequences. Note that additional files, including ONT coverage and trio coloring, are available for each assembly. You can browse these from the top-level assembly [folder](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/scratch/HG002/assemblies/graphs/).
 
Consensus quality exceeds Q60. Centers of 9/10 rDNA arrays are represented by N-gaps.
 
## Downloads
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/">Assembly FASTA files</a>
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies/polishing/HG002/v1.0/mapping/">Alignment files (BAM-formatted) of read data aligned to the V1.0 assembly</a>
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/scratch/HG002/assemblies/graphs/20220725/assembly.homopolymer-compressed.noseq.gfa">Assembly graph from Verkko v1.1 in GFA format, no sequences included (md5: a7473ba67b1109089217ee4b7fdf647a )
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/scratch/HG002/assemblies/graphs/20220812/assembly.homopolymer-compressed.noseq.gfa">Assembly graph from Verkko v1.2/MBG v1.0.13, no sequences included (md5: 86f896e5232a5aee95b47f32fbddd114)
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/scratch/HG002/assemblies/drafts/v0.7/assembly.v0.7.chromosome_paths.gaf">Genomic paths through both graphs used for consensus. ChrX and chr12 maternal were previously finished and are excluded </a> (md5: bf07da577141d4240f54b18091f0b35d)
 
# Sequencing Data
 
## HiFi Data
Sequenced used for the assembly were generated by HPRC and GIAB and are available from [AWS](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/scratch/HG002/sequencing/hifi/).
 
## Oxford Nanopore Data
Nanopore sequencing was performed by HPRC and GIAB. The fastq data used for the assembly are available from [AWS](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/scratch/HG002/sequencing/ont/). The raw fast5 files are available from the original data sources at [HPRC](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=NHGRI_UCSC_panel/HG002/nanopore/), [GIAB](https://ftp-trace.ncbi.nlm.nih.gov/ReferenceSamples/giab/data/AshkenazimTrio/HG002_NA24385_son/Ultralong_OxfordNanopore/), and [ONT](https://labs.epi2me.io/dataindex/) (Sept and Nov 2020 releases).
 
## StrandSeq
Strandseq data is available from the [HPRC](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=working/HPRC_PLUS/HG002/raw_data/Strand_seq/)
 
## HiC
Hi-C data is available from the [HPRC](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=working/HPRC_PLUS/HG002/raw_data/hic/downsampled/). The assembly used HG002.HiC_2_NovaSeq_rep1_run2_S1_L001_R1_001.fastq.gz and HG002.HiC_2_NovaSeq_rep1_run2_S1_L001_R2_001.fastq.gz.
 
# Notes on downloading files.
 
Files are generously hosted by Amazon Web Services. Although available as straight-forward HTTP links, download performance is improved by using the Amazon Web Services <a href="https://aws.amazon.com/cli/">command-line interface</a>. References should be amended to use the `s3://` addressing scheme, i.e. replace `https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/` with `s3://human-pangenomics/T2T` to download.
 
The s3 command can also be used to get information on the dataset, for example reporting the size of every file in human-readable format.
 
    aws s3 --no-sign-request ls --recursive --human-readable --summarize s3://human-pangenomics/T2T/scratch/HG002/
 
or to obtain technology-specific sizes.
 
    aws s3 --no-sign-request ls --recursive --human-readable --summarize s3://human-pangenomics/T2T/scratch/HG002/sequencing
    aws s3 --no-sign-request ls --recursive --human-readable --summarize s3://human-pangenomics/T2T/scratch/HG002/assemblies
 
Amending the `max_concurrent_requests` etc. settings as per <a href="https://docs.aws.amazon.com/cli/latest/topic/s3-config.html">this guide</a> will improve download performance further.
 
You can also browse all the files available on S3 via <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies">web interface</a>.
 
# Contact
 
Please raise issues on this Github repository concerning this dataset.
 
# History
 
    * v1.0: 31st October 2022. V1.0 release.
    * v0.7: 7th November 2022. Initial release.
