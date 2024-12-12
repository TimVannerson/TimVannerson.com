# RobinStocks Documentation 1.0.0 - Robinhood Functions 

## Sending Requests to API
Contains decorator functions and functions for interacting with global data.

### `robin_stocks.robinhood.helper.request_delete(url)`

Makes a DELETE request to the specified URL and returns the response.

**Parameters:**
- `url` (str): The URL to send a DELETE request to.

**Returns:**
- The data from the DELETE request.

---
### `robin_stocks.robinhood.helper.request_document(url, payload=None)`

Makes a GET request to the specified document URL and returns the session data.

**Parameters:**
- `url` (str): The URL to send a GET request to.
- `payload` (optional): The payload to include in the GET request.

**Returns:**
- The data from `session.get()` instead of `session.get().json()`.

---
### `robin_stocks.robinhood.helper.request_get(url, dataType='regular', payload=None, jsonify_data=True)`

Makes a GET request to the specified URL and returns the data.

**Parameters:**
- `url` (str): The URL to send a GET request to.
- `dataType` (str, optional): Determines how to filter the data. Possible values:
  - `'regular'`: Returns the unfiltered data.
  - `'results'`: Returns `data['results']`.
  - `'pagination'`: Returns `data['results']` and appends it with any data in `data['next']`.
  - `'indexzero'`: Returns `data['results'][0]`.
- `payload` (dict, optional): Dictionary of parameters to pass to the URL.
- `jsonify_data` (bool): If true, returns `requests.post().json()`, otherwise returns the response from `requests.post()`.

**Returns:**
- The data from the GET request. If `jsonify_data=True` and the HTTP code is not 200, returns either `[None]` or `None` based on the `dataType` parameter.

---
### `robin_stocks.robinhood.helper.request_post(url, payload=None, timeout=16, json=False, jsonify_data=True)`

Makes a POST request to the specified URL with the given payload and returns the response. Supports responses other than 200.

**Parameters:**
- `url` (str): The URL to send a POST request to.
- `payload` (dict, optional): Dictionary of parameters to pass to the URL.
- `timeout` (int, optional): The time in seconds for the POST request to wait for a response.
- `json` (bool): Sets the `content-type` parameter of the session header to `'application/json'`.
- `jsonify_data` (bool): If true, returns `requests.post().json()`, otherwise returns the response from `requests.post()`.

**Returns:**
- The data from the POST request.

## Logging In and Out
Contains all functions for the purpose of logging in and out to Robinhood.

### `robin_stocks.robinhood.authentication.login(username=None, password=None, expiresIn=86400, scope='internal', by_sms=True, store_session=True, mfa_code=None, pickle_name='')`

Logs the user into Robinhood by obtaining an authentication token and saving it to the session header. By default, the authentication token is stored in a pickle file for future logins.

**Parameters:**
- `username` (str, optional): The username for your Robinhood account, usually your email. Not required if credentials are already cached and valid.
- `password` (str, optional): The password for your Robinhood account. Not required if credentials are already cached and valid.
- `expiresIn` (int, optional): The time in seconds until your login session expires.
- `scope` (str, optional): Specifies the scope of the authentication.
- `by_sms` (bool, optional): Specifies whether to send a verification code via SMS (True) or email (False).
- `store_session` (bool, optional): Specifies whether to save the login authorization for future logins.
- `mfa_code` (str, optional): MFA token if enabled.
- `pickle_name` (str, optional): Allows users to name the pickle token file to switch between different accounts without re-login.

**Returns:**
- A dictionary with login information. The `access_token` key contains the access token, and the `detail` key contains information on whether the access token was generated or loaded from a pickle file.

---
### `robin_stocks.robinhood.authentication.logout()`

Removes authorization from the session header.

**Returns:**
- None

## Loading Profiles
Contains functions for getting all the information tied to a user account.

### `robin_stocks.robinhood.profiles.load_account_profile(account_number=None, info=None, dataType='indexzero')`

Gets the information associated with the account profile, including day trading information and cash being held by Robinhood.

**Parameters:**
- `account_number` (str, optional): The Robinhood account number.
- `info` (str, optional): The name of the key whose value is to be returned from the function.
- `dataType` (str, optional): Determines how to filter the data. Possible values:
  - `'regular'`: Returns the unfiltered data.
  - `'results'`: Returns `data['results']`.
  - `'pagination'`: Returns `data['results']` and appends it with any data in `data['next']`.
  - `'indexzero'`: Returns `data['results'][0]`.

**Returns:**
- A dictionary of key/value pairs. If a string is passed to the `info` parameter, returns a string corresponding to the value of the key whose name matches the `info` parameter.

**Dictionary Keys:**
- `url`
- `portfolio_cash`
- `can_downgrade_to_cash`
- `user`
- `account_number`
- `type`
- `created_at`
- `updated_at`
- `deactivated`
- `deposit_halted`
- `only_position_closing_trades`
- `buying_power`
- `cash_available_for_withdrawal`
- `cash`
- `cash_held_for_orders`
- `uncleared_deposits`
- `sma`
- `sma_held_for_orders`
- `unsettled_funds`
- `unsettled_debit`
- `crypto_buying_power`
- `max_ach_early_access_amount`
- `cash_balances`
- `margin_balances`
- `sweep_enabled`
- `instant_eligibility`
- `option_level`
- `is_pinnacle_account`
- `rhs_account_number`
- `state`
- `active_subscription_id`
- `locked`
- `permanently_deactivated`
- `received_ach_debit_locked`
- `drip_enabled`
- `eligible_for_fractionals`
- `eligible_for_drip`
- `eligible_for_cash_management`
- `cash_management_enabled`
- `option_trading_on_expiration_enabled`
- `cash_held_for_options_collateral`
- `fractional_position_closing_only`
- `user_id`
- `rhs_stock_loan_consent_status`

---
### `robin_stocks.robinhood.profiles.load_basic_profile(info=None)`

Gets the information associated with the personal profile, such as phone number, city, marital status, and date of birth.

**Parameters:**
- `info` (str, optional): The name of the key whose value is to be returned from the function.

**Returns:**
- A dictionary of key/value pairs. If a string is passed to the `info` parameter, returns a string corresponding to the value of the key whose name matches the `info` parameter.

**Dictionary Keys:**
- `user`
- `address`
- `city`
- `state`
- `zipcode`
- `phone_number`
- `marital_status`
- `date_of_birth`
- `citizenship`
- `country_of_residence`
- `number_dependents`
- `signup_as_rhs`
- `tax_id_ssn`
- `updated_at`

---
### `robin_stocks.robinhood.profiles.load_investment_profile(info=None)`

Gets the information associated with the investment profile. These are the answers to the questionnaire you filled out when creating your profile.

**Parameters:**
- `info` (str, optional): The name of the key whose value is to be returned from the function.

**Returns:**
- A dictionary of key/value pairs. If a string is passed to the `info` parameter, returns a string corresponding to the value of the key whose name matches the `info` parameter.

**Dictionary Keys:**
- `user`
- `total_net_worth`
- `annual_income`
- `source_of_funds`
- `investment_objective`
- `investment_experience`
- `liquid_net_worth`
- `risk_tolerance`
- `tax_bracket`
- `time_horizon`
- `liquidity_needs`
- `investment_experience_collected`
- `suitability_verified`
- `option_trading_experience`
- `professional_trader`
- `understand_option_spreads`
- `interested_in_options`
- `updated_at`

---
### `robin_stocks.robinhood.profiles.load_portfolio_profile(account_number=None, info=None)`

Gets the information associated with the portfolio profile, such as withdrawable amount, market value of the account, and excess margin.

**Parameters:**
- `info` (str, optional): The name of the key whose value is to be returned from the function.

**Returns:**
- A dictionary of key/value pairs. If a string is passed to the `info` parameter, returns a string corresponding to the value of the key whose name matches the `info` parameter.

**Dictionary Keys:**
- `url`
- `account`
- `start_date`
- `market_value`
- `equity`
- `extended_hours_market_value`
- `extended_hours_equity`
- `extended_hours_portfolio_equity`
- `last_core_market_value`
- `last_core_equity`
- `last_core_portfolio_equity`
- `excess_margin`
- `excess_maintenance`
- `excess_margin_with_uncleared_deposits`
- `excess_maintenance_with_uncleared_deposits`
- `equity_previous_close`
- `portfolio_equity_previous_close`
- `adjusted_equity_previous_close`
- `adjusted_portfolio_equity_previous_close`
- `withdrawable_amount`
- `unwithdrawable_deposits`
- `unwithdrawable_grants`

---
### `robin_stocks.robinhood.profiles.load_security_profile(info=None)`

Gets the information associated with the security profile.

**Parameters:**
- `info` (str, optional): The name of the key whose value is to be returned from the function.

**Returns:**
- A dictionary of key/value pairs. If a string is passed to the `info` parameter, returns a string corresponding to the value of the key whose name matches the `info` parameter.

**Dictionary Keys:**
- `user`
- `object_to_disclosure`
- `sweep_consent`
- `control_person`
- `control_person_security_symbol`
- `security_affiliated_employee`
- `security_affiliated_firm_relationship`
- `security_affiliated_firm_name`
- `security_affiliated_person_name`
- `security_affiliated_address`
- `security_affiliated_address_subject`
- `security_affiliated_requires_duplicates`
- `stock_loan_consent_status`
- `agreed_to_rhs`
- `agreed_to_rhs_margin`
- `rhs_stock_loan_consent_status`
- `updated_at`

