**version: 1.1 / stability: 2 (rather stable)**

`Feature` or `featureset` means a part of chart's functionality. There are simple (atomic) and complex (composite) features. Composite feature consists of simple features. Disabling composite feature makes all of its simple parts to be disabled. Supported features are listed below.

* `header_widget` (**on** by default -- further marked as <on>)
  * `header_widget_dom_node`
  * `header_symbol_search`
  * `header_resolutions`
    * `header_interval_dialog_button`
      * `show_interval_dialog_on_key_press`
  * `header_chart_type`
  * `header_settings`
  * `header_indicators`
  * `header_compare`
  * `header_undo_redo`
  * `header_screenshot`
  * `header_fullscreen_button`
  * `header_saveload`

* `context_menus` <on>
 * `pane_context_menu`
 * `scales_context_menu`
 * `legend_context_menu`
* `left_toolbar` <on>
* `control_bar` <on>
* `timeframes_toolbar` <on>
* `edit_buttons_in_legend` <on>
* `use_localstorage_for_settings` <on> (disabling this feature will hide all "Favorite this item" buttons )
* `volume_force_overlay` <on>
* `create_volume_indicator_by_default` <on>
* `right_bar_stays_on_scroll` <on>
* `right_bar_stays_on_scroll` <on>
* `border_around_the_chart` <on>
* `header_saveload` <on> (yep, it's not a part of `header_widget`)
* `constraint_dialogs_movement` <on>
* `adapt_onchart_logo_background` <on>
* `charting_library_debug_mode` <off>
* `narrow_chart_enabled` <off>
* `trading_options` <off>
* `move_logo_to_main_pane` <off>
