# Career_Change_Analysis
## Project Overview

This project explores whether an individual's field of study determines their occupation and the factors influencing career changes. Using a comprehensive dataset with over 30,000 records and 22 attributes, we analyzed trends and predictive patterns related to career transitions. The dataset includes details such as age, gender, years of experience, education level, job satisfaction, skill gap, and more. The primary target variable, "Likely to Change Occupation", indicates the likelihood of career transitions.

Our goal was to uncover trends in career stability, factors contributing to career changes, and relationships between attributes.


### Data Source
The primary data set used for this analysis is the "Career_change_dataset" offering insights into 

### Tools

Mysql

### Database Setup
#### 
The dataset was structured in a MySQL database, enabling robust querying and analysis.

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
#### 
The dataset was imported using the LOAD DATA INFILE command after enabling local_infile.

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
### Analytic Questions and Insights

What is the count of personnel with a PhD?
What are the age demographics of PhD holders?
Which field of study has the highest stability (no career change events)?
Which gender is more likely to change occupations?
What is the likelihood of career changes across age ranges?
What is the technology adoption rate by age group?
Does geographic mobility correlate with career change likelihood?
Which field of study has the highest career change rate?
How does certification influence career stability?
Does the presence of a skill gap vary by field of study?
What is the relationship between years of experience and career change likelihood?
How does job satisfaction influence career changes?
Are individuals in industries with higher growth rates less likely to switch careers?
Do individuals with strong professional networks exhibit lower career change rates?


Data Analysis
- Total personnel with a PhD: 9,777
```sql
SELECT COUNT(*)
FROM career_change_tb
WHERE Education_level = 'PhD';
-- Total personnel with a PhD: 9,777
```
- What field of study has the highest stability based on career change interest?
```sql
SELECT Field_of_study,
Count(*) AS Field, Career_change_interest
FROM career_change_tb
WHERE Education_level = 'PhD' AND Career_change_interest = 0
GROUP BY Field_of_study;
-- Art has the highest stability
```

- What gender has the highest likelihood for a career change
```sql
SELECT Gender,
COUNT(*) AS Total_personnell
FROM career_change_tb
WHERE Education_level = 'PhD' AND Likely_to_change_Occupation = 1
GROUP BY Gender
ORDER BY Total_personnell DESC;
-- Females have a slightly higher likelihood (2,799) compared to males (2,789).
```
- Likelihood to change occupation based on age range
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
    ORDER BY Total_personnell DESC;
    -- Personnel aged 55-59 are most likely to change occupations (732 instances).
    -- Personnel aged 25-29 are the least likely to change occupations (670 instances).
 ```
 - What is the technology adoption rate by age group?
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
COUNT(*) AS Total_personnell, Technology_adoption
FROM career_change_tb
WHERE Education_level = 'PhD'
GROUP BY Age_range, Technology_adoption  
ORDER BY Technology_adoption DESC;
 -- Personnels whose age ranges between 50-54 are seen to have the highest tech adoption rate with a total of 129 personnels with a tech adoption level of 10 and also have a total of 
    137 personnels  with a technology adoption level of 1
 -- Personnels with age range 40 - 44 have a total of 98 personnel who have a technology adoption rate  of 10, while age range 45 - 49 have 107 personnels with a technology adoption 
    rate of 1. 
```

- Does geographic mobility correlate with career change likelihood
```sql
 -- How does geographical mobility correlate with changing career likelihood
 SELECT Geographic_mobility,
 COUNT(*) AS GM
 FROM career_change_tb
 WHERE Education_level = 'PhD' AND Likely_to_change_Occupation = 1
 GROUP BY Geographic_mobility
 ORDER BY GM DESC;
 -- The data indicate Geographic mobility does not significantly affect career change likelihood.
    3857 geographically immobile personnel were likely to change careers, compared to 1,731 who were mobile.
```
- Which field of study has the highest career change rate?
```sql
SELECT Field_of_study,
Count(*) AS Field
FROM career_change_tb
WHERE 
     Education_level = 'PhD' 
     AND Career_change_events = 2 
GROUP BY Field_of_study
ORDER BY Field DESC
LIMIT 1;
-- Biology has the highest number of career change with a total of 703 changes.
```

