---
layout:  post
title:  "Fantasy Football - Tableau"
date:  2016-09-23 22:00:00 +0100
categories:  Visualisation
---

The dashboard described in this post is available on [tableau public][tableau].

### Motivation

The aim here is to create a dashboard to help decide which players to transfer in to your fantasy football team. For those who are unfamiliar, you have a set budget with which to populate your team. 
Each week your players receive points based on how well they perform.
So, the two most important questions you need to first ask are can I afford this player and what sort of points return can I expect?

Your team has to conform to a 'realistic' formation, so having exclusively strikers is not allowed. Typically you'll be looking for a player to fill a particular position so there needs to be an easy way to just compare players of the same position.

Finally, at the tail of the season there's no point buying the highest scoring player if he has performed poorly for the past few weeks. You want to get players who are on form so being able to see their recent performance is a definite plus.

So, with all these requirements in place, lets get to the dashboard!

### Points / Cost Ratio

The first port of call is giving a quick way to compare players points return relative to their cost. A scatterplot of cost versus total points gives a nice way to quickly make this comparison.
Additional dimensions can encode for the popularity (Using the bubble size) and the team (Matching the colour to the kit).

![Scatterplot](/images/scatter.png)

From this 


[tableau]: //public.tableau.com/views/FantasyPremierleagueUpdated/FantasyPremierleague?:embed=y&:display_count=yes
