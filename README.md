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
