---
layout: post
title: Mat Icon does not center in its div
date: 2019-07-26 16:52:20 +0300
description: Mat Icon does not center in its div
img: mat-icon-not-centered.png
fig-caption: # Add figcaption (optional)
tags: [Angular]
---
[Example on Stackblitz](https://stackblitz.com/edit/angular-mat-icon-center-div)

The following css should create a red circle around the icon:
```
mat-icon {
  padding: 50px;
  background: red;
  border-radius: 50%;
}
```
But instead the icon is rendered at the right bottom

![Icon](../assets/img/201907/icon-not-centered.png)

The cause is the box-sizing property at the beginning of the css:
```
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
```
To fix this effect add the default for box sizing to the icon:
```
mat-icon {
  box-sizing: content-box;
  padding: 50px;
  background: red;
  border-radius: 50%;
}
```