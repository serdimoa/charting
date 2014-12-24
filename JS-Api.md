#Overview

To feed your data to the charts using JS API, create an object that has a specific public interface. It’s rather compact; your object needs a few functions. Data caching (history & symbol info) is implemented in Charting Library. When you create an object implementing described interface, just pass it to Library widget constructor through `datafeed `argument.

#Methods

1. setup
2. searchSymbolsByName
3. resolveSymbol
4. getBars
5. subscribeBars
6. unsubscribeBars
7. calculateHistoryDepth
8. getMarks
9. getQuotes

###setup(reserved, callback)
1. `reserved` is not used now
2. `callback`: function(configurationData)
  1. `configurationData`: object (see below)

This call is intended to provide the object filled with configuration data. This data affects some of chart behavior aspects so it is called [[server-side customization|Charts-Customization-101#customization-done-through-data-stream]]. Charting Library expects you will call callback and pass your datafeed `configurationData` as an argument. Configuration data is an object; for now, following properties are supported:

#####exchanges
An array of exchange descriptors. Exchange descriptor is an object `{value, name, desc}`. `value` will be passed as `exchange` argument  to searchSymbolsByName (see below).

`exchanges` = []  leads to exchanges filter absence in Symbol Search list. Use `value` = "" if you want to create wildcard filter (all exchanges).

#####symbols_types
An array of filter descriptors. Filter descriptor is an object `{name, value}`. `value` will be passed as `symbolType` argument  to searchSymbolsByName. 

`symbolsTypes` = []  leads to types filter absence in Symbol Search list. Use `value` = "" if you want to create wildcard filter (all types)

#####supported_resolutions
An array of supported resolutions. Resolution may be a number or a string. If the resolution is a number, it is treated as minutes count. Strings may be "*D", "*W", "*M" (* means any number).

'resolutions'=undefined or [] leads to resolutons widget having its default content (see <http://tradingview.com/e/> ). Example: `[1, 15, 240, "D", "6M"]` will give you "1 minute, 15 minutes, 4 hours, 1 day, 6 months" in resolution widget.

#####supports_marks
Boolean showing whether your datafeed supports marks on bars or not. 

###searchSymbolsByName(userInput, exchange, symbolType, onResultReadyCallback)
1. `userInput`: string. It is text entered by user in symbol search field
2. `exchange`: string. The requested exchange (chosen by user). Empty value means no filter was specified.
3. `symbolType`: string. The requested symbol type: index, stock, forex e.t.c. (chosen by user). Empty value means no filter was specified.
4. `onResultReadyCallback`: function(result)
  1. `result`: array (see below)

This call is intended to provide the list of symbols matching to user's search query. `result` is expected to be smth like this:

```javascript
[
    {
        "symbol": <short symbol name>,
        "full_name": <full symbol name -- e.g., BTCE:BTCUSD>,
        "description": <symbol description>,
        "exchange": <symbol exchange name>,
        "ticker": <symbol ticker name, optional>,
        "type": "stock" | "futures" | "bitcoin" | "forex" | "index"
    }, {
        //    .....
    }
]
```

If no symbols are found, then callback should be called with an empty array.

###resolveSymbol(symbolName, onSymbolResolvedCallback, onResolveErrorCallback)
1. `symbolName`: string. Symbol name or `ticker` if provided.
2. `onSymbolResolvedCallback`: function([[SymbolInfo|Symbology]]
3. `onResolveErrorCallback`: function(reason)

Charting Library will call this function when it need to get [[SymbolInfo|Symbology]] by symbol's name.

###getBars(symbolInfo, resolution, from, to, onHistoryCallback, onErrorCallback)
1. `symbolInfo`: [[SymbolInfo|Symbology]] object
2. `resolution`: string
3. `from`: unix timestamp, leftmost required bar time
3. `to`: unix timestamp, rightmost required bar time
4. `onHistoryCallback`: function(bars)
  1. `bars`: array of `{time, close, open, high, low, volume}`
5. `onErrorCallback`: function(reason)

This function is called when chart needs a history fragment defined by dates range. The charting library expects `onHistoryCallback` to be called **just once** after receiving all the requesting history. No further calls are expected.

**Remark**: each bar object must have `time` and `close` properties. Others are optional.
**Remark 2**: `bar.time` is expected to be the amount of milliseconds since Unix epoch start in **UTC** timezone.


###subscribeBars(symbolInfo, resolution, onRealtimeCallback, subscriberUID)
1. `symbolInfo`: [[SymbolInfo|Symbology]] object
2. `resolution`: string
3. `onRealtimeCallback`: function(bar)
  1. `bar`: object `{time, close, open, high, low, volume}`
4. `subscriberUID`: object

Charting Library calls this function when it wants to receive realtime updates for a symbol. Chart expects you will call `onRealtimeCallback` every time you want to update the most recent bar or to append a new one. 

**Remark**: When you call `onRealtimeCallback` with bar having time equal to most recent bar's time, the whole last bar is replaced with the `bar` object you've passed into the call. Example:

1. The most recent bar is `{1419411578413, 10, 12, 9, 11}`
2. You call `onRealtimeCallback({1419411578413, 10, 14, 9, 14})`
3. Library finds out that bar with time `1419411578413` already exists and is the most recent one
4. Library replaces the whole bar so now the most recent bar is `{1419411578413, 10, 14, 9, 14}`

**Remark 2**: Is it possible either to update the most recent bar or to append a new one with `onRealtimeCallback`. You've get an error if you call this function trying to update a bar in history.

**Remark 3**: For now, there is no way to change bars in history after the chart received it.

###unsubscribeBars(subscriberUID)
1. `subscriberUID`: object

Library calls this function when is doesn't want to receive updates for this subscriber any more. `subscriberUID` will be the same object which Library passed to `subscribeBars` before.

###calculateHistoryDepth(resolution, resolutionBack, intervalBack)
1. `resolution`: requested symbol's resolution
2. `resolutionBack`: desired history period dimension. Supported values: `D` | `M`
3. `intervalBack`: amount or `resolutionBack` periods which Library is going to request

Charting Library calls this function when it is going to request some history data to give you an ability to override required history depth. It passes some arguments so you could know how much bars is it going to get. Here are a few examples:

* `calculateHistoryDepth("D", "M", 12)` called: the Library is going to request 12 months of daily bars
* `calculateHistoryDepth(60, "D", 15)` called: the Library is going to request 15 days of hourly bars

This function should return `undefined` if you do not want to override anything. If you do, it should return an object `{resolutionBack, intervalBack}`. Properties meaning is similar to respective arguments' one.

Example: 

Assume the implementation is
```javascript
Datafeed.prototype.calculateHistoryDepth = function(period,
resolutionBack, intervalBack) {
    if (period == "1D") {
        return {
            resolutionBack: 'M',
            intervalBack: 6
        };
    }
}
```

This means when Charting Library will request the data for '1D' resolution, the history will be 6 months in depth. In all other cases the history depth will have the default value.
