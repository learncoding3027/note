import os
import glob
import pandas as pd

# Your existing code here...

# Initialize a list to store the 'TOWNVILLAGECODE' and 'Pin-code' from recruitment files
recruitment_data = []

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file, index_col=False)
    
    # Extract 'TOWNVILLAGECODE' and 'Pin-code' columns
    recruitment_data.append(recruitment_df[['TOWNVILLAGECODE', 'PINCODE', 'RURALURBAN']])

# Concatenate data from all recruitment files
recruitment_data_combined = pd.concat(recruitment_data, ignore_index=True)

# Merge 'urban_check' with 'recruitment_data_combined' on 'Pin-code'
urban_check = pd.merge(urban_check, recruitment_data_combined, on='Pin-code', how='left')

# Initialize a list to store survey IDs where the combination of TOWNVILLAGECODE and "rural/urban" is "rural"
rural_ids = []

# Iterate through the rows of the survey DataFrame
for index, survey_row in urban_check.iterrows():
    survey_id = survey_row["Unique ID"]
    survey_pincode = survey_row["PINCODE"]
    rural_urban_value = survey_row["RURALURBAN"]

    # Check if the 'RURALURBAN' value is 'Rural'
    if rural_urban_value == "Rural":
        rural_ids.append(survey_id)

# Print the list of survey IDs where the combination of TOWNVILLAGECODE and "rural/urban" is "rural"
print("Survey IDs where the combination of TOWNVILLAGECODE and 'rural/urban' is 'Rural':", rural_ids)












import os
import glob
import pandas as pd

# Your existing code here...

# Initialize a list to store the 'TOWNVILLAGECODE' and 'Pin-code' from recruitment files
recruitment_data = []

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file, index_col=False)
    
    # Extract 'TOWNVILLAGECODE' and 'Pin-code' columns
    recruitment_data.append(recruitment_df[['TOWNVILLAGECODE', 'PINCODE', 'RURALURBAN']])

# Concatenate data from all recruitment files
recruitment_data_combined = pd.concat(recruitment_data, ignore_index=True)

# Merge 'urban_check' with 'recruitment_data_combined' on 'Pin-code'
urban_check = pd.merge(urban_check, recruitment_data_combined, on='Pin-code', how='left')

# Initialize a set to store unique "rural" TOWNVILLAGECODEs from recruitment files
rural_town_village_codes = set()

# Iterate through the recruitment data
for index, row in recruitment_data_combined.iterrows():
    if row['RURALURBAN'] == "Rural":
        rural_town_village_codes.add(row['TOWNVILLAGECODE'])

# Initialize a list to store survey IDs where the combination of TOWNVILLAGECODE and "rural/urban" is "rural"
rural_ids = []

# Iterate through the rows of the survey DataFrame
for index, survey_row in urban_check.iterrows():
    survey_id = survey_row["Unique ID"]
    survey_town_village_code = survey_row["TOWNVILLAGECODE"]
    
    # Check if the survey TOWNVILLAGECODE is in the set of "rural" TOWNVILLAGECODEs
    if survey_town_village_code in rural_town_village_codes:
        rural_ids.append(survey_id)

# Print the list of survey IDs where the combination of TOWNVILLAGECODE and "rural/urban" is "rural"
print("Survey IDs where the combination of TOWNVILLAGECODE and 'rural/urban' is 'rural':", rural_ids)

