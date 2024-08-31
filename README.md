# EDA-on-Google-PlayStore

#impoting the library

import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt
import warnings
warnings.filterwarnings('ignore')

df = pd.read_csv("googleplaystore.csv")  #data from csv file
df.columns
df.head()
df.describe()     #stats inform for numerical data #five point summary

df.duplicated().sum()
df = df.drop_duplicates()
df
![Screenshot 2024-08-31 173800](https://github.com/user-attachments/assets/e26b3796-952f-4a5d-9c91-4f1ac1d817af)


![Screenshot 2024-08-31 174017](https://github.com/user-attachments/assets/057ae780-9d8b-4ca7-a6e0-6eaa44533945)

df.dtypes
![Screenshot 2024-08-31 174031](https://github.com/user-attachments/assets/923f9f60-06db-491f-9c15-a50290e904ac)

df['Content Rating'].dtype
df['Reviews']
df['Reviews'].dtype
type(df.Reviews)
df.Reviews
df[~df.Reviews.str.isnumeric()]
df_copy = df.copy()
df_copy
![Screenshot 2024-08-31 174712](https://github.com/user-attachments/assets/a4f35afc-602a-47a3-b51c-baee75719501)

#restting the index as after dropping duplicates original index is still there

df_copy.reset_index(drop = True, inplace=True)
df_copy
df_copy[~df_copy.Reviews.str.isnumeric()]
df_copy = df_copy.drop(df_copy.index[9990])
df_copy[~df_copy.Reviews.str.isnumeric()]
df_copy['Reviews'] = df_copy['Reviews'].astype(int)
df_copy.Reviews.dtype
df_copy.info()
df_copy["Size"].unique()
item = '19M'
item[-1]
item.replace('M', '')


#1 MB = 1024 KB\

def size_process(item):
    if str(item)[-1] == 'M':
        res = float(str(item).replace('M', ''))
        res = res*1024
        return res
    elif str(item)[-1] == 'k':
        res = float(str(item).replace('k', ''))
        return res
    else:
        return str(np.NaN)

df_copy['Size'] = df_copy['Size'].apply(size_process)
df_copy.Size.dtype
df_copy.Size = df_copy.Size.astype('float')
df_copy.Size.dtype
df_copy.info()
df_copy['Installs']
df_copy['Installs'] = df_copy['Installs'].str.replace("+", "").str.replace(",", "")
df_copy['Installs'].astype(int)
df_copy["Price"].unique()


char_to_remove = ["+", ",", "$"] 
cols_to_remove = ["Installs", "Price"]

for char in char_to_remove:
    for col in cols_to_remove:
        df_copy[col] = df_copy[col].str.replace(char, "")


df_copy['Installs'] = df_copy['Installs'].astype(int)
df_copy['Price'] = df_copy['Price'].astype(float)
![Screenshot 2024-08-31 175329](https://github.com/user-attachments/assets/772a60af-1b21-4f19-afdc-86318c375767)

df_copy['Last Updated']
df_copy['Last Updated'] = pd.to_datetime(df_copy['Last Updated'])
df_copy['day'] = df_copy['Last Updated'].dt.day
df_copy['month'] = df_copy['Last Updated'].dt.month
df_copy['year'] = df_copy['Last Updated'].dt.year
df_copy.dtypes
df_copy['Android Ver'].unique()
df_copy['Android Ver'] = df_copy['Android Ver'].str.replace("and up", "").str.replace("Varies with device","")
df_copy['Android Ver'].unique()
df_copy.App
df[df.duplicated("App")]

![Screenshot 2024-08-31 175523](https://github.com/user-attachments/assets/2af8949d-bc55-4f4e-b1e1-d5f6d6071e22)
df_copy = df_copy.drop_duplicates(subset = ["App"], keep = 'first')
df_copy[df_copy.duplicated("App")]
df_copy.dtypes
df_copy.columns
#EDA
categorical_features = [feature for feature in df_copy.columns if df_copy[feature].dtype == 'O']
numerical_features = [feature for feature in df_copy.columns if df_copy[feature].dtype != 'O']
numerical_features
df_copy[categorical_features]
#categroical feature analysis
df_copy["Type"].value_counts(normalize = True)*100

for col in categorical_features:
    print(f"{col} : {df_copy[col].value_counts(normalize = True)*100}")

df_copy["Android Ver"].value_counts(normalize = True)*100
df_copy["Type"].value_counts(normalize = True)*100
![Screenshot 2024-08-31 175827](https://github.com/user-attachments/assets/5998e649-e66f-4436-9cf8-c5de22d89630)

![Screenshot 2024-08-31 175850](https://github.com/user-attachments/assets/2f8d5bbb-2296-496d-bb74-0861fa25ccf8)

![Screenshot 2024-08-31 175920](https://github.com/user-attachments/assets/03ca8597-9dbf-4c87-b0d0-74623e5c694b)

![Screenshot 2024-08-31 175928](https://github.com/user-attachments/assets/e42638ae-9f1d-44c8-85aa-8619edb8ea4c)
![Screenshot 2024-08-31 175936](https://github.com/user-attachments/assets/f43f51da-9251-433b-88bf-c9bafff5121e)
![Screenshot 2024-08-31 180012](https://github.com/user-attachments/assets/fcda8798-0452-4f7a-bf35-7fe90628a96b)

#Q. Which category is the most popular category in the app?

#pie chart

df_copy["Category"].value_counts().plot.pie(y = df["Category"], figsize = (12, 12), autopct = '%1.1f%%')
![Screenshot 2024-08-31 180248](https://github.com/user-attachments/assets/f1422d27-c641-4df2-8353-83fb9a9becc8)

#family is the most popular category with 19 % of share
#Q. what is the top 10 important category
cat = df_copy["Category"].value_counts()[:10]
category = cat.reset_index()
category.columns = ["Groups", "count"]
sns.barplot(category, x = category['Groups'], y = category['count'])
![Screenshot 2024-08-31 180415](https://github.com/user-attachments/assets/78643332-c0a6-423b-9709-2f31940324cd)

#data from google playstore csv file




