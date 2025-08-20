# Identifying Key Entities in Recipe Data
**Problem Statement:** The goal is to build a model, that will classify words into predefined categories such as ingredients, quantities and units, enabling the creation of a structured database of recipes and ingredients that can be used to power advanced features in recipe management systems, dietary tracking apps, or e-commerce platforms.

## Table of Contents
* [General Info](#general-information)
* [Conclusions](#conclusions)
* [Technologies Used](#technologies-used)
* [Acknowledgements](#acknowledgements)

<!-- You can include any other section that is pertinent to your problem -->

## General Information
  ### Business Objective:
  The business objective is to leverage the increasing popularity of online cooking platforms and meal-planning apps by enhancing the user experience. This can be achieved by implementing a custom-named entity recognition (NER) model to automatically tag ingredients, quantities and recipe names. This automation will streamline the process of organising recipes, improve searchability and enable users to easily find recipes based on available ingredients, portion sizes or specific dietary requirements. This will ultimately reduce the labour-intensive and inefficient manual tagging process, providing a more accessible and efficient way for businesses in the food and recipe industry to manage their recipe databases.

  ### Data Description:
  The given data is in JSON format, representing a structured recipe ingredient list with Named Entity Recognition (NER) labels. Below is a breakdown of the data fields: Key Description
  - `input`: Contains a raw ingredient list from a recipe.
  - `pos`: Represents the corresponding part-of-speech (POS) tags or NER labels, identifying quantities, ingredients, and units.

  ### Approach:
  - Extracted a rich set of `lexical`, `numeric`, and `contextual` features for each token, e.g.:
    - Lowercase form, lemma, part-of-speech tags, numeric checks, and punctuation presence.
    - Keywords and patterns to robustly recognize units (e.g., "cup", "kg") and quantities (including fractions and numbers).
    - Context: previous/next token features, BOS/EOS flags.
  - `Model Chosen`: Model build using Conditional Random Field (CRF).CRFs are ideal for sequence labelling, capturing dependencies between neighbouring tags and effectively utilizing hand-crafted contextual features, especially important in structured, ordered lists such as recipe ingredients.

  ### Techniques:
  - **Data Preparation:**
    - `Loading`: The recipe data was ingested from a structured JSON file containing two key fields: a raw ingredient string (input) and its corresponding entity label sequence (pos).
    - `Structuring`: Tokenized both ingredient (input) lines and POS/NER labels (pos) for each word in the ingredient strings.
    - `Unique Labels`: The only present entity labels are ingredient, quantity, and unit, confirming the narrow focus of the NER task.
    - `Assumptions`:
      - Only these three entity types are relevant and sufficient for the extraction task.
      - The small number of dropped rows does not meaningfully bias the data.
  - **Data Manipulation & Insights:**
    - `Validation`: Added new columns to validate that the number of tokens matches the number of labels. Verified and dropped mismatches to ensure every token match a label.
    - `Tokenization`: All recipes are now fully aligned: every word has a corresponding label.
  - **Data Splitting:**
    - `Splitting`: The clean data was split into training and validation sets with a 70:30 ratio to support robust model evaluation.
    - `Train/validation split`: 70:30 random split (196 training, 84 validation samples).
    - Only three label types (`ingredient`, `unit`, `quantity`) were present—no other entities
  - **Model Setup:** Instantiated CRF with the following parameters:
    - Algorithm: `lbfgs` (efficient for medium-sized data).
    - Regularization parameters: c1 and c2 set based on grid search or default values to control overfitting.
    - Maximum iterations: Typically set high (e.g., 100) for convergence.
  - **Model Training:** The CRF was trained using feature-extracted token sequences and corresponding label sequences from the training set.
  -	**Evaluation metrics**:
    - `Classification report`: High precision, recall, and F1-scores for all classes.
    - `Confusion matrix`: To evaluate the performance of a classification algorithm - diagonal of the confusion matrix represents correctly predicted instances, and the off-diagonal elements show where the model has misclassified or "confused" the classes.
  - **Final Results:**
    - `Training accuracy`: `99.03%`
    - `Validation accuracy`: Slightly below training, showing good generalization – `98.02%`

## Conclusions:
The CRF-based Named Entity Recognition model trained on the provided recipe data effectively extracts structured entities — `quantities`, `units`, and `ingredients` — enabling the transformation of informal ingredient lists into structured data. The detailed feature engineering and handling of class imbalance were crucial in achieving high accuracy and generalization.

## Technologies Used
- **python** - 3.13.1
- **numpy** - 2.2.1
- **pandas** - 2.2.3
- **matplotlib** - 3.10.0
- **seaborn** - 0.13.2
- **statsmodels** - 0.14.4
- **sklearn** - 1.6.1
- **keras** - 3.8.0
- **PIL** - 11.1.0
- **tensorflow** - 2.18.0
- **sklearn_crfsuite** - 0.5.0

## Acknowledgements

- This project was inspired by Upgrad IIIT Bangalore PG program on ML and AI.
- This project was based on Syntactic Processing


## Contact
Created by @[GVChalamaReddy](https://github.com/GVChalamaReddy)- feel free to contact me!
    
