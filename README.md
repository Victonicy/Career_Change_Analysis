# Career_Change_Analysis
## Project Overview


This project analyzes factors influencing career changes using a dataset of 30,000+ records. Key focus areas include demographics, academic backgrounds, job satisfaction, and economic factors. Machine learning models and statistical methods are applied to predict career transitions and uncover actionable insights.

### Data Source


### Tools

Mysql

### Database Creation

```sql
CREATE DATABASE career_change_db;
USE career_change_db;
```
### Table Creation

```sql
CREATE TABLE career_change_tb(
Id INT AUTO_INCREMENT PRIMARY KEY,
Field_of_study VARCHAR (200) NOT NULL,
Current_occupation VARCHAR (200),
Age INT NOT NULL,
Gender ENUM('Male', 'Female'),
Years_of_experience INT,
Education_level ENUM( 'High School', 'Bachelor\'s', 'Master\'s', 'PhD'),
Industry_growth_rate VARCHAR (20),
Job_satisfaction INT,
Work_life_balance INT,
Job_opportunities INT,
Salary DECIMAL(10.2),
Job_security INT,
Career_change_interest TINYINT,
Skill_gap TINYINT,
Family_influence ENUM('High', 'Low', 'Medium', 'None'),
Mentorship_available TINYINT,
Certifications TINYINT,
Freelancing_experience TINYINT,
Geographic_mobility TINYINT,
Professional_networks INT,
Career_change_events INT,
Technology_adoption INT,
Likely_to_change_Occupation TINYINT
);

```
### Loading of Data

```sql
LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/input_files/career_change_prediction_dataset.csv' 
INTO TABLE career_change_tb
FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS
(Id,Field_of_study, Current_occupation, Age, Gender, Years_of_experience, Education_level, 
Industry_growth_rate, Job_satisfaction, Work_life_balance, Job_opportunities, Salary, Job_security, Career_change_interest,
Skill_gap, Family_influence, Mentorship_available, Certifications, Freelancing_experience, Geographic_mobility,
Professional_networks, Career_change_events, Technology_adoption, Likely_to_change_Occupation);
```
### Data Cleaning and Preparation

Data Analysis

```sql
SELECT COUNT(*)
FROM career_change_tb
WHERE Education_level = 'PhD';
```

What field of study is associated with the highest Stability?
```sql
SELECT Field_of_study,
Count(*) AS Field, Career_change_interest
FROM career_change_tb
WHERE Education_level = 'PhD' AND Career_change_interest = 0
GROUP BY Field_of_study;
```

What gender has the highest likelihood for a career change?
```sql
SELECT Gender,
COUNT(*) AS Total_personnell
FROM career_change_tb
WHERE Education_level = 'PhD' AND Likely_to_change_Occupation = 1
GROUP BY Gender;
```
How does age impact the likelihood of a career change?
```sql
SELECT 
      CASE 
      WHEN Age BETWEEN 20 AND 24 THEN '20-24'
      WHEN Age BETWEEN 25 AND 29 THEN '25-29'
      WHEN Age BETWEEN 30 AND 34 THEN '30-34'
      WHEN Age BETWEEN 35 AND 39 THEN '35-39'
	  WHEN Age BETWEEN 40 AND 44 THEN '40-44'
	  WHEN Age BETWEEN 45 AND 49 THEN '45-49'
	  WHEN Age BETWEEN 50 AND 54 THEN '50-54'
	  WHEN Age BETWEEN 55 AND 59 THEN '55-59'
	ELSE 'Other'
    END AS Age_range,
    COUNT(*) AS Total_personnell
    FROM career_change_tb
    WHERE Education_level = 'PhD' AND Likely_to_change_Occupation = 1
    GROUP BY Age_range
    ORDER BY Age_range DESC;
```
How does geographical mobility correlate with changing career likelihood?
```sql
 SELECT Geographic_mobility,
 COUNT(*) AS GM
 FROM career_change_tb
 WHERE Likely_to_change_Occupation = 1
 GROUP BY Geographic_mobility;
```

What field of study has the highest career change rate?
```sql
SELECT Field_of_study,
Count(*) AS Field
FROM career_change_tb
WHERE 
     Education_level = 'PhD' 
     AND Career_change_events >= 1 
GROUP BY Field_of_study
ORDER BY Field DESC;
```
How does certification influence the likelihood of career stability?
```sql
SELECT Career_change_events, Certifications,
Count(*) AS Total
FROM career_change_tb
WHERE 
     Education_level = 'PhD' 
GROUP BY Career_change_events, Certifications
ORDER BY Career_change_events, Certifications;
```
does the presence of a skill gap vary by field of study?
```sql
SELECT Field_of_study, Skill_gap,
COUNT(*) AS Total
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Field_of_study, Skill_gap
ORDER BY Skill_gap DESC;
```

How does job satisfaction influence the decision to change career?
```sql
SELECT Years_of_experience, Likely_to_change_Occupation,
COUNT(*) AS Total_change_likelihood
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Years_of_experience, Likely_to_change_Occupation
ORDER BY Years_of_experience DESC;
```


