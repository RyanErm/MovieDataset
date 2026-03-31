# Pipeline

This file details how the Movie dataset can be used. 

Set up logging


```python
import logging
#set up logging
logging.basicConfig(
    level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s',
    filename='pipeline.log'
)
logger = logging.getLogger(__name__)
```

First load all of the data into a DuckDB database


```python
#importing 
import duckdb
import pandas as pd

#try-except blocks for error handling
try:
    con = duckdb.connect(database='movies.duckdb', read_only=False)
    con.execute("CREATE TABLE IF NOT EXISTS mov AS SELECT * FROM read_parquet('../data/mov.parquet');")
    con.execute("CREATE TABLE IF NOT EXISTS netmov AS SELECT * FROM read_parquet('../data/net_mov.parquet');")
    con.execute("CREATE TABLE IF NOT EXISTS rat AS SELECT * FROM read_parquet('../data/rat.parquet');")
    con.execute("CREATE TABLE IF NOT EXISTS users AS SELECT * FROM read_parquet('../data/users.parquet');")
    con.execute("CREATE TABLE IF NOT EXISTS history AS SELECT * FROM read_parquet('../data/watch_history.parquet');")

    con.close()
    logger.info("Loaded data into a Duckdb database")

except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")

```

Now moving on to querying and model building

Finding all ratings for movies that do appear on Netflix. This will be used as training data for the model. The query will return all columns from the users data table, the rating, all columns from the movie data table, and three columns from the users watch history.


```python
#SQL Query
try:
    con = duckdb.connect(database='movies.duckdb', read_only=False)
    #creating the database that the model will be trained upon
    net_df = con.execute("""
        SELECT 
            u.*, 
            r.rating, 
            m.*,
            h.num_movies_watched, h.total_minutes_watched
            FROM users u
        INNER JOIN history h ON h.userId = u.userId
        INNER JOIN rat r on r.userID = u.userId
        INNER JOIN mov m ON m.movieId = r.movieId
        LEFT JOIN netmov n ON n.movieId = m.movieId
        WHERE n.movieId IS NOT NULL;
    """).fetchdf()
    logger.info("Netflix Data extraction")
    #ensure that the connection is closed
    con.close()

except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")
#peak
net_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>userId</th>
      <th>job</th>
      <th>age</th>
      <th>salary</th>
      <th>rating</th>
      <th>movieId</th>
      <th>title</th>
      <th>year</th>
      <th>(no genres listed)</th>
      <th>Action</th>
      <th>...</th>
      <th>IMAX</th>
      <th>Musical</th>
      <th>Mystery</th>
      <th>Romance</th>
      <th>Sci-Fi</th>
      <th>Thriller</th>
      <th>War</th>
      <th>Western</th>
      <th>num_movies_watched</th>
      <th>total_minutes_watched</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>3.0</td>
      <td>494</td>
      <td>Executive Decision</td>
      <td>1996</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>1</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>3.0</td>
      <td>653</td>
      <td>Dragonheart</td>
      <td>1996</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>2</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>3.0</td>
      <td>778</td>
      <td>Trainspotting</td>
      <td>1996</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>3</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>3.0</td>
      <td>1391</td>
      <td>Mars Attacks!</td>
      <td>1996</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>4</th>
      <td>774</td>
      <td>Maintenance technician</td>
      <td>44</td>
      <td>22783.87</td>
      <td>3.0</td>
      <td>785</td>
      <td>Kingpin</td>
      <td>1996</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>29</td>
      <td>2987</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>



Finding all ratings for movies that do not appear on Netflix. Limiting this to the first 500,000 reviews for memory reasons. The query will return all columns from the users data table, the rating, all columns from the movie data table, and three columns from the users watch history.


```python
#SQL Query
try:
    con = duckdb.connect(database='movies.duckdb', read_only=False)
    #creating the database that the model will be tested upon
    movs_df = con.execute("""
        SELECT 
            u.*, 
            r.rating, 
            m.*,
            h.num_movies_watched, h.total_minutes_watched
            FROM users u
        INNER JOIN history h ON h.userId = u.userId
        INNER JOIN rat r on r.userID = u.userId
        INNER JOIN mov m ON m.movieId = r.movieId
        LEFT JOIN netmov n ON n.movieId = m.movieId
        WHERE n.movieId IS NULL
        LIMIT 500000;
    """).fetchdf()
    logger.info("Non Netflix Data extraction")
    #close
    con.close()
