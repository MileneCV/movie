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

### 3. Testing Assumptions
- **No significant outliers**

there were 494 outliers in unknow,
there were 72 outliers in PG,
there were 226 outliers in R,
there were 20 outliers in G,
there were 56 outliers in NR,
there were 158 outliers in PG-13,
there were 4 outliers in NC-1.

- **Normality**
Our p-values for all group are below 0.05 which means our data is NOT normally distributed. However, our our sample size is large enough (n>20) to proceed without satisfying this test.

- **Equal Variance** 
p<0.05 fail the assumption of equal variance we need select a non-parametric equivalent test - Kruskal-Wallis test

### 4 - Final Hypothesis Test
KruskalResult(statistic=28719.975655133385, pvalue=0.0)

### 5 - Interpret your p-value and reject or fail to reject your null hypothesis
p < 0.05 - reject null hypothesis and support the alternate hypothesis there is a significant difference in revenue amount between ratings.

### 6 - Show a supporting visualization that helps display the result

![image](https://user-images.githubusercontent.com/112773242/225708043-8ca697f5-d056-40e0-afcc-f1736fa916a2.png)

PG and PG3 were the rating with higher average revenue, NR, NC-17 and Unknow were the rating with lower average revenue.

![image](https://user-images.githubusercontent.com/112773242/225708221-ab71abe5-6ec9-4e1e-9f1e-ddc1d58c9bbd.png)

PG13 was ratting with the higher total revenue amount. G, NR and NC-17 were the ratting with lower total revenue.

#### Post-Hoc Multiple Comparison Test
There is no difference (p>0.05) between:
- NC-17 and NR
- NC-17 and unknow
- NR and unknow
- PG AND PG3

These groups have similar x values in tukey plot below:
![image](https://user-images.githubusercontent.com/112773242/225708813-8c7f6d30-ae18-4119-b233-b52fc46f94d0.png)



# Q2 - Do some movie genres earn more revenue than others?

### 1. State the Hypothesis & Null Hypothesis

H0 (Null Hypothesis): no difference in revenue amount between all genres.

HA(Alternative Hypothesis): there is a significant difference in revenue amount between genres.

### 2. Determine the correct test to perform.
Type of Data? numeric

How many groups/samples? more than two

Therefore, which test is appropriate? ANOVA

### 3. Testing Assumptions

- **No significant outliers - outliers ( z > 3)**

there were 452 outliers in Comedy,
there were 31 outliers in Music,
there were 229 outliers in Romance,
there were 115 outliers in Science Fiction,
there were 611 outliers in Drama,
there were 260 outliers in Action,
there were 119 outliers in Crime,
there were 184 outliers in Adventure,
there were 115 outliers in Animation,
there were 133 outliers in Fantasy,
there were 264 outliers in Horror,
there were 276 outliers in Thriller,
there were 59 outliers in History,
there were 142 outliers in Family,
there were 125 outliers in Mystery,
there were 12 outliers in Western,
there were 5 outliers in Unknow,
there were 29 outliers in War,
there were 2 outliers in TV Movie,
there were 4 outliers in Documentary,

- **Normality**
Our our sample size is large enough (n>20) to proceed without satisfying this test.

- **Equal Variance**
p<0.05 fail the assumption of equal variance we need select a non-parametric equivalent test - Kruskal-Wallis test

### 4 - Final Hypothesis Test
KruskalResult(statistic=7133.177100216466, pvalue=0.0)

### 5 - Interpret your p-value and reject or fail to reject your null hypothesis
p < 0.05 - reject null hypothesis and support the alternate hypothesis there is a significant difference in revenue amount between genres.

### 6 - Show a supporting visualization that helps display the result

![image](https://user-images.githubusercontent.com/112773242/225711730-88c2c34a-c965-48db-8e92-5b59be1885a2.png)

Adventure was the genre with higher average revenue. Documentary, TV movie horror and drama were the genres with lower average revenue.

![image](https://user-images.githubusercontent.com/112773242/225711828-d335db3b-d786-492c-97ee-0e1db6159e34.png)

Adventure and Action were the genres with higher total revenue. TV Movie, Documentary and unknow were the genres with lower total revenue.

### Post-Hoc Multiple Comparison Test
![image](https://user-images.githubusercontent.com/112773242/225712097-ca7d15b1-8468-4d26-b0c5-151248fc0b7a.png)

Adventure was the genres with with more different revenue.

# Q3 - Are some genres higher popularity than others?

### 1. State the Hypothesis & Null Hypothesis

H0 (Null Hypothesis): no difference in popularity between all genres.

HA(Alternative Hypothesis): there is a significant difference in popularity between genres

### 2. Determine the correct test to perform.

Type of Data? numeric

How many groups/samples? more than two

Therefore, which test is appropriate? ANOVA

### 3. Testing Assumptions

- **No significant outliers - outliers ( z > 3)**
there were 427 outliers in Comedy
there were 36 outliers in Music
there were 211 outliers in Romance
there were 30 outliers in Science Fiction
there were 863 outliers in Drama
there were 107 outliers in Action
there were 177 outliers in Crime
there were 15 outliers in Adventure
there were 56 outliers in Animation
there were 13 outliers in Fantasy
there were 338 outliers in Horror
there were 350 outliers in Thriller
there were 40 outliers in History
there were 21 outliers in Family
there were 117 outliers in Mystery
there were 2 outliers in Western
there were 138 outliers in Unknow
there were 33 outliers in War
there were 11 outliers in TV Movie
there were 17 outliers in Documentary

- **Normality**

Our sample size is large enough (n>20) to proceed without satisfying this test.

- **Equal Variance**

p<0.05 fail the assumption of equal variance we need select a non-parametric equivalent test - Kruskal-Wallis test

### 4 - Final Hypothesis Test
KruskalResult(statistic=25360.27351786209, pvalue=0.0)

### 5 - Interpret your p-value and reject or fail to reject your null hypothesis

p < 0.05 - reject null hypothesis and support the alternate hypothesis there is a significant difference in popularity between genres.

### 6 - Show a supporting visualization that helps display the result

![image](https://user-images.githubusercontent.com/112773242/225713535-9dcd6837-53a3-4ba6-a54f-13cbf87af148.png)

Adventure, Animation and Fantasy were genres with higher popularity. Documentary was the genre with lower popularity.

Let's do Post-Hoc Multiple Comparison Test to see if these difference are significant

### Post-Hoc Multiple Comparison Test

![image](https://user-images.githubusercontent.com/112773242/225714192-993246a9-31d3-47b2-a2ed-5b1916ef7971.png)

Genres with similar x value = there is no difference.

# Summary 

- The MPAA rating of a movie does affect how much revenue the movie generates.
   - Movies that have an MPAA Rating of PG and PG-13 make the most revenue.

- The genre of a movie does affect how much revenue a movie generates.
   - Adventure, Fantasy, Animation, Action, Family and Science Fiction generate the greatest revenue.

- Some genres have higher popularity than others. 
  - Adventure, Animation and Fantasy have higher popularity. 

# Recomendation 
- In order to make a successfuk movie, it would be recommended to produce a PG,PG-13 rated and adventure movie. 







