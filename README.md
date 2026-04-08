# Overview of the Problem

When discussions about basketball and the NBA happen, the comparison between dynasty's is often broached. For example, could the 2016 Warriors beat the 1996 Bulls? And how would the 2015 Caveliers fare in the fight? What about in a 7 game series? Well my aim for this project was to address those questions by building a model in which a user could input the teams and the years, and get a prediction of how a 7 game series between the two teams might look. 

# Description of the Dataset and it's Manipulation

The data was downloaded from Kaggle with this link: https://www.kaggle.com/datasets/wyattowalsh/basketball
The zip file contains a lot of files, but I didn't need to use all of them for this analysis. I focused on the following files, with the following columns (I wouldn't be using all the files or columns):

COMMON_PLAYERS:
['person_id', 'first_name', 'last_name', 'display_first_last', 'display_last_comma_first', 'display_fi_last', 'player_slug', 'birthdate', 'school', 'country', 'last_affiliation', 'height', 'weight', 'season_exp', 'jersey', 'position', 'rosterstatus', 'games_played_current_season_flag', 'team_id', 'team_name', 'team_abbreviation', 'team_code', 'team_city', 'playercode', 'from_year', 'to_year', 'dleague_flag', 'nba_flag', 'games_played_flag', 'draft_year', 'draft_round', 'draft_number', 'greatest_75_flag']

GAME_INFO:
['game_id', 'game_date', 'attendance', 'game_time']

GAME:
['season_id', 'team_id_home', 'team_abbreviation_home', 'team_name_home', 'game_id', 'game_date', 'matchup_home', 'wl_home', 'min', 'fgm_home', 'fga_home', 'fg_pct_home', 'fg3m_home', 'fg3a_home', 'fg3_pct_home', 'ftm_home', 'fta_home', 'ft_pct_home', 'oreb_home', 'dreb_home', 'reb_home', 'ast_home', 'stl_home', 'blk_home', 'tov_home', 'pf_home', 'pts_home', 'plus_minus_home', 'video_available_home', 'team_id_away', 'team_abbreviation_away', 'team_name_away', 'matchup_away', 'wl_away', 'fgm_away', 'fga_away', 'fg_pct_away', 'fg3m_away', 'fg3a_away', 'fg3_pct_away', 'ftm_away', 'fta_away', 'ft_pct_away', 'oreb_away', 'dreb_away', 'reb_away', 'ast_away', 'stl_away', 'blk_away', 'tov_away', 'pf_away', 'pts_away', 'plus_minus_away', 'video_available_away', 'season_type']

INACTIVE_PLAYERS:
['game_id', 'player_id', 'first_name', 'last_name', 'jersey_num', 'team_id', 'team_city', 'team_name', 'team_abbreviation']

LINE_SCORE:
['game_date_est', 'game_sequence', 'game_id', 'team_id_home', 'team_abbreviation_home', 'team_city_name_home', 'team_nickname_home', 'team_wins_losses_home', 'pts_qtr1_home', 'pts_qtr2_home', 'pts_qtr3_home', 'pts_qtr4_home', 'pts_ot1_home', 'pts_ot2_home', 'pts_ot3_home', 'pts_ot4_home', 'pts_ot5_home', 'pts_ot6_home', 'pts_ot7_home', 'pts_ot8_home', 'pts_ot9_home', 'pts_ot10_home', 'pts_home', 'team_id_away', 'team_abbreviation_away', 'team_city_name_away', 'team_nickname_away', 'team_wins_losses_away', 'pts_qtr1_away', 'pts_qtr2_away', 'pts_qtr3_away', 'pts_qtr4_away', 'pts_ot1_away', 'pts_ot2_away', 'pts_ot3_away', 'pts_ot4_away', 'pts_ot5_away', 'pts_ot6_away', 'pts_ot7_away', 'pts_ot8_away', 'pts_ot9_away', 'pts_ot10_away', 'pts_away']

OTHER_STATS:
['game_id', 'league_id', 'team_id_home', 'team_abbreviation_home', 'team_city_home', 'pts_paint_home', 'pts_2nd_chance_home', 'pts_fb_home', 'largest_lead_home', 'lead_changes', 'times_tied', 'team_turnovers_home', 'total_turnovers_home', 'team_rebounds_home', 'pts_off_to_home', 'team_id_away', 'team_abbreviation_away', 'team_city_away', 'pts_paint_away', 'pts_2nd_chance_away', 'pts_fb_away', 'largest_lead_away', 'team_turnovers_away', 'total_turnovers_away', 'team_rebounds_away', 'pts_off_to_away']

