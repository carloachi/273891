RECOMMENDER SYSTEM
1) Team members
Alessia Bresolin (275601)
Raffaele Girardi (265131)
Alessandro Ivashkevich (276871)
2) Introduction
What does our project consists in? We have identified ourselves into members of the data science team of a prestigious fashion firm. Our primary aim is to boost the revenues of the company using any possible legal mean. Our online platform does not suffer any particular issue. However, we want to improve our recommender system. In this project, we will test different systems and select the most efficient one.

3) Methods
We have been proposed 3 datasets containing information for: customers, transactions and articles. We have analyzed all of them through a deep explanatory data analysis.

a) Data Understanding
'recsys_customers.csv': customer id associated to age and optional participation to fashion news and club member.
'recsys_articles.csv': article id associated to the name of the product, colour of the product (from very detailed to very general), department, group of membership, section and type of garment.
'recsys_transactions.csv': purchase done by a customer on a certain date
b) Data Preparation
The output of this first step is a clean and more comprehensible dataset. This stage helps us to have a better insight of our data, especially about the relationship between customers and transactions. We have identified all the missing values and highlighted some important features of each dataset.

Deal with Null values in 'recsys_customer';
Create additional dataframes;
Deal with Null Values in 'recsys_article' by removing them;
c) EDA
Explanatory Data Analysis is a fundamental process in Machine Learning for data preparation. It is key to understand the intrinsic features of our datasets by applying different Python packages:

Pandas;
Numpy;
Sklearn;
Matplotlib.pyplot;
Seaborn.
Customers
We have data for more than 40 thousands customers. For each of them, we have records of their identifier, if they are subscribed to fashion news, if they have a club membership and their age. Through EDA we have noticed that there was a small percentage of Null values (0.32%) in the age column. We decided to replace them by the value of the mode of the age of all the customers.

We have grouped customers into age categories, calculated how many people belong to each of them and calculated the proportion of people who is subscribed to either the club membership or fashion news.

Club Membership!

Fashion News!

Transactions
For transactions, we have a file that stores all the purchases made by each customer and the date in which they made it. We calculated the number of transactions per customer and the maximum value of purchaess made by a single customer is 104 transactions. To get a better insight of the dataset, we have grouped all the transactions in classes and we noticed that only a small proportion of customers has done more than 30 transactions. We finally calculated also the number of customers that has bought each product.

Transaction Groups!

Articles
6536 articles are stored in our dataset, with all their characteristics. Each product belongs to many categories: they have a garment group, a section, a deparment, an index group and a type.

We use the Explanatory Data Analysis to look for the best identifier for all the products. We have decided to discard the more precise classes to avoid overfitting and also too general ones because they may lead to an inaccurate recommender system. We finally decided to take into consideration only the articles' type and section. Another attribute of the articles is the colour. Again, we have taken into consideration only the 'perceived_colour_master' which was, for us, the best middle way between dealing with overfitting and having a precise model.

Furthermore, we have noticed the presence of 'Unknown' items, which we have deleted because they represented just a small proportion of the data.

articles!

4) Recommender System
Our recommender system generates a list of articles that a customer might be interested in, basing on its actual purchases and on the similarities with other users.

a) Content filtering
The first type of recommendation system we have applied is: content filtering recommender system. The final suggestions are shaped on the basis of features of the products, in our case their color, group and type. Obviously, we could do the same, using all the other properties of the articles.

One Hot Encoding
We create a one hot encoding for the colour value of the articles. We do the same for the section and the type of the product. We obtain 3 tables with ID of articles as rows and all the features as columns. Inside the tables, the value of a cell will be 1 if a specific article will have a specific feature. We finally concatenate all the tables.

Cosine Similarity
To evaluate the similarity between products, we use cosine similarity, which avails of the cosine angle between two vectors. Clearly, the smaller the angle, the higher the degree of similarity.

