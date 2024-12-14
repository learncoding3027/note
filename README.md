# concatenating all datasets into one
all_data = pd.concat([american_airlines, allegiant_travel, alaska_air, delta_airlines, hawaiian_holdings, southwest_airlines,
                      barclays, credit_suisse, deutsche_bank, goldman_sachs, morgan_stanley, wells_fargo,
                      johnson_and_johnson, merck_and_co, pfizer, unitedhealthgroup,
                      bausch_health, roche_holding,
                      apple, amazon, facebook, alphabet, ibm, microsoft, sp500], ignore_index=True)

# displaying the concatenated dataframe
dekho(all_data)




# Merging all stocks data with annexure to get Industry and company name. 

stocks = pd.merge(all_data,annexure,on='Ticker',how ='left')


## 1. Understanding the Data


# Checking shape:

stocks.shape


# Checking null values:
stocks.isnull().sum()


stocks[stocks['Industry'].isna()]['Ticker'].unique()  # Below are the reasons for the null values


##### There are 7551 null present in Industry and company name because for following reasons:
###### 1. Ticker name mistake ALk wich should be ALK.
###### 2. There is extra space in "UNH " which should be "UNH".
###### 3. Industry type of S&P500 is not available so replacing NA in ndustry & Company Name.



all_data['Ticker'] = all_data['Ticker'].replace({'ALk':'ALK'})
all_data['Ticker'] = all_data['Ticker'].replace({'UNH ':'UNH'})
annexure['Ticker'] = annexure['Ticker'].replace({'UNH ':'UNH'})




stocks                   = pd.merge(all_data,annexure,on='Ticker',how ='left')
stocks['Industry']       = stocks['Industry'].fillna('S&P500')
stocks['Company Name']   = stocks['Company Name'].fillna('S&P500')


stocks.isnull().sum()  # No null present


# Checking the Merege dataset info
stocks.info()



# Checking null values:
stocks.isnull().sum()



# Checking the describe
stocks.describe()



# checking 
stocks.dtypes


# Merged and clean data
stocks.to_csv('Clean_data.csv')



import pandas as pd
from sklearn.preprocessing import MinMaxScaler

columns_to_normalize = ['Open', 'High', 'Low', 'Close', 'Adj Close', 'Volume']
non_numeric_columns = ['Date', 'Ticker', 'Industry', 'Company Name']

# Split the DataFrame into numeric and non-numeric parts
df_numeric         = stocks[columns_to_normalize]
df_non_numeric     = stocks[non_numeric_columns]

# Create a MinMaxScaler and normalize the numeric columns
scaler                = MinMaxScaler()
df_numeric_normalized = pd.DataFrame(scaler.fit_transform(df_numeric), columns=columns_to_normalize)

# Merge normalized numeric columns with non-numeric columns
df_normalized = pd.concat([df_non_numeric.reset_index(drop=True), df_numeric_normalized.reset_index(drop=True)], axis=1)

print("Normalized and Merged DataFrame:")
dekho(df_normalized)


