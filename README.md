# FIFA WorldCup 🏆 ⚽️ 🏆

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

⚽️ How many countries qualified for the World Cup in 1930 compared to 2014?

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
# Visualization:

### After my analysis in SQL, I imported the dataset into Tableau to be able to visualize some of the important aspects that I wanted to highlight in regards to this dataset:
### Here are some of the aspects I wanted to visualize:
1. The correlation between the amount of matches that were played and the amount of goals scored.
2. The attendance to the World Cup over the years.
3. The comparison in regards to home-team wins and away-team wins.
4. The several countries that won a World Cup

## Below is the dashboard that I created in Tableau:
![Dashboard 1](https://user-images.githubusercontent.com/109621999/202634794-e7ef81c5-2ad3-4bdc-8695-7df5914574f6.png)


### Here are some of my findings after analyzing this dataset:
- The World Cup with the highest attendance was the World Cup that was held in 1994, and the lowest attendance was in 1934. 
- Based on the numbers, I can confidently say that home-teams do perform better compared to away-teams. From my soccer knowledge, they get to play in their choice of jersey and the home-team side gets to sell more tickets (in some instances).
- The attendance over the years has had growing trend, but also had minor setbacks.
- The amount of goals scored, did not increase by much, even though the number of matches played did have a steady increase. 

## I hope you enjoy reading about this project as much as I had fun working on it and learning from it. 




