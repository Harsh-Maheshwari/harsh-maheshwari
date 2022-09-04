## Components of Machine Learning

Deep learning has helped beyond end-to-end training, we are experiencing a transition from 
parametric statistical descriptions to fully nonparametric models. When data are scarce, 
one needs to rely on simplifying assumptions about reality in order to obtain useful models. 
When data are abundant, this can be replaced by nonparametric models that fit reality more accurately. 

The Core components of a machine learning problem (like Regression, Classification, Tagging, Search/Ranking, 
Recommender Systems, Sequence Learning [Tagging and Parsing, Automatic Speech Recognition, Text to Speech, 
Machine Translation], Reinforcement Learning) always fall to the components described below 
> 1. The **data** that we can learn from.
> 2. A **model** of how to transform the data.
> 3. An **objective function** that quantifies how well (or badly) the model is doing.
> 4. An **algorithm** to adjust the model’s parameters **to optimize** the objective function.

### Data
A dataset generally has information in the form of examples [Rows] and features [Columns]. 

**Dimensionality** of data : If every example is characterized by the same number of numerical values i.e data has 
fixed-length feature vectors then this length is the dimensionality of that data.

**Amount** of Data: As the amount of data increases chances for a good model generally increases and the amount of assumptions made decreases
Many of the most exciting models in deep learning do not work without large datasets.
If the amount of data is small then the results generally fall and are similar to traditional machine learning and Numerical approaches.

**Quality** of Data ``garbage in, garbage out``: Lots of data is a guarentee of nothing unless it is the right 
data. What is right kind of data? Data without mistakes or human error, features present should be predictive of 
the target quantity of interest, correct representation of the reality without under/over representation of 
particular group(s). In sensitive applications of machine learning, like predictive policing, resume screening,
and risk models used for lending, we must be especially alert to the consequences of garbage data. Failure can 
also occur when the data do not merely under-represent some groups but reflect societal prejudices bias. 
Note that this can all happen without the data scientist actively conspiring, or even being aware.

#### Different types of Data
1. Structured, unstructured, semi-structured
2. Time-stamped data
3. Machine data 
4. Spatiotemporal data
5. Genomics data
6. Qualitative/Categorical: Nominal, Ordinal
7. Quantitative : Discrete [Interval], Continuous [Ratio]   

* Nominal Data is observed but not measured, is unordered but non-equidistant, and have no meaningful zero
* Ordinal Data is observed but not measured, is ordered but non-equidistant, and has no meaningful zero.
* Interval Data are measured and ordered with the nearest items but have no meaningful zero.
* Ratio Data are measured and ordered with equidistant items and a meaningful zero 
* If ordinal classes are numbered then is it discrete or ordinal? It is still ordinal. Even if the numbering is done, it doesn’t convey the actual distances between the classes.

### Model
Models are the computational machinery for ingesting data of one type, and spitting out predictions of a possibly 
different type. In particular, deep learning uses statistical models that can be estimated directly from data.

#### Different types of Models 

**Regression Algorithms**: 
Ordinary Least Squares Regression (OLSR),
Linear Regression,
Logistic Regression,
Stepwise Regression,
Multivariate Adaptive Regression Splines (MARS),
Locally Estimated Scatterplot Smoothing (LOESS)

**Instance-based Algorithms**:
k-Nearest Neighbor (kNN),
Learning Vector Quantization (LVQ),
Self-Organizing Map (SOM),
Locally Weighted Learning (LWL),
Support Vector Machines (SVM)

**Regularization Algorithms**:
Ridge Regression,
Least Absolute Shrinkage and Selection Operator (LASSO),
Elastic Net,
Least-Angle Regression (LARS)

**Decision Tree Algorithms**:
Classification and Regression Tree (CART),
Iterative Dichotomiser 3 (ID3),
C4.5 and C5.0,
Chi-squared Automatic Interaction Detection (CHAID),
Decision Stump,
M5,
Conditional Decision Trees

**Bayesian Algorithms**:
Naive Bayes,
Gaussian Naive Bayes,
Multinomial Naive Bayes,
Averaged One-Dependence Estimators (AODE),
Bayesian Belief Network (BBN),
Bayesian Network (BN)

**Clustering Algorithms**:
k-Means,
k-Medians,
Expectation Maximisation (EM),
Hierarchical Clustering

**Dimensionality Reduction Algorithms**:
Principal Component Analysis (PCA),
Principal Component Regression (PCR),
Partial Least Squares Regression (PLSR),
Sammon Mapping,
Multidimensional Scaling (MDS),
Projection Pursuit,
Linear Discriminant Analysis (LDA),
Mixture Discriminant Analysis (MDA),
Quadratic Discriminant Analysis (QDA),
Flexible Discriminant Analysis (FDA)

**Ensemble Algorithms**:
Bootstrapped Aggregation (Bagging),
Random Forest, AdaBoost, XGBoost, CatBoost,LightGBM, 
Weighted Average (Blending),
Stacked Generalization (Stacking),
Gradient Boosting Machines (GBM),
Gradient Boosted Regression Trees (GBRT),

**Association Rule Learning Algorithms** :
Apriori algorithm,
Eclat algorithm

**Artificial Neural Network Algorithms**:
Perceptron,
Multilayer Perceptrons (MLP),
Back-Propagation,
Stochastic Gradient Descent,
Hopfield Network,
Radial Basis Function Network (RBFN)

**Deep Learning Algorithms**:
Convolutional Neural Network (CNN),
Recurrent Neural Networks (RNNs),
Long Short-Term Memory Networks (LSTMs),
Stacked Auto-Encoders,
Deep Boltzmann Machine (DBM),
Deep Belief Networks (DBN),
Attention networks,
Transformers


### Objective Functions
Objective functions define a formal mathematical system measures of how good (or bad) a models is at learning. 
Note learning means improving at some task over time. Loss functions is another name for Objective Functions.

* Most common loss function to predict numerical values is **squared error**. 
* Most common loss function for classification is to minimize **error rate**
* **Training Data** : It is used to learn the best values of our model’s parameters by minimizing the loss incurred **[Objective Function]** on a sampled set consisting of some number of examples collected from the data for training 
* **Testing Data** : It is used to test the models performance **[Using Objective Function]** on a sampled set consisting of some unseen examples collected from the data which were not used in traning  

Results of a model should always be told for both traning and testing data. When a model performs well on the training set but fails to generalize to unseen data, it is said to be overfitted
#### Different types of Objective Fuctions


### Optimization Algorithms
Algorithms are used for searching the best possible parameters for minimizing the loss function. 
Popular optimization algorithms for deep learning are based on an approach called gradient descent.
#### Different types of Optimization Algorithms



Please have a look 
@ [D2L](https://d2l.ai/chapter_introduction/index.html#key-components), 
@ [Wiki](https://en.wikipedia.org/wiki/Category:Machine_learning_algorithms)
and 
@ [machinelearningmastery](https://machinelearningmastery.com/a-tour-of-machine-learning-algorithms/)
 for a much more detail overview of the topic 
