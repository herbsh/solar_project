
# Data Pipeline Master Document

- A summary of all steps and related notebooks that were used in creating data and analysis for the solar project   
- Notebooks are sequentially dependent 
- Update: June 24, 2020 

## Raw Data 

Descriptions of Raw Data sets

## Clustering 

- cluster to define market boundaries.  We need this label for step 3, 4 and 5. The clustering labels is applied on EnergySage(ES) installers' coordinates(lat, long)
- Notebook :  step1_clustering_with_gridsearch.ipynb 
- input: 
    - coordinates data: dec13_total_df.csv 
- output: 
    - labeled coordinates : es_labeled'+parameters+'.csv' or es_labeled'+parameters+'twostep.csv 
    - graph: graph of market centroids 
  

### TODO: figure out if we used the two-step clustering in the final data, and refactor that part 

## Feature derived from ES - Installer Level 

### part a: run NLP script to obtain sentiment score 

- notebook: step2_part_a_reviews_sentiment_score.ipynb
- input: /0_data/Lock_ES_RawData/installer_review_data_20180410.csv 
- output: /2_pipeline/reviews_sent_score_jan17_2020.csv , /3_output/sentscore_violin.png

### part b: obtain the rest of ES-installer level features 


- notebook: step2_part_b_es_feature_ind.ipynb 
- input: 
    - from raw data: /0_data/Lock_ES_RawData/installer_close_rates.csv, /0_data/Lock_ES_RawData/installer_review_data_20180410.csv, 
    - from pipeline: /2_pipeline/reviews_sent_score_jan16_2020.csv' 
- output: 
    - to pipeline: 2_pipeline/closerate_withratingcounts_sent_score.csv'


### part c: use stata to fill in time gap from the closerate and carryforward snapshots of reviews data 

- notebook: step2_part_c_filltimegap.ipynb 
- input: 
    - from pipeline: /2_pipeline/closerate_withratingcounts_sent_score.csv" 
- output: 
    - to pipeline: /2_pipeline/es_monthly_ind.dta" 


```python

```
