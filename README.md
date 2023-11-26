def mapp(df_demo_OG ,strata_cols, sg_dict):
    df_demo_OG = df_demo_OG[df_demo_OG['INDIVIDUAL_ID']>0]   #only individuals not HHs
    df_demo_OG.columns = df_demo_OG.columns.str.lower()
    ### mapping demo columns as per ue
    gender_map = {1:'M',2:'F'}
    nccs_map   = {'A1':'A','A2':'A','A3':'A','B1':'B','B2':'B','C1':'C','C2':'C','D1':'DE','D2':'DE','E1':'DE','E2':'DE'}
    hhs_map = {i:'S' if i <3 else  'L' for i in range(13) }
    hhs_map[3], hhs_map[4]  = 'M',  'M'
    
    df_demo_OG['sg']           = df_demo_OG['state_group'].map(sg_dict)
    df_demo_OG.rename(columns  ={'detailed_town_class':'tc_5'}, inplace=True)
    df_demo_OG['gender']       = df_demo_OG['sex'].map(gender_map)
    df_demo_OG['nccs_4']       = df_demo_OG['detailed_nccs'].map(nccs_map)
    df_demo_OG.loc[df_demo_OG['age_group'].between(2, 14, 'both'), 'age_group'] = '02-14 yrs'
    df_demo_OG.loc[df_demo_OG['age_group'].between(15, 21, 'both'), 'age_group'] = '15-21 yrs'
    df_demo_OG.loc[df_demo_OG['age_group'].between(22, 30, 'both'), 'age_group'] = '22-30 yrs'
    df_demo_OG.loc[df_demo_OG['age_group'].between(31, 40, 'both'), 'age_group'] = '31-40 yrs'
    df_demo_OG.loc[df_demo_OG['age_group'].between(41, 50, 'both'), 'age_group'] = '41-50 yrs'
    df_demo_OG.loc[df_demo_OG['age_group'].between(51, 60, 'both'), 'age_group'] = '51-60 yrs'
    df_demo_OG.loc[df_demo_OG['age_group'].between(61, 500, 'both'), 'age_group'] ='61+ yrs'
#     df_demo_OG['age_group']    = pd.cut(df_demo_OG['age_group'], ag_cuts, labels= ag_labels).astype(str) 
    df_demo_OG['hhs_3']        = df_demo_OG['hh_size'].map(hhs_map)
    df_demo_OG['ag_sex']       = df_demo_OG['gender'] + '_' +df_demo_OG['age_group'] 
    df_demo_OG['key']          = df_demo_OG[strata_cols].apply(tuple,axis=1)
    return df_demo_OG



    TypeError                                 Traceback (most recent call last)
Cell In[164], line 1
----> 1 mapp(f0,strata_cols,sg_dict)

Cell In[163], line 15, in mapp(df_demo_OG, strata_cols, sg_dict)
     13 df_demo_OG['nccs_4']       = df_demo_OG['detailed_nccs'].map(nccs_map)
     14 df_demo_OG.loc[df_demo_OG['age_group'].between(2, 14, 'both'), 'age_group'] = '02-14 yrs'
---> 15 df_demo_OG.loc[df_demo_OG['age_group'].between(15, 21, 'both'), 'age_group'] = '15-21 yrs'


TypeError: '>=' not supported between instances of 'str' and 'int'
