<h1><div align="center"><strong> EXPLORATORY DATA ANALYSIS ON SPOTIFY 2023 DATASET </strong></div></h1>

************************************************************************************************

<h2><div align="center"> <strong> OVERVIEW </strong></div></h2>

<div align="justify">This repository contains an <strong>Exploratory Data Analysis (EDA) of the Most Streamed Spotify Songs of 2023</strong>. <br><br> This analysis aims to explore, visualize, and interpret key factors influencing track popularity. By examining trends, patterns, and relationships between musical attributes, artist performance, and other various characteristics, this analysis aims to provide valuable insights into the dynamics of popular music in today's world. <br><br> The findings aim to offer a deeper understanding of the current music landscape and shed light on potential future trends in the industry.</div>

************************************************************************************************

<h2><div align="center"> <strong> GENERAL GUIDELINES </strong></div></h2><br>

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

<br>

************************************************************************************************

<h2><div align="center"> <strong> INTRODUCING THE DATASET </strong></div></h2><br>

*First, **import the libraries** needed.*
   
    import numpy as np
    import pandas as pd
    import matplotlib.pyplot as plt
    import seaborn as sns

<br> 
    
*Then, **import the dataset** of the Most Streamed Spotify Songs in 2023 <br> (https://www.kaggle.com/datasets/nelgiriyewithana/top-spotify-songs-2023)*
    
    sptfy23 = pd.read_excel('spotify-2023.xlsx') #convert csv to xlsx
    sptfy23

![image](https://github.com/user-attachments/assets/c17b2875-e370-4f87-bf97-3682da8dfa69)


************************************************************************************************

<h2><div align="center"><strong> GUIDE QUESTIONS </strong></div></h2><br>

<div align="center"><ins><strong> OVERVIEW OF DATASET </strong></ins></div><br> 

****Objective:**** 
- Understand the basic structure of the dataset and identify any potential data quality issues. 

****Task:**** 
- In this section, we will explore the dataset’s dimensions (rows and columns), datatypes, and check for any missing values.

<br>

****Code Proper:**** 

*<strong>1. </strong> <ins>How many rows and columns does the dataset contain?</ins>*
    
    #show the dimensions of the datasets: (row, cols)
    dimension = sptfy23.shape
    dimension
    
<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/ea4f2d18-9c48-4b2c-823f-6cb1781c6254)

<br>

> ****Analysis:**** The dataset contains 952 rows and 24 columns.

<br>

*<strong>2.1. </strong> <ins>What are the data types of each column?</ins>*

    #show the datatypes for each cols
    datatype = sptfy23.dtypes
    datatype

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/2b7ac214-8674-4432-be09-c31a2a1ce5c7)

<br>

> ****Analysis:**** The dataset includes a mix of datatypes: object, int64, and float64.

<br>

*<strong>2.2. </strong> <ins>Are there any missing values?</ins>*

    def missing_values(sptfy_dataset):
    
    for column in sptfy_dataset.columns: #iterate over each column
    
        missing_count = sptfy_dataset[column].isna().sum() #calculate the no. of missing values

        #if there is, then proceed
        if missing_count > 0: 
        
            #string all the info together and print
            percentage_missing = (missing_count / len(sptfy_dataset)) * 100 #calculate %
            print(f" For column, {column}: There are {missing_count} missing values, which is     
            {percentage_missing:.2f}% of the total entries.")
           
    missing_values(sptfy23) #call the function to display

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/fdfecd38-59a0-4ebe-aa20-71a9a9e53ec1)

<br>

> ****Analysis:**** Only two columns have missing values, **"Shazam Charts"** has <ins>50 missing values</ins> (5.25% of total entries) and **"Key"** has <ins>95 missing values</ins> (9.97%).

<br>

============================================================================================

<div align="center"><ins><strong> BASIC DESCRIPTIVE STATISTICS </strong></ins></div><br>

****Objective:**** 
- Calculate key summary statistics and understand the distribution of critical variables.

****Task:**** 
- In this section, we will compute the mean, median, and standard deviation for the streams column and examine the distribution of released_year and artist_count, which can identify any trends or outliers that might impact our analysis.

<br> 

****Code Proper:**** 

