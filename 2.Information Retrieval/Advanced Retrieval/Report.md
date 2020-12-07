# Report on Assignment 2B

**Team ID:** team-010

**Student Name(s):** Rabbir Bin Rabbani


## Feature development (max. 500 words, excluding table and list)

* Query-document features -
    * BM25 of Title, Content, Anchors - BM25 Score of the Title, Content and Anchors.
    * LM of Title, Content, Anchors - LM Score using Jelinek-Mercer smoothing of Title, Content and Anchors.
* Query Features -
    * Query Length - Number of words in the query.
    * Token Length - Number of words in the query after '_analyze' removes the stopwords and applies stemming.   
* Document Features -
    * Pagerank -  Pagerank of the document, using 'pagerank.docNameOrder'.
    * Length in Main Index - Length of the document in the MAIN_INDEX.
    * Length in Anchors Index - Length of the document in the ANCHORS_INDEX.

### Learning algorithm
I have used SKlearns GridsearchCV function on Decision Tree and Random Forest to find the best hyperparameter combination of each models, and comparatively Decision Tree seems to have the lower error, so I chose Decision Tree with *'max_depth=2'* and *'min_samples_split=200'*.
For this I have used an NDCG Scorer I found on a post in Kaggle, the link to this post is in the references section.

### Results

| **Features** | **Output file** | **NDCG@10** | **NDCG@20** | **NDCG@100** |
| -- | -- | -- | -- | -- |
| Only QD features | **ranking_QD.csv** | 0.046388364114609885 | 0.043307932502692895 | 0.025958321382731006 |
| QD + Q features | **ranking_QD_Q.csv** | 0.046388364114609885 | 0.043307932502692895 | 0.025958321382731006 |
| QD + D features | **ranking_QD_D.csv** | 0.046388364114609885 | 0.043307932502692895 | 0.025958321382731006 |
| ALL features (QD + Q + D) | **ranking_all.csv** | 0.046388364114609885 | 0.043307932502692895 | 0.025958321382731006 |


## Feature selection experiments (max. 400 words, excluding table)

I have used sklearns SelectKBest to find the best 5 features. I originally tried the same scorer as the NDCG scorer as before, but it was not working so I used chi-squared scores. They are as follows - 
    * bm25_anchors
    * bm25_content
    * bm25_title
    * doc_main_indx_length
    * doc_anchors_indx_length

### Results

| **Features** | **Output file** | **NDCG@10** | **NDCG@20** | **NDCG@100** |
| -- | -- | -- | -- | -- |
| ALL features (QD + Q + D) | **ranking_all_test.csv** | 0.046388364114609885 | 0.043307932502692895 | 0.025958321382731006 |
| Selected five best features | **ranking_best_featuers_test.csv** | 0.033444050971747394 | 0.04039123782635726 | 0.02421182661920442 |


## Kaggle submission (max. 300 words)

The features I calculated were fairly simple, so I have used all the features as it doesn't seem to change the score very much.

## Challenges
* In some cases, the term_vectors does not return any values, in these cases I'm returning a score of zero. for this reason, many BM25 and LM scores have a value of zero.

## References
* NDCG Scorer - https://www.kaggle.com/davidgasquez/ndcg-scorer