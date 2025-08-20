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
    
