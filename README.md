# Textual Element and User Engagement Impact on Predicting Recipe Ratings

Authors: Daniel Zhu & Christine Law

## Introduction

This site serves as an easily accessible report on our findings regarding the relationship of textual elements on recipe ratings. More specifically, we will be creating a predictive model to determine: **How do the textual elements and user engagement of a recipe (e.g., description, reviews) impact ratings?** This predictive model will provide valuable insight that benefit both content creators and customers. More specifically, content creators can use this information to create better-optimized content that meets their user preferences. On the other hand, consumers will benefit from more engaging, informative, and enjoyable recipe content.

Furthermore, when users read a recipe's description it may promise the 'best' or most 'flavorful' rendition of the food that is being made, but the actual review or rating may shine light on the truth of the recipe's quality. Therefore, we want to create a predictive model that can more accurately predict a recipe's rating and determine which variables from the dataset are able to do so.

Our analysis utilizes two different datasets: one focused on the contents and characteristics of a recipe, and one consisting of the recipes' ratings and reviews. Both datasets contain recipe entries posted since 2008. The recipe dataset consists of 10 variables while the ratings dataset consists of 5 variables, all of which we've listed in more detail below.   

1. `Recipes` (83782 entries)

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe

2. `Ratings` (731927 entries)

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

More specifically, our analysis and predictive model will focus on the columns `'description'`, `'review'`, and `'ratings'`.  

## Data Cleaning and Exploratory Data Analysis
In order to make the given datasets easily accessible for our data science question, we will merge the `'ratings'` and `'recipes'` dataset into one cohesive master dataset.

1. First we will split up the `'nutrition'` column into the respective columns `"[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)]"`

2. We will then merge the two datasets using a left merge on `'recipe_id'`.

3. Then, we will fill in all ratings of 0 with np.nan. In this case, a rating of 0 implies a missing rating for that recipe. Therefore, in order to avoid bias and a potential misconception of a lower and actual average rating, we will fill in all ratings of 0 with np.nan.

4. Next, we will create a Series containing the average rating per recipe and add that data into a new column within our already cleaned dataset. 

Our final cleaned DataFrame contains 234428 rows and 23 columns. Below are the first 5 entries and the columns we find the most relevant to answering our data science question. There will be repeats in regards to the recipe since we do not want to lose the data on their unique reviews despite it being for the same recipe.

