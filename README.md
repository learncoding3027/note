# %%time
df_all =      pd.concat(dfs_out)

cols_to_drop = ['key_OG', 'residual_list']
df_all = df_all.reset_index(drop=True)
df_all = df_all[[i for i in df_all.columns if i not in cols_to_drop]]

dekho(df_all,1)
pq.write_table(pa.Table.from_pandas(df_all), fr"{sec_col_type}_df_all_pre_rec_MP.parquet", compression='BROTLI') #'GZIP'a 


---------------------------------------------------------------------------
AttributeError                            Traceback (most recent call last)
C:\Users\SHAURY~1.VER\AppData\Local\Temp\3/ipykernel_12416/4097862995.py in <module>
      7 
      8 dekho(df_all,1)
----> 9 pq.write_table(pa.Table.from_pandas(df_all), fr"{sec_col_type}_df_all_pre_rec_MP.parquet", compression='BROTLI') #'GZIP'a

AttributeError: 'str' object has no attribute 'write_table'
