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
> - Understand the basic structure of the dataset and identify any potential data quality issues. 

****Task:**** 
> - In this section, we will explore the dataset’s dimensions (rows and columns), datatypes, and check for any missing values.

<br>

****Code Proper:**** 

*<strong>1. </strong> <ins>How many rows and columns does the dataset contain?</ins>*
    
    #show the dimensions of the datasets: (row, cols)
    dimension = sptfy23.shape
    dimension
    
<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/ea4f2d18-9c48-4b2c-823f-6cb1781c6254)

****Analysis:****

<br>

*<strong>2.1. </strong> <ins>What are the data types of each column?</ins>*

    #show the datatypes for each cols
    datatype = sptfy23.dtypes
    datatype

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/2b7ac214-8674-4432-be09-c31a2a1ce5c7)

****Analysis:****

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

****Analysis:****

============================================================================================

<div align="center"><ins><strong> BASIC DESCRIPTIVE STATISTICS </strong></ins></div><br>

****Objective:**** 
> - Calculate key summary statistics and understand the distribution of critical variables.

****Task:**** 
> - In this section, we will compute the mean, median, and standard deviation for the streams column and examine the distribution of released_year and artist_count, which can identify any trends or outliers that might impact our analysis.

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

****Analysis:****

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

    #boxplot of released year
    plt.subplot(2,2,2)
    sns.boxplot(sptfy23['released_year'])
    plt.title("Distribution of Release Years using Boxplot")
    plt.ylabel("Release Year")

<h5>For Artist Count:</h5>

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

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/6165c60c-a280-40ee-b65a-2a385e31f798)

****Analysis:****

<br> 

*<strong>4.2. </strong> <ins>Are there any noticeable trends or outliers?</ins>*

<h5>For Released Year:</h5>

    #create a summary of the statistics for 'released_year'
    released_year_stats = sptfy23['released_year'].describe()
    
    #display the results
    print("Released Year")
    print(released_year_stats)

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/4af60042-1ff1-4fb1-89aa-aaf97645e3ae)

****Analysis:****

<h5>For Artist Count:</h5>

    #create a summary of the statistics for 'artist_count'
    artist_count_stats = sptfy23['artist_count'].describe()
    
    #display results
    print("Artist Count")
    print(artist_count_stats)

<h5><em><ins> Output </ins></em></h5>

![image](https://github.com/user-attachments/assets/4c060d64-4590-4316-b6e5-55c861ed9557)

****Analysis:****

============================================================================================
  
<div align="center"><ins><strong> TOP PERFORMERS </strong></ins></div><br>

****Objective:****
> - Identify the tracks and artists that perform the best in terms of streaming numbers.
  
****Task:**** 
> - In this section, we will find the track with highest number of streams, display the top 5 most streamed tracks, and identify the top 5 most frequent artists based on the number of their tracks in the dataset > to see which artists dominate the 2023 Spotify scene.

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

****Analysis:****

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

****Analysis:****


============================================================================================

<div align="center"><ins><strong> TEMPORAL TRENDS </strong></ins></div><br>

****Objective:****
> - Analyze how the number of tracks and their release patterns change over time.

****Task:**** 
> - In this section, we will explore the trends in the number of tracks released over the years as well as analyze monthly release patterns, identifying any peaks or shifts in the music industry. 

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

****Analysis:****

<br>

*<strong>8. </strong> <ins>This time, does the number of tracks released per month follow any noticeable patterns? Which month sees the most releases?</ins>*

*First, let's **analyze** this  data:* 

      #count the no. of tracks released per year
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

****Analysis:****

============================================================================================

<div align="center"><ins><strong> GENRE AND MUSIC CHARACTERISTICS </strong></ins></div><br>

****Objective:****
> - Explore the relationship between musical attributes and track popularity.

****Task:**** 
> - In this section, we will examine the correlations between various musical attributes and how they influence the number of streams to better understand the factors driving a track's success. 

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

![image](https://github.com/user-attachments/assets/a660e2a1-08f9-4819-865f-8739da2b8b51)
![image](https://github.com/user-attachments/assets/777191e2-568a-4d65-b134-31948b99d53a)

****Analysis:****

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

****Analysis:****

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

****Analysis:****

============================================================================================

<div align="center"><ins><strong> PLATFORM POPULARITY </strong></ins></div><br>

****Objective:****
> - Compare track representation across different music platforms. 

****Task:**** 
> - In this section, we will analyze how tracks are distributed across platforms like Spotify Playlists, Spotify Charts, and Apple Playlists which will help identify which platform favors the most popular tracks.

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

****Analysis:****

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

****Analysis:****

============================================================================================

<div align="center"><ins><strong> ADVANCED ANALYSIS </strong></ins></div><br>

****Objective:****
> - Conduct a deeper analysis of track characteristics and their presence in playlists or charts. 

****Task:**** 
> - In this section, we will examine patterns related to the musical key and mode (Major vs. Minor) and its correlation with streams. We will also explore if certain artists are more likely to appear in playlists or charts, helping to analyze who are the most frequently appearing ones in both platforms.

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

![image](https://github.com/user-attachments/assets/dcb644b4-eb5e-4753-96f0-931ebb0e923b)

****Analysis:****

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

![image](https://github.com/user-attachments/assets/159f5234-ca84-4b43-b0f8-ff81d53f4e94)

****Analysis:****

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

****Analysis:****

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

****Analysis:****

<br>

************************************************************************************************
<h2><div align="center"><strong> CONCLUSION </strong></div></h2><br>



************************************************************************************************
<h2><div align="center"><strong> AUTHOR </strong></div></h2>

<h4><div align="center"><strong> Alesandra Joyce P. Maghanoy </strong></div></h4>

************************************************************************************************






