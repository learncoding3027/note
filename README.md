
import pandas as pd

# Sample DataFrame with ID, interviewer ID, name, latitude, longitude, and date columns
data = {'ID': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
        'InterviewerID': [101, 102, 101, 103, 102, 101, 103, 104, 105, 101],
        'Name': ['Alice', 'Bob', 'Alice', 'Alice', 'Bob', 'Alice', 'Alice', 'Bob', 'Alice', 'Bob'],
        'Latitude': [40.7128, 40.7128, 34.0522, 34.0522, 34.0522, 40.7128, 34.0522, 34.0522, 40.7128, 34.0522],
        'Longitude': [-74.0060, -74.0060, -118.2437, -118.2437, -118.2437, -74.0060, -118.2437, -118.2437, -74.0060, -118.2437],
        'Date': ['2023-10-03', '2023-10-03', '2023-10-03', '2023-10-04', '2023-10-04', '2023-10-05', '2023-10-05', '2023-10-06', '2023-10-06', '2023-10-07']}

df = pd.DataFrame(data)

# Convert 'Date' column to datetime format
df['Date'] = pd.to_datetime(df['Date'])

# Create a pivot table to count interviews based on GPS latitude and longitude
pivot_table = df.pivot_table(index=['Latitude', 'Longitude'], columns='Date', values='ID', aggfunc='count', fill_value=0)

# Filter the pivot table to get counts more than 20
filtered_pivot = pivot_table[pivot_table > 20].dropna()

# Display the filtered pivot table
print("Pivot table with counts more than 20:")
print(filtered_pivot)

# Extract the latitude and longitude indices from the filtered pivot table
filtered_indices = filtered_pivot.index.tolist()

# Filter the original DataFrame based on the filtered indices
filtered_data = df[df.apply(lambda x: (x['Latitude'], x['Longitude']) in filtered_indices, axis=1)]

# Display the filtered data with ID, interviewer ID, name, and count of interviews on a single date
print("\nFiltered Data:")
print(filtered_data[['ID', 'InterviewerID', 'Name', 'Date']])










import pandas as pd

# Sample DataFrame with ID, interviewer ID, name, and date columns
data = {'ID': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
        'InterviewerID': [101, 102, 101, 103, 102, 101, 103, 104, 105, 101],
        'Name': ['Alice', 'Bob', 'Alice', 'Alice', 'Bob', 'Alice', 'Alice', 'Bob', 'Alice', 'Bob'],
        'Date': ['2023-10-03', '2023-10-03', '2023-10-03', '2023-10-04', '2023-10-04', '2023-10-05', '2023-10-05', '2023-10-06', '2023-10-06', '2023-10-07']}

df = pd.DataFrame(data)

# Convert 'Date' column to datetime format
df['Date'] = pd.to_datetime(df['Date'])

# Group by 'Name' and 'Date' and count the number of interviews
interview_counts = df.groupby(['Name', 'Date']).size().reset_index(name='InterviewCount')

# Filter the data where interview count is more than 20
filtered_data = interview_counts[interview_counts['InterviewCount'] > 20]

# Display the result
print("Interviews with more than 20 counts:")
print(filtered_data)

# If you want to get the list of IDs for those interviews
filtered_ids = df[df['Name'].isin(filtered_data['Name']) & df['Date'].isin(filtered_data['Date'])]['ID'].tolist()
print("\nList of IDs for interviews with more than 20 counts:")
print(filtered_ids)











import pandas as pd

# Sample DataFrame with ID, name, and time columns
data = {'ID': [1, 2, 3, 4, 5],
        'Name': ['Alice', 'Bob', 'Alice', 'Alice', 'Bob'],
        'Time': ['10:00:00', '10:15:00', '10:03:00', '10:45:00', '11:00:00']}

df = pd.DataFrame(data)

# Convert 'Time' column to datetime.time format
df['Time'] = pd.to_datetime(df['Time']).dt.time

# Sort the DataFrame by name and time
df.sort_values(by=['Name', 'Time'], inplace=True)

# Create a new DataFrame to store time differences
time_diffs = []
current_name = None
prev_time = None

for _, row in df.iterrows():
    if current_name is None or current_name != row['Name']:
        current_name = row['Name']
        prev_time = row['Time']
    else:
        time_diff = datetime.combine(datetime.today(), row['Time']) - datetime.combine(datetime.today(), prev_time)
        time_diffs.append(time_diff)
        prev_time = row['Time']

df['Time_diff'] = time_diffs

# Filter the DataFrame to get IDs with time differences less than 5 minutes
# Ignore the first entry for each person (where Time_diff is NaN)
result_ids = df[(df['Time_diff'] < pd.Timedelta(minutes=5)) & ~df['Time_diff'].isna()]['ID'].tolist()

print("IDs with interview time differences less than 5 minutes (ignoring the first entry for each person):", result_ids)











import pandas as pd

# Sample DataFrame with ID, name, and time columns
data = {'ID': [1, 2, 3, 4, 5],
        'Name': ['Alice', 'Bob', 'Alice', 'Alice', 'Bob'],
        'Time': ['10:00:00', '10:15:00', '10:03:00', '10:45:00', '11:00:00']}

df = pd.DataFrame(data)

# Convert 'Time' column to datetime.time format
df['Time'] = pd.to_datetime(df['Time']).dt.time

# Sort the DataFrame by name and time
df.sort_values(by=['Name', 'Time'], inplace=True)

# Create a new DataFrame to store time differences
df['Time_diff'] = df.groupby('Name')['Time'].shift(-1) - df['Time']

