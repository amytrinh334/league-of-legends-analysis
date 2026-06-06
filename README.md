# Introduction

## General Introduction

League of Legends (commonly called LoL) is a highly popular, fast-paced competitive multiplayer online battle arena (MOBA) game developed by Riot Games. At its core, the game matches 2 teams of 5 players against each other, with each team fighting to defend their own half of the map while working to destroy the enemy team's main base structure, known as the Nexus. Each player is able to choose their own character and role in the team, making every match incredibly unique.

Every match is composed of many different gameplay statistics, each having its own impact on the result of the game. However, we noticed that there are 2 main statistics that tend to have the most impact: the number of kills the team obtains and the number of monsters the team kills (since they grant the players buffs). Therefore, we decided to focus our project on this aspect. 

Our main research question is **"Which game statistic is a better predictor of a match's result: a team's total kills or the total monster objectives?"** We will use data analysis techniques to clean and filter out the dataset in order to get a basic understanding of the data we are working with. We will then perform different hypothesis tests to analyze the dependencies of the game data. Lastly, we will develop a prediction model to see how accurate the gameplay statistics are in predicting the match results. Knowing which statistic is a better predictor can help players determine what aspects to focus on while playing the game to increase their chances of winning.

## Dataset Introduction

This dataset, curated by Oracle's Elixir, contains match data from multiple LoL esports leagues from 2025. It contains 120456 rows of data, which consists of individual player and team stats, as well as 165 columns of match data. A brief introduction to the dataset of some important columns to understand are as follows: 

| Column Name | Description |
| :--- | :--- |
| `gameid` | A unique identifier code for each specific match. |
| `participantid` | A unique number (from 1 through 10) assigned to each player in that specific match to distinguish them from one another. Numbers designated 100 or 200 indicate that the data is a summary of the team statistics. |
| `side` | Indicates which half of the map the team started on—either Blue (bottom-left) or Red (top-right). |
| `position` | The specific role the player took during the match (Top, Jungle, Mid, ADC/Bot, or Support). Positions designated team indicate that the data is a summary of the team statistics. |
| `result` | The result/outcome of each match. 1 indicates that the team/player won the match, 0 indicates that the team/player loss the match. |
| `kills` | The number of times the player/team landed the final blow to eliminate an enemy champion. |
| `deaths` | The number of times the player was eliminated by enemy champions, towers, or monsters. |
| `assists` | The number of times the player/team damaged or debuffed an enemy champion (or healed/buffed an ally) right before that enemy was killed by a teammate. |
| `doublekills` | The number of times a player killed 2 enemy champions within a short time frame. |
| `triplekills` | The number of times a player killed 3 enemy champions within a short time frame. |
| `quadrakills` | The number of times a player killed 4 enemy champions within a short time frame. |
| `pentakills` | The number of times a player killed 5 enemy champions, eliminating the entire enemy team, within a short time frame. |
| `dragons` | The total number of dragons secured by the player's team. |
| `infernals`<br>`mountains`<br>`clouds`<br>`oceans`<br>`chemtechs`<br>`hextechs`<br>`elders` | The specific breakdown of which type of dragons the team defeated. Each type grants a different permanent team-wide stat bonus. |
| `heralds` | The number of Rift Heralds captured. This objective spawns early in the match and can be summoned to smash enemy defensive towers. |
| `void_grubs` | The number of Voidgrubs defeated. These are small early-game monsters that grant a team a permanent buff to damage enemy towers faster. |
| `barons` | The number of Baron Nashors slain. Defeating Baron grants a massive temporary buff that makes nearby friendly minions much stronger, helping teams break into the enemy base. |


# Data Cleaning and Exploratory Data Analysis

## Data Cleaning

Before analyzing the data, we took several steps to clean the dataset. First, we filtered out the columns to only keep the match statistics that were relevant to our hypothesis testing and predictive model (kept columns are given descriptions in the table above). We then filtered out the data to only include team statistics instead of individual player statistics, since we wanted to focus on the entire team's match data and results. We then created renamed the team's kills columns to `total_kill` for easy understanding, and also curated a new column named `total_monster_objectives` which is the sum of the teams total dragons, heralds, void gribs, and baron kills. We decided to not drop any missing values for now, as we will peform some missingness dependency tests later on. After cleaning the dataset, we ended up with a total of 20076 rows and 25 columns.

