# Classification of Spotify Song Genres

Andrew Wong

## Abstract

The goal of my project was to use classification models to accurately predict the genres of songs from Spotify. The core algorithms of the model can benefit Spotify's auto-generated user playlists by classifying songs from the same genre and/or songs with similar sonic qualities. For this multiclass program, I achieved the highest accuracy using a gradient boosted trees model.

## Design

The metric I chose to focus on when comparing model performance was accuracy. When generating a suggested playlist for Spotify users we want the system to accurately suggest songs from the same genre, or at least suggest songs that generally sound similar if missclassified. Having an accurate classifcation model behind Spotify's suggestions algorithms can improve the overall user experience, and can make the platform more preferred over other music streaming services.

## Data

I downloaded this Spotify dataset as CSV from the /r/datasets subReddit. After cleaning and narrowing down the data, the dataset ended up with 25809 songs, 12 features for each, with 9 genre classes.

The genres were: 'Emo', 'Pop', 'Underground Rap', 'dnb', 'hardstyle', 'psytrance', 'techhouse', 'techno', 'trance'

Among the genres there were class imbalances in the dataset. With 'Underground Rap' (UR) being the largest, with a ratio of about 12:1 for UR:Pop, 3.5:1 for UR:Emo, and roughly 2:1 for UR:the rest of the genres.

Some feature highlights were: danceability, energy, key, loudness, tempo, valence, duration.

## Algorithms/Models

The entire dataset was split into 80/20 train vs. test holdout.

#### KNN

The model I started with as my baseline was K-Nearest Neighbors with N neighbors = 5. I regularized the feature values using a StandardScaler. I then tried KNN with cross validation and KNN using GridSearchCV which both produced slightly lower accuracy scores. The baseline KNN model performed the best with an accuracy score of 0.835 on the test data.

#### Random Forest Classifier

The next model I used was Random Forest Classifier with N estimators = 400, which produced significantly better results than the KNN models.

Test Set Accuracy =  0.92 <br>
Test Set F-score =  0.85 <br>
ROC AUC = 0.986 <br>

#### Gradient Boosted Trees - XGBoost

I wanted to try and further improve on the Random Forest model by using XGBoost. After some manual parameter tuning, the model was able to produce slightly better results:

Test Accuracy: 0.923
Test F1 Score: 0.856
Test Precision: 0.884
Test F1 Score: 0.856

I then wanted to test the performance of Random Oversampling on the XGBoost model to adjust for the class imbalances. I upsampled the other classes to the same size as the largest class (Underground Rap). This yielded marginally worse results on the test set:

Test Accuracy: 0.919
Test Precision: 0.858
Test Recall: 0.86
Test F1 Score: 0.859

# Results

The XGBoost model was my final model of choice, as it ultimately produced the best accuracy score of 0.923
Across all models, the Pop genre class had the lowest predicted accuracy. This may be partly due to fewer data points. The majority of misclassified Pop songs were classified as Underground Rap across all models. This may be because Pop is a general term or sub-genre under a wide range of genres. Since Rap/Hiphop consistently make the top music charts in the U.S. and are often sub-categorized as Pop, this may explain the errors in Pop classification.

Top 3 Most Accurate: <br>
Drum and Bass = 0.99 <br>
Hardstyle = 0.99 <br>
Psytrance = 0.94<br>

I plotted the feature importance on both F score and gain metrics. For gain, the top 5 most important features were Tempo, Instrumentalness, Duration, Danceability, and Speechiness. Tempo was the most significant feature in predicting the genre with an F score about 1.5x higher than Instrumentalness.

## Tools

Numpy, Pandas, SqlAlchemy for data manipulation <br>
Scikit-learn for modeling <br>
Matplotlib and Seaborn for plotting <br>
