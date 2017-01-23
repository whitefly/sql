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
