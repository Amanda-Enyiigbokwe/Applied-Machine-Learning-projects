# Natural Language Processing Pipeline and Text Classification

## Project Overview

This project builds a basic Natural Language Processing pipeline end-to-end on a real business problem. The goal is to predict, at the point of submission, the sentiment expressed in a customer review: `Positive`, `Neutral`, or `Negative`. 
By scoring incoming reviews automatically, customer-experience teams can triage feedback at scale, surface unhappy customers quickly for service recovery, and monitor satisfaction trends across channels, product categories, and regions.
The analysis goes beyond accuracy to explore the linguistic structure of the corpus. It cleans and tokenizes raw review text, conducts exploratory frequency analysis overall and by sentiment class, applies POS tagging and Named Entity Recognition to representative examples, converts cleaned text into TF-IDF features, trains a Logistic Regression classifier, evaluates the model on a held-out test set, and concludes with a business-oriented interpretation of the result and its limits.


## Dataset Description

**File:** `NLP Customer Review Sentiment.csv`  
**Records:** 120  
**Columns:** 8

This dataset contains customer reviews collected by MapleMart, an online retail company. Each row represents one submitted review. Features include identifiers (`ReviewID`, `ReviewDate`), channel information (`Channel`: Website, Mobile App, Email Survey, Social Media), geographic context (`City`), product context (`ProductCategory`, `CustomerSegment`), the free-text review itself (`ReviewText`), and the sentiment label (`SentimentLabel`). 
Only `ReviewText` is used as the NLP input feature; the remaining columns provide business context for interpretation.

## Target Variable

`SentimentLabel`: Multi-class classification label
- **Positive** = Customer expressed satisfaction
- **Neutral** = Factual or non-evaluative review
- **Negative** = Customer expressed dissatisfaction

Class distribution: **40 Positive, 40 Neutral, 40 Negative** perfectly balanced (a property of the synthetic dataset).

## Model Used

**Logistic Regression (scikit-learn)**  
Configuration: `class_weight='balanced'`, `max_iter=1000`, `random_state=42`  
Feature extraction: `TfidfVectorizer` with `ngram_range=(1, 2)`, `min_df=2`, `max_features=1000`, producing 288 features per review.  
Preprocessing pipeline: lowercase normalization; removal of URLs, hashtags, digits, and punctuation; NLTK word tokenization; English stopword removal; WordNet lemmatization. Vectorizer fitted on the cleaned corpus before the train–test split.

## Main Evaluation Results

| Metric | Score |
|---|---|
| Accuracy | 100.00% |
| Precision (macro avg) | 1.000 |
| Recall (macro avg) | 1.000 |
| F1-Score (macro avg) | 1.000 |
| Test set size | 24 reviews |
| Training set size | 96 reviews |
| Test split | 20% stratified by class |

All metrics are reported on the held-out test set for the final Logistic Regression model. The confusion matrix is diagonal: 8/8 Negative, 8/8 Neutral, and 8/8 Positive reviews were correctly classified.

**Note:** The headline accuracy of 100% is not a realistic estimate of performance on live customer reviews. It reflects three properties of the supplied dataset rather than the strength of the model: the corpus is small (120 rows), it is synthetic and templated (several reviews are near-duplicates of one another — e.g. *"The website checkout was simple. I may buy again if the price is lower next time."* appears multiple times), and the classes are perfectly balanced and lexically separable. Cross-validation and a larger, more diverse corpus would be required before any deployment claim.

## Main Business Interpretation

This model predicts the sentiment expressed in submitted customer reviews, enabling MapleMart to triage incoming feedback at scale, prioritize responses to dissatisfied customers, and track satisfaction trends across channels and product categories.

The most predictive vocabulary is small and intuitive. 
**Positive** reviews lean on a handful of evaluative adjectives: *excellent*, *easy*, *happy*, *better*.
**Negative** reviews lean on a different handful: *disappointed*, *confusing*, *open* (as in "box was open"), *refund*. 
**Neutral** reviews are dominated by factual nouns and negated phrases: *arrived*, *package*, *price*, *amazing* (typically in "not amazing"). 
The overall most frequent words across the corpus: *item*, *maplemart*, *product*, *arrived*, *order*, *delivery*, *quality*, *price* reflect exactly the touchpoints a retailer would expect customers to discuss: the product, fulfilment, pricing, and the brand.

The most costly model error in a production setting would be a **False Negative on the Negative class** a genuinely unhappy customer classified as Positive or Neutral. 
Each such error means a manager never sees the case and the customer churns silently. A False Positive (a satisfied customer flagged as Negative) is operationally cheap by comparison, since the service team simply notes on review that no action is needed. 
Recall on the Negative class should therefore be the headline metric to watch when the model is retrained on real data.

POS tagging confirms that adjectives carry most of the sentiment signal while nouns carry the topic. 
Named Entity Recognition partially identifies cities and brand-like tokens (though the generic NLTK chunker mislabels some, e.g. tagging `Oshawa` as `PERSON` rather than `GPE`). 
Even with imperfect tagging, the entity layer is useful: NER on cities and products would allow MapleMart to slice negative sentiment by geography or product line, feeding directly into operations and logistics decisions.

## Limitation

The dataset contains only 120 records and is synthetic, with several templated near-duplicate sentences and a perfectly balanced class distribution, none of which holds for real customer feedback. 
The 100% test accuracy is therefore an artifact of the dataset rather than evidence of generalization. The vocabulary after cleaning contains only ≈ 120 unique tokens across ≈ 1,100 total tokens, which is too small to learn nuanced sentiment expressions, sarcasm, negation patterns ("not bad"), or domain-specific language. 
The generic NLTK POS and NER models are also not tuned for retail text and mislabel some entities. The model should be retrained on a larger, more linguistically diverse sample of real MapleMart reviews (target ≥ 2,000 records), with k-fold cross-validation and a time-based held-out test set to guard against leakage from duplicates, and with stronger feature representations (e.g. transformer embeddings) and explicit handling of negation before any production deployment.

## How to Run

**Option 1 — View on GitHub:**  
Open `nlp_notebook.ipynb` directly in GitHub. Notebooks render with code, outputs, and markdown visible without any setup.

**Option 2 — Run in Google Colab:**  
Upload `nlp_notebook.ipynb` and `NLP Customer Review Sentiment.csv` to a Colab session, then run all cells from top to bottom (`Runtime > Run all`).

**Option 3 — Run Locally in Jupyter:**
```bash
pip install pandas scikit-learn matplotlib nltk openpyxl
```
Place the dataset Excel file in the same directory as the notebook, launch Jupyter, and run all cells in order. The setup cell automatically downloads the required NLTK resources (`punkt`, `stopwords`, `wordnet`, `averaged_perceptron_tagger`, `maxent_ne_chunker`, `words`).

---
