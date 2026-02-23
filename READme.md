# **Netflix**!
What started in 1997 as a DVD rental service has since exploded into one of the largest entertainment and media companies.
Given the large number of movies and series available on the platform, it is a perfect opportunity to flex your exploratory data analysis skills and dive into the entertainment industry. <br>
You work for a production company that specializes in nostalgic styles. You want to do some research on movies released in the 1990's. You'll delve into Netflix data and perform exploratory data analysis to better understand this awesome movie decade! <br>
You have been supplied with the dataset `netflix_data.csv`, along with the following table detailing the column names and descriptions. Feel free to experiment further after submitting!

## The data
### **netflix_data.csv**
| Column | Description |
|--------|-------------|
| `show_id` | The ID of the show |
| `type` | Type of show |
| `title` | Title of the show |
| `director` | Director of the show |
| `cast` | Cast of the show |
| `country` | Country of origin |
| `date_added` | Date added to Netflix |
| `release_year` | Year of Netflix release |
| `duration` | Duration of the show in minutes |
| `description` | Description of the show |
| `genre` | Show genre |


```python
# Importing pandas and matplotlib
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np

# Read in the Netflix CSV as a DataFrame
netflix_df = pd.read_csv("netflix_data.csv")
netflix_df['release_year'].unique()
```

## 1. Show in a histogram the frequencies of the movies during the 90s


```python
#Build a temporal dataframe for movies during 90s [1990 - 1999]
netflix_df_1990 = netflix_df[(netflix_df['release_year'] >= 1990) & (netflix_df['release_year'] <= 1999)]
netflix_df_1990['release_year'].unique()

plt.figure(figsize=(120,100))
plt.hist(x=netflix_df_1990['duration'],bins = 20)
plt.show()
```

## 2. What is the most common duration for movies during the 90s?


```python
#Find mode of duration for movies from 90s
netflix_df_1990['duration'].mode()
duration = int(94)
```

## 3. How many action movies from the 90s are short movies?
Short movies have a durations of less than 90min.


```python
#Show how genre is displayed as data
netflix_df['genre'].unique()
```




    array(['Dramas', 'Horror Movies', 'Action', 'International TV',
           'Documentaries', 'Independent Movies', 'Comedies', 'Sci-Fi',
           'International Movies', 'Children', 'TV Shows', 'Uncategorized',
           'Classic Movies', 'Thrillers', 'Stand-Up', 'Anime Features',
           'Music', 'Anime Series', 'Kids', 'Docuseries', 'Crime TV',
           'British TV', 'Cult Movies', 'TV Action', 'Romantic TV',
           'TV Horror', 'Romantic Movies', 'TV Comedies', 'Classic',
           'Reality TV', 'LGBTQ Movies'], dtype=object)




```python
#Count how many movies are short (duration less than 90 minutes) from 90s
netflix_df_1990_short_action_movies = netflix_df_1990[
    (netflix_df_1990['duration'] <90) &
    (netflix_df_1990['genre'] == 'Action')]
short_movie_count = int(len(netflix_df_1990_short_action_movies))
```

# Additional exploration analysis (Self-Guided)
Here are some other questions that are interesting to discover from present dataset -> Python
1. How many movies were produced each year? <br>
   Show your anwer in a column chart (vertical)
2. What are the top 10 most recurring / casted actors for the period of 2005 to 2010? <br>
   Show your results in a dataframe. -> SQL
3. How many movies produced each country during the 90s? <br>
   Show your results in a bar chart (horizontal)
4. What was the most popular genre for each year in dataset? <br>
   Show the frequency and it's relative weight for that year genre frequency in a table.

Each question will have it's section to answer.

### 1. How many movies were produced each year?
Show results in a column chart (vertical)


```python
#Make a dataframe with counts of movies by year.
netflix_production = netflix_df.groupby('release_year').count()
netflix_production = netflix_production[['show_id']]

#Build column chart for yearly releases in Netflix.
list_values = netflix_production['show_id'].values.tolist()
max_value = max(list_values)
colors = ['red' if v==max_value else 'blue' for v in list_values]
production_mean = np.mean(list_values)
plt.figure(figsize=(18,10))
plt.bar(x=netflix_production.index,
        height=netflix_production['show_id'],
        color = colors)
plt.axhline(production_mean, color='gray', linestyle='--', label=f'Average: {production_mean:.2f}')
plt.show()
```

### 2. What are the top 10 most recurring / casted actors for the period of 2005 to 2010? 
Show your results in a query table -> SQL


