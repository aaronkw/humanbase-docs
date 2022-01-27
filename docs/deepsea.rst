=======
Sei / DeepSEA
=======

Introduction
------------

Sei is a deep-learning-based framework for systematically predicting sequence regulatory activities and applying sequence information to understand human genetics data. Sei provides a global map from any sequence to regulatory activities, as represented by 40 sequence classes. Each sequence class integrates predictions for 21,907 chromatin profiles (transcription factor, histone marks, and chromatin accessibility profiles across a wide range of cell types) from the underlying Sei deep learning model. You can also find the Sei code repository `here <https://github.com/FunctionLab/sei-framework>`_ or read about our manuscript `here <https://www.biorxiv.org/content/10.1101/2021.07.29.454384v1>`_.

Sequence class-level variant effects are computed by comparing the predictions for the reference and the alternative alleles. A positive score indicates an increase in sequence class activity by the alternative allele and vice versa. Sequence class-level scores are computed by projecting the 21,907 chromatin profile predictions for the sequence to the unit vector that represents each sequence class.

For older DeepSEA models see:
:doc:`beluga` (2019)


Input
-----

File formats
~~~~~~~~~~~~
We support three types of input: vcf, fasta, bed. If you want to predict effects of noncoding variants, use vcf format input. If you want to predict chromatin feature probabilities for DNA sequences, use fasta format. If you want to specify sequences from the human reference genome (GRCh37/hg19), you can use bed format. See below for a quick introduction:

**VCF format** is used for specifying a genomic variant. A minimal example is ``chr1 109817590 - G T`` (if you want to copy cover this text as input, you will need to change spaces to tabs). The five columns are chromosome, position, name, reference allele, and alternative allele. Currently, the genome position needs to be in GRCh37/hg19

**Fasta format** input should include sequences of 4096bp length each. If a sequence is longer than 4096bp, only the center 4096bp will be used.

**Bed format** provides another way to specify sequences in human reference genome (hg19). The bed input should specify 4096bp-length regions. A minimal example is ``chr1 109817091 109821186``. The three columns are chromosome, start position, and end position.

Genome coordinates
~~~~~~~~~~~~~~~~~~
We support only ``GRCh37/hg19`` genome coordinates. You can use LiftOver to convert your coordinates to the correct version.

Large submissions
~~~~~~~~~~~~~~~~~
We recommend using the web server if you have <10,000 variants or sequences. You will experience degraded performance when submitting a larger set of sequences. In those instances, we suggest that you split the set into multiple <10,000 submissions, or run the standalone version on your local machine, or contact our group directly.


Output
------

Sequence classes
~~~~~~~~~~~~~~~~~~~~~~~~~

The Sei framework predicts 40 sequence class scores, covering a wide range of regulatory activities such as cell-type-specific enhancers and promoters, as well as 21,907 chromatin profiles for any DNA sequence.

To help interpretation, we grouped sequence classes into groups including P (Promoter), E (Enhancer), CTCF (CTCF-cohesin binding), TF (TF binding), PC (Polycomb-repressed), HET (Heterochromatin), TN (Transcription), and L (Low Signal) sequence classes. Please refer to our manuscript for a more detailed description of the sequence classes.

Note: sequence class predictions are only available for vcf inputs.

::

  | Sequence class label |               Sequence class name | Rank by size | Group |
  |---------------------:|----------------------------------:|-------------:|------:|
  |                 PC1  |       Polycomb / Heterochromatin  |            0 |   PC  |
  |                  L1  |                       Low signal  |            1 |    L  |
  |                 TN1  |                    Transcription  |            2 |   TN  |
  |                 TN2  |                    Transcription  |            3 |   TN  |
  |                  L2  |                       Low signal  |            4 |    L  |
  |                  E1  |                        Stem cell  |            5 |    E  |
  |                  E2  |                     Multi-tissue  |            6 |    E  |
  |                  E3  |               Brain / Melanocyte  |            7 |    E  |
  |                  L3  |                       Low signal  |            8 |    L  |
  |                  E4  |                     Multi-tissue  |            9 |    E  |
  |                 TF1  |                    NANOG / FOXA1  |           10 |   TF  |
  |                 HET1 |                  Heterochromatin  |           11 |  HET  |
  |                  E5  |                      B-cell-like  |           12 |    E  |
  |                  E6  |                  Weak epithelial  |           13 |    E  |
  |                 TF2  |                            CEBPB  |           14 |   TF  |
  |                 PC2  |                    Weak Polycomb  |           15 |   PC  |
  |                  E7  |            Monocyte / Macrophage  |           16 |    E  |
  |                  E8  |                Weak multi-tissue  |           17 |    E  |
  |                  L4  |                       Low signal  |           18 |    L  |
  |                 TF3  |                FOXA1 / AR / ESR1  |           19 |   TF  |
  |                 PC3  |                         Polycomb  |           20 |   PC  |
  |                 TN3  |                    Transcription  |           21 |   TN  |
  |                  L5  |                       Low signal  |           22 |    L  |
  |                 HET2 |                  Heterochromatin  |           23 |  HET  |
  |                  L6  |                       Low signal  |           24 |    L  |
  |                   P  |                         Promoter  |           25 |    P  |
  |                  E9  |                Liver / Intestine  |           26 |    E  |
  |                 CTCF |                     CTCF-Cohesin  |           27 |  CTCF |
  |                 TN4  |                    Transcription  |           28 |   TN  |
  |                 HET3 |                  Heterochromatin  |           29 |  HET  |
  |                 E10  |                            Brain  |           30 |    E  |
  |                 TF4  |                             OTX2  |           31 |   TF  |
  |                 HET4 |                  Heterochromatin  |           32 |  HET  |
  |                  L7  |                       Low signal  |           33 |    L  |
  |                 PC4  | Polycomb / Bivalent stem cell Enh |           34 |   PC  |
  |                 HET5 |                       Centromere  |           35 |  HET  |
  |                 E11  |                           T-cell  |           36 |    E  |
  |                 TF5  |                               AR  |           37 |   TF  |
  |                 E12  |                Erythroblast-like  |           38 |    E  |
  |                 HET6 |                       Centromere  |           39 |   HET |



Regulatory feature scores
~~~~~~~~~~~~~~~~~~~~~~~~~
* **diffs**: The difference between the the predicted probability of the reference allele and the alternative allele for a regulatory feature (:math:`p_{alt} -p_{ref}`).
