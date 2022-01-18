## Creating client
There are 2 clients available to interact with the CoinEx API, the `CoinExClient` and `CoinExSocketClient`.

*Create a new rest client*
````C#
var coinExClient = new CoinExClient(new CoinExClientOptions()
{
	// Set options here for this client
});
````

*Create a new socket client*
````C#
var coinExSocketClient = new CoinExSocketClient(new CoinExSocketClientOptions()
{
	// Set options here for this client
});
````

Different options are available to set on the clients, see this example
````C#
var coinExClient = new CoinExClient(new CoinExClientOptions()
{
	ApiCredentials = new ApiCredentials("API-KEY", "API-SECRET"),
	LogLevel = LogLevel.Trace,
	RequestTimeout = TimeSpan.FromSeconds(60)
});
````
Alternatively, options can be provided before creating clients by using `SetDefaultOptions`:
````C#
CoinExClient.SetDefaultOptions(new CoinExClientOptions{
	// Set options here for all new clients
});
var coinExClient = new CoinExClient();
````
More info on the specific options can be found on the [CryptoExchange.Net wiki](https://github.com/JKorf/CryptoExchange.Net/wiki/Options)

### Dependency injection
See [CryptoExchange.Net wiki](https://github.com/JKorf/CryptoExchange.Net/wiki/Clients#dependency-injection)

## Usage
Make sure to read the [CryptoExchange.Net wiki](https://github.com/JKorf/CryptoExchange.Net/wiki/Clients#processing-request-responses) on processing responses.

#### Get market data
````C#
// Getting info on all symbols
var symbolData = await coinexClient.SpotApi.ExchangeData.GetSymbolsAsync();

// Getting tickers for all symbols
var tickerData = await coinexClient.SpotApi.ExchangeData.GetTickersAsync();

// Getting the order book of a symbol
var orderBookData = await coinexClient.SpotApi.ExchangeData.GetOrderBookAsync("BTC-USDT", 0);

// Getting recent trades of a symbol
var tradeHistoryData = await coinexClient.SpotApi.ExchangeData.GetTradeHistoryAsync("BTC-USDT");
````

#### Requesting balances
````C#
var accountData = await coinexClient.SpotApi.Account.GetAccountsAsync();
````
#### Placing order
````C#
// Placing a buy limit order for 0.001 BTC at a price of 50000USDT each
var orderData = await coinexClient.SpotApi.Trading.PlaceOrderAsync(
                "BTCUSDT",
                OrderSide.Buy,
                OrderType.Limit,
                0.001m,
                50000);
									
													
// Place a stop loss order, place a limit order of 0.001 BTC at 39000USDT each when the last trade price drops below 40000USDT
var orderData = await coinexClient.SpotApi.Trading.PlaceOrderAsync(
                "BTCUSDT",
                OrderSide.Buy,
                OrderType.StopLimit,
                0.001m,
                39000,
                stopPrice: 40000);
````

#### Requesting a specific order
````C#
// Request info on order with id `1234`
var orderData = await coinexClient.SpotApi.Trading.GetOrderAsync("BTCUSDT", 1234);
````

#### Requesting order history
````C#
// Get all closed orders conform the parameters
 var ordersData = await coinexClient.SpotApi.Trading.GetClosedOrdersAsync("BTCUSDT");
````

#### Cancel order
````C#
// Cancel order with id `1234`
var orderData = await coinexClient.SpotApi.Trading.CancelOrderAsync("BTCUSDT", 1234);
````

#### Get user trades
````C#
var userTradesResult = await coinexClient.SpotApi.Trading.GetUserTradesAsync("BTCUSDT");
````

#### Subscribing to market data updates
````C#
var subscribeResult = await coinexSocket.SpotStreams.SubscribeToAllTickerUpdatesAsync(data =>
{
	// Handle ticker data
});
````

#### Subscribing to order updates
````C#
var subscribeResult = await kucoinSocketClient.SpotStreams.SubscribeToOrderUpdatesAsync(data =>
{
	// Handle order updates
});
````
