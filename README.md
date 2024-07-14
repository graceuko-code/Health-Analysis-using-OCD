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

- What is the total number of patients diagnosed per year?
- What is the number and percentage of patients by gender?
- What is the number and percentage of patients by ethnicity?
- What is the number and percentage of patients by type of compulsion?
- What is the most common Obsession Type (count) & its respective Average Obsession Score?

### Analysis

#### Descriptive Statistics using SQL

```sql
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
