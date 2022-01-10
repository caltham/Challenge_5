# Challenge_5

The goal of this project is to build a tool to help credit union members evaluate their financial health; to assess their monthly budgets, and to forecast a reasonably effective retirement plan based on their current holdings of cryptocurrencies, stocks, and bonds.

## Technology

<img width="400" alt="Import" src="https://user-images.githubusercontent.com/94569323/148706898-d39aab88-478e-48bc-a6eb-6dd7de59914a.png">
**Note(see above):** After importing these required technologies and following the steps below, the program should be capable of functioning properly.

## Installation Guide

Load the environment variables from the .env file by calling the load_dotenv function.
~~~
load_dotenv()
~~~

## Usage

### Create a Financial Planner for Emergencies

Create variables for the current number of coins for each cryptocurrency asset held in the portfolio.
~~~
btc_coins = 1.2
eth_coins = 5.3
~~~

1. Create a variable named monthly_income, and set its value to 12000.
~~~
monthly_income = 12000
~~~

Create variables for the Free Crypto API Call endpoint URLs for the held cryptocurrency assets.
~~~
btc_url = "https://api.alternative.me/v2/ticker/Bitcoin/?convert=USD"
eth_url = "https://api.alternative.me/v2/ticker/Ethereum/?convert=USD"
~~~

2. Use the Requests library to get the current price (in US dollars) of Bitcoin (BTC) and Ethereum (ETH) by using the API endpoints that the starter code supplied.

Using the Python requests library, make an API call to access the current price of BTC.
~~~
btc_response = requests.get(btc_url).json()
~~~

Use the json.dumps function to review the response data from the API call. Use the indent and sort_keys parameters to make the response object readable.
~~~
print(json.dumps(btc_response, indent=4, sort_keys=True))
~~~

Using the Python requests library, make an API call to access the current price ETH.
~~~
eth_response = requests.get(eth_url).json()
~~~

Use the json.dumps function to review the response data from the API call. Use the indent and sort_keys parameters to make the response object readable.
~~~
print(json.dumps(eth_response, indent=4, sort_keys=True))
~~~

3. Navigate the JSON response object to access the current price of each coin, and store each in a variable.

Navigate the BTC response object to access the current price of BTC
~~~
btc_price = btc_response['data']['1']['quotes']['USD']['price']
~~~

Print the current price of BTC
~~~
print(f"The price for Bitcoin is {btc_price}")
~~~

Navigate the BTC response object to access the current price of ETH
~~~
eth_price = eth_response['data']['1027']['quotes']['USD']['price']
~~~

Print the current price of ETH
~~~
print(f"The price for Ethereum is {eth_price}")
~~~

4. Calculate the value, in US dollars, of the current amount of each cryptocurrency and of the entire cryptocurrency wallet.

Compute the current value of the BTC holding 
~~~
btc_value = btc_coins * btc_price
~~~

Print current value of your holding in BTC
~~~
print(f"The value for the holding in Bitcoin is {btc_value}")
~~~

Compute the current value of the ETH holding 
~~~
eth_value = eth_coins * eth_price
~~~

Print current value of your holding in ETH
~~~
print(f"The value for the holding in Ethereum is {eth_value}")
~~~

Compute the total value of the cryptocurrency wallet by adding the value of the BTC holding to the value of the ETH holding.
~~~
total_crypto_wallet = eth_value + btc_value
~~~

Print current cryptocurrency wallet balance
~~~
print(f"The value for the cryotocurrency wallet is {total_crypto_wallet}")
~~~

### Evaluate the Stock and Bond Holdings by Using the Alpaca SDK.

Set variables for the current amount of shares held in both the stock (SPY) and bond (AGG) portion of the portfolio.
~~~
spy_shares = 110
agg_shares = 200
~~~

1. In the Starter_Code folder, create an environment file (.env) to store the values of your Alpaca API key and Alpaca secret key.

2. Set the variables for the Alpaca API and secret keys. Using the Alpaca SDK, create the Alpaca tradeapi.REST object. In this object, include the parameters for the Alpaca API key, the secret key, and the version number.

