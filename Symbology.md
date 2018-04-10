The Charting Library uses your own data where you define the symbology yourself. You may name the symbols as you see fit.

But there are some fine points that you should be aware of:

1. Our own symbology assumes that symbol names use `EXCHANGE:SYMBOL` format.
    The Library supports this by default. You may continue using if it meets your requirements.
1. If you already have or considering a different symbology then you might want to use the `ticker` field.

    `ticker` is the unique identifier of the symbol that is used **only** inside the Library. Your users will never be able to see it.
    Simply enter the `ticker` values in all of your SymbolInfo objects and Symbol Search results and expect that the Charting Library will request the data based on those values.

# SymbolInfo Structure

**This section is extremely important. 72.2% of all issues experienced by Charting Library users are caused by wrong/malformed SymbolInfo data.**

SymbolInfo is an object containing symbol metadata. This object is the result of resolving the symbol. SymbolInfo has following fields:

## name

It's the name of the symbol. It is a string that your users will be able to see. Also, it will be used for data requests if you are not using **tickers**.

## ticker

It's an unique identifier for this particular symbol in your symbology.
If you specify this property then its value will be used for all data requests for this symbol. `ticker` will be treated the same as `symbol` if not specified explicitly.

## description

Description of a symbol. Will be displayed in the chart legend for this symbol.

## type

Optional type of the instrument.

*Possible types are:*

- `stock`
- `index`
- `forex`
- `futures`
- `bitcoin`
- `expression`
- `spread`
- `cfd`
- or another string value.

## session

Trading hours for this symbol. See the [Trading Sessions](Trading-Sessions) article to learn more details.

## exchange, listed_exchange

Both of these fields are expected to have a short name of the exchange where this symbol is traded.

The name will be displayed in the chart legend for this symbol.

## timezone

Timezone of the exchange for this symbol. We expect to get the name of the time zone in `olsondb` format.

*Supported timezones are:*

- `Africa/Johannesburg`
- `America/Argentina/Buenos_Aires`
- `America/Bogota`
- `America/Caracas`
- `America/Chicago`
- `America/El_Salvador`
- `America/Los_Angeles`
- `America/Mexico_City`
- `America/New_York`
- `America/Phoenix`
- `America/Sao_Paulo`
- `America/Toronto`
- `America/Vancouver`
- `Asia/Almaty`
- `Asia/Ashkhabad`
- `Asia/Bangkok`
- `Asia/Dubai`
- `Asia/Hong_Kong`
- `Asia/Kathmandu`
- `Asia/Kolkata`
- `Asia/Seoul`
- `Asia/Shanghai`
- `Asia/Singapore`
- `Asia/Taipei`
- `Asia/Tehran`
- `Asia/Tokyo`
- `Australia/ACT`
- `Australia/Adelaide`
- `Australia/Brisbane`
- `Australia/Sydney`
- `Europe/Athens`
- `Europe/Berlin`
- `Europe/Istanbul`
- `Europe/London`
- `Europe/Madrid`
- `Europe/Moscow`
- `Europe/Paris`
- `Europe/Warsaw`
- `Europe/Zurich`
- `Pacific/Auckland`
- `Pacific/Chatham`
- `Pacific/Fakaofo`
- `Pacific/Honolulu`
- `US/Mountain`

## minmov, pricescale, minmove2, fractional

These three keys have different meanings when used for common and fractional prices.

### Common prices

`MinimalPossiblePriceChange = minmov / pricescale`

- `minmov` is the amount of price precision steps for 1 tick. For example, since the tick size for U.S. equities is `0.01`, `minmov` is 1. But the price of the E-mini S&P futures contract moves upward or downward by `0.25` increments, so the `minmov` is `25`.
- `pricescale` defines the number of decimal places. It is `10^number-of-decimal-places`. If a price is displayed as `1.01`, `pricescale` is `100`; If it is displayed as `1.005`, `pricescale` is `1000`.
- `minmove2` for common prices is `0` or it can be skipped.
- `fractional` for common prices is `false` or it can be skipped.

Example:

Typical stock with `0.01` price increment: `minmov = 1, pricescale = 100, minmove2 = 0`.

### Fractional prices

Fractional prices are displayed 2 different forms: 1) `xx'yy` (for example, `133'21`) 2) `xx'yy'zz` (for example, `133'21'5`).

- `xx` is an integer part.
- `minmov/pricescale` is a Fraction.
- `minmove2` is used in form 2.
- `fractional` is `true`

Example:

If `minmov = 1`, `pricescale = 128` and `minmove2 = 4`:

