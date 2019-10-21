---
layout: page
title: League Of Legends ARAM Match Predictor
subtitle: Intersection Between Data Science and Video Games 
---


### Background and Motivation

Within the popular game League of Legends, there is a game mode called ARAM (All Random All Middle). There are two teams with 5 players each, where all the players are given a randomly character from a set of 145 (as of patch 9.20) champions.  The team spawning at the bottom nexus is often referred to as the "blue" team and the team at the top nexus is referred as the "red" team.  The setting of the map takes place in bridge where all the players duke it out all at once. 

![GitHub Logo](/img/aram.jpg)


Lately I haven't been playing the regular summoner's rift match in League of Legends, rather opting for the ARAM map since it's faster and doesn't involve that much time investment.  Personally I find much more fun when you're constantly team battling instead of laning boringly for the 15 minutes summoners' rift. 

Of course since the characters are randomly chosen for each character, that got me thinking if you could predict which team would win given the team compositions of both teams.  I played a decent of games so I had a hunch on what teams I could win with and those I probably couldn't. But I wanted to test my own intuition vs what a machine learning model would think. 

### Where to get the data

This was an opportunity for me to work with Riot's (the company that developed the game) API. I utilized the 
[Cassiopeia](https://github.com/meraki-analytics/cassiopeia) library in Python to expedite the data extraction process. I really recommend any interested in utilizing the Riot API to use it, it makes the data extraction process so much more easy.  I couldn't find an easy way to extract all ARAM games, so I utilized a random search method to extract random games starting with a seed account (my account).  The API crawler then scans through my games and goes on to the next random player within my games.  I extracted about 100,000 ARAM games from patch 9.19 (the most recent patch) which took about 2 days.

### Design

First I thought about how I was going to design a data matrix, how to account for 2 teams, and the 135 champions.  If we let a single row represent a single match, we can define 135 columns as the features, reresenting each of the 135 champions, then each of the champions in the blue team will be marked as "+1" and the champions will be marked as a "-1", while all the other unpicked champions are left as "0".  Then the target can be a binary variable indicated whether the blue team won or loss. 

Thus for match $i$ the associated data vector is given by: X_{i,j} =

\begin{cases}
    \frac{x^2-x}{x},& \text{if } x\geq 1\\
    0,              & \text{otherwise}
\end{cases}

\begin{cases}
    \frac{x^2-x}{x},& \text{if } x\geq 1\\
    0,              & \text{otherwise}
\end{cases}

### Algorithm

For this project, I thought that utilizing logistic regression provides a highly interpretable model.

$$ \log(\frac{p}{1-p}) = \beta_0 + \beta_1 X_1 + \beta_2 X_2 + ... \beta_{145} X_{145} $$

Since the target is whether the blue team will win, $p$ is the probability of the blue team winning.  The coefficients associated with each champion has a natural interpretation of a score on well they perform in ARAM games.

For example if champion 1 is in the blue team it will contribute $+\beta_1$ to the log odds of $p$ since its adding to the chances of blue team winning, while if it's on the red team it will contribute $-\beta_1$ to the log odds of $p$. Thus the strength of a team's composition will be the sum of the 5 champion scores given by their coefficient we can rewrite the equation as:

$$ \log(\frac{p}{1-p}) = \beta_0 + (\text{sum of champion scores from blue team}) - (\text{sum of champion scores from blue team})  $$

If the sum in the right hand side is large and greater than zero, this means the blue has a higher probability of winning and vice versa.


