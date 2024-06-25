------JOINS Related Questions------
Question Link: https://www.hackerrank.com/challenges/asian-population/problem?isFullScreen=true

Given the CITY and COUNTRY tables, query the sum of the populations of all cities where the CONTINENT is 'Asia'.
Note: CITY.CountryCode and COUNTRY.Code are matching key columns.

--Solution
SELECT SUM(T1.POPULATION)
FROM CITY AS T1
LEFT JOIN COUNTRY AS T2
    ON T1.COUNTRYCODE = T2.CODE
    WHERE CONTINENT ="Asia";
