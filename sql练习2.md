#[SELECT basics](http://sqlzoo.net/wiki/SELECT_basics)

##1.The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes';
Modify it to show the population of Germany
```sql
SELECT population FROM world
  WHERE name = 'Germany'
```

##2 Checking a list The word IN allows us to check if an item is in a list. The example shows the name and population for the countries 'Luxembourg', 'Mauritius' and 'Samoa'.
Show the name and the population for 'Ireland', 'Iceland' and 'Denmark'.
```sql
SELECT name, population FROM world
  WHERE name IN ('Ireland', 'Iceland', 'Denmark');
```

##3 Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km. Modify it to show the country and the area for countries with an area between 200,000 and 250,000.
```sql
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
```

#[SELECT from WORLD Tutorial](http://sqlzoo.net/wiki/SELECT_from_WORLD_Tutorial)

##2
How to use WHERE to filter records. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.
```sql
SELECT name FROM world
WHERE population>200000000
```

##3
Give the name and the per capita GDP for those countries with a population of at least 200 million.
HELP:How to calculate per capita GDP
```sql
select name ,GDP/population  from world 
where population>200000000
```

##4 
Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.
```sql
 select name,population/1000000 from world
where continent='South America' 
```

##5
Show the name and population for France, Germany, Italy
```sql
select name,population from world
where name in ('France','Germany','Italy')
```

##6
Show the countries which have a name that includes the word 'United'
```sql
select name from world
where name like '%United%'
```

##7
Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.
Show the countries that are big by area or big by population. Show name, population and area.
```sql
select name,population,area from world where population >250000000
or area>3000000
```

##8.
Exclusive OR (XOR). Show the countries that are big by area or big by population but not both. Show name, population and area. 
or 和 xor 的区别
```sql
select name,population,area from world where population >250000000
xor area>3000000
```

##9
Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places.
For South America show population in millions and GDP in billions both to 2 decimal places.
Millions and billions
科学缩写法1e6,保留几位小数round
```sql
select name,round(population/(1e6),2),round(gdp/(1e9),2) from world
where continent='South America'
```
##10
Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.
Show per-capita GDP for the trillion dollar countries to the nearest $1000.
想法：人均gdp，在千位四舍五入 
round() 正参数在小数点后四舍五入，负参数代表小数前四舍五入
向上取整为ceiling，向下取整是floor 
不过貌似不能定义取整的位置
```sql
select name,round(gdp/population,-3) per_gdp  from world
where gdp>1e12
```

##11
The CASE statement shown is used to substitute North America for Caribbean in the third column.
Show the name - but substitute Australasia for Oceania - for countries beginning with N.
思想：原表是男女可以替换为01 或者原表是01然后替换为男女。或者用来按某种方式分组 比如各国属于的大洲
解法：在以n开头的国家中，大陆属性为Oceania替换为Australasia
```sql
SELECT name,
       CASE WHEN continent='Oceania' THEN 'Australasia'
            ELSE continent END
  FROM world
 WHERE name LIKE 'n%'
```

##12
Show the name and the continent - but substitute Eurasia for Europe and Asia; substitute America - for each country in North America or South America or Caribbean. Show countries beginning with A or B
解法：同样的替换，升级为多对1.可以使用in关键字
```sql
select name,case 
                 when continent in ('Europe' ,'Asia') then 'Eurasia'                       
                 when continent in ( 'North America' , 'South America','Caribbean') then 'America'                 
                 else continent end
from world
where name like 'a%' xor name like 'b%'
```

##13
Put the continents right...
Oceania becomes Australasia
Countries in Eurasia and Turkey go to Europe/Asia
Caribbean islands starting with 'B' go to North America, other Caribbean islands go to South America
Order by country name in ascending order
Test your query using the WHERE clause with the following
解题：一行when中同样可以使用多个条件
```sql
select name,continent,case
  when continent ='Oceania' then 'Australasia'
  when continent = 'Eurasia' or name='Turkey' then 'Europe/Asia'
  when continent ='Caribbean' and name like 'b%' then 'North America'
  when continent ='Caribbean' then  'South America'
  else continent end 
from world
WHERE tld IN ('.ag','.ba','.bb','.ca','.cn','.nz','.ru','.tr','.uk')
order by name asc
```

#[SELECT from Nobel Tutorial](http://sqlzoo.net/wiki/SELECT_from_Nobel_Tutorial)

##1.
Change the query shown so that it displays Nobel prizes for 1950.
```sql
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```

##2.
Show who won the 1962 prize for Literature.
```sql
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'
```

##3.
Show the year and subject that won 'Albert Einstein' his prize.
```sql
select yr ,subject from  nobel 
where winner='Albert Einstein'
```

##4.
Give the name of the 'Peace' winners since the year 2000, including 2000.
```sql
select winner from nobel
where subject='Peace' and yr>='2000'
```

##5.
Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.
```sql
select * from nobel
where  subject='Literature' and yr between 1980 and 1989
```

##6.
Show all details of the presidential winners:
Theodore Roosevelt
Woodrow Wilson
Jimmy Carter
```sql
select  * from nobel
where winner in ('Theodore Roosevelt','Woodrow Wilson','jimmy Carter')
```

