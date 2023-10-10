import pandas as pd
import numpy as np

# Read your data into a DataFrame (replace 'your_data.csv' with your actual data file)
data_df = pd.read_csv("your_data.csv")

# Check if 'TV available' is 'yes' and columns 'a', 'b', and 'c' have no null values
condition = (data_df['TV available'] == 'yes') & \
            (data_df[['a', 'b', 'c']].notnull().all(axis=1))

# Create a list of IDs where the condition is not met
null_value_ids = data_df[~condition]['id'].tolist()

# Print or return the list of IDs with null values in columns 'a', 'b', or 'c' when 'TV available' is 'yes'
print("IDs with null values in 'a', 'b', or 'c' when 'TV available' is 'yes':", null_value_ids)
