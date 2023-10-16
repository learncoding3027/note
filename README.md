i want to also add this column to urban check BARC Town / Village code which consider corresponding value of BARC Town / Village code from urban check along with pincode  and also use this TOWNVILLAGECODE column and pincode  from recruitment file and use this below code




urban_check = df[['Unique ID', 'Pin-code']] 

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT" 

# List all files in the folder
files = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
# Find the latest dated file

# latest_file = max(files, key=os.path.getctime)
recruitment_files= files[-20:]

# Create a set to store unique "rural" pincodes from recruitment files
rural_pincodes = set()

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file , index_col=False)
    
    # Filter recruitment data to get pincodes with "rural" in "rural/urban" column
    rural_pincodes.update(recruitment_df[recruitment_df["RURALURBAN"] == "Rural"]["PINCODE"])

# Initialize a list to store survey IDs where the combination of pincode and "rural/urban" is "rural"
rural_ids = []

# Iterate through the rows of the survey DataFrame
for index, survey_row in urban_check.iterrows():
    survey_id = survey_row["Unique ID"]
    survey_pincode = survey_row["Pin-code"]
    
    # Check if the survey pincode is in the set of "rural" pincodes
    if survey_pincode in rural_pincodes:
        rural_ids.append(survey_id)

# Print or return the list of survey IDs where the combination of pincode and "rural/urban" is "rural"
print("Survey IDs where the combination of pincode and 'rural/urban' is 'rural':", rural_ids)
