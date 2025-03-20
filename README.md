# Infectious Diseases Surveillance Hub(2009-2018)

## Table of Contents

- [Project Overview](#project-overview)
- [Data Sources](#data-sources)
- [Tools](#tools)
- [Data Cleaning/Preparation](#data-cleaning-preparation)
- [Exploratory Data Analysis](#exploratory-data-analysis)
- [Data Analysis](#data-analysis)
- [Results/Findings](#results-Findings)
- [Recommendations](#recommendations)
- [Limitations](#limitations)

## Project Overview

This project analyzes infectious disease trends in Nigeria from 2009 to 2018 using SQL for data cleaning and Power BI for visualization. It explores disease prevalence, mortality rates, gender-based distribution, urban vs. rural impact, and state-wise case trends.

![dashboard](https://github.com/user-attachments/assets/bcd8bfa7-9c5b-4091-8d94-34fdc5c6c17f)


### Data Sources


Infectious Diseases Data: The primary dataset used for this analysis is the "meningitis_clean" file

### Tools

- SQL - Data cleaning
- Power bi - for visualization

### Data Cleaning/Preparation

In the initial data preparation phase, we performed the following tasks:
1. Data loading and inspection.
2. Handling missing values.
3. Data cleaningand formatting.

### Exploratory Data Analysis

EDA involved exploring the infectious diseases data to answer key questions, such as:

- What are the most prevalent infectious diseases in Nigeria (2009-2018)?
- How does disease distribution compare between rural and urban areas?
- What is the overall mortality rate?
- How have disease cases fluctuated over the years?
- Which states reported the highest number of cases?
- How does the number of total cases compare to death cases by gender?
- How do disease cases vary by gender?

### Data Analysis 

Include some interesting code/features worked with

```sql
CREATE DATABASE MENINGITIS_DATASET; -- creating a database

-- CREATING A TABLE FOR INSERTING MENINGITIS DATASET
CREATE TABLE MENINGITIS_DATASET2
(
id int,
surname text,
firstname text,
middlename text,
gender text,
gender_male int,
gender_female int,
state text,
settlement text,
rural_settlement int,
urban_settlement int,
report_date text,
report_year int,
age int,
age_str text,
date_of_birth text,
child_group int,
adult_group int,
disease text,
cholera int,
diarrhoea int,
measles int,
viral_haemmorrhaphic_fever int,
meningitis int,
ebola int,
marburg_virus int,
yellow_fever int,
rubella_mars int,
malaria int,
serotype text,
NmA int,
NmC int,
NmW int,
health_status text,
alive int,
dead int,
report_outcome text,
unconfirmed int,
confirmed int,
null_serotype int
); 

SHOW VARIABLES LIKE 'secure_file_priv'; -- Trying to know mysql designated dataset storage point

LOAD DATA INFILE 'C:/ProgramData/MySQL/MySQL Server 8.0/Uploads/MENINGITIS_DATASET2.csv'
INTO TABLE meningitis_dataset2
FIELDS TERMINATED BY ','
IGNORE 1 LINES; -- INSERTING MENINGITIS DATASET2 INTO TABLE MENINGITIS_DATASET2

SELECT *
FROM meningitis_dataset2; -- TO SEE WHAT THE DATASET LOOKS LIKE

-- SINCE I'M NOT GOING TO DO A PREDICTIVE ANALYSIS, I WILL BE CREATING A NEW TABLE CONTAINING THE NECESSARY COLUMN NEEDED FOR CLEANING AND VISUALIZATION OBJECTIVES

CREATE TABLE MENINGITIS_DATASET3 AS
SELECT id, surname, firstname, middlename, gender, 
state, settlement, report_date, report_year, age,
date_of_birth, disease, health_status, report_outcome
FROM meningitis_dataset2; -- CREATING A NEW TABLE FROM AN EXISTING TABLE MENINGITIS_DATASET2 FOR VISUALIZTION

-- CHECKING FOR DUPLICATED ROWS USING THE ID NUMBER
SELECT id, COUNT(*)
FROM meningitis_dataset3
GROUP BY id
HAVING COUNT(*) > 1;

-- SINCE EACH ROW IS DUPLICATED, WE WILL HAVE TO BUILD A NEW TABLE CALLED MENINGITIS_CLEAN WHICH CONTAINS A UNIQUE ROW OF MENINGITIS_DATASET3, THEN DROP THE MENINGITIS_DATASET3 TABLE

CREATE TABLE MENINGITIS_CLEAN AS
SELECT DISTINCT *
FROM meningitis_dataset3;

-- DROPPING MENINGITIS_DATASET3 TABLE
DROP TABLE meningitis_dataset3;

-- CHECKING IF THERE'S STILL DUPLICATES IN THE NEW TABLE CREATED(MENINGITIS_CLEAN)
SELECT id, COUNT(*)
FROM meningitis_clean
GROUP BY ID
HAVING COUNT(*) > 1; #NO DUPLICATES

-- WORKING ON EACH COLUMN TO SEE IF THEY HAVE ISSUES LIKE SPACES ETC

-- ID COLUMNS 
-- CHECKING FOR SPACES
SELECT ID
FROM meningitis_clean
WHERE ID LIKE ' %' OR '% '; #NO SPACES 

-- CHECKING FOR SPECIAL CHARACTERS 
SELECT ID
FROM meningitis_clean
WHERE ID REGEXP '[^0-9]'; # NO SPECIAL CHARACTERS

-- CLEANING SURNAME
-- CHECKING FOR SPACES
SELECT SURNAME
FROM meningitis_clean
WHERE SURNAME LIKE '% ' OR ' %' ;

-- CHECKING FOR SPECIAL CHARACTERS 
SELECT SURNAME
FROM meningitis_clean
WHERE SURNAME REGEXP '[^a-zA-Z]'; # CONTAINS NAME WITH - BUT THAT'S THE ACTUAL WAY TO WRITE IT, SO WE WONT BE MAKING CHANGES TO IT.

-- CLEANING FIRSTNAME COLUMN 
-- CHECKING FOR SPACES 
SELECT FIRSTNAME
FROM meningitis_clean
WHERE FIRSTNAME LIKE '% ' OR ' %'; # NO SPACES 

-- CLEANING MIDDLE NAME
SELECT MIDDLENAME
FROM meningitis_clean
WHERE middlename LIKE '% ' OR ' %'; # NO SPACES

-- CLEANING GENDER COLUMN
SELECT GENDER
FROM MENINGITIS_CLEAN
WHERE GENDER LIKE '% ' OR ' %';

-- CLEANING STATE COLUMN
SELECT DISTINCT STATE
FROM meningitis_clean; -- NO SPECIAL CHARACTERS

-- CHECKING FOR SPACES
SELECT STATE
FROM meningitis_clean
WHERE GENDER LIKE '% ' OR ' %'; -- NO SPECIAL CHARACTERS

SELECT SETTLEMENT
FROM meningitis_clean
WHERE settlement LIKE '% ' OR ' %'; -- NO SPECIAL CHARACTERS

-- CLEANING REPORT DATE COLUMN
-- CHECKING WHERE REPORT_DATE IS NOT IN DATE LIKE FORMAT
SELECT REPORT_DATE
FROM meningitis_clean
WHERE report_date NOT LIKE '%-%-%'; # THEY ARE ALL IN DATE LIKE FORMAT

-- CHECKING FOR SPACES 
SELECT REPORT_DATE
FROM meningitis_clean
WHERE report_date LIKE '% ' OR ' %'; # NO LEADING OR TRAILING SPACES

-- CLEANING REPORT YEAR COLUMN
SELECT DISTINCT report_year
FROM meningitis_clean;

-- CHECKING FOR SPACES
SELECT REPORT_YEAR
FROM meningitis_clean
WHERE REPORT_YEAR LIKE '% ' OR ' %'; 

-- CLEANING AGE COLUMN
SELECT DISTINCT AGE 
FROM meningitis_clean
ORDER BY AGE ASC;

-- CHECKING FOR SPACES 
SELECT AGE 
FROM meningitis_clean
WHERE AGE LIKE '% ' OR ' %';

-- CLEANING DATE OF BIRTH
-- CHECKING WHERE DATE OF BIRTH IS NOT IN A DATE LIKE FORMAT 
SELECT DATE_OF_BIRTH
FROM meningitis_clean
WHERE date_of_birth NOT LIKE '%-%-%';

-- CHECKING FOR SPACES
SELECT DATE_OF_BIRTH 
FROM meningitis_clean
WHERE DATE_OF_BIRTH LIKE '% ' OR ' %'; # NO SPACES

-- CLEANING DISEASE COLUMN
SELECT DISTINCT DISEASE
FROM MENINGITIS_CLEAN;

-- CHECKING FOR SPACES 
SELECT DISEASE
FROM meningitis_clean
WHERE DISEASE LIKE '% ' OR ' %'; # NO SPACES

-- CLEANING HEALTH STATUS COLUMN
SELECT DISTINCT HEALTH_STATUS
FROM meningitis_clean; # JUST TOW ROWS(ALIVE, DEAD)

-- CHECKING FOR SPACES
SELECT HEALTH_STATUS
FROM meningitis_clean
WHERE health_status LIKE '% ' OR ' %'; # NO SPACES

-- CLEANING REPORT_OUTCOME
SELECT DISTINCT REPORT_OUTCOME
FROM meningitis_clean;

-- CHECKING FOR SPACES
SELECT REPORT_OUTCOME
FROM meningitis_clean
WHERE REPORT_OUTCOME LIKE '% ' OR ' %'; # NO SPACES

SELECT *
FROM meningitis_clean; 

-- EXPLORATORY ANALYSIS

-- NUMBER OF CASES
SELECT COUNT(ID)
FROM meningitis_clean; # 284,484 OF CASES

-- NUMBER OF CASES BY GENDER
SELECT GENDER, COUNT(*)
FROM meningitis_clean
GROUP BY GENDER
ORDER BY COUNT(*) DESC; # FEMALE(147,272), MALE(137,212)

-- NUMBER OF DEATH CASES BY STATE
SELECT STATE, COUNT(*) AS NUMBER_OF_DEATH_CASES
FROM meningitis_clean
WHERE HEALTH_STATUS = 'DEAD'
GROUP BY STATE
HAVING COUNT(*)
ORDER BY COUNT(*) DESC; # ONDO AND BAYELSA HAVE THE HIGHEST NUMBER OF DEAD CASES(3,970)

-- NUMBER OF OF CONFIRMED CASES BY STATE
SELECT STATE, COUNT(*) AS NUMBER_OF_CONFIRMED_CASES
FROM meningitis_clean
WHERE report_outcome = 'CONFIRMED'
GROUP BY STATE
HAVING COUNT(*)
ORDER BY COUNT(*) DESC; # ONDO AND NIGER STATE HAVE THE HIGHEST NUMBER OF CONFIRMED CASES 

-- SETTLEMENT WITH HIGHEST CASE(RURAL OR URBAN)
SELECT settlement, COUNT(*)
FROM meningitis_clean
GROUP BY settlement
ORDER BY COUNT(*) DESC; # RURAL SETTLEMENT HAS THE HIGHEST CASE OF DISEASE(142,563)

-- YEAR with the highest number of cases in descending order
SELECT REPORT_YEAR, COUNT(*)
FROM meningitis_clean
GROUP BY report_year
ORDER BY COUNT(*) DESC; # 2013 HAS THE HIGHEST NUMBER OF DISEASE CASES WITH THE NUMBER OF 28,745 CASES BEEN RECORDED.

-- DISEASES BY RATE OF OCCURANCE
SELECT DISEASE, COUNT(*)
FROM meningitis_clean
GROUP BY disease
ORDER BY COUNT(*) DESC; -- CHOLERA HAS THE NUMBER OF CASES 
```

### Results/Findings

The analysis results are summarized as follows:
1. Cholera had the highest reported cases (28,589).
2. Female cases (147,272) exceeded male cases (137,212).
3. Rural and urban cases were nearly equal (~142,000 each).
4. Most cases were reported in states like Niger, Ondo, and Kano.
5. Fluctuations in cases were observed over the years, peaking in 2013.
6. Females had more reported cases (147,272) than males (137,212). Females also had a higher number of death cases (73,810) compared to males (68,479)

### Recommendations

Based on the analysis, we recommend the following actions:
#### 1. Strengthen Disease Prevention & Control Measures
- Target High-Prevalence Diseases: Prioritize prevention programs for Cholera, Diarrhea, and Rubella, which had the highest reported cases.
- Improve Hygiene & Sanitation: Implement public health campaigns promoting clean water, sanitation, and food safety to reduce waterborne diseases.
#### 2. Address Gender-Specific Health Disparities
- Enhance Healthcare Access for Women: Since females had higher infection and death rates, ensure targeted health interventions and timely medical care for women.
- Further Research on Gender-Based Risk Factors: Investigate whether socioeconomic roles, healthcare access, or biological factors contribute to the disparity.
#### 3. Enhance Public Health Awareness & Vaccination Campaigns
- Encourage Immunization: Promote vaccines for preventable diseases like Measles and Rubella.
- Community Education Programs: Conduct awareness campaigns on symptoms, prevention, and treatment access to reduce mortality rates.
#### 4. Monitor Yearly Trends & Predict Future Outbreaks
- Investigate Fluctuations: The spikes in 2011, 2013, and 2017 suggest possible seasonal or environmental triggers for outbreaks.
- Develop Early Warning Systems: Use predictive analytics to anticipate future outbreaks and prepare intervention strategies.
#### 5. Improve Data Collection & Reporting Systems
- Enhance Surveillance & Digital Reporting: Invest in real-time disease tracking and better data collection to detect outbreaks faster.
- Encourage Open Data Sharing: Collaborate with global health organizations and researchers to improve responses.


### Limitations

I had to remove some columns from the dataset because we were focused on analyzing disease trends, mortality rates, gender distribution, settlement impact, and high-risk states. Columns that were irrelevant to these key areas or did not contribute to meaningful insights were excluded to ensure a cleaner and more targeted analysis.
