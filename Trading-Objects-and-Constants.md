:chart: All content on this page is relevant for [[Trading Platform]] only.

##Order

Describes a single order. 

* id : String
* symbol : String
* brokerSymbol : String. Can be empty if broker symbol is the same as TV symbol.
* type : [[OrderType|Trading-Objects-and-Constants#ordertype]]
* side : [[Side|Trading-Objects-and-Constants#side]]
* qty : Double
* status : [[OrderStatus|Trading-Objects-and-Constants#orderstatus]]
* price : double
* avg_price : double
* filledQty : double
* parentId : String. If order is a bracket parentOrderId should contain base order/position id.
* parentType: [[ParentType|Trading-Objects-and-Constants#parenttype]]

##Position

Describes a single position. 

* id: String. Usually id should be equal to brokerSymbol
* symbol : String
* brokerSymbol : String. Can be empty if broker symbol is the same as TV symbol.
* qty : Double positive
* side: [[Side|Trading-Objects-and-Constants#side]]
* avg_price : Double

##Execution

Describes a single execution.

* symbol : String
* brokerSymbol : String. Can be empty if broker symbol is the same as TV symbol.
* price : double
* time: time_t
* side : [[Side|Trading-Objects-and-Constants#side]]
* qty : double
 

##ActionMetainfo

Describes a single action to put it into a dropdown or a context menu. It is a structure.

* text : String
* checkable : Boolean. Set it to true if you need a checkbox.
* checked : Boolean
* Value of the checkbox.
* enabled: Boolean
* action: function. Action is executed when user clicks the item. It has 1 argument - value of the checkbox if exists.

##OrderType

String constants to describe an order status.

* market
* limit
* stop
* stoplimit

##Side

String constants to describe an order/execution side.

* buy
* Sell


##ParentType

String constants to describe a bracket owner.

order
position


##OrderStatus

String constants to describe an order status.

* placing 	order is not created on a broker side yet
* inactive 	bracket order is created but waiting for a base order to be filled
* working	order is created but not executed yet
* rejected	order is rejected for some reason
* filled	order is fully executed
* canceled	order is canceled