| name                                 |     id |      tags | description           |   rating |   average rating |   review	 |
|:-------------------------------------|-------:|----------:|:--------------------|---------:|-----------------:|---------------:|
| 1 brownies in the world    best ever | 333281 |        ['60-minutes-or-less', 'time-to-make', 'course... | these are the most; chocolatey, moist, rich, d... |        4 |                4 |        These were pretty good, but took forever to ba... |
| 1 in canada chocolate chip cookies   | 453467 |        ['60-minutes-or-less', 'time-to-make', 'cuisin.. | this is the recipe that we use at my school ca... |        5 |                5 |          Originally I was gonna cut the recipe in half ... |
| 412 broccoli casserole               | 306168 |        ['60-minutes-or-less', 'time-to-make', 'course... | since there are already 411 recipes for brocco... |        5 |                5 |          This was one of the best broccoli casseroles t... |
| 412 broccoli casserole               | 306168	|        ['60-minutes-or-less', 'time-to-make', 'course... | since there are already 411 recipes for brocco... |        5 |                5 |         I made this for my son's first birthday party ... |
| 412 broccoli casserole               | 306168	|        ['60-minutes-or-less', 'time-to-make', 'course... | since there are already 411 recipes for brocco... |        5 |                5 |          Loved this. Be sure to completely thaw the br...   |

### Univariate Analysis 
For our univariate analysis, we depicted the distribution of ratings, description length, and review length. 

In the distribution showcasing recipe ratings, there seem to be fewer lower ratings than higher ratings, resulting in a positive and upward trend. In other words, the recipes with the least entries are those with the lower ratings.

<iframe src="assets/dist_ratings.html" width="800" height="600" frameborder="0" ></iframe>

In our distribution of description length, we see that the range of the description lengths ranges from 0-400 words and that there are more reviews that tend to be on the shorter and brief side.

<iframe src="assets/dist_desc.html" width="800" height="600" frameborder="0" ></iframe>

Lastly, our distribution of review length showcases similar trends to the description length, with there being a higher frequency in shorter and more concise reviews.

<iframe src="assets/dist_reviews.html" width="800" height="600" frameborder="0" ></iframe>


### Bivariate Analysis 
For our bivariate analysis, we depicted scatterplots showcasing the relationship between 'Description Length vs. Rating' and 'Review Length vs. Rating'. The first graph reveals that a high-ranking recipe tends to have a longer description length compared to those with a lower rating. Similarly, the trend between the 'Review Length vs. Rating' showcases similar results with a recipe with a rank of 4 or 5 having a review length of 300-400 words and more data entries than recipes with lower ratings. This reasoning may be associated with what we will mention in our missingness section, and how reviewers may not leave a review unless they have strong emotions toward the recipe, and in this case, it seems that reviewers who had a positive impression on the recipe were more likely to leave a lengthy review. We will also delve deeper into these relationships in our predictive model.

<iframe src="assets/desc_len_vs_ratings.html" width="800" height="600" frameborder="0" ></iframe>

<iframe src="assets/review_len_vs_ratings.html" width="800" height="600" frameborder="0" ></iframe>


### Interesting Aggregates
For this section, we delved deeper into the relationship between the length of the reviews and the rating the recipe received. In order to do so, we first utilized the groupby method to group by `'review_length_range'` and then `'rating'`. We then aggregated the DataFrame by finding the average rating to produce the table shown below. The significance of this pivot table is that it allows us to see whether or not there is a present and significant correlation associated with the length of one's review and the rating it receives. By doing so, we are analyzing if there is another variable other than TF-IDF values that has the ability to predict the recipe ratings.

| review_length_range        | rating         |
| :------------ | :------------------ |
| 0-50  | 4.691819             |
| 51-100 | 4.682951           |
| 101-150     | 4.622610 |
| 151-200    | 4.564288        |
| 201-250   | 4.493744         |
| 251-300   |4.414634         |
| 301-350   | 4.418182         |
| 351-400   | 4.672727         |

## Assessment of Missingness
The final cleaned dataset we have cultivated contains a few missing values, especially in the columns `'rating'` and `'review'`. Since both of these values are important to our predictive model and analysis, we are going to assess their missingness. 

### NMAR Analysis
Upon analyzing the dataset, we believe that the `'review'` column showcases a missingness of NMAR. NMAR is defined as a column containing missing values dependent on another column in the dataset. Entries that are missing a value in the `'review'` column may be because the individual had no strong emotions toward the recipe itself, unmotivating themselves in leaving a review. This can be reflected by a missing value in the `'rating'` column or even the `'description'` or `'steps'` column. It's possible that the individual gained strong emotions, negative or positive, due to the number of steps of the recipe such as it being too complex or perfectly simple.

### Missingness Dependency
Now, we will more closely examine the `'rating'` column and determine if its missingness is dependent on the missingness of either `'review'` or `'description'`. In order to do so, we will carry out two separate `permutation tests`, each running the permutation test by shuffling the missingness of the rating 1000 times.

**_Review and Rating_**

**Null Hypothesis:** The missingness of ratings does not depend on the missingness of reviews. 

**Alternative Hypothesis:** The missingness of ratings does depend on the missingness of reviews. 

**Test Statistic:** Pearson Correlation (The correlation coefficient that measures linear correlation between two sets of data)

**Significance Level:** 0.05

**Observed statistic/P-Value:** 0.049

**insert graphs here**

>Therefore, we reject our null hypothesis that the missingness of ratings does not depend on the missingness of reviews. 

**_Description and Rating_** 

**Null Hypothesis:** The missingness of description does not depend on the missingness of reviews. 

**Alternative Hypothesis:** The missingness of description does depend on the missingness of reviews.

**Test Statistic:** Pearson Correlation 

**Significance Level:** 0.05

**Observed statistic/P-Value:** 0.533

**insert graphs here**

>Therefore, we fail to reject our null hypothesis that the missingness of description does not depend on the missingness or reviews. 

## Hypothesis Testing
As previously stated from our introduction section, we intend to see how textual elements and user engagement impact recipe ratings. In order to do so, we will cultivate various TF-IDF tables to analyze the textual elements in both the reviews and descriptions. To start off, we will begin by performing hypothesis tests on the length of the recipe review and determining whether or not it affects the average rating.

**Null Hypothesis:** The length of the recipe review does not affect the average rating.

**Alternative Hypothesis:** The length of the recipe review affects the average rating.

**Test Statistic:** The difference in mean ratings between recipes with long reviews and those with short reviews.

**Significance Level:** 0.05

**Observed statistic/P-Value:** 8.019x10^-19

>Therefore, we reject our null hypothesis, suggesting that the review length has a significant impact on the rating.

## Framing a Prediction Problem
Our goal is to predict the average rating of a recipe, depending on user engagement and textual elements, which would have us treat it as a classification problem. We can reorganize the average ratings by rounding the 'floats' into 'ints' and then categorize them into an ordinal qualitative variable. This allows us to build a multi-class classifier so that our model can predict one of these five possible values [1, 2, 3, 4, 5] for the average rating. 

We chose the average rating as our response variable because it effectively represents the overall evaluation of a recipe. Our previous analysis revealed a significant correlation between higher ratings and description length/review length. This suggests that the length of a recipe's description or review might be a useful predictor for the rating.

To evaluate our model, we will use the **accuracy** of our prediction model. Furthermore, our prediction will be based on the `'review'` and `'description'` columns in the rating dataset, as listed in the introduction section in order to predict our response variable `'rating'`.

## Baseline Model

Our baseline model will use RandomForestClassifier with the features being `'description_length'` and `'review_length'`, both of which are quantitative nominal variables, using an evaluation metric of the accuracy. Since both of our features are numerical and not categorical, there was no need to encode any of them. 

The overall performance of the model resulted in results with an `'accuracy'` of **0.701**. We believe this performance for a baseline model is fairly "good" since a ~70% accuracy rate is able to describe 3/4 of the predictions accurately. 

However, there are definitely ways in which we can improve this baseline model, one of them being feature engineering. This includes including possible predictors of the rating such as the number of ingredients and the preparation time. Other perhaps more advanced things we can do could be to use TF-IDF vectorization which can help capture the importance of words in the descriptions relative to the corpus. Also, we can also include sentiment analysis to see if that correlates with user rating. We will be delving deeper into this topic in the next section.

## Final Model

Upon creating our baseline model, which included `'description_length'` and `'review_length'`, we decided that or our final model, to add the following features:

1. `'description_sentiment'`
> As mentioned in our introduction, we wanted to analyze textual element impact on prediction recipe ratings and we are able to do so by utilizing a Python library called `'TextBlob'` which contains sentiment analysis that determines the sentiment polarity (positive, negative, neutral) and subjectivity of a given text. When creating our data science question, we assumed that description sentiment that carry a more positive connotation would likely result in a higher rating and those with a negative correlation with a lower rating. By including this feature, we are able to analyze the magnitude in which textual element can contribute toward predicting a recipe's rating. 

2. `'review_sentiment'`
> The reasons for including this feature is similar to including 'description_sentiment'. We utilized the same Python library `'TextBlob'` to quantify the positive, negative, or neutral connotation of the review. We believe that reviews with a higher sentiment score is likely to result in a higher rating since the review would be positive rather than negative, resulting in a corresponding high or low rating. By including this feature, there would be another variable in consideration to help more accurately predict the rating of a recipe. 

3. `'minutes'`
> We realized that recipes with a longer preparation and cooking time tend to have lower ratings such as 2 or 3, therefore we thought that including this extra feature would help in improving the accuracy of our prediction model. In other words, there seems to be a clear relationship between the 'minutes' and 'rating' which proves as an advantageous feature to include in our model since it's another variable that can accurately predict our model. This also relates to how currently, people are valuing short and fast recipes since it's more convenient with their busy schedules. 

4. `'n_steps'`
> The number of steps ties into our thought of wanting to analyze textual element and thus similar to our reasoning behind 'review_length' and 'description_length'. We believe there to be a correlation with how complex a recipe is and how many steps are required to complete said recipe. Those who are looking for recipes are most likely also looking for ones that are time-efficient and do not take too long. The more complex a recipe might be, the more prone mistakes and errors are able to happen, leaving a negative emotion toward it. 

5. `'n_ingredients'`
> Furthermore, the number of ingredients also contributes toward the complexity as well as the cost-efficiency of the recipe. We believe that the recipes that require a variety of ingredients to have lower ratings since it would cause the individual to buy ingredients they may not have in their pantry at home already, and also ingredients that they may not ever need to use again in the future. Therefore, we believe that adding 'n_ingredients' as a feature in our predictive model would help increase the accuracy.

We used RandomForestClassifier as our modeling algorithm and conducted RandomizedSearchCV to tune the hyperparameters of max_depth, bootstrap, max_features, min_samples_leaf, min_samples_split, and n_estimators of the RandomForestClassifier. Decision trees often exhibit high variance, and we selected these six hyperparameters to help manage variance and prevent overfitting to the training data. The optimal combination of hyperparameters is a max_depth of 10, bootstrap: False, max_features: 'sqrt', min_samples_leaf of 1, min_samples_split of 2, and a n_estimator of 100.

Our chosen metric, accuracy, of the final model is **0.721**, which is a 0.02 increase from the accuracy of the baseline model which was **0.701**. 

## Fairness Analysis

For our fairness analysis, the two groups we decided to test are short description length and long description length. In order to identify the threshold of what determines a short or long description length, we will be using the median, _35 words_, to classify long descriptions as those containing _more than 35 words_ and short descriptions as those containing _less than or equal to 35 words_. Our choice to use the median as the threshold rather than the mean is that the mean may not be an actual description length that exists within the column, It's important for us to evaluate the precision parity of the model for the two aforementioned groups since we believe it would be important for our prediction model to more accurately identify the rating of a recipe. In the chance that we produce false positives, that would be detrimental for users who are disincentivized to try certain recipes due to a mislabeling of shorter description lengths leading to lower ratings. 

**Null Hypothesis:** Our model is fair. Its precision for recipes with longer description length and shorter description length are roughly the same, and any differences are due to random chance.

**Alternative Hypothesis:** Our model is unfair. Its precision for recipes with longer description length and shorter description length  is lower than its precision for recipes with higher calories.

**Test Statistic:** Difference in precision (long descriptions - short descriptions)

**Significance Level:** 0.05

**Observed statistic/P-Value:** 0.952

> Therefore we fail to reject the null hypothesis which implies that our model is fair. Its precision for recipes with longer description length and shorter description length are roughly the same, and any differences are due to random chance.