- How does certification influence career stability?
```sql
SELECT Career_change_events, Certifications,
Count(*) AS Total
FROM career_change_tb
WHERE 
     Education_level = 'PhD' 
GROUP BY Career_change_events, Certifications
ORDER BY Total DESC; 
-- The data infers that the number of certification does not have a significant effect on the number of career changes
-- as 2339 personnells with no certificates changed their career once and 2292 personnells with no certificates changes
-- their career twice. 1003 personnell with certificate changed their career twice while 966 personnell with certificate changed their career once.
-- 957 personnell with one certificate had no career change.
```

- Does the presence of a skill gap vary by field of study?
```sql
SELECT Field_of_study, Skill_gap,
COUNT(*) AS Total
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Field_of_study, Skill_gap
ORDER BY Skill_gap DESC; 
-- Yes, the presence of skill gaps varies significantly across fields of study.
```

-What is the relationship between years of experience and career change likelihood?
```sql
SELECT Years_of_experience, Likely_to_change_Occupation,
COUNT(*) AS Total_change_likelihood
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Years_of_experience, Likely_to_change_Occupation
ORDER BY Years_of_experience DESC;
-- Career changes are likely across all experience levels, with 15 and 21 years showing the highest likelihood (160 instances each). Personnel with 7 years of experience had the lowest likelihood (107 instances).
```

- How does job satisfaction influence career changes?
```sql
SELECT Job_satisfaction, Likely_to_change_Occupation,
COUNT(*) AS Total_change_likelihood
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Job_satisfaction, Likely_to_change_Occupation
ORDER BY Job_satisfaction DESC;
-- 974 personnels with Job satisfaction level of 1, 977 personnels with job satisfaction level of 2, 949 personnels with Job satisfaction level of 3, all show likelihood to change 
   career, while 278 personnels with job satisfaction level of 10. 283 personnels with Job satisfaction level of 9 and 284 personnels with job satisfaction level of 8 show likelihood to 
   change career, this data indicates that personnels with the lowest job satisfaction level ranging from 4 to 1 show the highest likelihood to change career. 
```

- Are individuals in industries with higher growth rates less likely to switch careers?
```sql
SELECT Industry_growth_rate, Likely_to_change_Occupation,
COUNT(*) AS Total_change_likelihood
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Likely_to_change_Occupation, Industry_growth_rate
ORDER BY Likely_to_change_Occupation DESC;
-- No, individuals in industries with higher growth rates are more likely to switch career
```

- Do individuals with strong professional networks exhibit lower career change rates?
```sql
SELECT Professional_networks, Career_change_events,
COUNT(*) AS Total_change_by_personnell
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Professional_networks, Career_change_events
ORDER BY Professional_networks DESC;
-- individuals professional network has little influence on the number of career change events recorded
```

```sql
-- Does salary impact the likelihood of career changes?
SELECT Salary, Career_change_events
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Salary, Career_change_events
ORDER BY Salary ASC;
```

- Are individuals in industries with more job opportunities less likely to change careers?
```sql
SELECT Job_opportunities,
SUM(CASE WHEN Likely_to_change_Occupation = 1 THEN 1 ELSE 0 END) AS Total_likelihood_to_Change,
SUM(CASE WHEN Likely_to_change_Occupation = 0 THEN 1 ELSE 0 END) AS Total_likelihood_not_to_Change
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Job_opportunities, Likely_to_change_Occupation
ORDER BY Job_opportunities DESC;
```

- How does job security affect career change interest?
```sql
SELECT Job_security, Career_change_events,
COUNT(*) AS Total_change_by_personnell
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Job_security, Career_change_events
ORDER BY Job_security DESC;
```

