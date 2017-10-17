## Standard Errors

Here is some description.

### Cool Analysis

Check out this great data analysis work.

```markdown
`rm(list = ls(all = TRUE))
library(tidyverse)
library(tidyquant)
library(quantmod)
library(Quandl)

AORD <- read_csv("AORD_Index_20070724-20170227.csv")
head(AORD)`
```

```markdown
`#==============================================================================
#
# ASX most shorted stocks
#
#==============================================================================
rm(list = ls(all = TRUE))
library(tidyverse)
library(XML) 
library(stringr)
library(rvest)
library(hrbrthemes)

#--- Scraping Short Sales Data ------------------------------------------------
url <- "http://asic.gov.au/regulatory-resources/markets/short-selling/short-position-reports-table/"
links <- getHTMLLinks(url)
head(links)

# extract only links containing csv files
links_csv <- links[str_detect(links, ".csv")]
head(links_csv) 

shortsales_0922 <-
  read.csv(
    "http://asic.gov.au/Reports/Daily/2017/09/RR20170922-001-SSDailyAggShortPos.csv",
    header = TRUE,
    sep = '\t',
    fileEncoding = "utf-16"
  )
head(shortsales_0922)

# Rename columns
colnames(shortsales_0922) <- c("company", 
                               "ticker", 
                               "short_positions", 
                               "shares_on_issue", 
                               "short_ratio")
head(shortsales_0922)

#--- Ranking companies based on short sales / shares outstanding --------------
top_10 <- shortsales_0922 %>% 
  filter(rank(desc(short_ratio)) <= 10)
top_10

# Plot top 10
top_10 %>% 
  ggplot(aes(reorder(ticker, short_ratio), short_ratio, fill = short_ratio)) +
  geom_bar(stat = "identity", width = 0.5, color = "darkblue") +
  labs(title = "Top 10 most shorted ASX stocks",
       x = "",
       y = "Short Positions / Shares Outstanding (%)") +
  guides(fill = FALSE) +
  theme_ipsum()`
```
