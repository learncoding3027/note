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
    
    # Merge the survey and recruitment DataFrames on "Pin-code"
    merged_df = survey_df.merge(recruitment_df, on="Pin-code", how="inner")
    
    # Filter rows where "rural/urban" is "rural"
    rural_rows = merged_df[merged_df["rural/urban"] == "rural"]
    
    # Add unique IDs from rural rows to the rural_ids list
    rural_ids.extend(rural_rows["Unique ID"].unique())

# Print or return the list of survey IDs where the combination of pincode and "rural/urban" is "rural"
print("Survey IDs where the combination of pincode and 'rural/urban' is 'rural':", rural_ids)
