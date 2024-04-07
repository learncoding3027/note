def fix_overlapping_merges3(merge_list):
    merge_list  = merge_list.str.split('__').apply(set)
    merge_list_super = []
    for idx, merged_set in enumerate(merge_list):
        for other_set in merge_list[idx + 1:]:
            if merged_set & other_set:  # Check for overlapping elements
                merged_set |= other_set  # Merge the sets by union
                other_set |= merged_set  # Update the other set (optional, as the sets are the same)
        merge_list_super .append(sorted(merged_set))
    return merge_list_super



def margin_counts_df (df, strata_cols , clps_thresh   = 5, verbose = False):
    ## simply getting the mgn_cts df
    mgn_cts                  = { i: df.groupby(['sub_panel_key', i]).strata_count.sum() for i in strata_cols[1:]}
    mgn_cts                  = pd.concat(mgn_cts).reset_index()
    mgn_cts.rename(columns   = {'level_0':'strata', 'level_2':'margin'}, inplace=True)
    mgn_cts['merge_0']       = mgn_cts['margin']
    mgn_cts['merge_0_tot']   = mgn_cts['strata_count']
    mgn_cts['sec_col']       = mgn_cts[['sub_panel_key', 'strata','margin']].apply(tuple, axis =1 )
    mgn_cts['sec_col_bool']  = mgn_cts['sec_col'].isin(set_sec_col)
    # print(mgn_cts.sec_col_bool.sum(),mgn_cts.sec_col_bool.count())
#     dekho(mgn_cts,9)
    for x in range(1,max_merge+1):
        mgn_cts['mg_' + str(x)] = mgn_cts.apply(lambda row: levels[x][row['strata']].get(row['margin'], np.nan), axis=1)
    # if verbose:   dekho(mgn_cts,1,True)
    if verbose:  dekho(mgn_cts[mgn_cts.sec_col_bool])
    
    ## now beginning the merging
    dfm           = mgn_cts.copy()
    mg_cols       = ['strata', 'sub_panel_key', 'strata_count']
    for x in range(1,max_merge+1):
        leftc, rightc =  ['mg_' + str(x)], ['margin']
        merged_df     = pd.merge(dfm, dfm[mg_cols + rightc] , how='left', left_on=mg_cols[:-1]+leftc, right_on=mg_cols[:-1]+rightc, suffixes=('', '_'+rightc[0]))
        dfm['merge_' + str(x) + '_count'] = merged_df['strata_count_margin'].fillna(0)
        dfm['merge_' + str(x)]            = dfm['merge_' + str(x-1)].astype(str) + '__' + dfm['mg_' + str(x)].astype(str)
        dfm['merge_' + str(x)+'_tot']     = dfm['merge_' + str(x-1) + '_tot'] + dfm['merge_' + str(x) + '_count']

    dfm['lvl_merge0']          = np.where(dfm.merge_0_tot>clps_thresh, 0,    np.where(dfm.merge_1_tot>clps_thresh, 1,    np.where(dfm.merge_2_tot>clps_thresh, 2,     np.where(dfm.merge_3_tot>clps_thresh, 3    ,  4)) ))
    dfm['lvl_merge']           = np.maximum(dfm['lvl_merge0'], dfm.sec_col_bool)
    dfm['merge_final']         = np.where(dfm.lvl_merge==0, dfm.merge_0,     np.where(dfm.lvl_merge==1, dfm.merge_1,     np.where(dfm.lvl_merge==2, dfm.merge_2,      np.where(dfm.lvl_merge==3, dfm.merge_3      , dfm.merge_4)) ))
    dfm['strata_count_final']  = np.where(dfm.lvl_merge==0, dfm.merge_0_tot, np.where(dfm.lvl_merge==1, dfm.merge_1_tot, np.where(dfm.lvl_merge==2, dfm.merge_2_tot,  np.where(dfm.lvl_merge==2, dfm.merge_3_tot  , dfm.merge_4_tot)) ))
    dfm['merge_final'] = dfm['merge_final'].astype(str)
    dfm['merge_final_list']    = dfm.merge_final.str.split('__').apply(set)
    # if verbose:  dekho(dfm,1)
    if verbose:  dekho(dfm[dfm['lvl_merge']>0])
    
    dfm['merge_final_superset']  = dfm.groupby(mg_cols[:-1])['merge_final'].transform( fix_overlapping_merges3)
    
    pattern = '|'.join(['[', ']', "'"])  ## this isn't working any more.. will have to do it using hte standard method
    dfm['merge_final_super']     = dfm['merge_final_superset'].astype(str).str.replace(pattern,'', regex=True).str.replace('{','').str.replace('}','').str.replace('[','').str.replace(']','').str.replace("'",'').str.replace(', ','__') #.join() 
    dfm ['strata_count_super']   = dfm .groupby(mg_cols[:-1] + ['merge_final_super']).strata_count.transform('sum')
    if verbose:  dekho(dfm,1)
