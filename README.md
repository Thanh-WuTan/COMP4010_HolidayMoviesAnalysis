# Summer Movies Analysis  

## Project Overview  
This project is part of the **COMP4010: Data Visualization** course. The goal is to explore trends in summer movies using data from the [TidyTuesday](https://github.com/rfordatascience/tidytuesday) project. We focus on two key questions related to movie ratings, runtimes, and production trends over time.  

## Dataset Information  
The dataset is sourced from [2024-07-30 Summer Movies](https://github.com/rfordatascience/tidytuesday/blob/main/data/2024/2024-07-30/readme.md) release. It contains information about movies with "summer" in their title, derived from the [Internet Movie Database (IMDb)](https://developer.imdb.com/non-commercial-datasets/).

### Data Dictionary
`summer_movies.csv`

|variable        |class     |description     |
|:---------------|:---------|:---------------|
|tconst          |character |alphanumeric unique identifier of the title |
|title_type      |character |the type/format of the title (movie, video, or tvMovie) |
|primary_title   |character |the more popular title / the title used by the filmmakers on promotional materials at the point of release |
|original_title  |character |original title, in the original language |
|year            |integer   |the release year of a title |
|runtime_minutes |integer   |primary runtime of the title, in minutes |
|genres          |character |includes up to three genres associated with the title (comma-delimited)  |
|simple_title    |character |the title in lowercase, with punctuation removed, for easier filtering and grouping |
|average_rating  |double    |weighted average of all the individual user ratings on IMDb |
|num_votes       |integer   |number of votes the title has received on IMDb (titles with fewer than 10 votes were not included in this dataset) |

### Why This Dataset?  
We chose this dataset because:
- It allows interesting trend analysis in film quality and production choices. 
- It contains both numeric and category variables, making it suitable for visualization.  
- It offers real-world relevance, as IMDb ratings reflect audience preferences.  

## Research Questions 

### **Q1: Do higher-rated summer movies tend to have longer runtimes?**  

- **Variables Involved:**  
  - `average_rating` (IMDb user rating) – **Numerical**  
  - `runtime_minutes` (Movie duration) – **Numerical**  
  - `genres` (Movie genre) – **Categorical**  

- **Analysis Plan:**  
  - Create a **scatterplot** of `average_rating` vs. `runtime_minutes`.  
  - Use **color** to analyze differences across genres.  
  - Compute a **correlation coefficient** to measure the strength of the relationship.

### **Q2: Are filmmakers producing more summer movies in genres with higher average ratings over time?**  

- **Variables Involved:**  
  - `year` (Release year) – **Numerical**  
  - `genres` (Movie genre) – **Categorical**  
  - `average_rating` (IMDb rating) – **Numerical**  

- **Analysis Plan:**  
  - Compute **average IMDb rating per genre per year**.  
  - Count the **number of movies released per genre per year**.  
  - Compare trends using **two separate line charts**:  
    - A **line chart** showing the number of movies per genre over time.  
    - A **line chart** showing the average rating per genre over time.    
