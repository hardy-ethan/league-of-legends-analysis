# The Role of Roles in League of Legends

# Introduction

## Purpose and Motivating Question
The purpose of this website is to explore a dataset of over 10,000 League of Legends games. 

For those unfamiliar, League of Legends is a MOBA (Multiplayer Online Battle Area) video game, which are characterized by a top down view, multiplayer, team versus team experience. League of Legends also has multiple different "roles" a player can choose: Top Lane, Jungle, Mid Lane, Bot Lane, and Support.

To motivate this exploration, we provide a question: **How do different roles act within the game?** 

This question may seem simple on the surface, but the analysis of such data and relations may prove useful to both players and fans of professional play. If you're a player, what skills should you be focusing on in order to maximize your potential in your role? If your stats lead you to be classified as a different role, you may perform better in the game playing that role instead. For fans of professional apply, they can apply these same observations to their favorite players, and dive deep into how teams may agree or disagree with the statistics.

## Relevant Subset of the Data
This dataset is vast, with 150181 rows (including the header), and 161 columns. We feel that not all of these columns are directly relevant (for example, team statistics will not affect individual players), so a subset was chosen for analysis:

* **kills**: The number of kills the player got in the match
* **deaths**: The number of deaths the player had in the match
* **assists**: The number of assists the player had in the match
* **damagetochampions**: The amount of damage done to enemy champions
* **damagetakenperminute**: The amount of damage the player took per minute, averaged over the length of the match
* **wardsplaced**: The amount of wards (sight enhancing items) placed by the player in the match
* **earnedgold**: The amount of gold the player earned during the match
* **minionkills**: The amount of minions the player killed during the match
* **monsterkills**: The amount of monsters the player killed during the match
* **firstbloodvictim**: Whether or not the player was the first to die in the match
* **position**: The role the player played in the match

This subset was selected as it represents many different parts of a match, but does not involve team statistics and filters out colinear variables like _gold earned_ and _gold earned per minute_. It also keeps the number of total variables to consider down, which will be helpful in keeping the the data analysis actually useful for players (people do not want to be analyzing 40+ variables).

# Data Cleaning and Exploratory Data Analysis
## Data Cleaning

In order to perform analysis on this data, some cleaning was performed. First, rows with the **position** set as team were filtered out, as these were found to be matches with only partial data, and, importantly, did not contain data on individual players.

Then, mean imputation was performed on **damagemitigatedperminute**. Mean imputation was chosen as there is a lot of valid data in the dataset, and the reason why there is also a lot of missing data for these columns is not known and does not appear to depend on other columns.

Listwise Deletion was done on **damagetochampions**, **damagetakenperminute**, **wardsplaced**, **earnedgold**, **minionkills**, **monsterkills** 
because they only have 10 missing values, which is a very small portion of the dataset. 
It was also performed on **firstbloodvictim**, because **firstbloodvictim** is binary and true for only a single player in a match, 
so a mean does not make sense to take, and its data is also missing for every player in a match if it is missing for a single player, 
so we are effectively just dropping those games.

Finally, **kills**, **deaths**, and **assists** were not imputed, as they did not have any missing values.

The distribution of the imputed columns before and after imputation follow.

<iframe src="assets/imputed/damagemitigatedperminute.html" width="650" height="450" frameborder="0"></iframe>
<iframe src="assets/imputed/damagetochampions.html" width="650" height="450" frameborder="0"></iframe>
<iframe src="assets/imputed/damagetakenperminute.html" width="650" height="450" frameborder="0"></iframe>
<iframe src="assets/imputed/wardsplaced.html" width="650" height="450" frameborder="0"></iframe>
<iframe src="assets/imputed/earnedgold.html" width="650" height="450" frameborder="0"></iframe>
<iframe src="assets/imputed/minionkills.html" width="650" height="450" frameborder="0"></iframe>
<iframe src="assets/imputed/monsterkills.html" width="650" height="450" frameborder="0"></iframe>
<iframe src="assets/imputed/firstbloodvictim.html" width="800" height="600" frameborder="0"></iframe>

With all of these steps completed, the data now looks as follows:

|   kills |   deaths |   assists |   damagetochampions |   damagetakenperminute |   damagemitigatedperminute |   wardsplaced |   earnedgold |   minionkills |   monsterkills |   firstbloodvictim | position   |
|--------:|---------:|----------:|--------------------:|-----------------------:|---------------------------:|--------------:|-------------:|--------------:|---------------:|-------------------:|:-----------|
|       2 |        3 |         2 |               15768 |               1072.4   |                    777.793 |             8 |         7164 |           220 |             11 |                  0 | top        |
|       2 |        5 |         6 |               11765 |                944.273 |                    650.158 |             6 |         5368 |            33 |            115 |                  0 | jng        |
|       2 |        2 |         3 |               14258 |                581.646 |                    227.776 |            19 |         5945 |           177 |             16 |                  0 | mid        |
|       2 |        4 |         2 |               11106 |                463.853 |                    218.879 |            12 |         6835 |           208 |             18 |                  0 | bot        |
|       1 |        5 |         6 |                3663 |                475.026 |                    490.123 |            29 |         2908 |            42 |              0 |                  0 | sup        |

## Univariate Analysis
<iframe src="assets/position_distribution.html" width="650" height="450" frameborder="0"></iframe>
An important fact about this data is that each position is represented equally! 
Upon inspection of the dataset, it becomes apparent that the data is collected as a set of games, 
each of which always has one of each position! This is beneficial because we know that the data 
does not contain bias due to certain roles being more or less represented than each other.

## Bivariate Analysis
Basic stastics for the game include kills, deaths, and assists. 
One may wonder if these are distinct within positions, and we begin to explore that in the following plot.
<iframe src="assets/position_vs_kills.html" width="650" height="450" frameborder="0"></iframe>
Not only does each position seem to have a decently distinct distribution of kills, 
support is shown as having a much lower bottom 50%, which make sense upon introspection, 
as their job is to _support_ their teammates in getting kills, and not necessarily get the kills themselves.

## Interesting Aggregates
The following pivot table shows how different positions tend to be the first killed in the match (with 0.0 being not first killed, and 1.0 being first killed). We see that **bot**, **mid**, and **top** tend to be the most often, then **jng**, and finally **sup** being much lower. This data shows how "aggressive" these roles are, as one needs to meet another enemy in order to die.

| position   |   0.0 |   1.0 |
|:-----------|------:|------:|
| bot        | 84260 |  6202 |
| jng        | 60941 |  4772 |
| mid        | 69373 |  6004 |
| sup        | 17160 |  1877 |
| top        | 53154 |  6283 |

# Prediction Problem
Looking at all of this data is interesting, but we wish to revisit our original question "**How do different roles act within the game?**" In order to do this, we will predict which role a player played given their post-game data. This is a multiclass classification problem, where we are classifying the match data into one of five possible roles (the response variable). We choose this response variable because the role is one of the most important aspects of League of Legends gameplay. We perform our classification on game data after games have finished, so we know all of the actions players have taken in their matches. We chose to use accuracy to evaluate our classifier because each role is the same "value" within the game, and therefore predicting incorrectly is equally costly.

# Baseline Model
We first start with a model that uses three quantitative features: **kills**, **deaths**, and **assists**. We chose a One vs One classifier that uses Logistic Regression, and we did not need to perform any encoding on the features. Unfortunately, this model does not perform very well. Performing an accuracy test with a train-test split, we see that the accuracy is around 37%.5%, which is hardly better than randomly guessing which role a player played.

# Final Model
For our final model, we chose to add in the **damagetochampions**, **damagetakenperminute**, **damagemitigatedperminute**, **wardsplaced**, **earnedgold**, **minionkills**, **monsterkills** variables and apply a Quantile Transform to each of them. Adding more features give more insight into the game itself and applying the Quantile Transformation helped fight against the skewed nature of those features. We also chose to apply the transformation to our original features so that all features would have the same scale, which prevents them from dominating each other. We performed a Cross Validation Grid Search with cross-fold size 5 which ended up choosing a quantile count of 1000. Our final model proves much better than our Baseline Model, with an accuracy score of around 80%. This means that it is nearly four times better than randomly guessing!

# Contact
Feel free to reach out!

Name: Ethan Hardy

Email: hardyem at umich dot edu