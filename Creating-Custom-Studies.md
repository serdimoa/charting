We use our [Pine scripting language](https://www.tradingview.com/study-script-reference/) to create indicators for Charting Library. If you want to create a custom indicator you have to write it in Pine using TradingView site. Then contact us and we'll compile your Pine code to a code for Charting Library. Then use widget constructor's `indicators_file_name` parameter to show the library where to find your studies.


### How to show your data as an indicator

Here is an instruction for the case if you have some data which you want to show on the chart like an indicator and which cannot be computed from series data directly.

Plese follow these few steps:

  1. Contrive a new ticker for your data and set up your server to return valid SymbolInfo for this ticker.
  2. Also set up the server to return valid history data for this ticker.
  3. Write a simple script in Pine which will request this ticker's data (with `security()` function) and render it with an appropriate style.
  4. Send this script to us and we'll return the compiled version.
  5. Update your widget's initialization code to [create](https://github.com/tradingview/charting_library/wiki/Widget-Methods#createstudyname-forceoverlay-lock-inputs-callback-overrides) this indicator when chart is ready.

**Example**

Assume you want to show user's equity curve on his charts. You will have to do the following:

* Create a name for the new ticker. Assume it will be `#EQUITY` ticker. You can use any name you can imagine.
* Change your server's code to resolve this ticker as a valid symbol. Return the minimal valuable  SymbolInfo for this case.
* Make the server to return valid history for this ticker. I.e., the server could ask your database for equity history and return this data just as you return the history for generic symbols (like, say, `AAPL`).
* Write a script in Pine. This could be something likeshown below. Send this script to us.
```
       study("Equity", overlay = false)
       s = security("#EQUITY", period, close)
       plot(s, color=red)
```
* Plug the compiled indocator into your Charting Library with [indicators_file_name](https://github.com/tradingview/charting_library/wiki/Widget-Constructor#indicators_file_name) option.
* Change your widget's intiialization code. Add something like this
```
    widget = new TradingView.Widget(/* ... */);

    widget.onChartReady(function() {
        widget.createStudy('Equity', false, true);
    });
```
```