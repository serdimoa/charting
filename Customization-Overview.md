Customization is a broad concept which is why we included several articles about it.

## Customization done through data stream

These are mostly data-related customizations. They are done using a datafeed configuration response.

Here is an example of a configuration response.

```javascript
{
    supports_search: true,
    supports_group_request: false,
    supports_marks: true,
    exchanges: [
        {value: "", name: "All Exchanges", desc: ""},
        {value: "XETRA", name: "XETRA", desc: "XETRA"},
        {value: "NSE", name: "NSE", desc: "NSE"}
    ],
    symbolsTypes: [
        {name: "All types", value: ""},
        {name: "Stock", value: "stock"},
        {name: "Index", value: "index"}
    ],
    supportedResolutions: [ "1", "15", "30", "60", "D", "2D", "3D", "W", "3W", "M", '6M' ]
};
```

You can find the detailed descriptions of these parameters on the [JS API page](JS-Api#onreadycallback).

## Customizations that are done on the client-side

These customizations affect most of the UI/UX. They are done using chart widget constructor arguments.

Here is an example of a widget constructor call.

```javascript
var widget = new TradingView.widget({
    fullscreen: true,
    symbol: 'AA',
    interval: 'D',
    toolbar_bg: '#f4f7f9',
    allow_symbol_change: true,
    container_id: "tv_chart_container",
    datafeed: new Datafeeds.UDFCompatibleDatafeed("http://demo_feed.tradingview.com"),
    library_path: "charting_library/",
    locale: "en",
    drawings_access: { type: 'black', tools: [ { name: "Regression Trend" } ] },
    disabled_features: ["use_localstorage_for_settings", "volume_force_overlay"],
    enabled_features: ["move_logo_to_main_pane"],
    overrides: {
        "mainSeriesProperties.style": 0,
        "symbolWatermarkProperties.color" : "#944",
        "volumePaneSize": "tiny"
    },
    studies_overrides: {
        "bollinger bands.median.color": "#33FF88",
        "bollinger bands.upper.linewidth": 7
    },
    debug: true,
    time_frames: [
        { text: "50y", resolution: "6M" },
        { text: "1d", resolution: "5" },
    ],
    charts_storage_url: 'http://saveload.tradingview.com',
    client_id: 'tradingview.com',
    user_id: 'public_user',
    favorites: {
        intervals: ["1D", "3D", "3W", "W", "M"],
        chartTypes: ["Area", "Line"]
    }
});
```

More details are available in the [Widget Constructor Arguments article](Widget-Constructor).

## See Also

* [Widget's Constructor Arguments](Widget-Constructor)
* [Widget's Methods](Widget-Methods)
* [Customization Use Cases](Customization-Use-Cases)
