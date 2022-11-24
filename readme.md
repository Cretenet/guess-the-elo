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
* **Opening Name** : Initially, there were way too many different names, some of which appeared only once. I decided to simplify them in the following way : OpeningName #Numbre : NameOfVariation | OtherNameOfVariation => OpeningName. After that, there were still too many different names so I kept only the onces that appeared at least 100 times (the other onces were put in the category 'other').
* **Increment Code** : I decided to separate the total time and the increment time in two columns : e.g. increment_code = 25 + 5 => game_time = 25 and increment = 5
* **Victory_status and Winner** : I wanted to see these data from white's perspective. 'winner' became 'result' and the values went from 'black/white/draw' to 'lose/win/draw'. I also created two columns : out_of_time and resign, that are true if the victory_status is one of them. If both are false and the result is win or lose, then it means that the victory_status was 'mate'
* **IDs** : they were obviously all dropped
* **Moves** : Kept but useless without a chess engine that can tell us if the players played well
* **Opening_eco** : Dropped since we already have the opening_name
* **Dates (created_at, last_move_at)** : Dropped since obviously irrelevent

<hr/> 

## Preprocessing :
Before building the actual model, it was important to preprocess the data. I therefore decided to add a preprocessor to the pipeline. The preprocessor applied the following transformations :
* **'nb_turns', 'black_rating', 'nb_opening_moves', 'game_time', 'increment'** : Standardization, because they were all on different scales
* **'result'** : Since this categorical variable is ordinal (lose < draw < win), I used OrdinalEncoder()
* **'opening_name'** : I one hot encoded this categorical variable since it is not ordinal
Since sklearn does not handle target scaling in the piepline, the target also has to be manually standardized.
It is important to note that I splitted the dataset into a test set and a train set **before** the preprocessing (test_size=0.2)

<hr/>

## Model Building:
I first built different models and optimized them using GridSearchCV. Here is the list of the models I built and the optimized parameters (parameters that I tried to optimize but did not change anything to the performance are not mentionned here) :
* **Multiple Linear Regression** : Always start with the simplest. No parameter to adjust here.
* **Ridge Regression (alpha = 32.0)** : Mitigates the problem of multicollinearity.
* **Lasso Regression (alpha = 0.0001)** : Tends to select only the most usefull features
* **Random Forest Regression (n_estimators=500, max_features='sqrt', max_depth=40)** : Performs usually well when there are many dummy variables (like in this case)
* **SVM Regression (kernel='rbf', C=1.0)** : Allows to perform the kernel trick to see if adding dimensions to the problem helps to perform well.
The R2 metric was used to choose the best possible parameters.
* **VotingRegressor** : Use the best Linear, SVM and RandomForest together to see if it is possible to achieve a better result. (no Ridge and Lasso because they performed exactly like Linear)

<hr/>