except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")
#peak
movs_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>userId</th>
      <th>job</th>
      <th>age</th>
      <th>salary</th>
      <th>rating</th>
      <th>movieId</th>
      <th>title</th>
      <th>year</th>
      <th>(no genres listed)</th>
      <th>Action</th>
      <th>...</th>
      <th>IMAX</th>
      <th>Musical</th>
      <th>Mystery</th>
      <th>Romance</th>
      <th>Sci-Fi</th>
      <th>Thriller</th>
      <th>War</th>
      <th>Western</th>
      <th>num_movies_watched</th>
      <th>total_minutes_watched</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>3.0</td>
      <td>140</td>
      <td>Up Close and Personal</td>
      <td>1996</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>1</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>5.0</td>
      <td>260</td>
      <td>Star Wars: Episode IV - A New Hope</td>
      <td>1977</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>2</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>2.0</td>
      <td>608</td>
      <td>Fargo</td>
      <td>1996</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>3</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>4.0</td>
      <td>647</td>
      <td>Courage Under Fire</td>
      <td>1996</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>4</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>3.0</td>
      <td>648</td>
      <td>Mission: Impossible</td>
      <td>1996</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>



Test to ensure that there is no overlap with movies


```python
#test
try:    
    #see if the merge returns anything
    df_merged = movs_df.merge(net_df, on='movieId', how='inner')
except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")
df_merged.shape
```




    (0, 59)



Now Cleaning the data to ensure it is ready for the model. 


```python
#peak
movs_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>userId</th>
      <th>job</th>
      <th>age</th>
      <th>salary</th>
      <th>rating</th>
      <th>movieId</th>
      <th>title</th>
      <th>year</th>
      <th>(no genres listed)</th>
      <th>Action</th>
      <th>...</th>
      <th>IMAX</th>
      <th>Musical</th>
      <th>Mystery</th>
      <th>Romance</th>
      <th>Sci-Fi</th>
      <th>Thriller</th>
      <th>War</th>
      <th>Western</th>
      <th>num_movies_watched</th>
      <th>total_minutes_watched</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>3.0</td>
      <td>140</td>
      <td>Up Close and Personal</td>
      <td>1996</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>1</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>5.0</td>
      <td>260</td>
      <td>Star Wars: Episode IV - A New Hope</td>
      <td>1977</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>2</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>2.0</td>
      <td>608</td>
      <td>Fargo</td>
      <td>1996</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>3</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>4.0</td>
      <td>647</td>
      <td>Courage Under Fire</td>
      <td>1996</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
    <tr>
      <th>4</th>
      <td>773</td>
      <td>Sales manager</td>
      <td>64</td>
      <td>49268.03</td>
      <td>3.0</td>
      <td>648</td>
      <td>Mission: Impossible</td>
      <td>1996</td>
      <td>0</td>
      <td>1</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>39</td>
      <td>4680</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 30 columns</p>
</div>




```python
try:
    #splitting into input and prediction data
    y = movs_df["rating"]
    X = movs_df.drop("rating", axis=1)

    #getting rid of large categorical columns
    X.drop(["title", "year"], axis=1, inplace=True)

    #one hot encoding the job column
    #one hot encode
    dummies = pd.get_dummies(X["job"], prefix="job")
    #make it numerical
    dummies = dummies.astype(int)
    #join the columns
    X = X.join(dummies)
    #drop the old columns
    X.drop(columns=["job"], inplace=True)

    #finally, drop the user and movie ID columns

    X.drop(["movieId", "userId"], axis = 1, inplace=True)
    logger.info("Preparing the data for model training")

