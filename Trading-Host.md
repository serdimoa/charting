:chart: All content on this page is relevant for [[Trading Platform]] only.

Trading Host is API for interaction between [[Trading Controller]] and the Chart Trading Subsystem. Its main purpose is to exchange information between our charts with your trading logic. In terms of `JS`, it is an `object` with a set of functions. Here is a list of Hosts's **methods**.

##Methods

####showOrderDialog(symbol, orderOptionsWidget) : JQuery::Deferred
Shows a default order dialog, embeds orderOptionsWidget and executes handler if buy/sell is pressed. 

####showCancelOrderDialog(orderId, handler) : JQuery::Deferred
Shows a confirmation dialog and executes handler if YES/OK is pressed.

####showClosePositionDialog(position, handler) : JQuery::Deferred
Shows a confirmation dialog and executes handler if YES/OK is pressed.

####showReversePositionDialog(position, handler) : JQuery::Deferred
Shows a confirmation dialog and executes handler if YES/OK is pressed.

####showEditBracketsDialog(position, handler) : JQuery::Deferred
Shows a default edit brackets dialog and executes handler if MODIFY is pressed.

####activateBottomWidget : JQuery::Deferred
Opens bottom panel and switches tab to Trading.

####showTradingProperties()
Shows the properties dialog, switches current tab to Trading.

####showNotification(title, text, type, time)
Displays a notification. Type can be ‘success’ or ‘error’. Time is set in milliseconds. Default time is 10000.

####floatingTradingPanelVisibility: [[WatchedValue]]
Returns whether floatingTradingPanel is visible or not.

####activeOrdersOnly(): [[WatchedValue]]
Returns common setting option - show active orders.

####triggerShowActiveOrders()
Triggers show active orders.

####suggestedQty() : Object
Returned object properties:
1. value - use it to get current value
2. setValue - use it to set new value
3. changed : [[Subscription]]
It is to synchronize quantity in Trading Floating Panel and in the dialogs.

####defaultContextMenuActions()
Provides default buy/sell, show properties actions to be returned as a default by [chartContextMenuItems](https://github.com/tradingview/charting_library/wiki/Trading-Controller#chartcontextmenuitems).