Below is the head of our our cleaned data:

| gameid           |   participantid | side   | position   |   result |   total_kills |   deaths |   assists |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   dragons |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   elders |   heralds |   void_grubs |   barons |   total_monster_objectives |
|:-----------------|----------------:|:-------|:-----------|---------:|--------------:|---------:|----------:|--------------:|--------------:|--------------:|-------------:|-------------:|----------:|------------:|------------:|---------:|---------:|------------:|-----------:|---------:|----------:|-------------:|---------:|---------------------------:|
| LOLTMNT03_179647 |             100 | Blue   | team       |        0 |             3 |       13 |         5 |             0 |             0 |             0 |            0 |            0 |         0 |           0 |           0 |        0 |        0 |           0 |          0 |        0 |         0 |            0 |        0 |                          0 |
| LOLTMNT03_179647 |             200 | Red    | team       |        1 |            13 |        3 |        36 |             0 |             0 |             0 |            0 |            1 |         2 |           1 |           0 |        1 |        0 |           0 |          0 |        0 |         1 |            6 |        1 |                         10 |
| LOLTMNT06_96134  |             100 | Blue   | team       |        1 |            21 |       11 |        53 |             3 |             0 |             0 |            0 |            1 |         3 |           0 |           3 |        0 |        0 |           0 |          0 |        0 |         1 |            6 |        1 |                         11 |
| LOLTMNT06_96134  |             200 | Red    | team       |        0 |            10 |       21 |        22 |             0 |             0 |             0 |            0 |            0 |         2 |           0 |           0 |        0 |        1 |           0 |          1 |        0 |         0 |            0 |        0 |                          2 |
| LOLTMNT06_95160  |             100 | Blue   | team       |        0 |            18 |       22 |        30 |             2 |             0 |             0 |            0 |            0 |         0 |           0 |           0 |        0 |        0 |           0 |          0 |        0 |         0 |            2 |        0 |                          2 |


## Univariate Analysis
We performed a univariate analysis on the total number of team kills and monster objective kills by match. 

<iframe
  src="assets/kills_per_match.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This histogram graph shows the distribution of total team kills per match. It appears to have a bimodal distribution , with peaks at around 8 and 20 kills. It is likely that the peaks represent the number of kills that were obtained by losing vs winning teams. 

<iframe
  src="assets/monster_objectives_per_match.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This histogram graph shows the distribution of total team monster objective kills per match. The distribution of the data is approximately normal, meaning that the data is well behaved. The graph has a peak at around 6 monster objectives. 

## Bivariate Analysis
We also performed a bivariate analysis on the total number of team kills and monster objective kills by result. 

<iframe
  src="assets/kills_by_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This box plot shows the distribtion of total team kills by result, where losing teams (0) are green and winning teams (1) are orange. It appears that winning teams have a higher median number of total team kills by result at 22, but also has a larger variance. Losing teams have a lower median number of total team kills at 10.

<iframe
  src="assets/monster_objectives_by_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
This box plot shows the distribtion of total monster objective kills by result, where losing teams (0) are green and winning teams (1) are orange. It appears that winning teams have a higher median number of total monster objective kills by result at 7 and also has a smaller variance. Losing teams have a lower median number of total team kills at 3.

## Interesting Aggregates
We created a pivot table by grouping the data by results. This dataframe summarize the pattern that was revealed in our bivariate analysis. This revealed that teams that usually lost the match had a lower average amount of kills and monster objective kills than teams that won the match. We will go more into depth with this information during our hypothesis testing.

|   result |   total_kills |   total_monster_objectives |   dragons |   heralds |   barons |   void_grubs |
|---------:|--------------:|---------------------------:|----------:|----------:|---------:|-------------:|
|        0 |       10.5984 |                     3.6846 |   1.40974 |  0.337617 | 0.171249 |      1.76599 |
|        1 |       21.9984 |                     7.0272 |   3.03626 |  0.660291 | 0.886232 |      2.44441 |


# Assessment of Missingness

