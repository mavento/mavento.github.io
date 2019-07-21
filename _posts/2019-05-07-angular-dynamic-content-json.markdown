---
layout: post
title:  "Dynamic Angular Components from Json"
date:   2019-05-26 09:00:00 +0100
categories: Angular````
---
Loading components from JSON and add them dynamically into the dom. 

[Code on Github](https://github.com/mavento/angular-dynamic-component-json)

[Example on Stackblitz](https://stackblitz.com/edit/angular-dynamic-component-json)

The JSON Input loaded from the server to create the composite component:

```json
{
  "items": [
    {
      "id": 1,
      "type": "simple",
      "title": "Simple Component",
      "hidden": false
    },
    {
      "id": 2,
      "type": "feature",
      "title": "Feature Component",
      "hidden": false
    },
    {
      "id": 3,
      "type": "widget",
      "title": "Widget Component",
      "hidden": false
    }
  ]
}
```
The item component is the parent component and contains the components rendered from json (simple, feature and widget).
The template reference #appitem will be used as injection point:
```
@Component({
  selector: 'app-item',
  template: `
    <div id="container">
      <div #appitem></div>
    </div>`,
  styleUrls: ['./item.component.scss']
})
export class ItemComponent implements OnInit {
```
The property appitem gives access to the template reference from code:
```
@ViewChild('appitem', {static: true, read: ViewContainerRef}) appitem: ViewContainerRef;
```
ComponentFactoryResolver is used to create instances of the components. It exposes the method resolveComponentFactor and the result componentfactory
is used to create the components:
```
let factory = this.componentFactoryResolver.resolveComponentFactory(componentType)
this.componentRef = this.appitem.createComponent(factory);
```
Every dynamic created component must be registered within the entryComponents section in app.module.ts:
```
 entryComponents: [SimpleComponent, FeatureComponent, UnrecognizedComponent, WidgetComponent],
```