- Does family influence play a role in career change decisions?
```sql
SELECT Work_life_balance, Career_change_events,
COUNT(*) AS Total_change_by_personnell
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Work_life_balance, Career_change_events
ORDER BY Work_life_balance DESC;
```

- How does mentorship availability affect career change interest?
```sql
SELECT Mentorship_available, Career_change_events,
COUNT(*) AS Total_change_by_personnell
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Mentorship_available, Career_change_events
ORDER BY Mentorship_available DESC;
```

- Do individuals with freelancing experience have a higher propensity to switch careers?
```sql
SELECT Freelancing_experience, Career_change_events,
COUNT(*) AS Total_change_by_personnell
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Freelancing_experience, Career_change_events
ORDER BY Freelancing_experience DESC;
-- From the above data, Individuals with freelancing experience have a lower chance to change career
```

- Do individuals with freelancing experience have a better work life balance?
```sql
SELECT Work_life_balance,
SUM(CASE WHEN Freelancing_experience = 1 THEN 1 ELSE 0 END) AS Freelancer_exp,
SUM(CASE WHEN Freelancing_experience = 0 THEN 1 ELSE 0 END) AS Non_freelancers
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Freelancing_experience, Work_life_balance
ORDER BY Work_life_balance DESC;
```

- Does comfort with technology adoption correlate with career change likelihood?
```sql
SELECT Technology_adoption, Likely_to_change_Occupation,
COUNT(*) AS Total_change_likelihood
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Likely_to_change_Occupation, Technology_adoption
ORDER BY Technology_adoption DESC;
```

- How do previous career change events influence the likelihood of future changes?
```sql
SELECT Career_change_events, Likely_to_change_Occupation,
COUNT(*) AS Total_change_likelihood
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Likely_to_change_Occupation, Career_change_events
ORDER BY Likely_to_change_Occupation DESC;
```

- Can we predict career change likelihood based on job satisfaction and skills gap?
```sql
SELECT Skill_gap, Job_satisfaction, Likely_to_change_Occupation,
COUNT(*) AS Total_change_likelihood
FROM career_change_tb
WHERE 
     Education_level = 'PhD'
GROUP BY Likely_to_change_Occupation, Job_satisfaction, Skill_gap
ORDER BY Likely_to_change_Occupation DESC;
```

- Which demographic or professional groups should HR departments focus on for retention strategies?
```sql
SELECT Field_of_study, 
COUNT(*) AS Total_dependency
FROM career_change_tb
WHERE 
     Education_level = 'PhD' AND Likely_to_change_Occupation = 0
     GROUP BY Field_of_study
     ORDER BY Total_dependency DESC;
	-- Based on "field of study", Business graduates have a higher retention rate (447 personnell) as the have shown the highest tendency not to change their occupation
    -- up next is computer science graduates with a Total of 440 personnell and Biology graduates with a Total of 426 personnell

SELECT Current_occupation,
COUNT(*) AS Total_dependency
FROM career_change_tb
WHERE 
     Education_level = 'PhD' AND Likely_to_change_Occupation = 0
     GROUP BY Current_occupation
     ORDER BY Total_dependency DESC;
-- Based on Current occupation, personnells working as Lawyers have a higher retention rate (459), up next are Business Analyst(455) then Teachers (434)
```
### Key Findings

####
Field of Study and Stability:

- Art fields exhibit the highest career stability.
- Biology has the highest number of career changes among individuals with PhDs.

Demographics:

- Females are slightly more likely to change careers than males.
- Individuals aged 55-59 are most likely to transition careers.

Experience and Job Satisfaction:

- Career changes occur across all experience levels, with the highest changes seen at 15 and 21 years of experience.
- Low job satisfaction (levels 1–4) significantly increases the likelihood of career changes.

Certifications:

- Certification presence has limited influence on career stability.

Skill Gap and Growth Rate:

- Skill gaps vary by field and correlate with increased career changes.
- Higher industry growth rates do not deter career transitions.

Geographic Mobility:

- Mobility has little effect on career change likelihood.

