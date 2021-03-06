# Enhancing Airbnb Ratings through Natural Language Processing
Jon Yu

## Abstract

Airbnb has experienced exponential growth since its launch 2008. Aided by low mortgage rates in recent years which saw 56% quarter to quarter increase in multifamily unit investments and surge of new landlords (https://capitalcounselor.com/airbnb-statistics/), Airbnb currently observes 14,000 new hosts per month in 2021 (https://capitalcounselor.com/airbnb-statistics/). However, becoming a multi-property landlord does not necessarily pave the way to financial independence as the U.S. has reported 50% relative increase in unemployment from July 2019 to July 2021 (https://data.bls.gov/timeseries/LNS14000000).

As a newly entering entrepreneur who isn't an established superhost, what can you do to appeal to guests and maximize your Airbnb listing's occupancy rate? To aid in this pursuit, I decided to launch this natural language processing and unsupervised learning project. The end goal is to unravel undiscovered metrics which drive guest enjoyment and overall rating.

## Data
Honolulu, Hawaii, boasts the highest Airbnb occupancy rate in the U.S. at 68% (https://capitalcounselor.com/airbnb-statistics/). Hence, the below two data sets from Inside Airbnb (http://insideairbnb.com/get-the-data.html) detailing Hawaii Airbnb's serve as the crux of this project. 

1. 21,808 listing entries
2. 603,048 review entries (documents of the corpus)

The 300 reviews with no comments were discarded. Roughly 1.5% of the reviews were determined to be non-English using the LANGDETECT module. As the GOOGLETRANS module could not handle these due to hitting the API quote, these foreign reviews were also discarded.

Subsequent to cleaning and text preprocessing, two data frames were combined using a left merge on the review data set with coinciding listing ID.

## Algorithms/Tools
- Pandas to ingest and clean data
- LANGDETECT to automatically detect review language and GOOGLETRANS to attempt to translate the non-English reviews
- NLTK for text pre-processing, including lemmatization, stopwords
- WordCloud for visualizing highest frequency words in corpus
- Scikit-Learn for: 
    - Scaling
    - Vectorization (Count Vectorizer, TF-IDF)
    - Topic modeling (NMF, LSA)
    - Dimensionality reduction (PCA)
    - Clustering (KMeans)

## Findings
Text preprocessing and constructing the ever-evolving stopword list was an iterative procedure. Looking at the Word Cloud, the words "beach" and "clean" clearly stood out and lent direction to the possible topics.

<img src = "Images/WC.png" width = 800>

Among the four-way combination resulting from LSA, NMF, Count-Vectorizer, and TF-IDF using three to ten components, NMF TF-IDF at four components provided best interpretability.

<img src = "Images/NMF.png" width = 800>

PCA was tested, but ultimately not used for the final output. When testing dimensionality reduction via PCA, reduction to two dimensions resulted in too much data loss compared to three dimensions.

The topics/weights from NMF were scaled and fed into the K-means clustering model. Inspecting the inertia elbow, it wasn't apparent if 5 or 7 clusters would be ideal. 

<img src = "Images/Inertia.png" width = 500>

After plotting both scenarios, 5 cluster selection resulted in better topic weighting distinction in each cluster. 

<img src = "Images/7_Clusters.png" width = 400>
<img src = "Images/5_Clusters.png" width = 400>

## Conclusions
The topic weighting scores were implemented into a Tableau dashboard. The 4 topics derived could be toggled on/off in order to provide an adjusted rating to be compared with the original weighting.
<img src = "Images/Tableau.png" width = 800>

Naively, I concluded that focusing on the below parameters can help a new Airbnb host get a leg up on the competition.
1. Convenience
2. Posting Accuracy (consistency between listing description and actual offering)
3. Cleanliness
4. View of the Ocean

However, you can see that cleanliness and accuracy are already sub-categories that can be rated in the live implementation. Moreover, location is loosely associated with convenience and view of the ocean.
<img src = "Images/Existing.png" width = 800>

The revised conclusion is that Airbnb's data science & analytics teams have already conducted extensive research and implemented the most thoughtful parameters. For future work, I see value in:
1) Considering region specific metrics that cannot be cpatured by the existing, generalized metrics
2) Selectively filtering out existing sub-ratings by incorporating related words into the stop words

## Communication
The presentation slide deck is available [here](https://github.com/runjon90/NLP_Unsupervised_Learning/blob/main/Slidedeck.pdf). In addition to a [sample video](https://github.com/runjon90/NLP_Unsupervised_Learning/blob/main/Hidden%20Rating%20Demo%20on%20Tableau.mov) of the Tableu model depicting the alternative Airbnb rating system, an online deployment is also available at https://public.tableau.com/app/profile/jon1263/viz/NLPAirbnb/Sheet1?publish=yes. Lastly, the entirety of the code used can be found within this [repo](https://github.com/runjon90/NLP_Unsupervised_Learning/tree/main/Code).
