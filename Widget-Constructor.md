You may specify Charting library widget parameters when calling its constructor. E.g., your call may look like

```
new TradingView.widget({
    symbol: 'A',
    interval: 'D',
    timezone: "America/New_York",
    container_id: "tv_chart_container",
    locale: "ru",
    datafeed: new Datafeeds.UDFCompatibleDatafeed("https://demo_feed.tradingview.com")
});
```

Find full supported parameters list below.

#####symbol, interval [mandatory]
Initial symbol & interval of your chart.

#####container_id [mandatory]
`id` attribute of a DOM element you want to contain the widget.

#####datafeed [mandatory]
JavaScript object implementing appropriate interface ([[JS API|JS-Api]]) to feed the chart width data.

#####timezone <UTC>
Initial timezone of the chart. Numbers on time scale depend on this timezone.
See [[supported timezones list|Symbology#timezone]] for available values.

#####library_path
A path to `static` folder

#####width, height
The desired size of a widget. Please make sure the widget has enough space to look smart.

**Remark**: if you want the chart to occupy all the available space, do not use '100%' in those field. Use `fullscreen` parameter instead (see below). It's because of issues with DOM nodes resizing in different browsers.

#####toolbar_bg
Toolbars background color

#####disabled_studies <[]>
*version: 0.4, obsolete(1.1 - 1.5) (use studies_access instead)*
An array containing all the disabled studies names. Use names as you can see them in Indicators widget. 

#####enabled_studies <[]>
*version: 0.4, obsolete(1.1 - 1.5) (use studies_access instead)*
An array containing all the enabled studies names. Pass [] or nothing if you want to enable all. Has priority above `disabled_studies`

#####studies_access
*version: 1.1*
An object with following structure:
```
{
    type: "black" | "white",
    tools: [
        {
            name: "<study name>",
            grayed: true
        },
        < ... >
    ]
}
```
* `type` is a type of list. Supported values: `black` (all the listed items should be disabled), `white` (only the listed items should be enabled).
* `tools` is an array of objects. Each object could have following properties:
  * `name` (mandatory) is the name of a study. Use the same names as you can see them in Indicators widget
  * `grayed` is a boolean showing whether this study should be visible but look like it's disabled. If the study is grayed and user clicks it, then `onGrayedObjectClicked` is called.