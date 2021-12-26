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

