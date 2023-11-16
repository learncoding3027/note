%%time
required_dates   = [20231005, 20231006, 20231012, 20231013, 20231019, 20231020]
grp_cols = ['EID', 'Gender'] ## apparently 'Key ID' is just Gender

dp = '2hdp'
in_dir = r'C:\Users\OneDrive - Broadcast Audience Research Council\Work\OOH_Temp\Input_Files\\'

dict_ff, dict_vw, df_pvt, df_melt = {}, {}, {}, {}

for date_ in required_dates:
    print(date_)
    dict_vw[date_]                    = pd.read_csv(in_dir + fr"viewership_ooh_{str(date_)}.csv")
    if date_ != 20231020 :     
        dict_ff[date_]                = pd.read_csv(in_dir + fr"{str(date_)}_footfall.csv")
        og_cols                       = dict_ff[date_].columns
        dict_ff[date_]['time_sec']    = dict_ff[date_]['Time'].str[:2].astype(int) * 60 + dict_ff[date_]['Time'].str[3:].astype(int)
        dict_ff[date_]['hhdp']        = (dict_ff[date_]['time_sec']//30 )/2
        dict_ff[date_]['hdp']         = (dict_ff[date_]['time_sec']//60 )
        dict_ff[date_]['2hdp']        = (dict_ff[date_]['time_sec']//120 )*2
        dict_ff[date_]['n_MF_inside'] = dict_ff[date_].groupby(grp_cols)['Activity/Counter'].cumsum()
        dict_ff[date_]['SG']          = dict_ff[date_].EID.map(rest_SG_map)
        dict_ff[date_]['TC']          = dict_ff[date_].EID.map(rest_TC_map)
        dict_ff[date_]['City']        = dict_ff[date_].EID.map(rest_city_map)
        
        df_pvt[date_]                 = dict_ff[date_].pivot_table(index=grp_cols, columns=[dp], values = ['Activity/Counter'], aggfunc='sum', fill_value=0)
        df_pvt[date_].columns         = df_pvt[date_].columns.get_level_values(level=1)
        
        df_melt[date_]                = df_pvt[date_].melt(ignore_index=False).reset_index()
og_cols = [i for i in og_cols if not (i.startswith('Unname'))]    
og_cols





dates_of_intrst = {20241019 : [20231005, 20231012], 20241020 : [20231006, 20231013]}  ## don't really need the dict, just the keys for the moment
dekho(dict_ff[20231005])
save_csv = False #True
out_dir  = cwd + r'\Output_Files\\'
seed     = 0
rng      = np.random.default_rng(seed)

for date in dates_of_intrst.keys():
    print(date)
    np.random.seed(0) 
    dict_ff[date]                 = pd.concat( [dict_ff[date-10_007], dict_ff[date-10_014]] )
    dict_ff[date]['rand']         = rng.uniform(low=0,high=1,size=[len(dict_ff[date]),1])  #np.random.rand()
    dict_ff[date]                 = dict_ff[date] .sort_values([ 'Time_Sec', 'EID'])
    dict_ff[date]['n_lines_rest'] = dict_ff[date].groupby('EID').Lat.transform('count')
    dict_ff[date]['tbd_even']     = dict_ff[date].groupby(['EID', '2hdp','Activity/Counter']).Lat.transform('count')  #'Gender'
    ##Filtering 
#     dict_ff[date]                 = dict_ff[date] [ (dict_ff[date].n_lines_rest>10) & (dict_ff[date].rand<0.67 ) ]
    dict_ff[date]                 = dict_ff[date] [ (dict_ff[date].n_lines_rest>10) & (dict_ff[date].tbd_even%2==0 ) ]
    
    dict_ff[date]                 = dict_ff[date] .sort_values([ 'Time_Sec', 'EID'])
    dict_ff[date].Date            = date - 10_000
    dekho(dict_ff[date]) 
     
    df_pvt[date]                  = dict_ff[date].pivot_table(index=grp_cols, columns=[dp], values = ['Activity/Counter'], aggfunc='sum', fill_value=0)
    df_pvt[date].columns          = df_pvt[date].columns.get_level_values(level=1)
    df_melt[date]                 = df_pvt[date].melt(ignore_index=False).reset_index()
    
    if save_csv == True:          dict_ff[date][og_cols].to_csv(out_dir + fr"{str(date-10_000)}_footfall.csv", index=False)
