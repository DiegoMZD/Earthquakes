# Earthquakes and Premonitory tremors

I live in a seismic zone, in the south of Peru (Arequipa), here it is very common to have tremors, and when there are more tremors than "normal", older people say that a big earthquake is coming. I have researched about foreshocks as a warning of a large earthquake, but studies indicate that although some large earthquakes in history had some foreshocks, no evidence has been found that from foreshocks it is possible to predict the occurrence of a large earthquake, nor the magnitude of the earthquake.
As part of my training as a data scientist, I decided to do a small project that would allow me to do more research on this interesting topic, practice some of the skills I have learned, and learn some new ones.

## Data
The dataset used for this project is "All the Earthquakes Dataset : from 1990-2023" located in Kaggle. The Kaggle page of the dataset is: https://www.kaggle.com/datasets/alessandrolobello/the-ultimate-earthquake-dataset-from-1990-2023/data

## Methods and Results
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
* earthquake: a row from the earthquakes_list
* tremors list: the tremors_list defined before
* radius_km: the distance (in km) around the epicenter within which the effects of the premonitory tremor are observed
* period_days_list: the period of time before the earthquake
I define the calculate_distance function to calculate the distance between two points (earthquake and potential premonitory tremor locations), that takes the following arguments:
* center: tuple containing the latitude and longitude of the earthquake
* point: tuple containing the latitude and longitude of a potential premonitory tremor

The premonitory tremors function return the premonitory_tremors_list of an earthquake. To do that it filters the tremors_list by location (around the earthquakes's epicenter) and by time (days before the eartquake occurance). To filter by location I import the "geodesic" module from the geopy.distance library. To filter by time I need to substract days to a timestamp so I import the "timedelta" module from the datetime library.

![premonitory_tremors](https://github.com/DiegoMZD/Earthquakes/blob/main/images/6-premonitoryt.jpg)


## Conclusion

## Dependencies
To install the dependencies, run the following command:

```bash
pip install -r requirements.txt
```
## Licence
This project is distributed under the [Apache License 2.0](https://github.com/DiegoMZD/Earthquakes/blob/main/LICENSE.txt)


[def]: url
