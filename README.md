# kaggle_show_US

## Basic info
URL: https://www.kaggle.com/c/coleridgeinitiative-show-us-the-data

### Goal

**In this competition, you'll use natural language processing (NLP) to automate the discovery of how scientific data are referenced in publications. Utilizing the full text of scientific publications from numerous research areas gathered from CHORUS publisher members and other sources, you'll identify data sets that the publications' authors used in their work**.

**The goal in this competition is not just to match known dataset strings but to generalize to datasets that have never been seen before** using NLP and statistical techniques.

### Evaluation
Your predictions will be short excerpts from the publications that appear to note a dataset.
**Submissions are evaluated on a Jaccard-based FBeta score between predicted texts and ground truth texts, with Beta = 0.5 (a micro F0.5 score)**. Multiple predictions are delineated with a pipe (|) character in the submission file.

Note that ALL ground truth texts have been cleaned for matching purposes using the following code:

`def clean_text(txt):
    return re.sub('[^A-Za-z0-9]+', ' ', str(txt).lower())`

For each publication's set of predictions, a token-based Jaccard score is calculated for each potential prediction / ground truth pair. The prediction with the highest score for a given ground truth is matched with that ground truth.

Predicted strings for each publication are sorted alphabetically and processed in that order. Any scoring ties are resolved on the basis of that sort.
Any matched predictions where the Jaccard score meets or exceeds the threshold of 0.5 are counted as true positives (TP), the remainder as false positives (FP).
Any unmatched predictions are counted as false positives (FP).
Any ground truths with no nearest predictions are counted as false negatives (FN).
All TP, FP and FN across all samples are used to calculate a final micro F0.5 score. (Note that a micro F score does precisely this, creating one pool of TP, FP and FN that is used to calculate a score for the entire set of predictions.)

**Predictions that more accurately match the precise words used to identify the dataset within the publication will score higher. Predictions should be cleaned using the clean_text function from the Evaluation page to ensure proper matching**.

※コンペのevaluationの欄にPythonでのJaccard Scoreの計算式あり

### Timeline

- March 23, 2021 - Start Date.
- June 15, 2021 - Entry Deadline. You must accept the competition rules before this date in order to compete.
- June 15, 2021 - Team Merger Deadline. This is the last day participants may join or merge teams.
- June 22, 2021 - Final Submission Deadline.

All deadlines are at 11:59 PM UTC on the corresponding day unless otherwise noted. The competition organizers reserve the right to update the contest timeline if they deem it necessary.

## Data

Publications are provided in JSON format, broken up into sections with section titles.

**A percentage of the public test set publications are drawn from the training set - not all datasets have been identified in train, so these unidentified datasets have been used as a portion of the public test labels**. These should serve as guides for the difficult task of labeling the private test set.

Note that the **hidden test set has roughly ~8000 publications, many times the size of the public test set. Plan your compute time accordingly**.

### Files

- train - the full text of the training set's publications in JSON format, broken into sections with section titles
- test - the full text of the test set's publications in JSON format, broken into sections with section titles
- train.csv - labels and metadata for the training set
- sample_submission.csv - a sample submission file in the correct format

### Columns

- id - publication id - **note that there are multiple rows for some training documents, indicating multiple mentioned datasets**
- pub_title - title of the publication (a small number of publications have the same title)
- dataset_title - the title of the dataset that is mentioned within the publication
- dataset_label - a portion of the text that indicates the dataset
- cleaned_label - the dataset_label, as passed through the clean_text function from the Evaluation page

## Diary

2021年4月9日（金）
まずはEDA、問題理解

気付き:

- 基本欠損値なし
- やたら数の多いpublicationあり（偏りがある）
- ジャンル幅広・・・農業からアルツハイマーから教育から鳥の繁殖からCOVIDまで
- やはりCOVID多いな（なお表記揺れあり e.g.「covid」と「SARS-CoV-2」）
