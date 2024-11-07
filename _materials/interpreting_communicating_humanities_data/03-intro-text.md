---
title: "Introduction to Unstructured Text Data"
permalink: /materials/interpreting-communicating-humanities-data/03-intro-text
toc: true
altair: true
---

In the last lesson, we learned the foundations of EDA and how to use Pandas and Altair to explore and visualize data. In this lesson, we will be focusing on unstructured text data and how we can structure it for analysis.

In the previous lesson, you might have noticed that we used the following code to manipulate the novel titles:

```python
nyt_bestsellers_df = nyt_bestsellers_df.rename(columns={'title': 'nyt_title'})
nyt_bestsellers_df['title'] = nyt_bestsellers_df['nyt_title'].str.capitalize()
```

Part of what makes Pandas so powerful is that it has a number of built-in methods for working with `string` data, and you can access them by using the `.str` accessor method. An in-depth discussion of these methods are available on the Pandas documentation website [https://pandas.pydata.org/docs/user_guide/text.html](https://pandas.pydata.org/docs/user_guide/text.html), but I've also summarized many of the more popular methods below in this table:

| Pandas String Method | Explanation |
|---------------------|-------------|
| `df[‘column_name’].str.lower()` | lowercase all the values in a column |
| `df[‘column_name’].str.upper()` | uppercase all the values in a column |
| `df[‘column_name’].str.replace(‘old_string’, ‘new_string’)` | replace all instances of ‘old_string’ with ‘new_string’ in a column |
| `df[‘column_name’].str.split(‘delimiter’)` | split a column by ‘delimiter’ like a comma or period, or really anything |
| `df[‘column_name’].str.strip()` | remove leading and trailing whitespace from a column |
| `df[‘column_name’].str.len()` | count the number of characters in a column |
| `df[‘column_name’].str.contains(‘pattern’)` | check if a column contains a particular pattern |
| `df[‘column_name’].str.startswith(‘pattern’)` | check if a column starts with a particular pattern |
| `df[‘column_name’].str.endswith(‘pattern’)` | check if a column ends with a particular pattern |
| `df[‘column_name’].str.find(‘pattern’)` | find the first occurrence of a particular pattern in a column |
| `df[‘column_name’].str.findall(‘pattern’)` | find all occurrences of a particular pattern in a column |
| `df[‘column_name’].str.count(‘pattern’)` | count the number of occurrences of a particular pattern in a column |
| `df[‘column_name’].str.extract(‘pattern’)` | extract the first occurrence of a particular pattern in a column |
| `df[‘column_name’].str.extractall(‘pattern’)` | extract all occurrences of a particular pattern in a column |
| `df[‘column_name’].str.join(list)` |  join a list of strings with a delimiter |

In our case, we used the `str.capitalize()` method to capitalize the first letter of each word in the `title` column. This is a simple example, but it shows how powerful Pandas can be for working with text data.

Today we will be exploring more advanced methods for working with text data, and how we can structure it for analysis. But first, we need some text.

First, let's create a new folder in our `is310-coding-directory` called `text-analysis`, and in it create a new Jupyter Notebook called `IntroTextAnalysis.ipynb`.

In that notebook, you should load in both the `nyt_bestsellers_df` and `novels_df` DataFrames that we created in the last lesson, and then once again merge them together.

```python
combined_novels_nyt_df = novels_df.merge(nyt_bestsellers_df, how='left', on=['author', 'title'])
```

Now that we have our DataFrame, we can start to look more closely at some of the columns. Specifically, we could take a look at the columns with `url` in the DataFrame. We have four: `gr_url`, `wiki_url`, `pg_eng_url`, and `pg_orig_url`. We could subset our `novels_df` to only these columns and then use the `.describe()` method to see how many unique values we have.

```python
combined_novels_nyt_df[['gr_url', 'wiki_url', 'pg_eng_url', 'pg_orig_url']].describe()
```

Which gives us the following output:

<figure>
	<a href="{{site.baseurl}}/assets/images/novels_urls.png">
		<img src="{{site.baseurl}}/assets/images/novels_urls.png" class="image-popup">
	</a>
</figure>

This gives us a sense of the distribution of the URLs in our dataset. For example, we can see that the `pg_orig_url` column has only 64 non-null values, which means that there are 657 missing values. We could also just explore one row to get a better sense of what these URLs look like.

```python
combined_novels_nyt_df[['gr_url', 'wiki_url', 'pg_eng_url', 'pg_orig_url']][0:1].to_dict()
```

Which gives us the following output:

```python
{'gr_url': {0: 'https://www.goodreads.com/book/show/3836.Don_Quixote?from_search=true&from_srp=true&qid=Npjwg6DCCM&rank=1'},
 'wiki_url': {0: 'https://en.wikipedia.org/wiki/Don_Quixote'},
 'pg_eng_url': {0: 'https://www.gutenberg.org/cache/epub/996/pg996.txt'},
 'pg_orig_url': {0: 'https://www.gutenberg.org/cache/epub/2000/pg2000.txt'}}
```

This detail is helpful but let's first breakdown the different methods I'm using to get a sense of the data. First, I'm using the two square brackets to subset the DataFrame to only the columns I want, using the exact column names. Then I'm using square brackets again to select the index of the first row of the DataFrame. Finally, I'm using the `to_dict()` method to convert the DataFrame to a dictionary, which makes it easier to read the full values in the output.

If we copy the `pg` urls into our browser we can see that `pg_orig_url` is the original Spanish text of the novel, while `pg_eng_url` is the English translation. We could use either of these urls to scrape the text of the novels and then analyze them further.

Let's test that out with the `pg_eng_url` column. We can use the `requests` library to get the text of the novel. We are not using `BeautifulSoup` here since we are just essentially downloading the text file and there is no HTML to parse.

```python
import requests

url = novels_df['pg_eng_url'][0]
response = requests.get(url)
if response.status_code == 200:
	text = response.text
	print(text[:500])  # Print the first 500 characters of the text
else:
	print(f"Failed to retrieve text. Status code: {response.status_code}")
```

This code should print the following output:

```bash
The Project Gutenberg eBook of Don Quixote
    
This ebook is for the use of anyone anywhere in the United States and
most other parts of the world at no cost and with almost no restrictions
whatsoever. You may copy it, give it away or re-use it under the terms
of the Project Gutenberg License included with this ebook or online
at www.gutenberg.org. If you are not located in the United States,
you will have to check the laws of the country where you are located
before using this eBook.
```

Now we can see that we have successfully scraped the text of the novel! Our next step is to do this for all the novels in our dataset. We could create a new DataFrame to store the text of each novel, but we also want to be careful about how we handle missing values.

To do this we should loop through the `combined_novels_nyt_df` DataFrame and check if the `pg_eng_url` column is not null. If it is not null, we can scrape the text and store it in a new DataFrame. Here's how we could do that:

```python
import requests

def get_text(url):
	# Check if the url is not null
	if pd.notna(url):
		# Try getting the text from the url
		try:
			response = requests.get(url)
			# Check if the response is successful and return the text. Else return None
			if response.status_code == 200:
				return response.text
		except Exception as e:
			return None	
	return None
```

Now to run this function we could loop through our DataFrame, similar to how we have in the past but we cannot simply just loop through a DataFrame like we would a list:

```python
for row in combined_novels_nyt_df:
	print(row)
```

If you run this code, you will see that it prints out the column names of the DataFrame, not the rows. This is because a DataFrame is a more complex data structure than a list, and you need to use the `iterrows()` method to loop through the rows of a DataFrame.

```python
for index, row in combined_novels_nyt_df.iterrows():
	print(row)
```

Now we can access each row and therefore each column in the DataFrame. We can use this to apply our `get_text()` function to the `pg_eng_url` column.

```python
texts = []
for index, row in combined_novels_nyt_df.iterrows():
	text = get_text(row['pg_eng_url'])
	texts.append(text)

# Create a new DataFrame with the text of each novel
novel_texts_df = pd.DataFrame({'text': texts})
# Merge the new DataFrame with the original DataFrame
combined_novels_nyt_df = pd.concat([combined_novels_nyt_df, novel_texts_df], axis=1)
```

You'll notice here that I'm creating an empty list called `texts` and then appending the text of each novel to this list. This is a common pattern in Python, and it's a good way to store the results of a loop. Once we have all the texts, we can create a new DataFrame called `novel_texts_df` and then merge this DataFrame with the original DataFrame.

However, there's an even more efficient way to do this using the `apply()` method, documentation available here [https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.apply.html). This method applies a function to each row of a DataFrame, which is exactly what we want to do in this case.

```python
combined_novels_nyt_df['eng_text'] = combined_novels_nyt_df['pg_eng_url'].apply(get_text)
```

<figure>
	<a href="https://i.sstatic.net/b4J3f.png">
	<img src="https://i.sstatic.net/b4J3f.png" class="image-popup">
	</a>
</figure>

You'll notice in this graphic that this `apply()` method is similar to the `groupby` method we learned in the last lesson. It also indicates that apply is a method that can take arguments, specifically the `axis` argument which can be set to `0` or `1` to apply the function to the rows or columns of the DataFrame. In our case we are selecting one column, so we are using `apply` on a Series, which is why we don't need to specify the `axis` argument.

However, we could also do the following. Say we only wanted to get texts that existed in both English and the Original language. We could update our `get_text` function to take two urls and then apply it to the DataFrame like this:

```python
def get_text(row):
	eng_url = row.pg_eng_url
	orig_url = row.pg_orig_url
	# Check if the url is not null
	if pd.notna(eng_url) and pd.notna(orig_url):
		# Try getting the text from the url
		try:
			eng_text_response = requests.get(eng_url)
			# Check if the response is successful and return the text. Else return None
			if eng_text_response.status_code == 200:
				return eng_text_response.text
		except Exception as e:
			return None
	return None

combined_novels_nyt_df['pg_eng_text'] = combined_novels_nyt_df.apply(get_text, axis=1)
```

Here we are passing in the entire row to the function, which is useful if we need to use data from multiple columns to do our transformations. You might see a warning though when you run this code:

```bash
SettingWithCopyWarning: 
A value is trying to be set on a copy of a slice from a DataFrame.
Try using .loc[row_indexer,col_indexer] = value instead
```

This is telling us that we are trying to set a value on a copy of the DataFrame, which is not recommended. To avoid this warning, we can use the `loc` method to set the value on the original DataFrame.

```python
combined_novels_nyt_df.loc[:, 'pg_eng_text'] = combined_novels_nyt_df.apply(get_text, axis=1)
```

Now we are telling Pandas to select the entire DataFrame and then set the value of the `pg_eng_text` column to the result of the `get_text` function, but specifically using the `loc` method and `:` to select the entire DataFrame.

We could even updated our code to check for the status code of the `orig_url` as well, and then return both texts if they are successful.

```python
def get_text(row):
	eng_url = row.pg_eng_url
	orig_url = row.pg_orig_url
	# Check if the url is not null
	if pd.notna(eng_url) and pd.notna(orig_url):
		# Try getting the text from the url
		try:
			eng_text_response = requests.get(eng_url)
			orig_text_response = requests.get(orig_url)
			# Check if the response is successful and return the text. Else return None
			if eng_text_response.status_code == 200 and orig_text_response.status_code == 200:
				return eng_text_response.text, orig_text_response.text
		except Exception as e:
			return None, None	
	return None, None

combined_novels_nyt_df[['pg_eng_text', 'pg_orig_text']] = combined_novels_nyt_df.apply(get_text, axis=1, result_type='expand')
```

Now we are returning both the English and Original texts if they are successful. We are also using the `result_type='expand'` argument to expand the result of the function into separate columns.

While looping is still useful, it is highly recommended to use the `apply()` method when working with DataFrames in Pandas. It is more efficient and easier to read, and it is a good way to get started with more advanced data manipulation in Pandas. You can see the time comparison between different methods for looping and applying functions to DataFrames in this graphic below:

<figure>
	<a href="https://www.ml4devs.com/images/illustrations/pandas-dataframe-apply-performace-comparision.webp">
	<img src="https://www.ml4devs.com/images/illustrations/pandas-dataframe-apply-performace-comparision.webp" class="image-popup">
	</a>
</figure>

## Unstructured Data

So far we have primarily been focused on either collecting and curating data, or exploring it using data visualization and descriptive statistics. But a huge part of working with cultural data is actually analyzing it.

To do that though requires understanding how computational methods can help us find patterns and outliers in cultural data. Here's the graph from Ted Underwood that gives us some idea of the range of computational methods available to humanists (and remember this was published in 2015, so there are even more options now):

<figure>
    <a href="https://tedunderwood.files.wordpress.com/2015/05/casualmap1.jpg">
    <img src="https://tedunderwood.files.wordpress.com/2015/05/casualmap1.jpg" class="image-popup">
    </a>
</figure>

When working with data, you'll often come across the terms "structured" and "unstructured" data. There are lots of definitions online, like this graph below:

<figure>
    <a href="http://intellspot.com/wp-content/uploads/2020/04/Structured-vs-unstructured-data-an-infographic-1024x724.png">
    <img src="http://intellspot.com/wp-content/uploads/2020/04/Structured-vs-unstructured-data-an-infographic-1024x724.png" class="image-popup">
    </a>
</figure>

This graph is technically correct, but it is also a bit confusing when you think about it (even highly organized data can be difficult to analyze for example and why can't things like email be analyzed in spreadsheets?).

In our last lesson, we explored how some of the values in these spreadsheets are **categorical** values and how some are **numerical** values. And finally how some are a bit in between, like `year` which is technically represented as a number and we could treat it as either a **continuous** or **discrete** value (so for example, if it is a date that is continuous, but if it is a year it is discrete), or we could treat it as a categorical value that is **ordinal** (i.e. is a category with an implied order).

The big thing though is that all these values are structured. We do not need to do any processes to turn these values into data to analyze. We might group them, we might filter them, we might fill in missing values, and we can plot them -- but essentially this is a dataset ready to be analyzed.

Now that we have the texts of novels in our dataset, we have unstructured data. This is data that is not easily searchable or analyzed. It is often in the form of text, images, or audio, and it requires additional processing to make it useful. This is where text analysis comes in and specifically we need to go through a process of curating and cleaning the data **from unstructured to structured data**, similar to this graph below:

<figure>
    <a href="https://i.pinimg.com/736x/b0/04/b5/b004b543748ba1350c5d66c16c678607.jpg">
    <img src="https://i.pinimg.com/736x/b0/04/b5/b004b543748ba1350c5d66c16c678607.jpg" class="image-popup">
    </a>
</figure>

## Structuring Cultural Data

First, let's talk about what we mean by structured and unstructured data. Structured data is data that is already organized in a way that is easily searchable and can be analyzed (think most tabular data or metadata). This is the type of data that we have been working with so far in our datasets. Conversely, while we store unstructured data in tables, it is not easy to process it or find patterns in it. This is the type of data that we have in our novel texts.

To structure this data, we need to think about how we can break it down into smaller, more manageable pieces. This is where text analysis comes in. Now it's worth noting that there is no one way to structure data, and it often depends on your goals and the data you have. But in general it can involve things like cleaning the data, removing unnecessary information, and organizing the data in a way that makes it easier to analyze.

*Text analysis* is a very broad term for a whole set of practices and overlapping disciplines and fields, some of which are shown in this figure below:

<figure>
    <a href="https://miro.medium.com/max/1200/0*ewkxRItArykG27dU.png">
    <img src="https://miro.medium.com/max/1200/0*ewkxRItArykG27dU.png" class="image-popup">
    </a>
</figure>

But again this figure has its limits. For example, data mining is something that Library & Information Sciences undertakes, and databases are used across these domains.

Rather than trying to define text analysis as a whole, I find a more helpful distinction is talking about Information Extraction vs Information Retrieval. These are two of the most common ways to structure text data, and they are often used in combination.

**Information Retrieval (IR):**

<figure>
	<a href="{{site.baseurl}}/assets/images/IR_figure.png">
	<img src="{{site.baseurl}}/assets/images/IR_figure.png" class="image-popup">
	</a>
</figure>

Information retrieval first developed in the 1950s and 1960s (remember Vannevar Bush and the Memex!) and its goal is to find relevant documents for a user’s needs or queries, which is why it is most often associated with search engines. It requires parsing textual information and finding patterns in the text.

The key thing to remember about IR is that it is about finding relevant documents, not necessarily understanding the content of those documents. This is why it is often used for text classification of documents, and it can be done without understanding syntax and treating documents as a "bag of words."

TLDR:

- Goal of finding relevant documents for user’s needs/queries (eg. Google)
- Often used for text classification of documents
- Can be done without “understanding” syntax and treating documents as “bag of words”

**Information Extraction (IE)**

Information Extraction (or IE) is a bit different than IR. It is about extracting structured information from unstructured text. This is a bit more complex than IR, and it often involves things like Named Entity Recognition (or NER) and Sentiment Analysis.

<figure>
	<a href="{{site.baseurl}}/assets/images/IE_figure.png">
	<img src="{{site.baseurl}}/assets/images/IE_figure.png" class="image-popup">
	</a>
</figure>

It first developed in the 1990s and is focused on extracting specific features from documents. It requires both syntactic and semantic analysis, and it is often used in fields like Natural Language Processing (NLP) and Computational Linguistics.

TLDR:

- Goal of extracting specific features from documents
- Focused on linguistic analysis of textual features
- Requires both syntactic and semantic analysis

### Text Analysis with Pandas

Now that we have a sense of the different potential methods and approaches to text analysis, let's start with a simple example using Pandas. We can start by counting the number of words in each novel in our dataset.

```python
combined_novels_nyt_df['novel_length'] = combined_novels_nyt_df['eng_text'].str.len()
```

This code is using the `str.len()` method to count the number of characters in each row of the `eng_text` column. This is a simple way to get a sense of the length of each novel, but it is not a perfect measure of the number of words. To get a more accurate count, we could split the text into words and then count the number of words.

```python
combined_novels_nyt_df['novel_numb_words'] = combined_novels_nyt_df['eng_text'].str.split().apply(len)
```

This code is doing a few things. First, it is using the `str.split()` method to split the text on spaces. Then it is using the `apply()` method to apply the `len` function to each row of the DataFrame. This is a common pattern in Pandas, and it is a good way to apply functions to each row of a DataFrame.

We could also look at the frequency of terms. For example, we could count the number of times the words `they`, `she`, and `he` appear in each novel.

```python
combined_novels_nyt_df['they_counts'] = combined_novels_nyt_df['eng_text'].str.count('they')
combined_novels_nyt_df['she_counts'] = combined_novels_nyt_df['eng_text'].str.count('she')
combined_novels_nyt_df['he_counts'] = combined_novels_nyt_df['eng_text'].str.count('he')
```

Now we can plot the data.

```python
# Plot the data
combined_novels_nyt_df[['they_counts', 'she_counts', 'he_counts']].plot()
```

Which produces the following graph:

<figure>
    <a href="{{site.baseurl}}/assets/images/pronoun_counts.png">
    <img src="{{site.baseurl}}/assets/images/pronoun_counts.png" class="image-popup">
    </a>
</figure>

You'll notice that `he` is much more frequent than the other two pronouns. Now this might because of the novels we have in our dataset, but it could also be because of the way we are counting the words. To understand more let's dig into text analysis with Python more generally.

### Text Analysis with Python

So far we've just been working with Pandas, but in Python, there exists a few libraries specifically designed to work with text data.

- NLTK [https://www.nltk.org/](https://www.nltk.org/)

- SPACY [https://spacy.io/](https://spacy.io/)

- SCIKIT-LEARN [https://scikit-learn.org/stable/](https://scikit-learn.org/stable/)

Each of these libraries has its own history, and some of what they provide overlaps. Here's a helpful chart outlining some of their pros and cons.

<figure>
    <a href="https://activewizards.com/content/blog/Comparison_of_Python_NLP_libraries/nlp-librares-python-prs-and-cons01.png">
    <img src="https://activewizards.com/content/blog/Comparison_of_Python_NLP_libraries/nlp-librares-python-prs-and-cons01.png" class="image-popup">
    </a>
</figure>

Ultimately, which library you choose to use depends on what you want to do with your data, but there's some general principles for text analysis that you should consider regardless of method.

#### Word Counts and Zipf's Law

Today we are going to start with the basics of text analysis: word counts and one of the older libraries, NLTK.

<figure>
    <a href="{{site.baseurl}}/assets/images/nltk_github_history.png">
    <img src="{{site.baseurl}}/assets/images/nltk_github_history.png" class="image-popup">
    </a>
    <figcaption> NLTK's GitHub History <a href="https://github.com/nltk/nltk">https://github.com/nltk/nltk</a></figcaption>
</figure>

NLTK was first created in the late 1990s by Steven Bird and Edward Loper at the University of Pennsylvania. It was initially created to further research in corpus linguistics and natural language processing, but has since become a popular tool for text analysis in the humanities. In many ways, it comes out of a similar moment as the history of information retrieval and search engines, and has a similar goal of making text analysis more accessible.


The library NLTK has a helpful built in Class called `FreqDist` that takes a list of words and outputs their frequency in a corpus [http://www.nltk.org/api/nltk.html?highlight=freqdist](http://www.nltk.org/api/nltk.html?highlight=freqdist)

Let's try it out with a subset of our data. (Remember to install `nltk`).

```python
from nltk import word_tokenize
from nltk import FreqDist

tokens = FreqDist(sum(combined_novels_nyt_df[0:2]['pg_eng_text'].map(word_tokenize), []))
tokens.plot(30)
```

We should get a graph that looks like this:

<figure>
    <a href="{{site.baseurl}}/assets/images/freq_dist_counts.png">
    <img src="{{site.baseurl}}/assets/images/freq_dist_counts.png" class="image-popup">
    </a>
</figure>

In this graph, if we had used all the words we would see this trend continue, like in the graph below.

<figure>
    <a href="https://miro.medium.com/max/6072/1*GTpckiHyFLe04pUMeYDYOg.png">
    <img src="https://miro.medium.com/max/6072/1*GTpckiHyFLe04pUMeYDYOg.png" class="image-popup">
    </a>
</figure>

This is **Zipf’s law** a phenomenon where the most common word is twice as common as the second most common word, three times as common as the third most common word, four times as common as the fourth most common word, and so forth.

It is named after the linguist George Zipf, who first found the phenomenon while laboriously counting occurrences of individual words in Joyce’s Ulysses in 1935 (one of the earliest computing in the humanities projects!).

This is a core textual phenomenon, and one you must constantly keep in mind: common words are very common indeed, and logarithmic scales are more often appropriate for plotting than linear ones. This pattern results from many dynamic systems where the “rich get richer,” which characterizes all sorts of systems in the humanities. You can read more about the power of counting words here [https://tedunderwood.com/2013/02/20/wordcounts-are-amazing/](https://tedunderwood.com/2013/02/20/wordcounts-are-amazing/).

One of the key things to takeaway from understanding the distribution of words in a text is that this can help us answer different types of questions in the humanities. For example, if we use high frequency words we can often identify the authorship or style of a text, and if we use low frequency words we can often identify the topic or genre of a text.

While we will discuss more of this topic and genre identification of texts, we won't be covering authorship attribution so if you want to learn more I would highly recommend taking a look at François Dominic Laramée, "Introduction to stylometry with Python," *Programming Historian* 7 (2018), [https://doi.org/10.46430/phen0078](https://doi.org/10.46430/phen0078). Stylometry is the formal name for this type of analysis and it is a both a popular method for working with cultural data and one with a longer history. For example, in the 1960s the authorship of the Federalist Papers was determined using stylometry.

#### Tokenization

To start getting into text analysis, we need to start thinking about how we can break up our text into words. In the field of corpus linguistics, the term “word” is generally dispensed with as too abstract in favor of the idea of a “token” or “type.” Breaking a piece of text into words is thus called “tokenization.”

There are, in fact, at least 7 different choices you can make in a typical tokenization process.

- Should words be lowercased?
- Should punctuation be removed?
- Should numbers be replaced by some placeholder?
- Should words be stemmed (also called lemmatization).
- Should bigrams or other multi-word phrase be used instead of or in addition to single word phrases?
- Should stop-words (the most common words) be removed?
- Should rare words be removed?

Any of these can be combined: there at least a hundred common ways to tokenize even the simplest dataset.

We can try these out with our dataset. For example, would we get a different number of counts if we tokenized our pronouns?

```python
def tokenize_text(text):
	if pd.notna(text):
		return word_tokenize(text)
	else:
		return []

combined_novels_nyt_df['tokenized_text'] = combined_novels_nyt_df['eng_text'].apply(tokenize_text)
```

Now we can do the same pronoun analysis as before, but with our tokenized text.

```python
from collections import Counter


combined_novels_nyt_df['they_counts'] = combined_novels_nyt_df['pg_eng_tokens'].apply(lambda tokens: Counter(tokens)['they'])
combined_novels_nyt_df['she_counts'] = combined_novels_nyt_df['pg_eng_tokens'].apply(lambda tokens: Counter(tokens)['she'])
combined_novels_nyt_df['he_counts'] = combined_novels_nyt_df['pg_eng_tokens'].apply(lambda tokens: Counter(tokens)['he'])

# Plot the counts
combined_novels_nyt_df[['they_counts', 'she_counts', 'he_counts']].plot()
```

This will give us a similar graph to before, but now we are counting words instead of just string matching, which is more accurate since the letters `he` could be part of another word (in this case `they` and `she` even!).

<figure>
	<a href="{{site.baseurl}}/assets/images/tokenized_pronouns.png">
	<img src="{{site.baseurl}}/assets/images/tokenized_pronouns.png" class="image-popup">
	</a>
</figure>

This graph is a bit hard to read, but you can start to see that `he` is no closer to the range of the other pronouns. We could also use Altair to inspect a bit closer. However, to replicate the `plot` graph, requires **reshaping** our data and specifically something called a **melt** operation.

```python
melted_df = pd.melt(combined_novels_nyt_df, id_vars=['title', 'author', 'pub_year', 'genre'], value_vars=['they_counts', 'she_counts', 'he_counts'], var_name='pronoun', value_name='pronoun_counts')

alt.Chart(melted_df).mark_line().encode(
	x='title:N',
	y='pronoun_counts:Q',
	color='pronoun:N',
	tooltip=['title', 'author', 'pub_year', 'genre', 'pronoun_counts']
)
```

In this code we are using the `melt()` method to  so that the `they_counts`, `she_counts`, and `he_counts` columns are now in two columns: one for the pronoun and one for the counts. This is a common operation in data analysis, and it is often used to make it easier to plot data. You can see an example of what the melt operation does in the figure below:

<figure>
	<a href="https://pandas.pydata.org/pandas-docs/version/0.25.1/_images/reshaping_melt.png">
	<img src="https://pandas.pydata.org/pandas-docs/version/0.25.1/_images/reshaping_melt.png" class="image-popup">
	</a>
</figure>

Melting in Pandas is a way to reshape your data from wide to long, since sometimes you want to look at the relationship between multiple columns. In this case, we are melting the `they_counts`, `she_counts`, and `he_counts` columns into with the name of the column in the `var_name` column and the value of the column in the `value_name` column, respectively (in this case `pronoun` and `pronoun_counts`). You can read more about melting here here [https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-by-melt](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-by-melt). 

Now our current graph is a bit detailed, but we could group by genre and then plot the pronoun counts to see if there are any differences between genres.

```python
alt.Chart(melted_df[melted_df.genre != "na"]).mark_bar().encode(
	x='genre:N',
	y='sum(count):Q',
	color='pronoun:N',
	tooltip=['title', 'author', 'pub_year', 'genre', 'count']
)
```

And now we should see the follow chart:

<figure>
	<a href="{{site.baseurl}}/assets/images/genre_pronoun_chart.png">
	<img src="{{site.baseurl}}/assets/images/genre_pronoun_chart.png" class="image-popup">
	</a>
</figure>

Next if we were doing EDA we might try to normalize these counts by both the number of words in a novel and the number of novels in a genre. This would give us a better sense of the relative frequency of these pronouns in each genre. But let's move on to some more advanced text analysis techniques.

#### Lemmatizing/Stemming

Lemmatizing and Stemming are both ways of reducing words to their root form to make them more normalized for analysis. Lemmatizing is the process of reducing words to their base form, while stemming is the process of reducing words to their stem, demonstrated in the figure below:

<figure>
    <a href="https://devopedia.org/images/article/227/6785.1570815200.png">
    <img src="https://devopedia.org/images/article/227/6785.1570815200.png" class="image-popup">
    </a>
</figure>

The NLTK library we used for FreqDist also comes with stemmers and lemmatizers. 

```python
import nltk
from nltk.stem import PorterStemmer
porter = PorterStemmer()
```

Now to use these we need to apply them to each of the rows in our DataFrame. We could do this with a for loop, but it's more efficient to use the `apply()` method. 

```python
def stem_words(row):
	stemmed_words = ''
	for token in row.split(' '):
		stemmed_words += porter.stem(token) + ' '
	return stemmed_words

test_df = combined_novels_nyt_df[0:10]
test_df['pg_eng_text'] = test_df['pg_eng_text'].fillna('')
test_df['stemmed_text'] = test_df['pg_eng_text'].apply(stem_words)
```

You'll notice I'm only doing this for the first 10 rows of the DataFrame, since this can be a time consuming process. 

Now we can inspect the stemmed words. We can see what the first email looks like when it is stemmed:

```python
print(test_df['stemmed_text'][0][0:500])
```

This should give us the following output:
```shell
the project gutenberg ebook of don quixote
    
thi ebook is for the use of anyon anywher in the unit state and
most other part of the world at no cost and with almost no restrictions
whatsoever. you may copi it, give it away or re-us it under the terms
of the project gutenberg licens includ with thi ebook or online
at www.gutenberg.org. if you are not locat in the unit states,
y will have to check the law of the countri where you are located
befor use thi ebook.

title: don quixote
```

And compare it to the original:

```shell
The Project Gutenberg eBook of Don Quixote
    
This ebook is for the use of anyone anywhere in the United States and
most other parts of the world at no cost and with almost no restrictions
whatsoever. You may copy it, give it away or re-use it under the terms
of the Project Gutenberg License included with this ebook or online
at www.gutenberg.org. If you are not located in the United States,
you will have to check the laws of the country where you are located
before using this eBook.
```

We notice that stemming lowercased the words, but it did not remove the punctuation. It also did not remove the stop words. We can tell it stemmed the words because `united` became `unite`, and `states` became `state`. If we had lemmatized instead of stemmed, we would have gotten the same result, since lemmatizing is a more complex process that requires a dictionary of words to reduce them to their base form. Eventually though we might have seen examples like `are` becoming `be` and `located` becoming `locate`.

### Advanced Text Analysis Exploration

Now that you are starting to learn the basics of text analysis, you can start to explore more advanced techniques that you might want to use in your final project. Today I will be sharing code to help you get started with some information extraction using Named Entity Recognition and information retrieval using TF-IDF. Your goal is to get the code running and to see if you can start to understand how these techniques can be used to analyze text data.

Before you start though, one issue we haven't addressed is the fact that each Project Gutenberg book has a header and footer that we might want to remove before we start our analysis. This is a common issue with text data, and it is often called "noise" in the data. We could use regular expressions to remove this data, but there's also existing Python libraries for working with Project Gutenberg data. In particular, I've used the `gutenbergpy` library in the to clean our novels. You can find information about the library here [https://github.com/raduangelescu/gutenbergpy](https://github.com/raduangelescu/gutenbergpy) and install it with the following command:

```bash
pip install gutenbergpy
```

In particular, this library has a set of text markers that it uses to remove the headers and footers that you can see here [https://github.com/raduangelescu/gutenbergpy/blob/master/gutenbergpy/textget.py#L23](https://github.com/raduangelescu/gutenbergpy/blob/master/gutenbergpy/textget.py#L23) and in this figure:

<figure>
	<a href="{{site.baseurl}}/assets/images/gutenbergpy_markers.png">
	<img src="{{site.baseurl}}/assets/images/gutenbergpy_markers.png" class="image-popup">
	</a>
</figure>

To use the library, you can use the following code:

```python
import gutenbergpy.textget

def clean_book(row):
	# This gets a book by its gutenberg id number
	if pd.notna(row['pg_eng_url']):
		pg_id = row['pg_eng_url'].split('/pg')[-1].split('.')[0]
		try:
			raw_book = gutenbergpy.textget.get_text_by_id(pg_id) # with headers
			clean_book = gutenbergpy.textget.strip_headers(raw_book) # without headers
			return clean_book
		except Exception as e:
			print(f"Error getting book: {e} with id: {pg_id} and url: {row['pg_eng_url']}")
			return None

combined_novels_nyt_df['cleaned_pg_eng_text'] = combined_novels_nyt_df.progress_apply(clean_book, axis=1)
```

This code will get the text of the book by its Project Gutenberg ID, which we are deriving from the Gutenberg URL, and then remove the headers and footers. Such a step is necessary since otherwise the headers and footers would be included in our text analysis, which would skew our results.

You can download the notebook with the full code for this lesson <a href="{{site.baseurl}}/assets/files/is310IntroTextAnalysis.ipynb" download>here</a>.

#### Named Entity Recognition (NER)

What is Named Entity Recognition?

From the [Wikipedia article](https://en.wikipedia.org/wiki/Named-entity_recognition):

> “Named-entity recognition (NER) (also known as (named) entity identification, entity chunking, and entity extraction) is a subtask of information extraction that seeks to locate and classify named entities mentioned in unstructured text into pre-defined categories such as person names, organizations, locations, medical codes, time expressions, quantities, monetary values, percentages, etc.”

What does this look like in practice? Let's look at a few examples.

<figure>
	<a href="https://miro.medium.com/max/1400/0*zDbB-LV-Dlm_F_PX">
	<img src="https://miro.medium.com/max/1400/0*zDbB-LV-Dlm_F_PX" class="image-popup">
	</a>
</figure>

<figure>
	<a href="https://miro.medium.com/max/1400/1*w4449DJGuXlay02gFt8APg.png">
	<img src="https://miro.medium.com/max/1400/1*w4449DJGuXlay02gFt8APg.png" class="image-popup">
	</a>
</figure>

We can see that in these images, parts of the text are being highlighted and classified as different categories (or **entities** if you will). Many of these examples might seem straightforward, but how do we decide what is an entity exactly? And how did NER become a defined field instead of just being part of text mining?

<figure>
	<a href="https://miro.medium.com/max/1400/1*Uf_qQ0zF8G8y9zUhndA08w.png">
	<img src="https://miro.medium.com/max/1400/1*Uf_qQ0zF8G8y9zUhndA08w.png" class="image-popup">
	</a>
</figure>

This graph represents a common typology of NLP tasks, comparing it to Natural Language Understanding and Automatic Speech Recognition.

While technically correct, I find this graph confusing 🤔. The exact definition of NER’s boundaries relative to other nlp/nlu tasks is very porous. For example, in addition to recognizing entities, NER can also include parts of text classification, entity disambiguation, relationship extraction all parts of NER (though they are separated in this chart).

*So if this is so porous how did NER first develop?*

<figure>
	<a href="https://en.wikipedia.org/wiki/Information_extraction#History">
	<img src="{{site.baseurl}}/assets/images/ner_history.png" class="image-popup">
	</a>
</figure>

From this Wiki page, we can see that the concept of “named entity” first developed at the Message Understanding Conference-6 in 1995. Here’s an example template given to participants from MUC-3 and you can even read the original MUC manual here <https://www-nlpir.nist.gov/related_projects/muc/muc_sw/muc_sw_manual.html>:

<figure>
	<a href="{{site.baseurl}}/assets/images/muc_example.png">
	<img src="{{site.baseurl}}/assets/images/muc_example.png" class="image-popup">
	</a>
</figure>

The big takeaway is that NER is relatively new. The field really takes off as a field with Text Analysis Conferences beginning in 2009 <https://tac.nist.gov/> and it is also relatively recent in digital humanities.

The first DH conference presentation listing NER as a topic was in 2004:

<figure>
	<a href="https://dh-abstracts.library.cmu.edu/works?ordering=year&text=%22named+entity+recognition%22&conference=&work_type=">
	<img src="{{site.baseurl}}/assets/images/dh_ner_conference.png" class="image-popup">
	</a>
</figure>



#### TF-IDF & Stop Words	

So far we have been focused on counting and cleaning our textual data, but we could also try some more complex Information Retrieval techniques. One popular approach is an algorithm called `Term Frequency - Inverse Document Frequency` or TF-IDF. TF-IDF was first proposed in a 1972 paper by Karen Spärck Jones under the name “term specificity” and was later named TF-IDF by Steve Robertson in 1976. The algorithm is used to determine the importance of a word in a document relative to a collection of documents. The algorithm is based on the idea that a word is important if it appears frequently in a document, but infrequently in the rest of the documents in the collection.

<div class="notice--info">⚡️ Great resources for learning more about TF-IDF are: <a href="https://programminghistorian.org/en/lessons/analyzing-documents-with-tfidf"> Matthew J. Lavin, "Analyzing Documents with TF-IDF," <i>Programming Historian</i> 8 (2019)</a> and <a href="https://melaniewalsh.github.io/Intro-Cultural-Analytics/05-Text-Analysis/01-TF-IDF.html" >Melanie Walsh's Textbook</a></div>

The reason TF-IDF is so popular is because it is intended to surface terms that are distinctive across as set of documents (or a **corpus**). Often times with word counting you get lots of high frequency words (remember Zipf's law) but these words aren't always helpful if you are trying to understand the concepts within the text. High frequency words are more useful for author attribution.

So the way TF-IDF works is the following operations as visualized in this diagram:

<figure>
    <a href="https://miro.medium.com/max/1200/1*qQgnyPLDIkUmeZKN2_ZWbQ.png">
    <img src="https://miro.medium.com/max/1200/1*qQgnyPLDIkUmeZKN2_ZWbQ.png" class="image-popup">
    </a>
</figure>

To break this down into code it really looks like the following:

```python
term_frequency = number of times a given term appears in document

inverse_document_frequency = log(total number of documents / number of documents with term) + 1*****

tf-idf = term_frequency * inverse_document_frequency
```