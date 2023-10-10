pin_check=df2[['Unique ID','State','District', 'Pin-code']]   #survey
pincode=pincode[['State','District','Pin-code']]


# Create a set of unique (pincode, state, district) tuples from the CSV DataFrame
pin_set = set(zip(pincode["Pin-code"], pincode["State"], pincode["District"]))

# Initialize a list to store IDs from the survey data that do not have a match in the CSV data
unmatched_ids = []

# Iterate through the rows of the survey DataFrame
for index, row in pin_check.iterrows():
    row_data = (row["Pin-code"], row["State"], row["District"])
    
    # Check if the (pincode, state, district) tuple from the survey data is not in the CSV set
    if row_data not in pin_set:
        unmatched_ids.append(row["Unique ID"])

# Print or return the list of unmatched IDs from the survey data
print("IDs from survey data without a match in CSV data:", unmatched_ids)