---
### `robin_stocks.robinhood.profiles.load_user_profile(info=None)`

Gets the information associated with the user profile, such as username, email, and links to the URLs for other profiles.

**Parameters:**
- `info` (str, optional): The name of the key whose value is to be returned from the function.

**Returns:**
- A dictionary of key/value pairs. If a string is passed to the `info` parameter, returns a string corresponding to the value of the key whose name matches the `info` parameter.

**Dictionary Keys:**
- `url`
- `id`
- `id_info`
- `username`
- `email`
- `email_verified`
- `first_name`
- `last_name`
- `origin`
- `profile_name`
- `created_at`

## Getting Stock Information
Contains information in regards to stocks.
### `robin_stocks.robinhood.stocks.find_instrument_data(query)`

Searches for stocks that contain the query keyword and returns the instrument data.

**Parameters:**
- `query` (str): The keyword to search for.

**Returns:**
- A list of dictionaries containing the instrument data for each stock that matches the query.

**Dictionary Keys:**
- `id`
- `url`
- `quote`
- `fundamentals`
- `splits`
- `state`
- `market`
- `simple_name`
- `name`
- `tradeable`
- `tradability`
- `symbol`
- `bloomberg_unique`
- `margin_initial_ratio`
- `maintenance_ratio`
- `country`
- `day_trade_ratio`
- `list_date`
- `min_tick_size`
- `type`
- `tradable_chain_id`
- `rhs_tradability`
- `fractional_tradability`
- `default_collar_fraction`

---
### `robin_stocks.robinhood.stocks.get_earnings(symbol, info=None)`

Returns the earnings for different financial quarters.

**Parameters:**
- `symbol` (str): The stock ticker.
- `info` (str, optional): Filters the results to get a specific value.

**Returns:**
- A list of dictionaries. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `symbol`
- `instrument`
- `year`
- `quarter`
- `eps`
- `report`
- `call`

---
### `robin_stocks.robinhood.stocks.get_events(symbol, info=None)`

Returns the events related to a stock that the user owns.

**Parameters:**
- `symbol` (str): The stock ticker.
- `info` (str, optional): Filters the results to get a specific value.

**Returns:**
- A list of dictionaries. If the `info` parameter is provided, the function will extract the value of the key that matches the `info` parameter. Otherwise, the whole dictionary is returned.

**Dictionary Keys:**
- `account`
- `cash_component`
- `chain_id`
- `created_at`
- `direction`
- `equity_components`
- `event_date`
- `id`
- `option`
- `position`
- `quantity`
- `state`
- `total_cash_amount`
- `type`
- `underlying_price`
- `updated_at`

---
### `robin_stocks.robinhood.stocks.get_fundamentals(inputSymbols, info=None)`

Takes any number of stock tickers and returns fundamental information about the stock, such as sector, company description, dividend yield, and market cap.

**Parameters:**
- `inputSymbols` (str or list): A single stock ticker or a list of stock tickers.
- `info` (str, optional): Filters the results to provide a list of values corresponding to the key that matches `info`.

**Returns:**
- A list of dictionaries containing key/value pairs for each ticker. If the `info` parameter is provided, a list of strings is returned where the strings are the values of the key that corresponds to `info`.

**Dictionary Keys:**
- `open`
- `high`
- `low`
- `volume`
- `average_volume_2_weeks`
- `average_volume`
- `high_52_weeks`
- `dividend_yield`
- `float`
- `low_52_weeks`
- `market_cap`
- `pb_ratio`
- `pe_ratio`
- `shares_outstanding`
- `description`
- `instrument`
- `ceo`
- `headquarters_city`
- `headquarters_state`
- `sector`
- `industry`
- `num_employees`
- `year_founded`
- `symbol`

---
### `robin_stocks.robinhood.stocks.get_instrument_by_url(url, info=None)`

Takes a single URL for the stock. The URL should be located at `https://api.robinhood.com/instruments/<id>` where `<id>` is the ID of the stock.

**Parameters:**
- `url` (str): The URL of the stock. Can be found in several locations, including in the dictionary returned from `get_instruments_by_symbols(inputSymbols, info=None)`.
- `info` (str, optional): Filters the results to provide a list of values corresponding to the key that matches `info`.

**Returns:**
- If the `info` parameter is left as `None`, returns a dictionary of key/value pairs for a specific URL. Otherwise, returns the string value of the key that corresponds to `info`.

**Dictionary Keys:**
- `id`
- `url`
- `quote`
- `fundamentals`
- `splits`
- `state`
- `market`
- `simple_name`
- `name`
- `tradeable`
- `tradability`
- `symbol`
- `bloomberg_unique`
- `margin_initial_ratio`
- `maintenance_ratio`
- `country`
- `day_trade_ratio`
- `list_date`
- `min_tick_size`
- `type`
- `tradable_chain_id`
- `rhs_tradability`
- `fractional_tradability`
- `default_collar_fraction`

---
### `robin_stocks.robinhood.stocks.get_instruments_by_symbols(inputSymbols, info=None)`

Takes any number of stock tickers and returns information held by the market, such as ticker name, Bloomberg ID, and listing date.

**Parameters:**
- `inputSymbols` (str or list): A single stock ticker or a list of stock tickers.
- `info` (str, optional): Filters the results to provide a list of values corresponding to the key that matches `info`.

**Returns:**
- If the `info` parameter is left as `None`, returns a list of dictionaries containing key/value pairs for each ticker. Otherwise, returns a list of strings where the strings are the values of the key that corresponds to `info`.

**Dictionary Keys:**
- `id`
- `url`
- `quote`
- `fundamentals`
- `splits`
- `state`
- `market`
- `simple_name`
- `name`
- `tradeable`
- `tradability`
- `symbol`
- `bloomberg_unique`
- `margin_initial_ratio`
- `maintenance_ratio`
- `country`
- `day_trade_ratio`
- `list_date`
- `min_tick_size`
- `type`
- `tradable_chain_id`
- `rhs_tradability`
- `fractional_tradability`
- `default_collar_fraction`

---
### `robin_stocks.robinhood.stocks.get_latest_price(inputSymbols, priceType=None, includeExtendedHours=True)`

Takes any number of stock tickers and returns the latest price of each one as a string.

**Parameters:**
- `inputSymbols` (str or list): A single stock ticker or a list of stock tickers.
- `priceType` (str, optional): Can either be `'ask_price'` or `'bid_price'`. If this parameter is set, then `includeExtendedHours` is ignored.
- `includeExtendedHours` (bool): If `True`, returns the extended hours price if available; if `False`, returns only regular hours price.

**Returns:**
- A list of prices as strings.

---
### `robin_stocks.robinhood.stocks.get_name_by_symbol(symbol)`

Returns the name of a stock from the stock ticker.

**Parameters:**
- `symbol` (str): The ticker of the stock.

**Returns:**
- A string containing the simple name of the stock. If the simple name does not exist, returns the full name.

---
### `robin_stocks.robinhood.stocks.get_name_by_url(url)`

Returns the name of a stock from the instrument URL. The URL should be located at `https://api.robinhood.com/instruments/<id>` where `<id>` is the ID of the stock.

**Parameters:**
- `url` (str): The URL of the stock.

**Returns:**
- A string containing the simple name of the stock. If the simple name does not exist, returns the full name.

---
### `robin_stocks.robinhood.stocks.get_news(symbol, info=None)`

Returns news stories for a stock.

**Parameters:**
- `symbol` (str): The stock ticker.
- `info` (str, optional): Filters the results to get a specific value.

**Returns:**
- A list of dictionaries. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `api_source`
- `author`
- `num_clicks`
- `preview_image_url`
- `published_at`
- `relay_url`
- `source`
- `summary`
- `title`
- `updated_at`
- `url`
- `uuid`
- `related_instruments`
- `preview_text`
- `currency_id`

---
### `robin_stocks.robinhood.stocks.get_pricebook_by_id(stock_id, info=None)`

Represents Level II Market Data provided for Gold subscribers.

**Parameters:**
- `stock_id` (str): The Robinhood stock ID.
- `info` (str, optional): Filters the results to get a specific value. Possible options are `url`, `instrument`, `execution_date`, `divisor`, and `multiplier`.

**Returns:**
- A dictionary of asks and bids.

---
### `robin_stocks.robinhood.stocks.get_pricebook_by_symbol(symbol, info=None)`



Represents Level II Market Data provided for Gold subscribers.

**Parameters:**
- `symbol` (str): The stock symbol.
- `info` (str, optional): Filters the results to get a specific value. Possible options are `url`, `instrument`, `execution_date`, `divisor`, and `multiplier`.

**Returns:**
- A dictionary of asks and bids.

---
### `robin_stocks.robinhood.stocks.get_quotes(inputSymbols, info=None)`

Takes any number of stock tickers and returns information pertaining to their price.

**Parameters:**
- `inputSymbols` (str or list): A single stock ticker or a list of stock tickers.
- `info` (str, optional): Filters the results to provide a list of values corresponding to the key that matches `info`.

**Returns:**
- If the `info` parameter is left as `None`, returns a list of dictionaries containing key/value pairs for each ticker. Otherwise, returns a list of strings where the strings are the values of the key that corresponds to `info`.

