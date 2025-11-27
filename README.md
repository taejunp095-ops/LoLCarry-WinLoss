# LoLCarry-WinLoss
UCSD DSC80 Final Project

## Introduction 
The dataset chosen for this project is the dataset of professional League of Legends games (specifically of the year 2025), found at Oracle's Elixir. The datset includes specific information on both player performance and overall team performance, serving as a useful dataset for providing insight on statistics at the professional level. As one of the most popular games across the world, the game is a great topic of interest especially at the professional level, and analysis of such data can impact the millions of users who play the game. Using the dataset, the project attempts to answer the age-old question of the game that has sparked much debate among its users: Which role has the greatest ability to carry the game?

Although a significant limitation arises from the fact that the datset includes only professional gameplay that may be far off from what actual users experience, the project was conducted on the assumption that professional play is the best representation of an "ideal" game that could arise given any game. Therefore, while the project will focus on the impact of different roles in professional games, it may be likely that similar trends will appear in games for the larger user base. Furthermore, the most important consideration for this project was the damage aspect of the five different roles. While this may disadvantage the utility roles of jungle and support, the idea of a traditional "carry" is most correlated with the damage of the given role and is one of the simplest metrics to utilize when performing analysis. 

Instead of conducting a hypothesis test on the entire five roles of the game, two most significant roles were selected based on average damage per gold by position at the end of the game. The average damage per gold was calculated using the totaldamage column divided by damagetochampions column of the given dataset. 
<iframe
  src="assets/Avg_Dpg.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

From the graph above, "mid" and "bot" roles had the greatest average damage per gold, which led to the refined question: Which lane has the greatest impact (ability to carry) a game, bot or mid?

The datset has 118932 total rows and 164 total columns, but the only relevant questions for the specific question were "position", "damagetochampions", "damageshare", and "totalgold". 

- "position": the role played by the player in the match ("team" for rows that correspond to the entire team), (string)
- "damagetochampions": the total damage done by a player to players of the opposing team during the game, (int)
- "damageshare": the total percentage of damage done by a player within the team in a single game, (int)
- "totalgold": the total gold earned by a player during the entire game, (int) 

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning
The first step in data cleaning was to only keep the necessary rows from the dataset. As the goal was to compare the carry ability of the mid and bot roles of the game, only rows where the position column was "mid" and "bot" were kept. Next, irrelevant columns were dropped with only "position", "damageshare", "damagetochampions", and "totalgold" were left in the columns. 

The remaining dataframe was checked for missing or inappropriate values, which did not exist in the dataframe.

After checking for missingness, a new column called "carry_rating" was created in order to calculate the test statistic. The new column was created using the formula of average damage per gold * damageshare. Further explanation on the choice of the test statistic is included in the hypothesis testing section. 

| position   |   damagetochampions |   totalgold |   damageshare |   carry_rating |
|:-----------|--------------------:|------------:|--------------:|---------------:|
| mid        |               13952 |        9032 |      0.278244 |       0.429812 |
| bot        |                6898 |        9407 |      0.137567 |       0.100876 |
| mid        |               15467 |       11477 |      0.288128 |       0.388296 |
| bot        |               14474 |       13632 |      0.26963  |       0.286284 |
| mid        |               33093 |       15931 |      0.33056  |       0.686663 |

### Univariate Analysis

The histogram of the total damage is skewed to the right for both mid and bot positions, meaning that there are outliers to the right of the graph for both roles. This is natural, as abnormally long games may exist where total damage is inflated while extremely short games are unlikely to occur, especially in pro play. 
<iframe
  src="assets/Dmg_Dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The histogram of the total gold seems to be normally distributed for bot positions but slightly skewed to the right for mid positions, indicating that bot lane tends to have less outliers compared to the mid lane which might indicate a greater consistent performance overall in the bot lane.
<iframe
  src="assets/Gold_Dist.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Bivariate Analysis

The scatterplot indicates that there is a small positive correlation between the total gold and damage done by champions for both mid and bot positions. While it is expected that the total gold earned should affect the damage as gold allows the player's character to become stronger, the lack of a extremely strong correlation shows that total gold is not the sole factor in determining the total damage and that both factors are related but not strongly dependent. This allows us to use both total gold and total damage when calculating the test statistic.
<iframe
  src="assets/Gold vs. Dmg.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The scatterplot indicates that there is no easily identifiable correlation between the carry rating and the total gold columns, as there seems to be no positive or negative trend. This shows that carry rating is a sufficient test statistic to be used, as it is not dominated by total gold alone and captures the difference that exist between roles. 
