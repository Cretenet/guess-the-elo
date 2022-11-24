# Machine Learning Based Elo Estimator : Project Overview

- Create a tool aloowing chess players to estimate the elo (rating) of an unranked player based on their performance during a tournament. Can also be used if the player is suspected to be underrated.
- Download the data made available by lichess.com (can be found on kaggle : https://www.kaggle.com/datasets/datasnaek/chess?resource=download)
- Cleaned data and engineered features from the information available in the database
- Optimized Linear, Lasso, Ridge, Random Forest, and SVM Regressors making use of GridSearchCV to reach the best model.

<hr/>

## Ressources

- **Python Version :** 3.8.10
- **Packages :** Numpy / Matplotlib / Pandas / Scikit-Learn

<hr/>

## Dataset
Here is a list of the columns of the dataset:
* Game ID
* Rated (True / False)
* Starting Time of the game
* Ending Time of the game
* Number of Turns
* Victory status (resign / mate / outoftime / draw)
* Winner (white / black / draw)
* Game Total Time and Increment (e.g. 20 + 5 means 20 minutes of game and 5 seconds addes each turn)
* White Player ID
* White Player Rating
* Black Player ID
* Black Player Rating
* All Moves of the game (in standard chess notation)
* Opening Eco : Standardised code of the opening (see list : https://www.365chess.com/eco.php)
* Opening Name
* Opening Ply : Number of moves in the opening phase

<hr/>

## Data Cleaning and Engineering
There are no missing values in this dataset. However, some columns are so far unusable in our Machine Learning Models. The steps are described in data_exploration.ipynb but here is a list of what was done :
* Opening Name : Initially, there were way too many different names, some of which appeared only once. I decided to simplify them in the following way : OpeningName #Numbre : NameOfVariation | OtherNameOfVariation => OpeningName. After that, there were still too many different names so I kept only the onces that appeared at least 100 times (the other onces were put in the category 'other').
* Increment Code : I decided to separate the total time and the increment time in two columns : e.g. increment_code = 25 + 5 => game_time = 25 and increment = 5
* Victory_status and Winner : I wanted to see these data from white's perspective. 'winner' became 'result' and the values went from 'black/white/draw' to 'lose/win/draw'. I also created two columns : out_of_time and resign, that are true if the victory_status is one of them. If both are false and the result is win or lose, then it means that the victory_status was 'mate'
* IDs : they were obviously all dropped
* Moves : Kept but useless without a chess engine that can tell us if the players played well
* Opening_eco : Dropped since we already have the opening_name
* Dates (created_at, last_move_at) : Dropped since obviously irrelevent

<hr/> 

## Preprocessing :
