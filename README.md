# Earthquakes and Premonitory tremors

I live in a seismic zone, in the south of Peru (Arequipa), here it is very common to have tremors, and when there are more tremors than "normal", older people say that a big earthquake is coming. I have researched about foreshocks as a warning of a large earthquake, but studies indicate that although some large earthquakes in history had some foreshocks, no evidence has been found that from foreshocks it is possible to predict the occurrence of a large earthquake, nor the magnitude of the earthquake.
As part of my training as a data scientist, I decided to do a small project that would allow me to do more research on this interesting topic, practice some of the skills I have learned, and learn some new ones.

## Data
The dataset used for this project is "All the Earthquakes Dataset : from 1990-2023" located in Kaggle. The Kaggle page of the dataset is: https://www.kaggle.com/datasets/alessandrolobello/the-ultimate-earthquake-dataset-from-1990-2023/data

## Methods and Results
I started my work in this jupyter notebook: https://github.com/DiegoMZD/Earthquakes/blob/main/jupyter_notebooks/Earthquakes.ipynb
First it is neccesary to import Numpy (Python library for numerical computing) and Pandas (Python library for data manipulation and analysis). Then I load the data, that is also in the folder data, with the name "Earthquakes-1990-2023.csv": https://github.com/DiegoMZD/Earthquakes/blob/main/data/Eartquakes-1990-2023.csv

