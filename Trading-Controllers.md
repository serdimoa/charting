Trading Controller is a thing which will make your trading live. Its main purpose is to connect our charts with your trading logic. In terms of `JS`, it is an `object` which is expected to expose the specific interface. Here is a list of Controller's properties which Terminal will expect to have.


###supportFloatingPanel()
Function should return `true` for Floating Trading Panel to be displayed.

###supportBottomWidget()
Function should return `true` for Bottom Trading Panel to be displayed.

###buttonDropdownItems()
Bottom Trading Panel has a button with a list of dropdown items. Return an array of objects. Each object has the following properties:
1. `text` - the only mandatory field. Use `'-'` to display a separator line.
2. `checkable` - set it to `true` to have a checkbox
3. `checked` - initial value of the checkbox
4. `action` - function to perform when the item is clicked

###chartContextMenuItems()
Chart can have a sub-menu `Trading` in the context menu. Return list of items for a sub-menu. Format is the same as for `buttonDropdownItems`.

###bottomContextMenuItems()
Bottom Trading Panel can have a context menu. Return a list of items for this menu. Format is the same as for `buttonDropdownItems`.

###isTradable(symbol)
This function is required for the Floating Trading Panel. Ability to trade via the panel depends on the result of this function: `true` or `false`.

###showOrderDialog(order)
This function is invoked by the Floating Trading Panel when user clicks on order creation. Order is an object with the following properties:
1. `type`: `"limit"` or `"market"`
2. `side`: `"sell"` or `"buy"`
3. `price`: price at the moment when a user clicks an order
4. `symbol`: current symbol string
5. `qty`: quantity
6. `limit_price`: is used when a user clicks on a bid/ask order
7. `formatted_limit_price`: is a limit_price formatted for displaying in an edit field

###createBottomWidget(container)
This function is invoked when it is needed to create a Bottom Trading Panel. You should create DOM object and append it to the `container`. The container shows a vertical scroll bar when it is needed.

**When you use sample trading dialogs you need to have in addition the following functions:**

###placeOrder(symbol, side, orderType, qty, price, stopLoss, takeProfit, expiration)
1. `symbol` - symbol string
2. `side`: `"sell"` or `"buy"`
3. `orderType`: `"limit"`, `"market"` or `"stop"`
4. `qty`: quantity
5. `price`: entered price
6. `stopLoss`: entered stop loss price
7. `takeProfit`: entered take profit price
8. `expiration`: object with the only valuable property `expiration` - UTC time of expiration

The function is invoked to finish creating an order. It should return $.Deferred object. When it is resolved it should have an order data argument which is an object with the following properties:
1. `symbol` - symbol string
2. `side`: `"sell"` or `"buy"`
3. `type`: `"limit"`, `"market"` or `"stop"`
4. `qty`: quantity
5. `price`: price
6. `status`: possible values: `working`, `filled`, `rejected`, `invalid`
7. `misc.reject_reason`: reason text
8. `id` - mandatory unique identified of the order

###modifyOrder(id, qty, price, stopLoss, takeProfit, expiration)

The function is invoked to modify an order.

###closePosition(symbol)

The function is invoked to create a close position order.

###getOrder(id)

The function is invoked to get an order by its `id`.

###reversePosition(symbol)

Create a reverse position order when this function is called.