#     dekho(dfm)
    # dfm[(dfm.sub_panel_key == ('Rajasthan', 'Rural', 'CDE')) & (dfm.strata == 'hhs_3')].sort_values('strata_count').reset_index(drop=True)
    return dfm    #, dfm[mg_cols + rightc]



def get_dfm_dicts(dfm, mg_cols, verbose=False):
    dfmo     = dfm. groupby(mg_cols[:-1] + ['merge_0'])          .merge_final_super  .first().reset_index()
    # dfmo     = dfm. groupby(mg_cols[:-1] + ['merge_final_super']).strata_count       .sum()  .reset_index()
    # dfm_dict = dfm. groupby(mg_cols[:-1] + ['merge_0'])          .merge_final_super  .first().to_dict()
    if verbose:  dekho(dfmo)
    dfm_dicts = { sc : dfmo[dfmo.strata==sc][['sub_panel_key', 'merge_0', 'merge_final_super']].set_index(['sub_panel_key', 'merge_0']).merge_final_super.to_dict() for sc in strata_cols[1:]}
    return dfm_dicts




def update_ue_file(df, strata_cols, dfm_dicts, verbose=False):
    df[[sc + '_OG' for sc in strata_cols[1:]]]                 = df[strata_cols[1:]]
    df[['Individuals_OG', 'strata_count_OG']]                  = df[['Individuals', 'strata_count']]
    for sc in strata_cols[1:]:                                 ## strata_cols should be like ['sg', 'tc_2', 'nccs_3', 'ag_sex']
        df[sc ]                                                = df[['sub_panel_key',sc]].apply(tuple, axis=1).map(dfm_dicts[sc])
    df['key_mer']                                              = df[strata_cols].apply(tuple, axis = 1)
    df[['strata_count_mer', 'Individuals_mer']]                = df.groupby(['sub_panel_key'] + strata_cols)[['strata_count_OG', 'Individuals_OG']].transform('sum')
#     df['rim_pp']                                               = df['Individuals_mer']/df['strata_count_mer']
    print("sub_wt_mer =", df.drop_duplicates(subset='key_rim')['Individuals_mer'].sum(),' Og_wt =',df.Individuals_OG.sum())
    return df




dfm                     = margin_counts_df ( ue_tv.copy(),  strata_cols, verbose=verbose )
dfm_dicts               = get_dfm_dicts    ( dfm.copy(), mg_cols, verbose=verbose )
df                      = update_ue_file   ( ue_tv.copy(),  strata_cols, dfm_dicts,  verbose=verbose)
ue_tv                   = df.copy()
dekho(ue_tv)




output

sub_wt_mer = 383768262.0  Og_wt = 383768262.0
['ue_tv']
(2632, 28)
SG_name	TC_code1	NCCS_code1	Sex_code1	Age_Group_code1	TV/ Non TV	Individuals	TC_code1_og	TC_match_del	tc_2	nccs_3	ag_sex	key_og	key	sub_panel_key	strata_count	tc_2_OG	nccs_3_OG	ag_sex_OG	Individuals_OG	strata_count_OG	key_rim	strata_count_rim	Individuals_rim	rim_pp	key_mer	strata_count_mer	Individuals_mer
0	PUN/CHA	123	4	1	7	TV	4395.0	2	[123]	123	34	1_7	(PUN/CHA, 2, 4, 1_7)	(PUN/CHA, 123, 34, 1_7)	(PUN/CHA, 123)	11	123	34	1_7	4395.0	11	(PUN/CHA, 123, 34, 1_7)	66	68141.0	1032.439394	(PUN/CHA, 123, 34, 1_7)	66	68141.0
1	PUN/CHA	123	4	2	7	TV	10200.0	3	[123]	123	34	2_7	(PUN/CHA, 3, 4, 2_7)	(PUN/CHA, 123, 34, 2_7)	(PUN/CHA, 123)	12	123	34	





