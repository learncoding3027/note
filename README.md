import pandas as pd
import matplotlib.pyplot as plt

# Creating a sample dataframe
data = {
    'ID': [1, 1, 1, 2, 2, 2, 3, 3, 3],
    'Delta1': [2, 4, 6, 1, 3, 5, 2, 3, 4],
    'Delta2': [3, 5, 7, 2, 4, 6, 3, 4, 5],
    'Delta3': [4, 6, 8, 3, 5, 7, 4, 5, 6]
}

df = pd.DataFrame(data)

# Group the data by 'ID'
grouped = df.groupby('ID')

# Plotting line charts for each delta
for key, group in grouped:
    plt.plot(group['ID'], group['Delta1'], label='Delta1')
    plt.plot(group['ID'], group['Delta2'], label='Delta2')
    plt.plot(group['ID'], group['Delta3'], label='Delta3')
    plt.xlabel('ID')
    plt.ylabel('Values')
    plt.title(f'Line Chart for ID {key}')
    plt.legend()
    plt.show()
    
