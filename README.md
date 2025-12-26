Project Overview: Predictive Targeting for Financial Services
This project focuses on leveraging Diagnostic and Predictive Analytics to optimize the direct marketing campaign for a major financial institution's Term Deposit product.

The primary goal was to move beyond costly mass-marketing by identifying the key customer characteristics and behaviors that drive subscription success. The analysis provides a data-driven framework for improving campaign Return on Investment ##(ROI) and significantly reducing operational marketing costs.

Technical Methodology & Code Highlights
The entire analytical workflow, from data preparation to visualization and insight generation, was implemented using Python's core data science stack.

Step 1: Data Setup and Feature Engineering
This critical step involved preparing the raw data and engineering a high-impact feature to simplify the complex customer contact history.
import pandas as pd
import numpy as np

# Load the raw dataset
df = pd.read_csv("bank-full.csv", sep=';') 

# --- CRITICAL FEATURE ENGINEERING: 'Was_C' ---
# The original 'pdays' column records days since the last contact (or -1 if not contacted).
# We transform this into a powerful binary predictor: 
# 'No' = Client was never successfully contacted in a previous campaign (pdays < 0).
# 'Yes' = Client had a previous contact (pdays >= 0).
df['Was_C '] = np.where(df['pdays'] < 0, 'No', 'Yes') 

# Convert the target variable 'y' to numeric (1=Yes, 0=No) for easy calculation of mean/conversion rate
df['y'] = df['y'].replace({'yes': 1, 'no': 0})

Step 2: Diagnostic Analysis (Exploratory Data Analysis - EDA)
In-depth analysis was performed on key variables to isolate segments with the highest propensity to subscribe. The mean of the binary target variable (df['y'].mean()) is used to calculate the conversion rate for each segment.

A. Impact of Contact History (Was_C )
This analysis directly validates the power of the newly engineered feature by comparing conversion rates.
print("Conversion Rate by Previous Contact Success:")
conversion_by_contact = df.groupby('Was_C ')['y'].mean()
print(conversion_by_contact)

# Visualizing the difference:
conversion_by_contact.plot(kind='bar', title='Conversion Rate by Previous Contact')

B. Impact of Marital Status
Determining which marital status segment converts at the highest rate.
print("Conversion Rate by Marital Status:")
conversion_by_marital = df.groupby('marital')['y'].mean().sort_values(ascending=False)
print(conversion_by_marital)

C. Impact of Age Group
Binning the continuous age variable to identify optimal demographic brackets for targeting.
# Define bins (example groups: young, middle-aged, senior)
age_bins = [18, 30, 45, 60, 100] 
df['Age_Group'] = pd.cut(df['age'], bins=age_bins)

print("Conversion Rate by Age Group:")
conversion_by_age = df.groupby('Age_Group')['y'].mean()
conversion_by_age.plot(kind='bar', title='Conversion Rate by Age Group')

D. Impact of Call Duration
Analysis of the strong correlation between call length (duration) and a successful subscription.
# Create duration bins (e.g., in minutes) to show the conversion rate increase
duration_bins = [0, 60, 180, 360, 9999] 
df['Duration_Group'] = pd.cut(df['duration'], bins=duration_bins)

print("Conversion Rate by Call Duration:")
df.groupby('Duration_Group')['y'].mean().plot(kind='bar')

E. Month Analysis
Identifying which campaign months yield the highest success rates for better resource planning.
print("Conversion Rate by Campaign Month (Top 3):")
conversion_by_month = df.groupby('month')['y'].mean().sort_values(ascending=False)
print(conversion_by_month.head(3))

F. Impact of Job Title
Identifying professional segments that exhibit the highest propensity to subscribe to the Term Deposit product.
print("Conversion Rate by Job Title (Top 5):")
conversion_by_job = df.groupby('job')['y'].mean().sort_values(ascending=False)
print(conversion_by_job.head(5))

# Used for visualization in the presentation deck
# conversion_by_job.plot(kind='barh', title='Conversion by Job Title')

Business Outcome
The generated insights provide an optimized targeting strategy:

Efficiency: The bank can strategically reduce the outreach list by filtering out low-propensity customers (e.g., those with poor contact history or in low-performing demographic/timing segments).

ROI: Campaign conversion rates are significantly increased in the newly defined target segments, boosting overall profitability.



