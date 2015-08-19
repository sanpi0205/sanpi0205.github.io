---
layout: post
title:  "Javascript baidu heatmap"
date:   2015-08-18 22:44:40
categories: Javascript
---

### Javascript+百度地图绘制heatmap

#### html文件

{% highlight html %}

<!DOCTYPE html>
<html>
    <head>
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <meta name="viewport" content="initial-scale=1.0, user-scalable=no" />
        <script type="text/javascript" src="http://api.map.baidu.com/api?v=2.0&ak= your key"></script>
        <script type="text/javascript" src="http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js"></script>
        <script type="text/javascript" src="http://api.map.baidu.com/library/Heatmap/2.0/src/Heatmap_min.js"></script>
        <title>热力图功能示例</title>
        <style type="text/css">
            ul,li{list-style: none;margin:0;padding:0;float:left;}
            html{height:100%}
            body{height:100%;margin:0px;padding:0px;font-family:"微软雅黑";}
            #container{height:500px;width:100%;}
            #r-result{width:100%;}
            </style>
    </head>
    <body>
        <div id="container"></div>
        <div id="r-result">
            <input type="button"  onclick="openHeatmap();" value="显示热力图"/><input type="button"  onclick="closeHeatmap();" value="关闭热力图"/>
        </div>
    </body>
</html>

{% endhighlight %}

#### javascipt部分

{% highlight js %}

<script type="text/javascript">
    var map = new BMap.Map("container");          // 创建地图实例
    var tt;
    
    var point = new BMap.Point(116.418261, 39.921984);
    map.centerAndZoom(point, 12);             // 初始化地图，设置中心点坐标和地图级别
    map.enableScrollWheelZoom(); // 允许滚轮缩放
    
    // points为数据格式，本例中用jquery加载json文件    
    var points =[
                 {"lng":116.418261,"lat":39.921984,"count":50},
                 {"lng":116.423332,"lat":39.916532,"count":51},
                 {"lng":116.419787,"lat":39.930658,"count":15},
                 {"lng":116.418455,"lat":39.920921,"count":40},
                 {"lng":116.418843,"lat":39.915516,"count":100},
                 {"lng":116.425867,"lat":39.918989,"count":8}];
                 
	 if(!isSupportCanvas()){
	     alert('热力图目前只支持有canvas支持的浏览器,您所使用的浏览器不能使用热力图功能~')
	 }
	//详细的参数,可以查看heatmap.js的文档 https://github.com/pa7/heatmap.js/blob/master/README.md
	// source: http://www.patrick-wied.at/static/heatmapjs/?utm_source=gh
	//参数说明如下:
	/* visible 热力图是否显示,默认为true
	 * opacity 热力的透明度,1-100
	 * radius 势力图的每个点的半径大小   
	 * gradient  {JSON} 热力图的渐变区间 . gradient如下所示
	 *	{
	 .2:'rgb(0, 255, 255)',
	 .5:'rgb(0, 110, 255)',
	 .8:'rgb(100, 0, 255)'
	 }
	 其中 key 表示插值的位置, 0~1. 
	 value 为颜色值. 
	 */

	 // jquery 加载 json 数据
	$.ajax({
		url: 'data.json',
		type: 'GET',
		dataType: 'json',
		success: function(data) {
			show_data(data);
		}
	});

	heatmapOverlay = new BMapLib.HeatmapOverlay({"radius":5});
	map.addOverlay(heatmapOverlay);
	//heatmapOverlay.setDataSet({data:points, max:100});

	function show_data(data){
		heatmapOverlay.setDataSet({data: data, max:150109});
	}

	//是否显示热力图
	function openHeatmap(){
	    heatmapOverlay.show();
	}
	function closeHeatmap(){
	    heatmapOverlay.hide();
	}
	//closeHeatmap();
	openHeatmap();
	function setGradient(){
	    /*格式如下所示:
	     {
		  		0:'rgb(102, 255, 0)',
		 	 	.5:'rgb(255, 170, 0)',
			  	1:'rgb(255, 0, 0)'
	     }*/
	    var gradient = {};
	    var colors = document.querySelectorAll("input[type='color']");
	    colors = [].slice.call(colors,0);
	    colors.forEach(function(ele){
	                   gradient[ele.getAttribute("data-key")] = ele.value; 
	                   });
	                   heatmapOverlay.setOptions({"gradient":gradient});
	}
	//判断浏览区是否支持canvas
	function isSupportCanvas(){
	    var elem = document.createElement('canvas');
	    return !!(elem.getContext && elem.getContext('2d'));
}
</script>

{% endhighlight %}

参考文献:

* [百度地图 heatmap][API]
* [百度Geocoding API][geocoding]
* [Heatmap js][heatmap]

[API]: http://developer.baidu.com/map/jsdemo.htm#c1_15
[geocoding]: http://developer.baidu.com/map/index.php?title=webapi/guide/webservice-geocoding
[heatmap]: https://github.com/pa7/heatmap.js



