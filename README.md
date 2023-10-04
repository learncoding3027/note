
import os
import glob

folder_path = "/path/to/your/folder"  # Replace with the actual folder path

# List all files in the folder
files = glob.glob(os.path.join(folder_path, 'HH_Status_*'))

# Extract file names without the date and find the latest dated file
file_names = [os.path.splitext(os.path.basename(file))[0] for file in files]
latest_file = max(files, key=os.path.getctime)

# Extract the date from the latest file name
date_from_latest_file = os.path.splitext(os.path.basename(latest_file))[0].split('_')[2]

# Construct the output file name with the date
output_file_name = f"file_{date_from_latest_file}.csv"

print("Extracted file names without date:", file_names)
print("Latest dated file:", latest_file)
print("Output file name with date:", output_file_name)









import os
import glob

folder_path = "/path/to/your/folder"  # Replace with the actual folder path

# List all files in the folder
files = glob.glob(os.path.join(folder_path, 'HH_Status_*'))

# Extract file names without the date
file_names = [os.path.splitext(os.path.basename(file))[0] for file in files]

# Find the latest dated file
latest_file = max(files, key=os.path.getctime)

print("Extracted file names without date:", file_names)
print("Latest dated file:", latest_file)








, "2023-10-01", "2023-10-02", "2023-10-02"],
    "time": ["08:00:00", "08:02:00", "10:00:00", "10:03:00"],
}

interview_df = pd.DataFrame(data)

# Function to calculate the time difference between two interview times in minutes
def calculate_time_difference_in_minutes(interview1, interview2):
    time_format = "%H:%M:%S"
    time1 = datetime.strptime(interview1, time_format)
    time2 = datetime.strptime(interview2, time_format)
    time_difference = abs((time1 - time2).total_seconds()) / 60  # Convert to minutes
    return time_difference

# Set the time difference threshold in minutes (5 minutes)
threshold_minutes = 5

# Initialize a list to store IDs of interviews with time differences less than the threshold
interview_ids_with_threshold = []

# Sort the DataFrame based on date and time
interview_df = interview_df.sort_values(by=["date", "time"])

# Calculate time differences for interviews on the same date and time
for index1, index2 in combinations(range(len(interview_df)), 2):
    row1 = interview_df.iloc[index1]
    row2 = interview_df.iloc[index2]






    if row1["date"] == row2["date"] and row1["time"] == row2["time"]:
        time_difference = calculate_time_difference_in_minutes(row1["time"], row2["time"])
        if time_difference < threshold_minutes:
            interview_ids_with_threshold.extend([row1["id"], row2["id"]])

# Remove duplicates from the list
interview_ids_with_threshold = list(set(interview_ids_with_threshold))

# Print the IDs of interviews with time differences less than 5 minutes
print("Interview IDs with time differences less than 5 minutes:", interview_ids_with_threshold)







import pandas as pd

# Sample data in "design" and "rec" DataFrames
design_data = pd.DataFrame({
    'Town code': [101, 102, 103],
    'Parent code': [201, 202, 203]
})

rec_data = pd.DataFrame({
    'Town code': [101, 103, 102, 104, 105]
})

# Create a dictionary from the "design" DataFrame
town_to_parent_dict = design_data.set_index('Town code')['Parent code'].to_dict()

# Map the values to the "rec" DataFrame based on the "Town code" column
rec_data['Parent code'] = rec_data['Town code'].map(town_to_parent_dict)

# Print the "rec" DataFrame with the "Parent code" column added
print(rec_data)






import pandas as pd

# Sample data in "design" and "rec" DataFrames
design_data = pd.DataFrame({
    'Town code': [101, 102, 103],
    'Parent code': [201, 202, 203]
})

rec_data = pd.DataFrame({
    'Town code': [101, 103, 102, 104, 105]
})

# Merge the "rec" DataFrame with the "design" DataFrame based on the "Town code" column
merged_data = rec_data.merge(design_data, left_on='Town code', right_on='Town code', how='left')

# The "Parent code" column from the "design" DataFrame is now added to the "rec" DataFrame
print(merged_data)
