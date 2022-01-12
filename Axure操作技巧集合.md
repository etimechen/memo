# Axure操作技巧集合

## Axure插入eCharts图表

Axure默认集成jquery库

1. 拖入矩形框到页面中

2. 矩形框里新建交互，函数里填入

```
javascript:
var script = document.createElement('script');
script.type = "text/javascript";
script.src ="https://cdn.jsdelivr.net/npm/echarts/dist/echarts.min.js";
document.head.appendChild(script);
setTimeout(function(){
    var dom =$('[data-label=test]').get(0);
    var myChart = echarts.init(dom);
    
    var option = {
        
    };
    
    if (option && typeof option === "object"){
       myChart.setOption(option, true);    
    }}, 800);
```

其中 `data-label=` 后面的名称为矩形框的名称

3. option中的内容去eCharts的示例上复制粘贴

## Axure 注意事项

* Axure 不支持js定时器，需要通过动态面板的状态来执行定时操作
