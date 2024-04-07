verbose       = False
set_sec_col= {'None': set()}
mx_merg_req=['ag_sex', 'nccs_3', 'tc_2']
config_file_path = r"\\10.10.2.199\Department_Share\MSCI_Team\MSci_Pure_Science\Methodology_Work\OOH_2022-11-23\Work\Vijay\OOH UE Iteration\Production\UE Jun_RIM\Merging algo RIM_OOH.xlsx"
mx_merg = {}
for i in mx_merg_req:     
    try:
        mx_merg[i] = pd.read_excel( config_file_path, sheet_name=i+'_ooh', skiprows=[0], index_col=0)
    except:
        print("Error in df_merg - most probably Merging Algo excel file is unavailable, or Code_Strata worksheet is missing in the excel file")
        break
            
print(mx_merg.keys())

levels = {}
max_merge = 4
for x in range(1,max_merge+1):
    levels[x]  = { j : { i :  (mx_merg[j][ mx_merg[j][i]>0 ][i] .sort_values().index[x-1]) if len(mx_merg[j][ mx_merg[j][i]>(x-1) ])>0 else np.nan  for i in mx_merg[j].columns} for j in mx_merg.keys() }
levels.keys()
levels


output

dict_keys(['ag_sex', 'nccs_3', 'tc_2'])
{1: {'ag_sex': {'1_1': '2_1',
   '1_2': '2_2',
   '1_3': '1_4',
   '1_4': '1_3',
   '1_5': '1_4',
   '1_6': '1_7',
   '1_7': '1_6',
   '2_1': '1_1',
   '2_2': '1_2',
   '2_3': '2_4',
   '2_4': '2_3',
   '2_5': '2_4',
   '2_6': '2_7',
   '2_7': '2_6'},
  'nccs_3': {1: 2, 2: 1, '34': 2},
  'tc_2': {'123': 4, '4': 123}},
 2: {'ag_sex': {'1_1': '1_2',
   '1_2': '1_1',
   '1_3': '1_5',
   '1_4': '1_5',
   '1_5': '1_6',
   '1_6': '1_5',
   '1_7': '1_5',
   '2_1': '1_2',
   '2_2': '1_1',
   '2_3': '2_5',
   '2_4': '2_5',
   '2_5': '2_6',
   '2_6': '2_5',
   '2_7': '2_5'},
  'nccs_3': {1: 34, 2: 34, '34': 1},
  'tc_2': {'123': nan, '4': nan}},
 3: {'ag_sex': {'1_1': '2_2',
   '1_2': '2_1',
   '1_3': '1_6',
   '1_4': '1_6',
   '1_5': '1_3',
   '1_6': '1_4',
   '1_7': '1_4',
   '2_1': '2_2',
   '2_2': '2_1',
   '2_3': '2_6',
   '2_4': '2_6',
   '2_5': '2_3',
   '2_6': '2_4',
   '2_7': '2_4'},
  'nccs_3': {1: nan, 2: nan, '34': nan},
  'tc_2': {'123': nan, '4': nan}},
 4: {'ag_sex': {'1_1': '1_3',
   '1_2': '1_3',
   '1_3': '1_7',
   '1_4': '1_7',
   '1_5': '1_7',
   '1_6': '1_3',
   '1_7': '1_3',
   '2_1': '1_3',
   '2_2': '1_3',
   '2_3': '2_7',
   '2_4': '2_7',
   '2_5': '2_7',
   '2_6': '2_3',
   '2_7': '2_3'},
  'nccs_3': {1: nan, 2: nan, '34': nan},
  'tc_2': {'123': nan, '4': nan}}}
