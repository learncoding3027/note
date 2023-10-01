# note

hello

from datetime import datetime

# Sample interview data with IDs and timings
interviews = [
    {"id": 1, "time": "2023-10-01 08:00:00"},
    {"id": 2, "time": "2023-10-01 15:30:00"},
    {"id": 3, "time": "2023-10-01 21:45:00"},
    {"id": 4, "time": "2023-10-01 06:30:00"},
]

# Define the time range
start_time = datetime.strptime("07:00:00", "%H:%M:%S")
end_time = datetime.strptime("20:00:00", "%H:%M:%S")

# Initialize a list to store IDs of interviews outside the range
outside_range_ids = []

# Check interview timings
for interview in interviews:
    interview_time = datetime.strptime(interview["time"], "%Y-%m-%d %H:%M:%S")
    if interview_time < start_time or interview_time > end_time:
        outside_range_ids.append(interview["id"])

# Print the list of interview IDs outside the specified range
print("Interview IDs outside the range (7:00:00 AM - 8:00:00 PM):", outside_range_ids)



######################################################3


from datetime import datetime
from itertools import combinations

# Sample interview data with interviewer name, date, and time
interviews = [
    {"name": "Interviewer1", "date": "2023-10-01", "time": "08:00:00"},
    {"name": "Interviewer2", "date": "2023-10-01", "time": "09:30:00"},
    {"name": "Interviewer1", "date": "2023-10-02", "time": "10:00:00"},
    {"name": "Interviewer2", "date": "2023-10-02", "time": "11:30:00"},
    # Add more interviews here
]

# Function to calculate the time difference between two interview times
def calculate_time_difference(interview1, interview2):
    time_format = "%H:%M:%S"
    time1 = datetime.strptime(interview1["time"], time_format)
    time2 = datetime.strptime(interview2["time"], time_format)
    return abs((time1 - time2).total_seconds())

# Sort the interviews based on date and time
sorted_interviews = sorted(interviews, key=lambda x: (x["date"], x["time"]))

# Initialize a dictionary to store time differences by interviewer
time_differences = {}

# Calculate time differences for interviews on the same date and time
for interview1, interview2 in combinations(sorted_interviews, 2):
    if interview1["date"] == interview2["date"] and interview1["time"] == interview2["time"]:
        key = f"{interview1['name']} on {interview1['date']} at {interview1['time']}"
        time_difference = calculate_time_difference(interview1, interview2)
        time_differences[key] = time_difference

# Print the time differences
for key, difference in time_differences.items():
    print(f"{key}: {difference} seconds")
