#Importing required libraries:

import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt

#Utilising full display

from IPython.display import display, HTML
display(HTML("<style>.container { width:110% !important; }</style>"))


import warnings
warnings.filterwarnings('ignore')

from sklearn.preprocessing import MinMaxScaler

#setting the jupyter view:

pd.options.display.max_columns = None
pd.options.display.max_rows = 10


# Funtion to check data info(shape, size, head)

def dekho(df,n=2, sample = False):
    '''To check shape and first n rows of dataframe - 
    just for visualizing first few rows and shape/size of data (No significance in code) '''
    print(df.shape, end=',\t')
    print([name for name in globals() if globals()[name] is df])
    display(df.sample(n))      if sample==True       else        display(df.head(n))
