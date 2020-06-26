
# Raw Data 

Descriptions of Raw Data sets

# Data Pipeline Master Document

- A summary of all steps and related notebooks that were used in creating data and analysis for the solar project   
- Notebooks are sequentially dependent 
- Update: June 24, 2020 

## Clustering 

- cluster to define market boundaries.  We need this label for step 3, 4 and 5. The clustering labels is applied on EnergySage(ES) installers' coordinates(lat, long)
- Notebook :  step1_clustering_with_gridsearch.ipynb 
- input: 
    - coordinates data: dec13_total_df.csv 
- output: 
    - labeled coordinates : es_labeled'+parameters+'.csv' or es_labeled'+parameters+'twostep.csv 
    - graph: graph of market centroids 
  

### TODO: figure out if we used the two-step clustering in the final data, and refactor that part 
 - we used the es_labeled90_100_2two_step.csv 

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

## Feature from ES - Market Level 

- note: this will be depended on clustering step outcome 
- notebook : step3_es_feature_mktlevel.ipynb 
- input: 
    - market clustering labels: ./2_pipeline/es_labeled90_100_2two_step.csv' 
    - additional coordinates: '../0_data/dec14_total_df.csv' 
    - installer monthly :   '../2_pipeline/es_monthly_ind.dta' 
    - original reviews : /0_data/Lock_ES_RawData/installer_review_data_20180410.csv' 

- output: 
    -  '../2_pipeline/es_marketlevel'+parameters+'.csv')

## Feature from ES from webscraping 
- notebook: step4_addwebscrape_es_panel.ipynb
- input: 
    - building on output from step3 : '../2_pipeline/es_marketlevel'+parameters+'.csv')
    - add data obtained from webscraping EnergySage website '../0_data/es_web.csv' 
- output: 
    - '../2_pipeline/es_panel_step4'+parameters+'.csv'

## Features from TTS (Tracking The Sun) - Market Level 
- notebook: step5_tts_feature_mkt_level.ipynb 
- input: 
    - clustering result:  '../2_pipeline/es_labeled90_100_2two_step.csv'
    - zip code and coordinates data: "../0_data/RawData_geospatial//uszip_latlong.dta" 
- output 
    - intermediary: market condition data - '../2_pipeline/marketconditions'+parameter+'.csv'
    - market-month level data with zipcode_total_rev(revenue) '../2_pipeline/tts_mktcondidtions'+parameters+'.csv'


## Add Price info from matching TTS with ES installers 
- notebook: step6_tts_priceinfo.ipynb
- input: 
    - matching table that matches TTS with ES '../0_data/es_tts_matchingtable.xlsx' 
    - price : '../0_data/TTS_v2018.dta' 
    -  es panel data from step 4 
    - es_marketlevel data from step 3 
- output: 
    - Installer-month level own price and others' price on the market:   2_pipeline/'price_for_es_from_'+parameters+'.csv'

## Combine ES and TTS Market Level 
- notebook: step7_combine_es_tts_mktlevel_indlevel.ipynb
- input: 
    - from step 5: tts_mkt_condition=pd.read_csv('../2_pipeline/tts_mktcondidtions'+parameters+'.csv')
    - from step 6: tts_priceinfo=pd.read_csv('../2_pipeline/price_for_es_from_'+parameters+'.csv')
    - from step 4: es_panel=pd.read_csv('../2_pipeline/es_panel_step4'+parameters+'.csv')
- output:
    - '../2_pipeline/panel_step7'+parameters+'.csv'


## Processing and cleaning the individual level data 
- notebook: step8_clean_data_ind.ipynb
- input: "../2_pipeline/panel_step790_100_2two_step.csv"
- output: "../2_pipeline/final_step_analysis_ind_jan17.dta"

## Use BERT to Vectorize reviews Texts 
- notebook: step9_use_bert_vectorize.ipynb
- input: raw reviews texts '../0_data/Lock_ES_RawData/installer_review_data_20180410.csv'
- output: '../3_output/ALL_BERT_distances_pairwise_dec30.csv' 


```python

```
