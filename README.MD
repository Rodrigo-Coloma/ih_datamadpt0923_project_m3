# Kaggle diamonds competition

##  Status
This is an ongoing kaggle competition between ironhack students where
 the goal is to predict the prize of diamonds. You can follow the [link](https://www.kaggle.com/competitions/ihdatamadpt0923projectm3/leaderboard) for more information.

![Example Map](data/src/diamonds_image.webp)


## Data source
We were probided with a database file containing 1 transactional and 6 dimension tables. When joined together they created a dataframe with 40455 diamond sales with the price at which the diamond was sold and 9 features of the diamond:


- price: price in USD
- carat: weight of the diamond
- cut: quality of the cut (Fair, Good, Very Good, Premium, Ideal)
- color: diamond colour, from J (worst) to D (best)
- clarity: a measurement of how clear the diamond is (I1 (worst), SI2, SI1, VS2, VS1, VVS2, VVS1, IF (best))
- x: length in mm
- y: width in mm
- z: depth in mm
- depth: total depth percentage
- table: width of top of diamond relative to widest point
- city: city where the diamonds is reported to be sold.


## Technology Stack
Main language used is **Python** altoough a **SQL** was required to create the base dataset. The main libraries involved have been pandas and numpy for data wrangling and sklearn, xgboost, temnsorflow and lightgbm to construct the candidate models.

## Configuration
 To run the script locally, clone the repository, create a virtual environment, install Python, and run `pip install -r requirements.txt` in the project folder.

## Pipeline

After cleaning and observing the data the data it is preprocessed as follows:

### Outlier management

For every column with numerical values we replace all the extreme values (less than the mean minus 3.7 times the standard deviation or highr than the mean plus 3.7 times the standard deviation)

### Label encoding

The four categorical features are encoded according to the values especified in data section
As we expect a non linear relasionship between the price and the classes (ideal is not 5 times more expensive than fair) we asign the worst class a 1 and the rest the square root of their position in the list.

### Feature engineering

We add all the features that appear to have some meaning. For features that have synergy between them, we multiply them. Also, we creaye x/y proportion as it significantly impacts the price of the diamond

added features:

- x/y
- table * depth
- carat / volume
- carat * clarity
- color * cut
- color * clarity
- color * cut * clarity

### Model selection
After trying numerous models the one that achieves best result and therefore the chosen one is XGBoost.


### Model tuning

Thanks to GridserachCV we find two different sweet spots for the model , with the following hyperparameters:

```
Parameters 1:  {'colsample_bytree': 0.62, 'gamma': 0, 'learning_rate': 0.0065, 'max_depth': 7, 'missing': inf, 'n_estimators': 2100, 'subsample': 0.95}
Score 1:  -506.43566728045136

Parameters 2:  {'colsample_bytree': 0.76, 'gamma': 0, 'learning_rate': 0.0126, 'max_depth': 7, 'missing': inf, 'n_estimators': 1150, 'subsample': 0.95}
Score 2 :  -508.06598813979053
```

## Folder Structure

```
└── project
    ├── __wip__
    ├── .venv
    └── data
    │    ├── src
    │    └──clean
    ├── notebooks
    │   ├── data_wrangling.ipynb
    │   ├── dlightgbm.ipynb
    │   ├── tabnet.ipynb
    │   ├── tensorflow.ipynb
    │   ├── voting.ipynb
    ├── submissions
    ├── .gitignore
    ├── main_pipeline.ipynb
    ├── LICENSE
    ├── README.md
    ├── requirements.txt
```
## License
[License](LICENSE.txt)