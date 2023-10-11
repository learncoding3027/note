i want also load HH status*, HH member* ,HHmemeber_ * but take only eight numeric digit and not other files with more than that after under score after that make copy of member and tv files and from that copy drop duplicates of column DQAID and then match the count of all DQAID in three files one of original HH staus file and other from that copies of two file and each days 


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
