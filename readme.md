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
