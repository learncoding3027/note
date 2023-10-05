i1.write me python code to check interview timing is only between 7:00:00 to 20:00:00 and if any ids in present out of this time  provide me list of ids which are doing interview in odd timing.

import pandas as pd

# Sample DataFrame with ID and Time columns in time format
data = {'ID': [1, 2, 3, 4, 5],
        'Time': ['08:30:00', '19:45:00', '21:15:00', '11:30:00', '14:00:00']}

df = pd.DataFrame(data)

# Define the time range (from 7:00:00 to 20:00:00)
start_time = pd.Timestamp('07:00:00').time()
end_time = pd.Timestamp('20:00:00').time()

# Function to check if a time falls within the specified time range
def is_within_time_range(time_str):
    time_val = pd.Timestamp(time_str).time()
    return start_time <= time_val <= end_time

# Filter the DataFrame to get IDs with interview timings within the range
within_range_ids = df[df['Time'].apply(is_within_time_range)]['ID'].tolist()

print("IDs with interview timings within the specified range:", within_range_ids)











from datetime import datetime

# Sample interview data with ID and timing
interview_data = [
    {"ID": 1, "Time": "10:30:00"},
    {"ID": 2, "Time": "14:45:00"},
    {"ID": 3, "Time": "06:15:00"},
    {"ID": 4, "Time": "20:30:00"},
    {"ID": 5, "Time": "22:00:00"},
]

# Function to check if a time is odd (not between 7:00:00 and 20:00:00)
def is_odd_time(time_str):
    try:
        time = datetime.strptime(time_str, "%H:%M:%S").time()
        return not (time >= datetime.strptime("07:00:00", "%H:%M:%S").time() and
                    time <= datetime.strptime("20:00:00", "%H:%M:%S").time())
    except ValueError:
        return True  # Invalid time format is considered odd

# Find IDs with odd interview timings
odd_timing_ids = [data["ID"] for data in interview_data if is_odd_time(data["Time"])]

print("IDs with odd interview timings:", odd_timing_ids)





2. Find the time difference between the interview so that specific person on that specific date sorting name , date and time together and then based on the time find the time difference of the specific person on that specific date only and if that is less than 5 min provide me the list of the of the ids.

from datetime import datetime
from collections import defaultdict

# Sample interview data with ID, name, date, and time
interview_data = [
    {"ID": 1, "Name": "Alice", "Date": "2023-10-03", "Time": "10:00:00"},
    {"ID": 2, "Name": "Bob", "Date": "2023-10-03", "Time": "10:15:00"},
    {"ID": 3, "Name": "Alice", "Date": "2023-10-03", "Time": "10:30:00"},
    {"ID": 4, "Name": "Alice", "Date": "2023-10-03", "Time": "10:45:00"},
    {"ID": 5, "Name": "Bob", "Date": "2023-10-03", "Time": "11:00:00"},
    {"ID": 6, "Name": "Alice", "Date": "2023-10-04", "Time": "09:00:00"},
    {"ID": 7, "Name": "Alice", "Date": "2023-10-04", "Time": "09:15:00"},
]

# Sort the interview data by name, date, and time
sorted_data = sorted(interview_data, key=lambda x: (x["Name"], x["Date"], x["Time"]))

# Create a dictionary to store interview times by person and date
interview_times = defaultdict(list)

for entry in sorted_data:
    key = (entry["Name"], entry["Date"])
    interview_times[key].append(datetime.strptime(entry["Time"], "%H:%M:%S"))

# Find time differences and IDs with differences less than 5 minutes
result_ids = []

for key, times in interview_times.items():
    times.sort()  # Sort interview times for each person and date
    for i in range(1, len(times)):
        time_diff = (times[i] - times[i - 1]).total_seconds() / 60  # Calculate time difference in minutes
        if time_diff < 5:
            result_ids.append(key[0])  # Add the person's name to the result

# Remove duplicates and sort the result
result_ids = sorted(set(result_ids))

print("IDs with interview time differences less than 5 minutes:", result_ids)






3. Find out count of number of interviews by that name and on that particular day and if the count is more than 20 then add ids it in list.

from datetime import datetime
from collections import defaultdict

# Sample interview data with ID, name, date, and time
interview_data = [
    {"ID": 1, "Name": "Alice", "Date": "2023-10-03", "Time": "10:00:00"},
    {"ID": 2, "Name": "Bob", "Date": "2023-10-03", "Time": "10:15:00"},
    {"ID": 3, "Name": "Alice", "Date": "2023-10-03", "Time": "10:30:00"},
    {"ID": 4, "Name": "Alice", "Date": "2023-10-03", "Time": "10:45:00"},
    {"ID": 5, "Name": "Bob", "Date": "2023-10-03", "Time": "11:00:00"},
    {"ID": 6, "Name": "Alice", "Date": "2023-10-04", "Time": "09:00:00"},
    {"ID": 7, "Name": "Alice", "Date": "2023-10-04", "Time": "09:15:00"},
]

# Sort the interview data by name, date, and time
sorted_data = sorted(interview_data, key=lambda x: (x["Name"], x["Date"], x["Time"]))

# Create a dictionary to store the count of interviews by name and date
interview_counts = defaultdict(int)

for entry in sorted_data:
    key = (entry["Name"], entry["Date"])
    interview_counts[key] += 1

# Find names with interview counts greater than 20 and add IDs to a list
result_names = []

for key, count in interview_counts.items():
    if count > 20:
        result_names.append(key[0])

print("Names with interview counts greater than 20:", result_names)





4. Find out the count of number of interview based on gps lat and long and if count is more than 20 then add ids it in list.
from collections import defaultdict

# Sample interview data with ID, latitude, and longitude
interview_data = [
    {"ID": 1, "Latitude": 34.0522, "Longitude": -118.2437},
    {"ID": 2, "Latitude": 40.7128, "Longitude": -74.0060},
    {"ID": 3, "Latitude": 34.0522, "Longitude": -118.2437},
    {"ID": 4, "Latitude": 34.0522, "Longitude": -118.2437},
    {"ID": 5, "Latitude": 40.7128, "Longitude": -74.0060},
    {"ID": 6, "Latitude": 34.0522, "Longitude": -118.2437},
    # Add more data here
]

# Create a dictionary to store the count of interviews by latitude and longitude
interview_counts = defaultdict(int)

for entry in interview_data:
    key = (entry["Latitude"], entry["Longitude"])
    interview_counts[key] += 1

# Find IDs with interview counts greater than 20 and add them to a list
result_ids = []

for key, count in interview_counts.items():
    if count > 20:
        result_ids.append(key)

print("IDs with interview counts greater than 20 (Latitude, Longitude):", result_ids)
