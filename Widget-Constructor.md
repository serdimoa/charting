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

Find full supported parameters list below. Please remember that changing those parameters after chart is initialized **does not work**. If you want to change the state of a chart after it was initialized, use [[Widget methods|Widget-Methods]] instead.

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

#####drawings_access
*version: 1.1*
This proprty has the same structure as the `studies_access` described above. Use the same names as you see them in UI.
**Remark**: There is a special case for font-based drawings. Use "Font Icons" name for them. This group is a special case and its drawings cannot be enabled or disabled particularly -- one can either enable or disable the whole group.

#####saved_data
JS object containing saved chart content (see save/load calls below). Use this parameter if you already have chart's JSON when creating chart. If you want to load chart content to chart that is already initialized then use `loadData()` widget method. 

#####locale
Locale to be used by Charting Library. See [[Localization]] section for details.

#####overrides
The object containing default Widget properties overrides. Overriding a property means assigning a default value to it.
You can override most of Charting Library properties (which also may be edited by user through UI) using `overrides` parameter of Widget constructor. `overrides` supposed to be an object having range of fields. Each field name is a name of overridden property and the field value is the desired value for those property. Example:

```
overrides: {
    "symbolWatermarkProperties.transparency": 100
}
```

This override will make the watermark 100% opaque (invisible). All customizable properties are listed in [[separate article|Overrides]]