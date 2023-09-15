# Microsft Movie studio insight

## Introduction

Welcome to the Movie review Project! This repository contains the findings and insights from our in-depth exploration of movie data. This project provides valuable information and trends within the cinematic world to help us in building a movie studio

## Overview

Microsoft's business case involves creating a new movie studio, but the company lacks expertise in filmmaking. To address this challenge, the project's goal is to explore and analyze existing data related to the film industry. By conducting exploratory data analysis (EDA), the project seeks to identify trends, patterns, and factors associated with successful movies in terms of box office performance.Microsoft aims to create and produce movies that resonate with audiences and achieve success at the box office. This industry is highly competitive and influenced by various factors, including film genres, release strategies, marketing, and audience preferences.

## Business understanding

What is microsoft looking to achieve in the movie industry ?

The primary business goal is to determine what types of films perform well at the box office and translate these findings into actionable recommendations for Microsoft's new movie studio in order for them to create successful movies that generate significant revenues.
In this analysis, we are going to look at movie data that have been provided from various sources in order to determine how a microsoft movie studio can be established and generate tremendous revenue. What should be done in order for it to be successful like other movie studios like Warner Bros studio.

## Data Understanding and analysis


The data sources for this analysis was pulled from various files which include:

#### ['im.db'](https://www.imdb.com/)
#### ['bom.movie_gross.csv.gz'](https://www.boxofficemojo.com/)
#### ['rt.movie_info.tsv.gz'](https://www.rottentomatoes.com/)
#### ['tmdb.movies.csv.gz'](https://www.themoviedb.org/tv/airing-today)
#### ['tn.movie_budgets.csv.gz'](https://www.the-numbers.com/)

## Getting the Data

we start by importing the neccesary Libraries

```python
import pandas as pd
import numpy as pd
import sqlite3

Next, we load our datasets



df1 = pd.read_csv('bom.movie_gross.csv.gz')
df1.head()


df2 = pd.read_csv('tmdb.movies.csv.gz')
df2.head()

```

df3 = pd.read_csv('tn.movie_budgets.csv.gz')
df3.head()



query = "SELECT * FROM movie_basics"
df4 = pd.read_sql_query(query, conn)
df4.head() 



query = "SELECT * FROM movie_ratings"
df5 = pd.read_sql_query(query, conn)
df5.head()


After loading our dataset we try and merge the data frames in order to get a single dataset




import pandas as pd

Combine the dataframes using pd.concat() based on movie_id
combined_df = pd.concat([df4, df5, df1], axis=1, join='inner')

To remove duplicate columns (movie_id in this case) from the merged dataframe
 
combined_df2 = combined_df.loc[:, ~combined_df.columns.duplicated()]

 Display the first few rows of the combined dataframe
 
movies_reviews_df = combined_df2
movies_reviews_df.head()


Now we have our data set but before we start our analysis we must first clean our data in order to get the right analysis. 


 ## Univariate Analysis
 
 It gives us the characteristics and behaviour of a single variable

we will start by plotting histograms to visualize and compare how different variables work  in the dataframe movies_review_df  i.e variables that contain floats and integers




plotting histograms to visualize patterns in the data
import matplotlib.pyplot as plt
df20.hist(figsize = (20,10), bins= 24)
plt.show()


## Conclusions from our 1 st univariate analysis 

The histogram for the start year shows that the data is distributed across the years 2010 to 2019. There is a relatively even distribution of movies over this period, with some variation.

The histogram for runtime minutes reveals that most movies have a runtime between 80 to 100 minutes. There are fewer movies with extremely short or long runtimes, indicating a peak around the average runtime. This means its has a symmetrical distribution

The histogram for average ratings shows a distribution that is slightly left-skewed. Most movies have ratings between 6.5 and 7.5, with fewer movies receiving very low or very high ratings.


The next univariate analysis will be on movie release year trends. A line graph will be plotted in order to visualise the trend in movie years

 


 Explore movie release year trends
 
year_trends = movies_reviews_df['start_year'].value_counts().sort_index()
year_trends


 Plotting year_trends as a line chart
import matplotlib.pyplot as plt

