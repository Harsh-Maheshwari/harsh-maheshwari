**Regression** refers to a set of methods for modeling the relationship between one or more independent variables and a dependent variable. 

#### Assumptions in Linear Regression
we assume that the relationship between the independent variables x and the dependent variable y is linear, i.e., that y can be expressed as a weighted sum of the elements in x, given some noise on the observations. 
Second, we assume that any noise is well-behaved (following a Gaussian distribution).

#### Governing Equations
* $\hat{y} =  w_{1}x_{1} + ... + w_{d}x_{d} + b$ --- scaler form
* $\hat{y} = \mathbf{w^Tx} + b$ --- vector form

If the Design matrix $\mathbf{X}$ contains one row for every example and one column for every feature then 

* $\mathbf{\hat{y}} = \mathbf{Xw} + b$ 

Given features of a training
dataset $\mathbf{X}$ and corresponding (known) labels $\mathbf{y}$, the goal of linear regression is to find the weight vector $\mathbf{w}$ and the bias term $\mathbf{b}$ that given features of a new data example sampled from the same distribution as $\mathbf{X}$, the new exampleʼs label will (in expectation) be predicted with the lowest error.

#### Loss Function

Before we start thinking about how to fit data with our model, we need to determine a measure of
fitness. The loss function quantifies the distance between the real and predicted value of the target. The loss will usually be a non-negative number where smaller values are better and perfect predictions incur a loss of 0. The most popular loss function in regression problems is the squared error. When our prediction for an example i is $\hat{y}^{(i)}$ and the corresponding true label is $y^{(i)}$, the squared error is given by :

* $l^{i}(\mathbf{w},b)=0.5(\hat{y}^{i}-y^{i})^2$

To measure the quality of a model on the
entire dataset of _n_ examples, we simply average (or equivalently, sum) the losses on the training
set.

* $L(\mathbf{w},b) = (1/n)* \sum_{i=1}^{n} 0.5(\mathbf{w^{T}x}^{i}+b-y^{i})^{2}$

When training the model, we want to find parameters $(\mathbf{w}^{*}; b^{*})$ that minimize the total loss across
all training examples:

$\mathbf{w}^{*}; b^{*} = argmin$ $L(\mathbf{w}; b)$

#### Analytic Solution

linear regression can be solved analytically by applying
a simple formula.