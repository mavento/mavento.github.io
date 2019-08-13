---
layout: post
title: Change Listener for widgets
date: 2017-10-25 13:32:20 +0300
description: Change Listener for widgets
img: extjsapp.jpg
fig-caption: # Add figcaption (optional)
tags: [Angular]
---
If you add a change listener the following way, its executed for every widget on store load:
```javascript
    columns: [
        {
            dataIndex: 'contactTypeId', flex: 1,
            xtype: 'widgetcolumn',
            bind: {text: '{global_Type}'},
            widget: {
                xtype: 'combobox',
                name: 'contactTypes',
                reference: 'contactTypes',
                queryMode: 'local',
                displayField: 'value',
                valueField: 'id',
            }
```
To avoid this behaviour you have to add the change listener after the store load event.

So in your model define your store with a listener:
```javascript
Ext.define('App.view.DetailModel', {
    extend: 'Ext.app.ViewModel',
    alias: 'viewmodel.detailModel',

    data: {
        global_Type: 'global_Type',
        ...
    },

    constructor: function (config) {
        this.superclass.constructor.call(this, config);
       ...
    },
    stores: {
        contacts: {
            fields: [...}
            ],
            buffered: true,
            remoteSort: true,
            remoteFilter: true,
            pageSize: 250,
            leadingBufferZone: 300,
            autoLoad: false,
            proxy: {
                type: 'ajax',
                extraParams: {
                    tenantId: 'default',
                    componentId: 'default'
                },
                api: {
                    read: '...'
                },
                reader: {
                    type: 'json',
                    rootProperty: 'data',
                    successProperty: 'success',
                    totalProperty: 'totalCount'
                },// sends single sort as multi parameter
                simpleSortMode: true,
                listeners: {
                    exception: function (proxy, response, operation) {
                        console.error('Error Reading View');
                    }
                }
            },
            sorters: [{
                property: 'detail',
                direction: 'ASC'
            }],
            listeners: {
                load: 'onStoreLoad'
            }
        }      
    }   
});

```
Implement the onChange Event in your ViewController: 
```javascript
Ext.define('App.view.DetailController', {
    extend: 'Ext.app.ViewController',
    alias: 'controller.detailController',

   
    config: {
        tenantId: null, context: {},
        control: {
            '#': {
                beforerender: 'onBeforerender',
                afterrender: 'onAfterrender'
            },
           ...
        }
    },
       
    onChange: function (field, value) {
        var me = this, view = me.getView(), store = view.getStore(), phantom;
        var widgetRecord = field.getWidgetRecord();
        if (widgetRecord && field.isValid()) {
            me.suspendFieldEvents();
            var dataIndex = field.getWidgetColumn().dataIndex;
            var validFrom = widgetRecord.get('validFrom') ? widgetRecord.get('validFrom').getTime() : null,
                validTo = widgetRecord.get('validTo') ? widgetRecord.get('validTo').getTime() : null;
            if (dataIndex === 'contactTypeId') {
                contactTypeId = parseInt(value);
            }
            if (dataIndex === 'validFrom') {
                validFrom = value.getTime();
            }
            if (dataIndex === 'validTo') {
                validTo = value.getTime();
            }
            var record = Ext.create('App.model.Detail', {
                id: widgetRecord.get('id'),
                validFrom: validFrom,
                validTo: validTo

            });
          //save the record
          me.resumeFieldEvents();
        }
    },
    suspendFieldEvents: function () {
            var me = this, formPnl = me.getView();
            var fields = formPnl.query('field');
            Ext.each(fields, function (field) {
                field.suspendEvent('change');
            });
     },
     resumeFieldEvents: function () {
             var me = this, view = me.getView();
             var fields = view.query('field');
             Ext.each(fields, function (field) {
                 field.resumeEvent('change');
             });
      },
});


``` 


