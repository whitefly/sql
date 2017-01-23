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
