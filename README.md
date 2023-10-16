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
