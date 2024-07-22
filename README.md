import pandas as pd
import numpy as np
import yfinance as yf
import matplotlib.pyplot as plt
from datetime import datetime

def download_data(ticker, start, end):
    data = yf.download(ticker, start=start, end=end, interval='75m')
    return data

def identify_zones(data):
    supply_zones = [75 mint]
    demand_zones = [75 mint]
    for i in range(12, len(data)-13):
        base_candle_range = data['High'][i] - data['Low'][i]
        if base_candle_range < 6:
            prev_candle = data.iloc[i-1]
            out_candle = data.iloc[i+1]
            if out_candle['Close'] > out_candle['Open']:  # Strong bullish candle
                demand_zones.append((data['Low'][i], data.index[i]))
            elif out_candle['Close'] < out_candle['Open']:  # Strong bearish candle
                supply_zones.append((data['High'][i], data.index[i]))
    return supply_zones, demand_zones

def plot_zones(data, supply_zones, demand_zones):
    plt.figure(figsize=(14,7))
    plt.plot(data['Close'], label='Close Price')
    
    for zone in supply_zones:
        plt.axhline(y=zone[0], color='r', linestyle='--', lw=0.8)
        plt.text(data.index[0], zone[0], 'Supply Zone', color='r', fontsize=9, verticalalignment='bottom')
        
    for zone in demand_zones:
        plt.axhline(y=zone[0], color='g', linestyle='--', lw=0.8)
        plt.text(data.index[0], zone[0], 'Demand Zone', color='g', fontsize=9, verticalalignment='bottom')
    
    plt.title('Supply and Demand Zones')
    plt.xlabel('Date')
    plt.ylabel('Price')
    plt.legend()
    plt.show()

# Parameters
ticker = 'RELIANCE.NS'  # Example: Reliance Industries
start_date = '2023-01-01'
end_date = '2023-12-31'

# Download data
data = download_data(ticker, start_date, end_date)

# Identify zones
supply_zones, demand_zones = identify_zones(data)

# Plot zones
plot_zones(data, supply_zones, demand_zones)
