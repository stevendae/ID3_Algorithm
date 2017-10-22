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
| ELEVATION       | low | D6 | d2 | 0.0 | 0.6793 | 0.8774 |
|        | medium | D7 | d3,d4 | 1.0 |  |  |
|       | high | D8 | d1,d5,d7 | 0.9183 |  | |
|        | highest | D9 | d6 | 0.0 |  |  |

Elevation has the largest information gain of the three features and so is selected by the algorithm at the root node of the tree. D6 and D9 are pure sets, and these partitions can be converted into leaf nodes. The D7 and D8 partitions, however, contain instances with a mixture of target feature levels, so the algorithm needs to continue splitting these partitions. The next descriptive feature is selected using the same procedure of determining which feature has the highest information gain.

Addressing D7 first:

##### H(VEGETATION,D7) = -∑ P(VEGETATION = l) x log2(P(VEGETATION = l)) 
###### l ∈ {chapparal,riparian,conifer}

##### = - ((1/2) x log2(1/2)) + (1/2 x log2(1/2)) + (0/2 x log2(0/2))
##### = 1.0 bits


| SPLIT BY FEATURE    | LEVEL  | PARTITION  | INSTANCES  | PARTITION ENTROPY  | REM.  | INFO GAIN  |
| ------------- |:-------------:|:-------------:|:-------------:|:-----:|:-------------:|:-----:|
| STREAM       | true | D10 | d3 | 0.0 | 0.0 | 1.0 |
|        | false | D11 | d4 | 0.0 |  |  |
| SLOPE       | flat | D12 |  | 0.0 | 1.0 | 0.0 |
|        | moderate | D13 |  | 0.0 |  |  |
|        | steep | D14 | d3,d4 | 1.0 |  |  |

Stream has highest info gain. D10 and D11 partition into pure sets. D12 and D13 have no instances, an D14 has two where they can be partitioned for STREAM. 

Perform same method for D8. You will arrive to a situation where for a moderate slope, there are no instances that have the results of the descriptive features and outputs a target. Data does not exist. Hence it returns the chapparal target level because chapparal is the majority target level in the partition at the parent node (D8) of this leaf node. 

This example illustrates one way in which the predictions made by the model generalize beyond the dataset. Whether generalizations made by the model are correct will depend on whether the assumptions used in generating the model (i.e. the inductive bias) are appropriate. 

The ID3 algorithm works in exactly the same way for larger, more complicated datasets; there is simply more computation involved. Since it was first proposed, there have been many modifications to the original ID3 algorithm to handle variations that are common in real-world datasets. 

### Extensions and Variations

Entropy-based information gain, however, does have some drawbacks. In particular, it preferences features with many levels because these features will split the data into many small subsets, which will tend to be pure, irrespective of any correlation between the descriptive feature and the target feature.

One way of addressing this issue is to use information gain ratio instead of entropy. The information gain ratio is computed by dividing the information gain of a feature by the amount of information used to determine the value of the feature.

Information gain has the advantage that it is computationally less expensive than information gain ratio. If there is variation across the number of values in the domain of the descriptive features in a dataset, however, information gain ratio may be a better option. These factors aside, the effectiveness of descriptive feature selection metrics can vary from domain to domain. So we should experiment with different metrics to find which one results in the best models for each dataset.

Another commonly used measure of impurity is the Gini index:











