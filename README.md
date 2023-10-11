import pandas as pd
import numpy as np
pd.set_option('display.max_rows', 100)
pd.set_option('display.max_columns', 100)
import warnings
warnings.filterwarnings('ignore')
import os 
import glob


# Folder path
folder_path = r"\\10.10.2.199\Application_Share\DV_INPUT\RECRUITMENT" 

# List all files in the folder
files = glob.glob(os.path.join(folder_path, 'HH_Status_*'))
# Find the latest dated file

# latest_file = max(files, key=os.path.getctime)
recruitment_files= files[-40:]


# Iterate through the recruitment files
for recruitment_file in recruitment_files:
    # Read the recruitment file into a DataFrame
    recruitment_df = pd.read_csv(recruitment_file , index_col=False)
    
    
    
    
DQAID	WID
3323178570	10145645
3323188980	10145691

