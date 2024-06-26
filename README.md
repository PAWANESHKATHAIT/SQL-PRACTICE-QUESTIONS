------JOINS Related Questions------
Question Link: https://www.hackerrank.com/challenges/asian-population/problem?isFullScreen=true

Q.1 : Given the CITY and COUNTRY tables, query the sum of the populations of all cities where the CONTINENT is 'Asia'.
Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

--Solution
SELECT SUM(T1.POPULATION)
FROM CITY AS T1
LEFT JOIN COUNTRY AS T2
    ON T1.COUNTRYCODE = T2.CODE
    WHERE CONTINENT ="Asia";
    

Question Link: https://www.hackerrank.com/challenges/average-population-of-each-continent/problem?isFullScreen=true

Q.2 : Given the CITY and COUNTRY tables, query the names of all the continents (COUNTRY.Continent) and their respective average city populations (CITY.Population) rounded down to the nearest integer.

Note: CITY.CountryCode and COUNTRY.Code are matching key columns. 

FLOOR keyword- return the largest interger values. 
Handle the missing values also.

SELECT T2.CONTINENT, FLOOR(AVG(T1.POPULATION)) 
FROM CITY AS T1
RIGHT JOIN COUNTRY AS T2
    ON T1.COUNTRYCODE = T2.CODE
    GROUP BY T2.CONTINENT
    HAVING AVG(T1.POPULATION) IS NOT NULL;   --handle the missing value 