plt.figure(figsize=(12, 6))
plt.plot(year_trends.index, year_trends.values, marker='o', linestyle='-')
plt.title('Movie Release Year Trends')
plt.xlabel('Year')
plt.ylabel('Number of Movies Released')
plt.grid(True)
plt.show()



In the line chart above we can see there is a steady decrease in movie release since 2010 to 2016 but after 2016 there have been an increase in movie releases. 2019 had the highest number of movies released ever according to the analysis. We can identify specific years with spikes or drops in movie releases, which may be due to factors such as major industry events, economic conditions, or cultural trends. We can analyze the general pattern of movie releases, such as whether there are cyclical patterns or a continuous upward trend. Based on these observations, we can draw conclusions about the historical trends in movie releases and potentially make hypotheses about the underlying factors driving these trends.


We are also going to explore the popularity of genres in this univariate analysis by plotting a graph which will enables us visualize and know which genres are prefered.





 #Explore the popularity of movie genres
import matplotlib.pyplot as plt

      #Split and explode genres, then count the frequency of each genre
genres = movies_reviews_df['genres'].str.split(',').explode().str.strip()
popular_genres = genres.value_counts()

     #Plot the popular genres as a bar chart
plt.figure(figsize=(12, 6))
popular_genres.plot(kind='bar', color='skyblue')
plt.title('Popularity of Movie Genres')
plt.xlabel('Genre')
plt.ylabel('Number of Movies')
plt.xticks(rotation=45)
plt.grid(axis='y')
plt.show()     





we can conclude that according to our data the most viewed genre is drama the followed by documentary.This means that most viewers and film makers prefer drama and documentaries because they are the most watched genre compared to the others. What we actually did was we re arranged the genres into a descending order and plottted it against the number of movies in oorder to get the best visualization of our data I would advise my colleagues that we should focus mainly on drama documentaries and comedy in order to get more views and more sales for as to flourish and meet our goals as a company



   ### Bivariate analysis
shows relationship between two variables

 we look and analyze movie performance by studio based on total domestic gross earnings by plotting a bar chart.

 
 
     Analyze movie performance by studio based on total domestic gross earnings
import matplotlib.pyplot as plt

# Group data by studio, sum the domestic gross earnings, and sort in descending order
studio_performance = movies_reviews_df.groupby('studio')['domestic_gross'].sum().sort_values(ascending=False)
studio_performance

     # Plot the studio performance as a bar chart
plt.figure(figsize=(12, 6))
studio_performance.plot(kind='bar', color='skyblue')
plt.title('Movie Performance by Studio (Total Domestic Gross Earnings)')
plt.xlabel('Studio')
plt.ylabel('Total Domestic Gross Earnings')
plt.xticks(rotation=45)
plt.grid(axis='y')
plt.show()



By analyzing the bar chart of movie performance by studio, we can identify which studios have achieved the highest total domestic gross earnings in my dataset and that is BV studios. This analysis can provide insights into the financial success of studios within the movie industry. We can use this information for various purposes, including studio comparisons, investment decisions, and understanding the competitive landscape of the industry.
 


 ### Multivariate analysis
 It involves examining relationships between multiple variables in a dataset simultaneously.In the dataset 'movies_review_df' we can perform multivariate analysis to explore how multiple variables, such as 'averagerating,' 'numvotes,' and 'runtime_minutes,' are related. One way to visualize this is by creating a heatmap of the correlations between these variables


 

     # Analyze correlations between variables and create a heatmap
import seaborn as sns
import matplotlib.pyplot as plt

    # Compute the correlation matrix
correlation_matrix = movies_reviews_df.corr()

    # Create a heatmap using seaborn
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', fmt=".2f", linewidths=0.5)
plt.title('Correlation Heatmap')
plt.show()



### CONCLUSIONS

The movie dataset movies_review_df provides valuable insights into movie characteristics, trends, and performance. Key takeaways include the observation of changing movie release trends over the years, the popularity of drama and documentaries among viewers, and the financial success of BV studios. Further analysis and investigation can be conducted to explore the underlying factors driving these trends and correlations, allowing for more informed decision-making within the movie industry. It's important to note that while correlations provide insights into linear relationships, other complex relationships and factors may influence movie performance and trends.























   -




 
# movie_reviews-project
# movie_reviews-project
