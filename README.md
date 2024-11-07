<h1><div align="center"><strong> EXPLORATORY DATA ANALYSIS ON SPOTIFY 2023 DATASET </strong></div></h1>

************************************************************************************************

<h2><div align="center"> <strong> OVERVIEW </strong></div></h2>

<div align="justify">This repository contains an <strong>Exploratory Data Analysis (EDA) of the Most Streamed Spotify Songs of 2023 dataset</strong>. <br><br> This analysis aims to explore, visualize, and interpret key factors influencing track popularity. By examining trends, patterns, and relationships between musical attributes, artist performance, and other various characteristics, this analysis aims to provide valuable insights into the dynamics of popular music in today's world. <br><br> The findings aim to offer a deeper understanding of the current music landscape and shed light on potential future trends in the industry.</div>

************************************************************************************************

<h2><div align="center"> <strong> GENERAL GUIDELINES </strong></div></h2>

**1. Dataset Exploration:** 
- Begin by familiarizing yourself with the structure of the dataset. Check for missing values and data types, and perform an initial exploration to understand the different features available.

**2. Descriptive Statistics:** 
- Provide summary statistics to give an overview of key metrics such as the number of streams, release dates, and musical attributes (e.g., BPM, danceability).

**3. Visualization:** 
- Use appropriate visualizations (e.g., bar charts, histograms, scatter plots) to uncover trends and patterns in the data. Ensure that your plots are well-labeled and easy to interpret.

**4. Correlation Analysis:**
- Investigate correlations between different variables and provide insights based on your findings. Explore relationships between streams and other musical characteristics like tempo, energy, or playlists.

**5. Insights and Recommendation:** 
- Based on your analysis, offer any insights or recommendations regarding the tracks, artists, or musical trends that could be useful for understanding what makes a track popular.

************************************************************************************************

<h2><div align="center"> <strong> INTRODUCING THE DATASET </strong></div></h2>

****Code Proper:**** 

First, import the libraries needed.
   
    import numpy as np
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns
    
Then, import the dataset.
    
    sptfy23 = pd.read_excel('spotify-2023.xlsx') #convert csv to xlsx
    sptfy23