##7.
Show the winners with first name John
```sql
select winner from nobel
where winner like 'john%'
```

##8.
Show the Physics winners for 1980 together with the Chemistry winners for 1984.
```sql
select * from nobel where yr='1980' and subject ='Physics'
union
select * from nobel where yr='1984' and subject ='Chemistry'
```

##9.
Show the winners for 1980 excluding the Chemistry and Medicine
```sql
select * from nobel
where yr='1980' and subject not in ('Chemistry','Medicine')
```

##10.
Show who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)
```sql
select * from nobel where subject ='Medicine' and yr<1910
union
select * from nobel where subject ='Literature' and yr>=2004
```

##11.
Find all details of the prize won by PETER GRÜNBERG
Non-ASCII characters
解题：貌似有变音符也能正确识别出来。so这题的意义不明
```sql
select * from nobel 
where winner ='PETER GRÜNBERG'
```

##12.
Find all details of the prize won by EUGENE O'NEILL
Escaping single quotes
解题：转义符号\的使用
```sql
select * from nobel 
where winner ='EUGENE O\'NEILL'
```

##13.
Knights in order
List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.
```sql
select winner, yr, subject from nobel 
where winner like 'Sir%'
order by yr desc,winner asc
```

##14.
The expression subject IN ('Chemistry','Physics') can be used as a value - it will be 0 or 1.
Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.
解题：想按suject和winner顺序排列，但是化学和物理要排最后。所以，最优先级是化学和物理  然后才是subject和winner
所以先把化学和物理给筛选出来 使用了in关键字 化学和物理转为1 其他为0
```sql
SELECT winner, subject
  FROM nobel
 WHERE yr=1984
 ORDER BY subject in ('Chemistry','Physics'),subject,winner
```

#[SELECT within SELECT Tutorial](http://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial)

##1.
List each country name where the population is larger than that of 'Russia'.
解题：这里没有共同的属性，比如同一个大陆。所以简单的使用一个独立的子查询即可。
有共同的属性才使用有连接的子查询
```sql
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='russia')
```

##2.
Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
```sql
select name from world 
where  continent='Europe'  and  gdp/population >
(select gdp/population from world where name='United Kingdom' )
```

##3.
List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.
```sql
select name,continent from world 
where continent in 
(select continent from world where name in ('Argentina','Australia'))
order by name 
```

##4.
Which country has a population that is more than Canada but less than Poland? Show the name and the population.
```sql
select name,population from world  
where population > (select population from world where name='Canada' )
and population <  (select population from world where name='Poland' )
```

##5.
Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.
Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
解题：用来展示人口百分比 concat进行字符串拼接
疑问：能不能专门弄一个变量来存储德国人口这个固定值
```sql
select name,concat(round(100*population/(select population from world where name='Germany')),'%')  from world 
where continent ='Europe'
```
的确可以定义一个变量来存储之前查询的值，方便下次重复使用
```sql
set @p=(select population from world where name='Germany');

select name,concat(round(100*population/@p),'%')  from world 
where continent ='Europe'
```
##6.
Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)
1聚焦函数对null的处理
`avg，sum，max和min都会忽略null值`
count(\*)计算行数（不忽略null），count(属性名)忽略null
```sql
 select name from world
 where gdp >(select max(gdp) from world where continent ='Europe' )
```

##7.
Find the largest country (by area) in each continent, show the continent, the name and the area:
```sql
select continent,name,area from world w1
where area = (select max(area) from world w2 where w1.continent =w2.continent)
```

##8.
List each continent and the name of the country that comes first alphabetically.
1 题目意思：每一个大洲中按首字母排序的第一个国家
1 解题：min和max函数也可用于字符串，首字符按a-z从小到大 min会取出首字符最小的
```sql
select continent ,min(name)  from world
group by continent 
```

##9.
Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population.
```sql
select name,continent,population from world where continent in
(select continent from world 
group by continent having max(population)<=25e6)
```
直观逻辑的东西可能运行比较慢
如果看做一行一行筛选，如果出现子列满足某条件的，就考虑成立
exist看做通过子列的count来判断是否通过筛选
反正怎么样都逃不开w1.continent = w2.continent
```sql
select name,continent ,population from world w1
where not exists  (select * from world w2 
                                       where w1.continent = w2.continent 
                                         and w2.population >25e6)
-- 或者使用all或者max                                        
select name ,continent ,population from world w1 
where 25e6 >=all(select population from world w2 
                                            where w2.continent = w1.continent ) 
```
**把sql看做一行一行的筛选，where后面的条件判断只会返回0或1来判断单行是否通过筛选**

##10.
Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.
解题：比相同洲的其他国家都要多3倍，所以要排除自己
```sql
select name, continent from world w1
where population/3 >all(select population from world w2 
                                where w1.continent = w2.continent 
                                  and w1.name !=w2.name)
```

# [SUM and COUNT](http://sqlzoo.net/wiki/SUM_and_COUNT)