except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")

```


```python
X.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>age</th>
      <th>salary</th>
      <th>(no genres listed)</th>
      <th>Action</th>
      <th>Adventure</th>
      <th>Animation</th>
      <th>Children</th>
      <th>Comedy</th>
      <th>Crime</th>
      <th>Documentary</th>
      <th>...</th>
      <th>job_Maintenance technician</th>
      <th>job_Marketing manager</th>
      <th>job_Nursing assistant</th>
      <th>job_Office clerk</th>
      <th>job_Registered nurse</th>
      <th>job_Retail sales associate</th>
      <th>job_Sales manager</th>
      <th>job_Server</th>
      <th>job_Truck driver</th>
      <th>job_Web developer</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>64</td>
      <td>49268.03</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>64</td>
      <td>49268.03</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>64</td>
      <td>49268.03</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>64</td>
      <td>49268.03</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>64</td>
      <td>49268.03</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 44 columns</p>
</div>




```python
y.head()
```




    0    3.0
    1    5.0
    2    2.0
    3    4.0
    4    3.0
    Name: rating, dtype: float64



#### Model Work

For this project, a Random Forest regressor will be used to predict the ratings of movies that do not appear on Netflix yet. A Random Forest model was chosen as it is an ensemble model that combines many smaller decision trees (which will hopefully lead to more robust/less variable predictions). Additionally it was chosen as it is a very easy model to visualize/comprehend and it can work with the large amount of data being processed in this project. Finally, a regressor was chosen because the ratings (target variable) are semi discrete (e.g. 0.5, 1.0), so a regressor is equipped to handle these values. 


```python
# Importing packages
import pandas as pd
from sklearn.ensemble import RandomForestRegressor
from sklearn.pipeline import Pipeline
from sklearn.model_selection import GridSearchCV
from sklearn.model_selection import train_test_split
from sklearn.metrics import r2_score, mean_squared_error
import matplotlib.pyplot as plt
from sklearn.tree import plot_tree
```

First the data is split up and the pipeline is built. 


```python
try:
    #further split the data to obtain a new training and testing set
    X_train_rf, X_test_rf, y_train_rf, y_test_rf = train_test_split(X, y, test_size=0.2, random_state=42)


    #create a pipeline
    pipeline_regressor = Pipeline([
        ('regressor', RandomForestRegressor(random_state=42))
    ])
    logger.info("Building the random forest model")

except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")

```

Then the optimal hyperparameters for this model are found using GridSearchCV. 


```python
try:

    #create a parameter table
    parameter_grid_regressor = {"regressor__n_estimators": [20, 100], "regressor__max_depth": [5, 10]}
    #find best parameter
    grid_search_ridge = GridSearchCV(pipeline_regressor, parameter_grid_regressor, cv=3)
    #fit
    grid_search_ridge.fit(X_train_rf, y_train_rf.values.ravel())
    print(grid_search_ridge.best_params_)
    logger.info("Finding the optimal hyperparameters")
except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")

```

    {'regressor__max_depth': 10, 'regressor__n_estimators': 100}
    

The optimal parameters are then used to train the model


```python
try:
    #grab all the optimal parameters
    best_estimators = grid_search_ridge.best_params_["regressor__n_estimators"]
    best_depth = grid_search_ridge.best_params_["regressor__max_depth"]

    #create a new pipeline that includes the optimized parameters
    pipeline_regressor_improved = Pipeline([
        ('regressor', RandomForestRegressor(n_estimators=best_estimators, max_depth=best_depth, max_features="sqrt", random_state=42))
    ])

    #fit the pipeline again with the optimized parameters
    logger.info("Refitting the model with the ideal hyperparameters")
    pip_improved = pipeline_regressor_improved.fit(X_train_rf, y_train_rf.values.ravel())

    #make predictions
    y_pred = pipeline_regressor_improved.predict(X_test_rf)

    logger.info("Predicting using the ideal hyperparameters")
    #score
    r2 = r2_score(y_test_rf, y_pred) # Predict on the test set
    print("R2 is equal to:", r2)
    #score
    MSE = mean_squared_error(y_test_rf, y_pred)
    print("MSE is equal to: ", MSE)
    
    logger.info("Getting Metrics")

except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")

```

    R2 is equal to: 0.13308367243893915
    MSE is equal to:  1.0310155599429518
    

