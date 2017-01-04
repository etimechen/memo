# ExtJs 当饼图只有一项数据记录占比为100%时tooltip和events无效问题修复

## 问题描述

```
var store = Ext.create('Ext.data.JsonStore', {
        fields: ['name', 'number'],
        data: [{
            'name': 'Homer',
            "number": 100
        }]
    });

Ext.create({
    xtype: 'panel',
    layout: {
        type: 'vbox'
    },
    items: [{
        xtype: 'polar',
        store: store,
        insetPadding: 20,
        innerPadding: 20,               
        legend: {
            docked: 'bottom'
        },
        interactions: ['rotate'],
        series: [{
            type: 'pie',
            label: {
                display: 'outside',
                field: 'name'
            },
            xField: 'number',
            tooltip: {
                style: 'background: #fff',
                renderer: function(storeItem, item) {
                    this.setHtml('test');

                }

            }
        }],
        width: '100%',
        height: 300
    }],
    width: 500,
    height: 600,
    renderTo: Ext.getBody()
});

```

当只有一项数据记录,饼图占比为100%时不显示tooltip,并且itemclick事件无效,演示地址: [https://fiddle.sencha.com/#fiddle/c42&view/editor](https://fiddle.sencha.com/#fiddle/c42&view/editor)

## 解决方法

> 重写饼图的betweenAngle方法,如果使用Sencha Cmd构建,应将此文件放入overrides文件夹内执行 `sencha app refresh`

```
Ext.define('overrides.chart.Pie', {
	override: 'Ext.chart.series.Pie',
	betweenAngle: function(x, a, b) {
        if (a === 0 && b > (Math.PI*2-0.00000001)) {
        	return true;
        } else {
        	var pp = Math.PI * 2,
            offset = this.rotationOffset;
	        if (!this.getClockwise()) {
	            x *= -1;
	            a *= -1;
	            b *= -1;
	            a -= offset;
	            b -= offset;
	        } else {
	            a += offset;
	            b += offset;
	        }
	        b -= a;
	        x -= a;
	        x %= pp;
	        b %= pp;
	        x += pp;
	        b += pp;
	        x %= pp;
	        b %= pp;
	        return x < b;
        }
    }
});
```

修复后的效果演示地址: [https://fiddle.sencha.com/#fiddle/jjq&view/editor](https://fiddle.sencha.com/#fiddle/jjq&view/editor)