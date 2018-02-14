## Data Questions

<!--
To be consistent please make sure that each item of file has the following structure:
<details><summary>
<b>NUM. TITLE</b>
</summary><p>

DESCRIPTION

</details>
-->

<!-- markdownlint-disable no-inline-html -->

<details><summary>
<b>1. How do I connect my data? How to add new ticker symbols?</b>
</summary><p>

The Charting Library should to be used by technical specialists.
It requires advanced skills in JavaScript and deep knowledge of WEB protocols.
You should know it yourself or have/hire people who know this.
Additionally, if you don't have a WEB API, you will need at least a server language programmer and a system administrator to implement a WEB API on the server side.

We’ve done lot of work to make the process of connecting data simple and clear.

First, you need to read and understand this article: [How to connect my data](How-To-Connect-My-Data.md)

If you still have questions, open [Demo Chart](https://demo_chart.tradingview.com), then open Debugger-Network and filter requests by `demo_feed`. You will see all requests and corresponding responses in the [UDF](UDF.md) format.

</details>

<details><summary>
<b>2. Do you have an example of JS API implementation?</b>
</summary><p>

If you look at the picture below, you will see the UDF Adapter as an example of the JS API implementation. Its code is not minified and it is written in such a way that our clients can understand how it works.

[Scheme](How-To-Connect-My-Data.md#udf-scheme)

</details>

<details><summary>
<b>3. Do you have an example of a WebSocket data transport?</b>
</summary><p>

We don’t have an example of such integration, but we still hope to make this example in the future.

</details>

<details><summary>
<b>4. Do you have an example of a back-end data feed on ASP.NET, Python, PHP etc. ?</b>
</summary><p>

The only example of a back-end feed that we have is written on Javascript for NodeJS. You can find it here: [yahoo_datafeed](https://github.com/tradingview/yahoo_datafeed)

</details>

<details><summary>
<b>5. How can I display my data stored in a TXT/CSV/Excel file?</b>
</summary><p>

First of all, the Charting Library is not intended to display data from files. It is used to display bars data from a server. Secondly, you should keep in mind that according to the agreement you should use Charting Library on public websites only. If you still want to use a file as the source of data you will need to do the following steps:

1. Write an application using any server language (.NET, PHP, NodeJS, Python etc.). This application should read the file and provide the data from it in [UDF](UDF.md) format over HTTP(S).

    Note: You can provide data in another format or use websocket to transfer it, but in this case you will need to implement a [JS-Api](JS-Api.md) adapter on a client.
1. You should either have a static IP or register a domain so a browser can send requests to your server.
1. Open `index.html` and replace `demo_feed.tradingview.com` with the URL to your server.

</details>

<details><summary>
<b>6. Why my data is not displayed / displayed incorrectly / incorrectly fetched from a server?</b>
</summary><p>

The first thing you should do is open `index.html` or your script where you create the library widget and put the following line in the initialization options of the widget: `debug: true,`.
Once you have done that, you will see lot of helpful information in your browser console.
Most of important actions that happen in the library are explained in the console.

Please read [Symbology](Symbology.md) thoroughly. Most of errors with data happen because of incorrect symbol settings.

</details>

<details><summary>
<b>7. Charting Library is constantly asking for data. How to tell it that data is finished?</b>
</summary><p>

Specifically for this purpose there is a flag that can be added to the responses from your server that tells the library that there is no more data on the server.
It is called `no_data` for [UDF](UDF.md#bars) and `noData` for [JS API](JS-Api.md#getbarssymbolinfo-resolution-from-to-onhistorycallback-onerrorcallback-firstdatarequest)

</details>

<details><summary>
<b>8. How to change the number of decimal places of prices on a chart?</b>
</summary><p>

Please read [Symbology](Symbology.md) thoroughly. Number of decimal places is calculated based on `minmov` and `pricescale` values.

</details>

<details><summary>
<b>9. What if I have a single price for each timestamp?</b>
</summary><p>

You still can display your data if you have only one price for each timestamp, but obviously you will not be able to display the data as bars/candles. Since the library is intended to display different styles of data: candles, bars, histogram, you are supposed to provide Open, High, Low, Close and optional Volume for each timestamp. If you have only one price, you can pass `Open = High = Low = Close = price`. For better view of this data, you can change default chart style to “Line” (see GUI Questions).

</details>

## GUI Questions

<details><summary>
<b>1. How can I subscribe to chart events?</b>
</summary><p>

We have a few ways to subscribe to the events:

1. Subscribing to general events that are related to a whole chart layout, not a specific chart.
    [Open article](Widget-Methods.md#subscribing-to-chart-events)
1. Subscribing to events that are related to a single chart
    [Open article](Chart-Methods.md#subscribing-to-chart-events)

Check the result value of subscription methods.
Some of them return a [Subscription](Subscription.md) object that has methods `subscribe`/`unsubscribe`. The others accept a callback function.

</details>

<details><summary>
<b>2. How to change default bars style from Candles to another one?</b>
</summary><p>

You need to use [overrides](Widget-Constructor.md#overrides) of the Widget Constructor. Add `mainSeriesProperties.style` key. You can find allowed values in [this article](Overrides.md)

</details>

<details><summary>
<b>3. How can I change the list of resolutions (time intervals) on a chart / make them grayed?</b>
</summary><p>

* List of the resolutions displayed in a pop-up on a chart is defined by [supported_resolutions](JS-Api.md#supported_resolutions) from the data feed configuration.
* Resolutions available for a certain instrument are defined by [supported_resolutions](Symbology.md#supported_resolutions) from the instrument/symbol information.
* If you support intraday resolutions, you need to set [has_intraday](Symbology.md#has_intraday)
* Additionally, if you support seconds, you need to set [has_seconds](Symbology.md#has_seconds)
* If you support daily resolutions, you should set [has_daily](Symbology.md#has_daily)
* If you support weeks and months, you should set [has_weekly_and_monthly](Symbology.md#has_weekly_and_monthly)
* Additionally, you should set the resolutions, which are provided by your server for [intraday resolutions](Symbology.md#intraday_multipliers) and separately for [seconds](Symbology.md#seconds_multipliers).
* If an instrument supports (`supported_resolutions`) more resolutions that can be provided by the server (`intraday_multipliers`), the other resolutions are constructed by the chart.

</details>

<details><summary>
<b>4. How to hide a GUI Element (symbol, interval, button etc.)?</b>
</summary><p>

* Most of GUI elements can be hidden using [Featuresets](Featuresets.md). Please look at the [Interactive map of featuresets](http://tradingview.github.io/featuresets.html) to find what you need.
* There are base elements that cannot be hidden, but if you still want to get rid of them you can use [CSS customization](Widget-Constructor.md#custom_css_url). Please note that the names, classes and identifiers of DOM elements may be changed in future versions of the product without any notifications.

</details>

## Other Questions

<details><summary>
<b>1. What is the the difference between Widget, Charting Library and Trading Terminal?</b>
</summary><p>

* [Widget](https://tradingview.com/widget/) is connected to TradingView data. Perfect for websites, blogs and forums where you need a fast & free solution.

    Integration is simply cutting & pasting pre-made iframe code. It has lots of display modes.

* [Charting Library](https://www.tradingview.com/HTML5-stock-forex-bitcoin-charting-library/) is a chart with your own data.

    This is a standalone solution that you download, host on your servers, connect your own data & use in your site/app for free.

* [Trading Terminal](https://www.tradingview.com/trading-terminal/) is a standalone product that is licensed to brokers.

    It includes all features available in the Charting Library, but it also has trading functionality, multiple chart layouts, watchlists, details, news widgets and other advanced tools.

    It has its own licensing fees associated with it.

</details>

<details><summary>
<b>2. How do I add a custom indicator?</b>
</summary><p>

At the moment there is only one way to add custom indicators. It is described in a [dedicated article](Creating-Custom-Studies.md).

</details>

<!-- markdownlint-enable no-inline-html -->
