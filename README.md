import pandas as pd

# Read the survey file into a DataFrame
survey_df = pd.read_csv("survey_file.csv")  # Replace with your survey file name

# List of 10 recruitment file names
recruitment_files = ["recruitment_file1.csv", "recruitment_file2.csv", "recruitment_file3.csv", "recruitment_file4.csv", "recruitment_file5.csv",
             "recruitment_file6.csv", "recruitment_file7.csv", "recruitment_file8.csv", "recruitment_file9.csv", "recruitment_file10.csv"]

# Initialize a list to store survey IDs where "rural" is found
rural_ids = []

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file)
    
    # Iterate through the rows of the survey DataFrame
    for index, row in survey_df.iterrows():
        survey_id = row["Unique ID"]
        survey_pincode = row["Pin-code"]
        
        # Check if the survey pincode exists in the recruitment DataFrame
        if survey_pincode in recruitment_df["Pin-code"].values:
            # Find the corresponding row in the recruitment DataFrame
            recruitment_row = recruitment_df[recruitment_df["Pin-code"] == survey_pincode].iloc[0]
            rural_urban = recruitment_row["rural/urban"]
            
            # Check if "rural/urban" is "rural"
            if rural_urban == "rural":
                rural_ids.append(survey_id)

# Print or return the list of survey IDs where "rural" is found
print("Survey IDs where 'rural' is found in recruitment data:", rural_ids)
