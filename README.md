# Textual Element Impact on Predicting Recipe Ratings

Authors: Daniel Zhu & Christine Law

## Abstract

This site serves as a easily accessible report on our findings regarding the relationship of textual element on recipie ratings. More specifically, we will be creating a predictive model to determine: **How do the textual elements and user engagement of a recipe (e.g., description, reviews) impact ratings?** This predictive model will provide valuable insight that benefit both content creators and customers. More specifically, content creators can use this information to create better-optimized content that meets their user preference. On the other hand, consumers will benefit from a more engaging, informative, and enjoyable recipe content.

## Introduction
Our analysis utilizes two different datasets: one focused on the contents and characteristics of a recipie, and one consisting of the recipies' ratings and review. Both datasets contain recipie entries posted since 2008. The recipie dataset consists of 10 variables while the ratings dataset consists of 5 variables, all of which we've listed in more detail below.  

1. `Recipies` (83782 entries)

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
In order to make the given datasets easily accessible for our data science question, we will merge the `'ratings'` and `'recipies'` dataset into one cohesive master dataset.

1. First we will split up the `'nutrition'` column into the respective columns `"[calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), and carbohydrates (PDV)]"`

2. We will then merge the two datasets using a left merge on `'recipe_id'`.

3. Then, we will fill in all ratings of 0 with np.nan. In this case, a rating of 0 implies a missing rating for that recipe. Therefore, in order to avoid bias and a potential misconception of a lower and actual average rating, we will fill in all ratings of 0 with np.nan.

4. Next, we will create a Series containg the average rating per recipe and add that data into a new column within our already cleaned dataset. 

Our final cleaned DataFrame contains 234428 rows and 23 columns. Below are the first 5 entries and the columns we find the most relevant to answering our data science question. There will be repeats in regards to the recipe since we do not want to lose the data on their unique reviews despite it being for the same recipie.