*First, convert **"streams"** data into <ins>int64</ins> using this code:*
    
    def convertion(columns):
    
    for col in columns:  #iterate over each column
    
        sptfy23[col] = pd.to_numeric(sptfy23[col], errors='coerce')  #convert to numeric; set non-numeric entries to NaN
        
        sptfy23.dropna(subset=[col], inplace=True)  #remove rows with non-numeric entries (NaN)
        
        sptfy23[col] = sptfy23[col].astype('int64')  #convert the data to int64

    cols = ['streams']  #store the list of columns to be converted in 'cols'
  
    convertion(cols)  #call the function

    sptfy23.dtypes  #display the converted data types of each cols

<br> 

*Thus, if you will notice below, the datatype for **"streams"** is now changed from object to <ins>int64</ins>:*

![image](https://github.com/user-attachments/assets/7c1c3c43-a9c8-42c9-9bac-3c5274bbca42)

<br>

*<strong>3. </strong> <ins>What are the mean, median, and standard deviation of the streams column?</ins>*

    streams_mean = sptfy23['streams'].mean()  #compute mean of streams
    
    streams_median = sptfy23['streams'].median()  #compute median of streams
    
    streams_std = sptfy23['streams'].std()  #compute standard deviations of streams

    #display results
    print("Mean: ", streams_mean)
    print("Median: ", streams_median)
    print("Standard Deviation: ", streams_std)

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/5ed8da13-5927-477b-a8eb-30c3601d7181)

<br>

> ****Analysis:**** The **"Streams"** column shows values of **mean** = 514,137,425, **median** = 290,530,915, and **standard deviation** = 566,856,949.

<br>

*<strong>4.1. </strong> <ins>What is the distribution of released_year and artist_count?</ins>*

    #create a histogram and boxplot to show distribution 
    plt.figure(figsize=(12,10))    

<h5>For Released Year:</h5>

    #histogram of released year
    plt.subplot(2,2,1)
    sns.histplot(sptfy23['released_year'], bins=10, kde=True)
    plt.title("Distribution of Release Years using Histogram")
    plt.xlabel("Release Year")
    plt.ylabel("Count")

    #display the graphs
    plt.tight_layout()
    plt.show()
    
<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/c731e8c6-bf11-461e-9d47-648188984a42)

<br>

> ****Analysis:**** Most songs in the dataset are from recent years (2010s onward), showing steady growth over time.

<br>

<h5>For Artist Count:</h5>

    #histogram of artist count
    plt.subplot(2,2,3)
    sns.histplot(sptfy23['artist_count'], bins=10, kde=True)
    plt.title("Distribution of Artist Count using Histogram")
    plt.xlabel("Artist Count")
    plt.ylabel("Count")

    #display the graphs
    plt.tight_layout()
    plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/8916c616-3fc9-461f-b549-99c0393166b8)

<br>

> ****Analysis:**** Most tracks feature solo artists, though collaborations with 2-3 artists are also common.

<br> 

*<strong>4.2. </strong> <ins>Are there any noticeable trends or outliers?</ins>*

<h5>For Released Year:</h5>

    #boxplot of artist count
    plt.subplot(2,2,4)
    sns.boxplot(sptfy23['artist_count'])
    plt.title("Distribution of Artist Count using Boxplot")
    plt.ylabel("Artist Count")
    
    #display the graphs
    plt.tight_layout()
    plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/4a96c1aa-68cc-4b3a-812d-663738389e3d)

<br>

> ****Analysis:**** The boxplot reveals many outliers, which gradually decrease as we move from the 2020s back to the 1900s.

<br>

<h5>For Artist Count:</h5>

    #boxplot of artist count
    plt.subplot(2,2,4)
    sns.boxplot(sptfy23['artist_count'])
    plt.title("Distribution of Artist Count using Boxplot")
    plt.ylabel("Artist Count")

    #display the graphs
    plt.tight_layout()
    plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/d3f4d938-df0d-49df-89f5-d2967a3bdbd9)

<br>

> ****Analysis:**** The boxplot shows solo tracks are most frequent, followed by duos and trios, yet there are a few outliers with 4-8 artists collaborating, though they are less common.

<br>

============================================================================================
  
