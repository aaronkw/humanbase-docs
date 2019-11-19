=======
DeepSEA
=======

Introduction
------------

DeepSEA is a deep learning-based algorithmic framework for predicting the chromatin effects of sequence alterations with single nucleotide sensitivity. DeepSEA can accurately predict the epigenetic state of a sequence, including transcription factors binding, DNase I sensitivities and histone marks in multiple cell types, and further utilize this capability to predict the chromatin effects of sequence variants and prioritize regulatory variants.

DeepSEA is described in the following manuscript:

Jian Zhou, Olga G. Troyanskaya. Predicting the Effects of Noncoding Variants with Deep learning-based Sequence Model. Nature Methods (2015).

Input
------------

DeepSEA predicts genomic variant effects on a wide range of chromatin features at the variant position (Transcription factors binding, DNase I hypersensitive sites, and histone marks in multiple hunman cell types). DeepSEA can also be ultilized for predicting chromatin features for any DNA sequence.

We support three types of input: vcf, fasta, bed. If you want to predict effects of noncoding variants, use vcf format input. If you want to predict chromatin feature probabilities for DNA sequences, use fasta format. If you want to specify sequences from the human reference genome (GRCh37/hg19), you can use bed format. See below for a quick introduction, and we provide detailed description of format requirement on the DeepSEA input page:

VCF format is used for specifying a genomic variant. A minimal example is chr1 109817590 - G T (if you want to copy cover this text as input, you need to change spaces to tabs since html webpage can not display tab). The five columns are chromosome, position, name, reference allele, and alternative allele.

Fasta format input should include sequences of 1000bp length each. If a sequence is longer than 1000bp, only the center 1000bp will be used. A minimal example is ::

  >TestSequence
  TATCTCTCATGTTTCTGGTATAGATGGTATATATGTTAATCTTGTTCCTGAGGTCTGTTTTTTATTTTTGTCATTAAAGT
  GGGAATTAAATAGTTTTGTAGTGCATATAAATTAAAGAAAAAGTTCACATAAGCATATTTGCCAATCATCTCAAAATGCT
  ATATTCTCCTTCACGGTTTTGAAAATAATTCAGGGTTTTCTCTTCCTCATTGCTTTCCCACCAACTGACAGTATTATTTT
  CTTAGTCATTTTACTGACCTTTGAAATTACTCCTTTGAGGTCTTCTAAAAAATTTTATGGGCTCTGCTGCTTTTTGGTGG
  CCTCCTTGTATCATTTATTCTATTACAGGACGACTTACAAAAGGAAGCACATAAATTGACCCATATACATATCCTATCAT
  TGGGGAGTTTCTGTGCAAATGTTATTTATTGGAAGCTATTACTAAGAATTGTAAGAAAAATAATTGGTATTGATGCAGCT
  AGTATGGTTCCTGTAATTATCGTACTCAGCCACGTAAATCATAGCTATATGTAGCCAAAGATCCATGAACAAAATTTCCA
  GTAACATCATTATAATTCAAAAGGCAGACTTTCAGAACCAGACAGACTTGAATTTAAATTCTAGCTTTACCACACATGAA
  TTTAACCTTGTGGAAGGTTAACCTATCTAAACTCATGTTTCTTCATTGGTAGCTGATAAAATTAAGGATCATGTATATAA
  CCACCTAGTAGAGTTGTTTAAGAAACTGTTAGAATTCCATAAATTGTTAGTATTAATGAGTTTTTGTTGGACATGTGTTA
  GGCTAGGCCACTCCTTGACCTTCATAGAGGTATGGATTATGACACAAATTCTAAACTGTAGGTAGGCATGGCTTTGTAGC
  AAGTATTAAAATAGTAAATATTTTATTTTTATAAGATAAATGTAAACCTTTTAAAAGTTTCATTACATTTGTATTTATGA
  AATATCATCCTATATCAACTATAGAGAGAAGATCGCAAGA


Bed format provides another way to specify sequences in human reference genome (hg19). The bed input should specify 1000bp-length regions. A minimal example is chr1 109817091 109818090. The three columns are chromosome, start position, and end position.
We recommend using the server if you have <50,000 variants or sequences. For larger set, you may run the standalone version on your local machine.

We support only GRCh37/hg19 genome coordinates. You can use LiftOver to convert your coordinates to the correct version.


In-silico mutagenesis
------------
Perofrm "In silico saturated mutagenesis" analysis to discover informative sequence features within any sequence. Specifically, it performs computational mutation scanning to assess effect of mutating every base of the input sequence on chromatin feature predictions. This method for context-specific sequence feature extraction fully utilizes the DeepSEA’s capability of utilizing flanking context sequences information.

We support three types of input for specifying a sequence to analyze: vcf, fasta, bed. See the Input section for a brief introduction to the formats. Note that sequence profiler only accepts one sequence / region / variant as input. If a variant is given, we perform analysis on the sequence carrying the alternative allele (if you want to analyze the reference allele, just use bed or fasta format input instead).

You also need to specify the chromatin feature that you want to analyze (e.g. CEBPB in H1-hESC cell with no treatment). If you have no idea which chromatin feature to look at, you may provide your input to DeepSEA and check which chromatin features are predicted to be on for your sequence.

ISM outputs effects for each of three possible substitutions of all 1000 bases on the chromatin feature you specified. 


