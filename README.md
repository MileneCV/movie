# Movie Production
- Author: Milene Carmes Vallejo

![image](https://user-images.githubusercontent.com/112773242/225512721-2c6af25a-0d1d-4673-86e0-670024b34ca2.png)

(public domain leisure CC0 image).

## Business Problem
For this project, I have been hired to produce a MySQL database on Movies from a subset of IMDB's publicly available dataset. 
I will use this database to analyze what makes a movie successful and will provide recommendations to the stakeholder on how to make a successful movie.

## Specifications

The stakeholder only wants you to include information for movies based on the following specifications:

- Exclude any movie with missing values for genre or runtime
- Include only full-length movies (titleType = "movie").
- Include only fictional movies (not from documentary genre)
- Include only movies that were released 2000 - 2021 (include 2000 and 2021)
- Include only movies that were released in the United States

## Part 1: Download several files from IMDB’s movie data set and filter out the subset of moves requested by the stakeholder.
## The Data
IMDB Provides Several Files with varied information for Movies, TV Shows, Made for TV Movies, etc.

- Overview/Data Dictionary: https://www.imdb.com/interfaces/
- Downloads page: https://datasets.imdbws.com/

From their previous research, they realized they want to focus on the following files:

- title.basics.tsv.gz
- title.ratings.tsv.gz
- title.akas.tsv.gz

## Part 2: Use an API to extract box office revenue and profit data to add to your IMDB data and perform exploratory data analysis.

### 1 - How many movies had at least some valid financial information (values > 0 for budget OR revenue)? Exclude any movies with 0's for budget AND revenue from the remaining visualizations.
628 movies had at least some valid financial information

### 2 - How many movies are there in each of the certification categories (G/PG/PG-13/R)?

![image](https://user-images.githubusercontent.com/112773242/225515418-e1f9b9b2-8c62-4326-b009-a8dcbcb31b0b.png)

### 3 - What is the average revenue per certification category?

![image](https://user-images.githubusercontent.com/112773242/225515508-4ee5fd18-2baa-4a26-b2b6-9094bf6c5170.png)

### 4 - What is the average budget per certification category?

![image](https://user-images.githubusercontent.com/112773242/225515578-ad87adbb-ea05-4f4c-9213-85180c768cd5.png)

### Part 3: Construct and export a MySQL database using your data.

- The stakeholder wants to take the data you have been cleaning and collecting in Parts 1 & 2 of the project, and wants you to create a MySQL database for them.

- You should normalize the tables as best you can before adding them to your new database.

Note: an important exception to their request is that they would like you to keep all of the data from the TMDB API in 1 table together (even though it will not be perfectly normalized).
You only need to keep the imdb_id, revenue, budget, and certification columns

#### Check the primary key in mysql worbench

![image](https://user-images.githubusercontent.com/112773242/225519670-1fdb8c37-9f0f-49b5-9624-9b1ece35ee78.png)

### Part 4: Apply hypothesis testing to explore what makes a movie successful.

I will then use the MySQL database to answer several hypotheses about movie success.

# Q1: does the MPAA rating of a movie (G/PG/PG-13/R) affect how much revenue the movie generates?

### 1. State the Hypothesis & Null Hypothesis
- H0 (Null Hypothesis): no difference in revenue amount between all MPAA rating.
- HA(Alternative Hypothesis): there is a significant difference in revenue amount between ratings.

### 2. Determine the correct test to perform.
- Type of Data? numeric
- How many groups/samples? more than two
- Therefore, which test is appropriate? ANOVA

### ANOVA Assumptions
- No significant outliers
there were 494 outliers in unknow
there were 72 outliers in PG
there were 226 outliers in R
there were 20 outliers in G
there were 56 outliers in NR
there were 158 outliers in PG-13
there were 4 outliers in NC-1

- Normality
Our p-values for all group are below 0.05 which means our data is NOT normally distributed. However, our our sample size is large enough (n>20) to proceed without satisfying this test.

- Equal Variance 
p<0.05 fail the assumption of equal variance we need select a non-parametric equivalent test - Kruskal-Wallis test