def MP_get_dim_level_n_dicts(df, dim_names=dim_names):
    df              = df.sort_values(dim_names).copy()
    dim_levels      = { i: df[i].unique() for i in dim_names } # Get unique values for each dimension
    n_dim_levels    = { k:len(v) for k,v in dim_levels.items() }
    dim_name_dict   = {idx:name for  idx,name in enumerate(dim_names)}  
    dim_num_dict    = {name:idx for  idx,name in enumerate(dim_names)}
    return df, dim_levels, dim_name_dict, dim_num_dict, n_dim_levels


df_tv, dim_levels, dim_name_dict, dim_num_dict, n_dim_levels=MP_get_dim_level_n_dicts(df_tv, dim_names=dim_names)
dim_levels

output

{'SG_name': array(['AP/TELANGANA', 'BIHAR/JHARKHAND', 'DELHI SALES REGION',
        'GUJ/D&D/DNH', 'HAR/HP/J&K', 'KARNATAKA', 'KERALA', 'MAH/GOA',
        'MP/CG', 'NE/SIKKIM', 'ODISHA', 'PUN/CHA', 'RAJASTHAN',
        'TAMILNADU/PONDICHERRY', 'UP/UTTARAKHAND', 'WEST BENGAL'],
       dtype=object),
 'tc_2': array(['123', '4'], dtype=object),
 'nccs_3': array(['1', '2', '34'], dtype=object),
 'ag_sex': array(['1_1', '1_2', '1_3', '1_4', '1_5', '1_6', '1_7', '2_1', '2_2',
        '2_3', '2_4', '2_5', '2_6', '2_7'], dtype=object)}
1
df_tv.TC_code1.unique()


def MP_df_to_nd_nparray(df, dim_name_dict, dim_num_dict, dim_levels, sample_col=sample_col,  dim_names=dim_names):  
    samples_array = np.zeros( tuple([len(i) for i in dim_levels.values()]) )

    # Create dictionaries to map unique values to indices - LABEL NAME ENCODING
    dim_to_idx = { i: { dim:idx for idx, dim in enumerate(  sorted(dim_levels[i])  ) }  for i in dim_names }  ##sorted here is important because otherwise AG_SEX loses order and the wrong weights are assigned
    # print(dim_to_idx)
    # Fill the samples_array based on the dictionaries
    for idx, row in df.iterrows():
        dim_idx = { i: dim_to_idx[i][row[i]] for i in dim_names }
        samples_array[tuple(dim_idx.values())] = row[sample_col]
    return samples_array


samples_array= MP_df_to_nd_nparray(df_tv, dim_name_dict, dim_num_dict, dim_levels, sample_col=sample_col,  dim_names=dim_names)
print(samples_array.ravel().shape)
print(samples_array.ravel())

output

(1344,)
[63. 30. 45. ... 30. 18. 12.]







def MP_run_rimSV(df, samples, dim_name_dict, in_weight_col=in_weight_col, rim_weight_col=rim_weight_col, sample_col=sample_col, eps_margin=eps_margin, epsilon=epsilon,max_iterations=max_iterations, seedvalue=42, dim_names=dim_names):
    rng                    = random.seed(seedvalue)
    margin_wts             = { i : df.groupby(i)[in_weight_col].sum() for i in dim_names }
    margin_samples         = { i : df.groupby(i)[sample_col].sum() for i in dim_names }
    # print(margin_wts, margin_samples)
    total_marginal_sum = np.sum(margin_wts[ dim_names[0] ]) #+ np.sum(marginal_sex) + np.sum(marginal_economic)

    # Initialize weights as sample proportions
    weights            = samples / np.sum(samples)  #*total_marginal_sum
    print(samples.ravel().shape)
    print(weights.ravel().shape)
    residual_list      = []
    rand_prev, converged =-1,0  ##to keep track of which dimension was done earlier to not repeat that 
    for iteration in range(max_iterations):
        rand_int = randrange(len(dim_names))
        # Calculate scaling factors for each dimension
        if rand_int == rand_prev:
            rand_int = (rand_int+1)%(len(dim_names)) ## I'd consider "continue" instead, but then I'd have to decrement iteration as well

        dim_axis      = tuple([i for i in range(len(dim_names)) if i != rand_int])               ### for in range (0,1,2) if rand_int==1, this gives (0,2)
        dim_reshape   = tuple([-1 if i==rand_int else 1 for i in range(len(dim_names))])         ### this is used for reshape, for the same case 
        scaling       = margin_wts[dim_name_dict[rand_int]]  / np.sum(weights, axis=dim_axis)    ### just the coefficient ratios in their own dimension
        new_weights   = weights * scaling.values.reshape(dim_reshape) 
        
        new_weights  *= total_marginal_sum / np.sum(new_weights)   # Calculate the multiplication factor dynamically
        ##around here is where we'd like to cap for IN RIM CAPPING.. during the weighting part
        
        
        # Check for convergence
        residuals={}
        for int2 in range( len(dim_names) ):
            dim_axis2                       = tuple([i for i in range(len(dim_names)) if i != int2])  
            residuals[dim_name_dict[int2]]  = np.abs( np.sum( weights, axis=dim_axis2, keepdims=True).flatten() - margin_wts[dim_name_dict[int2]] ).sum() 
        residual                        = sum(residuals.values())
        if np.all(np.abs(new_weights - weights) < epsilon) and ( residual < eps_margin ): 
            converged=1
            break
        residual_list.append(residual)
        weights   = new_weights
