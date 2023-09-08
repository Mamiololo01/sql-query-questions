# sql-query-questions

Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table. 

select count(city) - count(distinct city) from station

Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically. 

select CITY, length(CITY) from STATION order by length(CITY), CITY limit 1;
select CITY, length(CITY) from STATION order by length(CITY) desc, CITY limit 1;

Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates.
 SELECT DISTINCT 
            CITY 
         FROM 
           STATION 
         WHERE CITY 
           REGEXP '^[aeiou]'

Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.
SELECT DISTINCT city FROM station WHERE city REGEXP '[aeiou]$';

Query the list of CITY names from STATION which have vowels (i.e., a, e, i, o, and u) as both their first and last characters. Your result cannot contain duplicates.

SELECT DISTINCT city FROM station WHERE city REGEXP "^[aeiou].*[aeiou]$";

Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.


SELECT DISTINCT 
            CITY 
         FROM 
           STATION 
         WHERE CITY 
           NOT REGEXP  '^[aeiou]'

SELECT DISTINCT city FROM station WHERE city NOT REGEXP '[aeiou]$';

Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

SELECT DISTINCT CITY 
FROM STATION 
WHERE (CITY NOT IN (SELECT DISTINCT CITY FROM STATION WHERE CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR CITY LIKE '%u'))
OR 
(CITY NOT IN (SELECT CITY FROM STATION WHERE CITY LIKE 'A%' OR CITY LIKE 'E%' OR CITY LIKE 'I%' OR CITY LIKE 'O%' OR CITY LIKE 'U%'));

Query the list of CITY names from STATION that do not start with vowels and do not end with vowels. Your result cannot contain duplicates.

SELECT DISTINCT CITY 
FROM STATION 
WHERE (CITY NOT IN (SELECT DISTINCT CITY FROM STATION WHERE CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR CITY LIKE '%u'))
AND
(CITY NOT IN (SELECT CITY FROM STATION WHERE CITY LIKE 'A%' OR CITY LIKE 'E%' OR CITY LIKE 'I%' OR CITY LIKE 'O%' OR CITY LIKE 'U%'));

Query the Name of any student in STUDENTS who scored higher than Marks. Order your output by the last three characters of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.

SELECT NAME
FROM STUDENTS
WHERE MARKS > 75
ORDER BY RIGHT(NAME,3) ASC, ID ASC;

Write a query that prints a list of employee names (i.e.: the name attribute) for employees in Employee having a salary greater than per month who have been employees for less than months. Sort your result by ascending employee_id.

SELECT NAME FROM EMPLOYEE 
WHERE SALARY > 2000 AND MONTHS < 10 
ORDER BY EMPLOYEE_ID;

Query a count of the number of cities in CITY having a Population larger than 100000
SELECT COUNT(POPULATION)
FROM CITY
WHERE POPULATION > 100000

Query the total population of all cities in CITY where District is California.
SELECT SUM(POPULATION) 
FROM CITY
WHERE DISTRICT = "CALIFORNIA"

Query the average population of all cities in CITY where District is California.

SELECT AVG(POPULATION) 
FROM CITY
WHERE DISTRICT = "CALIFORNIA"

Query the average population for all cities in CITY, rounded down to the nearest integer.
SELECT FLOOR(AVG(POPULATION))
FROM CITY

Query the sum of the populations for all Japanese cities in CITY. The COUNTRYCODE for Japan is JPN.

SELECT SUM(POPULATION)
FROM CITY
WHERE COUNTRYCODE = "JPN"

Query the difference between the maximum and minimum populations in CITY.
SELECT MAX(POPULATION) - MIN(POPULATION)
FROM CITY

Samantha was tasked with calculating the average monthly salaries for all employees in the EMPLOYEES table, but did not realize her keyboard's key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary.
Write a query calculating the amount of error (i.e.: average monthly salaries), and round it up to the next integer.

SELECT CEIL(AVG(salary) - AVG(REPLACE(salary, '0', ''))) 
FROM employees

We define an employee's total earnings to be their monthly worked, and the maximum total earnings to be the maximum total earnings for any employee in the Employee table. Write a query to find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings. Then print these values as space-separated integers.

SELECT (salary * months) a, COUNT(*) 
FROM employee 
GROUP BY a 
ORDER BY a DESC 
LIMIT 1

Query the following two values from the STATION table:
1. The sum of all values in LAT_N rounded to a scale of decimal places.
2. The sum of all values in LONG_W rounded to a scale of decimal places.

SELECT ROUND(SUM(lat_n),2), ROUND(SUM(long_w),2) 
FROM STATION

Query the sum of Northern Latitudes (LAT_N) from STATION having values greater than and less than . Truncate your answer to decimal places.

SELECT ROUND(SUM(LAT_N), 4) FROM STATION
WHERE LAT_N > 38.7880 AND LAT_N < 137.2345

Query the greatest value of the Northern Latitudes (LAT_N) from STATION that is less than 137.2345 . Truncate your answer to 4 decimal places.

SELECT ROUND(MAX(LAT_N),4)
FROM STATION
WHERE LAT_N < 137.2345

Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in STATION that is less than 137.2345  Round your answer to decimal places.

SELECT ROUND(long_w, 4) 
FROM station 
WHERE lat_n < 137.2345  
ORDER BY lat_n DESC 
LIMIT 1

Query the smallest Northern Latitude (LAT_N) from STATION that is greater than 38.7780 . Round your answer to decimal places.

SELECT ROUND(MIN(LAT_N),4) FROM STATION
WHERE LAT_N > 38.7780

Query the Western Longitude (LONG_W)where the smallest Northern Latitude (LAT_N) in STATION is greater than 38.7780 . Round your answer to 4 decimal places.

SELECT ROUND(long_w, 4) 
FROM station 
WHERE lat_n = (SELECT MIN(lat_n) FROM station WHERE lat_n > 38.7780)

Consider P₁(a,b) and P₂(c,d) to be two points on a 2D plane.
* 		happens to equal the minimum value in Northern Latitude (LAT_N in STATION).
* 		happens to equal the minimum value in Western Longitude (LONG_W in STATION).
* 		happens to equal the maximum value in Northern Latitude (LAT_N in STATION).
* 		happens to equal the maximum value in Western Longitude (LONG_W in STATION).
Query the Manhattan Distance between points P₁ and P₂ and round it to a scale of 4 decimal places.

SELECT ROUND((MAX(lat_n) - MIN(lat_n)) + (MAX(long_w) - MIN(long_w)), 4) 
FROM station


Consider P₁(a,c)and P₂(b,d) to be two points on a 2D plane where (a, b) are the respective minimum and maximum values of Northern Latitude (LAT_N) and (c, d) are the respective minimum and maximum values of Western Longitude (LONG_W) in STATION.
Query the Euclidean Distance between points P₁ and P₂ and format your answer to display 4 decimal digits.

SELECT ROUND(SQRT(POWER((MAX(lat_n) - MIN(lat_n)),2) + POWER((MAX(long_W) - MIN(long_w)),2)), 4) 
FROM station