<iframe
  src="assets/Carry vs. Gold.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

### Interesting Aggregates

The grouped table below shows the average damageshare, damagetochampions, and total gold for bot and mid lanes respectively. When observing the values in the table, it can be seen that the bot position has overall higher values in all three columns, indicating that it is more likely for the bot position to have more impact in games. Although whether the differences hold statistical significance is unclear, it gives an initial indication that bot laners contribute more consistently to team performance and have more impact on average. 

|   damageshare |   damagetochampions |   totalgold |
|--------------:|--------------------:|------------:|
|      0.270009 |             24364.4 |     14103.4 |
|      0.257582 |             22942.8 |     13144.4 |

## Assessment of Missingness
### NMAR Analysis
There are no likely columns in the original dataset where the column is truly NMAR. The missing values in the dataset are all dependent on the "position" column, making the missingness either MAR or MD. The missingness of some values come from the fact that some metrics are unique to individual players and not the team itself, such as "playername" which has to be missing for a team as a team has no individual player name. There are also instances of some of the "dragons" columns being missing, but the "dragons (type unknown)" columns captures the unknown data in the cases where some of the columns are missing, and therefore the columns are not missing at random but dependent on the "dragons (type unknown)" column. 

### Missingness Dependency 
The chosen column for missingness analysis was the "pick1" column, which was then compared to the "result" column which indicated whether the team/player won the match. By observing the graph, it can be seen that the "pick1" column's missingness was not dependent on the "result" column, as the p value exceeds the statistical significance level, failing to reject the null hypothesis that the missingness was due to randmo chance.

<iframe
  src="assets/Missingness_result.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The "pick1" column was then compared to the "damagetochampions" column which indicated the total damage done by the team/player. By observing the graph, it can be seen that the "pick1" column's missingness is dependent on the "damagetochampions" column. This can be interpreted also as a dependence on the "position" column, as "damagetochampions" is much higher when the "position" column is "team" due to the fact that the "damagetochampions" column for a team is the sum of all the "damagetochampions" values of the individual players of a team. Therefore, the "pick1" column is dependendent on any value that is drastically different for a whole team instead of a player, making the missingness MAR. 

<iframe
  src="assets/Missingness_damagetochampions.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Hypothesis Testing
**Null Hypothesis**: Bot laners and mid laners carry their teams (have the most impact) equally on average

**Alternate Hypothesis**: Bot laners and mid laners do not carry their teams (have the most impact) equally on average

**Test Statistic**: carry_score (damage share * damage per gold)
Damage share and gold efficiency are both necessary for evaluating carry score when comparing the mid and bot roles because each metric captures a different dimension of how players impact the game. Damage share reflects how much of the team’s total damage a player is responsible for, making it a direct measure of how central that player is to the team’s overall damage output. Gold efficiency, measures how effectively a player converts the resources they receive into damage. Two players—one mid and one bot—may deal similar total damage, but if the mid laner did it with significantly less gold, that indicates a higher level of performance relative to their resources. Using only damage share would unfairly favor roles or games where one position naturally receives more gold, while using only gold efficiency would overlook players who contribute meaningfully more damage overall. By combining both metrics into a carry score, we can more fairly compare mid and bot performance and more accurately detect whether one role truly carries more on average.

**Significance Value**: 0.05
This is the default significance value in most statistical analyses to make the test not too strict nor lenient.

<iframe
  src="assets/Permutation_Test.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

**P-Value**: 0

The permutation test comparing the carry scores of mid and bot players resulted in a two-tailed p-value of 0 (to numerical precision). This means that none of the thousands of simulated differences under the null hypothesis were as extreme as the observed difference in mean carry score between the two roles. In other words, the probability of seeing a difference this large purely by chance—assuming mid and bot players have the same underlying carry ability—is effectively zero.

Because the p-value is far below the typical significance threshold of α = 0.05, we reject the null hypothesis that mid and bot players have equal carry ability. The observed difference is statistically significant and cannot be attributed to random variation alone. Instead, the result provides strong evidence that one role consistently has a higher average carry score than the other.

This outcome aligns with the underlying structure of League of Legends gameplay: the bot lane (ADC role) often receives more team resources and plays a damage-focused champion class, while mid lane performance varies more by champion archetype and game state. The result suggests that the carry score successfully captures these systematic gameplay differences and that the bot role (if higher) or mid role (if higher) genuinely carries more on average.