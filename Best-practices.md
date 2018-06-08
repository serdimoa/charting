## Creating the best user experience

We love our charts. We want them to be the best, the most beautiful, the most responsive and the most powerful charts in the whole HTML5 world. We are working hard to reach these goals.

We know all about our charts and about creating the best user experience using them and we'd be glad to share our knowledge with you. This document describes several best practices for integrating the Charting Library into your website/application. The main point is to always think about your users and their experience.

### 1. Understand what Charting Library IS and what it ISN’T

Charting Library is a charting component that is able to show prices, charts & technical analysis tools. Our Library does the chart magic, and nothing more. If you want some additional functionality (like chats, special symbols list, hottest deals, ads, etc) the best approach is to implement them outside of the chart. However, if you want to link your outside functionality with the library, you can use the library’s API to link them.

### 2. Return exactly as many bars as the Library requests

Library will ask your backend for data and provide the required data range bounds with each request. Respect these bounds and return data filling this range as fully as possible. Do not return more bars. Do not return bars out of the requested range. If you want to extend the default data range requested by the Library, use our JS API (see calculateHistoryDepth).

### 3. Return exactly as many marks as the Library requests

The same as for the bars above. Send only marks matching the requested range.

### 4. Do not override calculateHistoryDepth() to get more than 2 screens of data

Charting Library avoids loading content which the user didn’t ask for. Loading more bars into the chart means loading the CPU and memory with more operations than necessary. This means responsiveness of the whole solution decreases.

### 5. Do not make your chart look messy

Users like beautiful charts. So do we. Please remember to keep your chart looking good when customizing size or style. Avoid embedding custom controls that look alien to the entire chart's style.

### 6. Avoid making very small charts

The smallest meaningful size that the Library supports is 600x600px. Avoid making charts smaller because it looks like a mess. Use the `mobile` preset, or hide some toolbars if you need charts smaller than mentioned above.

### 7. Use the appropriate language

The Charting Library is translated into dozens of languages. Use the one that fits your users' needs.

### 8. If you are experiencing issues

We are always eager to help you. But, unfortunately, we are really busy guys, so we don’t have much time. Please help us spend our time effectively and always update your Library's build to the latest `unstable` version to check if the issue still happens. Contact us if it does.

Also, check the data you are passing to the Charting Library and make sure it matches our requirements as described in the documentation. Pay special attention to SymbolInfo content because it's the most common place to make an error (according to our statistics).

You can watch the output of our [demo data service](https://demo_feed.tradingview.com/quotes?symbols=AAPL) and compare it to yours to ensure your backend behaves like it should.

Always use `debug: true` in the Widget constructor options during the development and always remove it in the production to make the code work faster.

### 9. Read the docs

We spent a lot of time creating those docs for you to make your life easier. Please give it a try.

### 10. Choose an appropriate data transport for your solution

Pay attention to differences between JS API and UDF, and choose the API that fits your needs best.
Do not use UDF if you need really fast data updates or data streaming.
Do not use UDF with data grouping (see `supports_group_request`) if your backend has more than a dozen symbols.

### 11. Do not try to sniff our code and use undocumented features

All features not mentioned in our documentation are subjects to change without any warnings and backward compatibility. Also, altering the source code is strictly prohibited by the legal agreement you signed.

### 12. Do not use our demo datafeed on your production website

This datafeed is just a demo and is not intended for real usage. It may be unstable and can't bear significant load.

### 13. Use the API for customization. Avoid editing CSS

We do not guarantee CSS selectors' backward compatibility.

### 14. Set up your server to gzip files when sending to client

This is the common best practice for static HTML content. Gzipping the Library's HTML file will decrease your users' waiting time.

### 15. Set minimum expiration time for charting_library.min.js

All files in the Charting Library contain hash in their names, except for `charting_library.min.js` that you add to your HTML files.
When you update the Charting Library to a newer version all file names are changed as well.
If a browser loads `charting_library.min.js` from the cache, then all the links in this file are going to be broken.
The expiration time for this file should be set to the minimum in order to make sure that it’s not cached by the browser.
