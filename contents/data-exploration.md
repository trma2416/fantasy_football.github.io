## Data Provenance and Acquisition   
**Source:**   
API URLs:  
1. Historical game data: "https://site.web.api.espn.com/apis/common/v3/sports/football/nfl/athletes/{pid}/gamelog"  
2. Current standings: "https://site.api.espn.com/apis/v2/sports/football/nfl/standings"  
3. Next game schedule: "https://site.api.espn.com/apis/site/v2/sports/football/nfl/teams/{team_abbr_lower}/schedule"
4. Historical fantasy data (actual and projected weekly fantasy points): [2025 Fantasy Player Data](https://lm-api-reads.fantasy.espn.com/apis/v3/games/ffl/seasons/2025/segments/0/leaguedefaults/3?view=kona_player_info)
5. Game conditions (stadium, roof, surface, weather): nflverse schedules, loaded with the `nflreadpy` Python package (`nfl.load_schedules(seasons=2025)`)
   
**Date accessed:** 10/12/2026 (nflverse schedules: 10/15/2026). These results will change over time because data is added after new games are played.  
**License/term:** No formal license. The ESPN endpoints (Sources #1–#4) are public but undocumented, and ESPN's Terms of Use apply. The nflverse schedule data (Source #5) is openly published by the nflverse project.  
**Geographic and Time coverage:** No relevant geographic criteria. Time covered is the 2025 NFL regular season (weeks 1–18).  
**Approximate size:** The final cleaned dataset (`data/df_final.csv`) has 7,572 rows and 81 columns covering 685 players (QB, RB, WR, TE, K) and all 18 regular-season weeks. It is about 2.5 MB on disk and 5.9 MB in memory. The underlying game log CSV is about 1.6 MB.  
**Unit of observation:** Each row represents one regular-season game played by one player during the 2025 season (one player-game).  
**Explain why the source is appropriate for the research questions and what important information it does not contain.**  
Historical game data: This is appropriate for the research question because it looks at the underlying historical data of each player and how they have played in the past. We need this data to answer what score they might get in the next game. This information does not contain who they will play in the next game or how good the next game's team is.  
Current Standings: This is appropriate for the research question because it looks at the current ranks of different teams. This will help us answer how difficult the next game will be for our player. If the opponent's defense rank is high, then our result will be impacted. This source does not contain the future standings, and it gives end-of-season totals rather than a team's rank at the time of each game.  
Next game schedule: This is appropriate for the research question because it tells us who our player will be against for the next game. We need to analyse the next opponent, for which we need to actually know who the next opponent is. This source will give us this information. This source does not contain any other information except for just the schedule, so we will use our other data sources to cover the needed information.  
Historical fantasy data: This is appropriate for the research question because it gives the actual fantasy points each player scored each week, which is the outcome we want to predict. It also gives ESPN's projected points for that week, which we can use as a baseline to compare our model against. This source does not contain the underlying game stats, so we join it with Source #1. Some player-weeks have no projection (we fill most of these with the player's median projection; see Data Cleaning).  
Game conditions: This is appropriate for the research question because weather, roof type and playing surface may affect how a player performs (for example, wind lowers passing and kicking output). This source does not contain player-level information, and temperature and wind are not recorded for games played in domes or under closed roofs (we fill these in during cleaning).  
**Describe the acquisition method:** API (Sources #1–#4 through `requests` in `src/data_extract.py` and `src/import_hist.py`; Source #5 through the `nflreadpy` package)  

## Exploratory Data Analysis
A summary of `notebooks/FINAL_EDA.ipynb`, which uses `data/df_final.csv`. Figures are saved in `figures/`.

**Dataset overview:** 7,572 player-game rows for 685 players over 18 weeks: WR 2,806, RB 1,808, TE 1,739, QB 676 and K 543. There are no duplicate `player_id`/`season`/`week` keys and no fully duplicated rows. Stat columns that do not apply to a position are mostly missing (for example, passing stats are missing for about 91% of rows and kicking stats for about 93%). The defensive stat columns (tackles, stuffs, passes defended, interception returns) are missing for all but one row, so they are not useful for this project.

**Fantasy points (target):** No missing values. Mean 6.53, median 3.92, standard deviation 7.60, range −3.06 to 55.40, skewness 1.48. The distribution is right-skewed: 71.9% of rows are positive, 27.0% are exactly zero and 1.2% are negative.
<img src="static/assets/img/Fantasy_score_categories.png" alt="plot" width="25%">
 Zero scores are most common for TEs (40.5%) and WRs (30.9%), and rare for kickers (3.1%) and QBs (4.1%).

**By position**: 
<img src="static/assets/img/dist_fantasy_pts_by_pos.png" alt="plot" width="25%">
    
    
| Position | Rows | Mean | Median | Std. dev. |
| --- | --- | --- | --- | --- |
| QB | 676 | 13.11 | 13.11 | 9.48 |
| K | 543 | 8.27 | 8.00 | 4.80 |
| RB | 1,808 | 6.67 | 3.60 | 8.07 |
| WR | 2,806 | 5.97 | 3.40 | 7.22 |
| TE | 1,739 | 4.16 | 1.70 | 5.81 |
    
        
Scoring differs a lot by position, so position should be a model feature or models should be built or evaluated per position.

**Weekly trend**:
<img src="static/assets/img/avg_med_fantasy_pts.png" alt="plot" width="25%">
 The weekly mean stays between about 5.8 (week 18) and 7.1 (week 5), and the median between 3.3 and 4.6. There is no clear upward or downward trend. The number of rows per week changes (362 to 447) because of bye weeks and player participation.

**ESPN projections as a baseline**: 
<img src="static/assets/img/proj_vs_actual_fantasy_pts.png" alt="plot" width="25%">
 Across the 7,542 rows with a projection, projected and actual points have a correlation of 0.709, a mean absolute error (MAE) of 3.70 points and a mean error (actual − projected) of −0.19. These numbers include the projections filled with `med_proj_pts`.

    
| Position | ESPN MAE | Mean signed error (projected − actual) | Boom (≥5 above) | Near projection | Bust (≥5 below) |
| --- | --- | --- | --- | --- | --- |
| QB | 5.53 | 0.33 | 19.9% | 55.3% | 24.8% |
| WR | 3.83 | 0.45 | 12.7% | 71.4% | 16.0% |
| K | 3.82 | −0.34 | 16.9% | 70.5% | 12.5% |
| RB | 3.67 | 0.18 | 12.7% | 72.3% | 15.1% |
| TE | 2.79 | −0.13 | 11.0% | 82.1% | 6.9% |

    
<img src="static/assets/img/proj_error_ESPN.png" alt="plot" width="40%">
<img src="static/assets/img/dist_espn_proj_err.png" alt="plot" width="40%">
<img src="static/assets/img/dist_proj_err_by_pos.png" alt="plot" width="40%">
<img src="static/assets/img/boom_vs_bust.png" alt="plot" widht="40%">

Projection errors are centered close to 0 for every position, so ESPN has little overall bias. QBs have the largest errors and the most boom and bust weeks, but they also score the most points, so a fixed 5-point threshold makes their rates look larger than they are relative to their scoring.

**Home vs. away**: 
<img src="static/assets/img/home_vs_away.png" alt="plot" width="25%">
3,904 home rows and 3,668 away rows. Home averages are slightly higher for K (8.60 vs 7.91), QB, RB and WR, and slightly lower for TE (4.13 vs 4.20). The distributions overlap heavily, so any home-field effect is small.

**Modeling implications:**
- Use ESPN's projections as the baseline to beat, with time-ordered validation (train on earlier weeks, test on later weeks).
- Include position as a feature or evaluate models separately by position.
- Build features from earlier weeks only (lagged stats). Same-week stats, season-wide medians and end-of-season defense ranks would leak future information.
- Home/away and game conditions are worth testing as features, but the EDA alone does not show that they improve predictions.
