aDD THIS TO THE BELOW OF THE CODE ALSO I HAVE KEPT THE OUTPUT BUT I AM GETTING NONE IN ALL MAPCODE COLUMN
# Find IDs where the "code" column and "mapped code" column do not match
    nccs_check = list(df2[df2['nccs'] != df_nccs['mapped code']]['DQAID'])
    
    if not len(nccs_check)>0:
        print("IDs with mismatched 'code' and 'mapped code':", nccs_check)
    else:
        print("All IDs have matching 'code' and 'mapped code'.")




code_mapping = {(1,0):'E3',
(1,1):'E2',
(1,2):'E1',
(1,3):'D2',
(1,4):'D1',
(1,5):'C2',
(1,6):'C1',
(1,7):'C1',
(1,8):'B1',
(1,9):'B1',
(1,10):'B1',
(1,11):'B1',
(2,0):'E2',
(2,1):'E1',
(2,2):'E1',
(2,3):'D2',
(2,4):'C2',
(2,5):'C1',
(2,6):'B2',
(2,7):'B1',
(2,8):'A3',
(2,9):'A3',
(2,10):'A3',
(2,11):'A3',
(2,0):'E2',
(2,1):'E1',
(2,2):'E1',
(2,3):'D2',
(2,4):'C2',
(2,5):'C1',
(2,6):'B2',
(2,7):'B1',
(2,8):'A3',
(2,9):'A3',
(2,10):'A3',
(2,11):'A3',
(3,0):'E2',
(3,1):'E1',
(3,2):'D2',
(3,3):'D1',
(3,4):'C2',
(3,5):'C1',
(3,6):'B2',
(3,7):'B1',
(3,8):'A3',
(3,9):'A3',
(3,10):'A3',
(3,11):'A3',
(3,0):'E2',
(3,1):'E1',
(3,2):'D2',
(3,3):'D1',
(3,4):'C2',
(3,5):'C1',
(3,6):'B2',
(3,7):'B1',
(3,8):'A3',
(3,9):'A3',
(3,10):'A3',
(3,11):'A3',
(4,0):'E2',
(4,1):'E1',
(4,2):'D2',
(4,3):'D1',
(4,4):'C1',
(4,5):'B2',
(4,6):'B1',
(4,7):'A3',
(4,8):'A3',
(4,9):'A2',
(4,10):'A2',
(4,11):'A2',
(5,0):'E2',
(5,1):'D2',
(5,2):'D1',
(5,3):'C2',
(5,4):'C1',
(5,5):'B1',
(5,6):'A3',
(5,7):'A3',
(5,8):'A2',
(5,9):'A2',
(5,10):'A2',
(5,11):'A2',
(6,0):'E1',
(6,1):'D2',
(6,2):'D1',
(6,3):'C2',
(6,4):'B2',
(6,5):'B1',
(6,6):'A3',
(6,7):'A2',
(6,8):'A2',
(6,9):'A1',
(6,10):'A1',
(6,11):'A1',
(6,0):'E1',
(6,1):'D2',
(6,2):'D1',
(6,3):'C2',
(6,4):'B2',
(6,5):'B1',
(6,6):'A3',
(6,7):'A2',
(6,8):'A2',
(6,9):'A1',
(6,10):'A1',
(6,11):'A1',
(7,0):'D2',
(7,1):'D2',
(7,2):'D1',
(7,3):'C2',
(7,4):'B2',
(7,5):'B1',
(7,6):'A3',
(7,7):'A2',
(7,8):'A2',
(7,9):'A1',
(7,10):'A1',
(7,11):'A1',
(7,0):'D2',
(7,1):'D2',
(7,2):'D1',
(7,3):'C2',
(7,4):'B2',
(7,5):'B1',
(7,6):'A3',
(7,7):'A2',
(7,8):'A2',
(7,9):'A1',
(7,10):'A1',
(7,11):'A1'
}



import pandas as pd
import os
import glob

def map_to_code(row):
    education = row['T4M_8']
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
    status_df['T5D12']=status_df['T5D12'].astype(float).astype(int)
    # Filter mem_df for T4M_7 = 1
    mem_df_filtered = mem_df[mem_df['T4M_6'].str.contains('3')]
    
    # Select desired columns for mem_dataframe
    mem_dataframe = mem_df_filtered[['DQAID', 'T4M_8','T4M_6', 'T4M_7']]
    
    # Merge the DataFrames on 'DQAID'
    nccs_checks = status_df.merge(mem_dataframe, on='DQAID', how='inner')
    
    # Select specific columns from status_df
    nccs_checks = nccs_checks[['DQAID','T4M_8', 'T5D12','T4M_6', 'T5D13']]
    nccs_checks['mapped code'] = nccs_checks.apply(map_to_code, axis=1)
    # Now 'nccs_checks' contains 'DQAID', 'T4M_8', 'T4M_7', 'T5D12', and 'T5D13'
    
    # Your further processing of 'nccs_checks' here
    print(f'NCCS checks for file date {status_date}')
    print(nccs_checks)
    print('-' * 50)
     
    # Find IDs where the "code" column and "mapped code" column do not match
    nccs_check = list(df2[df2['nccs'] != df_nccs['mapped code']]['DQAID'])
    
    if not len(nccs_check)>0:
        print("IDs with mismatched 'code' and 'mapped code':", nccs_check)
    else:
        print("All IDs have matching 'code' and 'mapped code'.")




NCCS checks for file date 20231010.csv
            DQAID T4M_8  T5D12 T4M_6 T5D13 mapped code
0      3221743398     5      6   1;3    B1        None
1      3117028094     4      7     3    B1        None
2      3117181143     7      8     3    A2        None
3      3117668465     4      5   1;3    C1        None
4      3216292756     5      7   1;3    A3        None