**Dictionary Keys:**
- `ask_price`
- `ask_size`
- `bid_price`
- `bid_size`
- `last_trade_price`
- `last_extended_hours_trade_price`
- `previous_close`
- `adjusted_previous_close`
- `previous_close_date`
- `symbol`
- `trading_halted`
- `has_traded`
- `last_trade_price_source`
- `updated_at`
- `instrument`

---
### `robin_stocks.robinhood.stocks.get_ratings(symbol, info=None)`

Returns the ratings for a stock, including the number of buy, hold, and sell ratings.

**Parameters:**
- `symbol` (str): The stock ticker.
- `info` (str, optional): Filters the results to contain a dictionary of values corresponding to the key that matches `info`. Possible values are `summary`, `ratings`, and `instrument_id`.

**Returns:**
- A dictionary containing ratings. If the `info` parameter is provided, the dictionary will contain values corresponding to the key that matches `info`.

**Dictionary Keys:**
- `summary` (dict)
- `ratings` (list of dicts)
- `instrument_id` (str)
- `ratings_published_at` (str)

---
### `robin_stocks.robinhood.stocks.get_splits(symbol, info=None)`

Returns the date, divisor, and multiplier for when a stock split occurred.

**Parameters:**
- `symbol` (str): The stock ticker.
- `info` (str, optional): Filters the results to get a specific value. Possible options are `url`, `instrument`, `execution_date`, `divisor`, and `multiplier`.

**Returns:**
- A list of dictionaries. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `url`
- `instrument`
- `execution_date`
- `multiplier`
- `divisor`

---
### `robin_stocks.robinhood.stocks.get_stock_historicals(inputSymbols, interval='hour', span='week', bounds='regular', info=None)`

Represents the historical data for a stock.

**Parameters:**
- `inputSymbols` (str or list): A single stock ticker or a list of stock tickers.
- `interval` (str, optional): Interval to retrieve data for. Possible values are `'5minute'`, `'10minute'`, `'hour'`, `'day'`, `'week'`. Default is `'hour'`.
- `span` (str, optional): Sets the range of the data to be either `'day'`, `'week'`, `'month'`, `'3month'`, `'year'`, or `'5year'`. Default is `'week'`.
- `bounds` (str, optional): Represents whether the graph will include extended trading hours or just regular trading hours. Possible values are `'regular'`, `'trading'`, and `'extended'`. Default is `'regular'`.
- `info` (str, optional): Filters the results to provide a list of values corresponding to the key that matches `info`.

**Returns:**
- A list of dictionaries where each dictionary is for a different time. If multiple stocks are provided, the historical data is listed one after another.

**Dictionary Keys:**
- `begins_at`
- `open_price`
- `close_price`
- `high_price`
- `low_price`
- `volume`
- `session`
- `interpolated`
- `symbol`

---
### `robin_stocks.robinhood.stocks.get_stock_quote_by_id(stock_id, info=None)`

Represents basic stock quote information.

**Parameters:**
- `stock_id` (str): The Robinhood stock ID.
- `info` (str, optional): Filters the results to get a specific value. Possible options are `url`, `instrument`, `execution_date`, `divisor`, and `multiplier`.

**Returns:**
- A dictionary containing basic stock quote information. If the `info` parameter is provided, the dictionary will contain values corresponding to the key that matches `info`.

**Dictionary Keys:**
- `ask_price`
- `ask_size`
- `bid_price`
- `bid_size`
- `last_trade_price`
- `last_extended_hours_trade_price`
- `previous_close`
- `adjusted_previous_close`
- `previous_close_date`
- `symbol`
- `trading_halted`
- `has_traded`
- `last_trade_price_source`
- `updated_at`
- `instrument`

---
### `robin_stocks.robinhood.stocks.get_stock_quote_by_symbol(symbol, info=None)`

Represents basic stock quote information.

**Parameters:**
- `symbol` (str): The stock ticker.
- `info` (str, optional): Filters the results to get a specific value. Possible options are `url`, `instrument`, `execution_date`, `divisor`, and `multiplier`.

**Returns:**
- A dictionary containing basic stock quote information. If the `info` parameter is provided, the dictionary will contain values corresponding to the key that matches `info`.

**Dictionary Keys:**
- `ask_price`
- `ask_size`
- `bid_price`
- `bid_size`
- `last_trade_price`
- `last_extended_hours_trade_price`
- `previous_close`
- `adjusted_previous_close`
- `previous_close_date`
- `symbol`
- `trading_halted`
- `has_traded`
- `last_trade_price_source`
- `updated_at`
- `instrument`

---
### `robin_stocks.robinhood.stocks.get_symbol_by_url(url)`

Returns the symbol of a stock from the instrument URL. The URL should be located at `https://api.robinhood.com/instruments/<id>` where `<id>` is the ID of the stock.

**Parameters:**
- `url` (str): The URL of the stock.

**Returns:**
- A string containing the ticker symbol of the stock.

## Getting Option Information
Contains functions for getting information about options.

### `robin_stocks.robinhood.options.find_options_by_expiration(inputSymbols, expirationDate, optionType=None, info=None)`

Returns a list of all the option orders that match the search parameters.

**Parameters:**
- `inputSymbols` (str): The ticker of either a single stock or a list of stocks.
- `expirationDate` (str): The expiration date in the format `YYYY-MM-DD`.
- `optionType` (str, optional): Can be either `'call'` or `'put'` or left blank to get both.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for all options of the stock that match the search parameters. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.find_options_by_expiration_and_strike(inputSymbols, expirationDate, strikePrice, optionType=None, info=None)`

Returns a list of all the option orders that match the search parameters.

**Parameters:**
- `inputSymbols` (str): The ticker of either a single stock or a list of stocks.
- `expirationDate` (str): The expiration date in the format `YYYY-MM-DD`.
- `strikePrice` (str): The strike price to filter for.
- `optionType` (str, optional): Can be either `'call'` or `'put'` or left blank to get both.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for all options of the stock that match the search parameters. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.find_options_by_specific_profitability(inputSymbols, expirationDate=None, strikePrice=None, optionType=None, typeProfit='chance_of_profit_short', profitFloor=0.0, profitCeiling=1.0, info=None)`

Returns a list of option market data for several stock tickers that match a range of profitability.

**Parameters:**
- `inputSymbols` (str or list): A single stock ticker or a list of stock tickers.
- `expirationDate` (str, optional): The expiration date in the format `YYYY-MM-DD`. Leave as `None` to get all available dates.
- `strikePrice` (str, optional): The price of the option. Leave as `None` to get all available strike prices.
- `optionType` (str, optional): Can be either `'call'` or `'put'` or left blank to get both.
- `typeProfit` (str): Will either be `'chance_of_profit_short'` or `'chance_of_profit_long'`.
- `profitFloor` (int): The lower percentage on a scale of 0 to 1.
- `profitCeiling` (int): The higher percentage on a scale of 0 to 1.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for all stock option market data. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.find_options_by_strike(inputSymbols, strikePrice, optionType=None, info=None)`

Returns a list of all the option orders that match the search parameters.

**Parameters:**
- `inputSymbols` (str): The ticker of either a single stock or a list of stocks.
- `strikePrice` (str): The strike price to filter for.
- `optionType` (str, optional): Can be either `'call'` or `'put'` or left blank to get both.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for all options of the stock that match the search parameters. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.find_tradable_options(symbol, expirationDate=None, strikePrice=None, optionType=None, info=None)`

Returns a list of all available options for a stock.

**Parameters:**
- `symbol` (str): The ticker of the stock.
- `expirationDate` (str, optional): The expiration date in the format `YYYY-MM-DD`.
- `strikePrice` (str, optional): The strike price of the option.
- `optionType` (str, optional): Can be either `'call'` or `'put'` or left blank to get both.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for all calls of the stock. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.get_aggregate_open_positions(info=None, account_number=None)`

Collapses all open option positions for a stock into a single dictionary.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.
- `account_number` (str, optional): The Robinhood account number.

**Returns:**
- A list of dictionaries of key/value pairs for each order. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.get_aggregate_positions(info=None, account_number=None)`

Collapses all option orders for a stock into a single dictionary.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.
- `account_number` (str, optional): The Robinhood account number.

**Returns:**
- A list of dictionaries of key/value pairs for each order. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.get_all_option_positions(info=None, account_number=None)`

Returns all option positions ever held for the account.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.
- `account_number` (str, optional): The Robinhood account number.

**Returns:**
- A list of dictionaries of key/value pairs for each option. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.get_chains(symbol, info=None)`

Returns the chain information of an option.

**Parameters:**
- `symbol` (str): The ticker of the stock.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary of key/value pairs for the option. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.get_market_options(info=None)`

Returns a list of all options.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each option. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.get_open_option_positions(account_number=None, info=None)`

Returns all open option positions for the account.

