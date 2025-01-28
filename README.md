# IMDb Database Analysis Project

## Overview
This project involves querying and analyzing a relational database of IMDb movie and TV episode data. The database includes millions of rows, covering movie titles, genres, directors, actors, ratings, and more. The project explores various questions through SQL queries to uncover insights into movie production trends, dual actor/director contributions, and career trajectories of actors and actresses.

## Objectives
1. Analyze movie production trends for selected genres over a specific time period.
2. Identify actors who have also directed movies in a specific genre.
3. Study the career trajectory of a specific actor/actress.

---

## Tools and Technologies
- **DBMS**: Oracle DBMS (Accessed via Omega at UTA).
- **Language**: SQL for querying the database.

---

## Analyses

### **Analysis 1: Movie Production Trends (2000–2015)**
- **Query**:
  ```sql
  SELECT COUNT(TITLETYPE) AS Movies_produced, GENRES, STARTYEAR
  FROM sharmac.TITLE_BASICS
  WHERE GENRES IN ('Crime', 'Thriller', 'Mystery') 
    AND TITLETYPE = 'movie' 
    AND STARTYEAR BETWEEN '2000' AND '2015'
  GROUP BY GENRES, STARTYEAR
  ORDER BY STARTYEAR;

#### Objective: 
  Identify the number of movies produced for each year (2000–2015) for three genres: Crime, Thriller, and Mystery.
#### Key Insights:
- Thriller movies were produced at a higher rate compared to Crime and Mystery genres.
- Movie production increased consistently across all genres during the period.

### Analysis 2: Actors as Directors
- **Query**:
  ```sql
  SELECT COUNT(TITLE_BASICS.TITLETYPE), TITLE_BASICS.ORIGINALTITLE, TITLE_BASICS.STARTYEAR, NAME_BASICS.PRIMARYNAME
  FROM TITLE_CREW_DIR
  INNER JOIN NAME_BASICS ON NAME_BASICS.NCONST = TITLE_CREW_DIR.DIRECTORS
  INNER JOIN TITLE_PRINCIPALS ON TITLE_PRINCIPALS.TCONST = TITLE_CREW_DIR.TCONST
  INNER JOIN TITLE_BASICS ON TITLE_BASICS.TCONST = TITLE_CREW_DIR.TCONST
  WHERE TITLE_BASICS.GENRES LIKE '%Crime%' 
    AND TITLE_BASICS.TITLETYPE = 'movie'
  GROUP BY TITLE_BASICS.ORIGINALTITLE, TITLE_BASICS.STARTYEAR, NAME_BASICS.PRIMARYNAME;

#### Objective: 
  Identify actors who directed movies within the Crime genre.
#### Example Result:
  - Orson Welles directed and acted in several movies, including Citizen Kane and The Lady from Shanghai.

### Analysis 3: Career Trajectory of Emma Thompson
- **Query**:
  ```sql
  SELECT TITLE_BASICS.ORIGINALTITLE, TITLE_BASICS.STARTYEAR, COUNT(TITLE_BASICS.TITLETYPE)
  FROM TITLE_PRINCIPALS
  INNER JOIN NAME_BASICS ON NAME_BASICS.NCONST = TITLE_PRINCIPALS.NCONST
  INNER JOIN TITLE_BASICS ON TITLE_BASICS.TCONST = TITLE_PRINCIPALS.TCONST
  WHERE NAME_BASICS.NCONST = 'nm0000668' 
    AND TITLE_BASICS.TITLETYPE = 'movie'
  GROUP BY TITLE_BASICS.ORIGINALTITLE, TITLE_BASICS.STARTYEAR;

#### Objective: 
  Analyze the career of Emma Thompson based on her movie appearances.
#### Key Insights:
  - Emma Thompson consistently appeared in movies from 1989 to 2017.
  - Peaks in her career were observed in 2013 and 2016, with three movies released each year.
