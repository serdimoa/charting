:chart: All content on this page is relevant for [[Trading Platform]] only.

Trading Host is API for interaction between [[Trading Controller]] and the Chart Trading Subsystem. Its main purpose is to exchange information between our charts with your trading logic. In terms of `JS`, it is an `object` with a set of functions. Here is a list of Hosts's **methods**.

##Commands

####showCancelOrderDialog(orderId, handler) : [JQuery::Deferred](https://api.jquery.com/category/deferred-object/)
Shows a confirmation dialog and executes handler if YES/OK is pressed.

####showClosePositionDialog([[position|Trading-Objects-and-Constants#position]], handler) : [JQuery::Deferred](https://api.jquery.com/category/deferred-object/)
Shows a confirmation dialog and executes handler if YES/OK is pressed.

####showReversePositionDialog([[position|Trading-Objects-and-Constants#position]], handler) : [JQuery::Deferred](https://api.jquery.com/category/deferred-object/)
Shows a confirmation dialog and executes handler if YES/OK is pressed.

####showEditBracketsDialog([[position|Trading-Objects-and-Constants#position]], handler) : [JQuery::Deferred](https://api.jquery.com/category/deferred-object/)
Shows a default edit brackets dialog and executes handler if MODIFY is pressed.

####activateBottomWidget : [JQuery::Deferred](https://api.jquery.com/category/deferred-object/)
Opens bottom panel and switches tab to Trading.

####showTradingProperties()
Shows the properties dialog, switches current tab to Trading.

####showNotification(title, text, type, time)
Displays a notification. Type can be ‘success’ or ‘error’. Time is set in milliseconds. Default time is 10000.

####triggerShowActiveOrders()
Triggers show active orders.

##Getters and Setters

####floatingTradingPanelVisibility: [[WatchedValue]]
Returns whether floatingTradingPanel is visible or not.

####activeOrdersOnly(): [[WatchedValue]]
Returns common setting option - show active orders.

####suggestedQty() : Object
Returned object properties:
1. value - use it to get current value
2. setValue - use it to set new value
3. changed : [[Subscription]]
It is to synchronize quantity in Trading Floating Panel and in the dialogs.

####defaultContextMenuActions()
Provides default buy/sell, show properties actions to be returned as a default by [chartContextMenuItems](https://github.com/tradingview/charting_library/wiki/Trading-Controller#chartcontextmenuitems).

##Data Updates
Using of these methods is required to notify the chart that it needs to update information.

####orderUpdate([[position|Trading-Objects-and-Constants#order]])
Call this method when an order is added or changed.

####positionUpdate ([[position|Trading-Objects-and-Constants#position]])
Call this method when a position is added or changed.

####executionUpdate([[position|Trading-Objects-and-Constants#execution]])
Call this method when an execution is added.

####fullUpdate()
Call this method when an data is invalidated. For example, user account has been changed.

####plUpdate(positionId, pl)
Call this method when a broker connection has received a PL update. This method should be used when `supportPLUpdate` flag is set in `configFlags`.