**Parameters:**
- `account_number` (str, optional): The Robinhood account number.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each option. If `info` is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.options.get_option_historicals(symbol, expirationDate, strikePrice, optionType, interval='hour', span='week', bounds='regular', info=None)`

Returns the data that is used to make the graphs.

**Parameters:**
- `symbol` (str): The ticker of the stock.
- `expirationDate` (str): The expiration date in the format `YYYY-MM-DD`.
- `strikePrice` (str): The price of the option.
- `optionType` (str): Can be either `'call'` or `'put'`.
- `interval` (str, optional): Interval to retrieve data for. Possible values are `'5minute'`, `'10minute'`, `'hour'`, `'day'`, `'week'`. Default is `'hour'`.
- `span` (str, optional): Sets the range of the data to be either `'day'`, `'week'`, `'year'`, or `'5year'`. Default is `'week'`.
- `bounds` (str, optional): Represents whether the graph will include extended trading hours or just regular trading hours. Possible values are `'regular'`, `'trading'`, and `'extended'`. Default is `'regular'`.
- `info` (str, optional): Will filter the results to provide a

 list of values corresponding to the key that matches `info`.

**Returns:**
- A list that contains a list for each symbol. Each list contains a dictionary where each dictionary is for a different time.

---
### `robin_stocks.robinhood.options.get_option_instrument_data(symbol, expirationDate, strikePrice, optionType, info=None)`

Returns the option instrument data for the stock option.

**Parameters:**
- `symbol` (str): The ticker of the stock.
- `expirationDate` (str): The expiration date in the format `YYYY-MM-DD`.
- `strikePrice` (str): The price of the option.
- `optionType` (str): Can be either `'call'` or `'put'`.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary of key/value pairs for the stock. If `info` is provided, the value of the key that matches `info` is extracted.

---
### `robin_stocks.robinhood.options.get_option_instrument_data_by_id(id, info=None)`

Returns the option instrument information.

**Parameters:**
- `id` (str): The ID of the stock.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary of key/value pairs for the stock. If `info` is provided, the value of the key that matches `info` is extracted.

---
### `robin_stocks.robinhood.options.get_option_market_data(inputSymbols, expirationDate, strikePrice, optionType, info=None)`

Returns the option market data for the stock option, including the greeks, open interest, chance of profit, and adjusted mark price.

**Parameters:**
- `inputSymbols` (str): The ticker of the stock.
- `expirationDate` (str): The expiration date in the format `YYYY-MM-DD`.
- `strikePrice` (str): The price of the option.
- `optionType` (str): Can be either `'call'` or `'put'`.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary of key/value pairs for the stock. If `info` is provided, the value of the key that matches `info` is extracted.

---
### `robin_stocks.robinhood.options.get_option_market_data_by_id(id, info=None)`

Returns the option market data for a stock, including the greeks, open interest, chance of profit, and adjusted mark price.

**Parameters:**
- `id` (str): The ID of the stock.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary of key/value pairs for the stock. If `info` is provided, the value of the key that matches `info` is extracted.

---
### `robin_stocks.robinhood.options.spinning_cursor()`

This is a generator function to yield a character.

---
### `robin_stocks.robinhood.options.write_spinner()`

Function to create a spinning cursor to indicate that the code is working on getting market data.

## Get Market Information
Contains functions for getting market level data.

### `robin_stocks.robinhood.markets.get_all_stocks_from_market_tag(tag, info=None)`

Returns all the stock quote information that matches a tag category.

**Parameters:**
- `tag` (str): The category to filter for. Examples include `'biopharmaceutical'`, `'upcoming-earnings'`, `'most-popular-under-25'`, and `'technology'`.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries containing key/value pairs for each mover. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `ask_price`
- `ask_size`
- `bid_price`
- `bid_size`
- `last_trade_price`
- `last_extended_hours_trade_price`
- `previous_close`
- `adjusted_previous_close`
- `previous_close_date`
- `symbol`
- `trading_halted`
- `has_traded`
- `last_trade_price_source`
- `updated_at`
- `instrument`

---
### `robin_stocks.robinhood.markets.get_currency_pairs(info=None)`

Returns currency pairs.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries containing key/value pairs for each currency pair. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `asset_currency`
- `display_only`
- `id`
- `max_order_size`
- `min_order_size`
- `min_order_price_increment`
- `min_order_quantity_increment`
- `name`
- `quote_currency`
- `symbol`
- `tradability`

---
### `robin_stocks.robinhood.markets.get_market_hours(market, date, info=None)`

Returns the opening and closing hours of a specific market on a specific date. Also indicates if the market is open on that date. Can be used with past or future dates.

**Parameters:**
- `market` (str): The `'mic'` value for the market. Can be found using `get_markets()`.
- `date` (str): The date for which you want information in the format `YYYY-MM-DD`.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary containing key/value pairs for the specific market. If the `info` parameter is provided, the string value for the corresponding key is returned.

**Dictionary Keys:**
- `date`
- `is_open`
- `opens_at`
- `closes_at`
- `extended_opens_at`
- `extended_closes_at`
- `previous_open_hours`
- `next_open_hours`

---
### `robin_stocks.robinhood.markets.get_market_next_open_hours(market, info=None)`

Returns the opening and closing hours for the next open trading day after today. Also indicates if the market is open on that date.

**Parameters:**
- `market` (str): The `'mic'` value for the market. Can be found using `get_markets()`.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary containing key/value pairs for the specific market. If the `info` parameter is provided, the string value for the corresponding key is returned.

**Dictionary Keys:**
- `date`
- `is_open`
- `opens_at`
- `closes_at`
- `extended_opens_at`
- `extended_closes_at`
- `previous_open_hours`
- `next_open_hours`

---
### `robin_stocks.robinhood.markets.get_market_next_open_hours_after_date(market, date, info=None)`

Returns the opening and closing hours for the next open trading day after a specified date. Also indicates if the market is open on that date.

**Parameters:**
- `market` (str): The `'mic'` value for the market. Can be found using `get_markets()`.
- `date` (str): The date you want to find the next available trading day after, in the format `YYYY-MM-DD`.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary containing key/value pairs for the specific market. If the `info` parameter is provided, the string value for the corresponding key is returned.

**Dictionary Keys:**
- `date`
- `is_open`
- `opens_at`
- `closes_at`
- `extended_opens_at`
- `extended_closes_at`
- `previous_open_hours`
- `next_open_hours`

---
### `robin_stocks.robinhood.markets.get_market_today_hours(market, info=None)`

Returns the opening and closing hours of a specific market for today. Also indicates if the market is open on that date.

**Parameters:**
- `market` (str): The `'mic'` value for the market. Can be found using `get_markets()`.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary containing key/value pairs for the specific market. If the `info` parameter is provided, the string value for the corresponding key is returned.

**Dictionary Keys:**
- `date`
- `is_open`
- `opens_at`
- `closes_at`
- `extended_opens_at`
- `extended_closes_at`
- `previous_open_hours`
- `next_open_hours`

---
### `robin_stocks.robinhood.markets.get_markets(info=None)`

Returns a list of available markets.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries containing key/value pairs for each market. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `url`
- `todays_hours`
- `mic`
- `operating_mic`
- `acronym`
- `name`
- `city`
- `country`
- `timezone`
- `website`

---
### `robin_stocks.robinhood.markets.get_top_100(info=None)`

Returns a list of the Top 100 stocks on Robinhood.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries containing key/value pairs for each mover. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `ask_price`
- `ask_size`
- `bid_price`
- `bid_size`
- `last_trade_price`
- `last_extended_hours_trade_price`
- `previous_close`
- `adjusted_previous_close`
- `previous_close_date`
- `symbol`
- `trading_halted`
- `has_traded`
- `last_trade_price_source`
- `updated_at`
- `instrument`

---
### `robin_stocks.robinhood.markets.get_top_movers(info=None)`

Returns a list of the Top 20 movers on Robinhood.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries containing key/value pairs for each mover. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `ask_price`
- `ask_size`
- `bid_price`
- `bid_size`
- `last_trade_price`
- `last_extended_hours_trade_price`
- `previous_close`
- `adjusted_previous_close`
- `previous_close_date`
- `symbol`
- `trading_halted`
- `has_traded`
- `last_trade_price_source`
- `updated_at`
- `instrument`

---
### `robin_stocks.robinhood.markets.get_top_movers_sp500(direction, info=None)`

Returns a list of the top S&P 500 movers up or down for the day.

**Parameters:**
- `direction` (str): The direction of movement, either `'up'` or `'down'`.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries containing key/value pairs for each mover. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `instrument_url`
- `symbol`
- `updated_at`
- `price_movement`
- `description`

## Getting Positions and Account Information
Contains functions for getting information related to the user account.

### `robin_stocks.robinhood.account.build_holdings(with_dividends=False)`

Builds a dictionary of important information regarding the stocks and positions owned by the user.

**Parameters:**
- `with_dividends` (bool): True if you want to include dividend information.

**Returns:**
- A dictionary where the keys are the stock tickers, and the value is another dictionary that includes the stock price, quantity held, equity, percent change, equity change, type, name, ID, PE ratio, percentage of portfolio, and average buy price.

---
### `robin_stocks.robinhood.account.build_user_profile(account_number=None)`

Builds a dictionary of important information regarding the user account.

**Returns:**
- A dictionary that has total equity, extended hours equity, cash, and dividend total.

---
### `robin_stocks.robinhood.account.delete_symbols_from_watchlist(inputSymbols, name='My First List')`

Deletes multiple stock tickers from a watchlist.

**Parameters:**
- `inputSymbols` (str or list): May be a single stock ticker or a list of stock tickers.
- `name` (str, optional): The name of the watchlist to delete data from.

**Returns:**
- Result of the delete request.

---
### `robin_stocks.robinhood.account.deposit_funds_to_robinhood_account(ach_relationship, amount, info=None)`

Submits a post request to deposit a certain amount of money from a bank account to Robinhood.

**Parameters:**
- `ach_relationship` (str): The URL of the bank account you want to deposit the money from.
- `amount` (float): The amount of money you wish to deposit.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for the transaction.

---
### `robin_stocks.robinhood.account.download_all_documents(doctype=None, dirpath=None)`

Downloads all the documents associated with an account and saves them as a PDF. If no name is given, the document is saved as a combination of the creation date, type, and ID. If no directory is given, the document is saved in the root directory of the code.

**Parameters:**
- `doctype` (str, optional): The type of document to download, such as an account statement.
- `dirpath` (str, optional): The directory where to save the documents.

**Returns:**
- The list of documents from `get_documents(info=None)`.

---
### `robin_stocks.robinhood.account.download_document(url, name=None, dirpath=None)`

Downloads a document and saves as it as a PDF. If no name is given, the document is saved as the name that Robinhood has for the document. If no directory is given, the document is saved in the root directory of the code.

**Parameters:**
- `url` (str): The URL of the document. Can be found by using `get_documents(info='download_url')`.
- `name` (str, optional): The name to save the document as.
- `dirpath` (str, optional): The directory where to save the document.

**Returns:**
- The data from the GET request.

---
### `robin_stocks.robinhood.account.get_all_positions(info=None)`

Returns a list containing every position ever traded.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries containing key/value pairs for each ticker. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `url`
- `instrument`
- `account`
- `account_number`
- `average_buy_price`
- `pending_average_buy_price`
- `quantity`
- `intraday_average_buy_price`
- `intraday_quantity`
- `shares_held_for_buys`
- `shares_held_for_sells`
- `shares_held_for_stock_grants`
- `shares_held_for_options_collateral`
- `shares_held_for_options_events`
- `shares_pending_from_options_events`
- `updated_at`
- `created_at`

---
### `robin_stocks.robinhood.account.get_all_watchlists(info=None)`

Returns a list of all watchlists that have been created. Everyone has a 'My First List' watchlist.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of the watchlists. Keywords are `url`, `user`, and `name`.

---
### `robin_stocks.robinhood.account.get_bank_account_info(id, info=None)`

Returns a single dictionary of bank information.

**Parameters:**
- `id` (str): The bank ID.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A dictionary of key/value pairs for the bank. If the `info` parameter is provided, the value of the key that matches `info` is extracted.

---
### `robin_stocks.robinhood.account.get_bank_transfers(direction=None, info=None)`

Returns all bank transfers made for the account.

**Parameters:**
- `direction` (str, optional): Possible values are 'received'. If left blank, the function will return all withdrawals and deposits that are initiated from Robinhood. If the value is 'received', the function will return transfers initiated from your bank rather than Robinhood.
- `info` (str, optional): Will filter the results to get a specific value. 'Direction' gives if it was a deposit or withdrawal.

**Returns:**
- A list of dictionaries containing key/value pairs for each transfer. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.get_card_transactions(cardType=None, info=None)`

