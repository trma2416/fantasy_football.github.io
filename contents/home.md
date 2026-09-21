
## Introduction

**Purpose:**
The main purpose of the project is to help fantasy football players decide which players to start and which players to keep on the bench. NFL games run on a weekly cycle, so fantasy football managers need to make decisions every week on which players to start based on their potential fantasy score. 

**Key Stakeholders:**
Every year, around 30-40 million people in the US play fantasy football. These are our main stakeholders who can benefit from our project. Although our project cannot reach these millions of people, 2 out of 3 people on our team will use it regularly each week.

**Currently done & its gap:**
ESPN already provides an estimated fantasy score for every player, but it is only based on their most up-to-date average from historical data. Just taking the average is simple but a very poor way to predict. The gap in this approach is that every game is different, and there are a lot of factors that play into each game. For example, a player might score more if another specific player is also playing on their team, or if the opposing defense is ranked lowest, or it might even depend on the weather. We plan on engineering our own features that combine multiple other quantitative variables. We also plan on scraping the web for news data on specific players.

**Blueprint visual:**
![Introduction Visual](static/assets/img/Introduction%20visual.jpg)

**Blueprint explanation:**
The image above gives an overall blueprint of our project. We will first start by analyzing what exact features we are looking for. After engineering the features we want, we will start grabbing both types of data: box score data and news data. We will then use these data to do data analysis so we can connect our features with the data. The next step will be to create and execute an algorithm that uses all of the information we gathered to project estimated fantasy scores. With these predictive scores, the project will provide us with a decision on whether to start or bench a player. We will spend one week each on all of these steps. 

**Research Questions:**
- What are some specific factors that impact a player's performance?
- What is the most efficient way to scrape news data and turn it into numerical values?
- How does the result differ if we only use historical data, instead of also including new data, to predict a player's performance?
- Which machine learning model would work best for our project?
