# 📺 Sql-Netflix-project
![net](https://github.com/KrishnaBabu-Khethavath/Sql-Netflix-project/blob/main/logo%20(1).png)

## 📊 Overview
This project involves a comprehensive analysis of Netflix's movies and TV shows data using SQL. The goal is to extract valuable insights and answer various business questions based on the dataset. The following README provides a detailed account of the project's objectives, business problems, solutions, findings, and conclusions.

## 🎯 Objectives
- Analyze the distribution of content types (movies vs TV shows).
- Identify the most common ratings for movies and TV shows.
- List and analyze content based on release years, countries, and durations.
- Explore and categorize content based on specific criteria and keywords.

## 📁 Dataset : [Click Here](https://github.com/KrishnaBabu-Khethavath/Sql-Netflix-project/blob/main/netflix_titles.csv)

## 📜 Sql queries 👇

```sql
-- Easy Level Questions:

-- 1.How many movies are there on Netflix?
SELECT COUNT(*) as count
FROM netflix
WHERE type = 'Movie';

-- 2.How many TV shows are there on Netflix?
SELECT COUNT(*) as count
FROM netflix
WHERE type = 'TV Show';

-- 3.What is the total number of titles on Netflix?
SELECT COUNT(*) as total_titles
FROM netflix;

-- 4.List the top 5 genres listed on Netflix.
SELECT listed_in, COUNT(*) as count
FROM netflix
GROUP BY listed_in
ORDER BY count DESC
LIMIT 5;

-- 5.How many titles are rated 'TV-MA'?
SELECT COUNT(*) as count
FROM netflix
WHERE rating = 'TV-MA';

-- 6.List the top 3 countries by number of titles produced.
SELECT country, COUNT(*) as count
FROM netflix
WHERE country IS NOT NULL
GROUP BY country
ORDER BY count DESC
LIMIT 3;

-- 7.How many titles have the 'PG-13' rating?
SELECT COUNT(*) as count
FROM netflix
WHERE rating = 'PG-13';

-- 8.List the top 3 directors with the highest number of titles.
SELECT director, COUNT(*) as count
FROM netflix
WHERE director IS NOT NULL
GROUP BY director
ORDER BY count DESC
LIMIT 3;


-- Medium Level Questions:

-- 1.Count the number of titles released in each year.
SELECT release_year, COUNT(*) as count
FROM netflix
GROUP BY release_year
ORDER BY release_year;

-- 2.List the top 5 actors appearing in the most titles.
SELECT casts, COUNT(*) as count
FROM netflix
WHERE casts IS NOT NULL
GROUP BY casts
ORDER BY count DESC
LIMIT 5;

-- 3.Find titles with a duration of more than 2 hours.
SELECT title, duration
FROM netflix
WHERE duration LIKE '% min' AND CAST(SUBSTR(duration, 1, LENGTH(duration)-4) AS INTEGER) > 120;

-- 4.How many titles are listed in the genre 'Dramas'?
SELECT COUNT(*) as count
FROM netflix
WHERE listed_in LIKE '%Dramas%';

-- 5.List the top 3 titles with the highest release year.
SELECT title, release_year
FROM netflix
ORDER BY release_year DESC
LIMIT 3;

-- 6.Which directors have at least 10 titles on Netflix?
SELECT director, COUNT(*) as count
FROM netflix
WHERE director IS NOT NULL
GROUP BY director
HAVING COUNT(*) >= 10
ORDER BY count DESC;

-- 7.How many titles were added in December 2021?
SELECT COUNT(*) as count
FROM netflix
WHERE EXTRACT(YEAR FROM TO_DATE(date_added, 'MMMM DD, YYYY')) = 2021
AND EXTRACT(MONTH FROM TO_DATE(date_added, 'MMMM DD, YYYY')) = 12;

-- 8.Find the average duration of all movies.
SELECT AVG(CAST(SUBSTR(duration, 1, LENGTH(duration)-4) AS INTEGER)) as avg_duration
FROM netflix
WHERE type = 'Movie';


-- Hard Level Questions:

-- 1.Identify the correlation between release year and number of titles produced.
SELECT release_year, COUNT(*) as count
FROM netflix
GROUP BY release_year
ORDER BY release_year;

-- 2.Find the titles that have both 'Comedy' and 'Dramas' listed in their genres.
SELECT title
FROM netflix
WHERE listed_in LIKE '%Comedy%' AND listed_in LIKE '%Dramas%';

-- 3.Calculate the average release year for titles with 'PG-13' rating.
SELECT AVG(release_year) as avg_release_year
FROM netflix
WHERE rating = 'PG-13';

-- 4.Find the top 5 countries with the highest average rating score.
SELECT country, AVG(rating_score) as avg_rating_score
FROM (
    SELECT country, 
           CASE 
               WHEN rating = 'TV-MA' THEN 8
               WHEN rating = 'PG-13' THEN 7
               WHEN rating = 'R' THEN 6
               ELSE 5
           END as rating_score
    FROM netflix
) as rating_data
GROUP BY country
ORDER BY avg_rating_score DESC
LIMIT 5;

-- 5.Find titles that were released between 2010 and 2020 and have a rating of 'R'.
SELECT title, release_year, rating
FROM netflix
WHERE release_year BETWEEN 2010 AND 2020 AND rating = 'R';

-- 6.Find the number of titles that have been added in the past 6 months.
SELECT COUNT(*) as count
FROM netflix
WHERE TO_DATE(date_added, 'Month DD, YYYY') >= (CURRENT_DATE - INTERVAL '6 months');

-- 7.Find the titles with the most number of actors listed in their cast.
SELECT title, LENGTH(casts) - LENGTH(REPLACE(casts, ',', '')) + 1 as number_of_actors
FROM netflix
ORDER BY number_of_actors DESC
LIMIT 10;

-- 8.Calculate the percentage of movies and TV shows for each rating.
SELECT rating, 
       type, 
       COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (PARTITION BY type) as percentage
FROM netflix
GROUP BY rating, type
ORDER BY type, percentage DESC;
```
🏁 Conclusion
- Content Distribution: The dataset contains a diverse range of movies and TV shows with varying ratings and genres. 
- Common Ratings: Insights into the most common ratings provide an understanding of the content's target audience.
- Geographical Insights: The top countries and the average content releases by India highlight regional content distribution. 
- Content Categorization: Categorizing content based on specific keywords helps in understanding the nature of content available on Netflix.
