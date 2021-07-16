# Terry Stop Analysis
<br>In the 1968 Supreme Court case "Terry v. Ohio", the court found that a
police officer was not in vilation of the "unresonable search and seizure
clause of the Fourth Amendment after he stopped and frisked suspects
only because their behavior was suspicious. Thus the phrase "Terry
Stops" are in reference to stops made of suspicious drivers.
This is an analysis of over 48,000 Terry Stops, with a goal of predicting if an arrest will be made
based off time of day, whether a suspect was frisked, and racial & gender demographics of both
the suspects and officers.
The overall goal of the analysis is to have the highest possible recall, to minimize false positives,
accidentally classifying subject who were not arrested as arrested.


#### Initial Examination and Cleaning. 
<br> After performing a .head() on the data, I decided to immediately drop "Subject ID", "GO_SC_Num", "Terry Stop ID", and "Officer ID" because they were individial ID numbers and not categorical values. Next I dropped all three of the various "Call Type" columns because they were missing over 13,000 values and would give us less value to work with. After dropping I decided to do some feature engineering by organizing the "Officer YOB" data by decades. I knew I wanted to focus on arrests as the featured variable, however this dataset has two conflicting sources of arrest data, one in "Stop Resolution" and "Arrest Flag". The ratio between non-arrests and arrests in the "Arrest Flag" column was nearly 9 to 1 and would most certainly be much more difficult to work with than the 3 to 1 ratio in "Stop Resolution". After sorting all of the other non-arrest stop resolutions into their own columnn in "Stop Resolution", it was time to clean up the demographic information in the race and gender of the suspects and
officers. Every column had multiple names for the same thing "unknown". After dropping "Weapon Type" for too many null values, I f
focused on creating new features based off the "Reported Time" column, first breaking them up by hour and then divided the hour values by time of day, Morning, Afternoon, and Night. Similar to "Reported Time", I divided "Reported Date" by month. "Precinct" was lightly cleaned before "Beat", "Sector", and "Officer Squad" for reasons similar to why I dropped "Subject ID", et al. Now that the data is cleaned, it's time to move to visualizing. 

#### Data Visualization

<br> After cleaning the data and before modeling, I decided to do some visualizations of the remaining data. After removing null values, I was left with 36363 entries. The first visualization is a break down of stops that ended in arrest. 11,278 stops ended in arrest with 25,085 ending with a non-arrest. Further breakdowns showed that of the age group 26-35 had the highest percentage of arrests when divided by age of subject. White subjects were the most stopped, however American Indian/Native Alaskans had the highest percentage of arrests when stopped. Men were pulled over more likely than women, but the rate of arrest was only 3% apart. Same with gender of officer. The race of the officer, however painted a different picture. While white officers made up nearly 3 times all of the other officers combined, their rate of arrest was the same or about all races recorded, except Asian, Black, and Native Hawaiin/Pacific Islander.
27 Finally, breaking down time by AM and PM resulted in arrest rates withing 0.2%, and breakdown by hour shows that there are more arrests in the late night hours of 11pm-3am and the afternoon/evening 4pm-7pm.
28 29 30
31 #### First Model: Logistic Regression
32 <br> The inital model I went with was a Linear Regression model. After
    OneHotEncoding and scaling my data, the initial model returned and accuracy of
    57%, or just barely over a coin flip. Looking at a confusion matrix, the model
    seemed to predict an arrest when in fact the subject was not arrested. The
    other tests of Precision, Recall, and F1 score were average, hoveirng in the
    50% range.
33
34 #### Second Model: K Nearest Neighbors
35 <br> Running a K Nearest Neighbors model increased the accurcacy to 64% but
    unfortuneately knocked the other tests down to 20%-30% range. After running a
    GridSearchCV to finding the best parameters for a model and running the model
    again, the accuracy jumped up to 68%, whil tanking the recall and F1 score.
36
37 #### Third Model: XGBoost
38 <br> XGBoost gave us the best accuracy and precision yet, with 69% and 50%
    each. However recall and F1 went way dowm, sub 1%. After running a GridSearchCV
    on XGBoost, which ran for 30 minutes, I was unable to get that accuracy abou
    69%.
39
40 #### Final model: Decision Tree
41 <br> Decision fared worst, not getting us to the heights of 69% and when
    running a GridSearchCV, the model actually broke, being unable to return a
    score for recall, precision, or F1 score.


#### Conclusion. 
After running 7 models, the highest accuracy achieved was from an
XGBoost model with the best parameters found through GridSearchCV,
and even at that it was only able to be 69% accurate. 