Note for this model. The R-squared value is likely very low due to synthetic data. For an actual implementation of this model, real user data would be preferred so that the model can make better predictions. 

Time to create a visualization


```python
try:
        
    #visualize one of the trees
    rf = pipeline_regressor_improved.named_steps["regressor"]
    tree_plot = rf.estimators_[0]
    cols = X_train_rf.columns
    #using matplotlib
    plt.figure(figsize=(33, 15))
    plot_tree(tree_plot, feature_names=cols, filled=True, rounded=True, fontsize=12, max_depth = 3)
    plt.title("A Decision Tree from the Random Forest ML Model", loc="left", fontsize = 30, fontweight = "bold")
    plt.savefig("../images/tree.png")
    plt.show()
    logger.info("Creating and saving an image")

except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")
```


    
![png](pipeline_files/pipeline_28_0.png)
    


This visualization showcases a singular decision tree from the random forest model implemented above. A random forest model incorporates many individual decision trees to make its predictions. A decision tree is a very easily visualized model as it shows the logical decision breaks that lead to the decision of the model. Since it is easily visualized, users/decision makers are able to more easily comprehend how the model that they are using is working. I choose this visualization for this reason, so that the model can be more easily interpretted by people. Additionally, I ensured that the title of the visualization was off-center so as to draw more attention to it. 


### Further usage of the dataset

The dataset can also be used to find average movie ratings for different subgroups of people. Below is a SQL query that finds the average rating and number of ratings for combinations of movie genres, age ranges, and occupation. 


```python
try:
    #connect to database
    con = duckdb.connect(database='movies.duckdb', read_only=False)
    # Multi-dimensional aggregation query
    query = """
    SELECT
        u.job,
        -- Get all genres dynamically by one-hot columns
        CASE
            WHEN m.Comedy = 1 THEN 'Comedy'
            WHEN m.Drama = 1 THEN 'Drama'
            WHEN m.Action = 1 THEN 'Action'
            WHEN m.Adventure = 1 THEN 'Adventure'
            ELSE 'Other'
        END AS genre,
        CASE
            WHEN u.age < 30 THEN '<30'
            WHEN u.age < 50 THEN '30-49'
            ELSE '50+'
        END AS age_group,
        AVG(r.rating) AS avg_rating,
        COUNT(*) AS num_ratings
    FROM rat r
    JOIN users u ON r.userId = u.userId
    JOIN mov m ON r.movieId = m.movieId
    JOIN history w ON u.userId = w.userId
    WHERE w.total_minutes_watched > 500
    GROUP BY u.job, genre, age_group
    ORDER BY u.job, genre, age_group
    """
    # Execute query
    multi_dim_results = con.execute(query).fetchdf()

    # Print the first 20 rows of the multi-dimensional aggregation
    print("Average ratings by Job × Genre × Age Group (for users with >500 min watched):\n")
    #printing just the head
    print(multi_dim_results.head(20))
    logger.info("Testing a query")

except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")
```

    Average ratings by Job × Genre × Age Group (for users with >500 min watched):
    
                             job      genre age_group  avg_rating  num_ratings
    0                 Accountant     Action       50+    3.447561       279326
    1                 Accountant  Adventure       50+    3.596552        47151
    2                 Accountant     Comedy       50+    3.434071       572459
    3                 Accountant      Drama       50+    3.686488       555337
    4                 Accountant      Other       50+    3.542462       163852
    5           Accounting clerk     Action     30-49    3.454031       282035
    6           Accounting clerk  Adventure     30-49    3.611457        47911
    7           Accounting clerk     Comedy     30-49    3.439288       560869
    8           Accounting clerk      Drama     30-49    3.702389       554519
    9           Accounting clerk      Other     30-49    3.547449       165842
    10  Administrative assistant     Action     30-49    3.455937       283832
    11  Administrative assistant  Adventure     30-49    3.600449        48303
    12  Administrative assistant     Comedy     30-49    3.451646       567676
    13  Administrative assistant      Drama     30-49    3.709229       558875
    14  Administrative assistant      Other     30-49    3.564578       163593
    15                   Cashier     Action       <30    3.454158       277336
    16                   Cashier  Adventure       <30    3.590839        46610
    17                   Cashier     Comedy       <30    3.421607       561745
    18                   Cashier      Drama       <30    3.685954       548700
    19                   Cashier      Other       <30    3.540853       165289
    

