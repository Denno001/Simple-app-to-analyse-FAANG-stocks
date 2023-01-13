import streamlit as st
import yfinance as yf
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt


#...page labelling i.e header and sub-header
st.header('Simple App for Analysing FAANG Stocks')
st.write('''This app uses data from the yfinance python library with respect to opening prices, closing prices
adjusted close, volume, high and lows of the FAANG stocks namely; Meta Platforms Inc.(FB), Amazon Inc.(AMZN), Apple Inc.(AAPL), Netflix Inc.(NFLX) and Alphabet Inc.(GOOGL)
''')
('---')

#.....first container with 2 columns
st.write('### Data for the last 10 years (2012-2022)')
#....setting up 2 columns in ratio 2:1
col1, col2 = st.columns([2,1])
col1.subheader('Complete Data')
col2.subheader('Filtered Data')

#...extracting data from yfinance library
data = yf.download("FB AMZN AAPL NFLX GOOGL", start="2012-01-01", end="2022-12-31")
data2 = data['Adj Close']
data3 = data2.reset_index().set_index('Date', drop=False)
data3 = data3.reset_index(drop=True)

#..col1 to show data
col1.write(data)

#..inserting sidebar and its label
st.sidebar.header('Select Year')
year = st.sidebar.selectbox('Year', list(reversed(range(2012,2023))))
data3 = data3[data3['Date'].dt.year == year]


#...col2 to show data2
col2.write(data3)

#...showing data dimension
st.write('Data Dimension:(Rows, Columns)' +'Complete Data' + str(data.shape) + 'Filtered Data' + str(data3.shape))
('---')

#...ploting the 10 year chart
st.write('### 10-Year Chart')
fig, ax = plt. subplots()
data2.plot(ax=ax,)
ax.set_ylabel('Price')
st.pyplot(fig)
('---')

#...ploting selected annual charts
st.write('### Selected Year Chart')
fig, ax = plt. subplots()
data3.set_index('Date').plot(ax=ax)
ax.set_ylabel('Price')
st.pyplot(fig)


