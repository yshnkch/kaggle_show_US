# kaggle_show_US

## Basic info
URL: https://www.kaggle.com/c/coleridgeinitiative-show-us-the-data

This competition will build just such an open and transparent approach. 

The results will show how public data are being used in science and help the government make wiser, more transparent public investments.

It will help move researchers and governments from using ad-hoc methods to automated ways of finding out what datasets are being used to solve problems, what measures are being generated, and which researchers are the experts. **Previous competitions have shown that it is possible to develop algorithms to automate the search and discovery of references to data**. Now, we want the Kaggle community to develop the best approaches to identify critical datasets used in scientific publications.

**In this competition, you'll use natural language processing (NLP) to automate the discovery of how scientific data are referenced in publications. Utilizing the full text of scientific publications from numerous research areas gathered from CHORUS publisher members and other sources, you'll identify data sets that the publications' authors used in their work**.

If successful, you'll help support evidence in government data. Automated NLP approaches will enable government agencies and researchers to quickly find the information they need. The approach will be used to develop data usage scorecards to better enable agencies to show how their data are used and bring down a critical barrier to the access and use of public data.

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

※コンペのevaluationの欄にPythonでのJaccard Scoreの計算式あり

## Diary

2021年4月9日（金）
まずはEDA、問題理解

気付き:

- 基本欠損値なし
- やたら数の多いpublicationあり（偏りがある）
- ジャンル幅広・・・農業からアルツハイマーから教育から鳥の繁殖からCOVIDまで
- やはりCOVID多いな（なお表記揺れあり e.g.「covid」と「SARS-CoV-2」）
