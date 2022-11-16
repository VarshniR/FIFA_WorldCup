# FIFA_WorldCup ⚽️

##### I have always been a huge fan of Soccer. Growing up I always watched soccer games with my family, and it was really something for us to bond over. 
We watched many games together, to the point where we went to watch live games! That was a dream come true!

While I have been practicing SQL and Tableau for a while now, it's been difficult for me to pick a project that I am very passionate about. 
But then I was thinking about the FIFA World Cup that is about to start and had the idea to work on a project that I could add to my portfolio. 

Now, is probably the time to cofess that Argentina is my favorite team 🇦🇷🇦🇷🇦🇷 
And for those that may think that it's because of the GOAT Messi, no that's not the case. 
I do think he deserves to win the World Cup, so I hope he does this coming World Cup.

I found a FIFA World Cup dataset on Kaggle (https://www.kaggle.com/datasets/abecklas/fifa-world-cup).

The dataset cointains data from every World Cup that has been played between the period 1930 - 2014. 
I want to add a detail, that the World Cups that were planned for 1942 and 1946 were cancelled due to World War 2. 


### Here are some of the questions that I tried to answer by using SQL:

⚽️ Which team won the most World Cups, and how many wins was that?
```
SELECT winner, COUNT(winner) as total_wins
	FROM worldcups
	GROUP BY winner
	ORDER BY total_wins DESC;
```

⚽️ Which Final or Semi-final games were decided by penalties?
```
SELECT home_team, away_team, win_condition as penalties
	FROM worldcupmatches
	WHERE win_condition LIKE '%pen%' 
	AND (stage = 'Final' OR stage = 'Semi-finals')
ORDER BY year;
```

⚽️ Which years had the highest and lowest attendance to the World Cup games?

⚽️ What's the average number of goals scored per World Cup?
```
SELECT ROUND(AVG(goals_scored),2) AS avg_goals
	 FROM worldcups
```

⚽️ A comparison between the total number of countries that qualified for the World Cup in 1930, compared to 2014.
```
SELECT qualified_teams AS qualified_1930, 
(SELECT qualified_teams AS qualified_2014 
 	FROM worldcups WHERE year = 2014 )
	FROM worldcups
	WHERE year = 1930
```

⚽️ Even when it is the World Cup there's always a "home-team" and an "away-team". There's always talk about how the home-team usually plays a better game, so I thought it was interesting to see the amount of games that were won, lost or a draw by home-team vs away-team.
```
SELECT 
  SUM(CASE WHEN home_team_goals > away_team_goals THEN 1 ELSE 0 END) AS Total_Home_Team_Wins,
  SUM(CASE WHEN away_team_goals > home_team_goals THEN 1 ELSE 0 END) AS Total_Away_Team_Wins,
  SUM(CASE WHEN away_team_goals = home_team_goals THEN 1 ELSE 0 END) AS Total_Draws
    FROM worldcupmatches;
```




