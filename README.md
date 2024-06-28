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

--Solution 
SELECT T2.CONTINENT, FLOOR(AVG(T1.POPULATION)) 
FROM CITY AS T1
RIGHT JOIN COUNTRY AS T2
    ON T1.COUNTRYCODE = T2.CODE
    GROUP BY T2.CONTINENT
    HAVING AVG(T1.POPULATION) IS NOT NULL;   --handle the missing value 


Question Link: https://www.hackerrank.com/challenges/the-report/problem?isFullScreen=true

Q.3 : Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

--Solution
-- Query for grades 8 and above
SELECT s.Name, g.Grade, s.Marks
FROM Students s
JOIN Grades g
    ON s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
    WHERE g.Grade >= 8

UNION ALL
--Query for grades below 8
SELECT 'NULL' AS Name, g.Grade, s.Marks
FROM Students s
JOIN Grades g
    ON s.Marks BETWEEN g.Min_Mark AND g.Max_Mark
    WHERE g.Grade < 8
    ORDER BY Grade DESC, Name ASC, Marks ASC;
    

Question Link: https://leetcode.com/problems/average-time-of-process-per-machine/description/

--SOLUTION 
SELECT t1.machine_id, ROUND(AVG(t2.timestamp - t1.timestamp), 3)
AS processing_time
FROM Activity t1
LEFT JOIN Activity t2 
    ON t1.process_id = t2.process_id
    AND t1.machine_id = t2.machine_id
    AND t1.timestamp < t2.timestamp
GROUP BY t1.machine_id;

Question Link : https://leetcode.com/problems/employee-bonus/description/

--Solution
SELECT T1.name, T2.bonus
FROM Employee T1
LEFT JOIN Bonus T2
    ON T1.empId = T2.empId
    WHERE bonus < 1000
    OR bonus IS NULL;