![image](https://github.com/user-attachments/assets/c17b2875-e370-4f87-bf97-3682da8dfa69)


************************************************************************************************

<h2><div align="center"><strong> GUIDE QUESTIONS </strong></div></h2>

<div align="center"><ins><strong> OVERVIEW OF DATASET </strong></ins></div><br> 

****Objective:**** 
- Understand the basic structure of the dataset and identify any potential data quality issues. 

****Task:**** 
-  In this section, we will explore the dataset’s dimensions (rows and columns) and data types. We will also check for any missing values and address them as needed.

<br>

****Code Proper:**** 

*How many rows and columns does the dataset contain?*
    
    #show the dimensions of the datasets: (row, cols)
    dimension = sptfy23.shape
    dimension
    
<h5><ins><em> Result: </em></ins></h5>

![image](https://github.com/user-attachments/assets/ea4f2d18-9c48-4b2c-823f-6cb1781c6254)

<br>

*What are the data types of each column?*

    #show the datatypes for each cols
    datatype = sptfy23.dtypes
    datatype

<h5><ins><em> Result: </em></ins></h5>

![image](https://github.com/user-attachments/assets/2b7ac214-8674-4432-be09-c31a2a1ce5c7)

<br>

*Are there any missing values?*

    def missing_values(sptfy_dataset):
    for column in sptfy_dataset.columns: #iterate over each column
        missing_count = sptfy_dataset[column].isna().sum() #calculate the no. of missing values
        if missing_count > 0: #if there is, then proceed
            percentage_missing = (missing_count / len(sptfy_dataset)) * 100 #calculate %
            print(f" For column, {column}: There are {missing_count} missing values, which is     
            {percentage_missing:.2f}% of the total entries.")
            #string all the infos together and print

    missing_values(sptfy23) #call the function to display

<h5><ins><em> Result: </em></ins></h5>

![image](https://github.com/user-attachments/assets/fdfecd38-59a0-4ebe-aa20-71a9a9e53ec1)


****Analysis:****

============================================================================================

<div align="center"><ins><strong> BASIC DESCRIPTIVE STATISTICS </strong></ins></div><br>

****Objective:****
- Calculate key summary statistics and understand the distribution of critical variables.

****Task:**** 
- In this section, we will compute the mean, median, and standard deviation for the streams column and examine the distribution of released_year and artist_count, which can identify any trends or outliers that might impact our analysis.

<br> 

****Code Proper:**** 

First, convert <strong> "streams" </strong> data into <ins>int64</ins> using this code:
    
    def convertion(columns):
    for col in columns:  #iterate over each column
        sptfy23[col] = pd.to_numeric(sptfy23[col], errors='coerce')  #convert to numeric; set non- 
        numeric entries to NaN
        sptfy23.dropna(subset=[col], inplace=True)  #remove rows with non-numeric entries (NaN)
        sptfy23[col] = sptfy23[col].astype('int64')  #convert the data to int64

    cols = ['streams']  #store the list of columns to be converted in 'cols'
    convertion(cols)  #call the function

    sptfy23.dtypes  #display the converted data types of each cols

Thus, if you will notice below, the datatype for <strong> "streams" </strong> is now changed from object to <ins>int64</ins>:

![image](https://github.com/user-attachments/assets/7c1c3c43-a9c8-42c9-9bac-3c5274bbca42)

<br>

*What are the mean, median, and standard deviation of the streams column?*

    streams_mean = sptfy23['streams'].mean()  #compute mean of streams
    streams_median = sptfy23['streams'].median()  #compute median of streams
    streams_std = sptfy23['streams'].std()  #compute standard deviations of streams

    #display results
    print("Mean: ", streams_mean)
    print("Median: ", streams_median)
    print("Standard Deviation: ", streams_std)

<h5><ins><em> Result: </em></ins></h5>

![image](https://github.com/user-attachments/assets/5ed8da13-5927-477b-a8eb-30c3601d7181)

<br>

*What is the distribution of released_year and artist_count?*

    #create a histogram and boxplot to show distribution
    plt.figure(figsize=(12,10))    

*For **Released Year**:*

    #histogram of released year
    plt.subplot(2,2,1)
    sns.histplot(sptfy23['released_year'], bins=10, kde=True)
    plt.title("Distribution of Release Years using Histogram")
    plt.xlabel("Release Year")
    plt.ylabel("Count")

    #boxplot of released year
    plt.subplot(2,2,2)
    sns.boxplot(sptfy23['released_year'])
    plt.title("Distribution of Release Years using Boxplot")
    plt.ylabel("Release Year")

*For **Artist Count**:*

    #histogram of artist count
    plt.subplot(2,2,3)
    sns.histplot(sptfy23['artist_count'], bins=10, kde=True)
    plt.title("Distribution of Artist Count using Histogram")
    plt.xlabel("Artist Count")
    plt.ylabel("Count")
    
    #boxplot of artist count
    plt.subplot(2,2,4)
    sns.boxplot(sptfy23['artist_count'])
    plt.title("Distribution of Artist Count using Boxplot")
    plt.ylabel("Artist Count")
    
    #display the graphs
    plt.tight_layout()
    plt.show()

<h5><ins><em> Result: </em></ins></h5>

![image](https://github.com/user-attachments/assets/6165c60c-a280-40ee-b65a-2a385e31f798)

<br> 

*Are there any noticeable trends or outliers?*

*For **Released Year**:*

    #create a summary of the statistics for 'released_year'
    released_year_stats = sptfy23['released_year'].describe()
    
    #display the results
    print("Released Year")
    print(released_year_stats)

<h5><ins><em> Result: </em></ins></h5>

![image](https://github.com/user-attachments/assets/4af60042-1ff1-4fb1-89aa-aaf97645e3ae)

*For **Artist Count**:*

    #create a summary of the statistics for 'artist_count'
    artist_count_stats = sptfy23['artist_count'].describe()
    
    #display results
    print("Artist Count")
    print(artist_count_stats)

<h5><ins><em> Result: </em></ins></h5>

![image](https://github.com/user-attachments/assets/4c060d64-4590-4316-b6e5-55c861ed9557)




****Analysis:****

============================================================================================
  
<div align="center"><ins><strong> TOP PERFORMERS </strong></ins></div><br>

****Objective:****
- Identify the tracks and artists that perform the best in terms of streaming numbers.
  
****Task:**** 
- In this section, we will find the track with highest number of streams, display the top 5 most streamed tracks, and identify the top 5 most frequent artists based on the number of their tracks in the dataset to see which artists dominate the 2023 Spotify scene.

****Code Proper:**** 

****Analysis:****

============================================================================================

<div align="center"><ins><strong> TEMPORAL TRENDS </strong></ins></div><br>

****Objective:****
- Analyze how the number of tracks and their release patterns change over time.

****Task:**** 
- In this section, we will explore the trends in the number of tracks released over the years as well as analyze monthly release patterns, identifying any peaks or shifts in the music industry. 

****Code Proper:**** 

****Analysis:****

============================================================================================

<div align="center"><ins><strong> GENRE AND MUSIC CHARACTERISTICS </strong></ins></div><br>

****Objective:****
- Explore the relationship between musical attributes and track popularity.

****Task:**** 
- In this section, we will examine the correlations between various musical attributes and how they influence the number of streams to better understand the factors driving a track's success. 

****Code Proper:**** 

****Analysis:****

============================================================================================

<div align="center"><ins><strong> PLATFORM POPULARITY </strong></ins></div><br>

****Objective:****
- Compare track representation across different music platforms. 

****Task:**** 
- In this section, we will analyze how tracks are distributed across platforms like Spotify Playlists, Spotify Charts, and Apple Playlists which will help identify which platform favors the most popular tracks.

****Code Proper:**** 

****Analysis:****

============================================================================================

<div align="center"><ins><strong> ADVANCED ANALYSIS </strong></ins></div><br>

****Objective:****
- Conduct deeper analysis on track characteristics and their presence in playlists or charts. 

****Task:**** 
- In this section, we will examine patterns related to the musical key and mode (Major vs. Minor) and its correlation with streams. We will also explore if certain artists are more likely to appear in playlists or charts, helping to analyze who are the most frequently appearing ones in both platforms.

****Code Proper:**** 

****Analysis:****

============================================================================================

************************************************************************************************
<h2><div align="center"><strong> INSIGHTS AND RECOMMENDATIONS </strong></div></h2>



************************************************************************************************
<h2><div align="center"><strong> AUTHOR </strong></div></h2>

<h4><div align="center"><strong> Alesandra Joyce P. Maghanoy </strong></div></h4>

************************************************************************************************