Returns all debit card transactions made on the account.

**Parameters:**
- `cardType` (str, optional): Will filter the card transaction types. Can be 'pending' or 'settled'.
- `info` (str, optional): Will filter the results to get a specific value. 'Direction' gives if it was a debit or credit.

**Returns:**
- A list of dictionaries containing key/value pairs for each transaction. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.get_day_trades(info=None)`

Returns recent day trades.

Parameters:
- `info` (str, optional): Will filter the results to get a specific value.

Returns:
- A list of dictionaries containing key/value pairs for each day trade. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.
---
### `robin_stocks.robinhood.account.get_dividends(info=None)`

Returns a list of dividend transactions that include information such as the percentage rate, amount, shares of held stock, and date paid.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each dividend payment. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `id`
- `url`
- `account`
- `instrument`
- `amount`
- `rate`
- `position`
- `withholding`
- `record_date`
- `payable_date`
- `paid_at`
- `state`
- `nra_withholding`
- `drip_enabled`

---
### `robin_stocks.robinhood.account.get_dividends_by_instrument(instrument, dividend_data)`

Returns a dictionary with three fields when given the instrument value for a stock.

**Parameters:**
- `instrument` (str): The instrument to get the dividend data.
- `dividend_data` (list): The information returned by `get_dividends()`.

**Returns:**
- `dividend_rate`: The rate paid for a single share of a specified stock.
- `total_dividend`: The total dividend paid based on total shares for a specified stock.
- `amount_paid_to_date`: Total amount earned by the account for this particular stock.

---
### `robin_stocks.robinhood.account.get_documents(info=None)`

Returns a list of documents that have been released by Robinhood to the account.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each document. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.get_interest_payments(info=None)`

Returns a list of interest payments.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each interest payment. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.get_latest_notification()`

Returns the time of the latest notification.

**Returns:**
- A dictionary of key/value pairs. But there is only one key, `last_viewed_at`.

---
### `robin_stocks.robinhood.account.get_linked_bank_accounts(info=None)`

Returns all linked bank accounts.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each bank.

---
### `robin_stocks.robinhood.account.get_margin_calls(symbol=None)`

Returns either all margin calls or margin calls for a specific stock.

**Parameters:**
- `symbol` (str, optional): Will determine which stock to get margin calls for.

**Returns:**
- A list of dictionaries of key/value pairs for each margin call.

---
### `robin_stocks.robinhood.account.get_margin_interest(info=None)`

Returns a list of margin interest.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each interest. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.get_notifications(info=None)`

Returns a list of notifications.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each notification. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.get_open_stock_positions(account_number=None, info=None)`

Returns a list of stocks that are currently held.

**Parameters:**
- `account_number` (str, optional): The Robinhood account number.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each ticker. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `url`
- `instrument`
- `account`
- `account_number`
- `average_buy_price`
- `pending_average_buy_price`
- `quantity`
- `intraday_average_buy_price`
- `intraday_quantity`
- `shares_held_for_buys`
- `shares_held_for_sells`
- `shares_held_for_stock_grants`
- `shares_held_for_options_collateral`
- `shares_held_for_options_events`
- `shares_pending_from_options_events`
- `updated_at`
- `created_at`

---
### `robin_stocks.robinhood.account.get_referrals(info=None)`

Returns a list of referrals.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each referral. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.get_stock_loan_payments(info=None)`

Returns a list of loan payments.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each payment. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.get_subscription_fees(info=None)`

Returns a list of subscription fees.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each fee. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.get_total_divi()`

Returns a float number representing the total amount of dividends paid to the account.

**Returns:**
- Total dollar amount of dividends paid to the account as a 2 precision float.

---
### `robin_stocks.robinhood.account.get_watchlist_by_name(name='My First List', info=None)`

Returns a list of information related to the stocks in a single watchlist.

**Parameters:**
- `name` (str, optional): The name of the watchlist to get data from.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries that contain the instrument URLs and a URL that references itself.

---
### `robin_stocks.robinhood.account.get_wire_transfers(info=None)`

Returns a list of wire transfers.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for each wire transfer. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.account.load_phoenix_account(info=None)`

Returns unified information about your account.

**Parameters:**
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs. If the `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

**Dictionary Keys:**
- `account_buying_power`
- `cash_available_from_instant_deposits`
- `cash_held_for_currency_orders`
- `cash_held_for_dividends`
- `cash_held_for_equity_orders`
- `cash_held_for_options_collateral`
- `cash_held_for_orders`
- `crypto`
- `crypto_buying_power`
- `equities`
- `extended_hours_portfolio_equity`
- `instant_allocated`
- `levered_amount`
- `near_margin_call`
- `options_buying_power`
- `portfolio_equity`
- `portfolio_previous_close`
- `previous_close`
- `regular_hours_portfolio_equity`
- `total_equity`
- `total_extended_hours_equity`
- `total_extended_hours_market_value`
- `total_market_value`
- `total_regular_hours_equity`
- `total_regular_hours_market_value`
- `uninvested_cash`
- `withdrawable_cash`

---
### `robin_stocks.robinhood.account.post_symbols_to_watchlist(inputSymbols, name='My First List')`

Posts multiple stock tickers to a watchlist.

**Parameters:**
- `inputSymbols` (str or list): May be a single stock ticker or a list of stock tickers.
- `name` (str, optional): The name of the watchlist to post data to.

**Returns:**
- Result of the post request.

---
### `robin_stocks.robinhood.account.unlink_bank_account(id)`

Unlinks a bank account.

**Parameters:**
- `id` (str): The bank ID.

**Returns:**
- Information returned from post request.

---
### `robin_stocks.robinhood.account.withdrawl_funds_to_bank_account(ach_relationship, amount, info=None)`

Submits a post request to withdraw a certain amount of money to a bank account.

**Parameters:**
- `ach_relationship` (str): The URL of the bank account you want to withdraw the money to.
- `amount` (float): The amount of money you wish to withdraw.
- `info` (str, optional): Will filter the results to get a specific value.

**Returns:**
- A list of dictionaries of key/value pairs for the transaction.

## Placing and Cancelling Orders
Contains all functions for placing orders for stocks, options, and crypto.

### `robin_stocks.robinhood.orders.cancel_all_crypto_orders()`
Cancels all crypto orders.

**Returns:**
- Returns the order information for the orders that were cancelled.

---
### `robin_stocks.robinhood.orders.cancel_all_option_orders()`
Cancels all option orders.

**Returns:**
- Returns the order information for the orders that were cancelled.

---
### `robin_stocks.robinhood.orders.cancel_all_stock_orders()`
Cancels all stock orders.

**Returns:**
- The list of orders that were cancelled.

---
### `robin_stocks.robinhood.orders.cancel_crypto_order(orderID)`
Cancels a specific crypto order.

