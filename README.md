import pandas as pd

# Read the survey file into a DataFrame
survey_df = pd.read_csv("survey_file.csv")  # Replace with your survey file name

# Read your CSV file into a DataFrame
your_csv_df = pd.read_csv("your_csv_file.csv")  # Replace with your CSV file name

# Create a set of unique (pincode, state, district) tuples from the CSV DataFrame
csv_set = set(zip(your_csv_df["pincode"], your_csv_df["state"], your_csv_df["district"]))

# Initialize a list to store IDs from the survey data that do not have a match in the CSV data
unmatched_ids = []

# Iterate through the rows of the survey DataFrame
for index, row in survey_df.iterrows():
    row_data = (row["pincode"], row["state"], row["district"])
    
    # Check if the (pincode, state, district) tuple from the survey data is not in the CSV set
    if row_data not in csv_set:
        unmatched_ids.append(row["id"])

# Print or return the list of unmatched IDs from the survey data
print("IDs from survey data without a match in CSV data:", unmatched_ids)
