You may specify Charting library widget parameters when calling its constructor. Here is an example of a call.

```javascript
new TradingView.widget({
    symbol: 'A',
    interval: 'D',
    timezone: "America/New_York",
    container_id: "tv_chart_container",
    locale: "ru",
    datafeed: new Datafeeds.UDFCompatibleDatafeed("https://demo_feed.tradingview.com")
});
```

Below is a complete list of supported parameters. Note that you can't change the parameters once the Charting Library is created. Use [Widget methods](Widget-Methods) if you wish to modify the parameteres after the creation of the Charting Library.

### symbol, interval

The default symbol & time interval of your chart. The `interval` value is described in the [Section](Resolution). *Mandatory*

### timeframe

Sets the default timeframe of the chart. Timeframe is a period of bars that will be loaded and shown on the screen.
Valid timeframe is a number with a letter D for days and M for months.

### container_id

`id` is an attribute of a DOM element that you wish to inlude in the widget. *Mandatory*

### datafeed

JavaScript object that implements the ([JS API](JS-Api)) interface to supply the chart with data. *Mandatory*

### timezone

Default timezone of the chart. The time on the timescale is displayed accodring to this timezone.
See the [list of supported timezones](Symbology#timezone) for available values. Set it to `exchange` to use the exchange timezone. Use the [overrides](#overrides) section if you wish to override the default value.

### debug

Setting this property to `true` will make the chart write detailed API logs into the browser console. Alternatively, you can use the `charting_library_debug_mode` featureset to enable it.

### library_path

A path to a `static` folder.

### width, height

The desired size of a widget. Please make sure that there is enough space for the widget to be displayed correctly.

**Remark**: If you want the chart to use all the available space use the `fullscreen` parameter instead of setting it to '100%'.

### fullscreen

*Default:* `false`

Boolean value showing whether the chart should use all the available space in the window.

### autosize

*Default:* `false`

Boolean value showing whether the chart should use all the available space in the container and resize when the container itself is resized.

### symbol_search_request_delay

A threshold delay in milliseconds that is used to reduce the number of symbol search requests when the user is typing a name of a symbol in the search box.

### auto_save_delay

A threshold delay in seconds that is used to reduce the number of `onAutoSaveNeeded` calls.

### toolbar_bg

Background color of the toolbars.

### study_count_limit

*Starting from version 1.5.*

Maximum amount of studies on the chart of a multichart layout. Minimum value is 2.

### studies_access

*Starting from version 1.1.*

An object with the following structure:

```javascript
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

* `type` is the list type. Supported values are: `black` (all listed items should be disabled), `white` (only the listed items should be enabled).
* `tools` is an array of objects. Each object could have the following properties:
  * `name` (*Mandatory*) is the name of a study. Use the same names as in the pop-ups of indicators.
  * `grayed` is a boolean showing whether this study should be visible but look as if it's disabled. If the study is grayed out and user clicks it, then the `onGrayedObjectClicked` function is called.

### drawings_access

*Starting from version 1.1.*

This property has the same structure as the `studies_access`argument that is described above. Use the same names as you see in the UI.

**Remark**: There is a special case for font-based drawings. Use the "Font Icons" name for them. Those drawings cannot be enabled or disabled separately - the entire group will have to be either enabled or disabled.

### saved_data

JS object containing saved chart content. Use this parameter when creating the widget if you have a saved chart already. If you want to load the chart content when the chart is initialized then use [load() method](https://github.com/tradingview/charting_library/wiki/Widget-Methods#loadstate) of the widget.

### locale

Locale to be used by Charting Library. See [Localization](Localization) section for details.

### numeric_formatting

The object containing formatting options for numbers. The only possible option is `decimal_sign` currently.
Example: `numeric_formatting: { decimal_sign: "," }`

### customFormatters

It is an object that contains the following fields:

1. timeFormatter
1. dateFormatter

You can use these formatters to adjust the display format of the date and time values.
Both values are objects that include functions such as `format` and `formatLocal`:

```javascript
function format(date)
function formatLocal(date)
```

These functions should return the text that specifies date or time. `formatLocal` should convert date and time to local timezone.

Example:

```javascript
customFormatters: {
  timeFormatter: {
    format: function(date) { var _format_str = '%h:%m'; return _format_str.replace('%h', date.getUTCHours(), 2). replace('%m', date.getUTCMinutes(), 2). replace('%s', date.getUTCSeconds(), 2); }
  },
  dateFormatter: {
    format: function(date) { return date.getUTCFullYear() + '/' + date.getUTCMonth() + '/' + date.getUTCDate(); }
  }
}
```

### overrides

The object that contains new values for default widget properties.
You can override most of the Charting Library properties (which also may be edited by user through UI) using `overrides` parameter of Widget constructor. `overrides` is supposed to be an object. The keys of this object are the names of overidden properties. The values of these keys are the new values of the properties. Example:

```javascript
overrides: {
    "symbolWatermarkProperties.color": "rgba(0, 0, 0, 0)"
}
```

This code will make the watermark 100% opaque (invisible). All customizable properties are listed in [separate article](Overrides). You can use [Drawings-Overrides](Drawings-Overrides) starting from v 1.5.

### disabled_features, enabled_features

The array containing names of features that should be enabled/disabled by default. `Feature` means part of the functionality of the chart (part of the UI/UX). Supported features are listed [here](Featuresets).

Example:

```javascript
TradingView.onready(function()
{
    var widget = new TradingView.widget({
        /* .... */
        disabled_features: ["header_widget", "left_toolbar"],
        enabled_features: ["move_logo_to_main_pane"]
    });
});
```

### snapshot_url

This URL is used to send a POST request with base64-encoded chart snapshots when a user presses the snapshot button. This endpoint should return the full URL of the saved image in the the response.

### indicators_file_name

Path to the file that contains your compiled indicators. See more details [here](Creating-Custom-Studies).

### preset

`preset` is a name of predefined widget settings. For now, the only value supported in the `preset` is  `mobile`. The example of this `preset` is [available here](https://demo_chart.tradingview.com/mobile_black.html).

### studies_overrides

Use this option to customize the style or inputs of the indicators. You can also customize the styles and inputs of the `Compare` series using this argument. See more details [here](Studies-Overrides)

### time_frames

List of visible timeframes that can be selected at the bottom of the chart.

Example:

```javascript
time_frames: [
    { text: "50y", resolution: "6M", description: "50 Years" },
    { text: "3y", resolution: "W", description: "3 Years", title: "3yr" },
    { text: "8m", resolution: "D", description: "8 Month" },
    { text: "3d", resolution: "5", description: "3 Days" },
    { text: "1000y", resolution: "W", description: "All", title: "All" },
]
```

Timeframe is an object containing the `text` and `resolution` properties. The `text` property should have the following format: `<integer><y|m|d>` ( \d+(y|m|d) as Regex ). Resolution is a string and its format is described here - [here](Resolution). See [this topic](Time-Frames) to learn more about timeframes.

The `description` property was added in v 1.7 and is displayed in the pop-up menu. This parameter is optional. If it isn't specified then the `title` or `text` property is used as a description.

The `title` property was added in v 1.9 and its value will override the default title generated based on the `text` property. This parameter is optional.

### charts_storage_url, client_id, user_id

These arguments are related to the high-level API for saving/loading the charts. See more details [here](Saving-and-Loading-Charts).

### charts_storage_api_version

A version of your backend. Supported values are: `"1.0"` | `"1.1"`. Study Templates are supported starting from version `"1.1"`.

### load_last_chart

Set this parameter to `true` if you want the library to load the last saved chart for a user (you should implement [save/load](Saving-and-Loading-Charts) first to make it work).

### theme

Adds custom theme color for the chart. Supported values are: `"Light"` | `"Dark"`.

### custom_css_url

*Starting from version 1.4.*

Adds your custom CSS to the chart. `url` should be an absolute or relative path to the 'static` folder.

### loading_screen

*Starting from version 1.12.*

Customization of the loading spinner. Value is an object with the following possible keys:

* `backgroundColor`
* `foregroundColor`

Example:

```javascript
loading_screen: { backgroundColor: "#000000" }
```

### favorites

Items that should be maked as favorite by default. This option requires that the usage of localstorage is disabled (see [featuresets](Featuresets) to know more). The `favorites` property is supposed to be an object. The following properties are supported:

* **intervals**: an array of time intervals that are marked as favorite. Example: `["D", "2D"]`
* **chartTypes**: an array of chart types that are marked as favorite. The names of chart types are identical to chart's UI in the English version. Example: `["Area", "Candles"]`.

### save_load_adapter

*Starting from version 1.12.*

An object containing the save/load functions. If it is available it should have the following methods:

**Chart layouts:**

 1. `getAllCharts(): Promise<ChartMetaInfo[]>`

    A function to get all saved charts.

    `ChartMetaInfo` is an object with the following fields:
     * `id` - unique ID of the chart.
     * `name` - name of the chart.
     * `symbol` - symbol of the chart.
     * `resolution` - resolution of the chart.
     * `timestamp` - last modified date (number of milliseconds since midnight `01/01/1970` UTC) of the chart.

 1. `removeChart(chartId): Promise<void>`

     A function to remove a chart. `chartId` is a unique ID of the chart (see `getAllCharts` above).

 1. `saveChart(chartData: ChartData): Promise<ChartId>`

     A function to save a chart.

    `ChartData` is an object with the following fields:
     * `id` - unique ID of the chart (may be `undefined` if it wasn't saved before).
     * `name` - name of the chart.
     * `symbol` - symbol of the chart.
     * `resolution` - resolution of the chart.
     * `content` - content of the chart.

    `ChartId` - unique ID of the chart (string)

 1. `getChartContent(chartId): Promise<ChartContent>`

     A function to load the chart from the server.

    `ChartContent` is a string with the chart content (see `ChartData::content` field in `saveChart` function).

**Study Templates:**

 1. `getAllStudyTemplates(): Promise<StudyTemplateMetaInfo[]>`

     A function to get all saved study templates.

    `StudyTemplateMetaInfo` is an object with the following fields:
     * `name` - name of the study template.

 1. `removeStudyTemplate(studyTemplateInfo: StudyTemplateMetaInfo): Promise<void>`

     A function to remove a study template.

 1. `saveStudyTemplate(studyTemplateData: StudyTemplateData): Promise<void>`

     A function to save a study template.

    `StudyTemplateData` is an object with the following fields:
     * `name` - name of the study template.
     * `content` - content of the study template.

 1. `getStudyTemplateContent(studyTemplateInfo: StudyTemplateMetaInfo): Promise<StudyTemplateContent>`

     A function to load a study template from the server.

    `StudyTemplateContent` - content of the study template (string)

 If both `charts_storage_url` and `save_load_adapter` are available  then `save_load_adapter` will be used.

 **IMPORTANT:** All functions should return a `Promise` (or `Promise`-like objects).

### settings_adapter

*Starting from version 1.11.*

An object that contains set/remove functions. Use it to save chart settings to your preferred storage (including server-side). If it is available then it should have the following methods:

1. `initialSettings: Object`

    An object with the initial settings

1. `setValue(key: string, value: string): void`

    A function that is called to store key/value pair.

1. `removeValue(key: string): void`

    A function that is called to remove a key.

## Trading Terminal only

### widgetbar

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

The object that contains settings for the widget panel on the right side of the chart. Watchlist, news and details widgets on the right side of the chart can be enabled using the `widgetbar` field in Widget constructor:

```javascript
widgetbar: {
    details: true,
    watchlist: true,
    watchlist_settings: {
        default_symbols: ["NYSE:AA", "NYSE:AAL", "NASDAQ:AAPL"],
        readonly: false
    }
}
```

* `details` (*default:* `false`): Enables details widget in the widget panel on the right.
* `watchlist` (*default:* `false`): Enables watchlist widget in the widget panel on the right.
* `watchlist_settings.default_symbols` (*default:* `[]`): Sets the list of default symbols for watchlist.
* `watchlist_settings.readonly` (*default:* `false`): Enables read-only mode for the watchlist.

### rss_news_feed

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

Use this property to change the RSS feed for news. You can set a different RSS for each symbol type or use a single RSS for all symbols. The object should have the `default` property, other properties are optional. The names of the properties match the symbol types. Each property is an object (or an array of objects) with the following properties:

1. `url` - is a URL to be requested. It can contain tags in curly brackets that will be replaced by the terminal: `{SYMBOL}`, `{TYPE}`, `{EXCHANGE}`.
1. `name` - is a name of the feed to be displayed underneath the news.

Here is an example:

```javascript
{
    "default": [ {
        url: "https://articlefeeds.nasdaq.com/nasdaq/symbols?symbol={SYMBOL}",
        name: "NASDAQ"
      }, {
        url: "http://feeds.finance.yahoo.com/rss/2.0/headline?s={SYMBOL}&region=US&lang=en-US",
        name: "Yahoo Finance"
      } ]
}
```

Another example:

```javascript
{
    "default": {
        url: "https://articlefeeds.nasdaq.com/nasdaq/symbols?symbol={SYMBOL}",
        name: "NASDAQ"
    }
}
```

One more example:

```javascript
{
    "default": {
        url: "https://articlefeeds.nasdaq.com/nasdaq/symbols?symbol={SYMBOL}",
        name: "NASDAQ"
     },
    "stock": {
        url: "http://feeds.finance.yahoo.com/rss/2.0/headline?s={SYMBOL}&region=US&lang=en-US",
        name: "Yahoo Finance"
    }
}
```

### news_provider

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

An object that specifies the news provider. It may contain the following properties:

1. `is_news_generic` - if set to `true` then the title of the news widget will not include a symbol name (`Headlines` will be included only). Otherwise `for SYMBOL_NAME` will be added.
1. `get_news` - use this property to set your own news getter function. Both the `symbol` and `callback` will be passed to the function.

    The callback function should be called with an array of news objects that have the following structure:

    * `title` (required) - the title of news item.
    * `published` (required) - the time of news item in ms (UTC).
    * `source` (optional) - source of the news item title.
    * `shortDescription` (optional) - Short description of a news item that will be displayed under the title.
    * `link` (optional) - URL to the news story.
    * `fullDescription` (optional) - full description (body) of a news item.

    **NOTE:** When a user clicks on a news item a new tab with the `link` URL will be opened. If `link` is not specified then a pop-up dialog with `fullDescription` will be shown.

    **NOTE 2:** If both `news_provider` and `rss_news_feed` are available then the `rss_news_feed` will be ignored.

Example:

```javascript
news_provider: {
    is_news_generic: true,
    get_news: function(symbol, callback) {
        callback([
            {
                title: 'News for symbol ' + symbol,
                shortDescription: 'Short description of the news item',
                fullDescription: 'Full description of the news item',
                published: new Date().valueOf(),
                source: 'My own source of news',
                link: 'https://www.tradingview.com/'
            },
            {
                title: 'Another news for symbol ' + symbol,
                shortDescription: 'Short description of the news item',
                fullDescription: 'Full description of the news item. Long text here.',
                published: new Date().valueOf(),
                source: 'My own source of news',
            }
        ]);
    }
}
```

### brokerFactory

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

Use this field to pass the function that returns a new object which implements [Broker API](Broker-API). This is a function that accepts [Trading Host](Trading-Host) and returns [Broker API](Broker-API).

### brokerConfig

:chart: *applies to [Trading Terminal](Trading-Terminal) only*

Use this field to set the configuration flags for the Trading Terminal. [Read more](Trading-Objects-and-Constants#configflags-object).

## See Also

* [Customization Overview](Customization-Overview)
* [Widget Methods](Widget-Methods)
* [Featuresets](Featuresets)
* [Saving and Loading Charts](Saving-and-Loading-Charts)
* [Overriding Default Properties of the Studies](Studies-Overrides)
* [Overriding Default Properties of the Chart](Overrides)
