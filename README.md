import pandas as pd

# Read the survey file into a DataFrame
survey_df = pd.read_csv("survey_file.csv")  # Replace with your survey file name

# List of 10 recruitment file names
recruitment_files = ["recruitment_file1.csv", "recruitment_file2.csv", "recruitment_file3.csv", "recruitment_file4.csv", "recruitment_file5.csv",
             "recruitment_file6.csv", "recruitment_file7.csv", "recruitment_file8.csv", "recruitment_file9.csv", "recruitment_file10.csv"]

# Initialize a list to store survey IDs where the combination of pincode and "rural/urban" is "rural"
rural_ids = []

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file)
    
    # Iterate through the rows of the survey DataFrame
    for index, survey_row in survey_df.iterrows():
        survey_id = survey_row["Unique ID"]
        survey_pincode = survey_row["Pin-code"]
        
        # Iterate through the rows of the recruitment DataFrame
        for _, recruitment_row in recruitment_df.iterrows():
            recruitment_pincode = recruitment_row["Pin-code"]
            rural_urban = recruitment_row["rural/urban"]
            
            # Check if the survey pincode matches the recruitment pincode and "rural/urban" is "rural"
            if survey_pincode == recruitment_pincode and rural_urban == "rural":
                rural_ids.append(survey_id)
                break  # Break out of the inner loop if a match is found for the current survey ID

# Print or return the list of survey IDs where the combination of pincode and "rural/urban" is "rural"
print("Survey IDs where the combination of pincode and 'rural/urban' is 'rural':", rural_ids)
