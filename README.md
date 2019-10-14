# Book_Recommendation_System_Project

# Collaborative-Filtering Methods

The collaborative-filtering methods used will attempt to predict how a user will rate a title with a five-star rating system.

For collaborative filtering methods only three features are utilized by the models used here, __user_id__, __book_id__, and __rating__. All other features have been dropped from the dataset used in the following models.

## Memory-Based/Neighborhood-Based Collaborative Filtering

The following memory-bases/neighborhood-based collaborative filtering models will be employed:

 - Basic KNN
 - KNN with Means
 - KNN Baseline
 
 When running each model, similarity must be measured through item-item similarity or user-user similarity. Since there were significantly less items than users, item-item similarity was utilized as this saved computation time.
 
 Each one of the methods listed above were run twice, each time using a different similarity metric, cosine similarity or pearson similarity.
 
 Root Mean Squared Error (RMSE) was used as an evaluation metric for how well each model predicted what users would rate an item.

![Similarity_metrics.png]()

![Basic_knn.png]()

![KNN_with_Means.png]()

![KNN_Baseline.png]()

The memory-based collaborative model that produced the lowest RMSE of __0.8370__ was the __KNN Baseline model__ utilizing a __pearson similarity metric__. This means, when this model is used to predict how a user will rate a title, it will be off by 0.8370 on average.

To see if a smaller RMSE could be produced another memory-based model was tested.

## Model-Based (Matrix Factorization) Collaborative Filtering

Next, a Singular Value Decomposition (SVD) model was tested.

![SVD.png]()

The __SVD model__ produced an even lower RMSE of __0.8299__. This means, when this model is used to predict how a user will rate a title, it will be off by 0.8299 on average.

Below is an example of how this model predicted one user would rate a book.

![SVD_example.png]()

The SVD model predicted that the user, with __user_id = 196__ would rate the book, with __book_id = 302__ 3.92 stars. This person had actually rated it 4 stars. The difference in prediction was 0.08 which is lower than the RMSE. This model had done very well in predicting how a user would rate a title in this example.

# Content-Based Recommendations

The content-based recommendation system will take an input title and recommend the top ten most similar items based on author name and tag name.

Since each title was assigned the most commonly used tag name for that book, each record of the same book had the same tag as well as the same author name. To save compuation time, duplicates were dropped. The size of the dataset was reduced from 5,976,479 observations, to 9,964 observations. This makes sense as there are 9964 unique titles included in this dataset.

Before employing the vectorizer, the data had to be converted into a signle string. The author's first and last names also needed to be concatenated so that the "Robert" in Robert Galbraith and Robert Jordan were not counted as the same "Robert". Each string was also set to lowercase.

![Words_of_interest_code.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Words_of_interest_code.png)
![Words_of_interest_data_preview.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Words_of_interest_data_preview.png)

Next, a count matrix was computed using __CountVectorizer()__. CountVectorizer was utilized over TF-IDF as each string in the_words_of_interest column were relatively short. Each word was considered equally as important as the others and we did not want some authors or tag names to be have been weighted differently because they were present in the dataset more or less times than others. Using the count matrix a Cosine similarity matrix was able to be computed.

![CountVectorize_CosineSimilarity.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/CountVectorize_CosineSimilarity.png)

Using the Cosine similarity matrix, we created a function that would take an input title and find the top 10 titles most similar to that title.

![Content_Recommendation_Function.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Content_Recommendation_Function.png)

Below are the top 10 recommendations that the recommendation system provided when given "The Hunger Games (The Hunger Games, #1)" as an input title.

![Hunger_Games_Recommendations.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Hunger_Games_Recommendations.png)