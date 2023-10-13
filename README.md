# Group by 'Name' and 'Date' and count the number of interviews
interview_counts = df2.groupby(['Interviewer Name', 'Survey Date']).size().reset_index(name='InterviewCount')
# Filter the data where interview count is more than 20
filtered_data = interview_counts[interview_counts['InterviewCount'] >= 20]
#Sort values by count
more_than_20=filtered_data.sort_values(by='InterviewCount',ascending=False)
more_than_20['Survey Date'] = more_than_20['Survey Date'].apply(lambda x: str(x).split()[0])

#list of IDs for those interviews
filtered_ids_20 = df2[df2['Interviewer Name'].isin(more_than_20['Interviewer Name']) & df2['Survey Date'].isin(filtered_data['Survey Date'])][['Unique ID','Interviewer ID','Interviewer Name','Survey Date','Survey Start time']]#.tolist()
filtered_ids_20['Survey Date'] = filtered_ids_20['Survey Date'].apply(lambda x: str(x).split()[0])
print("_"*100)
print("\nList of IDs for interviews with more than 20 counts:")
print("_"*100)
print(more_than_20)
print("="*100)
print("\nList of IDs for interviews with more than 20 counts with all details :")
print("_"*100)
print('\n',filtered_ids_20)
print("_"*100)


filtered_data_20_id=(filtered_ids_20['Unique ID'].tolist())
filtered_data_20_id


















# Group by 'Name' and 'Date' and count the number of interviews
interview_counts = df2.groupby(['Interviewer Name', 'Survey Date', 'GPS Lattitude', 'GPS Longitude']).size().reset_index(name='InterviewCount')
# Filter the data where interview count is more than 20
filtered_data = interview_counts[interview_counts['InterviewCount'] >= 20]
#Sort values by count
more_than_20_gps=filtered_data.sort_values(by='InterviewCount',ascending=False)
more_than_20_gps['Survey Date'] = more_than_20_gps['Survey Date'].apply(lambda x: str(x).split()[0])
#list of IDs for those interviews
filtered_ids_20_gps = df2[df2['Interviewer Name'].isin(more_than_20['Interviewer Name']) & df2['Survey Date'].isin(filtered_data['Survey Date'])][['Unique ID','Interviewer ID','Interviewer Name','Survey Date', 'GPS Lattitude', 'GPS Longitude']]#.tolist()
filtered_ids_20_gps['Survey Date'] = filtered_ids_20_gps['Survey Date'].apply(lambda x: str(x).split()[0])
print("_"*100)
print("\nList of IDs for interviews with more than 20 counts:")
print("_"*100)
print(more_than_20_gps)
print("="*100)
print("\nList of IDs for interviews with more than 20 counts with all details :")
print("_"*100)
print('\n',filtered_ids_20_gps)
print("_"*100)


filtered_data_gps_id=(filtered_ids_20_gps['Unique ID'].tolist())
print("List of Id's :")
filtered_data_gps_id
