:chart: All content on this page is relevant for [[Trading Platform]] only.

Trading Controller is a thing which will make your trading live. Its main purpose is to connect our charts with your trading logic. In terms of `JS`, it is an `object` which is expected to expose the specific interface. Here is a list of Controller's **methods** which Platform will expect to have.

##Required Methods 

####setHost(host)
This methods is called on initialization to pass a [[Trading Host]] to the controller.

####configFlags()
Implement this method to provide configuration flags object. The result is an object with the following keys, that can be `true`/`false`:

* supportReversePosition

    Broker supports reverse of a position.
    If it is not supported by broker, Chart will have the reverse button, but it will place a reverse order.

* supportClosePosition

    Broker supports close of a position.
    If it is not supported by broker, Chart will have the close button, but it will place a close order.

* supportReducePosition

    Broker supports changing of a position without orders.

* supportPLUpdate

    Broker provides PL for a position. If the broker calculates profit/loss by itself it should call [[plUpdate|Trading-Host#plupdatepositionid-pl]] as soon as PL is changed. Otherwise Chart will calculate PL as a difference between current trade and an average price of the position.

* supportBrackets

    Broker supports brackets (take profit and stop loss orders). If this flag is `true` the Chart will display an Edit button for positions and add `Edit position...` to the context menu of a position.

* supportMultiposition

    Supporting multiposition prevents creating default implementation for reverse position.

####positions : [Deferred](https://api.jquery.com/category/deferred-object/)
####orders : [Deferred](https://api.jquery.com/category/deferred-object/)
####executions(symbol) : [Deferred](https://api.jquery.com/category/deferred-object/)
These methods are called by the Chart to request positions/orders/executions and display them on a chart.
You should return the appropriate lists of [[positions|Trading-Objects-and-Constants#position]], [[orders|Trading-Objects-and-Constants#order]] or [[executions|Trading-Objects-and-Constants#execution]]

####supportFloatingPanel()
Function should return `true` for Floating Trading Panel to be displayed.

####supportBottomWidget()
Function should return `true` for Bottom Trading Panel to be displayed.

####buttonDropdownItems()
Bottom Trading Panel has a button with a list of dropdown items. This function is expected to return an array of [[ActionMetainfo|Trading-Objects-and-Constants#actionmetainfo]], each of them representing one dropdown item.

####chartContextMenuItems()
Chart can have a sub-menu `Trading` in the context menu. Return the list of items for a sub-menu. Format is the same as for `buttonDropdownItems`.

####bottomContextMenuItems()
Bottom Trading Panel can have a context menu. Return a list of items for this menu. Format is the same as for `buttonDropdownItems`.

####isTradable(symbol)
This function is required for the Floating Trading Panel. Ability to trade via the panel depends on the result of this function: `true` or `false`. You don't need to imlement this method if all the symbols can be traded.

####createBottomWidget(container)
This function is called when it is needed to create a Bottom Trading Panel. You should create DOM object and append it to the `container`. The container shows a vertical scroll bar when it is needed.

####showOrderDialog([[order|Trading-Objects-and-Constants#order]])
This function is invoked by the Floating Trading Panel when user requests to create or modify an order. 

So we give you the ability to use your own dialog and it's 100% up to you how to manage it.

####placeOrder(symbol, side, orderType, qty, price, stopLoss, takeProfit, expiration)

1. `symbol` - symbol string
2. `side`: `"sell"` or `"buy"`
3. `orderType`: `"limit"`, `"market"` or `"stop"`
4. `qty`: quantity
5. `price`: entered price
6. `stopLoss`: entered stop loss price
7. `takeProfit`: entered take profit price
8. `expiration`: object with the only valuable property `expiration` - UTC time of expiration

####modifyOrder(id, qty, price, stopLoss, takeProfit, expiration)
This method is invoked to modify an order if orders confirmation is off when a user drags the order.

####editPositionBrackets(positionId)
This method is invoked if `supportBrackets` configuration flag is on to display a dialog for editing of take profit and stop loss.

####closePosition(positionId)
This method is invoked if `supportClosePosition` configuration flag is on to close the position by id.
And this is it !

####reversePosition(positionId)
This method is invoked if `supportReversePosition` configuration flag is on to reverse the position by id.


And this is it !

#See Also
  * [[How to connect|Widget-Constructor#chart-trading_controller]] your trading controller to the chart
  * [[Trading Host]]