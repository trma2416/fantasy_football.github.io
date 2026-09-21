# **Proposal Overview**

# **Problem & Goal:** 
We want a model to help us decide which players we will drop and which will play for our team. The output of the model will be the fantasy football score for players/defense. We will use historical quantitative data from box scores along with qualitative data from news. Anyone who uses fantasy football can benefit from it. The expected output is a numerical value from 0 to 30. Negative values are invalid. Any numbers above 30 are outliers 

# **Planned data source(s):** 
The main source of our data will be ESPN. We will scrape historical box score data as our quantitative data and use their news articles for qualitative data. Some important variables: touchdowns, yards received/rushed, number of successful throws, number of successful catches, and opposing defense team rank. We will use web scraping to collect data from ESPN, subject to ping rate limits. Here are some example links:
- https://www.espn.com/nfl/player/gamelog/_/id/3139477/patrick-mahomes
- https://www.espn.com/nfl/scoreboard
- https://www.pro-football-reference.com/players/M/MahoPa00/gamelog/2023/

As for the qualitative news, we will need to do web scraping. If web scraping with BeautifulSoup doesn't work, then we will try to connect to ESPN’s direct api links. Also, the qualitative news is more of an extra analysis, while the historical quantitative data will be used for the core analysis. We will try to gather as much data from news as possible, but a significant amount is not needed.

# **Current questions:**
Two main questions we are looking at right now are what features we will need to engineer and what model would be most efficient.

# **Major constraints:** 
There are three major constraints we need to look at:
- Web scraping will be managed carefully to respect ESPN's ping rate limits.
- Invalid, null, or outlier data will be handled through thorough data cleaning.
- To counteract past data negatively influencing results due to player team changes over the years, model weights will be adjusted to reduce the influence of older data before 2026. 
