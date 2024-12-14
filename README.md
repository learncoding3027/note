#importing the datasets

annexure = pd.read_csv("Annexure-I.csv").dropna()

# aviation stocks dataset
american_airlines = pd.read_csv("AAL.csv").dropna()
american_airlines['Ticker'] = 'AAL'

allegiant_travel = pd.read_csv("ALGT.csv").dropna()
allegiant_travel['Ticker'] = 'ALGT'

alaska_air = pd.read_csv("ALk.csv").dropna()
alaska_air['Ticker'] = 'ALk'

delta_airlines = pd.read_csv("DAL.csv").dropna()
delta_airlines['Ticker'] = 'DAL'

hawaiian_holdings = pd.read_csv("HA.csv").dropna()
hawaiian_holdings['Ticker'] = 'HA'

southwest_airlines = pd.read_csv("LUV.csv").dropna()
southwest_airlines['Ticker'] = 'LUV'

# finance stocks dataset
barclays = pd.read_csv("BCS.csv").dropna()
barclays['Ticker'] = 'BCS'

credit_suisse = pd.read_csv("CS.csv").dropna()
credit_suisse['Ticker'] = 'CS'

deutsche_bank = pd.read_csv("DB.csv").dropna()
deutsche_bank['Ticker'] = 'DB'

goldman_sachs = pd.read_csv("GS.csv").dropna()
goldman_sachs['Ticker'] = 'GS'

morgan_stanley = pd.read_csv("MS.csv").dropna()
morgan_stanley['Ticker'] = 'MS'

wells_fargo = pd.read_csv("WFC.csv").dropna()
wells_fargo['Ticker'] = 'WFC'

# healthcare stocks dataset
johnson_and_johnson = pd.read_csv("JNJ.csv").dropna()
johnson_and_johnson['Ticker'] = 'JNJ'

merck_and_co = pd.read_csv("MRK.csv").dropna()
merck_and_co['Ticker'] = 'MRK'

pfizer = pd.read_csv("PFE.csv").dropna()
pfizer['Ticker'] = 'PFE'

unitedhealthgroup = pd.read_csv("UNH.csv").dropna()
unitedhealthgroup['Ticker'] = 'UNH'

# pharmaceutical stocks dataset
bausch_health = pd.read_csv("BHC.csv").dropna()
bausch_health['Ticker'] = 'BHC'

roche_holding = pd.read_csv("RHHBY.csv").dropna()
roche_holding['Ticker'] = 'RHHBY'

# technology stocks dataset
apple = pd.read_csv("AAPL.csv").dropna()
apple['Ticker'] = 'AAPL'

amazon = pd.read_csv("AMZN.csv").dropna()
amazon['Ticker'] = 'AMZN'

facebook = pd.read_csv("FB.csv").dropna()
facebook['Ticker'] = 'FB'

alphabet = pd.read_csv("GOOG.csv").dropna()
alphabet['Ticker'] = 'GOOG'

ibm = pd.read_csv("IBM.csv").dropna()
ibm['Ticker'] = 'IBM'

microsoft = pd.read_csv("MSFT.csv").dropna()
microsoft['Ticker'] = 'MSFT'

# s&p500 index
sp500 = pd.read_csv("S&P500.csv").dropna()
sp500['Ticker'] = 'S&P500'

