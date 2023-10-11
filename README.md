iimport pandas as pd
import os
import glob
import re

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# Initialize DataFrames to store the copies
hh_status_copy = pd.DataFrame()
hh_member_copy = pd.DataFrame()
hh_member_tv_copy = pd.DataFrame()

# Extract the date from the file name
date_regex = r'(HH_Status|HH_Member|HH_TV)_(\d{8})'

# Iterate through files
for file_path in glob.glob(os.path.join(folder_path, 'HH_*_*')):

    # Extract the date and file type from the file name
    match = re.search(date_regex, file_path)
    if match:
        file_type = match.group(1)
        file_date = match.group(2)
    else:
        continue  # Skip if date not found in file name

    # Read the CSV file into a DataFrame
    df = pd.read_csv(file_path, dtype=str)

    # Concatenate the extracted data to the respective copies
    if file_type == 'HH_Status':
        hh_status_copy = pd.concat([hh_status_copy, df], ignore_index=True)
    elif file_type == 'HH_Member':
        hh_member_copy = pd.concat([hh_member_copy, df], ignore_index=True)
    elif file_type == 'HH_TV':
        hh_member_tv_copy = pd.concat([hh_member_tv_copy, df], ignore_index=True)

# Remove duplicates from the copied DataFrames
hh_status_copy.drop_duplicates(subset=['DQAID'], inplace=True)
hh_member_copy.drop duplicates(subset=['DQAID'], inplace=True)
hh_member_tv_copy.drop_duplicates(subset=['DQAID'], inplace=True)

# Count DQAID occurrences in the original HH_Status file
original_hh_status = pd.read_csv(os.path.join(folder_path, 'HH_Status_*'), dtype=str)

count_original = original_hh_status['DQAID'].value_counts()
count_copy_hh_member = hh_member_copy['DQAID'].value_counts()
count_copy_hh_member_tv = hh_member_tv_copy['DQAID'].value_counts()

# Print the count of DQAID occurrences
print("Count of DQAID in original HH_Status file:")
print(count_original)
print("\nCount of DQAID in copied HH_Member files:")
print(count_copy_hh_member)
print("\nCount of DQAID in copied HH_Member_TV files:")
print(count_copy_hh_member_tv)
want also load HH status*, HH member* ,HHmemeber_ * but take only eight numeric digit and not other files with more than that after under score after that make copy of member and tv files and from that copy drop duplicates of column DQAID and then match the count of all DQAID in three files one of original HH staus file and other from that copies of two file and each days 


import pandas as pd
import numpy as np
import os
import glob

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# List all files in the folder
files = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
recruitment_files = files[-5:]

# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file, index_col=False)






import pandas as pd
import os
import glob
import re

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# Initialize DataFrames to store the copies
hh_status_copy = pd.DataFrame()
hh_member_copy = pd.DataFrame()
hh_member_tv_copy = pd.DataFrame()

# Extract the date from the file name
date_regex = r'(HH_Status|HH_Member|HH_TV)_(\d{8})'

# Iterate through files
for file_path in glob.glob(os.path.join(folder_path, 'HH_*_*')):

    # Extract the date and file type from the file name
    match = re.search(date_regex, file_path)
    if match:
        file_type = match.group(1)
        file_date = match.group(2)
    else:
        continue  # Skip if date not found in file name

    # Read the CSV file into a DataFrame
    df = pd.read_csv(file_path, dtype=str)

    # Concatenate the extracted data to the respective copies
    if file_type == 'HH_Status':
        hh_status_copy = pd.concat([hh_status_copy, df], ignore_index=True)
    elif file_type == 'HH_Member':
        hh_member_copy = pd.concat([hh_member_copy, df], ignore_index=True)
    elif file_type == 'HH_TV':
        hh_member_tv_copy = pd.concat([hh_member_tv_copy, df], ignore_index=True)

# Remove duplicates from the copied DataFrames
hh_status_copy.drop_duplicates(subset=['DQAID'], inplace=True)
hh_member_copy.drop duplicates(subset=['DQAID'], inplace=True)
hh_member_tv_copy.drop_duplicates(subset=['DQAID'], inplace=True)

# Count DQAID occurrences in the original HH_Status file
original_hh_status = pd.read_csv(os.path.join(folder_path, 'HH_Status_*'), dtype=str)

count_original = original_hh_status['DQAID'].value_counts()
count_copy_hh_member = hh_member_copy['DQAID'].value_counts()
count_copy_hh_member_tv = hh_member_tv_copy['DQAID'].value_counts()

# Print the count of DQAID occurrences
print("Count of DQAID in original HH_Status file:")
print(count_original)
print("\nCount of DQAID in copied HH_Member files:")
print(count_copy_hh_member)
print("\nCount of DQAID in copied HH_Member_TV files:")
print(count_copy_hh_member_tv)

