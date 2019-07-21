---
layout: post
title: ExtJS Component Events
date: 2018-04-26 13:32:20 +0300
description: ExtJS Component Events
img: extjsapp.jpg
fig-caption: # Add figcaption (optional)
tags: [ExtJS]
---
I found this, which is very useful in case you need to know what events are fired by an extjs component.
Just put this in initEvents function in the config of the component.   

```javascript
initEvents : function() {
	Ext.util.Observable.capture( this, function(event) {
	console.log(event); });
}
```