---
layout: post
title: Android browser recognizes CSS media type after page reloading
categories:
- Android
tags:
- Adndroid
- CSS
- HTML
- JavaScript
status: publish
type: post
published: true
comments: true
meta:
  _edit_last: '2'
  _wpas_skip_2592522: '1'
---
While trying to optimize site for mobile devices you have to add viewport meta tag:
``` html
<meta name="viewport" 
	content="target-densitydpi=device-dpi, width=device-width, height=device-height, initial-scale=1.0" />
```
You can add additional CSS file that will be used on mobile device only. To achieve this just add CSS media type. In Android developer's guide at <a href="http://developer.android.com/guide/webapps/targeting.html">http://developer.android.com/guide/webapps/targeting.html</a> it suggests to target device DPI value like:
<!--more-->
``` html
<link rel="stylesheet" href="css/mobile.css" type="text/css" 
	media="screen and (-webkit-device-pixel-ratio:1.0)" />
```
It works fine, but Google Chrome on desktop PC also loads this CSS file.

There is another solution to target mobile devices (both Android, iPhone, Blackberry): to specify maximal width of the screen like:
``` html
<link rel="stylesheet" href="css/mobile.css" type="text/css" 
	media="screen and (max-device-width:801px) and (max-width:801px)" />
```
The problem is that Android (at least 2.3 I've tested) recognizes this media type only after page reloading (refreshing) in browser. Googling the web I found the open <a title="issue" href="http://code.google.com/p/android/issues/detail?id=11961" target="_blank">issue</a>.

I solved it using JavaScript:
``` javascript
if(isMobile){
	var cssLink = document.createElement("link");
	cssLink.setAttribute("type", "text/css");
	cssLink.setAttribute("rel", "stylesheet");
	cssLink.setAttribute("href", "css/mobile.css");
	document.head.appendChild(cssLink);
}
```
I tested it on my device an it works fine, unless some little delay: the CSS file loads only after JavaScript executes. When google will fix the issue (I hope so...), there will be no delay, since original link is in the HEAD section.

Of course you have to detect mobile device using the JavaScript, I used solution suggested here: <a href="http://localstreamer.posterous.com/javascript-code-snippet-how-to-detect-all-mob">http://localstreamer.posterous.com/javascript-code-snippet-how-to-detect-all-mob</a>
