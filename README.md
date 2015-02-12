<!-- README.md is generated from README.Rmd. Please edit that file -->
**Not quite ready for showtime but release coming very soon!**

**NOTE: refactoring underway and README not up-to-date**
--------------------------------------------------------

Google Spreadsheets R API
-------------------------

[![Build Status](https://travis-ci.org/jennybc/gspreadr.png?branch=master)](https://travis-ci.org/jennybc/gspreadr)

Manage your spreadsheets with *gspreadr* in R.

*gspreadr* is patterned after [gspread](https://github.com/burnash/gspread), a Google Spreadsheets Python API

Features:

-   Open a spreadsheet by its **title** or **url**.
-   Extract range, entire row or column values.

![gspreadr](README-gspreadr.png)

Basic Usage
-----------

``` r
library(gspreadr)
```

``` r
# See what spreadsheets you have
list_spreadsheets()

# Open a worksheet from spreadsheet with one shot
ws <- open_at_once("Temperature", "Sheet1")

update_cell(ws, "B2", "January")

# Fetch a cell range
df <- read_range(ws, "A1:B7")
```

Authorization
-------------

##### Authorization using OAuth2 (recommended)

``` r
# Give gspreadr permission to access your spreadsheets and google drive
authorize() 
```

##### Alternate authorization: login with your Google account

``` r
login("my_email", "password")
```

More Examples
-------------

### Opening a spreadsheet

``` r
# You can open a spreadsheet by its title as it appears in Google Drive
ssheet <- open_spreadsheet("Gapminder")
```

##### Usually for public spreadsheets (set visibility = "public" for public spreadsheets):

``` r
# Open a spreadsheet by its key 
ssheet <- open_by_key("1hS762lIJd2TRUTVOqoOP7g-h4MDQs6b2vhkTzohg8bE")

# You can also use the entire URL
ssheet <- open_by_url("https://docs.google.com/spreadsheets/d/1hS762...")
```

``` r
# See the structure
str(ssheet)
```

### Creating and Deleting a spreadsheet

``` r
# Create a new spreadsheet by title
add_spreadsheet("New Spreadsheet")

# Move spreadsheet to trash
del_spreadsheet("New Spreadsheet")
```

### Opening a worksheet

``` r

# Get a list of all worksheets
list_worksheets(ssheet)

# You can open a worksheet by index. Worksheet indexes start from one.
ws <- open_worksheet(ssheet, 1)

# By title
ws <- open_worksheet(ssheet, "Africa")

# See the structure
str(ws)
```

### Viewing a worksheet

``` r
# Take a peek at your worksheet
view(ws)
```

``` r
# Take a peek at all the worksheets in the spreadsheet
view_all(ssheet)
```

### Creating and Deleting a worksheet

``` r
# Create a new worksheet
add_worksheet(ssheet, title = "foo", rows = 10, cols = 10)

# Delete worksheet
new_ws <- open_worksheet(ssheet, "foo")
del_worksheet(new_ws)
```

### Renaming a worksheet

``` r
rename_worksheet(ssheet, old_title = "Old Name", new_title = "Cooler Name")
```

### Getting all values from a row or range of rows

``` r
get_row(ws, 3)

get_rows(ws, from = 2, to = 5, header = TRUE)
```

### Getting all values from a column or range of columns

``` r
one_col <- get_col(ws, 1)

many_cols <- get_cols(ws, from_col = 1, to_col = 3)
```

### Getting a region of a worksheet

``` r

# By boundary rows and cols
read_region(ws, from_row = 1, to_row = 5, from_col = 1, to_col = 6)

# By range
read_range(ws, "A1:F5")
```

### Getting the entire worksheet

``` r
all_my_data <- read_all(ws)
```

### Getting all worksheets from a spreadsheet as a list of worksheet objects

``` r
my_ws <- open_worksheets(ssheet)
```

### Getting a cell value

``` r
# With label
get_cell(ws, "B2")

# With coordinates
get_cell(ws, "R2C2")
```

### Finding cells

``` r
# find a cell with value (cell of first appearance)
find_cell(ws, "Algeria")

# find all cells with value
find_all(ws, "Algeria")
```

### Updating cells

``` r
# update a single cell
update_cell(ws, "B4", "Oops")

get_cell(ws, "B4")

# Update cells in batch - specify range
update_cells(ws, "A1:C1", c("Country", "Continent", "Year"))

read_range(ws, "A1:C1")

# Update cells in batch - specify anchor cell
update_cells(ws, "D1", c("Life Expectancy", "Population", "GDP Per Capita"))

read_range(ws, "D1:F1")
```
