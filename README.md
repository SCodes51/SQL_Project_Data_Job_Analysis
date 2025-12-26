# Introduction
Navigated into the data job market for Data Analyst roles. The project explores the top paying jobs, in-demand skills and where the high demand meets high salary in analytics

SQL Queries - 
[project_sql Folder](/project_sql/)

# Background
Tried to search the data analyst job market, to pinpoint the top paid and in-demand skills streamlining others work to find optimal jobs.

### Questions asked were -

1. What are the top-paying data analyst jobs ?
2. What skills are required for these top-paying jobs ?
3. What skills are most in demand for data analysts ?
4. Which skills are associated with higher salaries ?
5. What are the most optimal skills to learn ?


# Tools Used
For the analysis of job market containing roles for data analyst, several key tools got used - 

- **SQL** : For querying the database to get the results
- **PostgreSQL** : The chosen database management system, ideal for handling the job posting data.
- **Visual Studio Code** : My go-to for database management and executing SQL queries.
- **Git & Github** : Essential for version control and sharing my SQL scripts and analysis and project tracking.

# The Analysis
Each query for this project aimed at investigating specific aspects of the data analyst job markets. Here's how i approached each question :

### 1. Top Paying Data Analyst Jobs
To identify the highest paying roles I filtered data analyst positions by average yearky salary and location focusing on remote jobs. This query highlights the high paying opportunities in the field.

```sql
SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name

FROM
    job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL

ORDER BY 
    salary_year_avg DESC
LIMIT 10
```

The breakdown of the top data anakyst jobs - 
- **Wide Salary Range:** Top paying data analyst roles span from $184,000 to $650,000 indicating significant salary potential in the field.
- **Diverse Employers:** Companies like SmartAsset, Meta and AT&T are among those offering the high salaries showing broad interest across different industries.
- **Job Title Variety:** There's a high diversity in job titles, from Data Analyst to Director of Analytics reflecting the varies roles and specializations within data analytics.

### 2. Skills for top paying jobs

```sql
WITh top_paying_jobs AS (

        SELECT
            job_id,
            job_title,
            job_location,
            job_schedule_type,
            salary_year_avg,
            job_posted_date,
            name AS company_name

        FROM
            job_postings_fact
            LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
        WHERE
            job_title_short = 'Data Analyst' AND
            job_location = 'Anywhere' AND
            salary_year_avg IS NOT NULL

        ORDER BY 
            salary_year_avg DESC
        LIMIT 10
)

SELECT 
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC
```
The breakdown of the most demanded skills for the top 10 highest paying data analyst jobs :
- **SQL**

- **Python**

- **Tableau**

- **Pandas**

- **Excel**

### 2. Skills for top paying jobs

```sql

SELECT 
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id

WHERE 
    job_title_short = 'Data Analyst' AND
    job_work_from_home = True
GROUP BY
    skills
ORDER BY 
    demand_count DESC

LIMIT 5
```

This query helped with identifying the skills most frequently requested in the job postings, directing focus to areas with high demand.

The breakdown for the most demanded skills for data analyst are :

- **SQL and Excel**

- **Programming and visualization tools** like **Python, Tableau and Power-BI**


### 4. Skills based on Salary

Took a look the the average salaries associated with different skills revealed which skills are the highest paying.
```sql
SELECT 
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id

WHERE 
    job_title_short = 'Data Analyst' 
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True
GROUP BY
    skills
ORDER BY 
    avg_salary DESC
LIMIT 25
```

Here's the breakdown of the results of the top payiing skills for Data Analytics:

- **High Demand for Big Data & ML skills** like PySpark, Couchbase for Big Data, DataRobot and Jupyter for machine learning along with Python libraries like Pandas and Numpy.

- **Software Development and Deployment proficiency:** like GitLab, Kubernetes, Airflow for data analysis and engineering.

- **Cloud Computing expertise:** like Elasticsearch, Databricks, GCP.

### 5. Most optimal skills to learn

The following query pinpoints the skills that are both in high demand and have high salaries offering a strategic focus for skill development.

```sql
SELECT 
    skills_demand.skill_id,
    skills_demand.skills,
    demand_count,
    avg_salary
FROM   
    skills_demand
    INNER JOIN average_salary ON skills_demand.skill_id = average_salary.skill_id

WHERE 
    demand_count > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
    
LIMIT 25

```
Here's the breakdown for the most optimal skills for data analysts :

- **High-Demand programming languages:** Python and R

- **Cloud Tools and Technologies:** Snowflake, Azure, AWS and BigQuery 

- **Business Intelligence anf Visualization Tools:** Tableau and Looker

- **Database Technologies:** NoSQL databases like Oracle, SQL Server. 


# Conclusions
### Insights

1. **Top-paying Data Analyst Jobs:** The highest paying jobs for data analysts that allow remote work offer a wide range of salaries, the highest at $650,000.
2. **Skills for Top-Paying Jobs:** High paying jobs require advanced proficiency in SQL, suggesting it's a critical skill for earning a top salary.
3. **Most In-Demand Skills:** SQL is the most demanded skill in the data analyst job market, thus making it essential for job seekers.
4. **Skills with Higher Salaries:** Specialized skills such as SVN and Solidity, are associated with the highest average salaries indicating a premium on niche experstise.
5. **Optimal Skills for Job Market Value:** SQL leads in demand and offers for a high average salary, positioning it as one of the most optimal skill for data analysts to learn.


