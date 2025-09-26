# Telomere-to-telomere consortium HG002 "Q100" project

The [Telomere-to-Telomere Consortium](https://sites.google.com/ucsc.edu/t2tworkinggroup), along with the [Human Pangenome Reference Consortium](https://humanpangenome.org) and the [Genome in a Bottle Consortium](https://www.nist.gov/programs-projects/genome-bottle), have sequenced, assembled and polished the [HG002](https://www.coriell.org/0/Sections/Search/Sample_Detail.aspx?Ref=GM24385&Product=CC) (also known as GM24385 and huAA53E0) cell line. The ultimate goal of this effort is to create a "genome benchmark" for the HG002 reference material that covers all bases of the diploid genome and is perfectly accurate. Hence, the "Q100" project nickname, which refers to a Phred quality score of 1 error per 10 billion bases.

Current benchmarks are typically defined as a list of variants called against a reference genome such as GRCh38. This is problematic in that the GRCh38 reference is incomplete, and there may be regions of the benchmark genome (e.g. HG002) that do not reliably align to the reference. A more natural and comprehensive benchmark representation is the complete sequence of the genome itself, i.e. a “genome benchmark” as opposed to a “variant benchmark”. Assembling the complete HG002 genome is our first step towards creating such a genome benchmark. We have developed software, [GQC](https://github.com/nhansen/GQC) ("Genome Quality Checker"), that evaluates assemblies and sequencing reads, outputing standardized metrics of completeness, continuity, and consensus accuracy.

The initial assembly used for this project was performed using HiFi data available from the HPRC, as well as ONT data available from the HPRC and GIAB. The [Verkko](https://github.com/marbl/verkko) assembler was used, followed by manual assignment of nodes to chromosomes, ONT-based patching to resolve HiFi coverage gaps, manual resolution of tangles, and Strand-Seq and Hi-C based scaffolding across the rDNA arrays. The v0.7 assembly release then underwent three rounds of extensive polishing, patching, and validation to produce the v1.1 release. Although the v1.1 assembly does contain gaps (scaffolded stretches of N's) for nine out of ten of the rDNA arrays, it is otherwise T2T ("telomere to telomere"), has nearly perfect haplotype phasing, and has an estimated consensus error rate of less than 1 per 10 million bases (merqury assigned QV of Q78). Work on the annotation, characterization, and correction of any remaining errors in the v1.1 assembly will continue as sequencing technology improves and any additional errors are identified. This will include the eventual inclusion and finishing of the rDNA arrays. If you identify any errors in the latest assembly, please raise an issue with all relevant evidence and information on the [associated issues repository](https://github.com/marbl/HG002-issues).

## Data reuse and license
All assembly data is released to the public domain ([CC0](https://creativecommons.org/publicdomain/zero/1.0/)) and we encourage its reuse. We would appreciate if you would acknowledge and cite the "Telomere-to-Telomere" (T2T) Consortium for the creation of this data. More information about our consortium can be found on the [T2T homepage](https://sites.google.com/ucsc.edu/t2tworkinggroup/) and a list of related citations is available below:

### Relevant citations:
- Hansen NF, et al. [A complete diploid human genome benchmark for personalized genomics](https://www.biorxiv.org/content/10.1101/2025.09.21.677443v1). bioRxiv, 2025.
- Rhie A, et al. [The complete sequence of a human Y chromosome](https://pubmed.ncbi.nlm.nih.gov/37612512/). Nature, 2023.
- Rautiainen M, et al. [Telomere-to-telomere assembly of diploid chromosomes with Verkko](https://pubmed.ncbi.nlm.nih.gov/36797493/). Nature Biotechnology, 2023.
- Wang T. [The Human Pangenome Project: a global resource to map genomic diversity](https://pubmed.ncbi.nlm.nih.gov/35444317/). Nature, 2022.
- Nurk, et al. [The complete sequence of a human genome](https://pubmed.ncbi.nlm.nih.gov/35357919/). Science, 2022.
- Zook J. et al. [A robust benchmark for detection of germline large deletions and insertions](https://pubmed.ncbi.nlm.nih.gov/32541955/). Nature Biotechnology, 2022.
 
# Assembly releases
### v1.1 (July, 2024)

[hg002v1.1.fasta.gz](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/hg002v1.1.fasta.gz) : T2T reconstruction of all 46 chromosomes of HG002 with fixes and improvements. This assembly was created by applying patches contained in the VCF file [hg002v1.0.1_to_hg002v1.1.vcf.gz](https://github.com/marbl/HG002-issues/blob/main/patches/already_applied/hg002v1.0.1_to_hg002v1.1.vcf.gz) to 4,013 errors in the v1.0.1 assembly. This v1.1 assembly's Merqury-estimated QV using a hybrid database as described in McCartney et al., [Chasing perfection: validation and polishing strategies for telomere-to-telomere genome assemblies](https://pubmed.ncbi.nlm.nih.gov/35361931/). Nat Methods, 2022, is 77.8, compared to an estimate QV of 75.1 for the v1.0.1 assembly.

GenBank accessions for v1.1 are: [GCA_018852605.3](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_018852605.3/) (paternal) and [GCA_018852615.3](https://www.ncbi.nlm.nih.gov/datasets/genome/GCA_018852615.3/) (maternal).

In addition to the v1.1 assembly, rDNA "morph" consensus sequences created by ribotin ([Rautiainen, Bioinformatics 2024](https://doi.org/10.1093/bioinformatics/btae124)) have been made available on [AWS](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies/annotation/rdna/hg002_rdnamorphs_v0.1/).

This genome is viewable in the [UCSC browser](https://genome.ucsc.edu/cgi-bin/hgHubConnect?hgHub_do_redirect=on&genome=HG002v1.1.PAT&hubUrl=https://research.nhgri.nih.gov/CustomTracks/T2T_hubs/T2Tgenomes/hub.txt).

### v1.0.1 (October, 2023)
N.B.: The original release of v1.0 fasta files contained ambiguity codes at three positions in the assembly, and these three bases were replaced with A/T/G/C bases in the v1.0.1 fasta files. There was no coordinate change in this replacement, and chain files, BAMs, VCFs created with v1.0 should be valid with the v1.0.1 reference fasta files.

[hg002v1.0.1.fasta.gz](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/hg002v1.0.1.fasta.gz) : T2T reconstruction of all 46 chromosomes of HG002 with fixes and improvements to the centromeres, telomeres, and rDNA junctions. This assembly was created by applying patches contained in the VCF file [hg002v0.9_to_hg002v1.0.vcf.gz](https://github.com/marbl/HG002-issues/blob/main/patches/already_applied/hg002v0.9_to_hg002v1.0.vcf.gz) to the v0.9 assembly. In addition to correcting 4,482 small assembly errors found using aligned high-accuracy Element and Onso short read data, 19 corrections were made to the centromere consensus on seven chromosomes, small corrections were made on the telomeres, and the consensus sequence at the edges of the rDNA arrays was improved and extended inward. Corrections were also made to the rDNA array on chr13_PATERNAL, which has no gap due to its smaller size. Newer ultra-long ONT reads led to a correction of the number of rDNA units in this unit from two to six. The v1.0 assembly has a Merqury-estimated QV of 75.1 (75.5 on the maternal chromosomes, 74.7 on the paternal chromosomes) using a hybrid database as described in McCartney et al., [Chasing perfection: validation and polishing strategies for telomere-to-telomere genome assemblies](https://pubmed.ncbi.nlm.nih.gov/35361931/). Nat Methods, 2022.

### v0.9 (July, 2023)
[hg002v0.9.fasta.gz](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/hg002v0.9.fasta.gz) : T2T reconstruction of all 46 chromosomes of HG002 with initial round of polishing and patching. This assembly was created by applying patches contained in the VCF file [hg002v0.7_to_hg002v0.9.vcf.gz](https://github.com/marbl/HG002-issues/blob/main/patches/already_applied/hg002v0.7_to_hg002v0.9.vcf.gz) to the v0.7 assembly. Proposed errors in the v0.7 assembly discovered by various methods were validated by a team of curators and applied if the majority of them were found to correct actual errors. In addition, larger "patches" were applied to problematic regions (e.g., regions with low consensus accuracy due to low read coverage or regions where there were false duplications or collapsed repeats). These patches were created by examining the corresponding region in a newer assembly (v0.8, see below), and replacing v0.7 with v0.8 if the v0.8 region was more consistent with read data. The v0.9 assembly has a Merqury-estimated QV of 71.8 (72.1 on the maternal chromosomes, 71.5 on the paternal chromosomes) using a hybrid database of k-mers found in HiFi and Illumina reads.

### v0.8 (July, 2023)
[hg002v0.8.fasta.gz](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/hg002v0.8.fasta.gz) : Verkko assembly of HG002 using improved HiFi and ONT data. This release consists of a Verkko assembly run on three cells of HiFi Revio sequencing reads and ONT R10 ultralong data. While the assembly is not T2T, its scaffolds have been labeled with their human chromosome names (in the form "chr#_MAT", "chr#_PAT", etc.) and its consensus quality was found to be more accurate than the v0.7 assembly was prior to polishing. For this reason, it was used as a source of consensus patches in regions of v0.7 that showed evidence of larger errors when creating version v0.9.

### v0.7 (November, 2022)
[hg002v0.7.fasta.gz](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/scratch/HG002/assemblies/drafts/assembly.v0.7.fasta.gz) : T2T reconstruction of all 46 chromosomes of HG002 along with mitochondria and EBV sequences. Maternal and paternal haplotypes are denoted by contig name. Unassigned contigs are from unresolved rDNA arrays. The pipeline used two versions of verkko combined with manual curation and resolution. Assemblies using both versions are available below along with the final traversals used for each chromosome and haplotype. Negative gaps indicate there was an overlap between the verkko contigs, supported by ONT-only assembly and sequences. Note that additional files, including ONT coverage and trio coloring, are available for each assembly. You can browse these from the top-level assembly [folder](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/scratch/HG002/assemblies/graphs/). Consensus quality exceeds Q60. Centers of 9/10 rDNA arrays are represented by N-gaps.
 
# Software tools
- [GQC](https://github.com/nhansen/GQC) software for benchmarking genome assemblies and read datasets using the HG002v1.1 assembly
- [hg002-q100-annotation](https://doi.org/10.5281/zenodo.16880402) software used to do the LiftOff-based gene annotation of the HG002v1.1 assembly

# Downloads
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies/">Assembly FASTA files</a>
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies/polishing/HG002/v1.0/mapping/">Alignment files of read data aligned to the V1.0 assembly (BAM format)</a>
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/scratch/HG002/assemblies/graphs/20220725/assembly.homopolymer-compressed.noseq.gfa">Assembly graph from Verkko v1.1 in GFA format, no sequences included (md5: a7473ba67b1109089217ee4b7fdf647a )
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/scratch/HG002/assemblies/graphs/20220812/assembly.homopolymer-compressed.noseq.gfa">Assembly graph from Verkko v1.2/MBG v1.0.13, no sequences included (md5: 86f896e5232a5aee95b47f32fbddd114)
- <a href="https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/scratch/HG002/assemblies/drafts/v0.7/assembly.v0.7.chromosome_paths.gaf">Genomic paths through both graphs used for consensus. ChrX and chr12 maternal were previously finished and are excluded </a> (md5: bf07da577141d4240f54b18091f0b35d)
 
## Sequencing data
Locations of the sequencing dataset used for this project can be found [here](Sequencing_data.md).

# Contact
 
Please raise issues on this Github repository concerning access to this dataset. For any errors identified in the assembly itself, please raise issues on the associated [HG002-issues repository](https://github.com/marbl/HG002-issues).
 
# History
 
    * v1.1: 22nd July 2024. V1.1 release.
    * v1.0: 31st October 2023. V1.0 release.
    * v0.7: 7th November 2022. Initial release.
