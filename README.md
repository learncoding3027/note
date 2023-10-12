def convert_to_int_or_99(value):
    if value is None or value == 'None':
        return 99
    try:
        return int(value)
    except (ValueError, TypeError):
        return 99

nccs_checks['T4M_8_1'] = nccs_checks['T4M_8'].apply(convert_to_int_or_99)






this code ran for few files but after that i got value error

import pandas as pd
import os
import glob

def map_to_code(row):
    education = row['T4M_8_1']
    count_durable = row['T5D12']

    key = (education, count_durable)

    if key in code_mapping:
        return code_mapping[key]
    else:
        return None  # Handle cases with no mapping
# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT"

# List all files in the folder
status = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
mem = glob.glob(os.path.join(folder_path, 'HHMember_Status_*'))
tv = glob.glob(os.path.join(folder_path, 'HHTV_Details_Status_*'))

status_files = status[-21:]
mem_files = mem[-21:]
tv_files = tv[-21:]

# Iterate through the files
for status_file, mem_file, tv_file in zip(status_files, mem_files, tv_files):
    # Get the date part from the status_file name
    status_date = os.path.basename(status_file).split("_")[-1]
    
    # Read the CSV files into DataFrames
    status_df = pd.read_csv(status_file, dtype=str, encoding='latin', index_col=False)
    mem_df = pd.read_csv(mem_file, dtype=str, encoding='latin', index_col=False)
    tv_df = pd.read_csv(tv_file, dtype=str, encoding='latin', index_col=False)
    
    # Drop duplicates of DQAID in copied DataFrames
    status_df.drop_duplicates(subset=['DQAID'], inplace=True)
    status_df['T5D12']=status_df['T5D12'].astype(float).astype(int)
    
    # Filter mem_df for T4M_7 = 1
    mem_df_filtered = mem_df[mem_df['T4M_6'].str.contains('3')]
    
    # Select desired columns for mem_dataframe
    mem_dataframe = mem_df_filtered[['DQAID', 'T4M_8','T4M_6', 'T4M_7']]
    
    # Merge the DataFrames on 'DQAID'
    nccs_checks = status_df.merge(mem_dataframe, on='DQAID', how='inner')
    nccs_checks['T4M_8_1'] = nccs_checks.T4M_8.fillna(99).astype(int)
    # Select specific columns from status_df
    nccs_checks = nccs_checks[['DQAID','T4M_8', 'T5D12','T4M_6','T4M_8_1' ,'T5D13']]

    # Apply the mapping function to create the "mapped code" column
    nccs_checks['mapped code'] = nccs_checks.apply(map_to_code, axis=1)

    # Find IDs where the "code" column and "mapped code" column do not match
    nccs_check = list(nccs_checks[nccs_checks['T5D13'] != nccs_checks['mapped code']]['DQAID'])

    if len(nccs_check) > 0:
        print(f" Count of Dqaid with mismatched 'NCCS' and 'mapped code for file date {status_date}':", len(nccs_check))
        print("-"*100)
    else:
        print("All Dqaid have matching 'NCCS' and 'mapped code'.")



ValueError                                Traceback (most recent call last)
<ipython-input-106-f31bbda1563c> in <module>
     47     # Merge the DataFrames on 'DQAID'
     48     nccs_checks = status_df.merge(mem_dataframe, on='DQAID', how='inner')
---> 49     nccs_checks['T4M_8_1'] = nccs_checks.T4M_8.fillna(99).astype(int)
     50     # Select specific columns from status_df
     51     nccs_checks = nccs_checks[['DQAID','T4M_8', 'T5D12','T4M_6','T4M_8_1' ,'T5D13']]

ValueError: invalid literal for int() with base 10: 'None'







# ...

# Merge the DataFrames on 'DQAID'
nccs_checks = status_df.merge(mem_dataframe, on='DQAID', how='inner')
nccs_checks['T4M_8_1'] = nccs_checks['T4M_8'].fillna(99).astype(int)  # Fill missing values with 99

# Select specific columns from status_df
nccs_checks = nccs_checks[['DQAID','T4M_8', 'T5D12','T4M_6','T4M_8_1' ,'T5D13']]

# Apply the mapping function to create the "mapped code" column
nccs_checks['mapped code'] = nccs_checks.apply(map_to_code, axis=1)

# Find IDs where the "code" column and "mapped code" column do not match
nccs_check = list(nccs_checks[nccs_checks['T5D13'] != nccs_checks['mapped code']]['DQAID'])

if len(nccs_check) > 0:
    print(f" Count of Dqaid with mismatched 'NCCS' and 'mapped code for file date {status_date}':", len(nccs_check))
    print("-"*100)
else:
    print("All Dqaid have matching 'NCCS' and 'mapped code'.")
    
