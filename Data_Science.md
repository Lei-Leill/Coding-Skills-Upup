# Pandas

## Understand the data
```
import panda as pd
data = pd.read_csv('file_name')
data.columns #打印column名字
data.head() #打印前5行
data[column_name].describe() #count, mean, min, 25%, 75%, max..
data.info()
data.dtypes # data type for each column 
```

## Row and Column in pd

```
data[column_name] #某一列
data.loc[0] #第一行
data.loc[2:8] # 两边都是included
df[df['column'] > value] # filters rows based on a condition on particular column
```

## Manipulate the data and Filtering and Data Clea
```
df1 = pd.DataFrame({'ID': [1, 2, 3], 'Name': ['Alice', 'Bob', 'Charlie']})
df2 = pd.DataFrame({'ID': [2, 3, 4], 'Age': [30, 25, 35]})

df['Job'] = ['student', 'teacher', 'engineer'] # Adding columns 
df.drop(columns=['ID'], axis = 1) # drop column
df.drop(index=[0, 1]) # drop row 0 and 1

result = pd.merge(df1, df2, on='ID', how='inner') # Inner Merge (only matching rows):
result = pd.merge(df1, df2, on='ID', how='left') # Left Merge (all rows from the left DataFrame)
result = pd.merge(df1, df2, on='ID', how='right') # Right Merge (all rows from the right DataFrame)
```
```
df1 = pd.DataFrame({'A': [1, 2], 'B': [3, 4]})
df2 = pd.DataFrame({'A': [5, 6], 'B': [7, 8]})

result = pd.concat([df1, df2], ignore_index=True) # horizontal
result = pd.concat([df1, df2], axis = 1) # vertical
```
## Groupby and aggregrate
```
df = pd.DataFrame({'Category': ['A', 'A', 'B', 'B', 'A'], 'Value': [10, 20, 30, 40, 50]})
grouped = df.groupby('Category').sum()  # Group by 'Category' and sum the 'Value'
result = df.groupby('Category').agg({'Value': ['sum', 'mean', 'max']})
result = df.groupby('Category')['Value'].agg(lambda x: x.max() - x.min())  # Range of 'Value' per 'Category'
grouped = df.groupby(['Category1', 'Category2']).sum()  # Group by both 'Category1' and 'Category2'

```


