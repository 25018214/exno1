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

```
import pandas as pd


df = pd.read_csv("Loan_data.csv")


print("Missing values before cleaning:")
print(df.isnull().sum())


for col in df.select_dtypes(include="number").columns:
    df[col] = df[col].fillna(df[col].median())


for col in df.select_dtypes(include="object").columns:
    if df[col].isnull().sum() > 0:
        df[col] = df[col].fillna(df[col].mode()[0])


print("\nMissing values after cleaning:")
print(df.isnull().sum())


print("\nCleaned Dataset:")
print(df.head())

df.to_csv("Cleaned_data.csv", index=False)

```

<img width="507" height="462" alt="Screenshot 2026-11-03 150705" src="https://github.com/user-attachments/assets/6d1938b3-9e7c-4cb3-a60f-2b06199e747b" />


```
import pandas as pd

df = pd.read_csv("Loan_data.csv")

print("Original Dataset:")
print(df.head())


num_cols = df.select_dtypes(include="number").columns

print("\nUnusual Measurements (Outliers):")

for col in num_cols:
    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)

    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    outliers = df[(df[col] < lower) | (df[col] > upper)]

    print("\nColumn:", col)
    print("Lower limit:", lower)
    print("Upper limit:", upper)
    print("Number of outliers:", len(outliers))

    if not outliers.empty:
        print(outliers[[col]])

print("\nOutlier detection completed!")
```

<img width="434" height="517" alt="Screenshot 2026-11-03 151245" src="https://github.com/user-attachments/assets/43cf059c-e463-40f2-8e4d-eee0389c4615" />



<img width="345" height="517" alt="Screenshot 2026-11-03 151303" src="https://github.com/user-attachments/assets/bd539f9a-ad30-4c14-af72-8059e2236037" />



<img width="374" height="421" alt="Screenshot 2026-11-03 151327" src="https://github.com/user-attachments/assets/50763b23-3c33-4b69-894d-920439332bd1" />



<img width="327" height="484" alt="Screenshot 2026-11-03 151338" src="https://github.com/user-attachments/assets/ce99dcee-573f-4287-8498-364b4adaa613" />

```
import pandas as pd
from scipy.stats import zscore

df = pd.read_csv("Loan_data.csv")

num_cols = df.select_dtypes(include="number").columns

for col in num_cols:
    print("\nColumn:", col)

    Q1 = df[col].quantile(0.25)
    Q3 = df[col].quantile(0.75)
    IQR = Q3 - Q1

    lower = Q1 - 1.5 * IQR
    upper = Q3 + 1.5 * IQR

    iqr_outliers = df[(df[col] < lower) | (df[col] > upper)]
    print("IQR outliers:", len(iqr_outliers))
    print(iqr_outliers[[col]])

    z = zscore(df[col], nan_policy="omit")
    z_outliers = df[abs(z) > 3]

    print("Z-score outliers:", len(z_outliers))
    print(z_outliers[[col]])

print("\nOutlier detection completed!")
```
<img width="320" height="513" alt="Screenshot 2026-11-03 151534" src="https://github.com/user-attachments/assets/55f23101-48d3-4f5b-adf7-a49f3e144560" />



<img width="243" height="506" alt="Screenshot 2026-11-03 151552" src="https://github.com/user-attachments/assets/6cd372be-55e2-4c4b-9068-0a6a19a0582a" />



<img width="240" height="520" alt="Screenshot 2026-11-03 151603" src="https://github.com/user-attachments/assets/c8bcbe6e-e284-412d-8b26-c457bc35648e" />



<img width="243" height="520" alt="Screenshot 2026-11-03 151611" src="https://github.com/user-attachments/assets/452bd371-a370-4363-8e73-815868d89ea2" />

#SAMPLEIDS.CSV
```
import pandas as pd

df = pd.read_csv("SAMPLEIDS.csv")

df = df.drop_duplicates()

print("Missing values:")
print(df.isnull().sum())

print("Duplicate registration numbers:")
print(df[df.duplicated("REGNO", keep=False)])

marks = ["M1", "M2", "M3", "M4"]

print("Inconsistent total marks:")
print(df[df["TOTAL"] != df[marks].sum(axis=1, min_count=4)])

df.to_csv("SAMPLEIDS_cleaned.csv", index=False)

print("Cleaning completed!")
```
<img width="379" height="338" alt="Screenshot 2026-11-03 153558" src="https://github.com/user-attachments/assets/0d8434b4-233b-4a97-9f6e-5d5f33f04b0b" />


# Result
          <<include your Result here>>