```python
-- Extract movies from 2005 to 2010
SELECT *
FROM 'netflix_data.csv'
WHERE release_year >= 2005 AND release_year <= 2010
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
      <th>show_id</th>
      <th>type</th>
      <th>title</th>
      <th>director</th>
      <th>cast</th>
      <th>country</th>
      <th>date_added</th>
      <th>release_year</th>
      <th>duration</th>
      <th>description</th>
      <th>genre</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>s4</td>
      <td>Movie</td>
      <td>9</td>
      <td>Shane Acker</td>
      <td>Elijah Wood, John C. Reilly, Jennifer Connelly...</td>
      <td>United States</td>
      <td>November 16, 2017</td>
      <td>2009</td>
      <td>80</td>
      <td>In a postapocalyptic world, rag-doll robots hi...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>1</th>
      <td>s5</td>
      <td>Movie</td>
      <td>21</td>
      <td>Robert Luketic</td>
      <td>Jim Sturgess, Kevin Spacey, Kate Bosworth, Aar...</td>
      <td>United States</td>
      <td>January 1, 2020</td>
      <td>2008</td>
      <td>123</td>
      <td>A brilliant group of students become card-coun...</td>
      <td>Dramas</td>
    </tr>
    <tr>
      <th>2</th>
      <td>s10</td>
      <td>Movie</td>
      <td>1920</td>
      <td>Vikram Bhatt</td>
      <td>Rajneesh Duggal, Adah Sharma, Indraneil Sengup...</td>
      <td>India</td>
      <td>December 15, 2017</td>
      <td>2008</td>
      <td>143</td>
      <td>An architect and his wife move into a castle t...</td>
      <td>Horror Movies</td>
    </tr>
    <tr>
      <th>3</th>
      <td>s43</td>
      <td>Movie</td>
      <td>Çok Filim Hareketler Bunlar</td>
      <td>Ozan Açıktan</td>
      <td>Ayça Erturan, Aydan Taş, Ayşegül Akdemir, Burc...</td>
      <td>Turkey</td>
      <td>March 10, 2017</td>
      <td>2010</td>
      <td>99</td>
      <td>Vignettes of the summer holidays follow vacati...</td>
      <td>Comedies</td>
    </tr>
    <tr>
      <th>4</th>
      <td>s45</td>
      <td>Movie</td>
      <td>Æon Flux</td>
      <td>Karyn Kusama</td>
      <td>Charlize Theron, Marton Csokas, Jonny Lee Mill...</td>
      <td>United States</td>
      <td>February 1, 2018</td>
      <td>2005</td>
      <td>93</td>
      <td>Aiming to hasten an uprising, the leader of an...</td>
      <td>Action</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>523</th>
      <td>s7764</td>
      <td>Movie</td>
      <td>Zenda</td>
      <td>Avadhoot Gupte</td>
      <td>Santosh Juvekar, Siddharth Chandekar, Sachit P...</td>
      <td>India</td>
      <td>February 15, 2018</td>
      <td>2009</td>
      <td>120</td>
      <td>A change in the leadership of a political part...</td>
      <td>Dramas</td>
    </tr>
    <tr>
      <th>524</th>
      <td>s7775</td>
      <td>Movie</td>
      <td>Zodiac</td>
      <td>David Fincher</td>
      <td>Mark Ruffalo, Jake Gyllenhaal, Robert Downey J...</td>
      <td>United States</td>
      <td>November 20, 2019</td>
      <td>2007</td>
      <td>158</td>
      <td>A political cartoonist, a crime reporter and a...</td>
      <td>Cult Movies</td>
    </tr>
    <tr>
      <th>525</th>
      <td>s7779</td>
      <td>Movie</td>
      <td>Zombieland</td>
      <td>Ruben Fleischer</td>
      <td>Jesse Eisenberg, Woody Harrelson, Emma Stone, ...</td>
      <td>United States</td>
      <td>November 1, 2019</td>
      <td>2009</td>
      <td>88</td>
      <td>Looking to survive in a world taken over by zo...</td>
      <td>Comedies</td>
    </tr>
    <tr>
      <th>526</th>
      <td>s7782</td>
      <td>Movie</td>
      <td>Zoom</td>
      <td>Peter Hewitt</td>
      <td>Tim Allen, Courteney Cox, Chevy Chase, Kate Ma...</td>
      <td>United States</td>
      <td>January 11, 2020</td>
      <td>2006</td>
      <td>88</td>
      <td>Dragged from civilian life, a former superhero...</td>
      <td>Children</td>
    </tr>
    <tr>
      <th>527</th>
      <td>s7783</td>
      <td>Movie</td>
      <td>Zozo</td>
      <td>Josef Fares</td>
      <td>Imad Creidi, Antoinette Turk, Elias Gergi, Car...</td>
      <td>Sweden</td>
      <td>October 19, 2020</td>
      <td>2005</td>
      <td>99</td>
      <td>When Lebanon's Civil War deprives Zozo of his ...</td>
      <td>Dramas</td>
    </tr>
  </tbody>
</table>
<p>528 rows × 11 columns</p>
</div>



As cast data is a group of cast, no individual actor can be extracted from data using SQL. <br>
Python will be used to generate list of individual actors and make the count according to original table from SQL


```python
#Extract all actors from 'cast' in movies from 2005 to 2010
netflix_actors['cast'].tolist()
all_actors = []
delimiter = ","
all_actors = [ cast_group.split(delimiter) for cast_group in netflix_actors['cast'].tolist()]
trimmed_actors = []
all_cast = []
for cast_group in all_actors:
    trimmed_actors = [actor.strip() for actor in cast_group]
    for actor in trimmed_actors:
        all_cast.append(actor)
