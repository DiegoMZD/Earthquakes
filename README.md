# Earthquakes and Premonitory tremors
I reside in a seismic region, located in the southern part of Peru, specifically in Arequipa, where tremors are a frequent occurrence. Locals often associate an increase in tremors with the impending possibility of a major earthquake. In my quest for understanding, I delved into research on foreshocks as potential precursors to significant seismic events. However, while historical data shows that certain major earthquakes were preceded by foreshocks, studies indicate that there is insufficient evidence to reliably predict the occurrence or magnitude of a large earthquake solely based on foreshocks.
Motivated by my journey as a data scientist, I embarked on a small-scale project to delve deeper into this intriguing subject. My aim was to apply and refine the skills I've acquired, and explore new avenues of learning

## Data
The dataset utilized for this project is the 'All the Earthquakes Dataset: from 1990-2023,' available on Kaggle. You can find the dataset on the Kaggle page at the following link: [All the Earthquakes Dataset: from 1990-2023](https://www.kaggle.com/datasets/alessandrolobello/the-ultimate-earthquake-dataset-from-1990-2023/data)

## Methods and Results
I initiated my work in a Jupyter Notebook accessible at the following link: [Earthquakes Jupyter Notebook](https://github.com/DiegoMZD/Earthquakes/blob/main/jupyter_notebooks/Earthquakes.ipynb). Initially, I imported two essential Python libraries: Numpy, which facilitates numerical computing, and Pandas, which is renowned for data manipulation and analysis. Subsequently, I loaded the dataset named 'Earthquakes-1990-2023.csv', which resides in the 'data' folder of the project repository. You can access the dataset directly via this link: [Earthquakes Dataset](https://github.com/DiegoMZD/Earthquakes/blob/main/data/Eartquakes-1990-2023.csv)

![dataload](https://github.com/DiegoMZD/Earthquakes/blob/main/images/1-load.jpg)

The dataset comprises over 3 million rows of seismic data. Given the substantial processing time required to apply filters and functions to the entire dataset, sampling becomes necessary. To streamline the process, I opted to retain the most recent 250,000 earthquakes, leveraging the dataset's chronological sorting. Notably, I focused exclusively on earthquakes by selecting the 'data_type' attribute to isolate seismic events, as my primary interest lay in identifying premonitory tremors.

![Data sampled](https://github.com/DiegoMZD/Earthquakes/blob/main/images/2-sampling.jpg)

The 'date' column in the dataset is of type 'pandas object' (string). To facilitate time-based filtering, it's imperative to convert these strings into timestamps. To accomplish this, I imported the 'parser' module from 'dateutil', a Python library renowned for its ease of date manipulation. Subsequently, I defined a function named 'to_timestamp' capable of parsing date strings. Utilizing the 'map' function, I applied this 'to_timestamp' function to all elements within the 'date' column. Finally, I replaced the original date strings in the column with the newly created timestamps.

![to-timestamp](https://github.com/DiegoMZD/Earthquakes/blob/main/images/3-timestamp.jpg)

I proceeded to drop columns that are not pertinent to my analysis. Following this, I classified seismic events into two categories: earthquakes, defined as those registering over 6 grades on the Richter scale, and simple tremors, categorized as those registering under 6 grades on the Richter scale.

![drop and earthquakes](https://github.com/DiegoMZD/Earthquakes/blob/main/images/4-earthquakeslist.jpg)
![tremors list](https://github.com/DiegoMZD/Earthquakes/blob/main/images/5-tremorslist.jpg)

For this project, a premonitory tremor is defined as follows: "An earthquake measuring under 6 grades on the Richter scale, occurring within a radius around the epicenter of the earthquake, and manifesting within a defined period of time before the earthquake."

To extract the premonitory tremors associated with any earthquake, I have defined the "premonitory_tremors" function. This function takes the following arguments:
* `earthquake`: a row from the `earthquakes_list` dataframe
* `tremors_list`: the dataframe containing the list of tremors defined earlier
* `radius_km`: the distance (in kilometers) around the epicenter within which the effects of the premonitory tremor are observed
* `period_days_list`: a list containing various periods of time before the earthquake
I have also defined the `calculate_distance` function, which calculates the distance between two points (the earthquake and potential premonitory tremor locations). It takes the following arguments:
* `center`: a tuple containing the latitude and longitude of the earthquake
* `point`: a tuple containing the latitude and longitude of a potential premonitory tremor

The "premonitory_tremors" function returns the list of premonitory tremors associated with an earthquake. To achieve this, it filters the `tremors_list` by location (around the earthquake's epicenter) and by time (days before the earthquake occurrence). For location filtering, I imported the "geodesic" module from the `geopy.distance` library. To filter by time, I utilize the "timedelta" module from the `datetime` library, enabling me to subtract days from a timestamp.

![premonitory_tremors](https://github.com/DiegoMZD/Earthquakes/blob/main/images/6-premonitoryt.jpg)

In this project, premonitory features serve as indicators derived from the premonitory tremors list associated with any earthquake. To extract these premonitory features, I have defined the "premonitory_features" function, which accepts the following arguments:

* `earthquakes_list`: the dataframe containing earthquake data
* `tremors_list`: the dataframe containing the premonitory tremors list
* `radius_km`: the distance (in kilometers) around the epicenter within which the effects of the premonitory tremors are observed
* `period_days_list`: a list containing various periods of time before the earthquake occurrence

A new dataframe named 'earthquakes_and_premonitory' is created as a copy of the 'earthquakes_list' dataframe, with the exclusion of the 'tsunami' and 'data_type' columns, as they are currently irrelevant. The function begins with a for loop iterating through the rows of the 'earthquakes_and_premonitory' dataframe. For each earthquake, the function executes the 'premonitory_tremors_list' function to obtain its associated premonitory tremors list.

Additionally, three lists are defined to store indicators based on the premonitory tremors list. Within the outer for loop, there's another loop that iterates through the 'period_days_list' to calculate the indicators for each day listed. The indicators obtained include the number of premonitory tremors for the day, the average magnitude, and the average significance for each earthquake.

![premonitory_features](https://github.com/DiegoMZD/Earthquakes/blob/main/images/7-premonitoryf.jpg)

After defining the function, it's time to execute it. I obtained the premonitory features for a list of days ranging from 1 (one day before the earthquake) to 14 (two weeks before the earthquake). The radius_km parameter was set to 300 km around the epicenter of the earthquake. I implemented a condition to drop earthquakes that couldn't have a complete set of features. Finally, I saved the results as a CSV file, which can be found in the data folder at the following link: [earthquakes_and_premonitory.csv](https://github.com/DiegoMZD/Earthquakes/blob/main/data/earthquakes_and_premonitory.csv)

![get_premonitory_features](https://github.com/DiegoMZD/Earthquakes/blob/main/images/8-getpremonitoryf.jpg)
![show_newdf](https://github.com/DiegoMZD/Earthquakes/blob/main/images/9-shownewdf.jpg)

The next part of this work is contained in the following Jupyter Notebook: [Premonitory.ipynb](https://github.com/DiegoMZD/Earthquakes/blob/main/jupyter_notebooks/Premonitory.ipynb). In this notebook, I began by importing the necessary libraries, including NumPy and Pandas, and proceeded to load the data for further analysis.

![load](https://github.com/DiegoMZD/Earthquakes/blob/main/images/10-load2.jpg)

After loading the data, I made a copy of it and renamed the first column as "ID". Additionally, I filled any "NaN" values in the dataset with "0".

![premfull](https://github.com/DiegoMZD/Earthquakes/blob/main/images/11-premfull.jpg)

The initial visualization I plotted is a bar plot representing the average number of premonitory tremors per day. I utilized the `matplotlib.pyplot` package, a fundamental Python library for data visualization. The plot suggested that there were more tremors on the day preceding the earthquake compared to other days. However, further analysis was required to confirm this observation.

![barplot](https://github.com/DiegoMZD/Earthquakes/blob/main/images/12-barplot.jpg)

Next, I applied an unsupervised machine learning algorithm called kMeans clustering. I imported the KMeans module from the `sklearn.cluster` library. The goal was to engineer a new feature called "location clusters for the earthquakes". Additionally, I utilized the `plt.imgread()` function to display a map below the clusters plot, enhancing the visualization of the clusters. Subsequently, I added the clusters feature to the "premonitory_full" dataframe.

![kmeans](https://github.com/DiegoMZD/Earthquakes/blob/main/images/13-kmeans.jpg)
![map](https://github.com/DiegoMZD/Earthquakes/blob/main/images/14-map.jpg)

![addclusters](https://github.com/DiegoMZD/Earthquakes/blob/main/images/15-addclusters.jpg)

To explore potential interesting features or relationships, I generated a heatmap of a correlation matrix. Initially, I utilized the `corr()` function to compute the correlation matrix, and then employed the `sns.heatmap()` function to visualize the correlation heatmap. In the heatmap, I observed a region where the squares appeared more red compared to other parts of the matrix. This region corresponded to the relationship between the newly created features, particularly "n_tremors".

![corr](https://github.com/DiegoMZD/Earthquakes/blob/main/images/16-corr.jpg)
![heatmap](https://github.com/DiegoMZD/Earthquakes/blob/main/images/17-heatmap.jpg)

With the observed insight in mind, I proceeded to further investigate the relationship between the features. To accomplish this, I utilized the `sns.boxplot()` function to compare the boxplots of all days (1-14). The plot indicated that the distribution of premonitory tremors across all days appeared similar, with the exception of day 7, which exhibited notable variation. However, beyond this observation, there wasn't much additional insight derived from the graphic.

![boxplots](https://github.com/DiegoMZD/Earthquakes/blob/main/images/18-boxplots.jpg)

Using the `describe()` method, I delved deeper into investigating the features. Consistent with the observations from the bar plot in the previous steps, the descriptive statistics revealed that the mean number of tremors on day one was higher compared to other preceding days. This prompts the question: Are there indeed more tremors observed on the day before an earthquake compared to any other preceding day?

![describe1](https://github.com/DiegoMZD/Earthquakes/blob/main/images/19-describe1.jpg)
![describe2](https://github.com/DiegoMZD/Earthquakes/blob/main/images/20-describe2.jpg)

To ascertain whether the number of tremors one day prior was significantly higher than any other day, I conducted 13 independent t-tests comparing day 1 to days 2 through 14. This involved obtaining the T-statistic, p-value, and making a decision based on the significance level. The results suggested that there were some days with a significant difference in the number of tremors compared to day 1.

![ttest](https://github.com/DiegoMZD/Earthquakes/blob/main/images/21-ttest.jpg)

Next, I explored whether there was any relationship between the number of premonitory tremors on four different days and the magnitude of the leading earthquake. I employed the `plt.scatter()` function to construct scatter plots for each combination of variables and used the `np.polyfit()` function to obtain the slope and intercept of the linear regression equation of the data. The analysis revealed that there was no apparent relationship between the number of premonitory tremors on different days and the leading earthquake magnitude.

![scatter](https://github.com/DiegoMZD/Earthquakes/blob/main/images/22-scatter.jpg)
![4plots](https://github.com/DiegoMZD/Earthquakes/blob/main/images/23-4plots.jpg)

Despite not uncovering any significant insights in the previous steps, I proceeded to explore the data using eight different machine learning models, incorporating all available features. The models tested included Linear Regression, RandomForest Regressor, Ridge, and GradientBoosting Regressor, each trained using two different scalers: StandardScaler and MinMaxScaler. Training was performed, and validation was conducted using cross-validation, with the evaluation metric being the R2 Score. As anticipated, all R2 score values were found to be notably low, indicating poor model performance.

![models](https://github.com/DiegoMZD/Earthquakes/blob/main/images/24-models.jpg)

## Conclusion
In this project, we embarked on an investigation into the phenomenon of premonitory tremors preceding earthquakes. Through data analysis and exploration, we observed that while there is anecdotal evidence suggesting a correlation between increased tremors and imminent earthquakes, our findings did not conclusively support this notion. Despite attempting various analytical and machine learning approaches, including feature engineering and model training, we were unable to identify robust predictors of earthquake occurrence based solely on premonitory tremors data.

However, our journey provided valuable insights into the challenges of earthquake prediction and the limitations of current methodologies. Further research and data collection efforts may be necessary to enhance our understanding of premonitory tremors and their relationship to seismic events. Additionally, incorporating additional data sources and refining analytical techniques could potentially yield more actionable insights in the future.

## Dependencies
To install the dependencies, run the following command:

```bash
pip install -r requirements.txt
```
## Licence
This project is distributed under the [Apache License 2.0](https://github.com/DiegoMZD/Earthquakes/blob/main/LICENSE.txt)


[def]: url
