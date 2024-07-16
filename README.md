# OCD HEALTH ANALYSIS

### Project Overview

This is to find out some facts about people living with OCD disease. This analysis aims to provide detailed insights into the prevalence, trends, and characteristics of OCD across different demographic groups. The dataset is obtained from Kaggle. It is a fictional dataset, so facts from this dataset shouldn’t be used for real-life assumptions.

### Background

Obsessive-Compulsive Disorder (OCD) is a chronic mental health condition characterized by persistent, unwanted thoughts (obsessions) and repetitive behaviors or mental acts (compulsions) that an individual feels compelled to perform. These obsessions and compulsions can significantly interfere with daily life and activities.

### Data Sources

OCD Patient Dataset: This dataset is obtained from Kaggle [ocd_patient_dataset.csv](https://www.kaggle.com/datasets/ohinhaque/ocd-patient-dataset-demographics-and-clinical-data)

### Tools

- Excel - Data Cleaning 
- SQL - Data Analysis
- Power BI - Creating the Report

### Data Cleaning and Preparation

In the initial data preparation phase, we perform the following tasks:
1. Data loading and inspection
2. Handling missing values
3. Data cleaning and formatting

### Exploratory Data Analysis
This involved exploring the dataset to answer key questions, such as:

- What is the total number of patients diagnosed per Month?
- What is the number and percentage of patients by gender?
- What is the number and percentage of patients by ethnicity?
- What is the number and average score of patients by type of compulsion?
- What is the most common Obsession Type (count) & its respective Average Obsession Score?

### Analysis

#### Descriptive Statistics using SQL

 Giving answers to the key questions; 

###### What is the total number of patients diagnosed per Month?

```SQL
-- First convert the datatype from text to date
SELECT 
  CAST(REPLACE(`OCD Diagnosis Date`, '-', '/') AS Date) 
FROM
  ocd_patient.ocd_patient_dataset;
UPDATE ocd_patient.ocd_patient_dataset 
SET `OCD Diagnosis Date` = REPLACE(`OCD Diagnosis Date`, '-', '/');
ALTER TABLE ocd_patient.ocd_patient_dataset CHANGE COLUMN `OCD Diagnosis Date` `OCD Diagnosis Date` VARCHAR(20);
UPDATE ocd_patient.ocd_patient_dataset 
SET `OCD Diagnosis Date` = DATE_FORMAT(STR_TO_DATE(`OCD Diagnosis Date`, '%d/%m/%Y'), '%Y-%m-%d');
ALTER TABLE ocd_patient.ocd_patient_dataset MODIFY COLUMN `OCD Diagnosis Date` DATE;
DESC ocd_patient.ocd_patient_dataset;

-- Count the number of patients diagnosed with OCD Per Month
SELECT 
  DATE_FORMAT(`OCD Diagnosis Date`, '%Y-%m-01 00:00:00') AS Month, 
  COUNT(`Patient ID`) patien_count 
FROM
  ocd_patient.ocd_patient_dataset 
GROUP BY 
  1 
ORDER BY
  1;
```

###### What is the number and percentage of patients by gender?

```SQL
WITH DATA AS(
  SELECT 
    Gender, 
    COUNT(`Patient ID`) AS Patient_count, 
    ROUND(AVG(`Y-BOCS Score (Obsessions)`), 2) AS avg_obs_score 
  FROM 
    ocd_patient_dataset 
  GROUP BY 1
) 
SELECT 
  SUM(CASE WHEN Gender = 'Female' THEN patient_count ELSE 0 END) AS count_female, 
  SUM(CASE WHEN Gender = 'Male' THEN patient_count ELSE 0 END) AS count_male, 
  ROUND(SUM(CASE WHEN Gender = 'Female' THEN patient_count ELSE 0 END)/ 
  (SUM(CASE WHEN Gender = 'Female' THEN patient_count ELSE 0 END)+ 
  SUM(CASE WHEN Gender = 'Male' THEN patient_count ELSE 0 END))* 100, 2 ) AS pct_female, 

  ROUND(SUM(CASE WHEN Gender = 'Male' THEN patient_count ELSE 0 END)/ 
  (SUM(CASE WHEN Gender = 'Female' THEN patient_count ELSE 0 END)+
  SUM(CASE WHEN Gender = 'Male' THEN patient_count ELSE 0 END))* 100, 2) AS pct_male 
FROM 
  DATA;
```

###### What is the number and percentage of patients by ethnicity?

```SQL
SELECT 
  Ethnicity, 
  COUNT(`Patient ID`) AS Patient_count, 
  AVG(`Y-BOCS Score (Obsessions)`) AS Obs_score 
FROM 
  ocd_patient.ocd_patient_dataset 
GROUP BY 
  1;
```

###### What is the number and average score of patients by type of compulsion?

```SQL
SELECT 
  `Compulsion Type`, 
  COUNT(`Patient ID`) AS patient_count, 
  ROUND(AVG(`Y-BOCS Score (Compulsions)`), 2) AS comp_score 
FROM 
  ocd_patient.ocd_patient_dataset 
GROUP BY 1 
ORDER BY 2;
```

###### What is the most common Obsession Type (count) & its respective Average Obsession Score?

```SQL
SELECT 
  `Obsession Type`, 
  COUNT(`Patient ID`) AS patient_count, 
  ROUND(
    AVG(`Y-BOCS Score (Obsessions)`), 2) AS obs_score 
FROM
  ocd_patient.ocd_patient_dataset 
GROUP BY 1 
ORDER BY 2;
```

### Key Findings

1. Gender Distribution
   
   - **Male Patients:** 50.2% (753 Patients)
   - **Female Patients:** 49.8% (747 Patients)
   - There was no significant gender difference observed in the severity of OCD symptoms.

2. Ethnicity Breakdown:

   - **Caucasian:** 398 patients
   - **Hispanic:** 392 patients
   - **Asian:** 386 patients
   - **African:** 324 patients
   - Ethnic differences in distribution were noted, with Caucasian patients reporting the highest number of patients and Africans reporting the lowest number of patients.

3. Types of Compulsions:

   - **Washing:** 320 patients
   - **Counting:** 316 patients
   - **Checking:** 292 patients
   - **Praying:** 286 patients
   - **Ordering:** 285 patients

4. Types of Obsessions:
 
   - **Harm-related:** 333 patients
   - **Contamination:** 306 patients
   - **Religious:** 303 Patients
   - **Symmetry:** 280 patients
   - **Hoarding:** 278 patients

5. Number of Diagnoses:
   - OCD patients were diagnosed every month from November 2013 to November 2022.
   - The highest number of patients was observed in March 2018, which was 25, and the lowest number was observed in November 2018, which was 2 patients.

### Conclusion

This analysis of OCD health data is focused on gender, ethnicity, compulsion types, obsession scores, and Number of patient diagnoses. It exposes some key facts among patients with OCD disease as it contributes to a better understanding and management of OCD across diverse populations.
