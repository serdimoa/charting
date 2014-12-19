**stability: 3 (stable)**

**The Charting Library does NOT include market data.** You must provide your own data in the required format. A sample data engine integrates Yahoo Finance API historical data. The charts can receive data in two ways:

1. Update in real-time with a PUSH type connection, for example through WebSocket. This way your charts will autoupdate with new prices when they arrive. To achieve this, you have to use the (JavaScript API)[JS-Api] and have your own transport method ready.

2. Update on a PULL/pulse/refresh basis (like most web-based charts today), where the chart data is updating every X number of seconds (the chart client will ask the server emulating PUSH updates), or only get reloaded manually by the user. For this, use the (UDF protocol)[UDF-protocol] and write your own datafeed wrapper. 
