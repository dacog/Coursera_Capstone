# Clustering cities with more than 100.000 inhabitands based their on infrastructure (Foursquare venues)
**Capstone Project - The Battle of Neighborhoods**


Diego Carrasco Gubernatis

Februery 21, 2019

# 1. Introduction
## 1.1 Background
Families moving to a new country face many hardships, from undestanding the culture, learning the languae, finding a job and finding a place to live. To ease the migration they would like to find a city similar (or dissimilar) to the one they were living in their home country, with a similar or better infrastructure and quality of life. It would be advantageous to have a comparison between the most important cities in their home country and they destiny, to find out which are similar.


## 1.2 Description of the problem

With so many alternatives and so many variables is easy to get lost and with so much information it's easy to misunderstand what the data is telling you. It would be great if there was a comparison of the different cities in a country of choice which gives you a general view of each place and it's pro's and contra's.

In this case the problem will be moving from a Southamerican country (Chile) to a European country (Germany), and the family would like to have a priority list of the places which better satisfy their requirements.

The requirements are:  
* schools
* hostpitals
* security
* shops
* entertainment

## 1.3 Interest

The finding of such an analysis would be of interest for families moving between Chile and Germany, and also for companies expoanding their businesses.
 
# 2. Data Acquisition and cleaning
## 2.1 Data sources
A list of the cities and they coordinates were found in Wikidata trough a query for data on cities with more than 100.000 population, with labels and coordinates, which can be found [here](http://tinyurl.com/yxkqyzu2)

The venues for each place were obtained using the Foursquare API for each city in both countries. This data was also saved as a csv file which can be found [here](https://raw.githubusercontent.com/dacog/Coursera_Capstone/master/resources/venues_decl.csv)

To get the venues I created a new python package with a function which uses the foursqueare package, then cleans the data and returns a dataframe. Tha package is called foursquare_api_tools and can be found on it's repository in GitHub [here](github.com/dacog/foursquare_api_tools)

## 2.2 Cleaning of the data
The data downloaded from Wikidata had be cleaned by splitting the coordinates column into Longitude and Latitude, dropping _na_ values and creating a new dataset with only the cities of Chile and Germnay. 

Then we obtained the venues for each city which is already cleaned by the function in the package I created, as it returns, in a dataframe, Name, City, Country, Latitude, Longitude, Category and Address from Foursquare. When obtaining the venues I added additional columns for country _(country2)_, city _(city2)_ and search query _(query)_ because I needed that information for the cluster analysis and foursquare returns empty data and differently-written names (they are written by users). Eith the 2 additional columns I was able to get a proper dataset for the analysis.

After cleaning the data I had 2 Countries, 84 Cities and 31589 venues using Foursquare:
* 2072 venues in Chile
* 29517 venues in Germany
* 346 uniques categories

# 3. Methodology

After creating the new dataset with cities form Chile and Germany I plotted the cities on a map using Folium to check if the coordinates were ok.

## 3.1 Venue categories for each city

After cleaning all the data and preparing the final datasets, I created a One Hot encoding for each category for each city base on the categories Forusquare returned and _city2_ column I added. I added this column because, for example, for Antofagasta (a city in Chile) there were 4 different way the users wrote it's name, and it was the same for each of the 84 cities. Adding the _cities2_ column allowed me to have the same city label for each venue.

After doing the One Hot encoding I had a dataset _cities_onehot_ with shape (31589, 347), which I grouped by City with the category mean.

Then I created a new dataframe _city\_venues\_sorted_ with the most commond venues for each city.

## 3.2 Cluster Analysis

I used K-Means Cluster Analysis with K=5 and Hierarchical Cluster Analysis (with 5 and 8 clusters) and got similar results, which were then plotted on a Folium Map with Colors for each cluster.

## 3.3 Comparing results

For K-Means Analysis I got the following results

* K-means for 0 has 58 cities
* K-means for 1 has 12 cities
* K-means for 2 has 3 cities
* K-means for 3 has 4 cities
* K-means for 4 has 3 cities

With Hierarchical Clustering I got, for 5 Clusters:

* Hierarchical for 0 has 5 cities
* Hierarchical for 1 has 9 cities
* Hierarchical for 2 has 4 cities
* Hierarchical for 3 has 4 cities
* Hierarchical for 4 has 58 cities

