# Introduction to Ritter's dataset

dataset available at http://github.com/aritter/twitter_nlp
paper available at https://homes.cs.washington.edu/~mausam/papers/emnlp11.pdf

## Problems identified
Two problems making it difficult to conduct named-entity recognition on Tweets are: 1. Large amounts of distinctive named entity types, which are of low frequency and make it difficult to find training examples even with a large sample of manually annotated tweets. 2. Not enough background contexts are provided within the 140 Twitter character limits to determine the type of the entity. 

## Approach applied
The NER of Ritter is divided into two tasks: 1. Use discriminative methods for segmenting named entities and 2. use distantly supervised approaches for classifying named entities. Due to the large amounts of distinctive named words, Ritter used a randomly sampled set of 2,400 tweets for NER to generate a larger annotated dataset to further effectively learn a model of named entities.

### 1. Use discriminative methods for segmenting named entities
Capitalization in Tweets is less informative than in formal news, so in-domain data is needed to train models which rely less heavily on capitalization, and also are able to utilize features provided by T-CAP which is a capitalization classifier(copy, needs editing). The set of 2,400 tweets(34K tokens) has been exhaustively annotated without applying the convension of treating @usernames as entities. 

T-SEG models Named Entity Segmentation as a sequence-labeling task using IOB encoding for representing segmentations (each word either begins, isinside, or is outside of a named entity), and uses Conditional Random Fields for learning and inference. Again we include orthographic, contextual and dictionary features; our dictionaries included a set of type lists gathered from Freebase. In addition, we use the Brown clusters and outputs of T-POS(the POS tagging system), T-CHUNK(the task of identifying non-recursive phrases) and T-CAP in generating features.

### 2. use distantly supervised approaches for classifying named entities
To solve the problem of large amounts of distinct words, large lists of entities andtheir types gathered from an open-domain ontology(Freebase) are used as a source of distant supervision, allowing use of large amounts of unlabeled data in learning.

Distant Supervision with Topic Models is applied for classifying named entities: LabeledLDA models each entity string as a mixture of types rather than using a single hidden variable to represent the type of each mention. Each mention is related to a bag of types compared to one single type. For example, θAmazon could correspond to a distribution over two types: COMPANY, and LOCATION, whereas θApple might represent a distribution over COMPANY, and FOOD. 

To make predictions, an informative Dirichlet prior based on θtrain e is performed for each entity and 100 iterations of Gibbs Sampling are applied holding the hidden topic variables in the training data fixed.