
# Movie Dataset Analysis

This project explores a Movie Dataset containing information on 10,000 movies collected from The Movie Database (TMDb). The dataset includes various attributes such as user ratings, revenue, cast, and genres. It also has inflation-adjusted financial metrics for accurate analysis of movie budgets and revenues in 2010 dollars.

## Features

- Dataset Overview
- Key Questions to Explore
- Data Wrangling
- Exploratory Data Analysis (EDA)
- Conclusion
- Dependencies
- How to Run


## Dataset Overview


- Rows: 10,866
- Columns: 21
- Source: TMDb
- Some columns contain multiple values separated by a pipe (|) character (e.g., cast, genres).
- Minor inconsistencies exist in the cast column, but no additional cleaning is necessary.
- Financial columns ending with _adj represent inflation-adjusted values.


## Key Questions to Explore

1. Does a higher budget lead to higher popularity? Is there a correlation?
2. How does runtime influence vote count and popularity?
3. Does higher popularity guarantee higher profits?
4. What features are associated with the Top 10 Revenue Movies?
5. Which genres are the most popular across different years?

## Data Wrangling

This section involves cleaning and preparing the dataset for analysis.

### Steps:
1. Remove duplicate rows and unnecessary columns (e.g., imdb_id, homepage).
2. Handle missing data:
  - Fill missing values in object columns with "missing".
  - Replace 0 values in budget with NaN for better financial analysis.
  - Check data types to ensure accuracy.
```bash
# Load the dataset
df = pd.read_csv('tmdb-movies.csv')

# Drop unhelpful columns
df.drop(['id', 'imdb_id', 'homepage', 'overview'], axis=1, inplace=True)

# Handle missing values
df['cast'].fillna('missing', inplace=True)
df['budget'] = df['budget'].replace(0, np.NaN)

# Remove duplicate rows
df.drop_duplicates(inplace=True)

```

## Exploratory Data Analysis (EDA)
###  1. Does Higher Budget Mean Higher Popularity?
A scatter plot shows no strong correlation between budget and popularity. However, grouping the movies by low and high budgets reveals that higher-budget movies are 55.5% more likely to be popular.

```
# Group by budget median
median_budget = df['budget'].median()
high_budget = df.query('budget >= @median_budget')
low_budget = df.query('budget < @median_budget')

# Calculate mean popularity
mean_high = high_budget['popularity'].mean()
mean_low = low_budget['popularity'].mean()

# Bar plot
plt.bar(['Low', 'High'], [mean_low, mean_high])
plt.title('Average Popularity by Budget')
plt.xlabel('Budget')
plt.ylabel('Popularity')
```
### Conclusion: Higher-budget movies tend to be more popular.

### 2. How Does Runtime Affect Popularity?
Movies are grouped by runtime:

Short (<100 min)
Medium (100-200 min)
Long (>200 min)
Results show that medium-length movies tend to have higher popularity.

```
# Group movies by runtime
short = df.query('runtime < 100')
medium = df.query('runtime <= 200')
long = df.query('runtime > 200')

# Calculate mean popularity
mean_pop_short = short['popularity'].mean()
mean_pop_medium = medium['popularity'].mean()
mean_pop_long = long['popularity'].mean()

# Bar plot
plt.bar(['Short', 'Medium', 'Long'], [mean_pop_short, mean_pop_medium, mean_pop_long])
plt.title('Popularity by Runtime')
plt.xlabel('Runtime')
plt.ylabel('Popularity')
```
### Conclusion: Movies with a runtime between 100-200 minutes are more likely to be popular.

### 3. Does Higher Popularity Mean Higher Profits?
To explore this, we created a profit column by subtracting the budget from revenue. Results indicate that popular movies generate significantly higher profits.

```
# Create profit column
df['profit'] = df['revenue'] - df['budget']

# Group by popularity median
median_popularity = df['popularity'].median()
low_pop = df.query('popularity < @median_popularity')
high_pop = df.query('popularity >= @median_popularity')

# Calculate mean profit
mean_profit_low = low_pop['profit'].mean()
mean_profit_high = high_pop['profit'].mean()

# Bar plot
plt.bar(['Low', 'High'], [mean_profit_low, mean_profit_high])
plt.title('Profit by Popularity')
plt.xlabel('Popularity')
plt.ylabel('Profit')
```
### Conclusion: Higher popularity correlates with higher profits.

### 4. Features of the Top 10 Revenue Movies
The top 10 highest-grossing movies are visualized using histograms to identify common characteristics.

```
top10_revenue = df.nlargest(10, 'revenue')
top10_revenue.hist(figsize=(12,12))
```
### Conclusion: These movies tend to have higher budgets, longer runtimes, and higher vote counts.

### 5. Which Genres Are Most Popular Year by Year?
Using a grouped analysis by year and genre, we explore how genre popularity trends have changed over time. (This section can be further expanded with visualizations like heatmaps.)

## Conclusion
This analysis demonstrates key insights into how budget, runtime, popularity, and profits interact in the movie industry. High-budget movies are generally more popular, and popularity is a strong indicator of profitability. Additionally, medium-length movies tend to perform better in terms of popularity.

## Dependencies
Make sure the following libraries are installed before running the code:

```
pip install pandas matplotlib seaborn numpy
```
## How to Run
1. Clone this repository.
2. Ensure the tmdb-movies.csv file is in the same directory.
3. Run the Jupyter notebook or Python script for detailed insights and visualizations.
