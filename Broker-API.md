:chart: All content on this page is relevant for [[Trading Terminal]] only.

Broker API is a thing which will make your trading live. Its main purpose is to connect our charts with your trading logic. In terms of `JS`, it is an `object` which is expected to expose the specific interface. Here is a list of API's **methods** which Terminal will expect to have.

## Required Methods

#### constructor(host)
The constructor of the Broker API usually takes [[Trading Host]].

#### positions : Promise
This methods is called by the Trading Terminal to request positions. You should return an array of [[positions|Trading-Objects-and-Constants#position]].

#### orders : Promise
This methods is called by the Trading Terminal to request orders. You should return an array of [[orders|Trading-Objects-and-Constants#order]].

#### executions(symbol) : Promise
This methods is called by the Trading Terminal to request executions. You should return an array of [[executions|Trading-Objects-and-Constants#execution]].

#### trades : Promise
This methods is called by the Trading Terminal to request trades (individual positions). You should return an array of [[trades|Trading-Objects-and-Constants#trade]].

#### chartContextMenuActions(e)
Chart can have a sub-menu `Trading` in the context menu. Return the list of items for a sub-menu. Format is the same as for `buttonDropdownItems`.

`e` is a context object passed by a browser

#### connectionStatus()
Usually you don't need to return values other then `1`, because the broker is already connected when you create the widget. You can use it if you want to display a spinner in the bottom panel while the data is loaded.
Possible return values are:

```
ConnectionStatus.Connected = 1
ConnectionStatus.Connecting = 2
ConnectionStatus.Disconnected = 3
ConnectionStatus.Error = 4
```

#### isTradable(symbol)
This function is required for the Floating Trading Panel. Ability to trade via the panel depends on the result of this function: `true` or `false`. You don't need to imlement this method if all the symbols can be traded.

#### accountManagerInfo()
This function should return information that will be used to build an account manager.
See [[Account Manager]] for more information.

#### showOrderDialog([[order|Trading-Objects-and-Constants#order]])
This function is invoked by the chart when user requests to create or modify an order.

So we give you the ability to use your own dialog and it's 100% up to you how to manage it.

#### placeOrder([[order|Trading-Objects-and-Constants#order]], silently)

Method is invoked when a user want to place an order. Order is pre-filled with partial or full information.
If `silently` is `true` no order dialog should be shown.

#### modifyOrder([[order|Trading-Objects-and-Constants#order]], silently, focus)
1. `order` is an order object to modify
2. `silently` - if it is `true` no order dialog should be shown
3. `focus` - [[OrderTicketFocusControl constants|Trading-Objects-and-Constants#orderticketfocuscontrol]]. It can be already initialized by the chart.

Method is invoked when a user want to modify an existing order.

#### cancelOrder(orderId, silently)
This method is invoked to cancel single order with given `id`.
If `silently` is `true` no dialogs should be shown.

#### cancelOrders(symbol, side, ordersIds, silently)
1. `symbol` - symbol string
2. `side`: [[Side constant|Trading-Objects-and-Constants#side]] or `undefined`
3. `ordersIds` - ids already collected by `symbol` and `side`
If `silently` is `true` no dialogs should be shown.

This method is invoked to cancel multiple orders for a `symbol` and `side`.

#### editPositionBrackets(positionId, focus)
This method is invoked if `supportPositionBrackets` configuration flag is on to display a dialog for editing of take profit and stop loss.
1. `positionId` is ID of existing position to be modified
2. `focus` - [[Focus constant|Trading-Objects-and-Constants#focusoptions]].

#### closePosition(positionId, silently)
This method is invoked if `supportClosePosition` configuration flag is on to close the position by id.
If `silently` is `true` no dialogs show be shown.

#### reversePosition(positionId, silently)
This method is invoked if `supportReversePosition` configuration flag is on to reverse the position by id.
If `silently` is `true` no dialogs should be shown.

#### editTradeBrackets(tradeId, focus)
This method is invoked if `supportTradeBrackets` configuration flag is on to display a dialog for editing of take profit and stop loss.
1. `tradeId` is ID of existing trade to be modified
2. `focus` - [[Focus constant|Trading-Objects-and-Constants#focusoptions]].

#### closeTrade(tradeId, silently)
This method is invoked if `supportCloseTrade` configuration flag is on to close the trade by id.
If `silently` is `true` no dialogs show be shown.

#### symbolInfo(symbol) : Deferred (or Promise)
1. `symbol` - symbol string

This method is invoked by the internal Order Dialog, DOM panel and floating trading panel to get symbol information.
Result is an object with the following data:
- `qty` - object with fields `min`, `max` and `step` that specifies Quantity field step and boundaries.
- `pipSize` - size of 1 pip (e.g., 0.0001 for EURUSD)
- `pipValue` - values of 1 pip in account currency (e.g., 1 for EURUSD for an account in USD)
- `minTick` - minimal price change (e.g., 0.00001 for EURUSD). It is used for price fields.
- `description` - a description to be displayed in the dialog
- `type` - instrument type, only `forex` matters - it enables negative pips check in the order dialog

#### accountInfo() : Deferred (or Promise)

This method is invoked by the internal Order Dialog to get account information.
It should return only one field for now:
1. currencySign: string - which is a sign of acccount currency

Since this method is called the broker should stop providing profit/loss.

#### subscribeEquity()

Method should be implemented if you use standard order dialog and support stop loss.
Since this method is called the broker should provide equity updates via [[equityUpdate|Trading-Host#equityupdateequity]] method.

#### unsubscribeEquity()

Method should be implemented if you use standard order dialog and support stop loss.
Since this method is called the broker should stop providing equity updates.

And this is it !

# See Also
  * [[How to connect|Widget-Constructor#chart-trading_controller]] your trading controller to the chart
  * [[Trading Host]]