- `119'16'0` represents `119 + 16/32`
- `119'16'2` represents `119 + 16.25/32`
- `119'16'5` represents `119 + 16.5/32`
- `119'16'7` represents `119 + 16.75/32`

More examples:

- `ZBM2014 (T-Bond)` with `1/32`: `minmov = 1`, `pricescale = 32`, `minmove2 = 0`
- `ZCM2014 (Corn)` with `2/8`: `minmov = 2`, `pricescale = 8`, `minmove2 = 0`
- `ZFM2014 (5 year t-note)` with `1/4 of 1/32`: `minmov = 1`, `pricescale = 128`, `minmove2 = 4`

## has_intraday

*Default:* `false`

Boolean value showing whether the symbol includes intraday (minutes) historical data.

If it's `false` then all buttons for intraday resolutions will be disabled for this particular symbol.

If it is set to `true`, all resolutions that are supplied directly by the datafeed must be provided in `intraday_multipliers` array.

## supported_resolutions

An array of resolutions which should be enabled for this symbol.

Each item of an array is expected to be a string. Format is described in another [article](Resolution).

If one changes the symbol and new symbol does not support the selected resolution then resolution will be switched to the first available one in the list.

Resolution availability logic (pseudocode):

```javascript
resolutionAvailable  =
    resolution.isIntraday
        ? symbol.has_intraday && symbol.supports_resoluiton(resolution)
        : symbol.supports_resoluiton(resolution);
```

In case of absence of `supported_resolutions` in a symbol info all DWM resolutions will be available. Intraday resolutions will be available if `has_intraday` is `true`.

Supported resolutions affect available timeframes too. The timeframe will not be available if it requires the resolution that is not supported.

## intraday_multipliers

*Default:* `[]`

It is an array containing intraday resolutions (in minutes) that the data feed may provide.

E.g., if the data feed supports resolutions such as `["1", "5", "15"]`, but has 1-minute bars for some symbols then you should set `intraday_multipliers` of this symbol to `[1]`. This will make Charting Library build 5 and 15-minute resolutions by itself.

## has_seconds

*Default:* `false`

Boolean value showing whether the symbol includes seconds in the historical data.

If it's `false` then all buttons for resolutions that include seconds will be disabled for this paricular symbol.

If it is set to `true`, all resolutions that are supplied directly by the data feed must be provided in `seconds_multipliers` array.

## seconds_multipliers

*Default:* `[]`

It is an array containing resolutions that include seconds (exluding postfix) that the data feed provides.

E.g., if the data feed supports resolutions such as `["1S", "5S", "15S"]`, but has 1-second bars for some symbols then you should set `seconds_multipliers` of this symbol to `[1]`. This will make Charting Library build 5S and 15S resolutions by itself.

## has_daily

*Default:* `false`

The boolean value showing whether data feed has its own daily resolution bars or not.

If `has_daily` = `false` then Charting Library will build the respective resolutions using 1-minute bars by itself. If not, then it will request those bars from the data feed.

## has_weekly_and_monthly

*Default:* `false`

The boolean value showing whether data feed has its own weekly and manthly resolution bars or not.

If `has_weekly_and_monthly` = `false` then Charting Library will build the respective resolutions using daily bars by itself. If not, then it will request those bars from the data feed.

## has_empty_bars

*Default:* `false`

The boolean value showing whether the library should generate empty bars in the session when there is no data from the data feed for this particular time.

I.e., if your session is `0900-1600` and your data has gaps between `11:00` and `12:00` and your `has_empty_bars` is `true`, then the Library will fill the gaps with bars for this time.

## force_session_rebuild

*Default:* `true`

The boolean value showing whether the library should filter bars using the current trading session.

If `false`, bars will be filtered only when the library builds data from another resolution or if `has_empty_bars` was set to `true`.

If `true`, then the Library will remove bars that dont't belong to the trading session from your data.

## has_no_volume

*Default:* `false`

Boolean showing whether the symbol includes volume data or not.

## volume_precision

*Default:* `0`

Integer showing typical volume value decimal places for a particular symbol. 0 means volume is always an integer. 1 means that there might be 1 numeric character after the comma.

## data_status

The status code of a series with this symbol. The status is shown in the upper right corner of a chart.

*Supported statuses are:*

- `streaming`
- `endofday`
- `pulsed`
- `delayed_streaming`

## expired

*Default:* `false`

Boolean value showing whether this symbol is an expired futures contract or not.

## expiration_date

Unix timestamp of the expiration date. One must set this value when `expired` = `true`.

Charting Library will request data for this symbol starting from that time point.

## sector

Sector for stocks to be displayed in the Symbol Info.

## industry

Industry for stocks to be displayed in the Symbol Info.

## currency_code

Currency to be displayed in the Symbol Info.