![The data is loaded as we can see the first five rows](https://github.com/DiegoMZD/Earthquakes/blob/main/images/1-load.jpg)

The data is a dataset with more than 3 millons rows, in terms of time of processing it takes a lot of time to apply the filters and functions that are showed later, so it is neccesary to sample the dataset. Since it is sorted by date, I keep the last 250 000 earthquakes. As you can see, I select only the "data_type" earthquake, because I was looking for premonitory tremors.

![Data sampled](https://github.com/DiegoMZD/Earthquakes/blob/main/images/2-sampling.jpg)

The date column datatype is pandas object (string), to apply time filters it is neccesary to convert it to timestamp. To do that I import the module "parser" from Dateutil (Python library that allows easy manipulation of dates). Then define a function "to_timestamp" that takes the date strings as arguments and "parse" them. Apply the funtion to all the elements in the date column using the map function, and finally replace the data in the date column with the created timestamps.

![to-timestamp](https://github.com/DiegoMZD/Earthquakes/blob/main/images/3-timestamp.jpg)

I drop columns that I am not interested in. Then I define the earthquakes (over 6 grades on the Richter scale) and simple tremors (under 6 grades on the Richter scale).

![drop and earthquakes](https://github.com/DiegoMZD/Earthquakes/blob/main/images/4-earthquakeslist.jpg)
![tremors list](https://github.com/DiegoMZD/Earthquakes/blob/main/images/5-tremorslist.jpg)

For this project, the definition of a premonitory tremor is: "an earthquake measuring under 6 grades on the Ritchter scale that took place within a radius around the epicenter of the earthquake and it manifests within a defined period of time before the earthquake"

To extract the premonitory tremors of any earthquake I defined the premonitory_tremors function. This function takes the following arguments:
* earthquake: a row from the earthquakes_list dataframe
* tremors list: the tremors_list dataframe defined before
* radius_km: the distance (in km) around the epicenter within which the effects of the premonitory tremor are observed
* period_days_list: a list with some periods of time before the earthquake
I define the calculate_distance function to calculate the distance between two points (earthquake and potential premonitory tremor locations), that takes the following arguments:
* center: tuple containing the latitude and longitude of the earthquake
* point: tuple containing the latitude and longitude of a potential premonitory tremor

The premonitory tremors function return the premonitory_tremors_list of an earthquake. To do that it filters the tremors_list by location (around the earthquakes's epicenter) and by time (days before the eartquake occurance). To filter by location I import the "geodesic" module from the geopy.distance library. To filter by time I need to substract days to a timestamp so I import the "timedelta" module from the datetime library.

![premonitory_tremors](https://github.com/DiegoMZD/Earthquakes/blob/main/images/6-premonitoryt.jpg)

In this project, the premonitory features are indicators based on the premonitory tremors list of any earthquake. To get the premonitory features I defined the premonitory_features function, it takes the following arguments:
* earthquakes_list: the dataframe that contains the earthquakes
* tremors_list: the same argument as the premonitory_tremors list
* radius_km: the same argument as the premonitory_tremors list
* period_days_list: the same argument as the premonitory_tremors list

A new dataframe "earthquakes_and_premonitory" is defined, as a copy of the earthquakes_list without the columns "tsunami" and "data_type", because these are not relevant now. The function starts with a for loop that iterates in the rows of the dataframe "earthquakes_and_premonitory". For every row (earthquake) I execute the premonitory_tremors_list function, to get its premonitory_tremors_list. Then I had defined 3 lists that contained the indicators based on the premonitory tremors list. Then inside the for loop, there is another for loop that iterates in the period_days_list, to get the indicators for every day listed in that list. As indicators, I got the number of premonitory tremors for day, the average magnitude, and the average significance, for every earthquake. The function is the following:

![premonitory_features](https://github.com/DiegoMZD/Earthquakes/blob/main/images/7-premonitoryf.jpg)

Then it is time to execute the function. I get the premonitory features for a list of days between 1 (day before the earthquake) and 14 (day located 2 weeks before the earthquake). Also the radius_km was set on 300 km around the epicenter of the earthquake. I wrote a condition to drop earthquakes that couldn't have a complete set of features. And then I save the results as a csv file, also located in the data folder: https://github.com/DiegoMZD/Earthquakes/blob/main/data/earthquakes_and_premonitory.csv

![get_premonitory_features](https://github.com/DiegoMZD/Earthquakes/blob/main/images/8-getpremonitoryf.jpg)
![show_newdf](https://github.com/DiegoMZD/Earthquakes/blob/main/images/9-shownewdf.jpg)

The next part of this work is in this jupyter notebook: https://github.com/DiegoMZD/Earthquakes/blob/main/jupyter_notebooks/Premonitory.ipynb
I started this new jupyter notebook importing numpy and pandas, and loading the data.

![load](https://github.com/DiegoMZD/Earthquakes/blob/main/images/10-load2.jpg)

Then I copied the data, renamed the first column as ID and filled any "NaN" value with "0"

![premfull](https://github.com/DiegoMZD/Earthquakes/blob/main/images/11-premfull.jpg)

The first visualization that I plotted is a bar plot for the average number of premonitory tremors by day. I used the matplotlib.pyplot package (the most important Python library for data visualization). It looked like the day before the earthquake there were more tremors than other days, but this needed to be tested.

![barplot](https://github.com/DiegoMZD/Earthquakes/blob/main/images/12-barplot.jpg)

Then I used a unsupervised machine learning algorithm called kMeans clustering. I import the KMeans module from the sklearn.cluster library. The objetive was to engineer a new feature "location clusters for the earthquakes". I also used the plt.imgread() funtion to put a map below the clusters plot, to made the clusters more visible. After that, I added the clusters feature to the "premonitory_full" dataframe.

![kmeans](https://github.com/DiegoMZD/Earthquakes/blob/main/images/13-kmeans.jpg)
![map](https://github.com/DiegoMZD/Earthquakes/blob/main/images/14-map.jpg)

![addclusters](https://github.com/DiegoMZD/Earthquakes/blob/main/images/15-addclusters.jpg)

I tried to find another interesting features or relations, so I plot a heatmap of a correlation matrix. I used the corr() function to get the correlation matrix, and the sns.heatmap() funtion to plot the corr heatmap. In the heatmap I saw a zone where the squares are more red than other parts in the matrix, this zone corresponded to the relation between the features created "n_tremors".

![corr](https://github.com/DiegoMZD/Earthquakes/blob/main/images/16-corr.jpg)
![heatmap](https://github.com/DiegoMZD/Earthquakes/blob/main/images/17-heatmap.jpg)

With that insight in mind, I decided to investigate more on this features. To did that I used the sns.boxplot() function to plot a comparison of the boxplots of all days (1-14). It looked like all days are similar, except day 7, but there is not much more I could tell about this graphic.

![boxplots](https://github.com/DiegoMZD/Earthquakes/blob/main/images/18-boxplots.jpg)

I also tried with the describe method to investigate more about this features. And as the barplot showed in the previus steps, the day one showed a higher mean. So, are there more tremors a day before an earthquake than any other previus day?

![describe1](https://github.com/DiegoMZD/Earthquakes/blob/main/images/19-describe1.jpg)
![describe2](https://github.com/DiegoMZD/Earthquakes/blob/main/images/20-describe2.jpg)



## Conclusion

## Dependencies
To install the dependencies, run the following command:

```bash
pip install -r requirements.txt
```
## Licence
This project is distributed under the [Apache License 2.0](https://github.com/DiegoMZD/Earthquakes/blob/main/LICENSE.txt)


[def]: url