<div align="center"><ins><strong> TOP PERFORMERS </strong></ins></div><br>

****Objective:****
- Identify the tracks and artists that perform the best in terms of streaming numbers.
  
****Task:**** 
- In this section, we will find the track with highest number of streams, display the top 5 most streamed tracks, and identify the top 5 most frequent artists based on the number of their tracks in the dataset > to see which artists dominate the 2023 Spotify scene.

<br>

****Code Proper:**** 

*<strong>5. </strong> <ins>Which track has the highest number of streams? Display the top 5 most streamed tracks.</ins>*

*First, let's **analyze** this data:*

      #sort the dataset by the 'streams' cols in descending order
      high_streams = sptfy23.sort_values(by='streams', ascending=False)
      
      #display the first five rows of the data set
      high_streams.head()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/10717936-7c29-4a12-b1a1-af4a5325a99e)

<br> 

*Then, let's **plot** it to better **visualize** it.*

      #create a barplot to visualize the data above
      plt.figure(figsize=(10,5))
      plt.title("Top 5 Most Streamed Tracks")
      
      sns.barplot(x="track_name", y="streams", data = high_streams.head())
      plt.xticks(rotation=25, ha="right", fontsize=10)
      
      plt.show()
      
      #display the exact values for each bar below
      for i, (index, row) in enumerate(high_streams.head().iterrows(), start=1):
          print(f"{i}. {row['track_name']} with a total of {row['streams']} streams.")

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/67c66388-8108-46bb-9704-5b0332f2f58a)

<br>

> ****Analysis:**** The top 5 most-streamed tracks of 2023 are "Blinding Lights," "Shape of You," "Someone You Loved," "Dance Monkey," and "Sunflower (Spider-Man: Into the Spider-Verse)," with streams between 2.8 billion and 3.7 billion.

<br>

*<strong>6. </strong> <ins>Who are the top 5 most frequent artists based on the number of tracks in the dataset?</ins>*

      #split 'artist(s)_name' to individual names of each artist for those who did collaborations
      sptfy23['artist(s)_name'] = sptfy23['artist(s)_name'].str.split(', ') 
      spotify_exploded = sptfy23.explode('artist(s)_name')
      
      #count the occurrences of each artists in the updated dataframe
      top_artists = spotify_exploded['artist(s)_name'].value_counts().head()
      
      #create a barplot to visualize the data
      plt.figure(figsize=(10, 6))
      sns.barplot(x=top_artists.index, y=top_artists.values)
      plt.title("Top Artists by Number of Tracks")
      plt.xlabel("Artists")
      plt.ylabel("Number of Tracks")
      plt.xticks(rotation=45)
      plt.show()
      
      #display the exact values for each bar below
      for i, (artist, count) in enumerate(top_artists.items(), start=1):
          print(f"{i}. {artist} with a total of {count} tracks.")

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/0072314f-52df-48c3-b5d8-19464d17aeaf)

<br>

> ****Analysis:**** The top 5 most frequent artists in 2023 are Bad Bunny, Taylor Swift, The Weeknd, SZA, and Kendrick Lamar, with tracks ranging from 23 to 40 each.

<br>

============================================================================================

<div align="center"><ins><strong> TEMPORAL TRENDS </strong></ins></div><br>

****Objective:****
- Analyze how the number of tracks and their release patterns change over time.

****Task:**** 
- In this section, we will explore the trends in the number of tracks released over the years as well as analyze monthly release patterns, identifying any peaks or shifts in the music industry. 

<br>

****Code Proper:**** 

*<strong>7. </strong> <ins>Analyze the trends in the number of tracks released over time. Plot the number of tracks released per year.</ins>*

*First, let's **analyze** this  data:* 

      #count the no. of tracks released per year
      tracks_per_year = sptfy23['released_year'].value_counts()
      
      #sort the values in descending order
      tracks_per_year.sort_values(ascending=False)

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/ade016b5-5277-4c7d-99fb-992ee887329d)

<br> 

*Then, let's **plot** it to better **visualize** the trends through the years.*

      #create a lineplot to visualize the data above
      plt.figure(figsize=(8, 5))
      sns.lineplot(x=tracks_per_year.index, y=tracks_per_year.values, marker = 'o')
      plt.title('Number of Tracks Released Per Year')
      plt.xlabel('Year')
      plt.ylabel('Number of Tracks Released')
      plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/5af0b72c-8fd8-4315-9533-af14eece72a7)