# Filter the DataFrame to get IDs with time differences less than 5 minutes
# Ignore the last entry for each person (where Time_diff is NaN)
result_ids = df[(df['Time_diff'] < pd.Timedelta(minutes=5)) & ~df['Time_diff'].isna()]['ID'].tolist()

print("IDs with interview time differences less than 5 minutes (ignoring the last entry for each person):", result_ids)





import pandas as pd

# Sample DataFrame with ID, name, date, and time columns
data = {'ID': [1, 2, 3, 4, 5],
        'Name': ['Alice', 'Bob', 'Alice', 'Alice', 'Bob'],
        'Date': ['2023-10-03', '2023-10-03', '2023-10-03', '2023-10-03', '2023-10-03'],
        'Time': ['10:00:00', '10:15:00', '10:03:00', '10:45:00', '11:00:00']}

df = pd.DataFrame(data)

# Convert 'Date' and 'Time' columns to datetime format
df['Time'] = pd.to_datetime(df['Time'])

# Sort the DataFrame by name, date, and time
df.sort_values(by=['Name', 'Date', 'Time'], inplace=True)

# Create a new DataFrame to store time differences
df['Time_diff'] = df.groupby('Name')['Time'].diff()

# Filter the DataFrame to get IDs with time differences less than 5 minutes
# Ignore the first entry for each person (where Time_diff is NaN)
result_ids = df[(df['Time_diff'] < pd.Timedelta(minutes=5)) & ~df['Time_diff'].isna()]['ID'].tolist()

print("IDs with interview time differences less than 5 minutes (ignoring the first entry for each person):", result_ids)





import pandas as pd

# Sample DataFrame with ID, name, date, and time columns
data = {'ID': [1, 2, 3, 4, 5],
        'Name': ['Alice', 'Bob', 'Alice', 'Alice', 'Bob'],
        'Date': ['2023-10-03', '2023-10-03', '2023-10-03', '2023-10-03', '2023-10-03'],
        'Time': ['10:00:00', '10:15:00', '10:03:00', '10:45:00', '11:00:00']}

df = pd.DataFrame(data)

# Convert 'Date' and 'Time' columns to datetime format
df['Time'] = pd.to_datetime(df['Time'])

# Sort the DataFrame by name, date, and time
df.sort_values(by=['Name', 'Date', 'Time'], inplace=True)

# Create a new DataFrame to store time differences
time_diff_df = df.groupby('Name')['Time'].diff().fillna(pd.Timedelta(seconds=0))

# Filter the DataFrame to get IDs with time differences less than 5 minutes
# Skip the first entry for each person
result_ids = df[(time_diff_df < pd.Timedelta(minutes=5)) & (time_diff_df >= pd.Timedelta(seconds=0))]['ID'].tolist()

print("IDs with interview time differences less than 5 minutes:", result_ids)








import pandas as pd

# Sample DataFrame with ID, name, date, and time columns
data = {'ID': [1, 2, 3, 4, 5],
        'Name': ['Alice', 'Bob', 'Alice', 'Alice', 'Bob'],
        'Date': ['2023-10-03', '2023-10-03', '2023-10-03', '2023-10-03', '2023-10-03'],
        'Time': ['10:00:00', '10:15:00', '10:03:00', '10:45:00', '11:00:00']}

df = pd.DataFrame(data)

# Convert 'Date' and 'Time' columns to datetime format
# df['DateTime'] = pd.to_datetime(df['Date'] + ' ' + df['Time'])
df['Time'] = pd.to_datetime(df['Time'])

# Sort the DataFrame by name, date, and time
df.sort_values(by=['Name', 'Date', 'Time'], inplace=True)

# Create a new DataFrame to store time differences
time_diff_df = df.groupby(['Name'])['Time'].diff().fillna(pd.Timedelta(seconds=0))
df['diff']=time_diff_df
# Filter the DataFrame to get IDs with time differences less than 5 minutes
result_ids = df[(time_diff_df < pd.Timedelta(minutes=5)) & (time_diff_df >= pd.Timedelta(seconds=0))]['ID'].tolist()

print("IDs with interview time differences less than 5 minutes:", result_ids)


        ID	Name	Date	                Time	             diff
	1	Alice	2023-10-03	2023-10-05 10:00:00	0 days 00:00:00
	3	Alice	2023-10-03	2023-10-05 10:03:00	0 days 00:03:00
	4	Alice	2023-10-03	2023-10-05 10:45:00	0 days 00:42:00
	2	Bob	2023-10-03	2023-10-05 10:15:00	0 days 00:00:00
	5	Bob	2023-10-03	2023-10-05 11:00:00	0 days 00:45:00









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











import pandas as pd

# Sample DataFrame with ID and Time columns in time format
data = {'ID': [1, 2, 3, 4, 5],
        'Time': ['08:30:00', '19:45:00', '21:15:00', '11:30:00', '14:00:00']}

df = pd.DataFrame(data)

# Define the time range (from 7:00:00 to 20:00:00)
start_time = pd.Timestamp('07:00:00').time()
end_time = pd.Timestamp('20:00:00').time()

# Function to check if a time falls outside the specified time range
def is_outside_time_range(time_str):
    time_val = pd.Timestamp(time_str).time()
    return not (start_time <= time_val <= end_time)

# Filter the DataFrame to get IDs with interview timings outside the range
outside_range_ids = df[df['Time'].apply(is_outside_time_range)]['ID'].tolist()

print("IDs with interview timings outside the specified range:", outside_range_ids)




1.write me python code to check interview timing is only between 7:00:00 to 20:00:00 and if any ids in present out of this time  provide me list of ids which are doing interview in odd timing.

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
