**stability: 3 (stable)**

**The Charting Library does NOT include market data.** You must provide your own data in the required format. A sample data engine integrates Yahoo Finance API historical data. The charts can receive data in two ways:

1. Update in real-time with a PUSH type connection, for example through WebSocket. This way your charts will autoupdate with new prices when they arrive. To achieve this, you have to use the [[JavaScript API|JS-Api]] and have your own transport method ready.

2. Update on a PULL/pulse/refresh basis (like most web-based charts today), where the chart data is updating every X number of seconds (the chart client will ask the server emulating PUSH updates), or only get reloaded manually by the user. For this, use the [[UDF protocol|UDF-protocol]] and write your own datafeed wrapper. 

###JavaScript API or UDF?

![](https://www.dropbox.com/s/dedg3tzlp7jj0v6/api_structure.png?dl=1)
The picture above illustrates the relationships between the [[default package|Package-Content]] components. Mandatory Charting Library parts are colored red. Green parts (default data transport) are included in default package (having non-minimized source code) and may be replaced. You may see the default data transport implements JS API. Also, default transport implements UDF protocol.