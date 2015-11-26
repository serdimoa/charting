:chart: All content on this page is relevant for [[Trading Terminal]] only.

Trading Terminal is a ready-to-use product for those who want to have a great charting solution along with the ability to trade right from the chart. This product is based on Charting Library and includes all its functionality, but also contains a bunch of new features.

The product is under development so some features are not there yet. They are marked with :clock4:.

###Trading Terminal Features

####Trading capabilities
You can trade right from the chart, and all you have to do to make this work is to implement your [[Trading Controller]] and plug it into the chart widget.

<a href="https://www.dropbox.com/s/6ttw7a7tl6ipt27/tt_trading.png?dl=0" target="_blank"><img src="https://www.dropbox.com/s/6ttw7a7tl6ipt27/tt_trading.png?dl=1" width="300"/></a>

####Sidebar Quotes (Symbols Details & Watchlist)
In Trading Terminal, you can have the Watchilsts and Details widget (see the snapshot below) functionality.

<a href="https://www.dropbox.com/s/hrs6l0ejwgvw0mr/tt_top.png?dl=0" target="_blank"><img src="https://www.dropbox.com/s/hrs6l0ejwgvw0mr/tt_top.png?dl=1" width="300"/></a>

######Read more about this feature:
  * [[How to enable sidebar quotes|Widget-Constructor#chart-widgetbar]]
  * How to provide the data for quotes: depends on what kind of data integration do you use --[[JS API|JS-Api#getquotessymbols-ondatacallback-onerrorcallback]] or [[UDF|UDF#quotes]]

####Sidebar Market News Feed
You can have the news feed right in the side bar of the chart. Our support for the news feeds is flexible: so, in example, you can have different feeds for different kinds of symbols and so on.

<a href="https://www.dropbox.com/s/qa7f42mszeexf96/tt_bottom.png?dl=0" target="_blank"><img src="https://www.dropbox.com/s/qa7f42mszeexf96/tt_bottom.png?dl=1" width="300"/></a>

######Read more about this feature:
  * [[How to enable sidebar news|Widget-Constructor#chart-widgetbar]]
  * [[How to set up which feeds to use|Widget-Constructor#chart-rss_news_feed]]

####Miltiple charts layout :clock4:
You can have multiple charts inside if the same widget. This gives your user the ability to use wide range of the strategies, as well as the ability to have a broad view of the market. You don't have to do anythng to enable of tweak it: it will work out-of-the-box as soon as we'll publish it.

<a href="https://www.dropbox.com/s/ev65w402f2n7hpw/tt_charts.png?dl=0" target="_blank"><img src="https://www.dropbox.com/s/ev65w402f2n7hpw/tt_charts.png?dl=1" width="300"/></a>

####Japanese charts types: Kagi, Renko, Point & Figure, Line Break :clock4:
These types of charts will be available out-of-the-box, just like Heikin Ashi is available in Charting Library.

####Volume Profile :clock4:
This study will require some server-side support. We'll provide more detals when it's ready.

####Drawing Tools Templates :clock4: 
This fucntionality will require the support from your backend. We'll update our open-source data backend to support this feature also, so consider using it to minimize the efforts from your side.

#See Also
  * [[How to connect your trading to the charts|Trading Controller]] 