# Disclaimer
# ElasticSearch examples
I've lined up a bunch of examples to showcase the features and the sheer power of [ElasticSearch](https://www.elastic.co/products/elasticsearch). A lot of the information is based on ["ElasticSearch, The Definitive Guide"](https://www.elastic.co/guide/en/elasticsearch/guide/current/index.html).

## Installing
Download ElasticSearch & Kibana [here](https://www.elastic.co/downloads), then follow these simple steps:

* Install both ElasticSearch & Kibana.
* Run ElasticSearch `./bin/elasticsearch`
* Run Kibana `./bin/kibana`
* Use the Kibana console by accessing [http://localhost:5601/app/kibana#/dev_tools/console](http://localhost:5601/app/kibana#/dev_tools/console)

## Exercise 1: the basics
Exercise 1 is very simple and the goal is to get the hang of the ElasticSearch RESTFul interface.

Topics:

* Navigating to the ElasticSearch landing page
* Searching all documents
* Counting documents
* Adding documents to the index
* Full document updates
* Partial document updates
* Retrieve individual documents
* Searching all documents for a specific index

[Load exercise 1](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/1_basics.json)

## Exercise 2: load data in bulk
In exercise 2 we will be indexing a lot of data. To improve the performance, we're doing this in bulk.

This data contains information from the [Combell blog](http://blog.combell.com). I've indexed the following information:

* Title
* Author
* Date
* Categories
* Language
* GUID

This data will be used in the other exercises.

[Load the blog data in bulk](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/2_blog_data_bulk.json)

## Exercise 3: search, getting to know the query DSL
In exercise 3 we're performing some basic queries using the ElasticSearch query DSL. The DSL is JSON-based and the queries are full-text searches.

Here's a couple of searches we're performing:

* Search for a single term in an index
* Search for multiple terms in an index
* Perform searches on multiple terms using the *"and"* operator
* Define the minimum number of matches a document should have
* Define the proximity of terms you're searching

[Load exercise 3](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/3_search.json)


## Exercise 4: analysis
In exercise 4, we're going to focus on the analysis of full-text and human language. We'll ignore the database capabilities of ElasticSearch and throw some text at it, and see how it tokenizes the data.

Depending on the analyzer you use, ElasticSearch will tokenize and store the data in a different way. Don't worry, the original data will remain in the source of the document, it's the inverted index that changes.

[Load exercise 4](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/4_analysis.json)


## Exercise 5: schemaless? Not really.
Exercise 5 is all about the schema of an index. ElasticSearch is marketed as being schemaless. In reality, ElasticSearch will guess the schema for you. 

I'll show you examples where it guesses successfully and examples where it doesn't.

[Load exercise 5](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/5_schemaless.json)


## Exercise 6: mapping
To avoid that ElasticSearch guesses the schema wrong, explicit mapping is a good idea. Exercise 6 will set up the right mapping for our blog example and re-insert the data. 

Integers and strings will be defined accordingly and the date will have the right format.

The explicit mapping will be used in exercise 7.

[Load exercise 6](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/6_mapping.json)


## Exercise 7: search using explicit mapping
The 2 searches in exercise 5 that failed will now be executed again. Thanks to explicit mapping, the output will be correct.

* Query 1 won't return anything, because the range doesn't match
* Queries 2 & 3 will return the documents that fit the data range

[Load exercise 7](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/7_with_schema.json)


## Exercise 8: non-analyzed fields
In exercise 8, we will define yet another mapping on our blog index. This mapping only treats the *"title"* field as full-text. The rest of the strings will not be analyzed and tokenized. They will be stored *"as is"*.

This data will be used in exercise 9.

[Load exercise 8](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/8_mapping_non_analyzed.json)


## Exercise 9: filters, full-text vs. exact values
In exercise 9, I'll show you the difference between full-text searches using queries and exact value matches using queries in filter mode.

The mapping that was done in exercise 8 has made sure there is now a *"keyword"* field on the title property. This means that queries on *"title"* are treated as full-text searches and boolean filters on the regular *"title.keyword"* field are treated as exact value matches.

In one of the examples, I'll also show you how to combine multiple queries and filters.

This is what we'll do in this exercise:

* Use a prefix query in filter context to perform a wildcard search, even if the fields are not analyzed
* Do a standard query using the *"keyword"* field
* Use a boolean query in filter mode to combine multiple filters based on the *"and"*, *"or"* & *"not"* operators
* Use a regular boolean query and notice how the behaviour of the *(should)* clause changes

[Load exercise 9](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/9_queries_and_filters.json)


## Exercise 10: language-based mapping
We will **again** remap the data. This time, we will treat the *"title"* property as an analyzed field. By default the *"standard"* analyzer is used. Because our data is both in Dutch and English, I added 2 fields:

* The *"en"* explicitly uses the English analyzer
* The *"nl"* explicitly uses the Dutch analyzer

This is the final version of the mapping. The other examples will use this mapping and data.

[Load exercise 10](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/10_mapping_language_analyzed.json)


## Exercise 11: using languages
Exercise 11 is all about the analysis of text, based on the language. Exercise 4 was a hint towards the analysis of data. Now we'll actually perform searches that depend on language analysis.

* Query 1 will look for the term *"work"* on the *"title"* property
* Query 2 will look for the term *"work"* on the *"title.en"* field (which uses the English analyzer)
* Query 3 will look for the term *"werk"* on the *"title"* property
* Query 4 will look for the term *"werk"* on the *"title.nl"* field (which uses the Dutch analyzer)

[Load exercise 11](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/11_languages.json)


## Exercise 12: geo data

In exercise 12, we'll create a new *"cities"* index, that contains all the cities that are located in the *West-Vlaanderen* province of Belgium. The index stores the name of the city and its geo coordinates.

The explicit mapping and the data will be used in other exercises.

[Load exercise 12](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/12_geo_data.json)


## Exercise 13: geo searches
In the previous exercise, we created a new index and indexed some geo data. In exercise 13, we'll actually perform searches on this data.

2 queries will be showcased:

* A query that displays all cities within 5km of Diksmuide
* A query that displays all cities that are located in a specific bounding box (between Koksijde & Nieuwpoort)

[Load exercise 13](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/13_geo_search.json)


## Excercise 14: aggregation data
In exercise 14, we'll load data into yet another index. This index is called *"cars"* and it contains car sales information. Every transaction keeps track of the following information:

* The price of the sale
* The make of the car that was sold
* The color of the car
* The data of the sale

This information will be used in exercise 15.

[Load exercise 14](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/14_aggregation_data.json)


## Exercise 15: performing aggregations
Aggregations are a very powerful feature of ElasticSearch. It's basically like *"group by"* in SQL, but way more powerful. Aggregations are the reason why ElasticSearch is popular in the big data and data science community. 

These are the aggregations we'll execute in this exercise:

* Get the top 10 most popular authors of the Combell blog
* Get the top 10 most popular authors of the Combell blog and display how many posts they wrote in each language
* Get all the blog posts written in Dutch, that were published in 2016. Use aggregations to see the amount per month
* Get the top 3 most popular cars
* Get the average price of a sold car
* Get extended statistics on the price of a sold car
* Get the total revenue for cars per price range, with an interval of 20000 USD
* Calculate the average price of a Ford, versus the total average price of all the cars that were sold

[Load exercise 15](http://localhost:5601/app/kibana#/dev_tools/console?load_from=https://raw.githubusercontent.com/ravisgf/es-handson/master/data/15_aggregations.json)
