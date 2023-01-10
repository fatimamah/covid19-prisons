# Visualization of COVID-19 Case Rates in Prisons and General Population

Info 3300: Data Driven Web Visalizations–Project 1

Group Members: Fatima Mahmoud (fam76), Ayesha Kemal (ak2235), and Aishwarya Singh (as2778)

To run web server, use `python -m http.server` for python 3

## Final Visualization

## Data

### Data Sources

- Our group obtained the prison data from [The Marshall Project and Associated Press’ datasets](https://data.world/associatedpress/marshall-project-covid-cases-in-prisons/workspace/data-dictionary) on data.world and more closely examined the [covid_prison_cases.csv](https://data.world/associatedpress/marshall-project-covid-cases-in-prisons/workspace/file?filename=covid_prison_cases.csv) and the [prison_populations.csv](https://data.world/associatedpress/marshall-project-covid-cases-in-prisons/workspace/file?filename=prison_populations.csv).
  - The data has been updated and changed, but our data is taken from March 25th, 2021.
- Our group obtained the general population data from [The New York Times dataset](https://github.com/nytimes/covid-19-data/tree/master/rolling-averages) on github and more closely examined the [us-states.csv](https://github.com/nytimes/covid-19-data/blob/master/rolling-averages/us-states.csv) (under the rolling averages folder).

### Data Cleaning

- Used [this online converter](https://www.convertcsv.com/csv-to-json.htm) to convert covid_prison_cases, prison_populations (named prison2 in our folder), and general covid cases from csv files to json files.

- **Prison Data:**
  - (covid_prison_cases) Changed the strings in the “date” column to date Objects.
  - Formatted the date to be month and year and put it in a new column: “month_year”
  - Filtered out “Federal” from state names
  - Filtered out null total prisoner rates
  - Filtered out dates in 2021
  - The raw, original data came in forms of weeks so we used d3.groups to group each row of data into the month they belonged to under the state they belonged to.
  - The d3 groups resulted in a highly nested array of arrays, so we had to use multiple loops to access the number of prison cases each week. 
  - Once we were able to access that data, we took the averages of all the prison cases per week (number of cases that week/monthly population) for that state for  a     given month and then divided by the number of weeks in the month.
  - We did it this way because we thought the data would be most accurate since we are given the number of cases per week. After that we had all the data we needed to    create our data structure.
  - We created a dictionary called pri_rates, which had the state names as keys and the values were a separate dictionary that contained id of the state and an            additional dictionary called properties that had all the months as keys and the covid prison rates as keys. An image of the data structure is shown below for          visualization purposes:


- **General Data:**  
  - Changed the strings in the “date” column to date Objects.
  - Formatted the date to be lowercase acronyms of the months and put it in a new column: “month_year”
  - Filtered out null values from column “cases_avg_per_100k.”
  - Filtered out dates in 2021.
  - Filtered out February and January of 2020.
  - Filtered out the states/places: District of Columbia, Northern Mariana Islands, Virgin Islands, Guam, and Puerto Rico
  - The raw, original data came in forms of weeks so we used d3.rollups to group each row of data into the month they belonged to under the state they belonged to. We     calculated the covid rate by taking the cases_avg_per_100k for each week dividing it by 100,000, then multiplying each by 100, and finally taking the mean of all       those weeks to get the covid rate for the month.
  - We used the same process to create the gen_rates dictionary as we did to create the pri_rates dictionary. We had the state names as keys and the values were a         separate dictionary that contained id of the state and an additional dictionary called properties.

### Data Variables

- pri_rates: contains our cleaned, filtered prison covid data. A dictionary of objects.
  - Key: the state name 
  - Value: a dictionary of ids and properties
    - Id: the id of the state, used to identify the state when filling the topojson in
    - properties: 
      - mar: the covid rate of prisons in that state for March 2020
      - apr: the covid rate of prisons in that state for April 2020
      - may: the covid rate of prisons in that state for May 2020
      - jun: the covid rate of prisons in that state for June 2020
      - jul: the covid rate of prisons in that state for July 2020
      - aug: the covid rate of prisons in that state for August 2020
      - sep: the covid rate of prisons in that state for September 2020
      - oct: the covid rate of prisons in that state for October 2020
      - nov: the covid rate of prisons in that state for November 2020
      - dec: the covid rate of prisons in that state for December 2020

- gen_rates: contains our cleaned, filtered general population covid data. A dictionary of objects.
  - Key: the state name 
  - Value: a dictionary of ids and properties
    - Id: the id of the state, used to identify the state when filling the topojson in
    - properties: 
      - mar: the covid rate of the general pop in that state for March 2020
      - apr: the covid rate of the general pop in that state for April 2020
      - may: the covid rate of the general pop in that state for May 2020
      - jun: the covid rate of the general pop in that state for June 2020
      - jul: the covid rate of the general pop in that state for July 2020
      - aug: the covid rate of the general pop in that state for August 2020
      - sep: the covid rate of the general pop in that state for September 2020
      - oct: the covid rate of the general pop in that state for October 2020
      - nov: the covid rate of the general pop in that state for November 2020
      - dec: the covid rate of the general pop in that state for December 2020

### Additional Data

- d3.geoAlbersUsa() shape map
- Professor Rzeszotarski’s Dataset: us-state-names.tsv
- Professor Rzeszotarski’s Dataset: Us1.json

## Visual Design Rationale

Our group first chose to display the two choropleths side by side. We chose to have them positioned side by side because we wanted to show the stark differences between covid rates in prisons and covid rates in the general population. 

For the prison covid rates, we first grouped all the weeks to their corresponding state using d3.groups, then used loops to divide the covid cases per week for each month by the population for that month from a different dataset, then found the mean of the weeks to find the final covid rate for that month. We only used the covid cases for prisoners-not the staff-because we wanted to see the covid rate for prisoners specifically. Staff are not confined to the prison or the same space the way prisoners are, so there is more chance for them to social distance than there is for prisoners. It wasn’t important to our story to include staff. 

For the general population rates, we grouped all the weeks by state and month and divided each week’s average cases per 100k people by 100,000 then multiplied by 100, then found the mean of the weeks to find the final covid rate for that month using d3.roll ups.We only used the months March-December of 2020 because they were the only months in the prison dataset with prison populations.

We filled in each state’s covid rate for each month by using a linear scale for the percentages because d3.linearScale can transform data values into positions on a scale. We used the domain 0 to 100 because the data values are percentages. The general population maps’ colors don’t change because all of the rates are under 1% so it stays at the lightest blue. Since it is a percentage and because it is being compared to the prison data, the scale has to stay the same at 0 to 100.

Our choropleths’ color is blue and sequential. We chose blue because blue is commonly thought of as a medical color or the color of hospitals, which is fitting because Covid-19 is a serious illness. We chose to only have blue and have it sequential (as opposed to diverging) by only varying the hue of the color blue because the difference in hue will show the difference in the rates and because we don’t have a central number/rate important enough to center our color palette around it like we would need for a diverging scale. The slider follows the same color palette as the map. It is gray and the button to move it is dark blue. 

States that contain no data for that month are colored light gray. We chose the color light gray because it is light so it’s not overwhelming and won’t take away the user’s attention and because it is neutral and the standard for when there is no data available. 

For some of the months, some states’ prison covid rates were over 100%. We explored the data more to find out why and we found that for those months, the covid cases in the states’ prisons were higher than the states’ prison populations. We don’t know why that is, but because The Marshall Project and The Associated Press are known, credible sources (and because other prison covid-19 data directed users to this dataset for the most accurate data), we decided to keep using the data, but make those states null for those specific months. It’s not very many months or states, so the choropleth was still usable. These states are also colored light gray to indicate that there is no data for that month, because there isn’t any usable data for it.

One hypothesis for why there are some rates of over 100% is that the data collection process for Covid cases in prisons as well as the population counts were either under/over reported. This might because prisons are often understaffed in general and during a global pandemic their workload must have been even more difficult trying to manage the spread of the virus in such a confined space.  

We chose to have the order of the visualizations be the choropleths first, then line graphs so the user can compare all the states together in each map, then get specific in the line graphs. So from large scale to smaller scale, that way users can visualize the overall data and if they would like more information they can see the individual data for each state. 

The line graphs take the data for each state and maps a line for the prison rates and a line for the general pop rates over time so the user can see how they compare to each other (increasing/decreasing/high rate/low rate/etc) in one glance. Aishwarya added x and y axis labels and a title to make it easy for the user to understand what the data is representing. I also added a legend at the bottom with blue for the general population (because we used that for the map) and orange for the prison population because well, prisons are associated with the color orange. If Aishwarya had not had the issue she talked about with the professor, she would have made a more proper legend with a border and colored text and have placed it next to the graph so that it would not distract from the x axis label. Currently, the legend is at the bottom of the graph and it makes it hard for the reader to find especially with the x axis label. She also would have more properly positioned and formatted both labels. She chose to add changing titles for the state so that the user would be clear about which state’s data they are looking at after they click on a button. She put it separate from the main graph title so that it would be easy for the user to see. In a perfect world, she would have formatted it better to make it more bold while also not taking away from the main title. She also would have included dots on the main curves to make it easier for the reader to see points. She had trouble on this portion of the project and was unable to find time to go to office hours due to the situation. 

## Interactive Design Rationale

For our choropleths, we have two interactions: the time slider and the hover over feature.

Slider with changing title: We chose to make a slider for the user to move in order to change the choropleths to different months. We chose a slider as opposed to buttons or a dropdown menu because a slider allows the user to change the months easily and to see the difference just by sliding the slider in one motion. It feels smooth and continuous, just like how time is continuous. Our slider is pretty big so the user can see that changing the months is a feature. We chose to have a title that changes as the slider input changes to inform the user of what month they are looking at and it being a title in the middle of the page, it brings attention to that month so the user doesn’t miss it.

Hover over: The user can hover over each state and see the state name and covid rate in a black box underneath the state. Each covid rate percentage is rounded to 2 decimal points as to have an easy to read number that will not take up a huge portion of the screen. For states that are grayed out, the box says “State Data not Available” to assure users that the maps are working and that the state should not be a blue color. The ability to hover over a state and see the details is important so the user can see specific differences. It is also especially important for the general population map where the color doesn't change because all of the rates are below 1%, but the rates slightly change to show those differences. 

The interactivity for the line graphs are buttons to see each state’s line graph. Originally, we were going to have the interactivity be that the states are clickable and to hover over each point in the lines. However, Aishwarya was unable to complete this portion of the project due to issues she spoke to the professor about. Aishwarya also added a changing title whenever the user clicks on a button so that they know which state’s data they are seeing. Aishwarya also stacked all the buttons on top of each so that they are easy to find. For the initial graph, Aishwarya chose the state Texas (for honestly no reason other than that it was the most prominent state). However, if she had not gone through that specific issue, she would have chosen to graph the country’s average covid rate over time rather than a specific state. That way no one state shines over the other. She would have also included instructions on how the clickable states would work. 

## The Story

Our motivation for this project were all the news headlines about the public health crisis in prisons. The Marshall Project ([1 in 5 Prisoners in the US has had Covid-19](https://www.themarshallproject.org/2020/12/18/1-in-5-prisoners-in-the-u-s-has-had-covid-19)), Equal Justice Initiative ([Studies detail large COVID outbreaks at US prisons, jails](https://eji.org/news/covid-19s-impact-on-people-in-prison/)), New York Times ([Covid-19: Infections Among U.S. Prisoners Have Been Triple Those of Other Americans and Incarcerated and Infected: How the Virus Tore Through the U.S. Prison System](https://www.nytimes.com/live/2021/04/10/world/covid-vaccine-coronavirus-cases)), and many more. Additionally, in many states there were calls to release inmates/incarcerated individuals from prisons during the pandemic to spare them from the huge chance of them contacting covid and potentially dying ([Cuomo releases 3000 people, More than 10000 Illinois prisoners released](https://www.adirondackdailyenterprise.com/news/2020/10/2200-have-been-released-from-prison-due-to-pandemic/), etc). We wanted to see for ourselves how covid-19 rates in prison differed from the general populations’ rates.

Our choropleths show that the differences between them are night and day. We were very surprised to see that the prison choropleth changes drastically throughout the months, getting worse, states being colored dark blue and the rates hitting close to 100% while the colors in the general population choropleth does not change at all, staying at the lightest shade of blue because it never goes over 1% for the covid rate. This is extremely alarming and conveys how high the spread of covid is in prisons. Outside of prison, we are all scared to contract covid, go to great lengths (remote work/school, masks, social distancing, not eating out) to decrease our chances of contracting it, and most likely know someone who has had covid and *that is with a case rate of less than 1% for our individual states*. For prison case rates to be so much higher is terrifying. Prisoners are experiencing a pandemic that is even worse than what the rest of us are living through and our choropleths show that really well.

According to the articles, a few reasons for this may be because it is hard social distance in prison because of the crowding and prisons have not quarantined their inmates properly, there isn’t frequent testing, and there is poor contact tracing. It would be interesting to see the resources and amount of funding prisons received to combat covid-19 with other populations such as the general population iof course, but also nursing homes, since they were also frequently in the news for high case rates and high vulnerability.


## Team Contributions

Fatima: cleaned prison and general population data, both choropleths, hover over feature, went to office hours, attended demo, and wrote report (30+ hours)  
Ayesha: cleaned prison and general population data, both choropleths, slider interactivity, legend, went to office hours, attended demo, and wrote report  (30+ hours)
Aishwarya: line graph and line graph interactivity, went to office hours, added additional CSS and labels, added information about the line graph to report (15 hours)

The longest part was cleaning the prison data. We had to combine two datasets to find the rates and change the data structure. We ran into a lot of problems we weren’t expecting while cleaning the data. It took about 15 hours. 

We also spent about 9 hours trying to find a way to make the code less redundant and more efficient/optimized but our functions didn’t work and we gave up on that in order to work on the rest of the project. It took the second longest time of anything we did. This was supposed to be Aishwarya’s job but unfortunately, due to Covid issues (that she discussed with Professor Jeff), she was unable to complete this portion of the project. 







  

