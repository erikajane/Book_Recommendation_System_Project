# Book_Recommendation_System_Project

# Content-Based Recommendations

The content-based recommendation system will take an input title and recommend the top ten most similar items based on author name and tag name.

Since each title was assigned the most commonly used tag name for that book, each record of the same book had the same tag as well as the same author name. To save compuation time, duplicates were dropped. The size of the dataset was reduced from 5976479 observations, to 9964 observations. This makes sense as there are 9964 unique titles included in this dataset.

Before employing the vectorizer, the data had to be converted into a signle string. The author's first and last names also needed to be concatenated so that the "Robert" in Robert Galbraith and Robert Jordan were not counted as the same "Robert". Each string was also set to lowercase.

![Words_of_interest_code.png]()
![Words_of_interest_data_preview.png]()

Next, a count matrix is computed using __CountVectorizer()__. CountVectorizer was utilized over TF-IDF as each string in th words_of_interest column were relatively short. Each word was considered equally as important as the others and we did not want some authors or tag names to be have been weighted differently because they were present in the dataset more or less times than others. Using the count matrix a Cosine similarity matrix was able to be computed.

![CountVectorize_CosineSimilarity.png]()

Using the Cosine similarity matrix, we created a function that would take an input title, find the top 10 titles most similar to that title.

![Content_Recommendation_Function.png]()

Below, are the top 10 recommendations that the recommendation system provided when given "The Hunger Games (The Hunger Games, #1)" as an input title.

![Hunger_Games_Recommendations.png]()