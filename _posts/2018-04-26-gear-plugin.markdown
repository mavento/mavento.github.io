---
layout: post
title:  "ExtJS Plugin Beispiel"
date:   2018-04-26 09:00:00 +0100
categories: javascript ExtJS````
---
Plugins in Ext JS werden verwendet, um  UI Komponenten neue benutzerdefinierte Funktionen hinzuzuf&uuml;gen.
Das Beispiel zeigt ein Plugin, welches einen zus&auml;tzlichen Container an ein Panel andockt.

![Gear Plugin](https://raw.githubusercontent.com/mavento/Gear/master/resources/gear.gif "Gear Plugin")


Die Klasse **Ext.plugin.Abstract** dient als Basis Klasse von der die Plugin Klasse erbt:

{% highlight javascript %}
Ext.define('Gear.ux.GearPlugin', {
    extend: 'Ext.plugin.Abstract',
    alias: ['plugin.gearPlugin'],

    requires: [
        'Ext.layout.container.Fit'
    ],

    parentGrid: null,
    dockContainer: null,
    defaultDockContainerHeight: 30,
{% endhighlight %}

Das Plugin muss die Methode **init** implementieren:

{% highlight javascript %}
    init: function (hostCompenent) {
        this.parentGrid = hostCompenent;
        if (!this.dockHeight) {
            this.dockHeight = this.defaultDockContainerHeight;
        }
        var containerCfg = this.getDockContainerConfig();
        this.dockContainer = this.parentGrid.addDocked(containerCfg)[0];

        this.addPluginTool();
    },
{% endhighlight %}

Die Methode **getDockContainerConfig**, welche bei der Initialisierung aufgerufen wird, erzeugt den Container, um welchen die 
UI Komponente erweitert wird:
{% highlight javascript %}
getDockContainerConfig: function () {
        return {
            xtype: 'container',
            dock: 'top',
            height: this.dockHeight,
            hidden: true,
            layout: {
                type: 'fit',
                titleCollapse: false
            },
            items: [
            ]
        };
    },
{% endhighlight %}
Am Ende der Initialisierung wird der Aufruf des Containers zur UI Komponente hinzugef&uuml;gt. Die Mehode **addPluginTool**
f&uuml;gt ein Gear Symbol zur Titelzeile hinzu, &uuml;ber welches der Container ein- und ausgeblendet werden kann: 
{% highlight javascript %}
 addPluginTool: function () {
        var me = this;

        this.parentGrid.addTool({
            type: 'gear',
            handler: function () {
                var panel = me.dockContainer;
                if (panel.isHidden()) {
                    panel.show();
                } else {
                    panel.hide();
                }
            }
        });
    }
{% endhighlight %}

## Beispiel

[Gear Plugin](https://fiddle.sencha.com/#view/editor&fiddle/2fe0 "Sencha Fiddle")