## NMAR Analysis
In our data we, had missing values from the columns `infernals`, `mountains`, `clouds`, `oceans`, `chemtechs`, `hextechs`, `elders`, `doublekills`,`triplekills`, `quadrakills`, and `pentakills`. All of these columns had the same amount of missing values, 1634. We believe that there are no columns in the dataset that are NMAR. The null values are either dragon types (mountain, elders, hextechs, chemtechs, oceans, clouds, infernals) and kill types (double, triple, quadra, penta). These values are likely to be dependent on other League of Legends stats, such as the number of dragons killed or the total number of team kills. It is unlikely that the missing values are only dependent on themselves, as they are probably MD, MAR, or MCAR.  The fact that they have the same amount of missing values suggest that it could possibly be Missing By Design (MD).

## Missingness Dependency
In this section, we will test if there is a depenency for the null values of the dragon types on the rest of the dataset. For simplicity, we will only be using one dragon type, `mountains`. Since there are many columns, we decided to run permuation tests on every column that did not have missing values to see the dependency of `mountains`.

**Null Hypothesis**: The distribution of mountains is independent of another column. Any observed difference in proportions is due to random chance.

**Alternative Hypothesis:** The distribution of mountains depends on another column.

**Test Statistic**: Total Variation Distance (TVD)

After running our permutation tests, we discovered that `mountains` is dependent on other columns such as `side`, `heralds`, `result`, `firstblood`, `clouds`, `hextechs`, `chemtechs`, `infernals`, and `oceans`. The `mountains` column is not dependent on other columns such as `void_grubs`, `barons`, `dragons`, `doublekills`, `elders`, `pentakills`, `triplekills`, `quardrakills`, and `position`.

What we found interesting is the the dragon type column is mainly not dependent on the amount of dragons killed, as it had the highest TVD of 0.44, but whether the dragon is another type. This makes sense however, since if the dragon is one type, it will not be of another.

Below is an example of plots for 'mountains' when it is tested against 'result', as well as 'mountains' when it is tested against 'barons.
<iframe
  src="assets/tvd_mountains_vs_barons.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The observed TVD for this test was approximately 0.38, and the p-value was equal to approximately 0.067. Since the p-value is greater than the 0.5 significance level, we fail reject the null hypothesis. Thus, the missingness of `mountains` does not depend on the `barons` column.

<iframe
  src="assets/tvd_mountains_vs_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The observed TVD for this test was approximately 0.16, and the p-value was equal to 0. Since the p-value is less than the 0.5 significance level, we reject the null hypothesis. Thus, the missingness of `mountains` depends on the `result` column.


# Hypothesis Testing
In this section, we want to asses if there is a significant difference between the distribution of total kills and total monster objectives between winning and losing teams. This information will be be helpful in understanding the accuracy of our prediction model. We will run two one-sided permutation tests using difference in means as the test statistic. 

### **Test 1: Kills**
**Null Hypothesis:** Winning teams and losing teams have the same distribution of total kills. Any difference in average kills is due to random chance.
**Alternative Hypothesis:** Winning teams have a higher mean number of kills than losing teams.
**Test statistics:** Difference in Means (Mean kills for winning teams - Mean kills for losing teams)

<iframe
  src="assets/test1_total_kills.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
After running our permutation tests, we got a p-value that is close to 0. Since it is less than the 0.05 significance level, we will reject the null hypothesis. Therefore, there is sufficient evidence that winning teams have a higher mean total kills than losing teams.

### Test 2: Monster Objectives
**Null Hypothesis:** Winning teams and losing teams have the same distribution of monster objectives. Any difference in average monster objectives is due to random chance.
**Alternative Hypothesis:** Winning teams have a higher average number of monster objectives than losing teams.
**Test statistics:** Difference in Means (Mean monster objectives for winning teams - Mean monster objectives for losing teams)

<iframe
  src="assets/test2_total_monsters.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
After running our permutation tests, we got a p-value that is close to 0. Since it is less than the 0.05 significance level, we will reject the null hypothesis. Therefore, there is sufficient evidence that winning teams have a higher mean total monster objective kills than than losing teams.

# Framing a Prediction Problem
# Baseline Model
# Final Model
# Fairness Analysis
<iframe
  src="assets/fairness_kills_model.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

<iframe
  src="assets/fairness_monster_objectives.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
# Conclusion

 
