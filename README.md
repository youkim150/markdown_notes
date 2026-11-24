The following tutorial takes great inspiration from [Dr. Tompkins STAT545 course website](https://ubc-stat.github.io/stat545/webpages/lectures_i/lec1b_introR.html)
## Installing R and RStudio
R and RStudio The main programming language used in the class is R and our main IDE (a text editor with helpful features for writing and running code) is RStudio. The following instructions are adapted from [Chapter 5 of Jenny Bryan’s “happy git” book](https://happygitwithr.com/install-r-rstudio.html).

1. Install (or update to) the latest release R on your computer (4.6.2 currently):

- Windows: [https://cran.r-project.org/bin/windows/base/](https://cran.r-project.org/bin/windows/base/)

3. Install RStudio

- [https://posit.co/download/rstudio-desktop/](https://posit.co/download/rstudio-desktop/)

**Even if you already have R/RStudio, please update them to the most recent version!**

## Navigating RStudio
RStudio has 4 panes.

| **Pane**                                 | **Purpose**                |
| ---------------------------------------- | -------------------------- |
| Source (Top left)                        | Where you edit R Scripts   |
| Console (Bottom left)                    | Execute R commands         |
| Environment (Top right)                  | View variables             |
| Files/Plots/Packages/Help (Bottom right) | Navigate filed and outputs |

## Creating an RStudio Project

1. Select file -> New Project
2. Choose New Directory -> New Project
3. Name your project (e.g. `2026MMDD_Trp_Analysis`)
4. Download the `simulated_tblastn_hits.csv` file and put it in the same folder as `2026MMDD_Trp_Analysis.Rproj`
5. In the **console** run:
	```r
	getwd()
	list.files()
	```
	- `getwd()` returns the current working directory.
	- `list.files()` displays the files in that directory
6. Confirm that you see `simulated_tblastn_hits.csv` on the list

## Basic R commands

Most of what you'll be typing is into the console.
### Variables
Variables store information that can be used later.

The `<-` operator is used to assign a value to a variable

If you run the following code line by line in the console, you will see the environment pane shows `x` and `y` while the operations below will output `15`, `50`, and `2`.
```r
x <- 10
y <- 5

x + y
x * y
x / y
```

### Comments
Comments are lines of code that R will ignore, specified with `#`.
Good practice includes using comments to explain certain sections of code.

```r
# The following lines of codes assign values to
# variables
x <- 10
y <- 5

# The following runs mathematical operations with the
# variables
x + y
x * y
x / y
```

### Data Structures
R can handle different data structures besides just numbers.
#### Strings
A string of characters in quotations
```r
bac_name <- Ligilactobacillus agilis
```
You can then access it by typing `bac_name` in the console

#### Numeric Vectors
Vectors refer to an ordered list of items of the same type. A numeric vector is simply a list of numbers
```r
BLI_ID <- c(1, 43, 3, 58)
```
Typing the name of the variable will output the list.
```r
BLI_ID
```
[1] 1 43 3 58

Accessing elements within a vector can be done by indexing it with [].
```r
BLI_ID[2]
```
[1] 042

#### Vector Functions
Functions can be performed on vectors
```r
offset_BLI_ID <- BLI_ID + 1
offset_BLI_ID
```
[1] 2 44 4 59

Other functions include `length()` for how long a vector is
```r
length(BLI_ID)
```
[1] 4

### Using RStudio and Saving R code
For any code you'd actually save, you store it in an R script. So far you've been entering code line by line in the console. So to save it for the future we write it in an R script.

In RStudiom go to:
- File > New File > R Script
- Then File > Save As
	- Save it in the folder where your `.Rproj` is
	- You can name the code as `r_practice.R`

Enter the following code into `r_practice.R`
```r
BLI_ID <- c(89, 65, 24 2)
offset_ID <- BLI_ID + 1
offset_ID
```
Save your work, then highlight your code and click the "Run" buton or ctrl + shift + enter to run all the code. You can also use ctrl + enter to run the current line of code you're on.

Notice that the environment now has new  values.

Anytime something is run in the console or on the R script, if you assign it to the same variable, it will be overwritten in the environment so be mindful of how you assign your variables.

# Assignment
Now that you've gone through the basics of R and R studio, lets hop into something more complicated.

We will now work with a simulated dataset representing the results of screening bacterial genomes for candidate tryptophan-metabolizing genes.

This is gets us similar information as we did with BLAST on PowerShell, only we're simulating it with R.

**The dataset is entirely simulated and does not represent actual experimental results.**

### Import the dataset
In the console, run:
```r
install.packages("tidyverse")
```

When the installation completes, open a new R Script to enter and run:
```r
library(tidyverse)

hits <- read_csv("simulated_tblastn_hits.csv")
```

The dataset is now stored as an object called `hits`.
### Inspect the data
Try each command separately:
```r
view(hits)
head(hits)
glimpse(hits)
dim(hits)
names(hits)
```

These commands allow you to inspect the structure and contents of your dataset.

### Identify the genes and isolates
Run:
```r
unique(hits$gene)

unique(hits$isolate_id)

n_distinct(hits$gene)

n_distinct(hits$isolate_id)
```

Here, `$` is used to access a particular column within a dataset.

The `unique()` function returns the distinct values, while `n_distinct()` counts them.

### Filtering data
In this project, we will use BLAST to identify sequences potentially homologous to known tryptophan-metabolizing enzymes.

We can filter BLAST results based on criteria such as amino acid identity, query coverage, and E-value.

For example:
```r
filtered_hits <- filter(
	hits,
    pct_identity >= 40,
    query_coverage >= 70,
    evalue <= 1e-5
  )

view(filtered_hits)
```

This code retains rows that satisfy all three specified screening criteria.

Try modifying the filtering criteria.

Change the minimum identity requirement from 40% to 50%.

```r
filtered_hits_50 <- filter(
	hits,
    pct_identity >= 50,
    query_coverage >= 70,
    evalue <= 1e-5
  )
```

Compare the number of rows:

```r
nrow(filtered_hits)
nrow(filtered_hits_50)
```

How does increasing the identity threshold affect the number of reported candidate matches?

### Summarizing results
Count how many filtered hits were identified for each gene:

```r
gene_counts <- count(filtered_hits, gene, sort = TRUE)

view(gene_counts)
```

This produces a simple summary table showing the distribution of qualifying hits among the candidate genes.

Save your R script and bring what you have prepared for Thursday.
