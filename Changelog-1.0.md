#1.0

* Widget.disabled_studies parameter implemented
* Widget.fullscreen parameter implemented
* FIXED: Pop-up symbol search dialog is broken #92
* IMPLEMENTED: Alter UDF getMarks call response format #88
* IMPLEMENTED: Implement an ability to hide drawings sidebar #81
* IMPLEMENTED: Add `resolution` argument to getMarks API call / UDF request #86
* FIXED: Resolutions fallback is broken for intraday resolutions #91
* Dialogs dragging constrains implemented #79, #56
* IMPLEMENTED: Implement end-of-history situation support #90
* TT: IMPLEMENTED: Quotes support (watchlist, details and datawindow)
* User's chart settings (colors e.t.c.) now are stored in LocalStorage
* Widget.setLanguage() call implemented
* FIXED: Rounding volume of bars to integer numbers #54
* IMPLEMENTED: Implement UDF data request frequency customization #80
* IMPLEMENTED: An ability to override screenshots storage URL #77
* FIXED: `Volume` study scaling issue #78
* FIXED: TypeError: Cannot set property 'innerHTML' of null when calling TradingView.Widget.load(state) #49
* FIXED: Incorrect render interval combobox after chart reload #53
* IMPLEMENTED: Implement UI timeframe selector #30
* FIXED: Add/Compare unsubscribe error #75
* FIXED: Switching to different type of chart still passes exchange:symbol instead of ticker for the resolving query #76
* FIXED: Multiple charts on the page issue #72
* IMPLEMENTED: Pivot point support #40
* IMPLEMENTED: Requesting more data when scrolling in history #67
* `Localization` section updated
* FIXED: Add/compare data request issue #70
* FIXED: Volume doesn't show after chart initialized #71
* Symbol Search behaviour improved
* IMPLEMENTED: Add GMT+7 timezone #48
* IMPLEMENTED: has_empty_bars and force_session_rebuild flags
* IMPLEMENTED: #38, Implement Widget.onIntervalChange() call
* IMPLEMENTED: #35, Symbol Search input validation
* IMPLEMENTED: #37, Hiding Symbol search field
* IMPLEMENTED: #44, Drawing vertical lines
* FIXED: symbol GET parameter parsing removed (#51)
* FIXED: #46, Chart cannot be scrolled to the most recent bar after a lot of realtime bars created.
* FIXED: #45, Overlay the main chart option is not working.
* FIXED: #36, Ampersand in queries.
* FIXED: #31, Missing files.
* IMPLEMENTED: #47, Implement `getMarks` call through UDF.
* FIXED: #33, Building D resolution from 15 min.
* FIXED: #23, SymbolInfo format changed (+`supported_resolutions`, see details).
* FIXED:  SymbolInfo.has_no_volume is not working #14.
* `dev` branch created on GitHub instead of `1.0`.

