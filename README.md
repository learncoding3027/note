import pandas as pd
from datetime import datetime
from itertools import combinations

# Sample interview data in a pandas DataFrame
data = {
    "id": [1, 2, 3, 4],
    "date": ["2023-10-01", "2023-10-01", "2023-10-02", "2023-10-02"],
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
