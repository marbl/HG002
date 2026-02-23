# HG002 Browser Track Data Files
 
##The T2T genome assembly hub
The two haplotypes of the HG002v1.1 assembly are viewable in a UCSC assembly hub hosted by NHGRI within its [T2T-genomes](https://genome.ucsc.edu/cgi-bin/hgHubConnect?hgHub_do_redirect=on&hgHubConnect.remakeTrackHub=on&hgHub_do_firstDb=1&hubUrl=https://research.nhgri.nih.gov/CustomTracks/T2T_hubs/T2Tgenomes/hub.txt) browser. The configuration files for the T2T Browser are stored on github [here](https://github.com/marbl/T2T-Browser). The larger data files that are sources for the data tracks in the HG002 browser are stored in the human-pangenomics AWS bucket. Their locations are listed below.

|Track name |Browser URL |Data URL |Description |
|:----------|:-----------|:--------|:-----------|
|GC Percent in 5-Base Windows    |[Browser](https://genome.ucsc.edu/cgi-bin/hgTrackUi?hgsid=3485989125_hZOvau9sKWiQnq0zFtprSaarVzoC&db=hub_4837794_HG002v1.1.PAT&c=chr1_PATERNAL&g=hub_4837794_HG002v1_1_PAT_gc5Base)|[Data](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies/annotation/seqfeatures/)||
|CpG Islands    |[Browser](https://genome.ucsc.edu/cgi-bin/hgTrackUi?hgsid=3485989125_hZOvau9sKWiQnq0zFtprSaarVzoC&g=hub_4837794_HG002v1_1_PAT_cpgIslands&hgTracksConfigPage=configure)|[Data](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies/annotation/seqfeatures/)||
|Microsatellite Repeats    |[Browser]()|[Data](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies/annotation/seqfeatures/)||
|v1.1 issues v4    |[Browser](https://genome.ucsc.edu/cgi-bin/hgTrackUi?hgsid=3723526455_SkoErASIw1Ivhp7VHU6iVBJ8Ar0Z&db=hub_4837794_HG002v1.1.MAT&c=chr15_MATERNAL&g=hub_4837794_HG002v1_1_MAT_v1_1_issues)|[Data](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies/annotation/assemblies/issues/hg002v1.1_issues.v4.bb)||
|v1.1 issues v4    |[Browser]()|[Data](https://s3-us-west-2.amazonaws.com/human-pangenomics/index.html?prefix=T2T/HG002/assemblies/annotation/assemblies/issues/hg002v1.1_issues.v4.bb)||
|v1.1 error kmers    |[Browser]()|[Data](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/qc/merqury/v1.1/hg002v1.1_k31_sprq_elmt_hybrid.error.bb)||
|HG002v1.1-HiFi-Revio-nucfreq |[Browser]()|[Data A](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/qc/nucfreq/hg002v1.1_hifi_revio_pbmay24.pri.first.bw), [Data B](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/qc/nucfreq/hg002v1.1_hifi_revio_pbmay24.pri.second.bw)||
|HG002v1.1-ONT-UL-nucfreq |[Browser]()|[Data A](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/qc/nucfreq/hg002v1.1_ont_epi2me_q28.pri.first.bw), [Data B](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/qc/nucfreq/hg002v1.1_ont_epi2me_q28.pri.second.bw)||
|Mononucleotide Runs |[Browser]()|[Data](https://s3-us-west-2.amazonaws.com/human-pangenomics/T2T/HG002/assemblies/annotation/seqfeatures/v1.1.mnr10.bb)||
