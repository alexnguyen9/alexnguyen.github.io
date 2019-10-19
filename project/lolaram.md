---
layout: page
title: League Of Legends ARAM Match Predictor
subtitle: Intersection Between Data Science and Video Games 
---


### Background and Motivation

Within the popular game League of Legends, there is a game mode called ARAM (All Random All Middle). There are two teams with 5 players each, where all the players are given a randomly character from a set of 145 (as of patch 9.20) champions.  The team spawning at the bottom nexus is often referred to as the "blue" team and the team at the top nexus is referred as the "red" team.  The setting of the map takes place in bridge where all the players duke it out all at once. 

![GitHub Logo](/img/aram.jpg  | width=100)

![](https://gyazo.com/eb5c5741b6a9a16c692170a41a49c858.png | width=100)

Lately I haven't been playing the regular summoner's rift match in League of Legends, rather opting for the ARAM map since it's faster and doesn't involve that much time investment.  Personally I find much more fun when you're constantly team battling instead of laning boringly for the 15 minutes summoners' rift. 

Of course since the characters are randomly chosen for each character, that got me thinking if you could predict which team would win given the team compositions of both teams.


### Design

First I thought about how I was going to design a data matrix, how to account for 2 teams, and the 135 champions.  If we let a single row represent a single match, we can define 135 columns as the features, reresenting each of the 135 champions, then each of the champions in the blue team will be marked as "+1" and the champions will be marked as a "-1", while all the other unpicked champions are left as "0"

### Algorithm
