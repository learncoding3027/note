
rural_pincodes = set()

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file)
    
    # Filter recruitment data to get pincodes with "rural" in "rural/urban" column
    rural_pincodes.update(recruitment_df[recruitment_df["RURALURBAN"] == "Rural"]["PINCODE"])
    print(rural_pincodes)



import pandas as pd

# Read the survey file into a DataFrame
survey_df = pd.read_csv("survey_file.csv")  # Replace with your survey file name

# List of 10 recruitment file names
recruitment_files = ["recruitment_file1.csv", "recruitment_file2.csv", "recruitment_file3.csv", "recruitment_file4.csv", "recruitment_file5.csv",
             "recruitment_file6.csv", "recruitment_file7.csv", "recruitment_file8.csv", "recruitment_file9.csv", "recruitment_file10.csv"]

# Create a set to store unique "rural" pincodes from recruitment files
rural_pincodes = set()

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file)
    
    # Filter recruitment data to get pincodes with "rural" in "rural/urban" column
    rural_pincodes.update(recruitment_df[recruitment_df["rural/urban"] == "rural"]["Pin-code"])

# Initialize a list to store survey IDs where the combination of pincode and "rural/urban" is "rural"
rural_ids = []

# Iterate through the rows of the survey DataFrame
for index, survey_row in survey_df.iterrows():
    survey_id = survey_row["Unique ID"]
    survey_pincode = survey_row["Pin-code"]
    
    # Check if the survey pincode is in the set of "rural" pincodes
    if survey_pincode in rural_pincodes:
        rural_ids.append(survey_id)

# Print or return the list of survey IDs where the combination of pincode and "rural/urban" is "rural"
print("Survey IDs where the combination of pincode and 'rural/urban' is 'rural':", rural_ids)
