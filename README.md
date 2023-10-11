import pandas as pd
import os
import glob

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# List all files in the folder
status = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
mem = glob.glob(os.path.join(folder_path, 'HHMember_Status_*'))
tv = glob.glob(os.path.join(folder_path, 'HHTV_Details_Status_*'))

# Sort files by creation time
status_files = sorted(status, key=os.path.getctime)
mem_files = sorted(mem, key=os.path.getctime)
tv_files = sorted(tv, key=os.path.getctime)

# Iterate through the files
for status_file, mem_file, tv_file in zip(status_files, mem_files, tv_files):
    # Get the date part from the status_file name
    status_date = os.path.basename(status_file).split("_")[-1]
    
    # Read the CSV files into DataFrames
    status_df = pd.read_csv(status_file, dtype=str, encoding='latin')
    mem_df = pd.read_csv(mem_file, dtype=str, encoding='latin')
    tv_df = pd.read_csv(tv_file, dtype=str, encoding='latin')
    
    # Drop duplicates of DQAID in copied DataFrames
    status_df.drop_duplicates(subset=['DQAID'], inplace=True)
    mem_df.drop_duplicates(subset=['DQAID'], inplace=True)
    tv_df.drop_duplicates(subset=['DQAID'], inplace=True)
    
    if status_df['DQAID'].value_counts().equals(mem_df['DQAID'].value_counts()) and status_df['DQAID'].value_counts().equals(tv_df['DQAID'].value_counts()):
        print(f'All counts match in {status_date} (Shape: {status_df.shape[0]})')
    else:
        print(f'Counts do not match in {status_date} (Shape: {status_df.shape[0]})')
        






import pandas as pd
import os
import glob

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# List all files in the folder
status = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
mem = glob.glob(os.path.join(folder_path, 'HHMember_Status_*'))
tv = glob.glob(os.path.join(folder_path, 'HHTV_Details_Status_*'))

status_files = status[-2:]
mem_files = mem[-2:]
tv_files = tv[-2:]

# Iterate through the files
for status_file, mem_file, tv_file in zip(status_files, mem_files, tv_files):
    # Read the CSV files into DataFrames
    status_df = pd.read_csv(status_file, dtype=str, encoding='latin')
    mem_df = pd.read_csv(mem_file, dtype=str, encoding='latin')
    tv_df = pd.read_csv(tv_file, dtype=str, encoding='latin')
    
    # Drop duplicates of DQAID in copied DataFrames
#     status_df.drop_duplicates(subset=['DQAID'], inplace=True)
    mem_df.drop_duplicates(subset=['DQAID'], inplace=True)
    tv_df.drop_duplicates(subset=['DQAID'], inplace=True)
    
    if status_df['DQAID'].shape==mem_df['DQAID'].shape==tv_df['DQAID'].shape:
        print(f'All counts match in {status_file}, {mem_file}, and {tv_file}.')
        print("-"*50)
    else:v
        print(f'Counts do not match in {status_file}, {mem_file}, and {tv_file}.')
        print("-"*50)





import pandas as pd
import os
import glob

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# List all files in the folder
status = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
mem = glob.glob(os.path.join(folder_path, 'HHMember_Status_*'))
tv = glob.glob(os.path.join(folder_path, 'HHTV_Details_Status_*'))

status_files = status[-2:]
mem_files = mem[-2:]
tv_files = tv[-2:]

# Iterate through the files
for status_file, mem_file, tv_file in zip(status_files, mem_files, tv_files):
    # Read the CSV files into DataFrames
    status_df = pd.read_csv(status_file, dtype=str, encoding='latin')
    mem_df = pd.read_csv(mem_file, dtype=str, encoding='latin')
    tv_df = pd.read_csv(tv_file, dtype=str, encoding='latin')
    
    # Drop duplicates of DQAID in copied DataFrames
    status_df.drop_duplicates(subset=['DQAID'], inplace=True)
    mem_df.drop_duplicates(subset=['DQAID'], inplace=True)
    tv_df.drop_duplicates(subset=['DQAID'], inplace=True)
    
    if status_df['DQAID'].value_counts().equals(mem_df['DQAID'].value_counts()) and status_df['DQAID'].value_counts().equals(tv_df['DQAID'].value_counts()):
        print(f'All counts match in {status_file}, {mem_file}, and {tv_file}.')
    else:
        print(f'Counts do not match in {status_file}, {mem_file}, and {tv_file}.')
        



import pandas as pd
import os
import glob

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# List all files in the folder
status = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
mem = glob.glob(os.path.join(folder_path, 'HHMember_Status_*'))
tv= glob.glob(os.path.join(folder_path, 'HHTV_Details_Status_*'))

status_files=status[-2:]
mem_files=mem[-2:]
tv_files=tv[-2:]