**Parameters:**
- `orderID` (_str_): The ID associated with the order. Can be found using `get_all_crypto_orders(info=None)`.

**Returns:**
- Returns the order information for the order that was cancelled.

---
### `robin_stocks.robinhood.orders.cancel_option_order(orderID)`
Cancels a specific option order.

**Parameters:**
- `orderID` (_str_): The ID associated with the order. Can be found using `get_all_option_orders(info=None)`.

**Returns:**
- Returns the order information for the order that was cancelled.

---
### `robin_stocks.robinhood.orders.cancel_stock_order(orderID)`
Cancels a specific stock order.

**Parameters:**
- `orderID` (_str_): The ID associated with the order. Can be found using `get_all_stock_orders(info=None)`.

**Returns:**
- Returns the order information for the order that was cancelled.

---
### `robin_stocks.robinhood.orders.find_stock_orders(**arguments)`
Returns a list of orders that match the keyword parameters.

**Parameters:**
- `arguments` (_str_): Variable length of keyword arguments. Example: `find_orders(symbol='FB', cancel=None, quantity=1)`

**Returns:**
- Returns a list of orders.

---
### `robin_stocks.robinhood.orders.get_all_crypto_orders(info=None)`
Returns a list of all the crypto orders that have been processed for the account.

**Parameters:**
- `info` (_Optional_[_str_]): Will filter the results to get a specific value.

**Returns:**
- Returns a list of dictionaries of key/value pairs for each crypto order. If `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.orders.get_all_open_crypto_orders(info=None)`
Returns a list of all the open crypto orders that have been processed for the account.

**Parameters:**
- `info` (_Optional_[_str_]): Will filter the results to get a specific value.

**Returns:**
- Returns a list of dictionaries of key/value pairs for each open crypto order. If `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.orders.get_all_open_option_orders(info=None, account_number=None)`
Returns a list of all the open option orders.

**Parameters:**
- `info` (_Optional_[_str_]): Will filter the results to get a specific value.
- `account_number` (_Optional_[_str_]): The account number.

**Returns:**
- Returns a list of dictionaries of key/value pairs for each open option order. If `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.orders.get_all_open_stock_orders(info=None, account_number=None)`
Returns a list of all the open stock orders.

**Parameters:**
- `info` (_Optional\[str]_): Will filter the results to get a specific value.
- `account_number` (_Optional\[str]_): The account number.

**Returns:**
- Returns a list of dictionaries of key/value pairs for each open stock order. If `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.orders.get_all_option_orders(info=None)`
Returns a list of all the option orders that have been processed for the account.

**Parameters:**
- `info` (_Optional\[str]_): Will filter the results to get a specific value.

**Returns:**
- Returns a list of dictionaries of key/value pairs for each option order. If `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.orders.get_all_stock_orders(info=None)`
Returns a list of all the stock orders that have been processed for the account.

**Parameters:**
- `info` (_Optional\[str]_): Will filter the results to get a specific value.

**Returns:**
- Returns a list of dictionaries of key/value pairs for each stock order. If `info` parameter is provided, a list of strings is returned where the strings are the value of the key that matches `info`.

---
### `robin_stocks.robinhood.orders.get_crypto_order_info(order_id)`
Returns the information for a single crypto order.

**Parameters:**
- `order_id` (_str_): The ID associated with the crypto order.

**Returns:**
- Returns a list of dictionaries of key/value pairs for the order.

---
### `robin_stocks.robinhood.orders.get_option_order_info(order_id)`
Returns the information for a single option order.

**Parameters:**
- `order_id` (_str_): The ID associated with the option order.

**Returns:**
- Returns a list of dictionaries of key/value pairs for the order.

---
### `robin_stocks.robinhood.orders.get_stock_order_info(orderID)`
Returns the information for a single stock order.

**Parameters:**
- `orderID` (_str_): The ID associated with the stock order. Can be found using `get_all_orders(info=None)`.

**Returns:**
- Returns a list of dictionaries of key/value pairs for the order.
---
### `robin_stocks.robinhood.orders.order(symbol, quantity, side, limitPrice=None, stopPrice=None, account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True, market_hours='regular_hours')`

A generic order function.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to sell.
- `quantity` (int): The number of stocks to sell.
- `side` (str): Either 'buy' or 'sell'.
- `limitPrice` (float, optional): The price to trigger the market order.
- `stopPrice` (float, optional): The price to trigger the limit or market order.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the purchase or selling of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_crypto_by_price(symbol, amountInDollars, timeInForce='gtc', jsonify=True)`

Submits a market order for a crypto by specifying the amount in dollars that you want to trade. Good for share fractions up to 8 decimal places.

**Parameters:**
- `symbol` (str): The crypto ticker of the crypto to trade.
- `amountInDollars` (float): The amount in dollars of the crypto you want to buy.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the buying of crypto, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_crypto_by_quantity(symbol, quantity, timeInForce='gtc', jsonify=True)`

Submits a market order for a crypto by specifying the decimal amount of shares to buy. Good for share fractions up to 8 decimal places.

**Parameters:**
- `symbol` (str): The crypto ticker of the crypto to trade.
- `quantity` (float): The decimal amount of shares to buy.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the buying of crypto, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_crypto_limit(symbol, quantity, limitPrice, timeInForce='gtc', jsonify=True)`

Submits a limit order for a crypto by specifying the decimal amount of shares to buy. Good for share fractions up to 8 decimal places.

**Parameters:**
- `symbol` (str): The crypto ticker of the crypto to trade.
- `quantity` (float): The decimal amount of shares to buy.
- `limitPrice` (float): The limit price to set for the crypto.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the buying of crypto, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_crypto_limit_by_price(symbol, amountInDollars, limitPrice, timeInForce='gtc', jsonify=True)`

Submits a limit order for a crypto by specifying the decimal price to buy. Good for share fractions up to 8 decimal places.

**Parameters:**
- `symbol` (str): The crypto ticker of the crypto to trade.
- `amountInDollars` (float): The amount in dollars of the crypto you want to buy.
- `limitPrice` (float): The limit price to set for the crypto.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the buying of crypto, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_fractional_by_price(symbol, amountInDollars, account_number=None, timeInForce='gfd', extendedHours=False, jsonify=True, market_hours='regular_hours')`

Submits a market order to be executed immediately for fractional shares by specifying the amount in dollars that you want to trade. Good for share fractions up to 6 decimal places. Robinhood does not currently support placing limit, stop, or stop loss orders for fractional trades.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to purchase.
- `amountInDollars` (float): The amount in dollars of the fractional shares you want to buy.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the purchase of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_fractional_by_quantity(symbol, quantity, account_number=None, timeInForce='gfd', extendedHours=False, jsonify=True)`

