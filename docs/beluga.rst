=======
DeepSEA (Beluga)
=======

Introduction
------------

DeepSEA is a deep learning-based algorithmic framework for predicting the chromatin effects of sequence alterations with single nucleotide sensitivity. DeepSEA can accurately predict the epigenetic state of a sequence, including transcription factors binding, DNase I sensitivities and histone marks in multiple cell types, and further utilize this capability to predict the chromatin effects of sequence variants and prioritize regulatory variants.

The 2019 version of DeepSEA, nicknamed '**Beluga**', can predict **2002** chromatin features. Beluga is described in:

Jian Zhou, Chandra L. Theesfeld, Kevin Yao, Kathleen M. Chen, Aaron K. Wong, and Olga G. Troyanskaya, **Deep learning sequence-based ab initio prediction of variant effects on expression and disease risk**. Nature Genetics (2018).

To determine if certain features (ie. transcription factors, marks, or cell types) are present/accounted for in the model, refer to the `supplemental feature table <https://s3-us-west-2.amazonaws.com/humanbase-dev/deepsea/examples/41588_2019_420_MOESM9_ESM.csv>`_ which has all the profiles used to train DeepSEA.

DeepSEA is originally described in the following manuscript:

Jian Zhou, Olga G. Troyanskaya. **Predicting the Effects of Noncoding Variants with Deep learning-based Sequence Model.** Nature Methods (2015).

To determine if certain features (ie. transcription factors, marks, or cell types) are present/accounted for in the model, refer to the `supplemental feature table <https://s3-us-west-2.amazonaws.com/humanbase-dev/deepsea/examples/41588_2019_420_MOESM9_ESM.csv>`_ which has all the profiles used to train DeepSEA.


Input
-----

DeepSEA predicts genomic variant effects on a wide range of chromatin features at the variant position (Transcription factors binding, DNase I hypersensitive sites, and histone marks in multiple human cell types). DeepSEA can also be utilized for predicting chromatin features for any DNA sequence.

File formats
~~~~~~~~~~~~
We support three types of input: vcf, fasta, bed. If you want to predict effects of noncoding variants, use vcf format input. If you want to predict chromatin feature probabilities for DNA sequences, use fasta format. If you want to specify sequences from the human reference genome (GRCh37/hg19), you can use bed format. See below for a quick introduction:

**VCF format** is used for specifying a genomic variant. A minimal example is ``chr1 109817590 - G T`` (if you want to copy cover this text as input, you will need to change spaces to tabs). The five columns are chromosome, position, name, reference allele, and alternative allele.

**Fasta format** input should include sequences of 2000bp length each. If a sequence is longer than 2000bp, only the center 2000bp will be used. A minimal example is ::

  >TestSequence
  TGGGATTACAGGCGTGAGCCACCGCGCCCGGCCCATTGTACCATTCTTAT
  GCCTTTGCGTCCTCATAGCTTAGCTCCCGTATATCAGTGAGAACATACTA
  TGTTTGGTTTTCCATACCCGAGTTACTTCACTTAGAATAATAGTCTCCAA
  TTTCATCCAGGTCAGTGCAAATGCGTTAATTCGTTCCTTTTATGGCTGAG
  TAGTATTCCATCATATATATATACTACAGTTTCTTTATCCACTCGTAAAT
  TGATGGGCATTTGTGTTGGAACACTTCTCCACTGCTGGTGGGAATGTAAA
  TTAGTGCAGCCACTATGGATAACAGTGTGGAGATTTGTTAAAGAACTAAA
  ACTAGAACTACCATTTGATCCAGCAATCCCACTACTGGGTATCTACCCAG
  AAGAAAAGAAGTCATTATTTGAAAAAGATACTTGCACGGGCATGTTTATA
  GCAGCACAATTCACAATTGTAGTTGTATTTCTTTAAGCGTGTCTTTTCAA
  TATCTCTCATGTTTCTGGTATAGATGGTATATATGTTAATCTTGTTCCTG
  AGGTCTGTTTTTTATTTTTGTCATTAAAGTGGGAATTAAATAGTTTTGTA
  GTGCATATAAATTAAAGAAAAAGTTCACATAAGCATATTTGCCAATCATC
  TCAAAATGCTATATTCTCCTTCACGGTTTTGAAAATAATTCAGGGTTTTC
  TCTTCCTCATTGCTTTCCCACCAACTGACAGTATTATTTTCTTAGTCATT
  TTACTGACCTTTGAAATTACTCCTTTGAGGTCTTCTAAAAAATTTTATGG
  GCTCTGCTGCTTTTTGGTGGCCTCCTTGTATCATTTATTCTATTACAGGA
  CGACTTACAAAAGGAAGCACATAAATTGACCCATATACATATCCTATCAT
  TGGGGAGTTTCTGTGCAAATGTTATTTATTGGAAGCTATTACTAAGAATT
  GTAAGAAAAATAATTGGTATTGATGCAGCTAGTATGGTTCCTGTAATTAT
  CGTACTCAGCCACGTAAATCATAGCTATATGTAGCCAAAGATCCATGAAC
  AAAATTTCCAGTAACATCATTATAATTCAAAAGGCAGACTTTCAGAACCA
  GACAGACTTGAATTTAAATTCTAGCTTTACCACACATGAATTTAACCTTG
  TGGAAGGTTAACCTATCTAAACTCATGTTTCTTCATTGGTAGCTGATAAA
  ATTAAGGATCATGTATATAACCACCTAGTAGAGTTGTTTAAGAAACTGTT
  AGAATTCCATAAATTGTTAGTATTAATGAGTTTTTGTTGGACATGTGTTA
  GGCTAGGCCACTCCTTGACCTTCATAGAGGTATGGATTATGACACAAATT
  CTAAACTGTAGGTAGGCATGGCTTTGTAGCAAGTATTAAAATAGTAAATA
  TTTTATTTTTATAAGATAAATGTAAACCTTTTAAAAGTTTCATTACATTT
  GTATTTATGAAATATCATCCTATATCAACTATAGAGAGAAGATCGCAAGA
  AGGCAGTGGCAGCAGAGGCTCCAGTTAGGAGGCTACTAGTCCAAATACAT
  TGCGATAAAAACTTGGCAAAAGGTGCTGGTAGTCTGATGAAATAAAGTAG
  ATAAATTTTAGAGGTATTTATAAAATAATTAAAGAATATTCAATAATAGG
  AGATATATTACCCAATAGAGTGGAGATTCAAAGATAACTCCGAAAGTTTT
  TTGCTAAAGCAACATTTGGCTGTGCTATCATTTACTAAGAAAGACAACAA
  GAGAGTAAAATCAAGTTTGAGGATGAAGTGAATTTATTCCTTTTTGATTG
  ATACATAATTGACATGTAATAAAACCCACAATGTTAAGAGTTCGGTTTGA
  TGTGCTTGACTATTTTAGGCACTGGTGTTATCACAACACAAGACAACAGA
  TAGGACATTCTCAGAAAATTTTTTCATGTCCCTTTCCAGTCAGTTTCAAG
  CCTTCTTTCCATGCAATAATTTTCTCACTTTGCCATTCTAGTAGGTGTGA