# Initialize DataFrames to store the copies
hh_status_copy = pd.DataFrame()
hh_member_copy = pd.DataFrame()
hh_tv_copy = pd.DataFrame()

# Iterate through the files
for status_file, mem_file, tv_file in zip(status_files, mem_files, tv_files):
    # Read the CSV files into DataFrames
    status_df = pd.read_csv(status_file, dtype=str, encoding='latin')
    mem_df = pd.read_csv(mem_file, dtype=str , encoding='latin')
    tv_df = pd.read_csv(tv_file, dtype=str , encoding='latin')
    
    # Drop duplicates of DQAID in copied DataFrames
    status_df.drop_duplicates(subset=['DQAID'], inplace=True)
    mem_df.drop_duplicates(subset=['DQAID'], inplace=True)
    tv_df.drop_duplicates(subset=['DQAID'], inplace=True)
    
    if status_df['DQAID'].value_counts()==mem_df['DQAID'].value_counts()==tv_df['DQAID'].value_counts():
        print('all count')
    






i want also load HH status*, HH member* ,HHmemeber_ * but take only eight numeric digit and not other files with more than that after under score after that make copy of member and tv files and from that copy drop duplicates of column DQAID and then match the count of all DQAID in three files one of original HH staus file and other from that copies of two file and each days 
load files in same sequence together example status_files ,mem_files, tv_files it will create list of files so it will have to do that above task with first file in list 1 and 1 st file in  list 2 and 1 st file in list 3 and make a copy of filesn in list 2 and 3 and drop duplicates of column DQAID and then match the count of all DQAID in three files one of original HH staus file and print the file name where there are no same found


import pandas as pd
import numpy as np
import os
import glob

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# List all files in the folder
status = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
mem = glob.glob(os.path.join(folder_path, 'HHMember_Status_*'))
tv = glob.glob(os.path.join(folder_path, 'HHTV_Details_Status_*'))
status_files = status[-5:]
mem_files = mem[-5:]
tv_files = tv[-5:]




import pandas as pd
import os
import glob

# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# List all files in the folder
status_files = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
mem_files = glob.glob(os.path.join(folder_path, 'HH_Member_*'))
tv_files = glob.glob(os.path.join(folder_path, 'HH_TV_*'))

# Sort the files by date in ascending order
status_files.sort()
mem_files.sort()
tv_files.sort()

# Initialize DataFrames to store the copies
hh_status_copy = pd.DataFrame()
hh_member_copy = pd.DataFrame()
hh_tv_copy = pd.DataFrame()

# Iterate through the files
for status_file, mem_file, tv_file in zip(status_files, mem_files, tv_files):
    # Read the CSV files into DataFrames
    status_df = pd.read_csv(status_file, dtype=str)
    mem_df = pd.read_csv(mem_file, dtype=str)
    tv_df = pd.read_csv(tv_file, dtype=str)

    # Concatenate the DataFrames to the respective copies
    hh_status_copy = pd.concat([hh_status_copy, status_df], ignore_index=True)
    hh_member_copy = pd.concat([hh_member_copy, mem_df], ignore_index=True)
    hh_tv_copy = pd.concat([hh_tv_copy, tv_df], ignore_index=True)

# Drop duplicates of DQAID in copied DataFrames
hh_status_copy.drop_duplicates(subset=['DQAID'], inplace=True)
hh_member_copy.drop_duplicates(subset=['DQAID'], inplace=True)
hh_tv_copy.drop_duplicates(subset=['DQAID'], inplace=True)

# Count DQAIDs in original and copied DataFrames
original_hh_status = pd.read_csv(status_files[-1], dtype=str)

count_original = original_hh_status['DQAID'].value_counts()
count_copy_hh_member = hh_member_copy['DQAID'].value_counts()
count_copy_hh_tv = hh_tv_copy['DQAID'].value_counts()

# Compare the counts
print("Count of DQAID in original HH Status file:")
print(count_original)
print("\nCount of DQAID in copied HH Member files:")
print(count_copy_hh_member)
print("\nCount of DQAID in copied HH TV files:")
print(count_copy_hh_tv)

# Find file names where counts do not match
mismatched_files = []

if not count_original.equals(count_copy_hh_member):
    mismatched_files.append("HH Member")

if not count_original.equals(count_copy_hh_tv):
    mismatched_files.append("HH TV")

if not count_copy_hh_member.equals(count_copy_hh_tv):
    mismatched_files.append("HH Member and HH TV")

if not mismatched_files:
    print("All counts match.")
else:
    print("Counts do not match in the following files:", mismatched_files)



UnicodeDecodeError: 'utf-8' codec can't decode byte 0xdf in position 49694: invalid continuation byte
