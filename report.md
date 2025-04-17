# Holiday Movies Analysis

## Introduction
This project aims to explore and uncover insights about holiday-themed movies using the [TidyTuesday 2023-12-12 Holiday Movies](https://github.com/rfordatascience/tidytuesday/blob/main/data/2023/2023-12-12/readme.md) release. It includes information from the Internet Movie Database (IMDb) and specifically focuses on films with titles containing keywords such as "holiday", "Christmas", "Hanukkah", or "Kwanzaa", as well as their variants.

This data set is significant because holiday films are a unique genre that generally reflects cultural trends, seasonal demand by audiences, and production trends. Through the analysis of this data set, we could observe holiday films' evolution over time in their genre, quality (measured through ratings), and production numbers. However, there are certain issues with the data set. For instance, it only includes movies with specified keywords in their titles, potentially leaving out some suitable films. Furthermore, user-generated IMDb ratings can be prone to bias and at times fail to reflect the overall value of the film. With these limitations in mind, the dataset does offer an opportunity to study holiday movie trends and trends.

### Data Dictionary

`holiday_movies.csv`

|variable        |class     |description     |
|:---------------|:---------|:---------------|
|tconst          |character |alphanumeric unique identifier of the title |
|title_type      |character |the type/format of the title (movie, video, or tvMovie) |
|primary_title   |character |the more popular title / the title used by the filmmakers on promotional materials at the point of release |
|original_title  |character |original title, in the original language |
|year            |double    |the release year of a title |
|runtime_minutes |double    |primary runtime of the title, in minutes |
|genres          |character |includes up to three genres associated with the title (comma-delimited) |
|simple_title    |character |the title in lowercase, with punctuation removed, for easier filtering and grouping |
|average_rating  |double    |weighted average of all the individual user ratings on IMDb |
|num_votes       |double    |number of votes the title has received on IMDb (titles with fewer than 10 votes were not included in this dataset) |
|christmas       |logical   |whether the title includes "christmas", "xmas", "x mas", etc|
|hanukkah        |logical   |whether the title includes "hanukkah", "chanukah", etc|
|kwanzaa         |logical   |whether the title includes "kwanzaa"|
|holiday         |logical   |whether the title includes the word "holiday"|


## Q1: Do higher-rated movies tend to have longer runtimes?

The first question we will explore is whether higher-rated movies tend to have longer runtimes. To answer this question, we will create a scatter plot of average ratings against runtime minutes in the original holiday movies dataset, and add correlation coefficient between these two variables to quantify the relationship.


![scatter](scatter.png)

However, we can not see the clear relationship here. The correlation coefficient is -0.2, which is relatively close to 0 and can not indicates any relationship between 2 variables.

Hence, we continue by grouping the data by genres and add the label to the plot to see if there is any difference.

![scatter_color](scatter_color.png)

However, we still can not see the relationship, and the color somehow made us confused. But, this motivate us to use some other unsupervised methods to gain more information about the underlying relationship between the two variables given different genres.

![gmm](GMM.png)

We use Gaussian Mixture Model (GMM) to cluster the data into some clusters, each has normal distribution, and plot the results. We can see that some center of distribitions are overlap. This explain why the scatter plot by genres is confused and this also indicates that the two variables are not independent, it raise us a hypothesis that we can further evaluate the relationship if accessing each genres separately.


![regression](scatters_regression.png)

Hence, we create a scatter plot for each genre and add a regression line. Now the relationship is clearer because some lines have non-zero slopes. This indicates that the relationship between average ratings and runtime minutes is not consistent across all genres.

![corr](corr.png)

In a more qualitative evaluation, we can use bar plot to show the correlation coefficient for each genre. Clearly, from the chart, we can see that genres such as Thriller and Western show a strong positive correlation, meaning that longer runtime tends to correlate with higher ratings for these genres. In contrast, genres like History and War exhibit a negative correlation, suggesting that longer runtimes in these genres tend to be associated with lower ratings.

Finally, the answer is YES, but restricted to some genres.


### **Q2: Are filmmakers producing more holiday movies in genres with higher average ratings over time?**  

Here is a fully rewritten and structured answer to **Question 2**, following your requested format with no emojis, explicit code blocks, and complete analysis and discussion:

---

### **Question 2: Are filmmakers producing more holiday movies in genres with higher average ratings over time?**

#### **Introduction**

This question explores whether there is a relationship between audience reception, measured via IMDb average ratings, and the volume of holiday movie production within specific genres over time. The aim is to understand whether genres that receive higher ratings are subsequently favored in future film production.

To answer this, we analyze three key variables: `genres`, `average_rating`, and `year`. We preprocess the dataset to extract genre labels and group the data into 5-year periods. We are particularly interested in five prominent genres that appear frequently in the holiday dataset: Comedy, Romance, Animation, Family, and Drama.

However, it is critical to recognize the limitation of the dataset. This is not a full representation of the movie industry—it is a filtered sample that only includes movies with holiday-related keywords such as “holiday,” “Christmas,” “Hanukkah,” or “Kwanzaa” in their titles. As such, even if we detect patterns within this dataset, they do not necessarily generalize to all movie production behaviors. Film production is likely influenced by trends in the broader movie landscape, not just the holiday-themed subset.

---

#### **Approach**

To investigate the question, we use two different visualization methods:

**1. 100% Stacked Area Chart**

This chart shows the **proportion of holiday movies produced in each genre over time**, based on 5-year intervals. By normalizing the production counts per period, we can observe how each genre’s relative importance has shifted historically. This chart is particularly well-suited for detecting **structural shifts** in the composition of genre popularity over time.
![stacked-area](https://github.com/user-attachments/assets/245e26ca-ed32-4a5d-806f-87189b99af7e)

**Code Block:**
```python
genre_counts = df_exploded.groupby(['year_period', 'genres']).agg(
    count=('tconst', 'count')
).reset_index()

pivot_counts = genre_counts.pivot(index='year_period', columns='genres', values='count').fillna(0)
pivot_props = pivot_counts.div(pivot_counts.sum(axis=1), axis=0)

pivot_props.plot(kind='area', stacked=True, figsize=(14, 6), colormap='tab20')
plt.title("Proportional Holiday Movie Production by Genre Over Time")
plt.xlabel("5-Year Period")
plt.ylabel("Proportion of Movies")
plt.legend(loc='upper left', bbox_to_anchor=(1, 1), title='Genre')
plt.tight_layout()
plt.show()
```

**2. Hybrid Dual-Axis Line Charts**

For each of the five selected genres, we plot two series over time:
- Average IMDb rating on the **left Y-axis**
- Change in number of movies produced (rate of change) on the **right Y-axis**

This dual-axis approach allows us to explore whether spikes in rating precede or follow spikes in production volume. By separating these dimensions but sharing the X-axis (time), we can visually assess lagged or mirrored relationships.
![animation-line](https://github.com/user-attachments/assets/dffba9b4-9934-42d8-a326-ba2a527318e3)
![romance-line](https://github.com/user-attachments/assets/6c1b2aa0-fe9b-4c9c-96a8-0e023a7c9335)
![family-line](https://github.com/user-attachments/assets/a66d5419-1512-45d4-8097-6f4e6cbc9f87)
![drama-line](https://github.com/user-attachments/assets/b140bf2e-3a61-4eea-b9d8-c2f429c79345)
![comedy-line](https://github.com/user-attachments/assets/b40aae64-2bd5-4500-91ec-05d7e90367ee)

**Code Block:**
```python
agg = df_exploded[df_exploded['genres'].isin(focus_genres)].groupby(
    ['year_period', 'genres']
).agg(
    avg_rating=('average_rating', 'mean'),
    movie_count=('tconst', 'count')
).reset_index()

agg = agg.sort_values(['genres', 'year_period'])
agg['production_change'] = agg.groupby('genres')['movie_count'].diff()

for genre in focus_genres:
    data = agg[agg['genres'] == genre]

    fig, ax1 = plt.subplots(figsize=(10, 5))
    ax2 = ax1.twinx()

    ax1.plot(data['year_period'], data['avg_rating'], color='tab:red', marker='o', label='Avg Rating')
    ax2.plot(data['year_period'], data['production_change'], color='tab:blue', marker='s', label='Production Δ')

    ax1.set_xlabel('5-Year Period')
    ax1.set_ylabel('Average IMDb Rating', color='tab:red')
    ax2.set_ylabel('Change in Movie Count', color='tab:blue')
    plt.title(f'{genre} Holiday Movies: Rating vs. Production Change')
    ax1.tick_params(axis='x', rotation=45)

    lines1, labels1 = ax1.get_legend_handles_labels()
    lines2, labels2 = ax2.get_legend_handles_labels()
    ax1.legend(lines1 + lines2, labels1 + labels2, loc='upper left')

    plt.tight_layout()
    plt.show()
```

---

#### **Analysis**

The 100% stacked area chart reveals strong long-term shifts in genre representation within holiday films. Notably:
- Comedy consistently dominates the dataset from the 1940s onward.
- Romance becomes increasingly prominent, especially in the 2000s and 2010s.
- Animation and Family, although highly rated in many periods, exhibit more volatility or decline in proportional presence.

The hybrid dual-axis plots add context to these shifts:
- Comedy maintains high production volume even during periods of declining average rating, suggesting factors other than audience feedback are driving its continued popularity.
- Romance shows some alignment, where high average ratings are followed by an uptick in production.
- In contrast, Animation and Family genres show high ratings but inconsistent or even negative production growth, indicating other constraints such as production cost or limited seasonal demand.
- Drama is relatively stable in rating but declines in production, possibly reflecting shifting audience expectations or distribution strategies.

---

#### **Discussion**

Based on the visual and numerical analysis, we conclude that **there is no strong or consistent evidence** that higher average ratings lead to increased production of holiday-themed movies in a genre. While there are isolated cases (such as Romance) that suggest a possible connection, these patterns do not generalize across all genres or time periods.

Moreover, this conclusion must be tempered by the fact that the dataset is **not representative of all movies**. It only includes titles with holiday-related keywords, which are a **specific subset** of the broader film ecosystem. Filmmakers may very well respond to rating signals across the full population of movies, but our data does not capture that context. In fact, it is likely that production decisions are influenced more by trends across the industry—such as box office performance, platform strategies, and cultural trends—than by the limited scope of holiday content alone.

Therefore, while the question is conceptually meaningful, our data does **not provide sufficient evidence to support the hypothesis** that higher ratings consistently drive production volume within holiday-themed genres.

---
