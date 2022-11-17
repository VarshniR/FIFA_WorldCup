# FIFA_WorldCup ⚽️

##### I am a huge soccer fan, and very excited about the upcoming World Cup games. 
I have always enjoyed watching soccer games, and thought with the upcoming FIFA World Cup this would be the perfect time to look into data from previous World Cups. 

Now, is probably also the best time to confess that Argentina is my favorite team 🇦🇷🇦🇷🇦🇷 
And for those that may think that it's because of the GOAT Messi, no that's not the case. 
I do think he deserves to win the World Cup, so I hope he does this coming World Cup 🤞🤞


I downloaded a FIFA World Cup dataset on Kaggle (https://www.kaggle.com/datasets/abecklas/fifa-world-cup).

The dataset cointains data from every World Cup that has been played between the period 1930 - 2014. 
An important remark regarding the data, due to World War 2 the World Cup games that were planned for 1942 and 1946 were cancelled. 

After carefully cleaning and wrangling the data in Excel, I imported the data into SQL to analyze the data and do some visualizations with Tableau. 


### Here are some of the questions that I asked myself:

⚽️ Which team won the most World Cups, and how many wins was that?

⚽️ Which Finals or Semi-final games were decided by penalties?

⚽️ Which years had the highest and lowest attendance to the World Cup games?

⚽️ What's the average number of goals scored per World Cup?

⚽️ A comparison between the total number of countries that qualified for the World Cup in 1930, compared to 2014.

⚽️ Does history show more home-team wins or away-team wins?


#### I utilized my SQL knowledge and was able to find answers for all the questions above and many more. 
Here's some of the queries that I used:

1. To answer the country that won the most World Cups and to find out what the number of that was I used the COUNT command and ordered them in DESCENDING order.
```
SELECT winner, COUNT(winner) as total_wins
	FROM worldcups
	GROUP BY winner
	ORDER BY total_wins DESC;
```
2. To find out which Finals and/or Semi-final games were decided by penalties I used the LIKE, AND/OR and WHERE commands and had the results ORDERED BY year.
```
SELECT home_team, away_team, win_condition as penalties
	FROM worldcupmatches
	WHERE win_condition LIKE '%pen%' 
	AND (stage = 'Final' OR stage = 'Semi-finals')
ORDER BY year;
```
3. To figure out which years had the lowest and highest attendance to the World Cup games, I used the UNION ALL command and had to solve it in a subquery and I ordered the results in ASCENDING order.
```
SELECT year, attendance
FROM worldcups
WHERE attendance = (SELECT MAX(attendance) FROM worldcups)
UNION ALL
SELECT year, attendance
FROM worldcups
WHERE attendance = (SELECT MIN(attendance) FROM worldcups)
ORDER BY attendance ASC;
```
4. To find out what the average number of goals scored is per World Cup I used the AVG command and to make it easy on the eye I used the ROUND command to keep the result to 2 decimals.
```
SELECT ROUND(AVG(goals_scored),2) AS avg_goals
	 FROM worldcups;
```
5. Even when it is the World Cup there's always a "home-team" and an "away-team". There's always talk about how the home-team usually plays a better game, so I thought it was interesting to see the amount of games that were won, lost or a draw by home-team vs away-team. To analyze the correlation between home-team wins, away-team wins and draws I used the CASE WHEN command in combination with SUM. 
```
SELECT 
  SUM(CASE WHEN home_team_goals > away_team_goals THEN 1 ELSE 0 END) AS Total_Home_Team_Wins,
  SUM(CASE WHEN away_team_goals > home_team_goals THEN 1 ELSE 0 END) AS Total_Away_Team_Wins,
  SUM(CASE WHEN away_team_goals = home_team_goals THEN 1 ELSE 0 END) AS Total_Draws
    FROM worldcupmatches;
```

