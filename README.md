df_tv=df2[['Unique ID','TV Owning','Black & White TV','Color TV/LCD/LED/PLASMA TV']]
No_TV = []

# Iterate through the DataFrame rows
for index, row in df_tv.iterrows():
    tv_status = 'Non-TV Owning HH' if row['Black & White TV'] == 'No' and row['Color TV/LCD/LED/PLASMA TV'] == 'No' else 'TV Owning HH'
    
    # Check if the calculated TV status does not match any of the existing TV columns
    if tv_status != row['TV Owning']:
        No_TV.append(row['Unique ID'])

# Print the list of IDs with mismatched TV status
print("IDs with mismatched TV status:", No_TV)





df_tv = df2[['Unique ID', 'TV Owning', 'Black & White TV', 'Color TV/LCD/LED/PLASMA TV']]

# Function to check TV ownership status
def check_tv_status(row):
    if row['Black & White TV'] == 'No' and row['Color TV/LCD/LED/PLASMA TV'] == 'No':
        return 'Non-TV Owning HH'
    else:
        return 'TV Owning HH'

# Apply the function to each row
df_tv['Calculated TV Status'] = df_tv.apply(check_tv_status, axis=1)

# Filter the IDs where the calculated TV status does not match the 'TV Owning' column
mismatched_ids = df_tv[df_tv['Calculated TV Status'] != df_tv['TV Owning']]['Unique ID'].tolist()

# Print the list of IDs with mismatched TV status
print("IDs with mismatched TV status:", mismatched_ids)
