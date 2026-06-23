# Support Ticket Classification & Data Integrity Audit

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?style=flat-square&logo=python&logoColor=white)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Latest-blueviolet?style=flat-square&logo=scikit-learn&logoColor=white)
![NLP](https://img.shields.io/badge/NLP-TF--IDF-success?style=flat-square)
![Data Audit](https://img.shields.io/badge/Data_Audit-Completed-orange?style=flat-square)

## Executive Summary
The objective of this project was to build a Natural Language Processing (NLP) pipeline capable of automatically reading customer support tickets and routing them to the correct department (Category) with an assigned urgency level (Priority). 

While the machine learning pipeline was successfully built and deployed, **this project ultimately serves as a critical Data Integrity Audit.** Through rigorous error analysis, the model proved that the provided Kaggle dataset contains artificially randomized target labels, demonstrating the crucial business principle: *Machine learning can only automate processes that have a genuine underlying pattern.*

---

## The NLP Pipeline Methodology
To attempt ticket triage, an industry-standard NLP pipeline was constructed:
1. **Text Preprocessing:** Raw ticket text was cleaned by converting to lowercase, stripping punctuation, and removing English stopwords using the `NLTK` library.
2. **Feature Engineering:** Both the *Ticket Subject* and *Ticket Description* were concatenated to maximize context. The text was then converted into numerical matrices using **TF-IDF (Term Frequency-Inverse Document Frequency)** to highlight unique, high-value keywords.
3. **Classification:** Two independent **Logistic Regression** models were trained—one to predict the Ticket Category (5 classes) and one to predict the Ticket Priority (4 classes).

---

## Results & The Reality Check
Upon evaluation, the models returned the following metrics:
* **Category Classification Accuracy:** ~20.3%
* **Priority Classification Accuracy:** ~26.6%

### The Diagnosis: Zero-Signal Synthetic Data
Mathematically, an accuracy of 20% across 5 categories and 26% across 4 priorities perfectly mirrors the statistical probability of **blind, random guessing**. 

A deep dive into the dataset's raw text revealed why: The dataset is synthetically generated using scraped text templates (e.g., *"I'm having an issue with the [Product]..."* followed by random, disconnected paragraphs). The labels for "Category" and "Priority" were assigned entirely at random by the dataset creator. A ticket demanding an "immediate refund for a broken screen" has an equal mathematical chance of being labeled as a "Billing Inquiry" or "Technical Issue." 

Because there is zero actual correlation between the words used by the customer and the label provided, the NLP model correctly recognized that there was no mathematical pattern to learn. 

---

## Business Takeaways for Operations
For Support Managers and SaaS Founders, this project highlights three critical operational realities:

1. **Garbage In, Garbage Out:** You cannot train an AI to automate your support desk using messy, improperly labeled historical tickets. If human agents randomly clicked tags in the past to close tickets faster, the AI will learn to do the exact same thing.
2. **Priority is rarely dictated by text alone:** Customers will almost always claim their issue is a "Critical Priority." In a real-world system, text classification should be combined with metadata (e.g., Customer Tier, Plan Price, Account Age) to truly determine SLA priority.
3. **The Value of the Audit:** Deploying a model blindly based on synthetic data would result in catastrophic routing failures in a live environment. This pipeline acts as an automated auditor, verifying whether historical data is actually clean enough to support an operational AI deployment.
