`Feature` or `featureset` is a string literal that can be used to change the functionality of the chart. There are simple (atomic) and complex (composite) features. Composite feature consists of simple features. Disabling composite feature disables all of its simple parts as well. Supported features are listed below.

Please note that the leading `-` characters are not part of the featureset name in the table below.

### Visibility of controls and other visual elements

[**Interactive Map of Featuresets**](http://tradingview.github.io/featuresets.html)

| ID                                      | Default State | Library Version | Description                                                |
|-----------------------------------------|---------------|-----------------|-------------|
| **header_widget**                       | on            |                 |                                                            |
| - header_widget_dom_node                | on            |                 | Disabling this feature hides the header widget DOM element |
| - header_symbol_search                  | on            |                 |                                                            |
| - symbol_search_hot_key                 | on            |       1.9       | Symbol search by pressing any key                               |
| - header_resolutions                    | on            |                 |                                                            |
| - - header_interval_dialog_button       | on            |                 |                                                            |
| - - - show_interval_dialog_on_key_press | on            |                 |                                                            |
| - header_chart_type                     | on            |                 |                                                            |
| - header_settings                       | on            |                 | Relates to Chart Properties button                         |
| - header_indicators                     | on            |                 |                                                            |
| - header_compare                        | on            |                 |                                                            |
| - header_undo_redo                      | on            |                 |                                                            |
| - header_screenshot                     | on            |                 |                                                            |
| - header_fullscreen_button              | on            |                 |                                                            |
| compare_symbol                          | on            |       1.5       | You can remove Compare/Overlay dialog from context menus using this featureset |
| border_around_the_chart                 | on            |                 |                                                            |
| header_saveload                         | on            |                 | Hides save/load buttons (the feature is not part of `header_widget` featureset)                    |
| left_toolbar                            | on            |                 |                                                            |
| control_bar                             | on            |                 | Relates to the navigation buttons at the bottom of the chart            |
| timeframes_toolbar                      | on            |                 |                                                            |
| **edit_buttons_in_legend**              | on            |                 |                                                            |
| - show_hide_button_in_legend            | on            |       1.7       |                                                            |
| - format_button_in_legend               | on            |       1.7       |                                                            |
| - study_buttons_in_legend               | on            |       1.7       |                                                            |
| - delete_button_in_legend               | on            |       1.7       |                                                            |
| **context_menus**                       | on            |                 |                                                            |
| - pane_context_menu                     | on            |                 |                                                            |
| - scales_context_menu                   | on            |                 |                                                            |
| - legend_context_menu                   | on            |                 |                                                            |
| main_series_scale_menu                  | on            |       1.7       | Displays the settings button in the bottom right corner of the chart |
| display_market_status                   | on            |                 |                                                            |
| remove_library_container_border         | on            |                 |                                           |
| chart_property_page_style               | on            |                 |                                           |
| property_pages                          | on            | 1.11            | Disables all property pages                |
| show_chart_property_page                | on            | 1.6             | Turning this feature off disables Properties        |
| chart_property_page_scales              | on            |                 |                                           |
| chart_property_page_background          | on            |                 |                                           |
| chart_property_page_timezone_sessions   | on            |                 |                                           |
| chart_property_page_trading             | on            |                 | This feature is for the Trading Terminal only           |
| countdown                               | on            | 1.4             | Displays a countdown label on a price scale         |
| caption_buttons_text_if_possible        | off           | 1.4             | Shows text instead of icons on the Indicators and Compare buttons in the header when possible|
| dont_show_boolean_study_arguments       | off           | 1.4             | Hides true/false study arguments |
| hide_last_na_study_output               | off           | 1.4             | Hides last n/a study output data        |
| symbol_info                             | on            | 1.5             | Enables the symbol info dialog                 |
| timezone_menu                           | on            | 1.5             | Disables timezone context menu    |
| snapshot_trading_drawings               | off           | 1.6             | Includes orders/positions/executions in the screenshot |
| source_selection_markers                | on            | 1.11            | Disables selection markers for series and indicators |
| keep_left_toolbar_visible_on_small_screens | off            | 1.11            | Prevents left toolbar from disappearing on small screens |
| go_to_date | on            | 1.11            | Allows you to jump to a particular bar using 'Go to' dialog |
| adaptive_logo | on            | 1.11            | Allows you to hide 'charts by TradingView' text on small-screen devices |
| show_dom_first_time                      | off           | 1.12            | Shows DOM panel when a user opens the Chart for the first time |
| hide_left_toolbar_by_default             | off           | 1.12            | Hides left toolbar when a user opens the Chart for the first time |

### Elements placement

| ID                           | Default State | Library Version | Description                                                        |
|------------------------------|---------------|-----------------|--------------------------------------------------------------------|
| move_logo_to_main_pane       | off           |                 | Places the logo on the main series pane instead of the bottom pane |

### Behavior

| ID  | Default State | Library Version | Description
|-------|---------------|-----------------|------------
| **use_localstorage_for_settings** | on |                 | Allows storing all properties (including favorites) to the localstorage
| - items_favoriting | on |                 | Disabling this feature hides all "Favorite this item" buttons
| - save_chart_properties_to_local_storage | on |                 |  Can be disabled to forbid storing chart properties to the localstorage while allowing to save other properties. The other properties are favorites in the Charting Library and Watchlist symbols and some panels states in the Trading Terminal.
| create_volume_indicator_by_default | on |                 |
| create_volume_indicator_by_default_once | on |                  |
| volume_force_overlay | on |                 | Places Volume indicator on the same pane with the main series
| right_bar_stays_on_scroll | on |                 | Determines the behavior of Zoom feature: bar under the mouse cursor stays in the same place if this feature is disabled
| constraint_dialogs_movement | on |                 | Keeps the dialogs within the chart
| charting_library_debug_mode | off |                 | Enables logs
| show_dialog_on_snapshot_ready | on |                 | Disabling this feature allows you to make a snapshot silently
| study_market_minimized | on |                 | Relates to Indicators dialog and determines whether it is compact or contains a search bar along with the categories
| study_dialog_search_control   | on    | 1.6             | Displays the search control in the indicators dialog
| side_toolbar_in_fullscreen_mode | off |                 | This enables Drawings Toolbar in the fullscreen mode
| same_data_requery             | off   |                 | Allows you to call `setSymbol` with the same symbol to refresh the data
| disable_resolution_rebuild    | off   |                 | Shows bar time exactly as provided by the data feed with no adjustments.
| chart_scroll                  | on    |   1.10          | Allows chart scrolling
| chart_zoom                    | on    |   1.10          | Allows chart zooming
| high_density_bars             | off   |   1.11          | Allows zooming out to show more than 60000 bars on a single screen
| cl_feed_return_all_data       | off   |   1.11          | Allows you to return more bars from the data feed than requested and displays it on a chart simultaneously
| uppercase_instrument_names    | on    | 1.12            | Disabling this feature allows a user to enter case-sensitive symbols

### "Important features"

| ID | Default State | Library Version | Description
|-------|---------------|-----------------|------------
| study_templates | off | |
| datasource_copypaste | on | | Enables copying of drawings and studies
| seconds_resolution| off | 1.4 | Enables the support of resolutions that start from 1 second

## :chart: Trading Terminal

| ID | Default State | Terminal Version | Description
|-------|---------------|------------------|------------
| support_multicharts | on | | Enables context menu actions (Clone, Sync) related to Multiple Chart Layout
| header_layouttoggle | on | | Shows the Select Layout button in the header
| show_logo_on_all_charts | off | |Shows the logo on every single chart of the multichart layout
| chart_crosshair_menu    | on  | 1.7 |Enables the "plus" button on the price scale for quick trading
| add_to_watchlist | on | 1.9 | Enables "Add symbol to Watchlist" item in the menu
| footer_screenshot | on | 1.11 | Shows a screenshot button in the footer (Account Manager)
| open_account_manager | on | 1.11 | Keeps the Account Manager opened by default
| trading_notifications | on | 1.11 | Shows trading notifications on the chart
| multiple_watchlists | on | 1.12 | Enables creating of multiple watchlists
| show_trading_notifications_history | on | 1.13 | Enables the Notifications Log tab in the bottom panel
