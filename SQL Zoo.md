# SQL Zoo

1. Select from Word
    1. SELECT name, continent, population FROM world
    2. SELECT name
    FROM world
    WHERE population > 200000000
    3. Select name, gdp/population
    From world
    Where population > 200000000
    4. Select name, population/1000000
    From world
    Where continent = 'South America'
    5. Select name, population
    From world
    Where name in ('France', 'Germany', 'Italy')
    6. Select name
    From world
    Where name Like '%United %'
    7. Select name, population, area
    From world
    Where area > 3000000 Or population > 250000000
    8. Select name, population, area
    From world
    Where (area > 3000000 And population < 250000000) Or  (area < 3000000 And population > 250000000)
    9. Select name, Round(population/1000000, 2), Round(gdp/1000000000, 2)
    From world
    Where continent = 'South America'
    10. Select name, Round(gdp/population/1000,0)*1000
    From world
    Where gdp > 1000000000000
    11. SELECT name, capital
    FROM world
    WHERE LEN(name) = LEN(capital)
    12. SELECT name, capital
    FROM world
    Where Left(name,1) = Left(capital,1) And name <> capital
    13. SELECT name
    FROM world
    WHERE name Like '%a%' And name Like '%e%' And name Like '%i%' And name Like '%o%' And name Like '%u%' And name Not Like '% %'
2. Select from nobel
    1. Select yr, subject, winner
    From nobel
    Where yr = 1950
    2. Select winner
    From nobel
    Where yr = 1962 And subject = 'Literature'
    3. Select yr, subject
    From nobel
    Where winner = 'Albert Einstein'
    4. Select winner
    From nobel
    Where subject = 'Peace' And yr >= 2000
    5. Select *
    From nobel
    Where subject = 'Literature' And yr Between 1980 And 1989
    6. Select *
    From nobel
    Where winner in ('Theodore Roosevelt', 'Woodrow Wilson', 'Jimmy Carter', 'Barack Obama')
    7. Select winner
    From nobel
    Where winner Like 'John%'
    8. Select yr, subject, winner
    From nobel
    Where (subject = 'Physics' And yr = 1980) Or (subject = 'Chemistry' And yr = 1984)
    9. Select yr, subject, winner
    From nobel
    Where yr = 1980 And subject Not In ('Chemistry', 'Medicine')
    10. Select yr, subject, winner
    From nobel
    Where (subject = 'Medicine' And yr < 1910) Or (subject = 'Literature' And yr >= 2004)
    11. Select yr, subject, winner
    From nobel
    Where winner = 'PETER GRÜNBERG'
    12. Select *
    From nobel
    Where winner = 'EUGENE O''NEILL'
    13. Select winner, yr, subject
    From nobel
    Where winner Like 'Sir%'
    Order by yr Desc, winner
    14. Select winner, subject From nobel 
    Where yr=1984 
    Order By subject IN ('Physics','Chemistry'),subject,winner
