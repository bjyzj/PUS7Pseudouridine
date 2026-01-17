# PUS7-INDUCED PSEUDOURIDINE SITES IDENTIFICATION USING EXTREME GRADIENT BOOSTING: SEQUENCE AND STRUCTURAL CONTEXT

This repository contains the preliminary results and code for data preparation, processing, modelling, and visualisation of preprocessed sequence information collected from different studies involving PUS7+ and PUS7- samples.

Among the more than 170 chemical modifications identified in RNA to date, one of the most abundant and extensively studied internal post-transcriptional modifications is pseudouridine (Ψ). Despite its importance, Ψ remains poorly understood due to the technical challenges and limitations associated with its accurate detection (Hermon, Sennikova and Becker, 2024). Given its significance and the existing knowledge gaps, this thesis investigates the sequence and structural features of Ψ and assesses their predictability at enzyme-specific Ψ modification sites.


### Abstract
Pseudouridylation (Ψ) is an essential RNA modification with diverse roles in translation, stability, and regulation, yet the determinants of enzyme-specific activity remain unclear. This thesis investigates PUS7-mediated pseudouridylation by integrating RNA sequence and secondary structural features into supervised classification models. Sequence features emerged as the strongest predictors, while structural descriptors contributed additional, context-dependent improvements. Notably, the combined sequence–structure model outperformed sequence-only and structure-only approaches, and feature importance results confirmed the biological relevance of enriched motifs such as UGU/UUG cores with AG/UC flanks. Structural analyses also suggested a preference for unpaired regions such as hairpin loops, consistent with known PUS7 activity, though these effects were limited by dataset size.
These findings highlight the need for larger, balanced datasets and careful consideration of protocol biases, including those introduced by reliance on reference genomes and the potential impact of single nucleotide variants (SNVs). Beyond structural insights, pseudouridylation has therapeutic implications in diseases such as cancer and Alzheimer’s, with PUS7 specifically implicated in glioblastoma immune evasion, neurological disorders, and as a target of small-molecule inhibitors that suppress tumorigenesis. Defining the sequence–structure determinants of PUS7 activity provides a framework for predictive modelling, which offers a powerful approach to anticipate modification sites and guide future therapeutic research.

<img width="300" height="250" alt="diss_workflow" src="https://github.com/user-attachments/assets/68774c57-81e9-4da3-a79f-0b382a262810" />


### RNA Secondary Structure Building
RNA secondary structures were predicted for PUS7-positive sequences using ERNIE-RNA (Yin et al., 2024). Secondary structure annotations were generated with bpRNA, which converts bpseq files into structure tree (.st) and secondary structure (.ss) formats, and, for a subset of sequences, structures were converted from CT format to dot-bracket notation using the ct2db function in ViennaRNA (v2.0; Lorenz et al., 2011). All structures were visualised with VARNA (v3.93; Darty, Denise and Ponty, 2009) (Figure 4).
<img width="1206" height="948" alt="image" src="https://github.com/user-attachments/assets/c015a355-862e-4a16-98bb-239a00dfb513" />


### Modelling XGBoost and Performance Evaluation
Supervised classification was performed to distinguish PUS7-dependent (positive) from PUS7-independent (negative) Ψ sites. In total, 1,396 sites were included for training, comprising 329 positives and 1,024 negatives. Predictor matrices were constructed to represent sequence-only (174 features), structure-only, and merged feature sets (186 features), all aligned to the same labelled sites with missing values imputed as zeros. Stratified five-fold cross-validation was applied to ensure consistent data splits across all predictor sets, and per-fold class weights were computed based on the ratio of negative to positive training samples and used to address imbalance. An eXtreme Gradient Boosting (XGBoost) classifier was trained within each fold, and out-of-fold predictions were aggregated to generate precision–recall curves, with performance summarised (Figure 5A). Feature importance was assessed using SHAP (SHapley Additive exPlanations) values to interpret the contribution of sequence- and structure-derived predictors to model predictions. For each cross-validation fold, an XGBoost classifier was trained on the training split with class weights adjusted for imbalance, and SHAP values were computed on the corresponding held-out set using the TreeExplainer framework. SHAP values from all folds were then combined to provide a complete set of out-of-fold explanations, ensuring that every site was evaluated only when held out from training. Mean absolute SHAP values were calculated to rank predictors by their average contribution to classification (Figure 5B). 

<img width="910" height="1196" alt="image" src="https://github.com/user-attachments/assets/75934277-7e16-4bb9-ae09-d654985874d0" />

<img width="1152" height="724" alt="image" src="https://github.com/user-attachments/assets/63ccc378-721a-4d64-96ff-ff3297006cbb" />


### References
Darty, K., Denise, A. and Ponty, Y., 2009. VARNA: Interactive drawing and editing of the RNA secondary structure. Bioinformatics, 25(15), p.1974.
Hermon, S.J., Sennikova, A. and Becker, S., 2024. Quantitative detection of pseudouridine in RNA by mass spectrometry. Scientific Reports, 14(1), p.27564.
Lorenz, R., Bernhart, S.H., Höner zu Siederdissen, C., Tafer, H., Flamm, C., Stadler, P.F. and Hofacker, I.L., 2011. ViennaRNA package 2.0. algorithms. Mol Biol, 6(1), p.26.
Yin, W., Zhang, Z., He, L., Jiang, R., Zhang, S., Liu, G., Zhang, X., Qin, T. and Xie, Z., 2024. ERNIE-RNA: an RNA language model with structure-enhanced representations. bioRxiv, pp.2024-03.