This could also be easily shifted to find the exact same stats for netflix movies. This is shown below. 


```python
try:
    #connect to database
    con = duckdb.connect(database='movies.duckdb', read_only=False)
    # Multi-dimensional aggregation query
    query = """
    SELECT
        u.job,
        -- Get all genres dynamically by one-hot columns
        CASE
            WHEN m.Comedy = 1 THEN 'Comedy'
            WHEN m.Drama = 1 THEN 'Drama'
            WHEN m.Action = 1 THEN 'Action'
            WHEN m.Adventure = 1 THEN 'Adventure'
            ELSE 'Other'
        END AS genre,
        CASE
            WHEN u.age < 30 THEN '<30'
            WHEN u.age < 50 THEN '30-49'
            ELSE '50+'
        END AS age_group,
        AVG(r.rating) AS avg_rating,
        COUNT(*) AS num_ratings
    FROM rat r
    JOIN users u ON r.userId = u.userId
    JOIN netmov m ON r.movieId = m.movieId
    JOIN history w ON u.userId = w.userId
    WHERE w.total_minutes_watched > 500
    GROUP BY u.job, genre, age_group
    ORDER BY u.job, genre, age_group
    """
    # Execute query
    multi_dim_results = con.execute(query).fetchdf()

    # Print the first 20 rows of the multi-dimensional aggregation
    print("Average ratings by Job × Genre × Age Group (for users with >500 min watched):\n")
    #printing just the head
    print(multi_dim_results.head(20))
    logger.info("Testing a query")

except Exception as e:
    print(f"An error occurred: {e}")
    logger.error(f"An error occurred: {e}")
```

    Average ratings by Job × Genre × Age Group (for users with >500 min watched):
    
                             job      genre age_group  avg_rating  num_ratings
    0                 Accountant     Action       50+    3.407677        42622
    1                 Accountant  Adventure       50+    3.292008         1589
    2                 Accountant     Comedy       50+    3.409642        79002
    3                 Accountant      Drama       50+    3.726066        94143
    4                 Accountant      Other       50+    3.458280        23646
    5           Accounting clerk     Action     30-49    3.419643        43618
    6           Accounting clerk  Adventure     30-49    3.307092         1537
    7           Accounting clerk     Comedy     30-49    3.416941        78234
    8           Accounting clerk      Drama     30-49    3.742438        94447
    9           Accounting clerk      Other     30-49    3.490085        23197
    10  Administrative assistant     Action     30-49    3.436246        43174
    11  Administrative assistant  Adventure     30-49    3.335669         1570
    12  Administrative assistant     Comedy     30-49    3.433269        78247
    13  Administrative assistant      Drama     30-49    3.749345        94277
    14  Administrative assistant      Other     30-49    3.508857        23032
    15                   Cashier     Action       <30    3.421138        42720
    16                   Cashier  Adventure       <30    3.279190         1456
    17                   Cashier     Comedy       <30    3.394422        77346
    18                   Cashier      Drama       <30    3.727301        92208
    19                   Cashier      Other       <30    3.470675        23035
    
