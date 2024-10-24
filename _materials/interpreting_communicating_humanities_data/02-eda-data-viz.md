---
title: "EDA & Data Visualization"
permalink: /materials/interpreting-communicating-humanities-data/02-eda-data-viz
excerpt: "An overview of the basics of Exploratory Data Analysis (EDA) and Data Visualization for Humanities Data."
toc: true
---

In our last lesson, we tried to answer the following questions:

1. What is the average number of visits for each state?
2. What is the average number of visits for each National Park?
3. How many National Parks are there in each state?

Here's my solutions to these questions:

```python
# 1. What is the average number of visits for each state?
parks_data_df.groupby(['state'])['recreation_visits'].mean().reset_index()
# 2. What is the average number of visits for each National Park?
parks_data_df.groupby(['park_name'])['recreation_visits'].mean().reset_index()
# 3. How many National Parks are there in each state?
parks_data_df.groupby(['state'])['park_name'].nunique().reset_index()
```

We could store each of these in new variables but we also want to visualize this data to help us better understand it. That's what we'll be exploring further in this next lesson.

Rather than working with just the National Parks Data, we can also start to explore the remainder of the datasets from the *Responsible Datasets in Context Project*. For example, let's take a look at the "Top 500 Greatest Novels" dataset by Anna Preus and Aashna Sheth [https://www.responsible-datasets-in-context.com/posts/top-500-novels/top-500-novels.html](https://www.responsible-datasets-in-context.com/posts/top-500-novels/top-500-novels.html).

First we need to create a new Jupyter notebook in our `pandas-eda` folder called `Top500NovelsEDA.ipynb` and then load in the data. How can we **read** data in Pandas?

<figure>
	<a href="https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExanV4dzd4NDFqZnVqcThvOXl4b2ppNGI0ZWJhbjIyNDNwODA5YWZxbSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/NFA61GS9qKZ68/giphy.gif">
		<img src="https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExanV4dzd4NDFqZnVqcThvOXl4b2ppNGI0ZWJhbjIyNDNwODA5YWZxbSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/NFA61GS9qKZ68/giphy.gif" class="image-popup">
	</a>
</figure>

While this cat is adorable, we can also use the `pd.read_csv()` method to read in our data. 

```python
import pandas as pd

novels_df = pd.read_csv("https://raw.githubusercontent.com/melaniewalsh/responsible-datasets-in-context/main/datasets/top-500-novels/library_top_500.csv")
```

Now we can start to explore this dataset. What methods could we use to get a sense of the data?

<figure>
	<a href="https://media.giphy.com/media/I1gO1FsTvdfDG/giphy.gif?cid=790b7611lrltc1tlaib9t4ttkiobydw0fyu6678snb2gsz06&ep=v1_gifs_search&rid=giphy.gif&ct=g">
		<img src="https://media.giphy.com/media/I1gO1FsTvdfDG/giphy.gif?cid=790b7611lrltc1tlaib9t4ttkiobydw0fyu6678snb2gsz06&ep=v1_gifs_search&rid=giphy.gif&ct=g" class="image-popup">
	</a>
</figure>

There's many answers to this question, but say we use `.info()`. We would see the following output:

```bash
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 500 entries, 0 to 499
Data columns (total 29 columns):
 #   Column                    Non-Null Count  Dtype  
---  ------                    --------------  -----  
 0   top_500_rank              500 non-null    int64  
 1   title                     500 non-null    object 
 2   author                    500 non-null    object 
 3   pub_year                  500 non-null    int64  
 4   orig_lang                 500 non-null    object 
 5   genre                     500 non-null    object 
 6   author_birth              499 non-null    object 
 7   author_death              496 non-null    object 
 8   author_gender             499 non-null    object 
 9   author_primary_lang       499 non-null    object 
 10  author_nationality        499 non-null    object 
 11  author_field_of_activity  329 non-null    object 
 12  author_occupation         458 non-null    object 
 13  oclc_holdings             495 non-null    float64
 14  oclc_eholdings            495 non-null    float64
 15  oclc_total_editions       495 non-null    float64
 16  oclc_holdings_rank        495 non-null    float64
 17  oclc_editions_rank        495 non-null    float64
 18  gr_avg_rating             500 non-null    float64
 19  gr_num_ratings            500 non-null    object 
 20  gr_num_reviews            500 non-null    object 
 21  gr_avg_rating_rank        500 non-null    int64  
 22  gr_num_ratings_rank       500 non-null    int64  
 23  oclc_owi                  495 non-null    float64
 24  author_viaf               500 non-null    object 
 25  gr_url                    480 non-null    object 
 26  wiki_url                  500 non-null    object 
 27  pg_eng_url                500 non-null    object 
 28  pg_orig_url               64 non-null     object 
dtypes: float64(7), int64(4), object(18)
memory usage: 113.4+ KB
```

This overview is telling us that we have far more columns in this datasets, each column's name and data type, and specifically the number of **non-null values** in each column.

## Missing Data in Pandas

