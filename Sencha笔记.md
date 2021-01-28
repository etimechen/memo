###Sencha有两种Chart

* 一种为Sencha Chart,默认使用canvas渲染,可以通过属性engine修改渲染引擎为svg,示例访问地址:file:///Users/chenliang/Works/Develop/extjs/ext-5.1.1/build/examples/kitchensink/index.html?charts=true
* 另一种为ExtJS Legacy Chart,之前extjs遗留下来的Chart,使用svg渲染,示例访问地址:file:///Users/chenliang/Works/Develop/extjs/ext-5.1.1/build/examples/charts-kitchensink/index.html
* 开启方法:在应用程序目录下修改app.json 

``` Json
	"requires": [
	        "sencha-charts" //开启Sencha Chart
	        "ext-charts" //开启extjs Chart
	    ],
```