
## Introduction
Explain the project topic, its significance, and the real-world context. Focus on a problem or decision, not on a list of algorithms.
The main purpose of the project is to help fantasy football players decide which players to start and which players to keep on the bench. NFL games run on a weekly cycle, so fantasy football managers need to make decisions every week on which players to start based on their potential fantasy score. 

Identify key stakeholders: people, organizations, communities, systems, or industries affected by the problem.
Every year, around 30-40 million people in the US play fantasy football. These are our main stakeholders who can benefit from our project. Although our project cannot reach these millions of people, 2 out of 3 people on our team will use it regularly each week.

Summarize what is already known or currently done and identify a meaningful gap or unanswered question.
ESPN already provides an estimated fantasy score for every player, but it is only based on their most up-to-date average from historical data. Just taking the average is simple but a very poor way to predict. The gap in this approach is that every game is different, and there are a lot of factors that play into each game. For example, a player might score more if another specific player is also playing on their team, or if the opposing defense is ranked lowest, or it might even depend on the weather. We plan on engineering our own features that combine multiple other quantitative variables. We also plan on scraping the web for news data on specific players.

Include at least one useful visual, diagram, map, chart, or contextual figure. Cite the source when it is not your own.

Include a short project blueprint describing what the team plans to investigate during the semester.
The image above gives an overall blueprint of our project. We will first start by analyzing what exact features we are looking for. After engineering the features we want, we will start grabbing both types of data: box score data and news data. We will then use these data to do data analysis so we can connect our features with the data. The next step will be to create and execute an algorithm that uses all of the information we gathered to project estimated fantasy scores. With these predictive scores, the project will provide us with a decision on whether to start or bench a player.

Context & Gap: While traditional fantasy management often relies on basic statistics or subjective intuition, integrating historical box scores with real-time news data offers a more comprehensive approach to evaluating player potential.Project Blueprint: During the semester, the team plans to investigate web scraping strategies for ESPN and Pro-Football-Reference, perform feature engineering on box score metrics, and build a scoring model ranging from 0 to 30.   

## Research Questions
What are some specific factors that impact a player's performance?
What is the most efficient way to scrape news data and turn it into numerical values?
How does the result differ if we only use historical data, instead of also including new data, to predict a player's performance?
Which machine learning model would best work for our project?
  
## Team
Group 2
Tristan Martinez   
Photo:
Professional bios:
Professional Links: 
Responsibilities: Data & Documentation

Nishant Devkota   
Photo:
Professional bios: Nishant Devkota is a data science master's student at the University of Colorado Boulder with a strong foundation in finance, accounting, and quantitative modeling. He brings professional experience as a Senior FP&A Analyst and Senior Data Analyst, specializing in automated reporting architectures, SQL-based data pipelines, and predictive modeling using Python, SQL, and Power BI.
Professional Links: https://www.linkedin.com/in/nishantdevkota/
Responsibilities: Coordination & Reproducibility

Tushar Koushik  
Photo:
Professional bios:
Professional Links: 
Responsibilities: Analysis/Modeling & Visualization

## Proposal Overview
Problem & Goal: 
We want a model to help us decide which players we will drop and which will play for our team. The output of the model will be the fantasy football score for players/defense. We will use historical quantitative data from box scores along with qualitative data from news. Anyone who uses fantasy football can benefit from it. The expected output is a numerical value from 0 to 30. Negative values are invalid. Any numbers above 30 are outliers 

Planned data source(s): 
The main source of our data will be ESPN. We will scrape historical box score data as our quantitative data and use their news articles for qualitative data. Some important variables: touchdowns, yards received/rushed, number of successful throws, number of successful catches, and opposing defense team rank. We will use web scraping to collect data from ESPN, subject to ping rate limits. Here are some example links:
https://www.espn.com/nfl/player/gamelog/_/id/3139477/patrick-mahomes
https://www.espn.com/nfl/scoreboard
https://www.pro-football-reference.com/players/M/MahoPa00/gamelog/2023/
As for the qualitative news, we will need to do web scraping. If web scraping with BeautifulSoup doesn't work, then we will try to connect to ESPN’s direct api links. Also, the qualitative news is more of an extra analysis, while the historical quantitative data will be used for the core analysis. We will try to gather as much data from news as possible, but a significant amount is not needed.

Current questions:
Two main questions we are looking at right now are what features we will need to engineer and what model would be most efficient.

Major constraints: 
There are three major constraints we need to look at:
1. Web scraping will be managed carefully to respect ESPN's ping rate limits.
2. Invalid, null, or outlier data will be handled through thorough data cleaning.
3. To counteract past data negatively influencing results due to player team changes over the years, model weights will be adjusted to reduce the influence of older data before 2026. 



