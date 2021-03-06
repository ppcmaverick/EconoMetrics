A Helpful Library
=================

The reason I started this library is that the matplotlib implementation
of candlestick charts was good but it was off in little ways.  For example,
the whisker line was visible through the box.  So I started with their
drawing code and tweaked it to draw a good candlestick plot.

I also found myself working with Panda's DataFrame component.  It's such
a handy component that I thought it made sense to draw charts directly
from a DataFrame.  So, as I need another chart, I'm going to add to this
collection.

Example Code
------------

    #
    # Based on the example program for Panda/matplotlib finance plots.

    import pandas.io.data as web
    import EconoMetrics.transformers as trans
    import EconoMetrics.charts as charts
    import matplotlib.pyplot as plt

    if __name__ == "__main__":
        dataframe = web.get_data_yahoo("MSFT", "09/01/2011", "09/01/2012")

        print dataframe[0:3]

        et = trans.EMATransformer()
        et.transform(dataframe)

        print dataframe[0:3]


        fig = plt.figure(figsize=(4,4))
        ax = fig.add_axes([0.1, 0.1, 0.8, 0.8])
        charts.candlestick(ax, dataframe, width=0.6)
        plt.plot(dataframe["Close_EMA_5"])

        plt.show()


EconoMetrics.charts.candlestick
--------------------------------

This draws a candlestick chart.  It expects a Panda DataFrame with the
following columns

* Open
* High
* Low
* Close

You can obtain a populated DataFrame by using the pandas.io.data.get_data_yahoo
function.

EconoMetrics.charts.macd
------------------------

This draws a moving average convergence/divergence graph, which includes
the MACD line (or the difference between the 12 and 26 day moving average),
the signal line (a 9 day moving average of the MACD line), and a histogram
illustrating the difference between the signal and MACD lines.  It expects
a DataFrame that contains the columns MACD, Signal and Histogram, but these
can be overriden.


EconoMetrics.charts.highlow
---------------------------

This draws smallish arrows at reation highs and reaction lows.  It expects
a column in the DataFrame called HighLow, with the value "High" for a
reaction high and "Low" for a reaction low.  The actual names can be overridden
in the arguments.

EconoMetrics.transformers.EMATransformer
----------------------------------------

Adds a column to a DataFrame that is the exponential moving average of the
target column.

EconoMetrics.transformers.MACDTransformer
-----------------------------------------

Adds columns for the MACD value, the Signal value and the Histogram to a
DataFrame.

EconoMetrics.transformers.HighLowTransformer
--------------------------------------------

Adds reaction highs and lows to a DataFrame.