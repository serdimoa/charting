Here is a list of supported widget's methods. You can call them using widget object returned to you by widget's constructor.

**Remark**: Please note that it's safe to call any method only **after** onChartReady callback is fired.
So the common practice is to do something like

```javascript
widget.onChartReady(function() {
    // now it's safe to call any other widget's methods
});
```

## Methods

**Before 1.5 [Chart Methods](Chart-Methods.md) belonged to the Widget. Please see the full list of actions [here](Chart-Methods.md).**

* [Subscribing To Chart Events](#subscribing-to-chart-events)
  * [onChartReady(callback)](#onchartreadycallback)
  * [onGrayedObjectClicked(callback)](#ongrayedobjectclickedcallback)
  * [onShortcut(shortcut, callback)](#onshortcutshortcut-callback)
  * [subscribe(event, callback)](#subscribeevent-callback)
  * [unsubscribe(event, callback)](#unsubscribeevent-callback)
* [Chart Actions](#chart-actions)
  * [chart()](#chart)
  * [setLanguage(locale)](#setlanguagelocale)
  * [setSymbol(symbol, interval, callback)](#setsymbolsymbol-interval-callback)
  * [remove()](#remove)
  * [closePopupsAndDialogs()](#closepopupsanddialogs)
  * [selectLineTool(drawingId)](#selectlinetooldrawingid)
  * [selectedLineTool()](#selectedlinetool)
  * [takeScreenshot()](#takescreenshot)
* [Saving/Loading Charts](#savingloading-charts)
  * [save(callback)](#savecallback)
  * [load(state)](#loadstate)
  * [getSavedCharts(callback)](#getsavedchartscallback)
  * [loadChartFromServer(chartRecord)](#loadchartfromserverchartrecord)
  * [saveChartToServer(onCompleteCallback, onFailCallback, saveAsSnapshot, options)](#savecharttoserveroncompletecallback-onfailcallback-saveassnapshot-options)
  * [removeChartFromServer(chartId, onCompleteCallback)](#removechartfromserverchartid-oncompletecallback)
* [Custom UI Controls](#custom-ui-controls)
  * [onContextMenu(callback)](#oncontextmenucallback)
  * [createButton(options)](#createbuttonoptions)
* [Dialogs](#dialogs)
  * [showNoticeDialog(params)](#shownoticedialogparams)
  * [showConfirmDialog(params)](#showconfirmdialogparams)
  * [showLoadChartDialog()](#showloadchartdialog)
  * [showSaveAsChartDialog()](#showsaveaschartdialog)
* [Getters](#getters)
  * [symbolInterval(callback)](#symbolintervalcallback)
  * [mainSeriesPriceFormatter()](#mainseriespriceformatter)
  * [getIntervals()](#getintervals)
  * [getStudiesList()](#getstudieslist)
* [Customization](#customization)
  * [addCustomCSSFile(url)](#addcustomcssfileurl)
  * [applyOverrides(overrides)](#applyoverridesoverrides)
  * [applyStudiesOverrides(overrides)](#applystudiesoverridesoverrides)
* :chart: [Trading Terminal specific](#chart-trading-terminal-specific)
  * [watchList()](#chart-watchlist)
* :chart: [Multiple Charts Layout](#chart-multiple-charts-layout)
  * [chart(index)](#chart-chartindex)
  * [activeChart()](#chart-activechart)
  * [chartsCount()](#chart-chartscount)
  * [layout()](#chart-layout)
  * [setLayout(layout)](#chart-setlayoutlayout)

## Subscribing To Chart Events

### onChartReady(callback)

1. `callback`: function()

The Charting Library will call the callback provided once when chart is initialized and ready.
You can safely call all other methods from this moment.

### onGrayedObjectClicked(callback)

1. `callback`: function(subject)
    1. `subject`: object `{type, name}`
        * `type`: `drawing` | `study`
        * `name`: string, name of a subject which was clicked

The Library will call the callback provided every time when user clicks on grayed out object.

Example:

```javascript
new TradingView.widget({
    drawings_access: {
        type: "black",
        tools: [
            { name: "Regression Trend" },
            { name: "Trend Angle", grayed: true },
        ]
    },
    studies_access: {
        type: "black",
        tools: [
            { name: "Aroon" },
            { name: "Balance of Power", grayed: true },
        ]
    },
    <...> // other widget settings
});

widget.onChartReady(function() {
    widget.onGrayedObjectClicked(function(data) {
        // this function will be called when one tries to
        // create Balance Of Power study or Trend Angle shape

        alert(data.name + " is grayed out!");
    })
});
```

### onShortcut(shortcut, callback)

1. `shortcut`
1. `callback`: function(data)

The Library will call the callback provided every time when shortcut is pressed.

Example:

```javascript
widget.onShortcut("alt+s", function() {
  widget.executeActionById("symbolSearch");
});
```

### subscribe(event, callback)

1. `event`: can be

| Event name | Library Version | Description |
|------------|-----------------|-------------|
| `toggle_sidebar` | | drawing toolbar is shown/hidden |
| `indicators_dialog` | | Indicators dialog is shown |
| `toggle_header` | | chart header is shown/hidden |
| `edit_object_dialog` | | Chart/Study Properties dialog is shown |
| `chart_load_requested` | | new chart about to be loaded |
| `chart_loaded` | | |
| `mouse_down` | | |
| `mouse_up` | | |
| `drawing` | 1.7 | a drawing is added to a chart. Arguments contains an object with  `value` field that is the name of the drawing. |
| `study` | 1.7 | an indicator is added to a chart. Arguments contains an object with  `value` field that is the name of the indicator. |
| `undo` | 1.7 | |
| `redo` | 1.7 | |
| `reset_scales` | 1.7 | reset scales button is clicked |
| `compare_add` | 1.7 | Compare dialog is shown |
| `add_compare` | 1.7 | Compare instrument is added |
| `load_study` template | 1.7 | A study template is loaded |
| `onTick` | | callback will be called every time when recent bar updates |
| `onAutoSaveNeeded` | | callback will be called every time when user changes the chart. `Chart change` means any user action that can be undone. The callback will not be called more than once in five seconds. See also [auto_save_delay](Widget-Constructor.md#auto_save_delay) |
| `onScreenshotReady` | | callback will be called every time when user creates a screenshot and server returns the created image name |
| `onMarkClick` | | callback will be called every time when user clicks a [mark on bar](Marks-On-Bars.md). Mark ID will be passed as an argument |
| `onTimescaleMarkClick` | | callback will be called every time when user clicks a timescale mark. Mark ID will be passed as an argument |
| `onSelectedLineToolChanged` | | callback will be called every time when selected line tool changes |
| :chart: `layout_about_to_be_changed` | | amount or placement of charts about to be changed |
| :chart: `layout_changed` | | amount or placement of charts is changed |
| :chart: `activeChartChanged` | | active chart is changed |

1. `callback`: function(arguments)

The library will call `callback` when GUI `event` is happened.
Every event can have different set of arguments.

### unsubscribe(event, callback)

Unsubscribe a previously subscribed `callback` listener from a given `event` (which is one of the events in the table above).

## Chart Actions

### chart()

Returns a chart object that you can use to call [Chart-Methods](Chart-Methods.md)

### setLanguage(locale)

1. `locale`: [language code](Localization.md)

Sets the Widget's language. For now, this call reloads the chart. **Please avoid using it**.

### setSymbol(symbol, interval, callback)

1. `symbol`: string
1. `interval`: string
1. `callback`: function()

Makes the chart to change its symbol and resolution. Callback is called after new symbol's data arrived.

### remove()

Removes chart widget from your page.

### closePopupsAndDialogs()

Calling this method closes a context menu or a dialog if it is shown.

### selectLineTool(drawingId)

1. `drawingId`: may be one of the [identifiers](Shapes-and-Overrides.md) or
    1. `cursor`
    1. `dot`
    1. `arrow_cursor`
    1. `eraser`
    1. `measure`
    1. `zoom`
    1. `brush`

Selection of a drawing or cursor which is identical to a single click on a drawing button.

### selectedLineTool()

Returns an [identifier](Shapes-and-Overrides.md) of the selected drawing or cursor (see above).

### takeScreenshot()

This method creates a snapshot of the chart and uploads it to the server.
When it is done the [onScreenshotReady](#subscribeevent-callback) callback function is called.
It includes the URL to the snapshot.

## Saving/Loading Charts

### save(callback)

1. `callback`: function(object)

Saves the chart state to JS object. Charting Library will call your callback and pass the state object as argument.

This call is a part of low-level [save/load API](Saving-and-Loading-Charts.md).

### load(state)

1. `state`: object

Loads the chart from state object. This call is a part of low-level [save/load API](Saving-and-Loading-Charts.md).

### getSavedCharts(callback)

1. `callback`: function(objects)

`objects` is an array of:

* `id`
* `name`
* `image_url`
* `modified_iso`
* `short_symbol`
* `interval`

Returns a list of chart descriptions saved on a server for current user.

### loadChartFromServer(chartRecord)

1. `chartRecord` is an object that you get using [getSavedCharts(callback)](#getsavedchartscallback)

Loads and displays a chart from a server.

### saveChartToServer(onCompleteCallback, onFailCallback, saveAsSnapshot, options)

1. `onCompleteCallback`: function()
1. `onFailCallback`: function()
1. `saveAsSnapshot`: should be always `false`
1. `options`: object `{ chartName }`
    * `chartName`: name of a chart. Should be specified for new charts and renaming.
    * `defaultChartName`: default name of a chart. It will be used if current chart has no name.

Saves current chart to the server.

### removeChartFromServer(chartId, onCompleteCallback)

1. `chartId`: `id` should be got from a record received using [getSavedCharts(callback)](#getsavedchartscallback)
1. `onCompleteCallback`: function()

Removes chart from the server.

## Custom UI Controls

### onContextMenu(callback)

1. `callback`: function(unixtime, price).
    This callback is expected to return a value (see below).

The Library will call the callback provided every time when user opens context menu on the chart.
Unix time and price of context menu point will be provided as arguments.
To customize context menu items you have to return array of items descriptors.

Item descriptor has following structure:

```javascript
{
    position: 'top' | 'bottom',
    text: 'Menu item text',
    click: <onItemClicked callback>
}
```

* `position`: position of item in context menu
* `text`: text of menu item
* `click`: callback which will be called when user select your menu item

To add a separator use minus sign. Example: `{ text: "-", position: "top" }`.

To remove an existing item from a menu use minus sign in front of the item text.
Example: `{ text: "-Objects Tree..." }`

Example:

```javascript
widget.onChartReady(function() {
    widget.onContextMenu(function(unixtime, price) {
        return [{
            position: "top",
            text: "First top menu item, time: " + unixtime + ", price: " + price,
            click: function() { alert("First clicked."); }
        },
        { text: "-", position: "top" },
        { text: "-Objects Tree..." },
        {
            position: "top",
            text: "Second top menu item 2",
            click: function() { alert("Second clicked."); }
        }, {
            position: "bottom",
            text: "Bottom menu item",
            click: function() { alert("Third clicked."); }
        }];
    });
```

### createButton(options)

1. `options`: object `{ align: "left" }`
    * `align`: `right` | `left`. default: `left`

Creates a new DOM element in chart top toolbar and returns **jQuery object** for this button.
You can use it to append custom controls right on the chart.

Example:

```javascript
widget.onChartReady(function() {
    widget.createButton()
        .attr('title', "My custom button tooltip")
        .on('click', function (e) { alert("My custom button pressed!"); })
        .append($('<span>My custom button caption</span>'));
});
```

## Dialogs

*Since version 1.6.*

### showNoticeDialog(params)

1. `params`: object:
    * `title`: text to be shown in the title
    * `body`: text to be shown in the body
    * `callback`: function to be called when ok button is pressed

This method shows a dialog with custom title and text and "OK" button.

### showConfirmDialog(params)

1. `params`: object:
    * `title`: text to be shown in the title
    * `body`: text to be shown in the body
    * `callback(result)`: function to be called when ok button is pressed.
        `result` is `true` if `OK` is pressed, otherwise it is `false`.

This method shows a dialog with custom title and text and "OK", "CANCEL" buttons.

### showLoadChartDialog()

Displays Load chart dialog.

### showSaveAsChartDialog()

Displays Save As... chart dialog.

## Getters

### symbolInterval(callback)

1. `callback`: function(result)
    1. `result`: object `{symbol, interval}`

**Since 1.4 the function returns the result immediately. Callback is kept for compatibility.**

Charting Library will call your callback with an object containing chart's symbol and interval.

### mainSeriesPriceFormatter()

Returns object with method `format` that you can use to format prices. Introduced in 1.5.

### getIntervals()

Returns an array of supported resolutions. Introduced in 1.7.

### getStudiesList()

Returns an array of all studies ids. They can be used to create a study.

## Customization

### addCustomCSSFile(url)

1. `url` should be absolute or relative path to 'static` folder

This method was introduced in version `1.3`.

Starting from `1.4` use [custom_css_url](Widget-Constructor.md#custom_css_url) instead.

### applyOverrides(overrides)

*Since version 1.5.*

1. `overrides` is an object.
    It is the same as [overrides](Widget-Constructor.md#overrides) in Widget Constructor.

This method applies overrides to properties without reloading the chart.

### applyStudiesOverrides(overrides)

*Since version 1.9.*

1. `overrides` is an object. It is the same as [studies_overrides](Widget-Constructor.md#studies_overrides) in Widget Constructor.

This method applies studies overrides to indicators' style or inputs without reloading the chart.

## :chart: Trading Terminal specific

The following methods are available in [Trading Terminal](Trading-Terminal.md) only.

### :chart: watchList()

*Since version 1.9.*

Returns an object to manipulate the watchlist. The object has the following methods:

1. `defaultList()` - allows you to get a default list of symbols.

1. `getList(id?: string)` - allows you to get a list of the symbols. If parameter `id` is not supplied, the current list will be returned. If there is no WatchList `null` will be returned.

1. `getActiveListId()` - allows you to get id of the current list. If there is no WatchList `null` will be returned.

1. `getAllLists()` - allows you to get all lists. If there is no WatchList `null` will be returned.

1. `setList(symbols: string[])` - allows you to set a list of symbols into the watchlist. It will replace the whole list. **Obsolete. Will be removed in version `1.13`. Use `updateList` instead.**

1. `updateList(listId: string, symbols: string[])` - allows you to edit list of the symbols.

1. `renameList(listId: string, newName: string)` - allows you to rename the list to `newName`.

1. `createList(listName?: string, symbols?: string[])` - allows you to create a list of symbols with `listName` name. If parameter `listName` is not supplied or there is no WatchList `null` will be returned;

1. `saveList(list: SymbolList)` - allows you to save a list of symbols, where `list` is an object with the next keys:

  ```js
  id: string;
  title: string;
  symbols: string[];
  ```
If there is no WatchList or an equivalent list already exists `false` will be returned, otherwise `true`.

1. `deleteList(listId: string)` - allows you to delete a list of symbols.

1. `onListChanged()` - you can subscribe using [[Subscription]] object returned by this function to be notified when the active watchlist's symbols are changed and unsubscribe from the event.

1. `onActiveListChanged()` - you can subscribe using [[Subscription]] object returned by this function to be notified when the active watchlist is changed and unsubscribe from the event.

1. `onListAdded()` - you can subscribe using [[Subscription]] object returned by this function to be notified when the new watchlist is added and unsubscribe from the event.

1. `onListRemoved()` - you can subscribe using [[Subscription]] object returned by this function to be notified when the watchlist is removed and unsubscribe from the event.

1. `onListRenamed()` - you can subscribe using [[Subscription]] object returned by this function to be notified when the watchlist is renamed and unsubscribe from the event.

## :chart: Multiple Charts Layout

### :chart: chart(index)

1. `index`: index of a chart starting from `0`. `index` is `0` by default.

Returns a chart object that you can use to call [Chart-Methods](Chart-Methods.md)

### :chart: activeChart()

Returns current active chart object that you can use to call [Chart-Methods](Chart-Methods.md)

### :chart: chartsCount()

Returns amount of charts in the current layout

### :chart: layout()

Returns current layout mode. Possible values: `4`, `6`, `8`, `s`, `2h`, `2-1`, `2v`, `3h`, `3v`, `3s`.

### :chart: setLayout(layout)

1. `layout`: Possible values: `4`, `6`, `8`, `s`, `2h`, `2-1`, `2v`, `3h`, `3v`, `3s`.

Changes current chart layout.

## See Also

* [Chart-Methods](Chart-Methods.md)
* [Customization Overview](Customization-Overview.md)
* [Widget Constructor](Widget-Constructor.md)
* [Saving and Loading Charts](Saving-and-Loading-Charts.md)
* [Overriding Studies' Defaults](Studies-Overrides.md)
* [Overriding Chart's Defaults](Overrides.md)
