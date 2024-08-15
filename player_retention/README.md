# A/B testing for player retention

### Context
This dataset includes A/B test results of Cookie Cats to examine what happens when the first gate in the game was moved from level 30 to level 40. When a player installed the game, they were randomly assigned to either gate_30 or gate_40.

### Content
The data is collected from 90,189 players that installed the game while the AB-test was running. The variables are:

- userid: A unique number that identifies each player.
- version: Whether the player was put in the control group (gate_30 - a gate at level 30) or the group with the moved gate (gate_40 - a gate at level 40).
- sum_gamerounds: the number of game rounds played by the player during the first 14 days after install.
- retention_1: Did the player come back and play 1 day after installing?
- retention_7: Did the player come back and play 7 days after installing?

Source: https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats

Acknowledgements: This dataset is provided by DataCamp
Cookie Cat, a popular mobile puzzle game developed by Tactile Entertainment.

## Inspecting the dataset and EDA
After loading the dataframe and saving it to a variable called 'df', a quick preview allows us to visualise how it presents itself and get a first understanding. The retention is recorded as boolean datatypes (True/False).

![001_dfHEAD](https://github.com/user-attachments/assets/6a638e88-fb0f-4b9f-8c06-174e8c5941c1)

A countplot of how many players were assigned the gate at level 30 ('gate_30') or the gate at level 40 ('gate_40') shows that the distribution is rather equal. Therefore, we will not need to scale the data further down the line as there are no discrepancies.

![002_gate_distribtion](https://github.com/user-attachments/assets/c61e6707-1696-46b7-b142-2d31f2aba3c8)

A look at the total number of games played by each indiviual player shows heavy outliers that make the data harder to visualise. Filtering the plot by putting a limit of 60 rounds makes the boxplot legible. However, it shifts the median to 10 rounds, whereas the median calculated from all the players is around 16 games. The third quartile shows a higher degree of variability in the total number of rounds.

![003_game_rounds](https://github.com/user-attachments/assets/676aee8c-84e6-4f1b-ad79-cad6827f08a3)

![004_describe](https://github.com/user-attachments/assets/b384d69c-44e2-4f43-b160-2c3358c92db6)

Applying a lambda function to create a new 'returned' column will count the number of players who returned at least once to the game, regardless of the gate or the day they returned.

![005_lambda_function](https://github.com/user-attachments/assets/aa9a61f2-6159-410a-bf80-b59fc84a36e4)


![006_return_count](https://github.com/user-attachments/assets/43dfe906-66e3-41a8-a6c5-653c5ca70ac6)

A final countplot visualises how many players returned at +1 or +7 days after downloading the game, depending on which level they encountered the gate.

![007_retention_plot_gate](https://github.com/user-attachments/assets/dc3438d5-5d15-4fff-a0d1-ce5943142267)

## Statistical analysis
### Defining the hypothesis

**Null Hypothesis (H₀)**: There is no difference in player behavior between the two groups (gate_30 and gate_40).  
**Alternative Hypothesis (H₁)**: There is a significant difference in player behavior between the two groups.

### Performing the analysis
I first looked at if the difference in return rates at +1 day after downloading the game was statistically significant. In order to do so, I called a contingency table and performed a Z-test.

![008_contingency_table_1d](https://github.com/user-attachments/assets/fa85a37f-1095-484a-aa60-4b78ba4b09c9)

![009_Zstat_1d](https://github.com/user-attachments/assets/1551dd80-8c8a-4e5b-9dbb-da44f0a90378)

The Z-statistic of 1.784 suggests that the observed result is 1.784 standard deviations away from the null hypothesis mean.
The P-value of 0.074 indicates that there is a 7.4% probability of obtaining a result as extreme as, or more extreme than, the observed result if the null hypothesis were true.
Given a common significance level of 0.05, we fail to reject the null hypothesis.

The result suggests that while the observed effect is noticeable, the evidence is not strong enough to conclude that there is a statistically significant effect or difference at the 0.05 level.

I repeated the operation at +7 days.

![010_contingency_table_7d](https://github.com/user-attachments/assets/704e9cec-b2ff-4a0d-8d83-3a8015ac0099)

![011_Zstat_7d](https://github.com/user-attachments/assets/de451070-0b84-460f-8deb-2f1621d53e79)

In this case, the Z-statistic is quite large and suggests the return rate at +7 days is higher for the second group ('gate_40') than the first group ('gate_30'). Furthermore, the p-value is less than 0.05 (p-value=0.002).  This implies that the observed increase in return rates for gate_40 compared to gate_30 is unlikely to be due to random variation.

### Inspecting effect size

We have established through the Z-test that there is a statistical significance in the return rates at +7 days depending on whether the gate is encountered at level 30 or level 40. However, we also need to examine the size of the effect in order to assess whether it is relevant from a business approach.  
Calculation Cohen's h will give us fruther information:

![012_cohensH](https://github.com/user-attachments/assets/446a0ed0-61dc-4762-8c79-1eafa691b495)

With a Cohen's h of 0.02, the size of the effect is quite small.

## Conclusion
Though our z-test reflects statistical significance, calculating Cohen's h shows a small practical significance. This means that moving the gate from level 30 to level 40 might not be relevant from a business perspective, especially if it entails high costs with a low return on investment. It would make sense to inspect further variables to understand which ones havec the highest impact on customer retention at the lowest business cost rates.