#All_cast has a length of 4873 actors (repeated)

unique_all_cast = list(dict.fromkeys(all_cast))
#unique_all_cast has a length of 3599 actors. Hopefully, non-repeated

#Make a Dataframe will all unique actors
all_actors_df = pd.DataFrame(data = unique_all_cast)
all_actors_df.columns = ['Actor']  #Rename column to 'Actor'

#Count the ocurrences of actor inside 'cast' in original dataframe
def count_ocurrences(row_df2,df1_column):
    search_string = row_df2['Actor']
    count_series = df1_column.str.count(search_string)
    total_count = count_series.sum()
    return total_count

all_actors_df['Movie_count'] = all_actors_df.apply(
    lambda row:count_ocurrences(row,netflix_actors['cast']),
    axis = 1
)
```

Now we can generate SQL based of previously built dataframe in Python


```python
SELECT *
FROM all_actors_df
ORDER BY Movie_count DESC
LIMIT 10
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
      <th>Actor</th>
      <th>Movie_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rajpal Yadav</td>
      <td>12</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Paresh Rawal</td>
      <td>12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Anupam Kher</td>
      <td>11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Akshay Kumar</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Kareena Kapoor</td>
      <td>9</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Boman Irani</td>
      <td>9</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Om Puri</td>
      <td>9</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ahmed Helmy</td>
      <td>9</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Shreyas Talpade</td>
      <td>8</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Katrina Kaif</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>



## 3. How many movies produced each country during the 90s? <br>
Show your results in a bar chart (horizontal)


```python
SELECT country, COUNT(show_id)
FROM netflix_df
WHERE release_year >= 1990 AND release_year <= 1999
GROUP BY country
ORDER BY count(show_id) DESC
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
      <th>country</th>
      <th>count(show_id)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>United States</td>
      <td>100</td>
    </tr>
    <tr>
      <th>1</th>
      <td>India</td>
      <td>34</td>
    </tr>
    <tr>
      <th>2</th>
      <td>United Kingdom</td>
      <td>17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hong Kong</td>
      <td>11</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Australia</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>France</td>
      <td>5</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Mexico</td>
      <td>3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Germany</td>
      <td>2</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Japan</td>
      <td>2</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Poland</td>
      <td>1</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Thailand</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Argentina</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Egypt</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Canada</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Build bar chart for yearly releases in Netflix.
list_values2 = netflix_country_90['count(show_id)'].values.tolist()
max_value2 = max(list_values2)
colors2 = ['green' if v==max_value2 else 'blue' for v in list_values2]
production_mean2 = np.mean(list_values2)

plt.figure(figsize=(18,10))
plt.barh(netflix_country_90['country'],
        width=netflix_country_90['count(show_id)'],
        color = colors2)
bars = plt.barh(netflix_country_90['country'],
        width=netflix_country_90['count(show_id)'],
        color = colors2)
plt.bar_label(bars,
             label_type='edge',
             padding=5,
             fontsize=15)
plt.axvline(production_mean2, color='gray', linestyle='--', label=f'Average: {production_mean:.2f}')
plt.show()
```  


## 4. What was the most popular genre for each year in dataset? <br>
Show the frequency and it's relative weight for that year genre frequency in a table.


```python
SELECT release_year, genre, count(show_id) AS qty
FROM netflix_df
GROUP BY release_year, genre
ORDER BY release_year DESC, genre DESC
```

```python
netflix_popular_topgenres = netflix_popular_genres.groupby(['release_year','genre']).max().sort_values(by=['qty'],ascending=False).reset_index()
netflix_popular_topgenres.drop_duplicates(subset=['release_year'], inplace=True)
netflix_popular_topgenres.sort_values(by='release_year', ascending=False)
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
      <th>release_year</th>
      <th>genre</th>
      <th>qty</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>280</th>
      <td>2021</td>
      <td>Anime Series</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2020</td>
      <td>Dramas</td>
      <td>87</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019</td>
      <td>Dramas</td>
      <td>142</td>
    </tr>
    <tr>
      <th>0</th>
      <td>2018</td>
      <td>Dramas</td>
      <td>187</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2017</td>
      <td>Dramas</td>
      <td>186</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>360</th>
      <td>1947</td>
      <td>Classic Movies</td>
      <td>1</td>
    </tr>
    <tr>
      <th>361</th>
      <td>1946</td>
      <td>Classic Movies</td>
      <td>1</td>
    </tr>
    <tr>
      <th>362</th>
      <td>1945</td>
      <td>Classic Movies</td>
      <td>1</td>
    </tr>
    <tr>
      <th>392</th>
      <td>1944</td>
      <td>Classic Movies</td>
      <td>1</td>
    </tr>
    <tr>
      <th>311</th>
      <td>1942</td>
      <td>Classic Movies</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>71 rows × 3 columns</p>
</div>



