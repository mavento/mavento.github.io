---
layout: post
title:  "ExtJS Component Events"
date:   2018-03-17 06:47:22 +0100
categories: javascript ExtJS
---
I found this, which is very useful in case you need to know what events are fired by an extjs component.
Just put this in initEvents function in the config of the component.   

```javascript
initEvents : function() {
	Ext.util.Observable.capture( this, function(event) {
	console.log(event); });
}
```