<br>

> ****Analysis:**** The number of tracks released per year rose significantly in recent years, peaking at 402 in 2022. In 2023, the count dropped to 175. This seems unlikely if we follow the trend of te plot. Thus, this inconsistency might be due to the dataset being compiled mid-year and the latter part was not recorded, affecting the data significantly.

<br>

*<strong>8. </strong> <ins>This time, does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?</ins>*

*First, let's **analyze** this  data:* 

      #count the no. of tracks released per month
      tracks_per_month = sptfy23['released_month'].value_counts()
      
      #sort the values in descending order
      tracks_per_month.sort_values(ascending=False)

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/8ef8fef2-aae3-4031-af30-e26bdf631b5c)

<br> 

*Then, let's **plot** it to better **visualize** the trends per month.*

      #create a lineplot to visualize the data above
      plt.figure(figsize=(12, 5))
      sns.lineplot(x=tracks_per_month.index, y=tracks_per_month.values, marker = 'o')
      plt.title('Number of Tracks Released Per Month')
      plt.xlabel('Month')
      plt.ylabel('Number of Tracks Released')
      plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/68387e57-fc52-44f9-a7af-fe02c1fe78cf)

<br>

> ****Analysis:**** The number of tracks released per month shows that January and May have the highest track releases, while August has the fewest. The number of tracks per month is scattered without a clear seasonal trend.

<br>

============================================================================================

<div align="center"><ins><strong> GENRE AND MUSIC CHARACTERISTICS </strong></ins></div><br>

****Objective:****
- Explore the relationship between musical attributes and track popularity.

****Task:**** 
- In this section, we will examine the correlations between various musical attributes and how they influence the number of streams to better understand the factors driving a track's success. 

<br>

****Code Proper:**** 

*<strong>9. </strong> <ins>Examine the correlation between streams and musical attributes like bpm, danceability_%, and energy_%. Which attributes seem to influence streams the most?</ins>*

      #create a scatterplot to visualize the correlation between streams and each musical attributes
      plt.figure(figsize=(15,15))    
      plt.suptitle('Correlation between Streams and Musical Attributes',fontsize=18)
      
      #list all the attributes to be used
      attributes = ['bpm', 'danceability_%', 'valence_%', 'energy_%', 
                    'acousticness_%', 'instrumentalness_%', 'liveness_%', 'speechiness_%']
      
      for i, attribute in enumerate(attributes, 1): #iterate through each attribute in the list
      
          plt.subplot(4, 2, i)
          sns.scatterplot(data=sptfy23, x=attribute, y='streams', alpha=0.6)
          plt.title(f"Streams vs {attribute}")
          plt.xlabel(attribute)
          plt.ylabel("Streams")
      
      plt.tight_layout()
      plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/0c1ce0a9-4581-4264-8304-9dc1f660223d)