##1.
Show the total population of the world.
```sql
SELECT SUM(population)
FROM world
```

##2.
List all the continents - just once each.
```sql
select distinct continent from world
```

##3.
Give the total GDP of Africa
```sql
select sum(gdp) from world
where continent='Africa'
```

##4.
How many countries have an area of at least 1000000
```sql
select count(*) from world
where area>=1e6
```

##5.
What is the total population of ('France','Germany','Spain')
```sql
select sum(population) from world
where name in ('France','Germany','Spain')
```

##6.
For each continent show the continent and number of countries.
```sql
select continent,count(name) from world
group by continent
```

##7.
For each continent show the continent and number of countries with populations of at least 10 million.
```sql
select continent,count(name) from world
where population>=10e6
group by continent
```

##8.
List the continents that have a total population of at least 100 million.
```sql
select continent from world 
group by continent 
                   having  sum(population)>=100e6
```

# [The JOIN operation](http://sqlzoo.net/wiki/The_JOIN_operation)

##1.
Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'
```sql
select matchid,player from goal
 where teamid='ger'
```

##2.
From the previous query you can see that Lars Bender's scored a goal in game 1012. Now we want to know what teams were playing in that match.
Notice in the that the column matchid in the goal table corresponds to the id column in the game table. We can look up information about game 1012 by finding that row in the game table.
Show id, stadium, team1, team2 for just game 1012
```sql
SELECT id,stadium,team1,team2
  FROM game 
  where id='1012'
```

##3
Modify it to show the player, teamid, stadium and mdate and for every German goal.
```sql
SELECT  player,teamid,stadium,mdate
  FROM game JOIN goal ON (id=matchid)
  where goal.teamid='ger'
```

##4.
Use the same JOIN as in the previous question.
Show the team1, team2 and player for every goal scored by a player called Mario player LIKE 'Mario%'
```sql
select a.team1,a.team2,b.player from game a join  goal b on a.id=b.matchid
where b.player like 'Mario%'
```

##5.
The table eteam gives details of every national team including the coach. You can JOIN goal to eteam using the phrase goal JOIN eteam on teamid=id
Show player, teamid, coach, gtime for all goals scored in the first 10 minutes gtime<=10
```sql
SELECT a.player, a.teamid, b.coach, a.gtime  
  FROM goal a join eteam b  on a.teamid=b.id 
 WHERE gtime<=10
```

##6.
To JOIN game with eteam you could use either
game JOIN eteam ON (team1=eteam.id) or game JOIN eteam ON (team2=eteam.id)
Notice that because id is a column name in both game and eteam you must specify eteam.id instead of just id
List the the dates of the matches and the name of the team in which 'Fernando Santos' was the team1 coach.
```sql
select a.mdate,b.teamname  from game a join eteam b on (a.team1=b.id)
where b.coach='Fernando Santos'
```

##7.
List the player for every goal scored in a game where the stadium was 'National Stadium, Warsaw'
```sql
select b.player from game a join goal b on (a.id=b.matchid) 
where a.stadium='National Stadium, Warsaw'
```

##8.
The example query shows all goals scored in the Germany-Greece quarterfinal.
Instead show the name of all players who scored a goal against Germany.
题目：凡是打进德国队球门的所有球员（可以有重复，要求去重）
```sql
SELECT distinct player
  FROM game JOIN goal ON matchid = id 
    WHERE ((team1='ger' and team2!='ger') or (team2='ger' and team1!='ger')) and teamid!='ger'
```

##9.
Show teamname and the total number of goals scored.
COUNT and GROUP BY
```sql
select teamname ,count(*)  from goal  a join eteam b on a.teamid=b.id 
group by a.teamid,b.teamname
```

10.
Show the stadium and the number of goals scored in each stadium.
```sql
select  a.stadium ,count(*) from game  a join goal b on  a.id=b.matchid
group by a.stadium 
```

11.
For every match involving 'POL', show the matchid, date and the number of goals scored.
```sql
SELECT matchid,mdate, count(*)
  FROM game JOIN goal ON matchid = id 
 WHERE (team1 = 'POL' OR team2 = 'POL')
group by goal.matchid,game.mdate
```
对于像这样的group by后查询多属性的，有木有好点的方法？一定要把多属性都group by么？

##12.
For every match where 'GER' scored, show matchid, match date and the number of goals scored by 'GER'
```sql
select matchid,mdate,count(*) from game a join goal b  
on  (a.id=b.matchid and b.teamid='ger')
group by matchid,mdate
```

##13.
Notice in the query given every goal is listed. If it was a team1 goal then a 1 appears in score1, otherwise there is a 0. You could SUM this column to get a count of the goals scored by team1. Sort your result by mdate, matchid, team1 and team2.
直观统计每场球两队的得分
```sql
SELECT mdate,
  team1,
  sum(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) score1,
  team2,
  sum(case when teamid=team2 then 1 else 0 end ) score2
  FROM game left JOIN goal ON matchid = id
  group by mdate,matchid,team1,team2
```
1问题：inner join 死活不对，left join就对。给我一个解释的原因？
