# Earthquakes and Premonitory tremors

I live in a seismic zone, in the south of Peru (Arequipa), here it is very common to have tremors, and when there are more tremors than "normal", older people say that a big earthquake is coming. I have researched about foreshocks as a warning of a large earthquake, but studies indicate that although some large earthquakes in history had some foreshocks, no evidence has been found that from foreshocks it is possible to predict the occurrence of a large earthquake, nor the magnitude of the earthquake.
As part of my training as a data scientist, I decided to do a small project that would allow me to do more research on this interesting topic, practice some of the skills I have learned, and learn some new ones.

## Data
The dataset used for this project is "All the Earthquakes Dataset : from 1990-2023" located in Kaggle. The Kaggle page of the dataset is: https://www.kaggle.com/datasets/alessandrolobello/the-ultimate-earthquake-dataset-from-1990-2023/data

## Results and Summary
First it is neccesary to import Numpy (Python library for numerical computing) and Pandas (Python library for data manipulation and analysis). Then I load the data, that is also in the folder data, with the name "Earthquakes-1990-2023.csv": https://github.com/DiegoMZD/Earthquakes/blob/main/data/Eartquakes-1990-2023.csv

![The data is loaded as we can see the first five rows](https://github.com/DiegoMZD/Earthquakes/blob/main/images/1-load.jpg)

The data is a dataset with more than 3 millons rows, in terms of time of processing it takes a lot of time to apply the filters and functions that are showed later, so it is neccesary to sample the dataset. Since it is sorted by date, I keep the last 250 000 earthquakes. As you can see, I select only the "data_type" earthquake, because I was looking for premonitory tremors.

![Data sampled](https://github.com/DiegoMZD/Earthquakes/blob/main/images/2-sampling.jpg)

The date column datatype is pandas object (string), to apply time filters it is neccesary to convert it to timestamp.To do that I import the module "parser" from Dateutil (Python library that allows easy manipulation of dates). Then define a function "to_timestamp" that takes the date strings as arguments and "parse" them. Apply the funtion usin the map function, and finally replace the data in the date column with the created timestamps.

![to-timestamp] (https://github.com/DiegoMZD/Earthquakes/blob/main/images/3-timestamp.jpg)



## Conclusion

## Dependencies
To install the dependencies, run the following command:

```bash
pip install -r requirements.txt
```
## Licence
This project is distributed under the [Apache License 2.0](https://github.com/DiegoMZD/Earthquakes/blob/main/LICENSE.txt)


[def]: url
