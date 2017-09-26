---
title: "Using RSQLite"
output:
  pdf_document: default
  html_document: default
  word_document: default
---
Here is an example of how to connect SQLite to R using the RSQLite package.  Once the connection to the database is set using the code on line 13, then we can bring data either previously stored in R (i.e. mtcars) or user generated data (i.e. testTable).  
```{r, message=FALSE, warning=FALSE}
library(RSQLite)
library(DBI)
mydb <- dbConnect(RSQLite::SQLite(), "my-db.sqlite")
mydb
#dbWriteTable(mydb, "mtcars", mtcars)
testTable = as.data.frame(matrix(rnorm(100), 25, 4))
#dbWriteTable(mydb, "testTable", testTable)
dbListTables(mydb)
```
Once we have the tables in the database, we can use standard SQL commands to query data.  For example, here I am grabbing cars with mpg's greater than 20, then ordering them by hp.  
```{r, message=FALSE, warning=FALSE}
dbGetQuery(mydb,'SELECT * FROM mtcars WHERE "mpg" > 20')

dbGetQuery(mydb,'SELECT * FROM mtcars WHERE "mpg" > 20 ORDER BY hp')
```
RSQLite also works with aggregating functions such as SUM and with operators such as OR and GROUP BY to drill down to your specific data needs.
```{r, message=FALSE, warning=FALSE}
dbGetQuery(mydb,'SELECT SUM(cyl) FROM mtcars')

dbGetQuery(mydb,'SELECT SUM(cyl) FROM mtcars WHERE vs = 1')

dbGetQuery(mydb,'SELECT SUM(cyl) FROM mtcars WHERE vs = 1 OR am = 1')

dbGetQuery(mydb,'SELECT SUM(cyl) FROM mtcars GROUP BY vs')
```