Final step
We are now ready to return the 10 recommended items for the input product, identified by its ID. We have noticed that there is the possibility to receive as recommended item, the same output that the funnction gets as input. This is not a problem because a user might decide to buy again the same product. Anyway, we inserted a function that checks for this issue and in the case in happens, the recommender system will remove the input item and add the 11th item in the rank in the output.

images!

b) Collaborative filtering: CRS matrix
The second type of recommendation system we have applied is: user-based filtering recommender system. This kind of system is based on the idea of considering customers' opinions on the different products and suggest an article based on other purchases of the client and purchases and opinions of customers that are similar to him. Basically, in this case, we use information collected by different customers to recommend products to the actual customer.

CSR Matrix
We began by creating a matrix with 'customer_id' and 'article_id' in order to map all the transactions that have occured in the dataset. However, the matrix will have plenty of unobserved elements because each user purchases only a small amount of products with respect to the entirety of them. We calculated the sparsity of the matrix and we noticed its value was way too small to consider it to make reliable predictions.

Increase sparsity
To limit this issue, we decided to consider only a part of the dataset. We discarded all the columns of the customers who bought less than a certain amount of products. Similarly, we discarded some of the rows that corresponded to the products that have been bought less.

KNN
We imported KNN from 'sklearn' library. The algorithm finds clusters of similar customers based on common transactions, and makes predictions using the average rating of top-k nearest neighbors. To evaluate the similarity, we use cosine similarity: the KNN algorithm will measure the distance to determine the “closeness” of instances.

images2!

c) Collaborative filtering: neural network
The last method we used for the recommender system is the artificial neural network. In this case, we generate recommendations based on the similarity between users’ transactions, rather than the similarity of customers and articles (done through the utility matrix).

Matrix factorization
Matrix factorization is a way to understand the features or information underlying the interactions between customers and articles. To find similarities and make predictions we check for the association between customers and articles matrices.
It works by decomposing the user-item interaction matrix into the product of two lower dimensionality rectangular matrices. We do this to counter the issue of sparsity because most customers buy only a minor set of customers.

Loss function
We use the loss function to look for the performance of our model and try to minimize the error. To optimize the perfromance of our function, we use the MSE and two regularization coefficients, which are L2 and the gravity term.
loss_function!

Train and test set To evaluate the performance of our recommendations, we split our dataset into test and train set and we plot the loss function. Our loss function in not perfect, but in our case the value of the amount of transactions represented on the y axis is pretty small and difficult to predict. We have played with our hyperparameters to try to settle this issue.

Final scores We are finally ready use the neural network for our recommendation system. We either use the dot product or cosine similarity to rate it adn we retrieve the top 5 recommendation results.

Final output and personal considerations
Our recommender engine has been implemented to boost the revenues of our company by predicting user preferences and recommending the right products to each user. We have applied different kinds of recommendations using content-based and collaborative filtering systems.

For what concerns our first model, the content-based one, the model is pretty accurate because we have many details regarding all the products. For instance, we can choose to use the department instead of the group to have an higher precision or the 'perceived_colour_value' instead of the 'perceived_colour_master'. We can use them as sort of hyperparameters to fit better our recommendations.

Instead, we have built the second model using collaborative filtering. We have used the CRS matrix, which is used in python to represent sparse matrices. Unfortunately, we believe this model was not suitable for our dataset. We have too many zero values and in order to get a decent level of sparsity we would need to reduce too much both the number of articles and of customers. With the initial dataset we have a sparsity of 0.11%, which we were able to enhance until reaching approximately a vaue of 0,6%. Probably, we could even rich a higher value but this action would lead to a further loss of information, which is not good for our user recomendation system.

The last method we tried to implement was the artificial neural network. This method was really hard to implement for us because we had to play a lot with all the parameters to check which values were the ones that better fitted our dataset.

A major complication we had to face in our models was the fact that we did not have any rating for a product, but we had to make use of the amount of transactions to make reccomendations between users. Their values are clearly small because most of the customers have purchased a low amount of products. Anyway we were able to create three different recommender systems, which our company can exploit to yield higher revenues.
