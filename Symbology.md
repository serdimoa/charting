Charting Library consumes your own data so symbology is 100% up to you. You may use whatever naming convention you want. Just return symbol info in Charting Library format and use arbitrary symbols names. Virtually, symbol name may be an arbitrary string containing any characters.

But there are some fine points you should know about:

1. Our own symbology assumes symbols names have `EXCHANGE:SYMBOL` format. Surely Library supports this by default. So if you like it -- just keep calm and use it.
2. Already having other symbology or just going to have one ? There is `ticker` term defined expecially for you. `Ticker` is symbol's unique identifier which is used **only** inside of Library. Your users will never see it. So just put `ticker` values in all of your SymbolInfo and Symbol Search results and expect Charting Library will ask you for data using those values.

###SymbolInfo Structure

**This section is extremely important. 34.2% of all issues experienced by Charting Library users are caused by wrong/malformed SymbolInfo data.**

SymbolInfo is an object containing symbol metadata. This object is the result of resolving the symbol. SymbolInfo has following fields:

######field0