3. Select from SELECT
    1. Select name
    From world
    Where population (Select population
    From world
    Where name = 'Russia')
    2. Select name
    From world
    Where continent = 'Europe'
    And gdp/population >
    (Select gdp/population From world Where name = 'United Kingdom')
    3. Select name, continent
    From world
    Where continent In
    (Select Distinct continent
    From world
    Where name in ('Argentina', 'Australia'))
    4. Select name, population
    From world
    Where population >
    (Select population From world Where name = 'Canada')
    And population <
    (Select population From world Where name = 'Poland')
    5. SELECT
    name,
    CONCAT(ROUND((population*100)/(SELECT population
    FROM world WHERE name='Germany'), 0), '%')
    FROM world
    WHERE population IN (SELECT population
    FROM world
    WHERE continent='Europe')
    6. Select name
    From world
    Where gdp > All(Select gdp From world Where continent = 'Europe' And gdp > 0)
    7. SELECT continent, name, area FROM world x
    WHERE area >= ALL
    (SELECT area FROM world y
    WHERE y.continent=x.continent
    AND area>0)
    8. Select continent, name
    From world a
    Where name <=
    All (Select name From world b Where a.continent = b.continent)
    9. Select name, continent, population
    From world
    Where continent In
    (Select continent From world a Where 25000000 > All(Select population From world b Where a.continent = b.continent))
    10. Select name, continent
    From world a
    Where population / 3 >
    All(Select population From world b Where a.continent = b.continent And [a.name](http://a.name/) <> [b.name](http://b.name/))
4. Sum and Count
    1. SELECT SUM(population)
    FROM world
    2. Select continent
    From world
    Group By continent
    3. Select Sum(gdp)
    From world
    Where continent = 'Africa'
    4. Select Count(name)
    From world
    Where area >= 1000000
    5. Select Sum(population)
    From world
    Where name in ('Estonia', 'Latvia', 'Lithuania')
    6. Select continent, Count(name)
    From world
    Group By continent
    7. Select continent, Count(name)
    From world
    Where population >= 10000000
    Group By continent
    8. Select continent
    From world
    Group By continent
    Having Sum(population) > 100000000
5. JOIN
    1. SELECT matchid, player FROM goal
    WHERE teamid = 'GER'
    2. SELECT id,stadium,team1,team2
    FROM game
    Where id = 1012
    3. SELECT player, teamid, stadium, mdate
    FROM game JOIN goal ON (id=matchid)
    Where teamid = 'GER'
    4. SELECT team1, team2, player
    FROM game JOIN goal ON (id=matchid)
    Where player Like 'Mario%'
    5. SELECT player, teamid, gtime
    FROM goal
    WHERE gtime<=10
    6. Select mdate, teamname
    From game JOIN eteam
    On team1 = [eteam.id](http://eteam.id/)
    Where coach = 'Fernando Santos'
    7. Select player
    From goal Join game
    On goal.matchid = [game.id](http://game.id/)
    Where stadium = 'National Stadium, Warsaw'
    8. SELECT Distinct player
    FROM game JOIN goal ON matchid = id
    WHERE teamid <> 'GER' And (team1='GER' Or team2='GRE')
    9. SELECT teamname, count(player)
    FROM eteam JOIN goal ON id=teamid
    Group By teamname
    ORDER BY teamname
    10. Select stadium, Count(player)
    From game Join goal
    On [game.id](http://game.id/) = goal.matchid
    Group By stadium
    11. SELECT matchid,mdate, Count(player)
    FROM game JOIN goal ON matchid = id
    Where (team1 = 'POL' OR team2 = 'POL')
    Group By matchid, mdate
    12. Select matchid, mdate, Count(player)
    From game Join goal
    On matchid = id
    Where teamid = 'GER'
    Group By matchid, mdate
    13. SELECT mdate,
    team1,
    Sum(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,
    team2,
    Sum(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) score2
    FROM game LEFT JOIN goal ON matchid = id
    Group BY mdate, team1, team2
6. More JOIN
    1. SELECT id, title
    FROM movie
    WHERE yr=1962
    2. Select yr
    From movie
    Where title = 'Citizen Kane'
    3. Select id, title, yr
    From movie
    Where title Like '%Star Trek%'
    Order By yr
    4. Select id
    From actor
    Where name = 'Glenn Close'
    5. Select id
    From movie
    Where title = 'Casablanca'
    6. select name from actor a
    join casting b on a.id=b.actorid
    where b.movieid=11768
    7. select name from actor a
    join casting b on a.id=b.actorid
    join movie c on [b.movieid=c.id](http://b.movieid=c.id/)
    where title='Alien'
    8. Select title
    From movie
    Join casting on [movie.id](http://movie.id/) = casting.movieid
    Join actor on [actor.id](http://actor.id/) = casting.actorid
    Where name = 'Harrison Ford'
    9. Select title
    From movie
    Join casting on [movie.id](http://movie.id/) = casting.movieid
    Join actor on [actor.id](http://actor.id/) = casting.actorid
    Where name = 'Harrison Ford' And ord <> 1
    10. Select title, name
    From movie
    Join casting on [movie.id](http://movie.id/) = casting.movieid
    Join actor on [actor.id](http://actor.id/) = casting.actorid
    Where yr = 1962 And ord = 1
    11. SELECT yr,COUNT(title) FROM
    movie JOIN casting ON movie.id=movieid
    JOIN actor   ON [actorid=actor.id](http://actorid=actor.id/)
    WHERE name='Rock Hudson'
    GROUP BY yr
    HAVING COUNT(title) > 2
    12. Select title, name
    From movie
    Join casting on [movie.id](http://movie.id/) = casting.movieid
    Join actor on [actor.id](http://actor.id/) = casting.actorid
    Where movieid In
    (Select movieid from casting a join actor b on a.actorid = [b.id](http://b.id/) Where [b.name](http://b.name/) = 'Julie Andrews')
    And ord = 1
    13. Select name
    From movie
    Join casting on [movie.id](http://movie.id/) = casting.movieid
    Join actor on [actor.id](http://actor.id/) = casting.actorid
    Where ord = 1
    Group By name
    Having Count(title) >= 15
    14. Select title, Count(*)
    From movie
    Join casting on [movie.id](http://movie.id/) = casting.movieid
    Join actor on [actor.id](http://actor.id/) = casting.actorid
    Where yr = 1978
    Group By title
    Order by Count(**) desc, title
    15. Select name
    From movie
    Join casting on [movie.id](http://movie.id/) = casting.movieid
    Join actor on [actor.id](http://actor.id/) = casting.actorid
    Where movieid in
    (Select movieid
    From movie
    Join casting on [movie.id](http://movie.id/) = casting.movieid
    Join actor on [actor.id](http://actor.id/) = casting.actorid
7. Using NULL
    1. Select name
    From teacher
    Where dept Is NULL
    2. SELECT [teacher.name](http://teacher.name/), [dept.name](http://dept.name/)
    FROM teacher INNER JOIN dept
    ON ([teacher.dept=dept.id](http://teacher.dept=dept.id/))
    3. SELECT [teacher.name](http://teacher.name/), [dept.name](http://dept.name/)
    FROM teacher Left JOIN dept
    ON ([teacher.dept=dept.id](http://teacher.dept=dept.id/))
    4. SELECT [teacher.name](http://teacher.name/), [dept.name](http://dept.name/)
    FROM teacher Right JOIN dept
    ON ([teacher.dept=dept.id](http://teacher.dept=dept.id/))
    5. Select name, coalesce(mobile, '07986 444 2266')
    From teacher
    6. Select [teacher.name](http://teacher.name/), Coalesce([dept.name](http://dept.name/), 'None')
    From teacher Left Join dept
    On teacher.dept = [dept.id](http://dept.id/)
    7. Select Count(name), Count(mobile)
    From teacher
    8. Select [dept.name](http://dept.name/), Count([teacher.name](http://teacher.name/))
    From teacher Right Join dept
    On [dept.id](http://dept.id/) = teacher.dept
    Group By [dept.name](http://dept.name/)
    9. Select name, Case When dept = 1 Then 'Sci' Else 'Art' End
    From teacher
    10. Select name, Case When dept = 1 Then 'Sci' When dept = 2 Then 'Art' Else 'None'
    End
    From teacher
8. Self Join
    1. Select count(*)
    From stops
    2. Select id
    From stops
    Where name =  'Craiglockhart'
    3. Select [stops.id](http://stops.id/), [stops.name](http://stops.name/)
    From stops Join route On [stops.id](http://stops.id/) = route.stop
    Where route.num = '4' And route.company = 'LRT'
    4. SELECT company, num, COUNT(*) FROM route
    WHERE stop in (149,53)
    GROUP BY company, num
    having COUNT(**)=2
    5. SELECT a.company, a.num, a.stop, b.stop
    FROM route a JOIN route b ON
    (a.company=b.company AND a.num=b.num)
    WHERE a.stop=53 And b.stop = 149
    6. SELECT a.company, a.num, [stopa.name](http://stopa.name/), [stopb.name](http://stopb.name/)
    FROM route a JOIN route b ON
    (a.company=b.company AND a.num=b.num)
    JOIN stops stopa ON ([a.stop=stopa.id](http://a.stop=stopa.id/))
    JOIN stops stopb ON ([b.stop=stopb.id](http://b.stop=stopb.id/))
    WHERE stopa.name='Craiglockhart' and [stopb.name](http://stopb.name/) = 'London Road'
    7. SELECT distinct a.company, a.num FROM route a
    JOIN route b ON (a.company=b.company AND a.num=b.num)
    WHERE a.stop=115 and b.stop=137
    8. SELECT a.company, a.num
    FROM route a JOIN route b ON
    (a.company=b.company AND a.num=b.num)
    JOIN stops stopa ON ([a.stop=stopa.id](http://a.stop=stopa.id/))
    JOIN stops stopb ON ([b.stop=stopb.id](http://b.stop=stopb.id/))
    WHERE stopa.name='Craiglockhart' and [stopb.name](http://stopb.name/) = 'Tollcross'
    9. Select [stops.name](http://stops.name/), route.company, route.num
    From route Join stops On (route.stop = [stops.id](http://stops.id/))
    Where num in
    (Select Distinct num From route Join stops On (route.stop = [stops.id](http://stops.id/)) Where [stops.name](http://stops.name/) = 'Craiglockhart')
    And route.company = 'LRT'
    Order By [stops.name](http://stops.name/)
    10. select bus1.num, bus1.company, name, bus2.num, bus2.company from
    (select start1.num, start1.company, stop1.stop from route start1
    join route stop1 on start1.num=stop1.num and start1.company=stop1.company and start1.stop!=stop1.stop and start1.stop=(select id from stops where name='Craiglockhart')) bus1
    join (select start2.num, start2.company, start2.stop from route start2
    join route stop2 on start2.num=stop2.num and start2.company=stop2.company and start2.stop!=stop2.stop and stop2.stop=(select id from stops where name='Lochend')) bus2
    on bus1.stop=bus2.stop
    join stops on stops.id=bus1.stop