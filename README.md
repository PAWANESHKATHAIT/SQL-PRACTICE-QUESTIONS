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
    

Question Link : https://leetcode.com/problems/average-time-of-process-per-machine/description/

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

Question Link: https://leetcode.com/problems/students-and-examinations/description/?envType=study-plan-v2&envId=top-sql-50 

cross join refers to combine the table without using any primary key

--Solution

SELECT
    s.student_id,
    s.student_name,
    sub.subject_name,
    COUNT(e.subject_name) AS attended_exams
FROM
    Students s
CROSS JOIN
    Subjects sub
LEFT JOIN
    Examinations e
ON
    s.student_id = e.student_id AND sub.subject_name = e.subject_name
GROUP BY
    s.student_id, s.student_name, sub.subject_name
ORDER BY
    s.student_id, sub.subject_name;


Question Link : https://www.hackerrank.com/challenges/what-type-of-triangle/problem?isFullScreen=true

--Solution 
SELECT 
    CASE 
        WHEN A + B <= C OR A + C <= B OR B + C <= A THEN 'Not A Triangle'
        WHEN A = B AND B = C AND A = C THEN 'Equilateral'
        WHEN A = B OR A = C OR B = C THEN 'Isosceles'
        ELSE 'Scalene'
    END AS TriangleType
FROM TRIANGLES;

Question Link: https://leetcode.com/problems/managers-with-at-least-5-direct-reports/description/?envType=study-plan-v2&envId=top-sql-50

--Solution 
SELECT e.name
FROM Employee e
JOIN(SELECT managerId
    FROM Employee 
    WHERE managerId IS NOT NULL
    GROUP BY managerId
    HAVING COUNT(*) >=5
    ) AS m
ON e.id = m.managerId;

Question Link : https://leetcode.com/problems/confirmation-rate/description/?envType=study-plan-v2&envId=top-sql-50

--Solution 
SELECT s.user_id,
       ROUND(IFNULL(SUM(CASE WHEN c.action = 'confirmed' THEN 1 ELSE 0 END) / COUNT(c.user_id), 0), 2) AS confirmation_rate
FROM Signups s
LEFT JOIN Confirmations c
    ON s.user_id = c.user_id
GROUP BY s.user_id;
 
Question Link : https://leetcode.com/problems/average-selling-price/?envType=study-plan-v2&envId=top-sql-50

COALESCE is used to handle missing or null values by providing a default value.
--Solution 
SELECT p.product_id,
    COALESCE(ROUND(SUM(u.units * p.price) / SUM(u.units), 2), 0) AS average_price
FROM Prices p
LEFT JOIN UnitsSold u
    ON p.product_id = u.product_id
    AND u.purchase_date BETWEEN p.start_date AND p.end_date
GROUP BY p.product_id;


--Intermediate Level--

Question Link : https://www.hackerrank.com/challenges/placements/problem?isFullScreen=true

--Solution
SELECT s.Name
FROM Students s
JOIN Friends f
    ON s.ID = f.ID
JOIN Packages p1 
    ON s.ID = p1.ID
JOIN Packages p2 
    ON f.Friend_ID = p2.ID
WHERE p2.Salary > p1.Salary
ORDER BY p2.Salary;

Question Link: https://leetcode.com/problems/list-the-products-ordered-in-a-period/description/?envType=study-plan-v2&envId=top-sql-50
--solotion
SELECT p.product_name, SUM(o.unit) as unit
FROM Products p
JOIN Orders o
    ON p.product_id = o.product_id 
WHERE  o.order_date >= '2020-02-01' AND o.order_date <= '2020-02-29'
GROUP BY p.product_name
HAVING unit >= 100;

--Advanced Function

Question Link: https://leetcode.com/problems/group-sold-products-by-the-date/description/?envType=study-plan-v2&envId=top-sql-50


~ Group_concat : used to concatenate data from multiple rows into one field. This is an aggregate (GROUP BY) function that returns a String value 
--solution
SELECT
    sell_date,
    COUNT(DISTINCT product) AS num_sold,
    GROUP_CONCAT(DISTINCT product ORDER BY product SEPARATOR ',') AS products