Set the variables for the Alpaca API and secret keys.
~~~
alpaca_api_key = os.getenv("ALPACA_API_KEY")
alpaca_secret_key = os.getenv("ALPACA_SECRET_KEY")
~~~

Create the Alpaca tradeapi.REST object.
~~~
alpaca = tradeapi.REST(
    alpaca_api_key,
    alpaca_secret_key,
    api_version="v2")
~~~

3. Set the parameters for the Alpaca API call.

Set the tickers for both the bond and stock portion of the portfolio.
~~~
tickers = ["SPY", "AGG"]
~~~

Set timeframe to 1D.
~~~
timeframe = "1D"
~~~

Format current date as ISO format. Set both the start and end date at the date of your prior weekday. Alternatively you can use a start and end date of 2020-08-07.
~~~
start_date = pd.Timestamp("2020-08-07").isoformat()
end_date = pd.Timestamp("2020-08-07").isoformat()
~~~

4. Get the current closing prices for SPY and AGG by using the Alpaca get_barset function. Format the response as a Pandas DataFrame by including the df property at the end of the get_barset function.

Use the Alpaca get_barset function to get current closing prices the portfolio. Be sure to set the `df` property after the function to format the response object as a DataFrame.
~~~
limit_rows = 1

alpaca_df = alpaca.get_barset(
    tickers,
    timeframe,
    start = start_date,
    end = end_date,
    limit = limit_rows
).df
~~~

Review the first 5 rows of the Alpaca DataFrame
~~~
alpaca_df.head()
~~~

5. Navigating the Alpaca response DataFrame, select the SPY and AGG closing prices, and store them as variables.

Access the closing price for AGG from the Alpaca DataFrame. Be sure the value is a floating point number.
~~~
agg_close_price = (alpaca_df["AGG"]["close"][0])
~~~

Print the AGG closing price.
~~~
print(agg_close_price)
~~~

Access the closing price for SPY from the Alpaca DataFrame. Be sure the value is a floating point number.
~~~
spy_close_price = (alpaca_df["SPY"]["close"][0])
~~~

Print the SPY closing price
~~~
print(spy_close_price)
~~~

6. Calculate the value, in US dollars, of the current amount of shares in each of the stock and bond portions of the portfolio, and print the results.

Calculate the current value of the bond portion of the portfolio.
~~~
agg_value = agg_close_price * agg_shares
~~~

Print the current value of the bond portfolio.
~~~
print(agg_value)
~~~

Calculate the current value of the stock portion of the portfolio.
~~~
spy_value = agg_close_price * spy_shares
~~~

Print the current value of the stock portfolio.
~~~
print(spy_value)
~~~

Calculate the total value of the stock and bond portion of the portfolio.
~~~
total_stocks_bonds = spy_value + agg_value
~~~

Print the current balance of the stock and bond portion of the portfolio.
~~~
print(total_stocks_bonds)
~~~

Calculate the total value of the member's entire savings portfolio by adding the value of the cryptocurrency walled to the value of the total stocks and bonds.
~~~
total_portfolio = total_stocks_bonds + total_crypto_wallet
~~~

Print current cryptocurrency wallet balance.
~~~
print(total_portfolio)
~~~

### Evaluate the Emergency Fund.

1. Create a Python list named savings_data that has two elements. The first element contains the total value of the cryptocurrency wallet. The second element contains the total value of the stock and bond portions of the portfolio.

Consolidate financial assets data into a Python list.
~~~
savings_data = [total_crypto_wallet, total_stocks_bonds]
~~~

Review the Python list savings_data.
~~~
savings_data
~~~

2. Use the savings_data list to create a Pandas DataFrame named savings_df, and then display this DataFrame.

Create a Pandas DataFrame called savings_df. 
~~~
savings_df = pd.DataFrame(savings_data,
    columns = ["amount"],
    index = ["crypto", "stock/bond"])
~~~

