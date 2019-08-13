---
layout: post
title: Angular Card List
date: 2019-08-07 13:32:20 +0300
description: Angular Card List
img: angular-card-list.jpg
fig-caption: # Add figcaption (optional)
tags: [Angular]
---
Loading components from JSON and add them dynamically into the dom. 

[Code on Github](https://github.com/mavento/angular-card-list)

[Example on Stackblitz](https://angular-card-list.stackblitz.io/)

Example of using Angular Material Cards in a list. Build with Angular Material and some custom CSS

https://material.angular.io

The example is a row-based card list with 2 columns. It’s a common UI pattern.

```html
 <div class="container" fxLayout="column" fxLayoutGap="10px">
        <div fxLayout="row wrap">
            <div *ngFor="let item of items"
                 fxFlex="50"
                 fxLayout="column"
                 class="p-1">
                <mat-card>
                   ...
                </mat-card>
            </div>
        </div>
    </div>
```


