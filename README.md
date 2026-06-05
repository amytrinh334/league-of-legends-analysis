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

Before analyzing the data, we took several steps to clean the dataset. First, we filtered out the columns to only keep the match statistics that were relevant to our hypothesis testing and predictive model (kept columns are given descriptions in the table above). We then filtered out the data to only include team statistics instead of individual player statistics, since we wanted to focus on the entire team's match data and results. We then created renamed the team's kills columns to `total_kill` for easy understanding, and also curated a new column named `total_monster_objectives' which is the sum of the teams total dragons, heralds, void gribs, and baron kills. We decided to not drop any missing values for now, as we will peform some missingness dependency tests later on. After cleaning the dataset, we ended up with a total of 20076 rows and 25 columns.

Below is the head of our our cleaned data:
| gameid           |   participantid | side   | position   |   result |   total_kills |   deaths |   assists |   doublekills |   triplekills |   quadrakills |   pentakills |   firstblood |   dragons |   infernals |   mountains |   clouds |   oceans |   chemtechs |   hextechs |   elders |   heralds |   void_grubs |   barons |   total_monster_objectives |
|:-----------------|----------------:|:-------|:-----------|---------:|--------------:|---------:|----------:|--------------:|--------------:|--------------:|-------------:|-------------:|----------:|------------:|------------:|---------:|---------:|------------:|-----------:|---------:|----------:|-------------:|---------:|---------------------------:|
| LOLTMNT03_179647 |             100 | Blue   | team       |        0 |             3 |       13 |         5 |             0 |             0 |             0 |            0 |            0 |         0 |           0 |           0 |        0 |        0 |           0 |          0 |        0 |         0 |            0 |        0 |                          0 |
| LOLTMNT03_179647 |             200 | Red    | team       |        1 |            13 |        3 |        36 |             0 |             0 |             0 |            0 |            1 |         2 |           1 |           0 |        1 |        0 |           0 |          0 |        0 |         1 |            6 |        1 |                         10 |
| LOLTMNT06_96134  |             100 | Blue   | team       |        1 |            21 |       11 |        53 |             3 |             0 |             0 |            0 |            1 |         3 |           0 |           3 |        0 |        0 |           0 |          0 |        0 |         1 |            6 |        1 |                         11 |
| LOLTMNT06_96134  |             200 | Red    | team       |        0 |            10 |       21 |        22 |             0 |             0 |             0 |            0 |            0 |         2 |           0 |           0 |        0 |        1 |           0 |          1 |        0 |         0 |            0 |        0 |                          2 |
| LOLTMNT06_95160  |             100 | Blue   | team       |        0 |            18 |       22 |        30 |             2 |             0 |             0 |            0 |            0 |         0 |           0 |           0 |        0 |        0 |           0 |          0 |        0 |         0 |            2 |        0 |                          2 |


## Univariate Analysis
We performed a univariate analysis
## Bivariate Analysis
## Interesting Aggregates
We created a pivot table by grouping the data by results. This revealed that teams that usually lost the match had a lower average amount of kills and monster objective kills than teams that won the match. We will go more into depth with this information during our hypothesis testing.

|   result |   total_kills |   total_monster_objectives |   dragons |   heralds |   barons |   void_grubs |
|---------:|--------------:|---------------------------:|----------:|----------:|---------:|-------------:|
|        0 |       10.5984 |                     3.6846 |   1.40974 |  0.337617 | 0.171249 |      1.76599 |
|        1 |       21.9984 |                     7.0272 |   3.03626 |  0.660291 | 0.886232 |      2.44441 |

# Assessment of Missingness
## NMAR Analysis
## Missingness Dependency

# Hypothesis Testing
# Framing a Prediction Problem
# Baseline Model
# Final Model
# Fairness Analysis
# Conclusion

 
