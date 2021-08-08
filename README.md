# EntityResolution_Assignment

### Problem Description

“Entity resolution” is the problem of identifying which records in a database represent the same entity. When dealing with user data, it is often difficult to control the quality of the data inputted into the system. The poor quality of the data may be characterized by:

1. Duplicated records
2. Records that link to the same entity across different data sources
3. Data fields with more than one possible representation (e.g. “P&G” and “Procter and Gamble”)

### Configuration

1. Install Record linkage library in Conda Environment or Google Colab using pip install recordlinkage
2. Alternaitvely, execute conda env create at the start of the project. 

### Datasets

1. Scholar.csv: dataset shape of (64260, 5)
2. DBLP.csv : Dataset shape of (2616, 5)


### Description of steps 

1. Data pre-processing: Cleaning of raw data from both datasets by converting the text into lower case, replacing special characters with whitespaces and by removing brackets/ noise using clean() function from Python Record Linkage toolkit. 

2. Indexing: Indexing creates pair of records called candidate links or candidate matches based on indexing algorithms such as 'full indexing', 'blocking' and 'sorted neighborhood'. It also detects duplicate records across the datasets and returns a complete set of record pairs.

3. 'Sorted neighbourhood' indexing is used on 'title' column on both datasets resulting in records pairs of 9457 with default windowsize=3. A large window size will result in more record pairs. Indexing is conducted on 'title' column as both datasets had no null values in the title column whilst other columns in the datasets had a lot of null values. Furthermore, Sorted Neighbourhood method is appropraite when there is relatively large amount of spelling mistakes. 

4. Comparing: It is used to compare records pairs as set of informative, discriminating and independent features is important for a good classification of record pairs into matching and distinct pairs. The outcome of the step is a features dataframe that shows the results of all of the comparisons. A 1 is a match and 0 is not.

5. Scoring method: A score is assigned to determine the quality of matches. A score of 4 indicates all 4 records matches across the datasets and a score of 0 suggests Zero matches (distinct record). 

6. A thresold of score above 2 is chosen to filter the final matches, which are 2056 matches (with score of 2.5 or above). Based on the business requirements/ the type of problem, a suitable score thresold could be selected. For this ER problem, a score of 2.5 or above is selected to identify the best matches across the two datasets. 

7. Final Mapping: Based on the thresold score of 2.5 or above, 2056 record matches are mapped. The results are merged and a new column 'Match_ID' based on Scholar_match and DBLP_Match is created. 

8. Final Step: The output will be stored and pushed into Github repository as a CSV file with a file name 'DBLP_Scholar_perfectMapping_RaviLachireddy.csv'. 


### Constraints and Assumptions

1. Instead of Full indexing on entire datasets, I have used Sorted neighborhood indexing (on title). This is because full indexing of the current datasets was resulting in 16.5 million record pairs. The training of those record pairs will be computationally expensive and time-consuming. The use of title column and sorted neighborhood was appropraite in the present context as it lead to better accuracy within my resources. 

2. Classification algorithms could be used for classifying into matches, non-matches and possibe matches if there is training data or if we could use this data as training data for future models. 

3. While, the current model address the issue of de-duplication of records it may have some matches with thresold score of 2.5. To address this issue, we could de-duplicate each dataset initially separately and then we could build our Entity resolution model. However, due to computational limitations and time constraints I have directly employed record linkage on both datasets which also addresses the de-duplication problem to great extent.


### Libraries Used 

1. Pandas
2. RecordLinkage
3. numpy


### Alternative Perspective

1. The problem could be also solved using Dedupe library, which performs deduplication and entity resolution on structured data. 
2. Entity resolution could be solved using NLP techniques (as a text similarity using TF-IDF, Cosine Similarity). 
