## Risk Prediction in Life Insurance Industry

### 1. Motivation!
**Business Context:**
Insurance companies that underwrite life insurance policies have to evaluate applications carefully due to the high payouts if a claim rises. 

Traditionally, actuaries evaluate life insurance applications mannually using formulae based rules and actuaries' experiences. It is common for these applications to take over a month from the date of application to be issued.

**Task:**
Can you design an automated program to predict the risk level of an insurance buyer to reduce the processing time?

### 2. Data Pre-processing
Below is how the data set looks like:

<img src="images/insurance/variables.png?raw=true"/>

Also, we noticed that there's a big part of data missing 

<img src="images/insurance/missing.png?raw=true"/>


We dropped the variables with more than 30% missing first. 

Then we conducted a hypothesis test (little's test), confirmed that the rest of the missing data is MAR (missing at random). Based on the missing mechanism, we imputed the rest of the missing data by MICE.

Now, we cleaned our data!

Next, we want to reduce the dimensions of our data. 

We experimented two ways to select and extract features - Principle Component Analysis (PCA) and Correlation-based Feature Selection (CFS).

### 3. Models
In this project, for each data set (PCA and CFS), we explored applying logistic regression, a single layer artifitial neural network, REP tree and random tree.

**Logistic Regression:** a natural choice for a multi-class classification problem!

**Artificial Neural Network:** inspired by how animal brains operate biologically. It consists of three main parts, an input layer, hidden layers and an output layer, and each layer contains interconnected nodes, which are also known as perceptrons. A perceptron is similar to a neuron in animal brains, but here it simply takes a signal, performs a multiple linear regression, then puts the result into an activation function, and passes it to the next layer. It is powerful as they can recognize underlying relationships in a vast amount of data.

**Reduced Error Pruning Tree (REP tree):** A single tree model, which makes use of the “regression tree logic to create trees in different iterations, and it develops its model by information gain and variance deduciton. This type of decision tree only creates a single tree and it is pruned afterwards based on weight decay in order to avoid overfitting.

**Random Tree:** The idea of Random Tree is simple: it uses only a subset of the attributes in the dataset to build trees. In general, this type of decision tree is significantly efficient when the dataset is large.


### 4. Results
Below is a comparison of the algorithms:

<img src="images/insurance/result.png?raw=true"/>


For this dataset, CFS is a better dimensionality reduction method than PCA as it results in lower MAE as well as RMSE across all learning algorithms. REPTree results in the same values of MAE and RMSE for both methods but for other algorithms, CFS turns out to be better than PCA.

Within CFS, Neural Networks yield the lowest MAE and RMSE and within PCA, it’s again Neural Networks. So overall, Neural Networks is the best supervised learning algorithm for this dataset.

