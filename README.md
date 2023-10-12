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
    # Get the date part from the status_file name
    status_date = os.path.basename(status_file).split("_")[-1]
    
    # Read the CSV files into DataFrames
    status_df = pd.read_csv(status_file, dtype=str, encoding='latin', index_col=False)
    mem_df = pd.read_csv(mem_file, dtype=str, encoding='latin', index_col=False)
    tv_df = pd.read_csv(tv_file, dtype=str, encoding='latin', index_col=False)
    
    # Drop duplicates of DQAID in copied DataFrames
    status_df.drop_duplicates(subset=['DQAID'], inplace=True)
    
    # Filter mem_df for T4M_7 = 1
    mem_df_filtered = mem_df[mem_df['T4M_7'] == '1']
    
    # Select desired columns for mem_dataframe
    mem_dataframe = mem_df_filtered[['DQAID', 'T4M_8', 'T4M_7']]
    
    # Merge the DataFrames on 'DQAID'
    nccs_checks = status_df.merge(mem_dataframe, on='DQAID', how='inner')
    
    # Select specific columns from status_df
    nccs_checks = nccs_checks[['DQAID', 'T5D12', 'T5D13']]
    
    # Now 'nccs_checks' contains 'DQAID', 'T4M_8', 'T4M_7', 'T5D12', and 'T5D13'
    
    # Your further processing of 'nccs_checks' here
    print(f'NCCS checks for file date {status_date}')
    print(nccs_checks)
    print('-' * 50)
    













i want to do qc checks of nncs group so from below mem_df get me only data where T4M_7 = 1 and then make new 
dataframe mem_dataframe=mem_df[['DQAID','T4M_8','T4M_7']] then now from status_df match dqaid of status df with d
dqaid of member dataframe and now from both make new dataframe of nccs checks having 'DQAID','T4M_8','T4M_7' from member
data frame and T5D12 AND T5D13 FROM STATUS FILE 

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
    # Get the date part from the status_file name
    status_date = os.path.basename(status_file).split("_")[-1]
    
    # Read the CSV files into DataFrames
    status_df = pd.read_csv( status_file, dtype=str, encoding='latin',index_col=False)
    mem_df = pd.read_csv(mem_file, dtype=str, encoding='latin',index_col=False)
    tv_df = pd.read_csv(tv_file, dtype=str, encoding='latin',index_col=False)
    
    # Drop duplicates of DQAID in copied DataFrames
    status_df.drop_duplicates(subset=['DQAID'], inplace=True)
    mem_df[mem_df['T4M_7']==1]
    mem_dataframe=mem_df[['DQAID','T4M_8','T4M_7']]
    
    
