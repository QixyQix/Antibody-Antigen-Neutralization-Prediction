# Predicting Antibody-Antigen Interactions With Transformer-based Machine Learning (SCSE22-0968)
Final Year Project to predict the neutralisation properties between antibodies and antigens, focused on COVID-19 through the use of natural language proessing, and transformer-based machine learning models. The report can be found in [FYP-Final-Report.pdf](FYP-Final-Report.pdf)

We finetune a pre-trained protein language model (PPLMs) to predict the neutralization properties of an antibody given a virus sequence using just the protein sequences alone, and experiment with the usage of full antibody protein seuqneces, as well as just the CDR3 portion of the antibody sequences.

This is compared against a traditional Machine Learning model (Logistic Regression)

## In this Repository:
- `01_Data_Cleaning` contains the code for the data cleaning and dataset generation portion of the project
    - Data from [Coronavirus Antibody Database (CoV-AbDab)](https://opig.stats.ox.ac.uk/webapps/covabdab/) is filtered
    - Referenced structures' FASTA sequences are downloaded
    - Virus protein sequences are put through tBLASTn (manually) to obtain nucleotide sequence
    - Nucleotide virus sequences put through GISAID AudacityInstant to determine virus variant
    - Virus variant mapped to labels used in CoV-AbDab
    - Dataset with examples of Antibody Protein Sequences, Virus Protein Sequences, and Neutralizing properties is generated
- `02_Dataset_Curation` contains code for the filtering of usable data from our generated dataset for this project
    - Only examples where the combined protein sequence length is <1024 bases/characters is used
    - Performing of train test split
- `03_Logistic_Regression` contains code involving Logistic Regression model which is compared against
    - Special thanks to Teng Ann whose code is used to base this portion of the project off
    - Featuremap encoding is performed the sequences
    - Featurized examples are then fitted on the Logistic Regression model
- `04_ESM2b_Finetuning` contains code involving the PPLM finetuning aspect of the project
    - The HuggingFace repository was used due to its popularity
    - The [ESM2b](https://huggingface.co/facebook/esm2_t12_35M_UR50D) PPLM was used
    - Model is finetuned over 10 epochs

## Results
- Finetuned Transformer-based PPLM achieves accuracies as high as **98.3%**
- Using full antibody sequence provides some additional benefit to model accuracy
![esm_balanced_full](/05%20Prediction%20Analysis/lr_bal_full.png "Confusion Matrix for Full Antibody Sequence (Logistic Regression)")
![esm_balanced_full](/05%20Prediction%20Analysis/esm_bal_full.png "Confusion Matrix for Full Antibody Sequence (Logistic Regression)")

## Notes
- The `training_combined.csv` file has been excluded from this repository due to its large file size, it can be recreated by combining `training_negative.csv` and `training_positive.csv` found in the `01_Data_Cleaning` folder