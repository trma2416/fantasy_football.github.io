
## Introduction
Explain the project topic, its significance, and the real-world context. Focus on a problem or decision, not on a list of algorithms.
The main purpose of the project is to help fantasy football players decide which players to start and which players to keep on the bench. NFL games run on a weekly cycle, so fantasy football managers need to make decisions every week on which players to start based on their potential fantasy score. 

Identify key stakeholders: people, organizations, communities, systems, or industries affected by the problem.
Every year, around 30-40 million people in the US play fantasy football. These are our main stakeholders who can benefit from our project. Although our project cannot reach these millions of people, 2 out of 3 people on our team will use it regularly each week.

Summarize what is already known or currently done and identify a meaningful gap or unanswered question.
ESPN already provides an estimated fantasy score for every player, but it is only based on their most up-to-date average from historical data. Just taking the average is simple but a very poor way to predict. The gap in this approach is that every game is different, and there are a lot of factors that play into each game. For example, a player might score more if another specific player is also playing on their team, or if the opposing defense is ranked lowest, or it might even depend on the weather. We plan on engineering our own features that combine multiple other quantitative variables. We also plan on scraping the web for news data on specific players.

Include at least one useful visual, diagram, map, chart, or contextual figure. Cite the source when it is not your own.


Include a short project blueprint describing what the team plans to investigate during the semester.



Context & Gap: While traditional fantasy management often relies on basic statistics or subjective intuition, integrating historical box scores with real-time news data offers a more comprehensive approach to evaluating player potential.Project Blueprint: During the semester, the team plans to investigate web scraping strategies for ESPN and Pro-Football-Reference, perform feature engineering on box score metrics, and build a scoring model ranging from 0 to 30.   

## Research Questions
How can we use players' past data and any ongoing news about them to influence our team selections?   Supporting Question 1: From quantitative box score data, which specific features will we need, and how are we going to weight those features?   Supporting Question 2: What is the most efficient way to scrape news data and turn it into numerical values?   

## Team
Tristan Martinez, Nishant Devkota, and Tushar Koushik (Group Number: TBD).   Profiles & Links: Professional bios, appropriate photos, and optional links (GitHub, LinkedIn, portfolios) can be added here as the website is finalized.Initial Responsibilities: Responsibilities will be distributed across data collection/scraping, feature analysis and modeling, visualization, and documentation/reproducibility.

## Proposal Overview
Problem & Goal: The primary goal is to predict fantasy football scores (with valid outputs between 0 and 30, treating negative values as invalid and values above 30 as outliers) to guide player selection and drops.   Data Sources: Historical box scores and news articles will be scraped primarily from ESPN, alongside reference data from Pro-Football-Reference. Key variables include touchdowns, rushing/receiving yards, successful throws and catches, and opposing defense team ranks.   Constraints & Risk Mitigation:Rate Limits: Web scraping will be managed carefully to respect ESPN's ping rate limits.   Data Quality: Invalid, null, or outlier data will be handled through thorough data cleaning.   Historical Bias: To counteract past data negatively influencing results due to player team changes over the years, model weights will be adjusted to reduce the influence of older data prior to 2026. 