| name                                 |     id |      tags | description           |   rating |   average rating |   review	 |
|:-------------------------------------|-------:|----------:|:--------------------|---------:|-----------------:|---------------:|
| 1 brownies in the world    best ever | 333281 |        ['60-minutes-or-less', 'time-to-make', 'course... | these are the most; chocolatey, moist, rich, d... |        4 |                4 |        These were pretty good, but took forever to ba... |
| 1 in canada chocolate chip cookies   | 453467 |        ['60-minutes-or-less', 'time-to-make', 'cuisin.. | this is the recipe that we use at my school ca... |        5 |                5 |          Originally I was gonna cut the recipe in half ... |
| 412 broccoli casserole               | 306168 |        ['60-minutes-or-less', 'time-to-make', 'course... | since there are already 411 recipes for brocco... |        5 |                5 |          This was one of the best broccoli casseroles t... |
| 412 broccoli casserole               | 306168	|        ['60-minutes-or-less', 'time-to-make', 'course... | since there are already 411 recipes for brocco... |        5 |                5 |         I made this for my son's first birthday party ... |
| 412 broccoli casserole               | 306168	|        ['60-minutes-or-less', 'time-to-make', 'course... | since there are already 411 recipes for brocco... |        5 |                5 |          Loved this. Be sure to completely thaw the br...   |

### Univariate Analysis 
For our univarite analysis, we depicted the distribution of ratings, description length, and review length. In the distribution showcasing recipe ratings, there seems to be less lower ratings than higher ratings, resulting in a positive and upward trend. In other words, the recipies with the least entries are those with the lower ratings. In our distribution of description length, we see that the range of the description lengths range from 0-400 words and that there are more reviews that tend to be on the shorter and brief side. Lastly, our distrubution of review length showcases similar trends to the description length, with there being a higher frequency in shorter and concise reviews.

**insert graphs here**

### Bivariate Analysis 
For our bivariate analysis, we depicted scatterplots showcasing the relationship between 'Description Length vs. Rating' and 'Review Length vs. Rating'. The first graph reveals that a high ranking recipe tends to have a longer description length compared to those with a lower rating. Similarly, the trend between the 'Review Length vs. Rating' showcases similar results with a recipe with a rank of 4 or 5 having a review length of 300-400 words and more data entries than recipies with lower ratings. This reasoning may be associated with what we will mention in our missingness section, and how reviewers may not leave a review unless they have strong emotions toward the recipe, and in this case it seems that reviewers who had a positive impression on the recipe were more likely to leave a lengthy review. We will also delve deeper about these relationships in our predictive model.

**insert graphs here**

### Interesting Aggregates
For this section, we delved deeper into the relationship between the length of the reviews and the rating the recipie recieved. In order to do so, we first utilized the groupby method to group by `'review_length_range'` and then `'rating'`. We then aggregated the dataframe by finding the average rating to produce the table shown below. The significance of this pivot table is that it allows us to see whether or not there is a present and significant corrleation associated with the length of ones review and the rating it recieves. By doing so, we are analyzing if there is another variable other than TF-IDF values that has the ability to predict the recipe ratings.

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
The final cleaned dataset we have cultivated contains a few missing values, especially in the columns 'rating' and 'review'. Since both of these values are important to our predictive model and analysis, we are going to assess their missingness. 

### NMAR Analysis
Upon analyzing the dataset, we believe that the 'review' column showcases a missingness of NMAR. NMAR is defined as a column containing missing values dependant on another column in the dataset. Entries that are missing a value in the 'review' column may be because the indivdual had no strong emotions toward the recipie itself, unmotivating themselves in leaving a review. This can be reflected by a missing value in the 'rating' column or even the 'description' or 'steps' column. It's possible that the individual gained strong emotions, negative or postive, due to the number of steps of the recipe such as it being too complex or perfectly simple.

### Missingness Dependency
Now, we will more closely examine the `'rating'` column and determine if its missingness is dependent on the missingness of either `'review'` or `'description'`. In order to do so, we will carry out two seperate `permutation tests` , each with running the permutation test by shuffling the missingness of the rating 1000 times.

> Review and Rating
**Null Hypothesis:** The missingness of ratings does not depend on the missingness of reviews. 

**Alternative Hypothesis:** The missingness of ratings does depend on the missingness of reviews. 

**Test Statistic:** Pearson Correlation (The correlation coefficient that measure linear correlation between two sets of data)

**Significance Level:** 0.05

**Observed statistic/P-Value:** 0.044

**insert graphs here**

>Therefore, we reject our null hypothesis that the missingness of ratings does not depend on the missingness or reviews. 

> Description and Rating 
**Null Hypothesis:** The missingness of description does not depend on the missingness of reviews. 

**Alternative Hypothesis:** The missingness of description does depend on the missingness of reviews.

**Test Statistic:** Pearson Correlation 

**Significance Level:** 0.05

**Observed statistic/P-Value:** 0.566

**insert graphs here**

>Therefore, we fail to reject our null hypothesis that the missingness of description does not depend on the missingness or reviews. 

## Hypothesis Testing
As previously stated from our introduction section, we intend to see how textual elements and user engagement impact recipe ratings. In order to do so, we will cultivate various TF-IDF tables to analyze the textual elements in both the reviews and description. To start of, we will begin by performing hypothesis tests on the lenght of the recipe description and determing whether or not it affects the average rating.

**Null Hypothesis:** The length of the recipe description does not affect the average rating.

**Alternative Hypothesis:** The length of the recipe description affects the average rating.

**Test Statistic:** The difference in mean ratings between recipes with long descriptions and those with short descriptions.

**Significance Level:** 0.05

**Observed statistic/P-Value:** _______

**insert graphs here**

>Therefore, we reject our null hypothesis, suggesting that the description length has a significant impact on the rating.

## Framing a Prediction Problem
Our goal is to predict the average rating of a recipe, depending on user enagment and textual elements, which would have us treat it as a classification problem. We can reorganize the average ratings by rounding the 'floats' into 'ints' and then categorize them into an ordinal quanlitative variable. This allows us to build a multi-class classifier so that our model can predict one of these five possible values for the average rating.

We chose the average rating as our response variable because it effectively represents the overall evaluation of a recipe. Our previous analysis revealed a significant correlation between higher ratings and description length/review length. This suggests that the length of a recipe's description or review might be a useful predictor for the rating.

**not sure if this is also what we want to do?**
To evaluate our model, we will use the F1 score rather than accuracy. This decision is due to the left-skewed distribution of ratings, with most falling in the higher range (4-5). Using accuracy could be misleading in such an imbalanced scenario, whereas the F1 score provides a more nuanced measure of the model's performance.

Our prediction will be based on all columns in the rating dataset, as listed in the introduction section. These features pertain to the recipes themselves, meaning they are available even if no ratings or reviews have been submitted yet.

## Baseline Model
Our baseline model will use a linear regression with the features being "description_length" and "n_steps", both of which are quantitative nominal variables, using an evaluation metric of the mean absolute error.

Some ways that we can improve this baseline model would be through feature engineering. 

This includes including possible predictors of the rating such as the number of ingredients and the preparation time. Other perhaps more advanced things we can do could be to use TF-IDF vectorization which can help capture the importance of words in the descriptions relative to the corpus. Also, we can also include sentiment analysis to see if that correlates with user rating.

## Final Model
State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process.

Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.

Optional: Include a visualization that describes your model’s performance, e.g. a confusion matrix, if applicable.

## Fairness Analysis

Clearly state your choice of Group X and Group Y, your evaluation metric, your null and alternative hypotheses, your choice of test statistic and significance level, the resulting p-value, and your conclusion.

Optional: Embed a visualization related to your permutation test in your website.