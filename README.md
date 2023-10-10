import pandas as pd

# Read the survey file into a DataFrame
survey_df = pd.read_csv("survey_file.csv")  # Replace with your survey file name

# Read your CSV file into a DataFrame
your_csv_df = pd.read_csv("your_csv_file.csv")  # Replace with your CSV file name

# Create a set of unique (pincode, state, district) tuples from the survey DataFrame
survey_set = set(zip(survey_df["pincode"], survey_df["state"], survey_df["district"]))

# Initialize a list to store IDs with discrepancies
discrepancy_ids = []

# Iterate through the rows of your CSV DataFrame
for index, row in your_csv_df.iterrows():
    row_data = (row["pincode"], row["state"], row["district"])
    
    # Check if the (pincode, state, district) tuple from your CSV is not in the survey set
    if row_data not in survey_set:
        discrepancy_ids.append(row["id"])

# Print or return the list of IDs with discrepancies
print("IDs with discrepancies in pincode, state, and district:", discrepancy_ids)

