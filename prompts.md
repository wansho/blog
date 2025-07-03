## 三国演义地图

我正在看三国演义，想自己在地图上做些笔记，标记刘关张的路线图，然后在我自己的网站上显示，请问我可以怎么搞，我希望我标记的这些数据都是可以保留的，我可以用百度地图实现这个需求吗？就是在地图上做一些标记，并且将这些标记连成线


这是我目前的代码，请你帮我改写一下，我有如下的需求，

1. 地图全屏显示
2. points 里面的信息，除了 name 和经纬度外，还要有一字段存储详细信息，还要有一个时间字段存储那个事件发生的时间，name 存储事件名，还要有一个字段存储这个地方古时候的地名
3. 地图上显示的每一个点，只显示古时候的地名、事件名和时间，如果我鼠标放到点上了，再显示详细信息
4. 点与点之间的连线，太丑了，不要用红色的，并且要带箭头，入度要带箭头

<!DOCTYPE html>  
<html>  
<head>  
    <meta name="viewport" content="initial-scale=1.0, user-scalable=no" />  
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />  
    <title>POI 😋</title>  
    <!-- <style type="text/css">  
        html{height:100%}  
        body{height:100%;margin:0px;padding:0px}  
        #container{
          float: left; /* 让 div 靠左 */
          width: 70%; /* 设置宽度占据父容器的 70% */
          height: 100%;
        }  
        #poi{
          float: left;
          width: 30%;
          height: 100%;
        }
    </style>   -->

    <script type="text/javascript" src="https://api.map.baidu.com/api?v=1.0&type=webgl&ak=QP8jicLhkpvN65WvTP6C8QKZwA1HLamO"></script>

</head>  
<body>


  <script>


    window.onload = function() {
      // 在此处编写需要在页面加载完成后执行的代码
      var map = new BMapGL.Map("map");
      map.centerAndZoom(new BMapGL.Point(116.404, 39.915), 5); // 初始化中心
      map.enableScrollWheelZoom(true); 
      var points = [
        {name: "桃园", lng: 112.5, lat: 34.6},
        {name: "虎牢关", lng: 113.4, lat: 34.5},
        {name: "汜水关", lng: 113.0, lat: 34.5},
        // 你自己填补更多历史地点
      ];

      var polylinePoints = [];

      points.forEach(function(p) {
        var point = new BMapGL.Point(p.lng, p.lat);
        var marker = new BMapGL.Marker(point);
        map.addOverlay(marker);
        var label = new BMapGL.Label(p.name, {position: point});
        map.addOverlay(label);
        polylinePoints.push(point);
      });

      var polyline = new BMapGL.Polyline(polylinePoints, {strokeColor:"blue", strokeWeight:4, strokeOpacity:0.8});
      map.addOverlay(polyline);

    };
    
  </script>

  <div id="map" style="width: 100%; height: 600px;"></div>

</body>  
</html>