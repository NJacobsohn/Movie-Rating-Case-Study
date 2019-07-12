# Recommender Case Study on MovieLens dataset for the Movies

### by:
- Nicolas Jacobsohn,
- Greg Noble,
- Peter Schmeiser,
- Joe Tustin

## Table of Contents
1. [Introduction](#Introduction)
2. [Starting Baseline](#StartingBaseline)
3. [New Proposed Model](#NewProposedModel)
4. [Conclusions](#conclusions)

## Introduction:
Can we beat the predictive ability of a mean of means in rating this movie dataset.  What is a mean of means?   For all users over all movies, you calculate a global average for all movie ratings on a scale of 0 to five.  Guess what: the average global rating is 3.54  Next, you average over each user and then you average over each movie.  These user and movie averages are then averaged with the global mean for each data point in the users vs movie data matrix. For example, user1 has a rating average of 2.55 and movie1("Toy Story") has an average rating of 3.87.  So, our matrix element (1,1) would be the mean of these three values, 3.32  However, it would be more meaningful to fill in values giving more weight to users who show similar trends to the user of interest and to give more weight to movie columns that are similar to the movie column of interest.  This new method of averaging takes into account similarity.

## Starting Baseline:
When you run the existing mean of means recommender model, you get a RMSE (Root Mean Square Error) 1.0173  What does this test metric mean?  For existing recommendations that we have,  the reconstructed recommendation model would be off by a value of 1 on a scale of 0-5 on average for each recommendation that we have in our dataset.  Not that great!!
Let's do better.

## New Proposed Model:
Alternating Least Square (ALS) is a matrix factorization algorithm and it runs itself in a parallel fashion. ALS is implemented in Apache Spark ML and built for a larges-scale collaborative filtering problems. ALS is doing a pretty good job at solving scalability and sparseness of the Ratings data, and it’s simple and scales well to very large datasets.
Some high-level ideas behind ALS are:
ALS minimizes two loss functions alternatively; It first holds user matrix fixed and runs gradient descent with item matrix; then it holds movie matrix fixed and runs gradient descent with user matrix

![](img/mat_fact.png)

Its scalability: ALS runs its gradient descent in parallel across multiple partitions of the underlying training data from a cluster of machines.  It is better because....  It is shown to be the best in the table below of various modeling techniques



**Table 1**: Modeling Methods and results

| Movielens(100k)	| RMSE |	MAE	| Time |
|---:|-----------:|:-----------------------|----:|
|SVD|	0.934	|0.737|	0:00:11|
|SVD++|	0.92|	0.722|	0:09:03|
|NMF	|0.963|	0.758	|0:00:15|
|Slope One|	0.946|	0.743	|0:00:08|
|k-NN|	0.98|	0.774	|0:00:10|
|Centered k-NN	|0.951|	0.749	|0:00:10|
|k-NN Baseline	|0.931|	0.733|	0:00:12|
|Co-Clustering	|0.963|	0.753|	0:00:03|
|Baseline|	0.944|	0.748	|0:00:01|
|Random	|1.514|	1.215	|0:00:01|
|ALS| 0.896|0.696| 0:00:04|

## Results: Example use of recommender

If we look at a random user, let's say User #7:
The three highest rated movies from this user were:
Our new model recommends to watch these three movies:
The old model (mean of means) recommends to watch these three movies:

As another double check on the value of our recommender:
We averaged the new ratings for all movies and the five highest movies were:

Topic 1 Top 10 Movies:
Storefront Hitchcock (1997)    Loading Value: 3.24
Caveman (1981)    Loading Value: 3.24
Funeral in Berlin (1966)    Loading Value: 3.24
Dangerous Beauty (1998)    Loading Value: 3.11
Incendies (2010)    Loading Value: 2.90
Dylan Moran: Monster (2004)    Loading Value: 2.88
Excision (2012)    Loading Value: 2.81
The Lair of the White Worm (1988)    Loading Value: 2.80
Dead Man's Shoes (2004)    Loading Value: 2.80
King Is Alive, The (2000)    Loading Value: 2.74

Topic 2 Top 10 Movies:
Twilight Saga: Breaking Dawn - Part 1, The (2011)    Loading Value: 4.24
Faust (1926)    Loading Value: 3.39
Mirror, The (Zerkalo) (1975)    Loading Value: 2.90
Am Ende eiens viel zu kurzen Tages (Death of a superhero) (2011)    Loading Value: 2.85
Book Thief, The (2013)    Loading Value: 2.85
Last Seduction, The (1994)    Loading Value: 2.84
Psycho II (1983)    Loading Value: 2.83
Homegrown (1998)    Loading Value: 2.73
Amityville II: The Possession (1982)    Loading Value: 2.72
Roger Dodger (2002)    Loading Value: 2.70






## Description of Dataset <a name="Description of Dataset"></a>

The small version of the MovieLens dataset consists of 100,004 ratings ranging from 0 to 5.  There are 9125 movies in the dataset and 671 unique users.





## Conclusions <a name="conclusions"></a>

1. Natural Language Processing is hard!  There is an excessive number of features leading to model complexity.  Using models that reduce or simplify this complexity is key as well as data cleaning measures which simplify the tf-idf matrix as well as the corpus dictionary of words to the most important.

2.  I need to re-address my principal component analysis.  I had extremely bad results which do not match my predictive success rate.  I will have to redo this part.  I would expect to have more of the variance accounted for by the first two principal components.  

3. This project is perfect for cross-validation and grid search analysis.  I would like to incorporate these methods in the future as well as adding one other modeling technique (ie-logistic regression).  Also, I would like to use my amazon account to run the simulation in order to increase the number of data rows as well as the number of features to include trigrams.

4. Feature engineering is key.  I would like to build a dictionary of the fifty most predictive features for both positive and negative results using both single words and bigrams, and then, weight these features more highly than my other features.  And....try, try again.

5. As a side note, check out "Return of the Killer Tomatoes" starring a very young George Clooney.  It received a Rotten Tomatoes score of: "Rotten".  It would be an interesting late night, B-movie to watch.  Would you agree with its rating?  As with all nlp, it is subjective which makes it hard to predict!!!
