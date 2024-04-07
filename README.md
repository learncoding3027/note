def MP_df_to_nd_nparray(df, dim_name_dict, dim_num_dict, dim_levels, sample_col=sample_col,  dim_names=dim_names):  
    samples_array = np.zeros( tuple([len(i) for i in dim_levels.values()]) )

    # Create dictionaries to map unique values to indices - LABEL NAME ENCODING
    dim_to_idx = { i: { dim:idx for idx, dim in enumerate(  sorted(dim_levels[i])  ) }  for i in dim_names }  ##sorted here is important because otherwise AG_SEX loses order and the wrong weights are assigned
    print(dim_to_idx)
    print(len(dim_to_idx))
    # Fill the samples_array based on the dictionaries
    for idx, row in df.iterrows():
        dim_idx = { i: dim_to_idx[i][row[i]] for i in dim_names }
        samples_array[tuple(dim_idx.values())] = row[sample_col]
    return samples_array
    

samples_array= MP_df_to_nd_nparray(df_tv, dim_name_dict, dim_num_dict, dim_levels, sample_col=sample_col,  dim_names=dim_names)
print(samples_array.ravel().shape)
print(samples_array.ravel())



{'SG_name': {'AP/TELANGANA': 0, 'BIHAR/JHARKHAND': 1, 'DELHI SALES REGION': 2, 'GUJ/D&D/DNH': 3, 'HAR/HP/J&K': 4, 'KARNATAKA': 5, 'KERALA': 6, 'MAH/GOA': 7, 'MP/CG': 8, 'NE/SIKKIM': 9, 'ODISHA': 10, 'PUN/CHA': 11, 'RAJASTHAN': 12, 'TAMILNADU/PONDICHERRY': 13, 'UP/UTTARAKHAND': 14, 'WEST BENGAL': 15}, 'tc_2': {'123': 0, '4': 1}, 'nccs_3': {'1': 0, '2': 1, '34': 2}, 'ag_sex': {'1_1': 0, '1_2': 1, '1_3': 2, '1_4': 3, '1_5': 4, '1_6': 5, '1_7': 6, '2_1': 7, '2_2': 8, '2_3': 9, '2_4': 10, '2_5': 11, '2_6': 12, '2_7': 13}}
4
(1344,)
[63. 30. 45. ... 30. 18. 12.]