Submits a market order to be executed immediately for fractional shares by specifying the amount that you want to trade. Good for share fractions up to 6 decimal places. Robinhood does not currently support placing limit, stop, or stop loss orders for fractional trades.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to purchase.
- `quantity` (float): The amount of the fractional shares you want to buy.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the purchase of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_limit(symbol, quantity, limitPrice, account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a limit order to be executed once a certain price is reached.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to purchase.
- `quantity` (int): The number of stocks to buy.
- `limitPrice` (float): The price to trigger the buy order.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the purchase of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_market(symbol, quantity, account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a market order to be executed immediately.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to purchase.
- `quantity` (int): The number of stocks to buy.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the purchase of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_option_limit(positionEffect, creditOrDebit, price, symbol, quantity, expirationDate, strike, optionType='both', account_number=None, timeInForce='gtc', jsonify=True)`

Submits a limit order for an option. i.e. place a long call or a long put.

**Parameters:**
- `positionEffect` (str): Either 'open' for a buy to open effect or 'close' for a buy to close effect.
- `creditOrDebit` (str): Either 'debit' or 'credit'.
- `price` (float): The limit price to trigger a buy of the option.
- `symbol` (str): The stock ticker of the stock to trade.
- `quantity` (int): The number of options to buy.
- `expirationDate` (str): The expiration date of the option in 'YYYY-MM-DD' format.
- `strike` (float): The strike price of the option.
- `optionType` (str): This should be 'call' or 'put'.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day. 'ioc' = immediate or cancel. 'opg' = execute at opening.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the buying of options, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_option_stop_limit(positionEffect, creditOrDebit, limitPrice, stopPrice, symbol, quantity, expirationDate, strike, optionType='both', account_number=None, timeInForce='gtc', jsonify=True)`

Submits a stop order to be turned into a limit order once a certain stop price is reached.

**Parameters:**
- `positionEffect` (str): Either 'open' for a buy to open effect or 'close' for a buy to close effect.
- `creditOrDebit` (str): Either 'debit' or 'credit'.
- `limitPrice` (float): The limit price to trigger a buy of the option.
- `stopPrice` (float): The price to trigger the limit order.
- `symbol` (str): The stock ticker of the stock to trade.
- `quantity` (int): The number of options to buy.
- `expirationDate` (str): The expiration date of the option in 'YYYY-MM-DD' format.
- `strike` (float): The strike price of the option.
- `optionType` (str): This should be 'call' or 'put'.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day. 'ioc' = immediate or cancel. 'opg' = execute at opening.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the buying of options, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_stop_limit(symbol, quantity, limitPrice, stopPrice, account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a stop order to be turned into a limit order once a certain stop price is reached.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to purchase.
- `quantity` (int): The number of stocks to buy.
- `limitPrice` (float): The price to trigger the market order.
- `stopPrice` (float): The price to trigger the limit order.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the purchase of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_stop_loss(symbol, quantity, stopPrice, account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a stop order to be turned into a market order once a certain stop price is reached.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to purchase.
- `quantity` (int): The number of stocks to buy.
- `stopPrice` (float): The price to trigger the market order.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the purchase of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_buy_trailing_stop(symbol, quantity, trailAmount, trailType='percentage', timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a trailing stop buy order to be turned into a market order when trailing stop price is reached.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to buy.
- `quantity` (int): The number of stocks to buy.
- `trailAmount` (float): How much to trail by; could be percentage or dollar value depending on `trailType`.
- `trailType` (str): Could be 'amount' or 'percentage'.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_crypto(symbol, side, quantityOrPrice, amountIn='quantity', limitPrice=None, timeInForce='gtc', jsonify=True)`

Submits an order for a crypto.

**Parameters:**
- `symbol` (str): The crypto ticker of the crypto to trade.
- `side` (str): Either 'buy' or 'sell'.
- `quantityOrPrice` (float): Either the decimal price of shares to trade or the decimal quantity of shares.
- `amountIn` (str, optional): If left as the default value of 'quantity', the order will attempt to trade cryptos by the amount of crypto you want to trade. If changed to 'price', the order will attempt to trade cryptos by the price you want to buy or sell.
- `limitPrice` (float, optional): The price to trigger the market order.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of crypto, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_option_credit_spread(price, symbol, quantity, spread, timeInForce='gtc', account_number=None, jsonify=True)`

Submits a limit order for an option credit spread.

**Parameters:**
- `price` (float): The limit price to trigger a sell of the option.
- `symbol` (str): The stock ticker of the stock to trade.
- `quantity` (int): The number of options to sell.
- `spread` (dict): A dictionary of spread options with the following keys:
  - `expirationDate`: The expiration date of the option in 'YYYY-MM-DD' format.
  - `strike`: The strike price of the option.
  - `optionType`: This should be 'call' or 'put'.
  - `effect`: This should be 'open' or 'close'.
  - `action`: This should be 'buy' or 'sell'.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day. 'ioc' = immediate or cancel. 'opg' = execute at opening.
- `account_number` (str, optional): The Robinhood account number.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the trading of options, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_option_debit_spread(price, symbol, quantity, spread, timeInForce='gtc', account_number=None, jsonify=True)`

Submits a limit order for an option debit spread.

**Parameters:**
- `price` (float): The limit price to trigger a sell of the option.
- `symbol` (str): The stock ticker of the stock to trade.
- `quantity` (int): The number of options to sell.
- `spread` (dict): A dictionary of spread options with the following keys:
  - `expirationDate`: The expiration date of the option in 'YYYY-MM-DD' format.
  - `strike`: The strike price of the option.
  - `optionType`: This should be 'call' or 'put'.
  - `effect`: This should be 'open' or 'close'.
  - `action`: This should be 'buy' or 'sell'.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day. 'ioc' = immediate or cancel. 'opg' = execute at opening.
- `account_number` (str, optional): The Robinhood account number.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the trading of options, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_option_spread(direction, price, symbol, quantity, spread, account_number=None, timeInForce='gtc', jsonify=True)`

Submits a limit order for an option spread, i.e., place a debit/credit spread.

**Parameters:**
- `direction` (str): Can be 'credit' or 'debit'.
- `price` (float): The limit price to trigger a trade of the option.
- `symbol` (str): The stock ticker of the stock to trade.
- `quantity` (int): The number of options to trade.
- `spread` (dict): A dictionary of spread options with the following keys:
  - `expirationDate`: The expiration date of the option in 'YYYY-MM-DD' format.
  - `strike`: The strike price of the option.
  - `optionType`: This should be 'call' or 'put'.
  - `effect`: This should be 'open' or 'close'.
  - `action`: This should be 'buy' or 'sell'.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day. 'ioc' = immediate or cancel. 'opg' = execute at opening.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the trading of options, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_crypto_by_price(symbol, amountInDollars, timeInForce='gtc', jsonify=True)`

Submits a market order for a crypto by specifying the amount in dollars that you want to trade. Good for share fractions up to 8 decimal places.

**Parameters:**
- `symbol` (str): The crypto ticker of the crypto to trade.
- `amountInDollars` (float): The amount in dollars of the crypto you want to sell.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of crypto, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_crypto_by_quantity(symbol, quantity, timeInForce='gtc', jsonify=True)`

Submits a market order for a crypto by specifying the decimal amount of shares to sell. Good for share fractions up to 8 decimal places.

**Parameters:**
- `symbol` (str): The crypto ticker of the crypto to trade.
- `quantity` (float): The decimal amount of shares to sell.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of crypto, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_crypto_limit(symbol, quantity, limitPrice, timeInForce='gtc', jsonify=True)`

Submits a limit order for a crypto by specifying the decimal amount of shares to sell. Good for share fractions up to 8 decimal places.

**Parameters:**
- `symbol` (str): The crypto ticker of the crypto to trade.
- `quantity` (float): The decimal amount of shares to sell.
- `limitPrice` (float): The limit price to set for the crypto.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of crypto, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_crypto_limit_by_price(symbol, amountInDollars, limitPrice, timeInForce='gtc', jsonify=True)`

Submits a limit order for a crypto by specifying the decimal price to sell. Good for share fractions up to 8 decimal places.

**Parameters:**
- `symbol` (str): The crypto ticker of the crypto to trade.
- `amountInDollars` (float): The amount in dollars of the crypto you want to sell.
- `limitPrice` (float): The limit price to set for the crypto.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the buying of crypto, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_fractional_by_price(symbol, amountInDollars, account_number=None, timeInForce='gfd', extendedHours=False, jsonify=True)`

Submits a market order to be executed immediately for fractional shares by specifying the amount in dollars that you want to trade. Good for share fractions up to 6 decimal places. Robinhood does not currently support placing limit, stop, or stop-loss orders for fractional trades.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to purchase.
- `amountInDollars` (

float): The amount in dollars of the fractional shares you want to buy.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the purchase of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_fractional_by_quantity(symbol, quantity, account_number=None, timeInForce='gfd', priceType='bid_price', extendedHours=False, jsonify=True, market_hours='regular_hours')`

Submits a market order to be executed immediately for fractional shares by specifying the amount that you want to trade. Good for share fractions up to 6 decimal places. Robinhood does not currently support placing limit, stop, or stop-loss orders for fractional trades.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to purchase.
- `quantity` (float): The amount of the fractional shares you want to buy.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gfd' = good for the day.
- `priceType` (str, optional): Sets the type of price to use. Default is 'bid_price'.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.
- `market_hours` (str, optional): Set to 'regular_hours' by default.

**Returns:**
- A dictionary that contains information regarding the purchase of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_limit(symbol, quantity, limitPrice, account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a limit order to be executed once a certain price is reached.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to sell.
- `quantity` (int): The number of stocks to sell.
- `limitPrice` (float): The price to trigger the sell order.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_market(symbol, quantity, account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a market order to be executed immediately.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to sell.
- `quantity` (int): The number of stocks to sell.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.
---
### `robin_stocks.robinhood.orders.order_sell_option_limit(positionEffect, creditOrDebit, price, symbol, quantity, expirationDate, strike, optionType='both', account_number=None, timeInForce='gtc', jsonify=True)`

Submits a limit order for an option, such as placing a short call or a short put.

**Parameters:**
- `positionEffect` (str): Either 'open' for a sell to open effect or 'close' for a sell to close effect.
- `creditOrDebit` (str): Either 'debit' or 'credit'.
- `price` (float): The limit price to trigger a sell of the option.
- `symbol` (str): The stock ticker of the stock to trade.
- `quantity` (int): The number of options to sell.
- `expirationDate` (str): The expiration date of the option in 'YYYY-MM-DD' format.
- `strike` (float): The strike price of the option.
- `optionType` (str): This should be 'call' or 'put'.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day. 'ioc' = immediate or cancel. 'opg' = execute at opening.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of options, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_option_stop_limit(positionEffect, creditOrDebit, limitPrice, stopPrice, symbol, quantity, expirationDate, strike, optionType='both', account_number=None, timeInForce='gtc', jsonify=True)`

Submits a stop order to be turned into a limit order once a certain stop price is reached.

**Parameters:**
- `positionEffect` (str): Either 'open' for a sell to open effect or 'close' for a sell to close effect.
- `creditOrDebit` (str): Either 'debit' or 'credit'.
- `limitPrice` (float): The limit price to trigger a sell of the option.
- `stopPrice` (float): The price to trigger the limit order.
- `symbol` (str): The stock ticker of the stock to trade.
- `quantity` (int): The number of options to sell.
- `expirationDate` (str): The expiration date of the option in 'YYYY-MM-DD' format.
- `strike` (float): The strike price of the option.
- `optionType` (str): This should be 'call' or 'put'.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day. 'ioc' = immediate or cancel. 'opg' = execute at opening.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of options, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_stop_limit(symbol, quantity, limitPrice, stopPrice, account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a stop order to be turned into a limit order once a certain stop price is reached.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to sell.
- `quantity` (int): The number of stocks to sell.
- `limitPrice` (float): The price to trigger the sell order.
- `stopPrice` (float): The price to trigger the limit order.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_stop_loss(symbol, quantity, stopPrice, account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a stop order to be turned into a market order once a certain stop price is reached.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to sell.
- `quantity` (int): The number of stocks to sell.
- `stopPrice` (float): The price to trigger the market order.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_sell_trailing_stop(symbol, quantity, trailAmount, trailType='percentage', timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a trailing stop sell order to be turned into a market order when the trailing stop price is reached.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to sell.
- `quantity` (int): The number of stocks to sell.
- `trailAmount` (float): How much to trail by; could be a percentage or dollar value depending on `trailType`.
- `trailType` (str): Could be 'amount' or 'percentage'.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the selling of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

---
### `robin_stocks.robinhood.orders.order_trailing_stop(symbol, quantity, side, trailAmount, trailType='percentage', account_number=None, timeInForce='gtc', extendedHours=False, jsonify=True)`

Submits a trailing stop order to be turned into a market order when the trailing stop price is reached.

**Parameters:**
- `symbol` (str): The stock ticker of the stock to trade.
- `quantity` (int): The number of stocks to trade.
- `side` (str): 'buy' or 'sell'.
- `trailAmount` (float): How much to trail by; could be a percentage or dollar value depending on `trailType`.
- `trailType` (str): Could be 'amount' or 'percentage'.
- `account_number` (str, optional): The Robinhood account number.
- `timeInForce` (str, optional): Changes how long the order will be in effect for. 'gtc' = good until canceled. 'gfd' = good for the day.
- `extendedHours` (str, optional): Premium users only. Allows trading during extended hours. Should be true or false.
- `jsonify` (str, optional): If set to False, function will return the request object which contains status code and headers.

**Returns:**
- A dictionary that contains information regarding the purchase or sale of stocks, such as the order id, the state of order (queued, confirmed, filled, failed, canceled, etc.), the price, and the quantity.

## Getting Crypto 
Contains functions to get information about crypto-currencies.

### `robin_stocks.robinhood.crypto.get_crypto_currency_pairs(info=None)`

Gets a list of all the cryptocurrencies that you can trade.

**Parameters:**
- `info` (str, optional): Filters the results to have a list of the values that correspond to the key that matches `info`.

**Returns:**
- A list of dictionaries containing key/value pairs for each ticker if `info` is left as `None`. If `info` is provided, it will return a list of strings where the strings are the values of the key that corresponds to `info`.

**Dictionary Keys:**
- `asset_currency`
- `display_only`
- `id`
- `max_order_size`
- `min_order_size`
- `min_order_price_increment`
- `min_order_quantity_increment`
- `name`
- `quote_currency`
- `symbol`
- `tradability`

---
### `robin_stocks.robinhood.crypto.get_crypto_historicals(symbol, interval='hour', span='week', bounds='24_7', info=None)`

Gets historical information about a cryptocurrency, including open price, close price, high price, and low price.

**Parameters:**
- `symbol` (str): The crypto ticker.
- `interval` (str): The time between data points. Options include '15second', '5minute', '10minute', 'hour', 'day', or 'week'. Default is 'hour'.
- `span` (str): The entire time frame to collect data points. Options include 'hour', 'day', 'week', 'month', '3month', 'year', or '5year'. Default is 'week'.
- `bounds` (str): The times of day to collect data points. Options include 'regular' (6 hours a day), 'trading' (9 hours a day), 'extended' (16 hours a day), '24_7' (24 hours a day). Default is '24_7'.
- `info` (str, optional): Filters the results to have a list of the values that correspond to the key that matches `info`.

**Returns:**
- A list of dictionaries containing key/value pairs for each ticker if `info` is left as `None`. If `info` is provided, it will return a list of strings where the strings are the values of the key that corresponds to `info`.

**Dictionary Keys:**
- `begins_at`
- `open_price`
- `close_price`
- `high_price`
- `low_price`
- `volume`
- `session`
- `interpolated`
- `symbol`

---
### `robin_stocks.robinhood.crypto.get_crypto_id(symbol)`

Gets the Robinhood ID of the given cryptocurrency used to make trades. This function uses an in-memory cache of the IDs to save a network round-trip when possible.

**Parameters:**
- `symbol` (str): The crypto ticker.

**Returns:**
- A string representing the symbol’s Robinhood ID.

---
### `robin_stocks.robinhood.crypto.get_crypto_info(symbol, info=None)`

Gets information about a cryptocurrency.

**Parameters:**
- `symbol` (str): The crypto ticker.
- `info` (str, optional): Filters the results to have a list of the values that correspond to the key that matches `info`.

**Returns:**
- A dictionary of key/value pairs for the cryptocurrency if `info` is left as `None`. If `info` is provided, it returns a string representing the value of the key that corresponds to `info`.

**Dictionary Keys:**
- `asset_currency`
- `display_only`
- `id`
- `max_order_size`
- `min_order_size`
- `min_order_price_increment`
- `min_order_quantity_increment`
- `name`
- `quote_currency`
- `symbol`
- `tradability`

---
### `robin_stocks.robinhood.crypto.get_crypto_positions(info=None)`

Returns crypto positions for the account.

**Parameters:**
- `info` (str, optional): Filters the results to get a specific value.

**Returns:**
- A list of dictionaries containing key/value pairs for each option. If `info` is provided, it returns a list of strings where the strings are the values of the key that matches `info`.

**Dictionary Keys:**
- `account_id`
- `cost_basis`
- `created_at`
- `currency`
- `id`
- `quantity`
- `quantity_available`
- `quantity_held_for_buy`
- `quantity_held_for_sell`
- `updated_at`

---
### `robin_stocks.robinhood.crypto.get_crypto_quote(symbol, info=None)`

Gets information about a cryptocurrency, including low price, high price, and open price.

**Parameters:**
- `symbol` (str): The crypto ticker.
- `info` (str, optional): Filters the results to have a list of the values that correspond to the key that matches `info`.

**Returns:**
- A dictionary of key/value pairs for the cryptocurrency if `info` is left as `None`. If `info` is provided, it returns a list of strings where the strings are the values of the key that corresponds to `info`.

**Dictionary Keys:**
- `ask_price`
- `bid_price`
- `high_price`
- `id`
- `low_price`
- `mark_price`
- `open_price`
- `symbol`
- `volume`

---
### `robin_stocks.robinhood.crypto.get_crypto_quote_from_id(id, info=None)`

Gets information about a cryptocurrency, including low price, high price, and open price, using the ID instead of the crypto ticker.

**Parameters:**
- `id` (str): The ID of a cryptocurrency.
- `info` (str, optional): Filters the results to have a list of the values that correspond to the key that matches `info`.

**Returns:**
- A dictionary of key/value pairs for the cryptocurrency if `info` is left as `None`. If `info` is provided, it returns a list of strings where the strings are the values of the key that corresponds to `info`.

**Dictionary Keys:**
- `ask_price`
- `bid_price`
- `high_price`
- `id`
- `low_price`
- `mark_price`
- `open_price`
- `symbol`
- `volume`

---
### `robin_stocks.robinhood.crypto.load_crypto_profile(info=None)`

Gets the information associated with the crypto account.

**Parameters:**
- `info` (str, optional): The name of the key whose value is to be returned from the function.

**Returns:**
- A dictionary of key/value pairs. If a string is passed into the `info` parameter, the function returns a string corresponding to the value of the key whose name matches the `info` parameter.

**Dictionary Keys:**
- `apex_account_number`
- `created_at`
- `id`
- `rhs_account_number`
- `status`
- `status_reason_code`
- `updated_at`
- `user_id`

## Export Information
Contains functions for exporting account data

### `robin_stocks.robinhood.export.create_absolute_csv(dir_path, file_name, order_type)`

Creates a filepath given a directory and file name.

**Parameters:**
- `dir_path` (str): Absolute or relative path to the directory where the file will be written.
- `file_name` (str): An optional argument for the name of the file. If not defined, the filename will be `stock_orders_{current date}`.
- `order_type` (str): Will be ‘stock’, ‘option’, or ‘crypto’.

**Returns:**
- An absolute file path as a string.

---
### `robin_stocks.robinhood.export.export_completed_crypto_orders(dir_path, file_name=None)`

Writes all completed crypto orders to a CSV file.

**Parameters:**
- `dir_path` (str): Absolute or relative path to the directory where the file will be written.
- `file_name` (str, optional): An optional argument for the name of the file. If not defined, the filename will be `crypto_orders_{current date}`.

---
### `robin_stocks.robinhood.export.export_completed_option_orders(dir_path, file_name=None)`

Writes all completed option orders to a CSV file.

**Parameters:**
- `dir_path` (str): Absolute or relative path to the directory where the file will be written.
- `file_name` (str, optional): An optional argument for the name of the file. If not defined, the filename will be `option_orders_{current date}`.

---
### `robin_stocks.robinhood.export.export_completed_stock_orders(dir_path, file_name=None)`

Writes all completed stock orders to a CSV file.

**Parameters:**
- `dir_path` (str): Absolute or relative path to the directory where the file will be written.
- `file_name` (str, optional): An optional argument for the name of the file. If not defined, the filename will be `stock_orders_{current date}`.

---
### `robin_stocks.robinhood.export.fix_file_extension(file_name)`

Ensures a file name ends with `.csv`.

**Parameters:**
- `file_name` (str): Name of the file.

**Returns:**
- A string where the file suffix is either added or replaced with `.csv`.

#
###### Backlinks:
- [Algotrading](Algotrading.md)