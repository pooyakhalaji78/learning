https://www.youtube.com/watch?v=7mz73uXD9DA&ab_channel=LukeBarousse
13:12
#----------------------------------------------
Data Science Job Postings 2023
ERD - Entity Relationship Diagram
![1](screenshots/1.png)

4 major tables:
    1. job_postings_fact
        fact table containing all job postings

    2 & 3. dimension tables: skills required
        skill_job_dim
            skills_dim

    4. dimension tables: different companies posting
        company_dim

#----------
jobs_id: unique id
    job_postings_fact

jobs_id: foreign key
    skills_job_dim



#---------------information----------------
Fact Tables:
    -core data for business analysis
    -measure & record business events
    -usually high volume
    -exists key -> dimension table

Dimension Tables:
    -Describe attributes/dimensions of data (skills, companies)

    -Support:
        filtering
        grouping
        labeling
    of facts in reports.

    -fewer rows of data

    -more descriptive
#----------------------------------------------
https://sqliteviz.com/app/#/workspace?hide_schema=1

[lukeb.co/sql_invoices_db](https://sqliteviz.com/app/#/workspace?hide_schema=1)

#------------------SQL Lite-----------------
the queries are NOT key sensitive:
    FROM = from
    SELECT = select

Basic Keywords:
    SELECT *
    FROM job_postings_fact

        SELECT: columns or data
        FROM: table
        *: select all columns

    SELECT 
        job_title_short, 
        job_location
    FROM 
        job_postings_fact

    or

    SELECT
        job_postings_fact.job_title_short,
        job_postings_fact.job_location
            *specified which table is the column from*
    FROM
        job_postings_fact
#-----------------LIMIT------------------

There can be lots of rows and columns called which is very resource intensive.

so limit the amount be "LIMIT" query.

    SELECT 
        job_title_short, 
        job_location
    FROM 
        job_postings_fact
    LIMIT 10


however on the server indentation does not matter so the output for server is:
SELECT job_title_short, job_location FROM job_postings_fact LIMIT 10
#---------------DISTINCT------------------
get unique rows (very resource intensive)

SELECT DISTINCT *now it shows the unique values*
    job_title_short
FROM
    job_postings_fact


end of a sql statement is done by ; sign.
#---------------WHERE------------------
sets a condition

SELECT
	job_title_short, -- column
    job_location, -- column
	job_via, -- column
	salary_year_avg -- column
FROM
	job_postings_fact #table
WHERE
    job_title_short = 'Data Analyst' -- "" for string

or

WHERE
    salary_year_avg > 9000 -- for numerical
#---------------Comments------------------
-- this is a comment

/*
    multiple lines of comment
*/

#---------------ORDER BY------------------
SELECT
	job_title_short,
    job_location,
	job_via,
	salary_year_avg
FROM
	job_postings_fact
WHERE
	job_title_short = 'Data Analyst'
ORDER BY
	salary_year_avg ASC -- ascending
    or
    salary_year_avg DESC -- descending

#---------------order of commands------------------
SELECT
FROM
WHERE
GROUP BY column
HAVING
ORDER BY
LIMIT
#-------------------------------------------------
34:00

comparison operators:
    =
    <> & NOT -> not equal
    <
    >
    >=
    <=

logical operators:
    AND
    OR
    BETWEEN
    IN

ex:

SELECT 
	job_title_short,
    job_location,
    job_via,
    salary_year_avg
    
FROM 
	job_postings_fact
    
WHERE NOT
	job_via <> 'via Ai-Jobs.net'

*this will now show the ones containing this term because there are 2 negative signs NOT and <>

#-------------------------------------------------
SELECT 
	job_title_short,
    job_location,
    job_via,
    salary_year_avg
    
FROM 
	job_postings_fact
    
WHERE
	salary_year_avg >140000 AND job_via = 'via Built In Seattle'
ORDER BY
	salary_year_avg ASC

#-------------------------------------------------
WHERE
	salary_year_avg BETWEEN 140000 AND 160000

#-------------------------------------------------
WHERE
    job_location IN ('Vienna, VA', 'Boston, MA')

* IN is mostly used in text data

#-------------------------------------------------
Instead of 
WHERE
	job_title_short = 'Data Analyst' OR
    job_title_short = 'Data Engineer'

we can use
WHERE
	job_title_short IN ('Data Analyst', 'Data Engineer')

#-------------------------------------------------
SELECT 
	job_title_short,
    job_location,
    job_via,
    salary_year_avg
    
FROM 
	job_postings_fact
    
WHERE
	job_location IN ('Boston, MA', 'Anywhere') AND
    (
    (job_title_short = 'Data Analyst' AND salary_year_avg > 100000) OR
    (job_title_short = 'Business Analyst' AND salary_year_avg > 70000)
    )

#-------------------Wildcards--------------------
used to substitute one/more characters in a string. (LIKE, %, _)

% Represents zero, one, or more characters
_ Represents a single character

ex:
WHERE
	job_title LIKE '%Analyst%'

*shows only the ones containing "Analyst"

%Analyst% : everything with "Analyst"
    Data Analyst
    Senior Analyst Role
    BusinessAnalyst123

%Analyst: ends with "Analyst"
    Data Analyst
    SeniorAnalyst
    123Analyst

Analyst% : start with "Analyst"
    Analyst
    Analyst123
    Analyst Role

%Business%Analyst%

% 0,1 or more
_ 1

#------------------------------------------------
AS - temporary name (alias)

SELECT 
	job_title_short AS job_title,
    job_location AS location,
    job_via AS online_platform,
    salary_year_avg AS salary
    
FROM 
	job_postings_fact AS jpc


or

SELECT 
	jpc.job_title_short AS job_title,
    jpc.job_location AS location,
    jpc.job_via AS online_platform,
    jpc.salary_year_avg AS salary
    
FROM 
	job_postings_fact AS jpc


you can also not put AS at all and instead just give one space

jpc.job_title_short job_title
jpc.job = jpc.job_title_short AS job_title

#------------------------------------------------
example:
SELECT
	job_postings_fact.job_id,
    job_postings_fact.job_title_short,
    job_postings_fact.job_location,
    job_postings_fact.job_via job_posted_site,
    job_postings_fact.job_posted_date,
    job_postings_fact.salary_year_avg AS avg_yearly_salary

FROM
	job_postings_fact

#------------------------------------------------
SELECT
	jpc.job_title,
    jpc.job_location AS location,
    jpc.salary_year_avg AS salary
	

FROM
	job_postings_fact AS jpc
WHERE
	(jpc.job_title LIKE '%Data%' OR 
    job_title LIKE '%Business%') AND
    job_title LIKE '%Analyst%' AND
    job_title NOT LIKE '%Senior%'

#------------------------------------------------
Arithmetic section:

SELECT 
	invoices_fact.project_company,
    invoices_fact.nerd_id, 
    invoices_fact.nerd_role,
    invoices_fact.hours_rate AS rate_original,
    invoices_fact.hours_rate - 5 AS rate_drop,
    invoices_fact.hours_rate + 5 AS rate_hike
FROM 
	invoices_fact

#------------------------------------------------
SELECT 
	invoices_fact.project_company,
    invoices_fact.nerd_id, 
    invoices_fact.nerd_role,
    invoices_fact.hours_spent,
    invoices_fact.hours_rate AS rate_original,
    invoices_fact.hours_rate + 5 AS rate_hike,
    (hours_rate + 5) * hours_spent AS project_total
FROM 
	invoices_fact
WHERE
	project_total > 1000

#------------------------------------------------
SELECT 
	hours_spent,
    hours_rate,
    hours_spent * hours_rate AS Total
	

FROM 
	invoices_fact
ORDER BY
	Total DESC

#------------------------------------------------
59:45
Aggregate Functions 

SUM()
COUNT()
MIN()
MAX()
AVG()

GROUP BY *to filter further

Select 
	jpf.job_title_short as jobs,
    COUNT(jpf.job_title_short) AS job_count,
	AVG(jpf.salary_year_avg) AS salary_avg,
    MIN(jpf.salary_year_avg) AS min_madani,
    MAX(jpf.salary_year_avg) AS max_madani
From 
	job_postings_fact AS jpf
GROUP BY
	jpf.job_title_short
HAVING
	COUNT(jpf.job_title_short) > 100
ORDER BY
	salary_avg

#------------------------------------------------
SELECT
	inv.project_id,
    inv.hours_spent,
    inv.hours_rate AS rate_original,
    SUM(inv.hours_spent * inv.hours_rate) AS project_original_cost,
    inv.hours_rate + 5 AS rate_hike,
    SUM(inv.hours_spent * (inv.hours_rate + 5)) AS project_projected_cost

FROM
	invoices_fact AS inv
GROUP BY
	inv.project_id
    
-- calculate the total earning (Hours_spent * hours_rate) per project
-- calculate a scenario where the hourly rate increases by 5$

#-------------NULL values-----------------
Select 
	jpf.job_title_short,
    jpf.job_location,
    jpf.job_via,
    jpf.salary_year_avg
From 
	job_postings_fact AS jpf
WHERE
	jpf.salary_year_avg IS NULL
ORDER BY
	jpf.salary_year_avg

#--------------SQL JOINS----------------
AB
LEFT JOIN #return all content of table A
RIGHT JOIN #return all content of table B
INNER JOIN #return contents in both tables
FULL OUTER JOIN #return 


Select 
	jpf.job_id,
    jpf.job_title_short,
    jpf.company_id
From 
	job_postings_fact AS jpf
LEFT JOIN company_dim AS cd
	ON jpf.company_id = cd.company_id
# you need to mention how are they going to
be connected to each other which here its ON.

#--------------------------------
Select 
	jpf.job_id,
    jpf.job_title_short,
    sjd.skill_id,
    sk.skills
From 
	job_postings_fact AS jpf
INNER JOIN 
	skills_job_dim AS sjd
    ON jpf.job_id = sjd.job_id
INNER JOIN
	skills_dim AS sk
    ON sjd.job_id = sk.skill_id

#-----------ORDER OF EXECUTION----------
FROM/JOIN
WHERE
GROUP BY
HAVING
SELECT
DISTINCT
ORDER BY
LIMIT/OFFSET
#------------------------------------
SELECT 
	skills.skills AS skill_name,
    COUNT(skills_to_job.job_id) AS number_of_job_postings,
    AVG(job_postings.salary_year_avg) AS average_salary_for_skill
	
FROM 
	skills_dim AS skills
LEFT JOIN
	skills_job_dim AS skills_to_job
    ON skills.skill_id = skills_to_job.skill_id
LEFT JOIN
	job_postings_fact AS job_postings 
    ON skills_to_job.job_id = job_postings.job_id
GROUP BY
	skills.skills
ORDER BY
	average_salary_for_skill DESC

#------------------------------------
sqliteviz prev
POSTGRES now