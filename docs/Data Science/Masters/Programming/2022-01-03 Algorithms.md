---
title: Algorithms
date: 2022-04-24 12:40:24
description:
tags: 
 - introduction_to_python
icon: material/emoticon-happy
status: [Todo|Inprogress|Published|Arxived]
external_references: 
urls: 
aliases: 
---

## Best resource to learn algorithms

<iframe src="https://scikit-learn.org/stable/user_guide.html" style="width:100%; height:1000px;" ></iframe>

## K Nearest Neighbour
### Failures of KNN
1. If query point is far away from rest of  the points then the algorithm fails 
2.  Jumbled or mixed up data, then there is no useful information and KNN (most algos) fails miserably 
### Distances in KNN
1. Euclidean Distance (L2 Norm)
2. Manhattan Distance (L1 Norm)
3. Minkowski Distance (Lp Norm)
4. Hamming Distance :  Number of dimensions where binary/string vectors differ





## Summaries

	Regression algorithm
	Linear regression
	Logistic regression
	Multiple Adaptive Regression (MARS)
	Local scatter smoothing estimate (LOESS)
	Ordinary Least Squares Regression (OLSR)
	
	Instance-based learning algorithm
	K — proximity algorithm (kNN)
	Learning vectorization (LVQ)
	Self-Organizing Mapping Algorithm (SOM)
	Local Weighted Learning Algorithm (LWL)
	Support Vector Machines (SVM)
	
	Regularization algorithm
	Ridge Regression
	LASSO（Least Absolute Shrinkage and Selection Operator）
	Elastic Net
	Minimum Angle Regression (LARS)
	
	Decision tree algorithm
	Classification and Regression Tree (CART)
	ID3 algorithm (Iterative Dichotomiser 3)
	C4.5 and C5.0
	CHAID（Chi-squared Automatic Interaction Detection(）
	Random Forest
	Multivariate Adaptive Regression Spline (MARS)
	Gradient Boosting Machine (GBM)
	Decision Stump
	Conditional Decision Trees
	M5
	
	
	Classification and Regression Tree (CART)
	Iterative Dichotomiser 3 (ID3)
	C4.5 and C5.0 (different versions of a powerful approach)
	Chi-squared Automatic Interaction Detection (CHAID)
	
	
	Bayesian algorithm
	Naive Bayes
	Gaussian Bayes
	Multinomial Naive Bayes / Polynomial naive Bayes
	AODE（Averaged One-Dependence Estimators）
	Bayesian Network (BN)
	Bayesian Belief Network
	
	Kernel-based algorithm
	Support vector machine (SVM)
	Radial Basis Function (RBF)
	Linear Discriminate Analysis (LDA)
	
	Clustering Algorithm
	K — mean
	K — medium number
	EM algorithm
	Hierarchical clustering
	
	Association rule learning
	Apriori algorithm
	Eclat algorithm
	
	Neural Networks
	Backpropagation algorithm (BP)
	Hopfield network
	Radial Basis Function Network (RBFN)
	
	Deep learning
	Deep Boltzmann Machine (DBM)
	Convolutional Neural Network (CNN)
	Recurrent neural network (RNN, LSTM)
	Stacked Auto-Encoder
	
	Dimensionality reduction algorithm
	Principal Component Analysis (PCA)
	Principal component regression (PCR)
	Partial least squares regression (PLSR)
	Salmon map
	Multidimensional scaling analysis (MDS)
	Projection pursuit method (PP)
	Linear Discriminant Analysis (LDA)
	Mixed Discriminant Analysis (MDA)
	Quadratic Discriminant Analysis (QDA)
	Flexible Discriminant Analysis (FDA)
	
	Ensemble Algorithms/Integrated algorithm
	Boosting
	Bootstrapped Aggregation (Bagging)
	AdaBoost
	Weighted Average (Blending)
	Stacked Generalization (Stacking)
	Gradient Boosting Machines (GBM)
	Gradient Boosted Regression Trees (GBRT)
	Random Forest
	
	Other algorithms
	Feature selection algorithm
	Performance evaluation algorithm
	Natural language processing
	Computer vision
	Recommended system
	Reinforcement learning
	Migration learning



1. Linear Regression: For statistical technique linear regression is used in which value of dependent variable is predicted through independent variables. A relationship is formed by mapping the dependent and independent variable on a line and that line is called regression line which is represented by Y= a*X + b.
Where Y= Dependent variable (e.g weight).
X= Independent Variable (e.g height)
b= Intercept and a = slope.

2. Logistic Regression: In logistic regression we have lot of data whose classification is done by building an equation. This method is used to find the discrete dependent variable from the set of independent variables. Its goal is to find the best fit set of parameters. In this classifier, each feature is multiplied by a weight and then all are added. Then the result is passed to sigmoid function which produces the binary output. Logistic regression generates the coefficients to predict a logit transformation of the probability.

3. Decision Tree: It belongs to supervised learning algorithm. Decision tree can be used to classification and regression both having a tree like structure. In a decision tree building algorithm first the best attribute of dataset is placed at the root, then training dataset is split into subsets. Splitting of data depends on the features of datasets. This process is done until the whole data is classified and we find leaf node at each branch. Information gain can be calculated to find which feature is giving us the highest information gain. Decision trees are built for making a training model which can be used to predict class or the value of target variable.

4. Support vector machine: Support vector machine is a binary classifier. Raw data is drawn on the n- dimensional plane. In this a separating hyperplane is drawn to differentiate the datasets. The line drawn from centre of the line separating the two closest data-points of different categories is taken as an optimal hyperplane. This optimised separating hyperplane maximizes the margin of training data. Through this hyperplane, new data can be categorised.

5. Naive-Bayes: It is a technique for constructing classifiers which is based on Bayes theorem used even for highly sophisticated classification methods. It learns the probability of an object with certain features belonging to a particular group or class. In short, it is a probabilistic classifier. In this method occurrence of each feature is independent of occurrence another feature. It only needs small amount of training data for classification, and all terms can be precomputed thus classifying becomes easy, quick and efficient.

6. KNN: This method is used for both classification and regression. It is among the simplest method of 
machine learning algorithms. It stores the cases and for new data it checks the majority of the k neighbours with which it resembles the most. KNN makes predictions using the training dataset directly.

7. K-means Clustering: It is an unsupervised learning algorithm used to overcome the limitation of clustering. To group the datasets into clusters initial partition is done using Euclidean distance. Assume if we have k clusters, for each cluster a centre is defined. These centres should be far from each other, and then each point is examined thus added to the belonging nearest cluster in terms of Euclidean distance to nearest mean, until no point remains pending. A mean vector is re-calculated for each new entry. The iterative relocation is done until proper clustering is done. Thus for minimizing the objective squared error function process is repeated by generating a loop.
Final results of the K-means clustering algorithm are:
1. The centroids of the K clusters, which are used to label new entered data.
2. Labels for the training data .

8. Random Forest: It is a supervised classification algorithm. Multiple number of decision trees taken together forms a random forest algorithm i.e the collection of many classification tree. It can be used for classification as well as regression. Each decision tree includes some rule based system. For the given training dataset with targets and features, the decision tree algorithm will have set of rules. In random forest unlike decision trees there is no need to calculate information gain to find root node. It use the rules of each randomly created decision tree to predict the outcome and stores the predicted outcome. Further it calculates the vote for each predicted target. Thus high voted prediction is considered as the final prediction from the random forest algorithm.

9. Dimensionality Reduction Algorithms: It is used to reduce the number of random variables by obtaining some principal variables. Feature extraction and feature selection are types of dimensionality reduction method. It can be done by PCA, Principal component analysis is a method of extracting important variables from large set of variables. It extracts the low dimensionality set of features from high dimensional data. It is used basically when we have more than 3 dimensional data.

10. Gradient boosting and Ada Boost Algorithms : Gradient boosting algorithm is a regression and classification algorithm. AdaBoost only selects those features which improves predictive power of the model. It works by choosing a base algorithm like decision trees and iteratively improving it by accounting for the incorrectly classified examples in the training set. Both of algorithms are used for the boosting of the accuracy of predictive model.