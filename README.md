import pandas as pd

# Example dataframes
df1 = pd.DataFrame({'Name': ['John', 'Peter', 'John', 'Alice', 'Peter', 'John']})
df2 = pd.DataFrame({'Name': ['John', 'Peter', 'Alice', 'Bob'], 'Count': [0, 0, 0, 0]})

# Compute value counts
value_counts = df1['Name'].value_counts()

# Update the second dataframe
for name, count in value_counts.items():
    if name in df2['Name'].values:
        df2.loc[df2['Name'] == name, 'Count'] = count
    else:
        df2 = df2.append({'Name': name, 'Count': count}, ignore_index=True)

# Fill missing values with 0
df2['Count'].fillna(0, inplace=True)

print(df2)







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
    
    
    fil_status=status_df[['T5D13']].isna().sum()
    


    if  not fil_status.empty:
        print(f'T5D13 (NaN) status file {status_date}\n',  fil_status )
        print('-'*50)





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
    
    # Filter status_df for rows where T5D13 is null
    status_df_null_T5D13 = status_df[status_df['T5D13'].isna()]
    
    # Get the DQAIDs from the filtered status_df
    null_T5D13_dqaid = status_df_null_T5D13['DQAID']
    
    if not null_T5D13_dqaid.empty:
        print(f'DQAID with T5D13 as NaN in status file {status_date}:\n', null_T5D13_dqaid)
        print('-' * 50)
        