PLAY_BY_PLAY:
['game_id', 'eventnum', 'eventmsgtype', 'eventmsgactiontype', 'period', 'wctimestring', 'pctimestring', 'homedescription', 'neutraldescription', 'visitordescription', 'score', 'scoremargin', 'person1type', 'player1_id', 'player1_name', 'player1_team_id', 'player1_team_city', 'player1_team_nickname', 'player1_team_abbreviation', 'person2type', 'player2_id', 'player2_name', 'player2_team_id', 'player2_team_city', 'player2_team_nickname', 'player2_team_abbreviation', 'person3type', 'player3_id', 'player3_name', 'player3_team_id', 'player3_team_city', 'player3_team_nickname', 'player3_team_abbreviation', 'video_available_flag']

PLAYER:
['id', 'full_name', 'first_name', 'last_name', 'is_active']

TEAM:
['id', 'full_name', 'abbreviation', 'nickname', 'city', 'state', 'year_founded']

The dimensions of the files are as follows:

COMMON_PLAYERS: (4171, 33)
GAME_INFO: (58053, 4)
GAME: (65698, 55)
INACTIVE_PLAYERS: (110191, 9)
LINE_SCORE: (58053, 43)
OTHER_STATS: (28271, 26)
PLAY_BY_PLAY: (13592899, 34)
PLAYER: (4831, 5)
TEAM: (30, 7)

# Analysis Overview

The first step in the analysis was to identify the starters of every team in every game. This way, we can compare the most starters to the win loss ratios and see what the best starter team for a particular team is, during a particular season of course. To do this, I opened the play_by_play file (which details every play of a every game) and filtered for period 1. Then I counted the players that showed up in period 1, and decided that the 5 players who showed up the most would be considered the starters of the game. I did this for both the home and away team. 

I then had to create a stats report for the players. To do this, I had to count up the points, rebounds, assists, blocks, and steals. This would eventually be added to the other players' stats sheets to come up with a metric for how good the team is playing as a whole. 

The next step was to get the actual names of the starters. Up until now, I had just been using player and team IDs. So I had to look into the play_by_play file and the player file to get the names of the players.

Now once I had the players' names and statesheets, I had to figure out what the optimal starting lineup for a team is depending on the season. I averaged out the statistics of the players per season, and compared the averages between each players, selecting the best 5 as part of the optimal lineup. I then summed up each person's average stats into one statline as the best possible statline for that team. 

I settled on a random forest classifier over a logistic regression as it would better account for the noise. I split the data into 80% training and 20% testing, eventually getting a model accuracy of
0.5974811176842654. One important thing to note is that the statistics (points, assists, rebounds, blocks, steals) had to be scaled so that the points don't outweigh the other metrics. I then moved onto a simulation, wherein a user can input the team 1's ID, team 1's year, team 2's ID, and team 2's year, and then get a prediction of over 7 games, who will win the series. An example is this: 
run_simulation(1610612744, 2016, 1610612760, 2010), which matches up the 2016 Warriors with the 2010 Thunder. The output looks like this:

Matchup: 2016 Golden State Warriors vs. 1998 Oklahoma City Thunder
Golden State Warriors Starters: Kevin Durant, Stephen Curry, Draymond Green, Klay Thompson, Harrison Barnes
Oklahoma City Thunder Starters: Gary Payton, Vin Baker, Detlef Schrempf, Hersey Hawkins, Dale Ellis
-------------------------------------------------------------------------------------------------
Stats A: [107.0306439   31.45030781  21.55303159   3.31268809   6.63507847]
Stats B: [78.40934405 26.19435212 17.3253742   1.7709223   6.11143151]
Team 1 is 58.55% favorite when playing on a nuetral court
-------------------------------------------------------------------------------------------------
Game 1: Team 1 wins
Game 2: Team 2 wins
Game 3: Team 1 wins
Game 4: Team 1 wins
Game 5: Team 1 wins
Final Result: Team 1 wins the series 4 - 1!

All the required dependencies are written into the code, so running the code will run the downloads. But an overview is that polars (to scan the files quickly), numpy, StandardScaler, RandomForestClassifier, and train_test_split are required. 

# Tradeoffs

Some tradeoffs were made during this analysis. The first being that not too many variables would be used outside of home court advantage and the players themselves. This is because the amount of game data was so much that any little change in win loss percentage might make a variable seem significant when it's actually not. A second tradeoff that I had to make was not to study inactive players. The original goal of this project was to identify the best starting five for the Warriors depending on how many players were inactive, and what team they faced. But I decided against that when I realized that it would be hard to exclude the inactive players completely, as they might have just been out for a couple of months or so. Another thing is that the data is not completely up to date, which means that players who were drafted into the league in 2024 and past are not included in the data sets. That means that it's harder to see the impact of inactive players in todays games.


Presentation link: https://docs.google.com/presentation/d/1gqwf-ev6d43oTii71yMYnCAbzTojujcdv49mHZwBpG4/edit?usp=sharing
