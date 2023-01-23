# Simple Binance WebSocket Template for .NET 7
## Requirements
[Binance.Net](https://github.com/JKorf/Binance.Net "Binance.Net")
`dotnet add package Binance.Net --version 8.3.0`

## Code
```csharp
using Binance.Net.Clients;
using Binance.Net.Enums;
using CryptoExchange.Net.Authentication;

namespace SimpleBinanceWebSocket;

public class Program
{
    static void Main(string[] args) => new Program().Start(args);

    private string _symbol => "BNBUSDT";    // Add symbol pair
    private string _key => "";              // Add API key
    private string _secret => "";           // Add Secret key
    private ApiCredentials _keys => new ApiCredentials(_key, _secret);

    private BinanceClient _client => new BinanceClient();
    private BinanceSocketClient _socketClient => new BinanceSocketClient();

    public void Start(string[] args)
    {
        _client.SetApiCredentials(_keys);
        _socketClient.SetApiCredentials(_keys);
        StartSocketLoop(KlineInterval.OneMinute);   // Add kline interval
        Console.ReadLine();
    }

    private async void StartSocketLoop(KlineInterval klineInterval)
    {
        await _socketClient.SpotStreams.SubscribeToKlineUpdatesAsync(_symbol, klineInterval, dataEvent =>
        {
            if (dataEvent.Data.Data.Final)
            {
                // Do stuff here
                Console.WriteLine(dataEvent.Data.Data.ClosePrice);
            }
        });
    }
}
```
