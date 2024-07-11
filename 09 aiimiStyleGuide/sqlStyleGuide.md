# **SQL Style Guide**

[IN PROGRESS]

This style guide mainly focuses on explicit and consistent code formatting, and not on the actual SQL syntax.  
SQL being over 50 years has developed its own "dialects" meaning the SQL you write for Azure T-SQL database will be different to Databricks SQL, and other variants include MySQL and SQLite.  
With this in mind this style guide is here to provide simple guidance, database specific rules will override this.

## Good Practice Guidelines
1. Always use the schema name when referencing tables.
1. Always use full keywords, don't use "NOCHECK" use "WITH NOCHECK" instead.
1. Don't be implicit, be explicit. Don't use "SELECT *" use "SELECT column1, column2" instead.
1. Always explicitly state the join type, don't use "JOIN" use "INNER JOIN" instead.
1. Always use and capitalise keywords like "AS" and "ON".
1. Your driving table should more often than not be the table with the least rows returned.
1. New lines should be used to make code more readable. 
1. Use tabs(or 4 spaces, but be consistent) to indent code to make it more readable.
1. commas at the start or end of a line, just be consistent.


```SQL
--Bad practice
SELECT c.company_name,
    c.company_id
    ,u.user_name FROM Companies c
JOIN users AS u
    on u.company_id=c.company_id

--Good practice
SELECT 
    c.company_name,
    c.company_id,
    u.user_name 
FROM dbo.companies AS c
INNER JOIN dbo.users AS u
    ON u.company_id = c.company_id
```

## Comments
* Comments are not source control
* Comments should not explain how but they should explain what and why we do something
* You can comment with either line comments or block comments it's mostly personal preference.
* -- Is easier and most code editors have hotkeys built in for adding and removing these comments. If you lose the format of your code these can be a nightmare.
* /**/ creates a block comment which will survive bad formatting but is not as widely accepted.

```SQL
--Bad practice
SELECT c.company_name
    ,u.user_name 
FROM dbo.companies AS c
INNER JOIN dbo.users AS u /* Inner join the users on company_id*/
    on u.company_id = c.company_id /*AD 20191108 fix to the join as the alias was wrong*/

--Good practice
SELECT c.company_name
    ,u.user_name
    ,p.nhs_id
FROM dbo.companies AS c
INNER JOIN dbo.users AS u /* Gets the user name which we need to then go get the patient information*/
    ON u.company_id = c.company_id
INNER JOIN dbo.patients AS p
    ON p.patient_id = u.patient_id
```

## views and cte's
* Views and CTE's should be used to make code more readable and reusable.
* Views should be used to store complex queries that are used in multiple places.
* CTE's should be used to make code more readable and to break down complex queries into smaller more manageable parts.

```SQL
-- No CTE Example
SELECT c.company_name
    ,u.user_name
    ,p.nhs_id
FROM dbo.companies AS c
INNER JOIN dbo.users AS u
    ON u.company_id = c.company_id
INNER JOIN dbo.patients AS p
    ON p.patient_id = u.patient_id
WHERE p.nhs_id = '1234567890'

-- CTE Example
WITH company_users AS (
    SELECT c.company_name
        ,u.user_name
        ,u.company_id
    FROM dbo.companies AS c
    INNER JOIN dbo.users AS u
        ON u.company_id = c.company_id
)
SELECT cu.company_name
    ,cu.user_name
    ,p.nhs_id
FROM company_users AS cu
INNER JOIN dbo.patients AS p
    ON p.patient_id = cu.company_id
WHERE p.nhs_id = '1234567890'

-- View Example
CREATE VIEW company_users AS
SELECT c.company_name
    ,u.user_name
    ,u.company_id
FROM dbo.companies AS c
INNER JOIN dbo.users AS u
    ON u.company_id = c.company_id;

SELECT cu.company_name
    ,cu.user_name
    ,p.nhs_id
FROM company_users AS cu
INNER JOIN dbo.patients AS p
    ON p.patient_id = cu.company_id
WHERE p.nhs_id = '1234567890'
```

