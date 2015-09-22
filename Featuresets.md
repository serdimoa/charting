`Feature` or `featureset` means a part of chart's functionality. There are simple (atomic) and complex (composite) features. Composite feature consists of simple features. Disabling composite feature makes all of its simple parts to be disabled. Supported features are listed below.

### Visibility of controls and other visual elements

| ID                                      | Default State | Description                                                |
|-----------------------------------------|---------------|------------------------------------------------------------|
| **header_widget**                       | on            |                                                            |
| - header_widget_dom_node                | on            | disabling this feature hides the header widget DOM element |
| - header_symbol_search                  | on            |                                                            |
| - header_resolutions                    | on            |                                                            |
| - - header_interval_dialog_button       | on            |                                                            |
| - - - show_interval_dialog_on_key_press | on            |                                                            |
| - header_chart_type                     | on            |                                                            |
| - header_settings                       | on            | relates to Chart Properties button                         |
| - header_indicators                     | on            |                                                            |
| - header_compare                        | on            |                                                            |
| - header_undo_redo                      | on            |                                                            |
| - header_screenshot                     | on            |                                                            |
| - header_fullsfcreen_button             | on            |                                                            |
| border_around_the_chart                 | on            |                                                            |
| header_saveload                         | on            | yep, it's not a part of `header_widget`                    |
| left_toolbar                            | on            |                                                            |
| control_bar                             | on            | relates to the navigation buttons at the bottom            |
| timeframes_toolbar                      | on            |                                                            |
| edit_buttons_in_legend                  | on            |                                                            |
| **context_menus**                       | on            |                                                            |
| - pane_context_menu                     | on            |                                                            |
| - scales_context_menu                   | on            |                                                            |
| - legend_context_menu                   | on            |                                                            |
| display_market_status                   | on            |                                                            |
| remove_library_container_border         | off           |                                                            |

### Elements placement

| ID                           | Default State | Description                                                        |
|------------------------------|---------------|--------------------------------------------------------------------|
| move_logo_to_main_pane       | off           | Places the logo on the main series pane instead of the bottom pane |
| header_saveload_to_the_right | off           | Moves Save and Load buttons to the right                           |

### Behavior

| ID	| Default State	| Description
|-------|---------------|------------
| **use_localstorage_for_settings**	| on	| allows to save user settings to the local storage
| - items_favoriting	| on	| disabling this feature hides all "Favorite this item" buttons
| - save_chart_properties_to_local_storage	| on	| disabling this feature prevents saving of chart properties (colors, styles, fonts) to the local storage, but still keeps saving of favorite items
| create_volume_indicator_by_default	| on	| 
| volume_force_overlay	| on	| places Volume indicator on the same pane with the main series
| right_bar_stays_on_scroll	| on	| determines zoom behavior: bar under cursor is kept if disabled
| constraint_dialogs_movement	| on	| prevents moving dialogs out of the chart
| charting_library_debug_mode	| off	| enables logs
| narrow_chart_enabled	| off	| allows to shrink the chart up to 240px horizontally
| show_dialog_on_snapshot_ready	| on	| disabling this feature allows you to make a snapshot silently
| study_market_minimized	| on	| relates to Indicators dialog, determines whether it is compact or contains a search bar and categories
| side_toolbar_in_fullscreen_mode	| off	| using this feature you can enable Drawings Toolbar in fullscreen mode
| adapt_onchart_logo_background	| on	| logo background color is changed to match the background

### "Big Rocks"

| ID	| Default State	| Description
|-------|---------------|------------
| study_templates | off |
| datasource_copypaste | on | enables copying of drawings and studies


## Trading Terminal

| ID	| Default State	| Description
|-------|---------------|------------
| trading_options | on | related to the Properties dialog