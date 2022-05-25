
=======
CLEVER
=======

Introduction
------------
CLEVER is a framework for ab initio sequence-based prediction of mutation gene expression effects for primary human cell types. With this web interface, we provide an explorer of cell type-specific expression effect predictions. The current release contains all ClinVar variants within 20kb to the representative TSS of a gene (normalized to 1000 Genomes variant effects)KSENIA: no effect threshold was employed -correct?.

The code for predicting expression effects for human genome variants and training new expression models is available at this `github repository <https://github.com/FunctionLab/KSENIA>`_.

The CLEVER framework is described in the following manuscript:

Ksenia Sokolova, Chandra L. Theesfeld, Aaron K. Wong, and Olga G. Troyanskaya, CLEVER: Predicting single-cell-resolved gene expression from sequence, Submitted, 2022

Download
--------
Predicted expression effects
~~~~~~~~~~~~~~~~~~~~~~~~~~~~
This is the bulk download `link <KSENIA>`_ of all mutation predictions.

Method Details
--------------
CLEVER uses exponential basis function-based linear models upon deep convolutional network model of chromatin effects. CLEVER predicts expression levels directly from sequence and is capable of predicting effects of sequence variations.

KSENIA For detailed procedures of the prediction, the chromatin predictions were computed from DeepSEA "Beluga" per 200bp bin, and 200 bins centered at TSS (40kb region) were used as input to predict expression effects. To reduce the dimensionality for ExPecto model training, the predicted chromatin spatial patterns were summarized to spatial features by 10 exponential basis functions. The summarized spatial features and gene expression levels were used to train regularized linear models for the final step of the prediction. The representative TSSes are selected based on FANTOM CAGE data.
