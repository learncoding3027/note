this code is not working so just can we just create key of pincode and townvillage code from urban check and from recuritment file do same create key of pincode and townvillage code where urban/rural =rural and then wwe will have only key of rural now search for same key present in urban ...if present just give me id where there is rural.



urban_check = df2[['Unique ID', 'Pin-code']] 
urban_check.rename(columns={'Pin-code':'PINCODE'},inplace=True)
# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT" 

# List all files in the folder
files = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
# Find the latest dated file

# latest_file = max(files, key=os.path.getctime)


# Initialize a list to store the 'TOWNVILLAGECODE' and 'Pin-code' from recruitment files
recruitment_data = []
recruitment_files= files[-2:]
# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file, index_col=False)
    
    # Extract 'TOWNVILLAGECODE' and 'Pin-code' columns
    recruitment_data.append(recruitment_df[['TOWNVILLAGECODE', 'PINCODE', 'RURALURBAN']])

# Concatenate data from all recruitment files
recruitment_data_combined = pd.concat(recruitment_data, ignore_index=True)

# Merge 'urban_check' with 'recruitment_data_combined' on 'Pin-code'
urban_check = pd.merge(urban_check, recruitment_data_combined, on='PINCODE', how='left')

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
rural_ids_set=list(set(rural_ids))        
# Print the list of survey IDs where the combination of TOWNVILLAGECODE and "rural/urban" is "rural"
print("Survey IDs where the combination of TOWNVILLAGECODE and 'rural/urban' is 'rural':", rural_ids_set)
