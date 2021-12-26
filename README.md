# R-Webscraping

<a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license"><img style="border-width: 0;" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png" alt="Creative Commons License" /></a>
This tutorial is licensed under a <a href="http://creativecommons.org/licenses/by-nc/4.0/" rel="license">Creative Commons Attribution-NonCommercial 4.0 International License</a>.

# Acknowledgements
This lab is sourced from the <a href="https://cougrstats.wordpress.com/2018/10/12/webscraping-in-r/"> CougRStat </a> lesson material.

# Functions we'll be using
These are the three functions that are used during thie assignment for webscraping. These are the only functions that are used from the “rvest” package. Everything else in this presentation is base R.

```R
read_html()
html_nodes()
html_table()
```

# What is Web Scraping?
Web scraping is the process of automatically collecting data from web pages without visiting them using a browser. This can be useful for collecting information from a multitude of sites very quickly. Also, because the scraper searches for the location of the information within the webpage it is possible to scrape pages that change daily to get the updated information.

# Scraping in R using rvest
We will focus on scraping without any manipulation of the webpages that we visit. Webpage manipulation while scraping is possible, but it can not be done exclusively in R which is why we will be ignoring it for this lesson.

# HTML
HTML stands for HyperText Markup Language. All HTML pages are built using the same format. A very generalized version of this is that a page should always have a header, a body, and a footnote. This isn’t always the case though and is up to the developer.

# HTML Tree
<img src = "figures/R-Webscraping-fig1.png">

# Information to Gather
Let’s collect some environmental data. I want to know what the weather station on the roof is reporting right now.
The url for the PACCAR Weather Station is http://micromet.paccar.wsu.edu/roof/

# Install rvest
This is a package for R that should download the webpage as html for further manipulation.
```R
# Load the library
if(!require(rvest)){
    install.packages("rvest")
    library(rvest)
}
```

# Download the HTML
First we need to tell R to navigate to the site and save the current html of the page.
```R
# Save the url as a variable
weather.station <- read_html('http://micromet.paccar.wsu.edu/roof/')
```

# Extract Values From Table
Next we specify the html nodes that we are interested in. In this case these are all referred to with the label “font” which allows us to specify that we want all values from the page that are labeled “font”.
```R
# Extract the table values from the HTML
table.values <- html_nodes(weather.station, xpath = '//font/text()')
```

# Visualize the table
```R
head(table.values, 13)
```

```{r}
## {xml_nodeset (13)}
##  [1]  
##  [2]  Latest time
##  [3]   2018-10-08 09:10:00 
##  [4] Net Radiation
##  [5]   106.7  Wm
##  [6] Temperature
##  [7]    8  &amp;deg C ( 46.4 &amp;deg F )
##  [8] Humidity
##  [9]    76.8 %
## [10] Pressure
## [11]    923.4 mbar
## [12]  Wind speed
## [13]    2.7 m/s (6 mph)
```

# Save the Values as Individual Variables
We’re going to save the values that we want from the previous list as individual variables
```R
# Time
scraped.datetime <- as.character(table.values[3])
# Radiation
radiation <- as.character(table.values[5])
# Temperature
temperature <- as.character(table.values[7])
# Humidity
humidity <- as.character(table.values[9])
# Pressure
pressure <- as.character(table.values[11])
# Wind Speed
wind.speed <- as.character(table.values[13])
# Rain
rain <- as.character(table.values[17])
```

# View the Variables to Check Formatting
Let’s view one of our variables to see how it is formatted now.
```R
# Print the variable to the console
scraped.datetime
```

```{r}
## [1] "  2018-10-08 09:10:00 "
```

# Split the Datetime into Date and Time
```R
# Use strsplit to separate into a list
datetime <- strsplit(scraped.datetime, " ")
# View the list after the split
datetime
```

```{r}
## [[1]]
## [1] ""           ""           "2018-10-08" "09:10:00"
```

```R
# Select and save the scraped date
scraped.date <- datetime[[1]][3]
# Select and save the scraped time
scraped.time <- datetime[[1]][4]
# Print the time
scraped.time
```


```{r}
## [1] "09:10:00"
```

# Create a Function to Scrape Radiation
This is our radiation scraping function

```R
scrape.raditation <- function(){
  # Download the html
  weather.station <- read_html('http://micromet.paccar.wsu.edu/roof/')
  # Extract the table values
  table.values <- html_nodes(weather.station, xpath = '//font/text()')
  # Save the radiation value
  radiation <- as.character(table.values[5])
  # Split the string
  radiation.temp <- strsplit(radiation, " ")
  # Return only the numerical value
  return(radiation.temp[[1]][3])
}
```

# Let’s Try Our Radiation Function
Execute the function

```R
scrape.raditation()
```
