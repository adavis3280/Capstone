## Creating an MLB At-Bat Simulator

The goals of this repository were to create a model and resulting function capable of simulating Major League Baseball (MLB) at-bats given a pitcher and a hitter and their relevant statistics.

Models used:
Neural Network from keras Library

Python libraries used:
Pandas
Numpy
Scikitlearn
Matplotlib
Seaborn
Keras

## Steps Involved

## Gather Data
Data was gathered from a number of sources of baseball data is widely available.

Play by play data to determine at bat outcomes was obtained from retrosheet.org and from mlb.com's database. The database contains every at bat from 2013-2018 in all Major League Baseball

Player-level statistical data was obtained from fangraphs.com, as well as Steamer player projections also obtained from the same source.

Statcast physical data was obtained from the Baseball Savant database located at mlb.com

All data was in downloadable csv format and no scraping was necessary.

Links:
https://www.fangraphs.com/leaders.aspx?pos=all&stats=bat&lg=all&qual=y&type=0&season=2019&month=0&season1=2019&ind=0&team=0&rost=0&age=0&filter=&players=0&startdate=2019-01-01&enddate=2019-12-31

https://www.fangraphs.com/projections.aspx?pos=all&stats=bat&type=steamer

https://www.retrosheet.org/game.htm

https://baseballsavant.mlb.com/statcast_leaderboard


## Exploratory Data Analysis

The primary focus of this section was to attach the appropriate statistical data to each row in the events/outcomes database from mlb.com.

First, the outcomes databased was changed from having 25 possible outcomes of an at bat to 11 distinct ones for modeling purposes. Some events that are dependent on game context, like 'Sacrifice Bunts' were dropped entirely.

The final classifications of at bat outcomes are:
'GB', '2B', '1B', 'K', 'BB', 'PU', 'FB', 'LD', 'HR', 'HBP', '3B'.

Data from fangraphs and statcast were merged to create databases with each row containing around 150 statistics for each player-season. (ie Mike Trout, 2015). This is done in Fangraphsdataclean.ipynb and saved in MergedHitters.csv and MergedPitchers.csv.

Data mentioned above from fangraphs and statcast were merged into the events database, mlb_PA_2013to2018.csv in FinalMLBDatabaseCreation.ipynb, so that each event had a pitcher statline for the season of the event attached, as well as a mirroring hitter statline.

 Final cleaned data will be saved to mlbxdb.csv in the data folder after running through the Fangraphsdataclean.ipynb and the FinalMLBDatabaseCreation.ipynb


## Model/Evaluate the Data
A large neural network from the Keras library was compiled and fit in NNetFitFunctioCreation.ipynb. The 'Softmax' multiclassifier was used to give each potential at bat outcome a probability. Parameters of this model are still being changed and could potentially require tuning.

## Create the function

Two functions were created in this step in NNetFitFunctioCreation.ipynb.

 --Construct Input()
 This function takes in a descriptive event vector, a batter stats vector, and a hitter stats vector,
 and constructs them into the correct data format to be fed into a fit neural network model to make predictions.

--simAB()
This function takes a batter name and a pitcher name and simulates the interaction of those two players.
The function requires specification of if either party is left-handed.
The function can be customized in the following ways:

pseason: The season in which the named pitcher's data
is pulled from so at bats from past versions of players can be simulated

hseason: Same as above, but for the hitter

season: The season in which the interaction takes place can be specified, as different years have
different baseline occurrence rates for each event

output: This tuple formats the output of the function
Term 1 can be one of three values.
    1. 'probs' will give a probability distribution of each event of the simulated at bat
    2. 'pa' will simulate one plate appearance randomly using the above distribution and give a result
    3. 'statline' will summarize this output into more common baseball statistics

Term 2 is the number of at bats over which this simulation occurs.
'probs' will not change regardless of this number
