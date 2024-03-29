# Book_Recommendation_System_Project

Team project by Erika D'Auria & Jennifer McKaig

## The Goal: 
To recommend books to users that they will enjoy based on insights gained from available user generated data.

## The Data: 
Goodreads 10,000 most popular books rated by 53,424 people. 

Files used: 
  - ratings.csv
  - book_tags.csv
  - tags.csv
  - books.csv

https://github.com/zygmuntz/goodbooks-10k

# Data Preprocessing & Exploratory Data Analysis

We merged the book_tags, tags, and books files and created a new dataframe with features:
  - book_id 
  - tag_name
  - author
  - title
  - language_code
  - average_rating
   
The shape of the merged data is much larger than the shape of the ratings data, due to duplicates. 
 - Merged data size:  (999912, 6)
 - Ratings data size:  (5976479, 3)

The duplicates provide identical user_id and book_id data, as well as individual ratings and individual tags for each book. Since we are using the ratings from a separate data source, we don't need the individual ratings. We wrote a function to find the most common tag given to each book. This common tag was added as a feature and the duplicates were dropped along with the tag_names feature. 

![Most_common_tag.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Most_common_tag.png)

We kept the ratings csv data as the primary data and left merged the tag data. 

The ratings are given on a scale of 1 through 5. The data shows that users tend to review the books that they like more than the ones that they do not.

    Counter({4: 2139018, 5: 1983093, 3: 1370916, 2: 359257, 1: 124195})

![Book_ratings_count.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Book_ratings_count.png)

The ratings per book show that a small minority of books are reviewed exponentially more than the average.

![Reviews_per_book.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Reviews_per_book.png)
  
    Most reviewed book:  The Hunger Games (The Hunger Games, #1), 217482 reviews
  
    Least reviewed book: Kindle User's Guide,  8 reviews

The number of books reviewed by users is evenly spread.

![Reviews_per_person.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Reviews_per_person.png)

The most popular tags on goodreads show that tagging is used for personal classification, rather than naming a specific genre. 

![Tag_counts.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Tag_counts.png)

Some of the most popular books on goodreads are series. 

![Title_count.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Title_count.png)
  

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

![Similarity_metrics.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Similarity_metrics.png)

![Basic_knn.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/Basic_knn.png)

![KNN_with_Means.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/KNN_with_Means.png)

![KNN_Baseline.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/KNN_Baseline.png)

The memory-based collaborative model that produced the lowest RMSE of __0.8370__ was the __KNN Baseline model__ utilizing a __pearson similarity metric__. This means, when this model is used to predict how a user will rate a title, it will be off by 0.8370 on average.

To see if a smaller RMSE could be produced another memory-based model was tested.

## Model-Based (Matrix Factorization) Collaborative Filtering

Next, a Singular Value Decomposition (SVD) model was tested.

![SVD.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/SVD.png)

The __SVD model__ produced an even lower RMSE of __0.8299__. This means, when this model is used to predict how a user will rate a title, it will be off by 0.8299 on average. This was the best of the collaborative filtering models tested in this project.

Below is an example of how this model predicted how one user would rate a book.

![SVD_example.png](https://github.com/erikajane/Book_Recommendation_System_Project/blob/master/Images/SVD_example.png)

The SVD model predicted that the user, with __user_id = 196__ would rate the book, with __book_id = 302__, 3.92 stars. This person had actually rated it 4 stars. The difference in prediction was 0.08 which is lower than the RMSE. This model had done very well in predicting how a user would rate a title in this example.

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