# Description
Coding Challenges - Hash me if you can
You have 2 seconds to hash this message using sha512 algorithm
Send the answer back using https://ringzer0ctf.com/challenges/13/[your_hash]

----- BEGIN MESSAGE -----
"Somehash"
----- END MESSAGE -----

## My solution
Since it's a coding challenge and the hash expire in 2 seconds, we need to write a script to read the long message, hash it and redirect to the page. Here is my little script using Python requests.

First, we get my session cookie, to still access the challenge by the request

```scookies = {"PHPSESSID" :"somerandomcookie"}
resp = req.post("https://ringzer0ctf.com/challenges/13", cookies=cookies)
```
Get page content, split our message and hash it

```resp = req.post("https://ringzer0ctf.com/challenges/13", cookies=cookies)
dump = resp.text.split('-----<br />')[1].split('<br />')[0]
key = dump.strip()
hashed_key = hashlib.sha512(str(key).encode("utf-8")).hexdigest()
```
As we are now on the right page, we get the flag. 

```get_flag = req.get("https://ringzer0ctf.com/challenges/13/"+hashed_key,cookies=cookies)
print(get_flag.text)
```

We can redirect our output to a file and then grep the flag

```$ 13.py > dump
$ grep "FLAG" dump
div class="alert alert-info">FLAG-mukgu5g2w932t2kx1nqnhhlhy4</div>
```

Full script

```import requests as req
import hashlib

cookies = {"PHPSESSID" :"somerandomcookie"}
resp = req.post("https://ringzer0ctf.com/challenges/13", cookies=cookies)
dump = resp.text.split('-----<br />')[1].split('<br />')[0]
key = dump.strip()
hashed_key = hashlib.sha512(str(key).encode("utf-8")).hexdigest()

get_flag = req.get("https://ringzer0ctf.com/challenges/13/"+hashed_key,cookies=cookies)
print(get_flag.text)
```