FROM Activities
GROUP BY sell_date
ORDER BY sell_date;

Question Link : https://leetcode.com/problems/find-users-with-valid-e-mails/?envType=study-plan-v2&envId=top-sql-50

REGEXP : It allows you to search for specific patterns in text data, making it a powerful tool for filtering, extracting, and validating text in SQL queries.

--Solution
SELECT *
FROM Users
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9_\.\-]*@leetcode(\\?com)?\\.com$';


--Basic Aggregate Functions--

Question Link : https://leetcode.com/problems/project-employees-i/description/?envType=study-plan-v2&envId=top-sql-50

--Solution 
SELECT p.project_id, ROUND(AVG(e.experience_years), 2) AS average_years 
FROM Project p
JOIN Employee e
    ON p.employee_id = e.employee_id
GROUP BY p.project_id;

Question Link : https://leetcode.com/problems/percentage-of-users-attended-a-contest/description/

--Solution 
SELECT 
    r.contest_id, 
    ROUND(COUNT(r.user_id) * 100.0 / (SELECT COUNT(*) FROM Users), 2) AS percentage
FROM 
    Register r
GROUP BY 
    r.contest_id
ORDER BY 
    percentage DESC, r.contest_id;

Question Link : https://leetcode.com/problems/queries-quality-and-percentage/submissions/

--Solution
SELECT
    query_name,
    ROUND(AVG(rating * 1.0 / position), 2) AS quality,
    ROUND(SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS poor_query_percentage 
FROM Queries
WHERE query_name IS NOT NULL
GROUP BY query_name;

Question Link : https://leetcode.com/problems/monthly-transactions-i/description/?envType=study-plan-v2&envId=top-sql-50

--Solution
SELECT 
    DATE_FORMAT(trans_date, '%Y'-%m') as month,
    country,
    COUNT(id) AS trans_count,
    SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) AS approved_count,
    SUM(amount) AS trans_total_amount,
    SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) AS approved_total_amount
FROM 
    Transactions
GROUP BY DATE_FORMAT(trans_date, '%Y'-%m'), country;
    
Question Link : https://leetcode.com/problems/immediate-food-delivery-ii/?envType=study-plan-v2&envId=top-sql-50

--Solution
WITH cte AS (
    SELECT 
        *,
        ROW_NUMBER() OVER(PARTITION BY customer_id ORDER BY order_date) AS rn
    FROM 
        delivery
)

SELECT 
    ROUND(SUM(IF(order_date = customer_pref_delivery_date, 1, 0)) * 100.0 / COUNT(*), 2) AS immediate_percentage
FROM 
    cte
WHERE 
    rn = 1;


Question Link : https://leetcode.com/problems/game-play-analysis-iv/description/
--Solution 
WITH temp AS (
    SELECT player_id, MIN(event_date) AS first_login_date
    FROM Activity 
    GROUP BY player_id
)

SELECT 
    ROUND(
        SUM(DATEDIFF(a.event_date, t.first_login_date) = 1) / COUNT(DISTINCT a.player_id), 2
    ) AS fraction
FROM Activity a
JOIN temp t 
    ON a.player_id = t.player_id;

--Advanced Select and Joins--

Question Link : https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/

--Solution 
WITH ManagerDetails AS (
    SELECT 
        e1.employee_id,
        e1.name,
        COUNT(e2.employee_id) AS reports_count,
        ROUND(AVG(e2.age), 0) AS average_age
    FROM Employees e1
    LEFT JOIN Employees e2 
        ON e1.employee_id = e2.reports_to
    WHERE e2.employee_id IS NOT NULL
    GROUP BY e1.employee_id, e1.name
    HAVING COUNT(e2.employee_id) > 0
)
SELECT 
    employee_id,
    name,
    reports_count,
    average_age
FROM ManagerDetails
ORDER BY employee_id;

