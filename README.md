# Iterative Dichotomizer 3 (ID3) algorithm

Algorithm designed to create the shallowest decision tree that is consistent with data given.
The ID3 algorithm builds the tree by:
- recursive
- depth-first
- choose best descriptive feature to test (the question that provides the most information aka reduces heterogeneity) 
- compute the **information gain** of the descriptive features in the training dataset

Ecological Modeling Example:


| ID        | STREAM           | SLOPE  | ELEVATION  | VEGETATION  |
| ------------- |:-------------:|:-------------:|:-------------:|-----:|
| 1       | false | steep | high | chapparal |
| 2       | true | moderate | low | riparian |
| 3       | true | steep | medium | riparian |
| 4       | false | steep | medium | chapparal |
| 5       | false | flat | high | conifer |
| 6       | true | steep | highest | conifer |
| 7       | true | steep | high | chapparal |

**Prediction Task:** classify the type of vegetation that is likely to be growing in areas of land based only on descriptive features extracted from maps of the areas. 

First step in building decision tree is to determine which of the three descriptive features is the best one to split the dataset on at the root node. Algorithm does this by computing the information gain for each feature. The total entropy for this dataset, which is required to calculate information gain, is computed as 

##### H(VEGETATION,D) = -∑ P(VEGETATION = l) x log2(P(VEGETATION = l)) 
###### l ∈ {chapparal,riparian,conifer}

##### = - (((3/7) x log2(3/7)) + (2/7 x log2(2/7)) + (2/7 x log2(2/7)))
##### = 1.5567 bits

| SPLIT BY FEATURE    | LEVEL  | PARTITION  | INSTANCES  | PARTITION ENTROPY  | REM.  | INFO GAIN  |
| ------------- |:-------------:|:-------------:|:-------------:|:-----:|:-------------:|:-----:|
| STREAM       | true | D1 | d2,d3,d6,d7 | 1.5000 | 1.2507 | 0.3060 |
|        | false | D2 | d1,d4,d5,d7 | 0.9183 |  |  |
| SLOPE       | flat | D3 | d5 | 0.0 | 0.9793 | 0.5774 |
|        | moderate | D4 | d2 | 0.0 |  |  |
|        | steep | D5 | d1,d3,d4,d6,d7 | 1.3710 |  |  |
| ELEVATION       | true | D1 | d2,d3,d6,d7 |  | 1.2507 | 0.3060 |
|        | false | D2 | d1,d4,d5,d7 | 0.9183 |  |  |
| STREAM       | true | D1 | d2,d3,d6,d7 | 1.5000 | 1.2507 | 0.3060 |
|        | false | D2 | d1,d4,d5,d7 | 0.9183 |  |  |
