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

