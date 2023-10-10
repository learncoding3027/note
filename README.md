import pandas as pd

# Read your data into a DataFrame (replace 'your_data.csv' with your actual data file)
data_df = pd.read_csv("your_data.csv")

# Define the list of columns to check for null values
columns_to_check = [
    'State', 'District', 'Sub-District', 'Town / Village name',
    'BARC Town / Village code', 'Census town / Village code', 'Pin-code',
    'Age Group', 'Highest Education of CWE', 'Durables', 'Two-Wheeler',
    'Car/Jeep/Van', 'Agricultural Land', 'Black & White TV',
    'Fridge-Refrigerator', 'Air-Conditioner', 'Washing Machine',
    'Color TV/LCD/LED/PLASMA TV', 'Personal Computer / Laptop',
    'Electricity Connection (availability of electricity)', 'Ceiling Fan',
    'Gas Stove Stove/PNG stove', 'TV Owning', 'Total number of Durables',
    'NCCS', 'Highest Education', 'Working Status', 'Going Out',
    'Rank Monday', 'Rank Tuesday', 'Rank Wednesday', 'Rank Thursday',
    'Rank Friday', 'Rank Saturday', 'Rank Sunday', 'Visiting Eatery Type',
    'Visiting eateries most often', 'Last visit to eatery',
    'Last visit eatery with whom'
]

# Filter the DataFrame to rows where 'TV availability at the eatery' is 'Yes'
filtered_df = data_df[data_df['TV availability at the eatery'] == 'Yes']

# Iterate through the specified columns and check for 'NA' (null) values
null_value_ids = []
for column in columns_to_check:
    null_rows = filtered_df[filtered_df[column].isna()]
    if not null_rows.empty:
        null_value_ids.extend(null_rows['Unique ID'].tolist())

# Remove duplicate IDs if needed
null_value_ids = list(set(null_value_ids))

# Print or return the list of 'Unique ID' values with null values in the specified columns
print("Unique IDs with null values in specified columns:", null_value_ids)