Display the savings_df DataFrame.
~~~
savings_df
~~~

3. Use the savings_df DataFrame to plot a pie chart that visualizes the composition of the member’s portfolio. The y-axis of the pie chart uses amount. Be sure to add a title.

Plot the total value of the member's portfolio (crypto and stock/bond) in a pie chart.
~~~
savings_df.plot.pie(y = "amount", title = "Savings Portfolio")
~~~
<img width="571" alt="Plot_1" src="https://user-images.githubusercontent.com/94569323/148709781-9da218e3-c186-4bf9-a470-632e077b74e5.png">

4. Using Python, determine if the current portfolio has enough to create an emergency fund as part of the member’s financial plan. Ideally, an emergency fund should equal to three times the member’s monthly income. To do this, implement the following steps.

Create a variable named emergency_fund_value, and set it equal to three times the value of the member’s monthly_income of 12000.
~~~
emergency_fund_value = monthly_income * 3
~~~

Evaluate the possibility of creating an emergency fund with 3 conditions.
~~~
if total_portfolio > emergency_fund_value:
    print("Congratulations! You have enough money in this fund.")
elif total_portfolio == emergency_fund_value:
    print("Congratulations! You have reached your financial goal.")
else:
    print(f"You are {emergency_fund_value - total_portfolio} away from your goal.")
~~~

### Create a Financial Planner for Retirement.

1. Make an API call via the Alpaca SDK to get 3 years of historical closing prices for a traditional 60/40 portfolio split: 60% stocks (SPY) and 40% bonds (AGG).

Set start and end dates of 3 years back from your current date. Alternatively, you can use an end date of 2020-08-07 and work 3 years back from that date. 
~~~
start_date_2 = pd.Timestamp("2017-08-07").isoformat()
~~~

Set number of rows to 1000 to retrieve the maximum amount of rows.
~~~
limit_rows = 1000
~~~

Use the Alpaca get_barset function to make the API call to get the 3 years worth of pricing data. The tickers and timeframe parameters should have been set in Part 1 of this activity. The start and end dates should be updated with the information set above. Remember to add the df property to the end of the call so the response is returned as a DataFrame
~~~
alpaca_df_2 = alpaca.get_barset(
    tickers,
    timeframe,
    start = start_date_2,
    end = end_date,
    limit = limit_rows
).df
~~~

Display both the first and last five rows of the DataFrame.
~~~
display(alpaca_df_2.head())
display(alpaca_df_2.tail())
~~~

2. Run a Monte Carlo simulation of 500 samples and 30 years for the 60/40 portfolio, and then plot the results.

Configure the Monte Carlo simulation to forecast 30 years cumulative returns. The weights should be split 40% to AGG and 60% to SPY. Run 500 samples.
~~~
MC_thirty_year = MCSimulation(
  portfolio_data = alpaca_df_2,
  weights = [.60,.40],
  num_simulation = 500,
  num_trading_days = 252*30
)
~~~

Review the simulation input data.
~~~
MC_thirty_year.portfolio_data.head()
~~~

Run the Monte Carlo simulation to forecast 30 years cumulative returns.
~~~
MC_thirty_year.calc_cumulative_return()
~~~

Visualize the 30-year Monte Carlo simulation by creating an overlay line plot.
~~~
MC_sim_line_plot = MC_thirty_year.plot_simulation()
MC_sim_line_plot
~~~
<img width="660" alt="Plot_2" src="https://user-images.githubusercontent.com/94569323/148710370-ccbd8e16-2d1d-4b15-b5a1-86abc74e68f5.png">

3. Plot the probability distribution of the Monte Carlo simulation.

Visualize the probability distribution of the 30-year Monte Carlo simulation by plotting a histogram.
~~~
MC_sim_dist_plot = MC_thirty_year.plot_distribution()
MC_sim_dist_plot
~~~
<img width="658" alt="Plot_3" src="https://user-images.githubusercontent.com/94569323/148710511-edca5bb0-1894-418b-9c35-131b9b84660d.png">

