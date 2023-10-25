# Football Matches - Tasks

Each of the questions/tasks below can be answered using a `SELECT` query. When you find a solution copy it into the code block under the question before pushing your solution to GitHub.

1) Find all the matches from 2017.

```sql
SELECT * FROM Matches WHERE season = 2017;
```

2) Find all the matches featuring Barcelona.

```sql
SELECT * FROM Matches WHERE hometeam = 'Barcelona' OR awayteam = 'Barcelona';
```

3) What are the names of the Scottish divisions included?

```sql
SELECT DISTINCT name FROM Divisions WHERE country = 'Scotland';
```

4) Find the value of the `code` for the `Bundesliga` division. Use that code to find out how many matches Freiburg have played in that division. HINT: You will need to query both tables

```sql
SELECT code 
FROM Divisions 
WHERE name = 'Bundesliga';

SELECT COUNT(*) 
FROM matches 
WHERE (hometeam = 'Freiburg' OR awayteam = 'Freiburg') 
AND division_code = 'D1';
```

5)  Find the teams which include the word "City" in their name. HINT: Not every team has been entered into the database with their full name, eg. `Norwich City` are listed as `Norwich`. If your query is correct it should return four teams.

```sql
SELECT DISTINCT hometeam
FROM Matches
WHERE hometeam LIKE '%City%'
UNION
SELECT DISTINCT awayteam
FROM Matches
WHERE awayteam LIKE '%City%';

```

6) How many different teams have played in matches recorded in a French division?

```sql
SELECT COUNT(DISTINCT team_name)
FROM (
    SELECT hometeam AS team_name
    FROM Matches
    WHERE division_code IN (
        SELECT code
        FROM Divisions
        WHERE country = 'France'
    )
    UNION
    SELECT awayteam AS team_name
    FROM Matches
    WHERE division_code IN (
        SELECT code
        FROM Divisions
        WHERE country = 'France'
    )
) teams_in_french_division;


```

7) Have Huddersfield played Swansea in any of the recorded matches?

```sql
SELECT COUNT(*)
FROM Matches
WHERE 
  (hometeam = 'Huddersfield' AND awayteam = 'Swansea')
  OR
  (hometeam = 'Swansea' AND awayteam = 'Huddersfield');
```

8) How many draws were there in the `Eredivisie` between 2010 and 2015?

```sql
SELECT COUNT(*) 
FROM Matches
WHERE division_code = 'N1'
  AND season >= 2010
  AND season <= 2015
  AND ftr = 'D';
```

9) Select the matches played in the Premier League in order of total goals scored (`fthg` + `ftag`) from highest to lowest. When two matches have the same total the match with more home goals (`fthg`) should come first. 

```sql
SELECT * FROM Matches 
WHERE division_code = 'E0'
ORDER BY (fthg + ftag) DESC;
```

10) Find the name of the division in which the most goals were scored in a single season and the year in which it happened.

```sql
SELECT
    (SELECT name FROM Divisions WHERE code = Matches.division_code) AS division_name,
    Matches.season,
    SUM(Matches.fthg + Matches.ftag) AS total_goals
FROM Matches
GROUP BY Matches.division_code, Matches.season
ORDER BY total_goals DESC
LIMIT 1;

```

### Useful Resources

- [Filtering results](https://www.w3schools.com/sql/sql_where.asp)
- [Ordering results](https://www.w3schools.com/sql/sql_orderby.asp)
- [Grouping results](https://www.w3schools.com/sql/sql_groupby.asp)