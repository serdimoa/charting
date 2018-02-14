We do our best to make every next version fully compatible with the previous one, but sometimes big changes requires you to make minor changes in your code when upgrading to a new version.

_Note: you can check the Charting Library version by executing `TradingView.version()` in a browser console._

Here is the list of breaking changes:

<!-- markdownlint-disable no-emphasis-as-header -->

## Version 1.13

- Action `takeScreenshot` from [executeActionById](Chart-Methods#executeactionbyidactionid) method is removed. Use [takeScreenshot](Widget-Methods#takescreenshot) method instead.
- Featureset `caption_buttons_text_if_possible` is enabled by default.

## Version 1.12

**Charting Library**

- `charting_library/charting_library.min.js` is now [UMD](https://github.com/umdjs/umd) module.

    If you only inline this script into HTML then nothing has changed for you.
    If you import it as a module you should import `widget`, `version` and `onready` functions directly from it.

- `searchSymbolsByName` is removed from `JS-API`, use `searchSymbols` instead.

Study overrides:

- Overrides for `Overlay` should be applied only via `studies_overrides` (and `applyStudiesOverrides` in runtime). In the previous versions you had to use `overrides` and `applyOverrides`). See [Studies-Overrides](Studies-Overrides) page.
- Starting from this version you are no longer able to override `showStudyArguments` and `showLastValue` using `options` keyword.

**Trading Terminal**

- `hasHistory` flag is removed. Use `historyColumns` to display History in the Account Manager.

The following items are still supported in the Trading Terminal, but will be deprecated in future versions:

- `subscribePL` and `unsubscribePL`. The broker should call `plUpdate` method of the Host every time the profit is changed.
- `supportDOM` is removed. DOM widget visibility can be set using `dome_widget` featureset.

**The Trading Controller is replaced with [Broker API](Broker-API)**.

The following changes should be applied to your Trading Controller implementation to move to new Broker API:

- Method `setHost` is removed. Host should be passed to the constructor of Broker API.
- Method `buttonDropdownItems` is removed. Broker API should update [Broker API](Trading-Host) using `setButtonDropdownActions`.
- Methods `configFlags` and `durations` are removed. Use [Widget Constructor](Widget-Constructor) fields instead.
- All methods that returned `$.Deferred` should be modified to return Promise.
- Method `chartContextMenuItems` is renamed to `chartContextMenuActions`.
- Method `isTradable` changed to return a Promise instead of a Boolean value.
- All string constants ("working", "buy" etc.) should be replaced with the appropriate number of constants.
- Position `avg_price` renamed to `avgPrice`.
- `tradingController` field in the [Widget Constructor](Widget-Constructor) is removed. Use `brokerFactory` instead.

## Version 1.11

**Trading Terminal**

The following items are still supported in Trading Terminal, but will be deprecated in future versions:

- `supportDOME` renamed to `supportDOM`.
- The signature of `showClosePositionDialog has been changed.
- `showEditBracketsDialog` was renamed to `showPositionBracketsDialog`.The signature has been changed.

## Version 1.10

- Default behavior of Volume indicator was changed.

    **Previous behavior**: Volume indicator is added/removed when an instrument or a resolution is switched depending on volume support by the instrument. You can get back to this behavior by disabling `create_volume_indicator_by_default_once` featureset.

    **New behavior**: Volume indicator is added when an empty chart is loaded for the first time provided that it is supported by an active instrument.

## Version 1.9

- We don't compile Pine scripts anymore. You can still use scripts that were compiled earlier.

## Version 1.8 of Trading Terminal

- The chart can no longer show active orders only. Appropriate methods have been removed.
- `showOrderDialog` receives an object instead of arguments list
- `showSampleOrderDialog` removed, use [showOrderDialog](Trading-Host#showorderdialogorder-handler-focus-promise) instead
- `showOrderDialog` removed from [Broker API](Broker-API), use `placeOrder` and `modifyOrder` receive `silently` argument instead
- `reversePosition`, `closePosition`, `cancelOrder` have an additional argument `silently`.

## Version 1.7

- Starting from this version calling `setSymbol` with the same symbol is no longer enough. You should call `onResetCacheNeededCallback` from `subscribeBars` first. Then you can use `setSymbol` or new `resetData` method of the chart.
- JSAPI protocol version 1 is not supported anymore. `nextTime` and `noData` must be provided.

## Version 1.5

- Added `source` argument to MACD. You should change MACD creation code to pass `source` also.

    `chartWidget.chart().createStudy('MACD', false, false, [12, 26, "close", 9])`

## Version 1.4

- Override `transparency` is not supported anymore. We added transparency support to every color. Use `rgba` form to define a color with transparency. Example:

    `"symbolWatermarkProperties.color" : "rgba(60, 70, 80, 0.05)"`

## Version 1.3

- Override `paneProperties.gridProperties.*` is not supported anymore.

    Please use `paneProperties.vertGridProperties.*` and `paneProperties.horzGridProperties.*`

- Override `mainSeriesProperties.candleStyle.wickColor` is not supported anymore.

    Use `mainSeriesProperties.candleStyle.wickUpColor` and `mainSeriesProperties.candleStyle.wickDownColor`

<!-- markdownlint-enable no-emphasis-as-header -->
