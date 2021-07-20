
# Netflix Best Movies

This project is to use EDA to find the best movies found on Netflix based on user IMDB ratings

![Logo](/image/netflix-logo.png)

    
## Data Used for this project

[IMDB Data](https://www.kaggle.com/stefanoleone992/imdb-extensive-dataset)  
For the IMDB dataset, the dataset I used was the imdb_movies.csv and the imdb_ratings.csv  

[Netflix Data](https://www.kaggle.com/shivamb/netflix-shows)  




  
## Loading Datasets
Loading the IMDb dataset into jupyter notebook and keeping the columns needed. Then the 2 datasets was merged into 1 IMDb dataframe.
```
imdb_movies = pd.read_csv('IMDb movies.csv')
imdb_movies = imdb_movies[['imdb_title_id', 'title', 'year', 'genre','date_published']]
imdb_ratings = pd.read_csv('IMDb ratings.csv')
imdb_ratings = imdb_ratings[['imdb_title_id','weighted_average_vote', 'total_votes']]
imdb = pd.merge(imdb_movies, imdb_ratings, how='outer', on='imdb_title_id')
imdb
``` 
Loading the Netflix dataset into jupyter notebook  
```
netflix = pd.read_csv('netflix_titles.csv')
netflix
```  

Merging the IMDb data with the Netflix dataset using the inner join method. Then I droppped the redundant columns.
```
merged = netflix.merge(imdb, how='inner', left_on=['title','release_year'], right_on=['title','year'])
merged.head()
merged = merged.drop(['show_id', 'type', 'date_added', 'listed_in', 'imdb_title_id', 'year'],axis=1)
merged
```
Now we have the final dataframe
![My image](https://github.com/arifzafri106/Best-Movies-on-Netflix-based-on-IMDB-ratings/blob/main/image/dataframe.PNG)

## EDA  
Now the datasets are merged, I can do EDA. The first was to find the top movies on Netflix based on the genre.  
The Top 5 Action movies based on user IMDb ratings. The following code was used to find the top movies for genres such as Horror and Comedy
``` 
top_action = merged.loc[(merged['genre'].str.contains('Action')) & (merged['total_votes']>300000)].sort_values('weighted_average_vote', ascending=False)
top_action.head()  
```  
Link of Results

Using the following code I could find the top movies on Netflix for all genres
```  
top_movies = merged.loc[merged['total_votes']>300000].sort_values('weighted_average_vote', ascending=False).head()
top_movies  
```  
![My image](https://github.com/arifzafri106/Best-Movies-on-Netflix-based-on-IMDB-ratings/blob/main/image/topallgenre.PNG)
![My image](https://github.com/arifzafri106/Best-Movies-on-Netflix-based-on-IMDB-ratings/blob/main/image/topmoviesgraph.png)  


To find the release year of movies found on netflix the following code was used
```  
year = merged.groupby('release_year').size().to_frame('No. of Movies').reset_index()
year.head()
```  
 
Based on that we can use relplot using seaborn to show the trend of the release year of movies for every year

```  
sns.relplot(x="release_year", y="No. of Movies", kind="line", data=year)
```  

![My image](https://github.com/arifzafri106/Best-Movies-on-Netflix-based-on-IMDB-ratings/blob/main/image/releaseyteargraph.png)  

