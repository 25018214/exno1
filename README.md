# Exno:1
Data Cleaning Process

# AIM
To read the given data and perform data cleaning and save the cleaned data to a file.

# Explanation
Data cleaning is the process of preparing data for analysis by removing or modifying data that is incorrect ,incompleted , irrelevant , duplicated or improperly formatted. Data cleaning is not simply about erasing data ,but rather finding a way to maximize datasets accuracy without necessarily deleting the information.

# Algorithm
STEP 1: Read the given Data

STEP 2: Get the information about the data

STEP 3: Remove the null values from the data

STEP 4: Save the Clean data to the file

STEP 5: Remove outliers using IQR

STEP 6: Use zscore of to remove outliers

# Coding and Output
            <<include your coding and its corressponding output screen shots here>>

#DATASET.CSV
```
mport pandas as pd

df = pd.read_csv("Data_set.csv")

print("Missing values before cleaning:")
print(df.isnull().sum())

categorical_cols = ["show_name", "aired_on", "original_network"]

for col in categorical_cols:
    df[col] = df[col].fillna("Unknown")

numeric_cols = ["rating", "current_overall_rank", "watchers"]

for col in numeric_cols:
    df[col] = df[col].fillna(df[col].median())

df["current_overall_rank"] = df["current_overall_rank"].round().astype("Int64")

print("\nMissing values after cleaning:")
print(df.isnull().sum())

df.to_csv("Data_set_cleaned.csv", index=False)

print("\nCleaned dataset saved successfully!")
```
<img width="1366" height="720" alt="Screenshot 2026-11-02 151018" src="https://github.com/user-attachments/assets/4519d391-3c08-4ac7-bc14-82062938c7d3" />


#HEIGTHS.CSV
```
import pandas as pd

df = pd.read_csv("heights.csv")

Q1 = df["height"].quantile(0.25)
Q3 = df["height"].quantile(0.75)

IQR = Q3 - Q1

lower = Q1 - 1.5 * IQR
upper = Q3 + 1.5 * IQR

outliers = df[(df["height"] < lower) | (df["height"] > upper)]

print("Unusual height measurements:")
print(outliers)
```

<img width="1366" height="720" alt="Screenshot 2026-11-02 151139" src="https://github.com/user-attachments/assets/d041c213-f169-4d43-a2f4-e9344f9b64c7" />


#IRIS.CSV
```
import pandas as pd

df = pd.read_csv("iris.csv")

cols = df.select_dtypes(include="number").columns

for col in cols:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    outliers = df[(df[col] < Q1 - 1.5 * IQR) |
                  (df[col] > Q3 + 1.5 * IQR)]

    print("IQR outliers in", col)
    print(outliers[col])

for col in cols:
    z = (df[col] - df[col].mean()) / df[col].std()

    print("Z-score outliers in", col)
    print(df.loc[z.abs() > 3, col])
```
<img width="1366" height="720" alt="Screenshot 2026-11-02 151341" src="https://github.com/user-attachments/assets/c5f68c70-3177-40bf-b608-629b077f6ad3" />

#LOAN_DATA.CSV
```
import pandas as pd
import numpy as np
from scipy import stats
import matplotlib.pyplot as plt

# LOAD DATA
df = pd.read_csv("Loan_data.csv")
print("\nRAW DATA (first 10 rows)\n", df.head(10))
print("\nMissing values per column\n", df.isnull().sum())
print("\nDuplicate rows:", df.duplicated().sum())
```
<img width="1366" height="720" alt="Screenshot 2026-11-02 151526" src="https://github.com/user-attachments/assets/4fb4b04f-0015-460b-b4d1-175d805f8219" />

<img width="1366" height="768" alt="Screenshot 2026-11-02 151542" src="https://github.com/user-attachments/assets/65346fc1-52ac-4efa-8f09-3fe02123b000" />

# Result
          <<include your Result here>>