4. Generate the summary statistics for the Monte Carlo simulation.

Generate summary statistics from the 30-year Monte Carlo simulation results. Save the results as a variable.
~~~
MC_summary_statistics = MC_thirty_year.summarize_cumulative_return()
~~~

Review the 30-year Monte Carlo summary statistics.
~~~
print(MC_summary_statistics)
~~~

### Analyze the Retirement Portfolio Forecasts.

Print the current balance of the stock and bond portion of the members portfolio.
~~~
print(total_stocks_bonds)
~~~

Use the lower and upper 95% confidence intervals to calculate the range of the possible outcomes for the current stock/bond portfolio.
~~~
ci_lower_thirty_cumulative_return = MC_summary_statistics[8] * total_stocks_bonds
ci_upper_thirty_cumulative_return = MC_summary_statistics[9] * total_stocks_bonds
~~~

Print the result of your calculations.
~~~
print(f"There is a 95% chance that an initial investment of ${total_stocks_bonds} in the portfolio"
  f" over the next 30 years will end within in the range of"
  f" {ci_lower_thirty_cumulative_return: .2f} and ${ci_upper_thirty_cumulative_return: .2f}.")
~~~

### Forecast Cumulative Returns in 10 Years.

Configure a Monte Carlo simulation to forecast 10 years cumulative returns. The weights should be split 20% to AGG and 80% to SPY. Run 500 samples.
~~~
MC_ten_year = MCSimulation(
  portfolio_data = alpaca_df_2,
  weights = [.80,.20],
  num_simulation = 500,
  num_trading_days = 252*10
)
~~~

Review the simulation input data.
~~~
MC_ten_year.portfolio_data.head()
~~~

Run the Monte Carlo simulation to forecast 10 years cumulative returns.
~~~
MC_ten_year.calc_cumulative_return()
~~~

Visualize the 10-year Monte Carlo simulation by creating an overlay line plot.
~~~
MC_sim_line_plot_2 = MC_ten_year.plot_simulation()
MC_sim_line_plot_2
~~~
<img width="657" alt="Plot_4" src="https://user-images.githubusercontent.com/94569323/148710957-6f6aa7c5-1d67-4662-afe6-5c0b6e468cc6.png">

Visualize the probability distribution of the 10-year Monte Carlo simulation by plotting a histogram.
~~~
MC_sim_dist_plot_2 = MC_ten_year.plot_distribution()
MC_sim_dist_plot_2
~~~
<img width="654" alt="Plot_5" src="https://user-images.githubusercontent.com/94569323/148711113-0e022c58-381b-4d69-8de0-02e5cfa7f2ed.png">

Generate summary statistics from the 10-year Monte Carlo simulation results. Save the results as a variable.
~~~
MC_summary_statistics_2 = MC_ten_year.summarize_cumulative_return()
~~~

Review the 10-year Monte Carlo summary statistics.
~~~
MC_summary_statistics_2
~~~

Print the current balance of the stock and bond portion of the members portfolio
~~~
print(total_stocks_bonds)
~~~

Use the lower and upper 95% confidence intervals to calculate the range of the possible outcomes for the current stock/bond portfolio.
~~~
ci_lower_ten_cumulative_return = MC_summary_statistics_2[8] * total_stocks_bonds
ci_upper_ten_cumulative_return = MC_summary_statistics_2[9] * total_stocks_bonds
~~~

Print the result of your calculations.
~~~
print(f"There is a 95% chance that an initial investment of {total_stocks_bonds} in the portfolio"
  f" over the next 10 years will end within in the range of"
  f" {ci_lower_ten_cumulative_return: .2f} and {ci_upper_ten_cumulative_return: .2f}.")
~~~

Answer the following question: Will weighting the portfolio more heavily to stocks allow the credit union members to retire after only 10 years?

Answer: Weighting the portfolio more heavily to stocks will not allow union members to retire after only 10 years.

## Contributors

Chancie Altham: Developer

## License

MIT License has been added to the project.