In the last lesson, we covered how we could add null values to a DataFrame, but now we want to explore how we can work with them. Pandas has a lot of documentation for handling **missing data** that you can read here [https://pandas.pydata.org/docs/user_guide/missing_data.html](https://pandas.pydata.org/docs/user_guide/missing_data.html). For our purposes, we're mostly concerned with the `isna()` and `notna()` methods.

While most of the columns have 500 non-null values, we can see that some of the columns have missing values. For example, `author_field_of_activity` has 329 non-null values, which means that there are 171 missing values. We can start to get a sense of what that column contains if we return to the dataset documentation, specifically the section "What's in the Data" [https://www.responsible-datasets-in-context.com/posts/top-500-novels/top-500-novels.html?tab=data-essay#whats-in-the-data(https://www.responsible-datasets-in-context.com/posts/top-500-novels/top-500-novels.html?tab=data-essay#whats-in-the-data)]. From there we can learn the following about this column:

> AUTHOR_FIELD_OF_ACTIVITY: Author’s primary fields of activity, according to VIAF. VIAF includes data from multiple global partner institutions, but we only collect VIAF data associated with the Library of Congress (LOC).

<figure>
	<a href="https://upload.wikimedia.org/wikipedia/commons/e/ed/VIAF_Screenshot_2012.png">
		<img src="https://upload.wikimedia.org/wikipedia/commons/e/ed/VIAF_Screenshot_2012.png" alt="VIAF" class="image-popup">
	</a>
</figure>

For those unfamiliar, VIAF stands for the Virtual International Authority File, which is a service that provides a way to link and share information about authors and their works across different libraries and institutions. The idea for VIAF originated in 1998 as an experiment in trying to link authority records. As a reminder, authority records are standardized records that provide information about a specific entity, such as an author or a work, like MARC records. VIAF was officially launched in 2003 and is operated by the Online Computer Library Center (OCLC) in partnership with various national libraries and institutions.

<figure>
	<a href="https://m.media-amazon.com/images/I/71J3wIQ6rRL._AC_UF1000,1000_QL80_.jpg">
		<img src="https://m.media-amazon.com/images/I/71J3wIQ6rRL._AC_UF1000,1000_QL80_.jpg" alt="VIAF" class="image-popup">
	</a>
</figure>

The impetus for the creation of VIAF was not just sharing information, but what LIS scholar Christine L. Borgman calls the "scaling problem in name disambiguation" in her book *Big Data, Little Data, No Data: Scholarship in the Networked World*. We've already talked about this briefly in discussing the challenges of "cleaning data", but the rise of multiple digital libraries and archives has made it increasingly difficult to accurately identify and attribute works to their correct authors, which is where projects like VIAF come in. However, as we discussed in class, there are still challenges with this system. For example, authors are not the ones who enter data into VIAF. Instead, it is often librarians or archivists who are responsible for creating and maintaining these records. This means that there can be discrepancies in how authors are represented, and it can be difficult to ensure that all works by a particular author are accurately attributed to them.

Now knowing this background, we can start to explore which authors have missing values in this column. We can do this by using the `isna()` method to check for missing values in the `author_field_of_activity` column.

```python
novels_df[novels_df['author_field_of_activity'].isna()]
```

Here we are using the `isna()` method to **filter** our DataFrame to only show rows where the `author_field_of_activity` column is null. If we wanted to see only rows where the column is not null, we could use the `notna()` method instead.

```python
novels_df[novels_df['author_field_of_activity'].notna()]
```

This will give us a DataFrame with all the authors who have a field of activity listed. We could also use the `~` operator to negate the boolean values returned by `isna()`.

```python
novels_df[~novels_df['author_field_of_activity'].isna()]
```

We haven't seen the `~` operator before, but it is a common way to negate boolean values in Python. So for example, if we had a boolean variable `x` that was `True`, then `~x` would be `False`. In addition, Pandas also has `isnull()` and `notnull()` methods that can be used interchangeably with `isna()` and `notna()`.

Finally, if we wanted to get rid of all rows in the `novels_df` DataFrame that have missing values in the `author_field_of_activity` column, we could either use our filtering logic to assign the result to a new variable or we could use the `dropna()` method.

```python
novels_df_cleaned = novels_df.dropna(subset=['author_field_of_activity'])
```

`dropna()` is a powerful method that can be used to remove missing values from a DataFrame. The `subset` parameter allows us to specify which columns we want to check for missing values. If we don't specify a subset, `dropna()` will remove any row that has a missing value in any column.

Here's a quick overview of the most common methods for handling missing values in Pandas:

| Pandas Method for Missing Values | Description | Usage |
|----------------------------------|-------------|-----|
| `isna()`                         | Returns a boolean DataFrame indicating whether each value is missing (NaN) | Can be used to filter DataFrames, Series, GroupBy objects, etc. or to identify the amount of missing data (`df.isna().sum()`) |
| `notna()`                        | Returns a boolean DataFrame indicating whether each value is not missing | Used similar to `isna()` |
| `isnull()`                      | Alias for `isna()` | Used interchangeably with `isna()` |
| `notnull()`                     | Alias for `notna()` | Used interchangeably with `notna()` |
| `dropna()`                      | Removes missing values from a DataFrame or Series | Can be used to drop rows or columns with missing values |
| `fillna()`                      | Fills missing values with a specified value or method | Can be used to replace NaN values with a specific value or interpolate missing values |

Knowing how to work with missing data is important since we often have gaps when working with cultural datasets. Furthermore, it's important to know that certain methods like `groupby()` will automatically ignore missing values, so we should always check the distribution of our data before we start analyzing it.

## Merging Data With Pandas

Now that we are understanding how to work with missing data, we can also start to explore how to augment our dataset in various ways. One of the most common ways to do this is through **merging** datasets together.

We've already seen one approach `pd.concat()` that allows us to concatenate DataFrames (aka add them together), but now we want to focus on the `merge()` method.

<figure>
	<a href="https://kentonrambsy.com/wp-content/uploads/2021/10/Post45-1024x429.jpg">
		<img src="https://kentonrambsy.com/wp-content/uploads/2021/10/Post45-1024x429.jpg" alt="Post45" class="image-popup">
	</a>
</figure>

First though we need another dataset to merge with our `novels_df`. We could take a look at sites like Kaggle to find more datasets, or more specific ones like the *Post45 Data Collective*, which "peer reviews and houses literary and cultural data from 1945 to the present" [https://data.post45.org/](https://data.post45.org/).

<figure>
	<a href="{{site.baseurl}}/assets/images/post45_data.png">
		<img src="{{site.baseurl}}/assets/images/post45_data.png" alt="Post45" class="image-popup">
	</a>
</figure>

While there's a number of relevant datasets in the *Post45 Data Collective*, we could use the one about *New York Times* Hardcover Fiction Bestsellers, 1931–2020, created by Jordan Pruett [https://data.post45.org/nyt-fiction-bestsellers-data/](https://data.post45.org/nyt-fiction-bestsellers-data/). According to the documentation, this dataset contains the following:

> The New York Times Hardcover Fiction Bestsellers (1931–2020) contains three related datasets. The first dataset provides a tabular representation of the hardcover fiction bestseller list of The New York Times every week between 1931 and 2020. The second dataset provides title-level data for every unique title that appeared on the hardcover fiction bestseller list during this time period. The third dataset provides HathiTrust Digital Library identifiers for every unique title that appeared on the hardcover fiction bestseller list and that also has a corresponding volume in the HathiTrust Digital Library.

> Previous research using similar data has been limited to partial segments of the list, such as the top 200 longest-running bestsellers since a certain date (Piper and Portelance, 2016) or bestsellers from only particular years (Sorenson, 2007). By contrast, this dataset covers the full list since its inception in 1931, along with each reported work’s title, author(s), date of appearance, and rank.

Getting this additional dataset will allow us to explore the relationship between the *New York Times* bestsellers and the top 500 novels as recorded by OCLC.

<figure>
	<a href="{{site.baseurl}}/assets/images/nyt_data.png">
		<img src="{{site.baseurl}}/assets/images/nyt_data.png" alt="NYT Data" class="image-popup">
	</a>
</figure>

If we click on the `Explore/download data` for the first dataset, we see a new page that has the remote url for the dataset: [https://raw.githubusercontent.com/ecds/post45-datasets/main/nyt_full.tsv](https://raw.githubusercontent.com/ecds/post45-datasets/main/nyt_full.tsv). 

Notice the `raw.githubusercontent.com` in the URL. This tells us that this is a raw file from a GitHub repository, which is a common way to share datasets, and we can even see the actual file here [https://github.com/Post45-Data-Collective/data/tree/main/nyt_hardcover_fiction_bestsellers](https://github.com/Post45-Data-Collective/data/tree/main/nyt_hardcover_fiction_bestsellers).

We can copy that and load it into a new DataFrame.

```python
nyt_bestsellers_df = pd.read_csv("https://raw.githubusercontent.com/ecds/post45-datasets/main/nyt_full.tsv")
```

However, we will likely see the following error in our Notebook:

```bash
ParserError: Error tokenizing data. C error: Expected 1 fields in line 163, saw 3
```

This error is telling us that there is an issue with the way the data is formatted. In this case, the dataset is actually a tab-separated values (TSV) file, which means that we need to specify the `sep` parameter when we read it in.

```python
nyt_bestsellers_df = pd.read_csv("https://raw.githubusercontent.com/ecds/post45-datasets/main/nyt_full.tsv", sep="\t")
```

Encoding errors are common when working with data, especially data that was produced by others. The reason for this is that different operating systems and software use different character encodings to represent text. When we save a file, it is encoded in a specific way, and when we open it, we need to know how it was encoded in order to read it correctly.

<figure>
    <a href="https://media.emailonacid.com/wp-content/uploads/2022/02/content_type_example.jpeg">
    <img src="https://media.emailonacid.com/wp-content/uploads/2022/02/content_type_example.jpeg" class="image-popup">
    </a>
</figure>

`ISO-8859-1` and `utf-8` are both character encodings used to represent text in computers. `ISO-8859-1`, also known as `Latin-1`, is a single-byte encoding that can represent the first 256 Unicode characters but only covers Western European languages. On the other hand, `utf-8` is far more popular because it is a variable-width character encoding that can encode all possible Unicode characters.

In our case, we didn't need to specify the encoding, but if we did, we could have added the `encoding` parameter to our `read_csv()` method.

```python
nyt_bestsellers_df = pd.read_csv("https://raw.githubusercontent.com/ecds/post45-datasets/main/nyt_full.tsv", sep="\t", encoding='utf-8')
```

Now that we have both datasets loaded, we can start to explore them. Let's take a look at the columns in the `nyt_bestsellers_df` DataFrame.

```python
nyt_bestsellers_df.info()
```

Which gives us the following output:

```bash
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 60386 entries, 0 to 60385
Data columns (total 6 columns):
 #   Column    Non-Null Count  Dtype 
---  ------    --------------  ----- 
 0   year      60386 non-null  int64 
 1   week      60386 non-null  object
 2   rank      60386 non-null  int64 
 3   title_id  60386 non-null  int64 
 4   title     60386 non-null  object
 5   author    60376 non-null  object
dtypes: int64(3), object(3)
memory usage: 2.8+ MB
```

We can see that this dataset has 60386 rows and 6 columns. The columns are `year`, `week`, `rank`, `title_id`, `title`, and `author`. We can also see that the `author` column has ten missing values, but that overall, this dataset has very few null values and is much larger than the `novels_df` DataFrame.

Now we can start to merge the two DataFrames together. We've mentioned merging datasets a bit when we discussed joining data in class and SQL, but now we want to dig into details. Often times when we're working with datasets, we will have our data split into multiple `csv` files or have differently shaped datasets (i.e. some of them will have more rows or columns than other datasets). Pandas has built in functionality that let's us merge together DataFrames so that we can perform analysis on the combined dataset, which you can read about in the very extensive documentation here [https://pandas.pydata.org/docs/user_guide/merging.html](https://pandas.pydata.org/docs/user_guide/merging.html).

While this documentation shows a number of ways to combined datasets (`concat` let's you combine datasets by stacking them on top of each other, `join` let's you combine datasets by joining them on a common index, and `merge` let's you combine datasets by joining them on a common column), we will focus on the `merge` method.

<figure>
    <a href="{{site.baseurl}}/assets/images/merge_syntax.png">
    <img src="{{site.baseurl}}/assets/images/merge_syntax.png" class="image-popup">
    </a>
</figure>

If we take a look at the documentation, we see the syntax above, which shows us that in Pandas we use the `merge()` function to combine datasets. The `merge()` function takes in a number of parameters, but the most important ones are `left`, `right`, `how`, and `on`. The `left` and `right` parameters are the DataFrames we want to merge, the `how` parameter is the type of join we want to undertake, and the `on` parameter is the column we want to join the DataFrames on.

We can also see an example of how this works in the figure below.

<figure>
    <a href="{{site.baseurl}}/assets/images/merge_example.png">
    <img src="{{site.baseurl}}/assets/images/merge_example.png" class="image-popup">
    </a>
</figure>

You'll notice that we can see a description of the types of `how` parameters we can use. These are the same types of joins that we discussed in class and in SQL. The `how` parameter can be set to `left`, `right`, `outer`, or `inner`. The `left` join will return all the rows from the left DataFrame and the matching rows from the right DataFrame. The `right` join will return all the rows from the right DataFrame and the matching rows from the left DataFrame. The `outer` join will return all the rows from the left DataFrame and the right DataFrame. The `inner` join will return only the rows that match in both DataFrames.

<figure>
    <a href="https://miro.medium.com/max/700/1*9eH1_7VbTZPZd9jBiGIyNA.png">
    <img src="https://miro.medium.com/max/700/1*9eH1_7VbTZPZd9jBiGIyNA.png" class="image-popup">
    </a>
</figure>

Now that we are starting to understand this logic, we can consider how we might merge our datasets. First, we can double check the columns in our `novels_df` and `nyt_bestsellers_df` DataFrames to see if there are any common columns we can use to merge them.

```python
novels_df.columns, nyt_bestsellers_df.columns
```

This gives us the following output:

```bash
(Index(['top_500_rank', 'title', 'author', 'pub_year', 'orig_lang', 'genre',
        'author_birth', 'author_death', 'author_gender', 'author_primary_lang',
        'author_nationality', 'author_field_of_activity', 'author_occupation',
        'oclc_holdings', 'oclc_eholdings', 'oclc_total_editions',
        'oclc_holdings_rank', 'oclc_editions_rank', 'gr_avg_rating',
        'gr_num_ratings', 'gr_num_reviews', 'gr_avg_rating_rank',
        'gr_num_ratings_rank', 'oclc_owi', 'author_viaf', 'gr_url', 'wiki_url',
        'pg_eng_url', 'pg_orig_url'],
       dtype='object'),
 Index(['year', 'week', 'rank', 'title_id', 'title', 'author'], dtype='object'))
```

We could eyeball the likely similar columns, but we can also use the `intersection()` method to find the common columns.

```python
set(novels_df.columns).intersection(set(nyt_bestsellers_df.columns))
```

This gives us the following output:

```bash
{'author', 'title'}
```

`intersection()` is a method that returns the common elements between two sets. `set()` is a built-in Python function that creates a set object, which is an unordered collection of unique elements. Sets can be very useful when you want to find the **unique** elements in a list. In this case, we are using `set()` to convert the column names of both DataFrames into sets, and then using `intersection()` to find the common columns.

Now that we know we have two columns we can use to merge the DataFrames, we can start to do that. Let's say we want to do a left join, which will return all the rows from the `novels_df` DataFrame and the matching rows from the `nyt_bestsellers_df` DataFrame.

```python
merged_df = novels_df.merge(nyt_bestsellers_df, how='left', on=['author', 'title'])
```

While this code should work, you'll notice if you print out the `merged_df` DataFrame that while there are new columns from the `nyt_bestsellers_df` in our `merged_df` there only `NaN` values in those columns. Why is that?

<figure>
	<a href="https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExaGZlbnI4M3d1dWo2bndkeWN4MTRjN2xrZWl2NWdxdjRnaDkwODhsZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/xUPGcjUQcWclgK94ti/giphy.gif">
	<img src="https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExaGZlbnI4M3d1dWo2bndkeWN4MTRjN2xrZWl2NWdxdjRnaDkwODhsZyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/xUPGcjUQcWclgK94ti/giphy.gif" class="image-popup">
	</a>
</figure>

To figure out where we went wrong, we need to check the values in the `author` and `title` columns in both DataFrames, and specifically see if any of them exist in both DataFrames. We can do this by using the `isin()` and `nunique()` method to get the unique values in each column.

```python
shared_authors = novels_df[novels_df['author'].isin(nyt_bestsellers_df['author'])]['author'].nunique()
shared_titles = novels_df[novels_df['title'].isin(nyt_bestsellers_df['title'])]['title'].nunique()

print(f"Number of shared authors: {shared_authors}")
print(f"Number of shared titles: {shared_titles}")
```

This prints out the following:

```bash
Number of shared authors: 122
Number of shared titles: 0
```

This tells us that while there are 122 shared authors, there are no shared titles. This is surprising, so let's investigate further. There's a number of ways we could solve this mystery, but let's first try seeing what the most prolific authors are in both datasets.

```python
novels_df[novels_df['author'].isin(nyt_bestsellers_df['author'])]['author'].value_counts().head(5)
```

This gives us the following output:

```bash
John Grisham          19
John Steinbeck         8
Nicholas Sparks        7
Stephen King           7
Dan Brown              5
Name: author, dtype: int64
```

And now let's do the same for the `nyt_bestsellers_df` DataFrame.

```python
nyt_bestsellers_df[nyt_bestsellers_df['author'].isin(novels_df['author'])]['author'].value_counts()
```

This gives us the following output:

```bash
Stephen King               892
John Grisham               789
David Baldacci             396
Nicholas Sparks            390
Herman Wouk                375
```

We can see that across both datasets `Stephen King` and `John Grisham` are the most prolific authors, but we still don't know why there are no shared titles. Let's try filtering for the titles in the `novels_df` and `nyt_bestsellers_df` DataFrames.

```python
novel_titles = novels_df[novels_df.author == "Stephen King"].title.unique()
nyt_titles = nyt_bestsellers_df[nyt_bestsellers_df.author == "Stephen King"].title.unique()

print(f"Stephen King's novels: {novel_titles}")
print(f"Stephen King's NYT bestsellers: {nyt_titles}")
```

This gives us the following output:

```bash
Stephen King's novels: ['The Stand' 'It' 'The Shining' 'Misery' 'Carrie' 'The Gunslinger'
 'Dreamcatcher']
Stephen King's NYT bestsellers: ['THE SHINING' 'THE STAND' 'THE DEAD ZONE' 'FIRESTARTER' 'CUJO'
 'DIFFERENT SEASONS' 'CHRISTINE' 'PET SEMATARY' 'SKELETON CREW'
 'THE BACHMAN BOOKS' 'IT' 'THE EYES OF THE DRAGON' 'MISERY'
 'THE TOMMYKNOCKERS' 'THE DARK HALF' 'FOUR PAST MIDNIGHT' 'NEEDFUL THINGS'
 "GERALD'S GAME" 'DOLORES CLAIBORNE' 'NIGHTMARES & DREAMSCAPES' 'INSOMNIA'
 'ROSE MADDER' 'DESPERATION' 'WIZARD AND GLASS' 'BAG OF BONES'
 'THE GIRL WHO LOVED TOM GORDON' 'HEARTS IN ATLANTIS' 'DREAMCATCHER'
 "EVERYTHING'S EVENTUAL" 'FROM A BUICK 8' 'THE DARK TOWER: Volumes 1-5'
 'SONG OF SUSANNAH' 'THE DARK TOWER' 'CELL' 'ELL' "LISEY'S STORY"
 'LISEY’S STORY' 'DUMA KEY' 'JUST AFTER SUNSET' 'UNDER THE DOME'
 'BLOCKADE BILLY' 'FULL DARK, NO STARS' '11/22/63'
 'THE WIND THROUGH THE KEYHOLE' 'DOCTOR SLEEP' 'MR. MERCEDES' 'REVIVAL'
 'FINDERS KEEPERS' 'THE BAZAAR OF BAD DREAMS' 'END OF WATCH'
 'THE OUTSIDER' 'ELEVATION' 'THE INSTITUTE' 'IF IT BLEEDS']
```

We can see that while King has had many bestsellers, there is overlap in the titles, but they are not the same. The titles in the `nyt_bestsellers_df` DataFrame are all in uppercase, while the titles in the `novels_df` DataFrame are in title case. This is likely the reason why there are no shared titles in the two DataFrames.

We could fix this a number of ways, but let's just convert the titles in the `nyt_bestsellers_df` DataFrame to title case.

```python
nyt_bestsellers_df = nyt_bestsellers_df.rename(columns={'title': 'nyt_title'})
nyt_bestsellers_df['title'] = nyt_bestsellers_df['nyt_title'].str.capitalize()
```

Now we can try merging the two DataFrames again.

```python
merged_df = novels_df.merge(nyt_bestsellers_df, how='left', left_on=['author', 'title'], right_on=['author', 'title'])
```

This time we should see that there are some matches! Now we can start to explore this data and consider what questions we want to ask.

## EDA With Pandas

So far, we have been focused on combining our datasets but now that we have started we also want to start inspecting them: this process is often called *Exploratory Data Analysis*.

<figure>
    <a href="{{site.baseurl}}/assets/images/eda.png">
    <img src="{{site.baseurl}}/assets/images/eda.png" class="image-popup">
    </a>
    <figcaption>From Zoë Wilkinson Saldaña "Sentiment Analysis for Exploratory Data Analysis" <a href="https://programminghistorian.org/en/lessons/sentiment-analysis">https://programminghistorian.org/en/lessons/sentiment-analysis</a></figcaption>
</figure>

You've likely heard this term before, but to give a bit of history In his 1977 book *Exploratory Data Analysis*, John W. Tukey argued that EDA could help suggest hypotheses that could be then tested statistically.

<figure>
    <a href="https://m.media-amazon.com/images/I/71aiG2h5WjL._AC_UF1000,1000_QL80_.jpg">
    <img src="https://m.media-amazon.com/images/I/71aiG2h5WjL._AC_UF1000,1000_QL80_.jpg" class="image-popup">
    </a>
</figure>

EDA emphasizes understanding the data’s underlying structure and extracting important variables, detecting outliers and anomalies, and testing underlying assumptions through visualizations, statistics, and other methods, without initially focusing on formal modeling or hypothesis testing. This approach differs with traditional hypothesis-driven analyses, promoting a more flexible and intuitive investigation into the data.

With Pandas we can perform all of the necessary steps for EDA.

1. We want to explore the data, so we could use `head()`, `tail()`, or `sample()` methods to display a subset of the dataset. We could also use `.shape` or `.dtypes` to see shaped of the DataFrame and the data types of each column.

2. Next we might use the `describe()` method to output a summary of our dataset. According to the documentation for this method, it provides "descriptive statistics include those that summarize the central tendency, dispersion and shape of a dataset’s distribution, excluding NaN values." [(Read the docs here)](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.describe.html).

3. After getting this bird's eye view of the distribution of the data, we could use `sort_values()` or `nsmallest()` / `nlargest()` to either organize the dataset or return the rows with maximum or minimum of values in a certain column.

4. We can use the `isna()` to see what values might be missing in the dataset. Remember that `isna()` can be run on either individual columns or the entire DataFrame, and that you can chain it with other methods `any()` to check if there are *any* null values in the data.

5. Finally we would use the `plot()` method to do some initial graphing of the trends in our data to fully help explore its distribution and potential correlations.

So in our case, we might first use `.dtypes` on our `merged_df` to see what types of data we have.

```python
merged_df.dtypes
```

Then we might use the `describe()` method to output a summary of our dataset.

```python
merged_df.describe()
```

We could also use the `sort_values()` method to organize the dataset by the `year` or `pub_year` column.

```python
merged_df.sort_values('year')
```

We could also use the `isna()` method to see what values might be missing in the dataset.

```python
merged_df.isna().any()
```

Now that we have a sense of the distribution of our data, we can start to visualize it to see if we can answer our questions.

Like many of the methods above, `plot` is built into Pandas and is a wrapper around the `matplotlib` library. This means that we can use the `plot()` method to create a variety of different plots. You can read more about the `plot()` method in the Pandas documentation here [https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.html](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.plot.html) and more generally about visualization with Pandas here [https://pandas.pydata.org/pandas-docs/stable/user_guide/visualization.html](https://pandas.pydata.org/pandas-docs/stable/user_guide/visualization.html).

For the `plot` syntax, we usually need to specify the type of graph we want to create, and then what we want on the x and y axis. For example, if we wanted to create a scatter plot looking at the relationship between the `pub_year` and `top_500_rank` columns, we could do the following:

```python
merged_df.plot(kind='scatter', x='pub_year', y='top_500_rank')
```

We should see the following graph:

<figure>
    <a href="{{site.baseurl}}/assets/images/toprank_year_novels.png">
    <img src="{{site.baseurl}}/assets/images/toprank_year_novels.png" class="image-popup">
    </a>
</figure>

While this gives us some sense of our data, we actually have a lot of different columns we could visualize, in which case it might be helpful to group or filter our data by a certain column. For example, we could group by `genre` and then plot the average `top_500_rank` for each genre.

```python
merged_df.groupby('genre')['top_500_rank'].mean().plot(kind='bar')
```

Which gives us the following output:

<figure>
	<a href="{{site.baseurl}}/assets/images/genre_mean.png">
	<img src="{{site.baseurl}}/assets/images/genre_mean.png" class="image-popup">
	</a>
</figure>

### "Cleaning" Data With Pandas

In our current example, we've already started to clean our data by merging the DataFrames together and transforming the data to make it more amenable to analysis. We might also want to subset the data:

```python
subset_merged_df = merged_df[merged_df.nyt_title.notna()]
```

As we discussed in class, a lot of what motivates this "cleaning", whether that means normalizing distributions, removing null values, and even transforming some of the data to standardize it, is the concept of **GIGO**.

<figure>
    <a href="https://www.harrised.com/150/images/gigo.jpeg">
    <img src="https://www.harrised.com/150/images/gigo.jpeg" class="image-popup">
    </a>
</figure>

GIGO stands for "Garbage In, Garbage Out" (to read more about the origins of GIGO, read this article [https://www.atlasobscura.com/articles/is-this-the-first-time-anyone-printed-garbage-in-garbage-out](https://www.atlasobscura.com/articles/is-this-the-first-time-anyone-printed-garbage-in-garbage-out)). GIGO is important because it essentially means that the quality of your data analysis is always dependent on the quality of your data.

However, for as much as we want to prioritize quality, what are some of the downsides or tradeoffs of cleaning data? Some potential considerations include:

- losing the specificity of the original data (especially a danger for historic data but also for data collected by certain institutions)
- privileging computational power over data quality or accuracy
- releasing datasets publicly without documenting these changes

This list is by no means exhaustive! But number one thing to remember with datasets is that **the act of collecting data is in of itself an interpretation**. And so cleaning data adds another layer of interpretation (sometimes many layers) to the dataset, which is why its crucial to keep a record of how you transform your data.

It is also important to realize that even with cleaning you will never have a *perfect* dataset and to remember that this is often an *iterative process* and not a one time thing. You will likely need to re-transform your data many times depending on your methods. We've only barely scratched the surface of data cleaning here (for some more examples you can check out [https://towardsdatascience.com/the-ultimate-guide-to-data-cleaning-3969843991d4](https://towardsdatascience.com/the-ultimate-guide-to-data-cleaning-3969843991d4)).

One best practice is to save multiple versions of your dataset as `csv` files, so that you don't either overwrite your original data or have to rerun previous transformations. Remember that with Pandas we can read and write `csv` files using `pd.read_csv()` and `{name of your dataframe}.to_csv()`.

In addition to thinking about our choices, it is also important to think about the types of data we are working with.

<figure>
    <a href="{{site.baseurl}}/assets/images/data_types.png">
    <img src="{{site.baseurl}}/assets/images/data_types.png" class="image-popup">
    </a>
</figure>

This graph outlines the main types of data you might encounter, which largely breaks down between qualitative (categorical) or quantitative (numerical).

For example, we have `genre` and `author` as categorical variables, while `top_500_rank` and `pub_year` are numerical variables.

## Data Visualization With Altair

Up to now we've been using Pandas built in `plot` methods to display our data. While this is helpful for quick analyses, you'll likely want more options for both how you visualize the data and interact with it.

<figure>
    <a href="https://source.opennews.org/articles/what-i-learned-recreating-one-chart-using-24-tools/" >
    <img src="https://media.opennews.org/img/24tools/big_chart.png" class="image-popup">
    </a>
</figure>

<figure>
    <a href="https://www.anaconda.com/blog/python-data-visualization-2018-why-so-many-libraries" >
    <img src="https://optimise2.assets-servd.host/voracious-blesbok/production/Blog/PythonVisLandscape.jpg?w=1200&auto=compress%2Cformat&fit=crop&dm=1632326979&s=35cf543e04fd14bcc881ef8e70363860" alt="python lib tools" class="image-popup">
    </a>
</figure>

In Python, there are a number of visualization libraries, including `Matplotlib`, `Seaborn`, and `Plotly` that have extensive communities and documentation. 

<figure>
	<a href="https://cdn.cssauthor.com/wp-content/uploads/2023/04/Altair.jpg?strip=all&lossy=1&ssl=1">
		<img src="https://cdn.cssauthor.com/wp-content/uploads/2023/04/Altair.jpg?strip=all&lossy=1&ssl=1" alt="Altair" class="image-popup">
	</a>
</figure>

Today we're going to focus on [Vega-Altair](https://altair-viz.github.io/index.html), which is a Python library built on top of Vega and Vega-Lite - two visualization libraries for JavaScript. Previously, this library was known as just `Altair` so for brevity in this lesson we will refer to it as such. The library was started in 2016 by Jake VanderPlas and has since grown to be one of the most popular libraries for data visualization in Python.

*Let's install Altair*

Remember to activate your virtual environment!

```sh
pip install "altair[all]"
```

*Now let's try it out in our Jupyter Notebook*

```python
import altair as alt
```

Because we are using Altair in a Jupyter Notebook we also need to add a few settings (you read more about this here [https://altair-viz.github.io/user_guide/display_frontends.html#](https://altair-viz.github.io/user_guide/display_frontends.html#)).

If you are running your Notebook in the browser, you can run the following after you import Altair (more info here [https://altair-viz.github.io/user_guide/display_frontends.html#displaying-in-jupyter-notebook](https://altair-viz.github.io/user_guide/display_frontends.html#displaying-in-jupyter-notebook)):

```python
# Optional in Jupyter Notebook: requires an up-to-date vega nbextension.
alt.renderers.enable('notebook')
```

If you are running it in VS Code, you can run the following (more info here [https://altair-viz.github.io/user_guide/display_frontends.html#displaying-in-vscode](https://altair-viz.github.io/user_guide/display_frontends.html#displaying-in-vscode)):

```python
# Optional in VS Code
alt.renderers.enable('mimetype')
```

Now we can try out Altair with one of the built-in datasets

```python
import altair as alt
from vega_datasets import data

source = data.cars()

alt.Chart(source).mark_circle(size=60).encode(
    x='Horsepower',
    y='Miles_per_Gallon',
    color='Origin',
    tooltip=['Name', 'Origin', 'Horsepower', 'Miles_per_Gallon']
)
```

You should now see the following graph:

<figure>
    <a href="{{site.baseurl}}/assets/images/altair_chart.png" >
    <img src="{{site.baseurl}}/assets/images/altair_chart.png" class="image-popup">
    </a>
</figure>

If you don't see the graph and you're running your Jupyter notebook in the browser, you might need to set the kernel of your notebook [https://stackoverflow.com/questions/47295871/is-there-a-way-to-use-pipenv-with-jupyter-notebook](https://stackoverflow.com/questions/47295871/is-there-a-way-to-use-pipenv-with-jupyter-notebook) or set the vega extension with following:

```sh
jupyter nbextension install --sys-prefix --py vega
```

*So let's breakdown what we're doing here*

First we are specifying a new [`Chart` class](https://altair-viz.github.io/user_guide/generated/toplevel/altair.Chart.html#altair.Chart). In Altair, there are a number of Chart types that we'll delve into later but it essentially is the basic class we'll be working with (more information here [https://altair-viz.github.io/user_guide/api.html#top-level-objects](https://altair-viz.github.io/user_guide/api.html#top-level-objects)).

<figure>
    <a href="{{site.baseurl}}/assets/images/top_level_charts.png" >
    <img src="{{site.baseurl}}/assets/images/top_level_charts.png" class="image-popup">
    </a>
</figure>

We pass our data to the `Chart` and then specify the type of `mark` we are using (in our case we are using `mark_point()` to make points). In Altair, we can use all types of marks to represent our data <https://altair-viz.github.io/user_guide/marks.html>.

<figure>
    <a href="{{site.baseurl}}/assets/images/mark_types.png" >
    <img src="{{site.baseurl}}/assets/images/mark_types.png" class="image-popup">
    </a>
</figure>

Finally we are calling encoding to specify what variable we want to represent on the `x` and `y` axis, as well as through the `color` encoding. Altair has many fields for encoding [https://altair-viz.github.io/user_guide/encoding.html](https://altair-viz.github.io/user_guide/encoding.html)

<figure>
    <a href="{{site.baseurl}}/assets/images/encodings.png" >
    <img src="{{site.baseurl}}/assets/images/encodings.png" class="image-popup">
    </a>
</figure>

<figure>
    <a href="{{site.baseurl}}/assets/images/encoding_data_types.png" >
    <img src="{{site.baseurl}}/assets/images/encoding_data_types.png" class="image-popup">
    </a>
</figure>
---

*So let's try recreating our graph charting the rank of genres over time*

```python
alt.Chart(merged_df).mark_line().encode(
	x='pub_year',
	y='top_500_rank',
	color='genre'
)
```

You should see the following graph:

<figure>
    <a href="{{site.baseurl}}/assets/images/altair_dialogue.png" >
    <img src="{{site.baseurl}}/assets/images/altair_dialogue.png" class="image-popup">
    </a>
</figure>

What are some of the problems with this graph? Notice that unlike Pandas plot, Altair is not rendering our year as a time series. This is because Altair is not recognizing our `year` column as a temporal field. We can fix this by specifying the data types for Altair [https://altair-viz.github.io/user_guide/encoding.html#encoding-data-types](https://altair-viz.github.io/user_guide/encoding.html#encoding-data-types).

```python
alt.Chart(grouped_dialogue_df).mark_line().encode(
    x='year:T',
    y='words',
)
```

This should give us the following graph:

<figure>
    <a href="{{site.baseurl}}/assets/images/altair_dialogue_temporal.png" >
    <img src="{{site.baseurl}}/assets/images/altair_dialogue_temporal.png" class="image-popup">
    </a>
</figure>

But now we have an even weirder looking graph. That's because even though we told Altair that year is temporal, our data type doesn't reflect that. We can fix this by using the `pd.to_datetime()` method to convert our `year` column to a datetime object.

```python
grouped_dialogue_df['date'] = grouped_dialogue_df['year'].astype(str) + '-01-01'
grouped_dialogue_df['date'] = pd.to_datetime(grouped_dialogue_df['date'])
```

Here I am telling Pandas to convert the `year` column to a string and then adding `-01-01` to the end of it to make it a datetime object. Then I am using the `pd.to_datetime()` method to convert it to a datetime object and saving it to a new column called `date`. The choice to save it to a new column is because we might want to keep the original `year` column for other analyses.

Now let's try rerunning our graph but using `date`.

```python
alt.Chart(grouped_dialogue_df).mark_line().encode(
    x='date:T',
    y='words',
)
```

This should give us the following graph:

<figure>
    <a href="{{site.baseurl}}/assets/images/altair_dialogue_temporal_date.png" >
    <img src="{{site.baseurl}}/assets/images/altair_dialogue_temporal_date.png" class="image-popup">
    </a>
</figure>

Now we have officially recreated what we did with `plot()` but this takes far more work, so why should we bother? The biggest reason is that Altair is more flexible and powerful than Pandas `plot()` and can create more complex visualizations. For example, we can add a `color` encoding to our graph to see how dialogue is split between genders.

```python
grouped_dialogue_df = dialogue_df.groupby(['year', 'gender'])['words'].sum().reset_index()
grouped_dialogue_df['date'] = grouped_dialogue_df['year'].astype(str) + '-01-01'
grouped_dialogue_df['date'] = pd.to_datetime(grouped_dialogue_df['date'])
alt.Chart(grouped_dialogue_df).mark_line().encode(
    x='date:T',
    y='words',
    color='gender'
)
```

This should give us the following graph:

<figure>
    <a href="{{site.baseurl}}/assets/images/altair_dialogue_temporal_gender.png">
    <img src="{{site.baseurl}}/assets/images/altair_dialogue_temporal_gender.png" class="image-popup">
    </a>
</figure>

This is a much more complex graph than we could have created with Pandas `plot()` and it's also more flexible. We can also add a `tooltip` encoding to our graph to see the exact values of the data.

```python
alt.Chart(grouped_dialogue_df).mark_line().encode(
    x='date:T',
    y='words
    color='gender',
    tooltip=['date', 'words']
)
```

We can also add a `title` to our graph to make it more readable.

```python
alt.Chart(grouped_dialogue_df).mark_line().encode(
    x='date:T',
    y='words
    color='gender',
    tooltip=['date', 'words']
).properties(
    title='Dialogue Over Time Colored By Gender'
)
```

As we are starting to see Altair is a very powerful library. We will continue to explore its functionality in the coming weeks, but would highly encourage you to take a look at the example visualizations on the Altair website [https://altair-viz.github.io/gallery/index.html](https://altair-viz.github.io/gallery/index.html) and the documentation [https://altair-viz.github.io/index.html](https://altair-viz.github.io/index.html).

## Visualizing the Data Homework

