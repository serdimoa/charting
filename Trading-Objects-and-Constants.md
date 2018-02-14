:chart: All content on this page is relevant for [Trading Terminal](Trading-Terminal.md) only.

**NOTE:** If you use TypeScript - you can import this constants/interfaces/types from `broker-api.d.ts` file.

## Broker Configuration

### configFlags: object

This is an object that should be passed in the constructor of the Trading Terminal to [brokerConfig](Widget-Constructor.md#brokerConfig). Each field should have a boolean value (`true`/`false`):

* `supportReversePosition`

    Broker supports reverse of a position.
    If it is not supported by broker, Chart will have the reverse button, but it will place a reverse order.

* `supportClosePosition`

    Broker supports close of a position.
    If it is not supported by broker, Chart will have the close button, but it will place a close order.

* `supportReducePosition`

    Broker supports changing of a position without orders.

* `supportPLUpdate`

    Broker provides PL for a position. If the broker calculates profit/loss by itself it should call [plUpdate](Trading-Host.md#plupdatepositionid-pl) as soon as PL is changed.
    Otherwise Chart will calculate PL as a difference between current trade and an average price of the position.

* `supportOrderBrackets`

    Broker supports brackets (take profit and stop loss) for orders.
    If this flag is `true` the Chart will display additional fields in the order ticket and Modify button on a chart and in the Account Manager.

* `supportPositionBrackets`

    Broker supports brackets (take profit and stop loss orders) for positions.
    If this flag is `true` the Chart will display an Edit button for positions and add `Edit position...` to the context menu of a position.

* `supportTradeBrackets`

    Broker supports brackets for trades (take profit and stop loss orders).
    If this flag is `true` the Chart will display an Edit button for trades (individual positions) and add `Edit position...` to the context menu of a trade.

* `supportTrades`

    Broker supports individual positions (trades).
    If it is set to `true`, there will be two tabs in the Account Manager - Individual Positions and Net Positions.

* `requiresFIFOCloseTrades`

    Trading account requires closing of trades in FIFO order.

* `supportCloseTrade`

    Individual positions (trades) can be closed.

* `supportMultiposition`

    Supporting multiposition prevents creating default implementation for reverse position.

* `showQuantityInsteadOfAmount`

    This flag can be used to change "Amount" to "Quantity" in the order dialog

* `supportLevel2Data`

    Level2 data is used for DOM widget. `subscribeDepth` and `unsubscribeDepth` should be implemented.

* `supportStopLimitOrders`

    This flag adds stop-limit orders type to the order dialog.

* `supportMarketBrackets`

    Using this flag you can disable brackets for market orders. By default it is enabled.

* `supportModifyDuration`

    Using this flag you can enable modification of the duration of the existing order. By default it is disabled.

### durations: array of objects

List of expiration options of orders. It is optional. Do not set it if you don't want the durations to be displayed in the order ticket.
The objects have two kes: `{ name, value }`.

Example:

```javascript
durations: [{ name: 'DAY', value: 'DAY' }, { name: 'GTC', value: 'GTC' }]
```

### customNotificationFields: array of strings

Optional field. You can use it if you have custom fields in orders or positions that should be taken into account when showing notifications.

For example, if you have field `additionalType` in orders and you want the chart to show a notification when it is changed, you should set:

```javascript
customNotificationFields: ['additionalType']
```

## Order

Describes a single order.

* `id` : String
* `symbol` : String
* `brokerSymbol` : String. Can be empty if broker symbol is the same as TV symbol.
* `type` : [OrderType](#ordertype)
* `side` : [Side](#side)
* `qty` : Double
* `status` : [OrderStatus](#orderstatus)
* `limitPrice` : double
* `stopPrice` : double
* `avgPrice` : double
* `filledQty` : double
* `parentId` : String. If order is a bracket parentOrderId should contain base order/position id.
* `parentType`: [ParentType](#parenttype)
* `duration`: [OrderDuration](#orderduration)

## Position

Describes a single position.

* `id`: String. Usually id should be equal to brokerSymbol
* `symbol` : String
* `brokerSymbol` : String. Can be empty if broker symbol is the same as TV symbol.
* `qty` : positive number
* `side`: [Side](#side)
* `avgPrice` : number

## Trade

Describes a single trade (individual position).

* `id`: String. Usually id should be equal to brokerSymbol
* `symbol` : String
* `date`: number (UNIX timestamp in milliseconds)
* `brokerSymbol` : String. Can be empty if broker symbol is the same as TV symbol.
* `qty` : Double positive
* `side`: [Side](#side)
* `price` : number

## Execution

Describes a single execution. Execution is a mark on a chart that displays trade information.

* `symbol` : String
* `brokerSymbol` : String. Can be empty if broker symbol is the same as TV symbol.
* `price` : number
* `time`: Date
* `side` : [Side](#side)
* `qty` : number

## ActionMetainfo

Describes a single action to put it into a dropdown or a context menu. It is a structure.

* `text` : String
* `checkable` : Boolean. Set it to true if you need a checkbox.
* `checked` : Boolean. Value of the checkbox.
* `enabled`: Boolean
* `action`: function. Action is executed when user clicks the item. It has 1 argument - value of the checkbox if exists.

## OrderType

Number constants to describe an order status.

```javascript
OrderType.Limit = 1
OrderType.Market = 2
OrderType.Stop = 3
OrderType.StopLimit = 4
```

## Side

Number constants to describe an order/execution side.

```javascript
Side.Buy = 1
Side.Sell = -1
```

## ParentType

Number constants to describe a bracket owner.

```javascript
ParentType.Order = 1
ParentType.Position = 2
```

## OrderStatus

Number constants to describe an order status.

```javascript
OrderStatus.Canceled = 1 // order is canceled
OrderStatus.Filled = 2 // order is fully executed
OrderStatus.Inactive = 3 // bracket order is created but waiting for a base order to be filled
OrderStatus.Placing = 4 // order is not created on a broker side yet
OrderStatus.Rejected = 5 // order is rejected for some reason
OrderStatus.Working = 6 // order is created but not executed yet
```

## OrderDuration

Duration or expiration of an order.

* `type`: string identifier from the list that you passes to [durations](#orderduration)
* `datetime`: number

## DOMEObject

Object that describes a single DOME response.

* `snapshot`: Boolean

    Positive value means that previous data should be cleaned

* `asks`: array of DOMELevel sorted by price ascendingly
* `bids`: array of DOMELevel sorted by price ascendingly

## DOMELevel

Single DOME price level object.

* `price`: double
* `volume`: double

## OrderTicketFocusControl

Number constants to set focus when you open standard Order dialog or Position dialog.

```javascript
OrderTicketFocusControl.StopLoss = 1 // focus stop loss control
OrderTicketFocusControl.StopPrice = 2 // focus stop price for StopLimit orders
OrderTicketFocusControl.TakeProfit = 3 // focus take profit control
```

## Brackets

* `stopLoss`: double
* `takeProfit`: double

## Formatter

An object with `format` method that can be used to format number to string.
