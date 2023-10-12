import pandas as pd
import os
import glob

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
    status_df = pd.read_csv( status_file, dtype=str, encoding='latin',index_col=False)
    mem_df = pd.read_csv(mem_file, dtype=str, encoding='latin',index_col=False)
    tv_df = pd.read_csv(tv_file, dtype=str, encoding='latin',index_col=False)
    
    # Drop duplicates of DQAID in copied DataFrames
    status_df.drop_duplicates(subset=['DQAID'], inplace=True)
    mem_df.drop_duplicates(subset=['DQAID'], inplace=True)
    tv_df.drop_duplicates(subset=['DQAID'], inplace=True)
    
    status_df_filtered = status_df[status_df['PI']!=12]
    fil_status=status_df_filtered[['T9Q17B_1']].isna().sum()
    


    if  not fil_status.empty:
        print(f'T9Q17B_1 (NaN) status file {status_date}\n',  fil_status )
        print('-'*50)
