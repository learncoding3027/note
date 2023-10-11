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