![image](https://github.com/user-attachments/assets/5b26fc58-f637-4776-85db-83d18f2f0fed)

> **For BPM:** There’s no strong correlation between BPM and streams, indicating that tempo alone doesn’t heavily impact a song’s popularity.

<br>

![image](https://github.com/user-attachments/assets/ed0f9504-0214-4d6e-9838-9cff0f568451)

> **For Danceability:** Danceability doesn’t show a clear relationship with streams, suggesting it’s not a key factor in popularity.

<br>

![image](https://github.com/user-attachments/assets/4b1e29ba-c001-48ce-980b-b94af7bb72f1)

> **For Valence:** Streams are spread across low and high valence, meaning that a song’s mood isn’t strongly tied to popularity.

<br>

![image](https://github.com/user-attachments/assets/ffe33dd0-4903-4824-bbec-2e77bc1bbb6e)

> **For Energy:** Energy levels show no clear pattern with streams; both low- and high-energy songs achieve similar popularity.

<br>

![image](https://github.com/user-attachments/assets/54cb3736-61f5-4abb-85bf-3f5ff111a674)

> **For Acousticness:** Lower acousticness tends to correlate with higher streams, suggesting listeners prefer more electronic or highly produced songs.

<br>

![image](https://github.com/user-attachments/assets/3d5dc745-8017-4344-8567-3baec9eeba67)

> **For Instrumentalness:** Songs with low instrumentalness (typically with vocals) generally have higher streams, while instrumental tracks are less popular.

<br>

![image](https://github.com/user-attachments/assets/9f499619-6cce-4d08-920b-651f3d0dec06)

> **For Liveness:** High liveness (live recording features) tends to correlate with lower streams, indicating a preference for studio-recorded songs.

<br>

![image](https://github.com/user-attachments/assets/5c7eda2f-f311-4b39-9a9c-d73af720a13f)

> **For Speechiness:** Songs with low speechiness (less spoken-word content) generally have higher streams, suggesting a listener preference for melodic tracks.

<br>

> **Overall**, tere are few strong correlations between musical attributes like BPM, danceability, valence, energy, and liveness with streaming popularity. However, low instrumentalness, lower acousticness, and lower speechiness may slightly favor higher streams, indicating a preference for vocal-driven, less acoustic, and more melodic songs. Popularity likely depends on a mix of factors beyond these attributes, such as artist reputation and marketing.

<br>

*<strong>10.1. </strong> <ins>Is there a correlation between danceability_% and energy_%?</ins>*

      #create a scatterplot to visualize the correlation between danceability and energy
      plt.figure(figsize=(6,3))    
      
      sns.scatterplot(data=sptfy23, x='danceability_%', y='energy_%', alpha=0.6)
      plt.title("Correlation between Danceability and Energy")
      plt.xlabel("Danceability")
      plt.ylabel("Energy")
      
      plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/7608eccc-ae4e-4f1f-a085-fa296c5bf9de)

<br>

> ****Analysis:**** Danceability and energy show a positive correlation, indicating that highly danceable songs are often high-energy, a common trait in pop and electronic music.

<br>

*<strong>10.2. </strong> <ins> How about valence_% and acousticness_%? </ins>*

      #create a scatterplot to visualize the correlation between valence and acousticness
      plt.figure(figsize=(6,3))
      
      sns.scatterplot(data=sptfy23, x='valence_%', y='acousticness_%', alpha=0.6)
      plt.title("Correlation between Valence and Acousticness")
      plt.xlabel("Valence")
      plt.ylabel("Acousticness")
      
      plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/6afa71e7-0d95-4420-a8ba-4cf2b5ed330c)

<br>

> ****Analysis:**** Valence and acousticness show no strong correlation, suggesting that both acoustic and electronic songs can be either upbeat or somber.

<br>

============================================================================================

<div align="center"><ins><strong> PLATFORM POPULARITY </strong></ins></div><br>

****Objective:****
- Compare track representation across different music platforms. 

****Task:**** 
- In this section, we will analyze how tracks are distributed across platforms like Spotify Playlists, Spotify Charts, and Apple Playlists which will help identify which platform favors the most popular tracks.

<br>

****Code Proper:**** 

*First, convert **"in_spotify_playlists", "in_apple_playlists", "in_deezer_playlists"** data into <ins>int64</ins> using this code:*

      def convertion(columns):
          for col in columns: #iterate over each column 
          
              sptfy23[col] = pd.to_numeric(sptfy23[col], errors='coerce') #convert to numeric; set non-numeric entries to NaN
              
              sptfy23.dropna(subset=[col], inplace=True) #remove rows with non-numeric entries (NaN)
              
              sptfy23[col] = sptfy23[col].astype('int64') #convert the data to int64
      
      #store the list of columns to be converted in 'cols'
      cols = ['in_spotify_playlists', 'in_apple_playlists', 'in_deezer_playlists'] 
      
      convertion(cols) #call the function
      
      sptfy23.dtypes  #display the converted data types of each cols

<br> 

*Thus, if you will notice below, the datatype for **"in_spotify_playlists", "in_apple_playlists", "in_deezer_playlists"** is now changed from object to <ins>int64</ins>:*

![image](https://github.com/user-attachments/assets/29dc6ec0-14a5-4170-b2c0-b2356c48018d)

<br>

*<strong>11.1. </strong> <ins>How do the numbers of tracks in spotify_playlists, deezer_playlists, and apple_playlists compare?</ins>*

      #create a list of all the playlist names to be analyzed
      playlists = ['in_spotify_playlists', 'in_apple_playlists', 'in_deezer_playlists']
      
      #create a dictionary where the keys=names of the playlist and the value=total sum of tracks there
      tracks = {platform: sptfy23[platform].sum() for platform in playlists}
      
      #convert the dictionary into a dataframe 
      compile_list = pd.DataFrame(tracks.items(), columns=['platform', 'tracks'])
      
      #create a barplot to visualize the data
      plt.figure(figsize=(7, 5))
      sns.barplot(x='platform', y='tracks', data=compile_list)
      plt.title("Comparison of Tracks Across Platforms")
      plt.xlabel("Platform")
      plt.ylabel("Tracks")
      plt.xticks(rotation=45)
      
      plt.tight_layout()  
      plt.show()
      
      #display the exact values of each bar
      for i, (track, count) in enumerate(tracks.items(), start=1):
          print(f"{i}. {track} = {count} tracks")

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/55c49e57-edc4-461d-bdee-e08c994e7b01)

<br>

> ****Analysis:**** Spotify has the most tracks, followed by Deezer and then Apple, with 4,952,842, 367,030, and 64,609 tracks respectively.

<br>

*<strong>11.2. </strong> <ins>Which platform seems to favor the most popular tracks?</ins>*

      #sort the values of 'high_streams' in descending order
      high_streams = sptfy23.sort_values(by='streams', ascending=False)
      
      #only take the first 5 highest tracks
      top_tracks = high_streams.head()
      
      #create a dictionary where the keys=names of the playlist and the value=total sum of 
      tracks there
      track_counts = {platform: top_tracks[platform].sum() for platform in playlists}
      
      #convert the dictionary into a dataframe 
      popular_tracks = pd.DataFrame(track_counts.items(), columns=['platform', 'track_counts'])
      
      #create a barplot to visualize the data
      plt.figure(figsize=(10, 6))
      sns.barplot(x='platform', y='track_counts', data=popular_tracks)
      plt.title("Comparison of Track Counts for Top Tracks Across Platforms")
      plt.xlabel("Platform")
      plt.ylabel("Total Track Count")
      plt.xticks(rotation=45)
      plt.tight_layout() 
      plt.show()
      
      #display the exact values of each bar
      for index, row in popular_tracks.iterrows():
          print(f"{row['platform']}: {row['track_counts']} tracks")

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/865585f9-c1ac-4c0d-ab09-dd2c0c765920)

<br>

> ****Analysis:**** Spotify hosts the most popular tracks, followed by Deezer and Apple, with 142,539, 16,467, and 2,050 tracks respectively.

<br>

============================================================================================

<div align="center"><ins><strong> ADVANCED ANALYSIS </strong></ins></div><br>

****Objective:****
- Conduct a deeper analysis of track characteristics and their presence in playlists or charts. 

****Task:**** 
- In this section, we will examine patterns related to the musical key and mode (Major vs. Minor) and its correlation with streams. We will also explore if certain artists are more likely to appear in playlists or charts, helping to analyze who are the most frequently appearing ones in both platforms.

<br>

****Code Proper:**** 

*<strong>12. </strong> <ins>Based on the streams data, can you identify any patterns among tracks with the same key or mode (Major vs. Minor)?</ins>*

<h5>For Keys:</h5>

      #group the values by 'key' and calculate the mean for each
      average_streams_by_key = sptfy23.groupby('key')['streams'].mean()
      
      #sort in descending order
      average_streams_by_key.sort_values(ascending=False)
      
      #create a barplot to visualize the data
      plt.figure(figsize=(10, 4))
      sns.barplot(x=average_streams_by_key.index, y=average_streams_by_key.values)
      plt.title("Average Streams by Track Key")
      plt.xlabel("Key")
      plt.ylabel("Average Streams")
      plt.tight_layout()
      plt.show()
      
      #calculate the count of tracks for each key
      key_counts = sptfy23['key'].value_counts()
      
      #display the exact values of each bar
      for i, (key, count) in enumerate(key_counts.items(), start=1):
          print(f"{i}. {key} - {count} tracks")

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/0450b580-f6f0-43f1-a04c-175d9286d7ac)

<br>

> ****Analysis:**** In the key of C#, both track count and average streams are highest, suggesting it’s a popular choice that also performs well. E is the next most successful key despite fewer tracks. Keys like G# and G have many tracks but lower average streams, suggesting they’re used frequently but are less associated with hits.

<br>

<h5>For Mode:</h5>

      #group the values by 'mode' and calculate the mean for each
      average_streams_by_mode = sptfy23.groupby('mode')['streams'].mean()
      
      #sort in descending order
      average_streams_by_mode.sort_values(ascending=False)
      
      #create a barplot to visualize the data
      plt.figure(figsize=(6, 4))
      sns.barplot(x=average_streams_by_mode.index, y=average_streams_by_mode.values)
      plt.title("Average Streams by Track Mode (Major vs. Minor)")
      plt.xlabel("Mode")
      plt.ylabel("Average Streams")
      plt.tight_layout()
      plt.show()
      
      #calculate the count of tracks for each mode
      mode_counts = sptfy23['mode'].value_counts()
      
      #display the exact values of each bar
      for i, (mode, count) in enumerate(mode_counts.items(), start=1):
          print(f"{i}. {mode} - {count} tracks")

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/12d06be0-8cff-4d76-b9a0-ddbd8759e633)

<br>

> ****Analysis:**** Songs in major modes tend to have higher streams and counts than minor ones, suggesting a preference for happier or more uplifting sounds. However, minor-key (somber) songs also maintain substantial popularity.

<br>

*<strong>13. </strong> <ins>Do certain genres or artists consistently appear in more playlists or charts? Perform an analysis to compare the most frequently appearing artists in playlists or charts.</ins>*

<h5>For Playlists:</h5>

      #create a list of all the playlists
      playlists = ['in_spotify_playlists', 'in_apple_playlists', 'in_deezer_playlists']
      
      #create a barplot to visualize the data
      fig, axes = plt.subplots(3, 1, figsize=(18, 18))
      
      axes = axes.flatten()  #flatten the array to access the loop easier
      
      plt.suptitle('Top 10 Most Frequently Appearing Artists in Playlists', fontsize=18)
      plt.subplots_adjust(top=0.9, hspace=8) 
      
      for ix, playlist in enumerate(playlists):  #iterate through each playlist in the list
      
          #group the data by artist name, find the total sum, and sort descendingly
          spotify_sub = spotify_exploded.groupby('artist(s)_name').sum(playlist).sort_values(playlist, ascending=False).reset_index()
          
          #create the barplot
          sns.barplot(x='artist(s)_name', y=playlist, data=spotify_sub.head(10), ax=axes[ix])
      
          #this is to add the exact values of each bar on top of it
          for p in axes[ix].patches:
              height = p.get_height()
              axes[ix].text(p.get_x() + p.get_width() / 2, height + 0.5,  
                            f'{height:.0f}', ha='center', va='bottom', fontsize=10)
      
      plt.tight_layout()
      plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/6d93abb9-5c4d-428b-b039-e558d726bc39)
![image](https://github.com/user-attachments/assets/5ba54967-b6e8-4b8f-8733-3159fe064af1)
![image](https://github.com/user-attachments/assets/1cd1681e-c79a-44f8-92f6-858cd5bb6f57)

<br>

> ****Analysis:**** The Weeknd ranks highest on Spotify and Apple playlists, though Eminem leads on Deezer and ranks second on Spotify. The Weeknd and Ed Sheeran are the only artists in the top 10 on all three platforms.

<br>

<h5>For Charts:</h5>

      #create a list of all the charts
      charts = ['in_spotify_charts', 'in_apple_charts', 'in_deezer_charts', 'in_shazam_charts'] 
      
      #create a barplot to visualize the data
      fig, axes = plt.subplots(4, 1, figsize=(18, 18))
      
      axes = axes.flatten() #flatten the array to access the loop easier
      
      plt.suptitle('Top 10 Most Frequently Appearing Artists in Charts', fontsize=18)
      plt.subplots_adjust(top=0.9, hspace=8) 
      
      for ix, chart in enumerate(charts): #iterate through each charts in the list
      
          #group the data by artist name, find the total sum, and sort descendingly
          spotify_sub = spotify_exploded.groupby('artist(s)_name').sum(chart).sort_values(chart, ascending=False).reset_index()
          
          #create the barplot
          sns.barplot(x='artist(s)_name', y=chart, data=spotify_sub.head(10), ax=axes[ix])
      
          #this is to add the exact values of each bar on top of it
          for p in axes[ix].patches:
              height = p.get_height()
              axes[ix].text(p.get_x() + p.get_width() / 2, height + 0.5,  
                            f'{height:.0f}', ha='center', va='bottom', fontsize=10)
      
      plt.tight_layout()
      plt.show()

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/5db9835c-6b1f-43f7-8e22-92defe7f7d18)
![image](https://github.com/user-attachments/assets/1b4fa689-fc4c-4e92-95de-0ccbb561e883)
![image](https://github.com/user-attachments/assets/4681bc1c-4ae2-4099-840e-12d0fbf4687a)
![image](https://github.com/user-attachments/assets/9d3ac67f-cbec-48fc-8b87-5b493a002731)

<br>

> ****Analysis:**** The Weeknd and Bad Bunny top multiple charts, with The Weeknd leading on Apple and Shazam, while Bad Bunny leading on Spotify and Deezer. Both artists appear across all platforms, demonstrating their widespread appeal.

<br>

************************************************************************************************

<h2><div align="center"><strong> CONCLUSION </strong></div></h2><br>

Through this analysis, we gained a comprehensive view of the factors contributing to a track's popularity on streaming platforms. 

<strong><ins>Insights</strong></ins>

**1. Release Trends and Streaming Numbers:**
- Tracks from recent years dominate the dataset, reflecting the growing presence of digital platforms and the rise of music streaming. The peak in 2022 suggests significant music industry activity, though the data for 2023 may be incomplete. Popular tracks also frequently come from solo artists, though collaborations are rising, possibly due to their appeal across multiple fanbases.

**2. Music Characteristics and Popularity:** 
- Despite some assumptions that certain musical attributes might heavily influence popularity, our findings reveal that BPM, danceability, energy, and valence alone do not strongly dictate streaming numbers. However, tracks with low acousticness, low instrumentalness, and lower speechiness are generally more popular, indicating a preference for melodic, vocal-driven, and electronically produced music. This preference aligns with trends in pop and hip-hop, where production quality and lyrical content are often prioritized.

**3. Music Platform Popularity:** 
- Spotify leads in hosting popular tracks, which may be attributed to its extensive library and global user base. Artists with widespread appeal, like The Weeknd and Bad Bunny, consistently top both playlists and charts on multiple platforms, suggesting that cross-platform visibility plays a role in maintaining popularity.

**4. Genre, Key, and Mode Preferences:** 
- Certain musical keys, particularly C# and E, are associated with high-performing tracks, possibly due to their frequency in popular genres like pop and hip-hop. Major modes appear slightly more popular than minor, aligning with a general listener preference for upbeat or “feel-good” music, though minor-key songs are still highly relevant and can resonate deeply with audiences.

<strong><ins>Recommendations:</strong></ins>

> The analysis reveals that popular tracks are highly produced, vocal-driven, and melodic, with lower acoustics, instrumentals, and speeches. This aligns with the preference for high-energy, danceable songs commonly found in pop and electronic music genres. Established artists like The Weeknd, Bad Bunny, and Taylor Swift dominate across platforms, indicating that an artist's reputation plays a key role in track success. Spotify stands out as the leading platform for popular music discovery, followed by Deezer, Apple, and Shazam. 

- Therefore, to enhance audience reach, record labels and artists could focus on producing energetic, danceable tracks and leverage Spotify’s platform and high-profile artist collaborations.

<br> **In summary**, while individual attributes alone do not strongly correlate with popularity, broader trends such as production quality, artist reach, and platform visibility play a significant role in determining what makes a track popular.

<br>

************************************************************************************************

<h2><div align="center"><strong> AUTHOR </strong></div></h2>

<h4><div align="center"><strong> Alesandra Joyce P. Maghanoy </strong></div></h4>

************************************************************************************************







