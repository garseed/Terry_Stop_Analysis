# Phase 2 Project

## Introduction


When given a dataset of single family home information, I decided to approach this project the way I would approach my own home search. The real estate market is overwhelming to most, so breaking down the way that I would approach this as a first time homebuyer looking for an approximation of what kind of house one could expect with their budget through three points: 

* Of the homes listed, what are the most important features to the search and which are superfluous? 
* Of the remaining features, which are the most linear with price?
* How much house can one get with their budget?

### Data limitations 
- <b> Only applicable to a specific area of Seattle </b>
<br> The dataset is only applicable to Seattle, WA, so this shouldn't be applied to anywhere outside of this area. 

### Results
<b>TITLE</b>
<br><i>Found in Home Price Analysis.ipynb</i>

#### What are the most important features when looking for a home?
<br> This dataset provides plenty of information abouth many homes listed in King County, from the number of bedrooms and bathrooms to more specific information like its latitude and longitude cooridinates and the square footage of the basement. Since the ultimate goal of the model training is for first time homebuyers trying to get a broad idea of how much home their budget will yield, the first step was to remove those data poins that are unnecesary to our goals. This included dropping:
* waterfront(If the house has a waterfront view)
* view(If the house has been viewed at all)
* yr_built (Year home was built
* yr_renovated (Year home was remodeld)
* lat (Latitude coordinate) 
* long (Longitude coordinate)
* sqft_living15 (Square footage of interior houseing living pace for nearest 15 neighbors
* sqft_lot15 (Square footage of the land lots of the nearest 15 neighbors)
<br><i>Descriptions found in data/column_names.md</i>

#### What features are the most correlated with the price of the home?
<br> Now that the extraneous pieces of data are gone, it's time to dig deeper into the remaining data. What's remaining is
* price
* bedrooms 
* bathrooms
* sqft_living
* sqft_lot
* floors
* condition
* grade

<br>After performing some initial analysis, the top three most correlated were sqft_living, grade, and bathrooms, with sqft_living being the most correlated. 

<br>With most data, there are outliers that can be removed to better fit the model. After looking at a box plot of each dataset, it was decided to remove from each dataeset:
* price: Removed values greater than 1.5 million
* sqft_living: Removed values greater than 4000
* bathrooms: Removed values greater than 3
* bedrooms: Remvoed values greater than 5
* grade: removed values greater than 10

<br>Once removed the data looks a lot better. The first model will test the relationship between price and sqft_living.

<br>The output from the model doesn't make much sense, so the next step is to log transform the price data to make the data slightly more normal. The price data could be more evenly distributed, so the best course of action would be to log transform the data. 

#### Running the Model: How much house can you get for your money?

<br>Now that the independent variable is more normal, it's time to rerun the model. 
<br>This looks better. What the model is saying is that we can explain 31% of the variance in the model, and that for every 1% increase in price, there is a 12.35 increase in square footage. 

#### Testing Linearity
<br> After running a rainbow test to determine if the model violates the linearity assumption, it's determined that the model violates the linearity assumption. 

#### Testing Homoscedasticity
<br> Visually inspecting, it the model looks like there is homoscedasticity.

#### Running the model a second time
<br> After adding 'grade' to the model, the model now reads that we can explain 39% of the variance. We have now received an warning that there may be strong multicollinearity with our data. 

#### Running the model a third time
<br> After adding the final dataset of "bathrooms", the r-squared has barely gone up and the 'bathrooms' have returned a negative coefficient. And after running the Variance Inflation Factor test, it has returned numbers greater than 5, which strongly suggest that there is multicolinearity.

## Conclusion

The goal with this project was to find the answer the question 