**Bed format** provides another way to specify sequences in human reference genome (hg19). The bed input should specify 2000bp-length regions. A minimal example is ``chr1 109817091 109819090``. The three columns are chromosome, start position, and end position.

Genome coordinates
~~~~~~~~~~~~~~~~~~
We support only ``GRCh37/hg19`` genome coordinates. You can use LiftOver to convert your coordinates to the correct version.

Large submissions
~~~~~~~~~~~~~~~~~
We recommend using the web server if you have <10,000 variants or sequences. You will experience degraded performance when submitting a larger set of sequences. In those instances, we suggest that you split the set into multiple <10,000 submissions, or run the standalone version on your local machine, or contact our group directly.

Output
------

Regulatory feature scores
~~~~~~~~~~~~~~~~~~~~~~~~~
* **diffs**: The difference between the the predicted probability of the reference allele and the alternative allele for a regulatory feature (:math:`p_{alt} -p_{ref}`).
* **e-value**: E-value is defined as the expected proportion of SNPs with a larger predicted effect. We calculate an 'e-value' based on the empirical distribution of that feature's effect (:math:`abs(p_{alt} -p_{ref})`) among gnomAD variants. For example, a feature e-value of 0.01 indicates that only 1% of gnomAD variants have a larger predicted effect.
* **z-score**: A scaled score where the feature diff score (:math:`p_{alt} -p_{ref}`) is divided by the root mean square of the feature diff score across gnomAD variants. Note that this is "sign-preserving", i.e. a negative z-score indicates that a mutation **decreases** the probability of a regulatory feature.

Variant scores
~~~~~~~~~~~~~~

* **Disease Impact Score (DIS)**: DIS is calculated by training a logistic regression model that prioritizes likely disease-associated mutations on the basis of the predicted transcriptional or post-transcriptional regulatory effects of these mutations (See Zhou et. al, 2019). The predicted DIS probabilities are then converted into 'DIS e-values', computed based on the empirical distributions of predicted effects for gnomAD variants. The final DIS score is:

  .. math::
      -log10(DIS evalue_{feature})

* **Mean -log e-value (MLE)**: For each predicted regulatory feature effect (:math:`abs(p_{alt}-p_{ref}`)) of a variant, we calculate a 'feature e-value' based on the empirical distribution of that feature's effects among gnomAD variants (see above Regulatory feature scores: e-value). The MLE score of a variant is

  .. math::
      \sum -log10(evalue_{feature}) / N

In-silico mutagenesis
---------------------
Perform "In silico saturated mutagenesis" (ISM) analysis to discover informative sequence features within any sequence. Specifically, it performs computational mutation scanning to assess the effect of mutating every base of the input sequence on chromatin feature predictions. This method for context-specific sequence feature extraction takes advantage of DeepSEA’s ability to utilize flanking context sequences information.

Note that ISM only accepts a sequence (FASTA file) as input.

ISM outputs effects for each of three possible substitutions of all 2000 bases, across all chromatin features.
