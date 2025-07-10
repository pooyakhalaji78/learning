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
    