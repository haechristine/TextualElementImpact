# Textual Element Impact on Predicting Recipe Ratings

Authors: Daniel Zhu & Christine Law

## Abstract

This site serves as a easily accessible report on our findings regarding the relationship of textual element on recipie ratings. More specifically, we will be creating a predictive model to determine: How do the textual elements and user engagement of a recipe (e.g., description, reviews) impact ratings? This predictive model will provide valuable insight that benefit both content creators and customers. More specifically, content creators can use this information to create better-optimized content that meets their user preference. On the other hand, consumers will benefit from a more engaging, informative, and enjoyable recipe content.

## Introduction
Our analysis utilizes two different datasets: one focused on the contents and characteristics of a recipie, and one consisting of the recipies' ratings and review. Both datasets contain recipie entries posted since 2008. The recipie dataset consists of 10 variables while the ratings dataset consists of 5 variables, all of which we've listed in more detail below.  

1. Recipies (83782 entries)

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

2. Ratings (731927 entries)

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |

More specifically, our analysis and predictive model will focus on the columns `'description'`, `'review'`, and `'ratings'`.  

## Data Cleaning and Exploratory Data Analysis
In order to make the given datasets easily accessible for our data science question, we will merge the 'ratings' and 'recipies' dataset into one cohesive master dataset.

1. First we will split up the 'nutrition' column into the respective columns "[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)]"

2. We will then merge the two datasets using a left merge on 'recipe_id'.

3. Then, we will fill in all ratings of 0 with np.nan. In this case, a rating of 0 implies a missing rating for that recipie. Therefore, in order to avoid bias and a potential misconception of a lower and actual average rating, we will fill in all ratings of 0 with np.nan.

4. Next, we will create a Series containg the average rating per recipe and add that data into a new column within our already cleaned dataset. 

Our final cleaned DataFrame contains 234428 rows and 23 columns. Below are the first 5 entries and the columns we find the most relevant to answering our data science question.

| name                                 |     id |      tags | submitted           |   rating |   average rating |   calories (#) |   sugar (PDV) | is_dessert   |   prop_sugar |
|:-------------------------------------|-------:|----------:|:--------------------|---------:|-----------------:|---------------:|--------------:|:-------------|-------------:|
| 1 brownies in the world    best ever | 333281 |        ['60-minutes-or-less', 'time-to-make', 'course... | 2008-10-27 00:00:00 |        4 |                4 |          138.4 |            50 | True         |    0.361272  |
| 1 in canada chocolate chip cookies   | 453467 |        ['60-minutes-or-less', 'time-to-make', 'cuisin.. | 2011-04-11 00:00:00 |        5 |                5 |          595.1 |           211 | False        |    0.354562  |
| 412 broccoli casserole               | 306168 |        ['60-minutes-or-less', 'time-to-make', 'course... | 2008-05-30 00:00:00 |        5 |                5 |          194.8 |             6 | False        |    0.0308008 |
| 412 broccoli casserole               | 306168	|        ['60-minutes-or-less', 'time-to-make', 'course... | 2008-02-12 00:00:00 |        5 |                5 |          878.3 |           326 | True         |    0.371172  |
| 412 broccoli casserole               | 306168	|        ['60-minutes-or-less', 'time-to-make', 'course... | 2012-03-06 00:00:00 |        5 |                5 |          267   |            12 | False        |    0.0449438 |

### Univariate Analysis 

### Bivariate Analysis 

### Interesting Aggregates


## Assessment of Missingness


## Hypothesis Testing
Null Hypothesis: The length of the recipe description does not affect the average rating.
Alternative Hypothesis: The length of the recipe description affects the average rating.

And our test statistic is:
The difference in mean ratings between recipes with long descriptions and those with short descriptions.

Thus, after testing our hypothesis:
We concluded with a p-value less than 0.05 that we reject the null hypothesis suggesting that the description length has a significant impact on the rating.

## Framing a Prediction Problem


## Baseline Model
Our baseline model will use a linear regression with the features being "description_length" and "n_steps", using an evaluation metric of the mean absolute error.

Some ways that we can improve this baseline model would be through feature engineering. 
This includes including possible predictors of the rating such as the number of ingredients and the preparation time. Other perhaps more advanced things we can do could be to use TF-IDF vectorization which can help capture the importance of words in the descriptions relative to the corpus. Also, we can also include sentiment analysis to see if that correlates with user rating.

## Final Model


## Fairness Analysis