#         print(weights)
        rand_prev = rand_int
#     np.sum(weights, axis=(1, 2), keepdims=True), np.sum(weights, axis=(0, 1), keepdims=True), np.sum(weights, axis=(0, 2), keepdims=True)
    print(df.shape)
    df[rim_weight_col]                        = weights.ravel()
    df[rim_weight_col.replace('strata','pp')] = df[rim_weight_col] / df[sample_col[:-3]]
    df['n_iterations']                        = iteration+1
    df['converged']                           = converged
    df['residual_list']                       = [residual_list] * len(df)
    
    # dekho(df)
    # print(df[[i for i in df.columns if 'eight' in i]].sum())
    # margin_wts_rim     = { i : df.groupby(i)[[rim_weight_col, in_weight_col]].sum() for i in dim_names }  ##maybe skip this for now.. 
    return df, converged,iteration+1,weights #, margin_wts_rim




df_tv, converged, iterations ,weights       = MP_run_rimSV(df_tv, samples_array, dim_name_dict=dim_name_dict)

output

(1344,)
(1344,)
(882, 28)
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
C:\Users\MONISH~1.LAL\AppData\Local\Temp\9/ipykernel_18184/3170671459.py in <module>
----> 1 df_tv, converged, iterations ,weights       = MP_run_rimSV(df_tv, samples_array, dim_name_dict=dim_name_dict)

C:\Users\MONISH~1.LAL\AppData\Local\Temp\9/ipykernel_18184/1049449256.py in MP_run_rimSV(df, samples, dim_name_dict, in_weight_col, rim_weight_col, sample_col, eps_margin, epsilon, max_iterations, seedvalue, dim_names)
     42 #     np.sum(weights, axis=(1, 2), keepdims=True), np.sum(weights, axis=(0, 1), keepdims=True), np.sum(weights, axis=(0, 2), keepdims=True)
     43     print(df.shape)
---> 44     df[rim_weight_col]                        = weights.ravel()
     45     df[rim_weight_col.replace('strata','pp')] = df[rim_weight_col] / df[sample_col[:-3]]
     46     df['n_iterations']                        = iteration+1

C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\frame.py in __setitem__(self, key, value)
   3610         else:
   3611             # set column
-> 3612             self._set_item(key, value)
   3613 
   3614     def _setitem_slice(self, key: slice, value):

C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\frame.py in _set_item(self, key, value)
   3782         ensure homogeneity.
   3783         """
-> 3784         value = self._sanitize_column(value)
   3785 
   3786         if (

C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\frame.py in _sanitize_column(self, value)
   4507 
   4508         if is_list_like(value):
-> 4509             com.require_length_match(value, self.index)
   4510         return sanitize_array(value, self.index, copy=True, allow_2d=True)
   4511 

C:\ProgramData\Anaconda3\lib\site-packages\pandas\core\common.py in require_length_match(data, index)
    529     """
    530     if len(data) != len(index):
--> 531         raise ValueError(
    532             "Length of values "
    533             f"({len(data)}) "

ValueError: Length of values (1344) does not match length of index (882)
