Charting Library consumes your own data so symbology is 100% up to you. You may use whatever naming convention you want. Just return symbol info in Charting Library format and use arbitrary symbols names. Virtually, symbol name may be an arbitrary string containing any characters.

But there are some fine points you should know about:

1. Our own symbology assumes symbols names have `EXCHANGE:SYMBOL` format. Surely Library supports this by default. So if you like it -- just keep calm and use it.
2. Already having other symbology or just going to have one ? There is `ticker` term defined expecially for you. `Ticker` is symbol's unique identifier which is used **only** inside of Library. Your users will never see it. So just put `ticker` values in all of your SymbolInfo and Symbol Search results and expect Charting Library will ask you for data using those values.

###SymbolInfo Structure

**This section is extremely important. 34.2% of all issues experienced by Charting Library users are caused by wrong/malformed SymbolInfo data.**

SymbolInfo is an object containing symbol metadata. This object is the result of resolving the symbol. SymbolInfo has following fields:

#####name
It's name of a symbol. It is a string which your users will see. Also, it will be used for data requests if you are not using **tickers**.

#####exchange-traded, exchange-listed
For now both of this fields are expected to have short name of exchange where this symbol is traded. This name will be printed in chart's legend for this symbol. This field is not used for other purposes now.

#####timezone
Exchange timezone for this symbol. We expect to get name of time zone in olsondb format. Supported timezones are:

* America/New_York
* America/Los_Angeles
* America/Chicago
* America/Toronto
* America/Vancouver
* America/Argentina/Buenos_Aires
* America/Bogota
* Europe/Moscow
* Europe/Athens
* Europe/Berlin
* Europe/London
* Europe/Madrid
* Europe/Paris
* Europe/Warsaw
* Australia/Sydney
* Asia/Bangkok
* Asia/Tokyo
* Asia/Taipei
* Asia/Singapore
* Asia/Shanghai
* Asia/Seoul
* Asia/Kolkata
* Asia/Hong_Kong

#####pricescale