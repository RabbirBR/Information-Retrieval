# Assignment 3 report

**Team number:** team-*004*

**Team members:** Anisa Zhurda, Rabbir Bin Rabbani

## Indexing and fields
We decided to create two indexes, a term-based index and a separate entity-based index. For our indexes, we are using simple settings with 1 'shard' and 1 'replica'. 

To preprocess the fields in our indexes we are using an analyzer which is similar to the one applied to our query set . First it lowercases the data, then removes stopwords and finally stems the text with the porter2 algorithm.

Since the dataset size is considerably big ,we decided to first put all the data in one dictionary and then bulk index the data in our indexes. And since it takes a very long time to process the data, we also decided to save this dictionary as a pickle file as a backup for possible future changes.

We decided to process the 6 files -
   1. Label.
   2. Pagelinks.
   3. Long Abstracts.
   4. Categories.
   5. Disambiguations.
   6. Person Data.
   
The preprocessing on these files is as follows:
 - **Labels** - We had to separate the words by capital letters.
 - **Pagelinks** - We combined the alternative links as a list by grouping them by the original link.
 - **Long Abstract** - We used our analyzer in our index preprocess it.
 - **Categories** - We combined all the categories of any given entity as a list by grouping them by the link.
 - **Disambiguations** - We combined all the disambiguations of any given entity as a list by grouping them by the link.
 - **Person Data** - Person data is a bit more complicated than the other files since it contains several different kind of information. We had to first group them by the link and then we had all the different type of values of each entity.
 
We manually create our content or 'catch-all' field while bulk indexing the data by joining the content of each field except of label .

## Implementation of MLM

The implementation of MLM was done using the term-based index and the fields label and content with the required corresponding weights 0.2 and 0.8,using the Dirichlet Smoothing formula with smoothing parameter 2000.
For each field we calculated the term based frequency in the document, length of that field,and we precomputed the probability of that term in the entire collection using a separate function *get_prob*. For an easier implementation we used termvectors to get the statical values needed for each of the computation,by specifying the field and document.
After normalizing the term-document score for each field ,and giving the specific weight for that field we sum them over fields and then over query terms.
The query-document relevance score is then sorted by decreasing order and we are taking top 100 most relevant documents and saving their corresponding label.  


## Implementation of SDM+ELR
This part consists of two main steps :
 - Computing SDM-a main summation in terms of query-document relevance scores using unigrams,ordered and unorderd bigrams,with documents taken from the term-based index.
 - Computing ELR-Using entity related index and entity annotated queries.
 
 For calculating the SDM unigram score ,we used LM with Dirichlet smoothing,only for the catch_all field,which consists of the same steps as the above MLM algorithm but with one field and without weighted significance as we did for multifields.The result will be only one main summation over query terms per document.
 On the other hand calculating the SDM score for ordered and unordered bigrams was more manual and without the help of built-in index methods such as termvectors.The steps are similar for both ordered and unordered bigrams.First we created several bigrams out of each query by using ngram with n=2,word based. Then we raw-counted each bigram in documents transformed in term sequences which was then used to create a three level dictionary (query->bigram->document) and the corresponding count. The rest is a computation of Dirichlet smoothing on bigram level,for which we calculated the length of the documents with a separate function length_doc_bigram ,and the probability of that bigram in the document and in the entire collection by using the raw counts . Again we summed the scores over bigrams,to get the query document relevance score .Finally we merged the scores of unigram,ordered and unorderd bigrams after updating them with the specific parameters and sorted by relevance.
 
 For calculating ELR for each annotated query we computed the binary indicator function regarding to each field,an overall count of the presence of all the entities of the query,in the actual field and the field length in case it wasnt empty.
 We summed over weighted fields and then over weighted q-entities,and paramterized the score to then merge it with the already computed SDM score.
 
 
The top 100 sorted scores will be our SDM+ELR results. 
 
## Implementation of Your model

We can use ELR as an extension of different MLR based models,such as LM, MLM, SDM, FSDM, PRMS. Since we already computed MLM score, we thought it would make sense to extend it with ELR and experiment over the results.


## Results

  - *Report the performance results in the table below, using each of your three models to output entity ranking predictions given `queries.txt`. Evaluate the models' predicictions with respect to `qrels.csv` using `3_Evaluation.ipynb`.*

| Model | NDCG@10 | NDCG@100 |
| -- | -- | -- |
| MLM title+content | *0.127* | *0.142* |
| SDM+ELR | *0.124* | *0.133* |
| MLM+ELR | *0.105* | *0.118* |


## Discussion

Losing information while preprocessing or indexing is going to affect the scores significantly.
Our models can be improved by creating  more structured and compact computations in a class,instead of implementing each model separately and we can try all possible combinations of MLR based models and ELR extension,to decide the best performing one.


<!--   - *Discuss the work done, the results, and the understanding gained from this experience. Outline the patterns you observed relating to the models' mechanisms and their respective performance.*
  - *Describe alternative approaches you tried in developing your own model, if any.*
  - *What are other approaches that could be useful, which you didn't try? Make an argument why these hypothetical approaches might be effective.* -->


<!-- ## References
  - *If you used external resources (websites, books, articles, etc.) make sure you acknowledge them here.* -->
