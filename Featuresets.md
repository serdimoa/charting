`Feature` or `featureset` means a part of chart's functionality. There are simple (atomic) and complex (composite) features. Composite feature consists of simple features. Disabling composite feature makes all of its simple parts to be disabled. Supported features are listed below. Default state (on/off) shown in braces.

### Visibility of controls and other visual elements

* `header_widget` [on]
  * `header_widget_dom_node` (disabling this feature hides the header widget DOM element)
  * `header_symbol_search`
  * `header_resolutions`
    * `header_interval_dialog_button`
      * `show_interval_dialog_on_key_press`
  * `header_chart_type`
  * `header_settings` (relates to Chart Properties button)
  * `header_indicators`
  * `header_compare`
  * `header_undo_redo`
  * `header_screenshot`
  * `header_fullsfcreen_button`
* `border_around_the_chart` [on]
* `header_saveload` (yep, it's not a part of `header_widget`) [on] 
* `left_toolbar` [on]
* `control_bar` [on] (relates to the navigation buttons at the bottom)
* `timeframes_toolbar` [on]
* `edit_buttons_in_legend` [on]
* `context_menus` [on]
 * `pane_context_menu`
 * `scales_context_menu`
 * `legend_context_menu`
* `display_market_status` [on]
* `remove_library_container_border` [off]

### Elements placement

* `move_logo_to_main_pane` [off]
* `header_saveload_to_the_right` [off]

### Behavior

* `use_localstorage_for_settings` [on]
 * `items_favoriting` (disabling this feature hides all "Favorite this item" buttons)
 * `save_chart_properties_to_local_storage` (disabling this feature prevents saving of chart properties (colors, styles, fonts) to the local storage, but still keeps saving of favorite items)
* `create_volume_indicator_by_default` [on]
* `volume_force_overlay` [on] (places Volume indicator on the same pane with the main series)
* `right_bar_stays_on_scroll` [on] (determines zoom behavior: bar under cursor is kept if disabled)
* `constraint_dialogs_movement` [on] (prevents moving dialogs out of the chart)
* `charting_library_debug_mode` [off] (enables logs)
* `narrow_chart_enabled` [off] (allows to shrink the chart up to 240px horizontally)
* `show_dialog_on_snapshot_ready` [on] (disabling this feature allows you to make a snapshot silently)
* `study_market_minimized` [on] (relates to Indicators dialog, determines whether it is compact or contains a search bar and categories)
* `side_toolbar_in_fullscreen_mode` [off] (using this feature you can enable Drawings Toolbar in fullscreen mode)
* `adapt_onchart_logo_background` [on] (logo background color is changed to match the background)

### "Big Rocks"

* `study_templates` [off]
* `datasource_copypaste` [on] (enables copying of drawings and studies)


## Trading Terminal

* `trading_options` [on] (related to the Properties dialog)