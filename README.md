import pandas as pd

# Read the survey file into a DataFrame
pin_check = pd.read_csv("survey_file.csv")  # Replace with your survey file name

# Initialize a list to store IDs from the survey data that do not have a match in any recruitment data
unmatched_ids = []

# List of 10 recruitment file names
recruitment_files = ["recruitment_file1.csv", "recruitment_file2.csv", "recruitment_file3.csv", "recruitment_file4.csv", "recruitment_file5.csv",
             "recruitment_file6.csv", "recruitment_file7.csv", "recruitment_file8.csv", "recruitment_file9.csv", "recruitment_file10.csv"]

# Create a set of unique (pincode, state, district) tuples from the pincode DataFrame
pin_set = set(zip(pincode["Pin-code"], pincode["State"], pincode["District"]))

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file)
    
    # Create a set of unique (pincode, state, district) tuples from the recruitment DataFrame
    recruitment_set = set(zip(recruitment_df["Pin-code"], recruitment_df["State"], recruitment_df["District"]))
    
    # Iterate through the rows of the survey DataFrame (pin_check)
    for index, row in pin_check.iterrows():
        row_data = (row["Pin-code"], row["State"], row["District"])
        
        # Check if the (pincode, state, district) tuple from the survey data is not in the recruitment set
        if row_data not in recruitment_set and row_data not in pin_set:
            unmatched_ids.append(row["Unique ID"])

# Print or return the list of unmatched IDs from the survey data
print("IDs from survey data without a match in any recruitment data:", unmatched_ids)










import pandas as pd

# Read the survey file into a DataFrame
pin_check = pd.read_csv("survey_file.csv")  # Replace with your survey file name

# Initialize a list to store IDs from the survey data that have a match in either the pincode data or any recruitment data
approved_ids = []

# List of 10 recruitment file names
recruitment_files = ["recruitment_file1.csv", "recruitment_file2.csv", "recruitment_file3.csv", "recruitment_file4.csv", "recruitment_file5.csv",
             "recruitment_file6.csv", "recruitment_file7.csv", "recruitment_file8.csv", "recruitment_file9.csv", "recruitment_file10.csv"]

# Create a set of unique (pincode, state, district) tuples from the pincode DataFrame
pin_set = set(zip(pincode["Pin-code"], pincode["State"], pincode["District"]))

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file)
    
    # Create a set of unique (pincode, state, district) tuples from the recruitment DataFrame
    recruitment_set = set(zip(recruitment_df["Pin-code"], recruitment_df["State"], recruitment_df["District"]))
    
    # Iterate through the rows of the survey DataFrame (pin_check)
    for index, row in pin_check.iterrows():
        row_data = (row["Pin-code"], row["State"], row["District"])
        
        # Check if the (pincode, state, district) tuple from the survey data is in either the recruitment set or the pincode set
        if row_data in recruitment_set or row_data in pin_set:
            approved_ids.append(row["Unique ID"])

# Print or return the list of approved IDs from the survey data
print("Approved IDs from survey data:", approved_ids)
