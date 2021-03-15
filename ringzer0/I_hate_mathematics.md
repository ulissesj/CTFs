# Description
Can you help me find the answer to this equation 
You have 2 seconds to send the answer
Send the answer back using https://ringzer0ctf.com/challenges/32/[answer]




----- BEGIN MESSAGE -----
2183 + 0x110a - 100111010010 = ?
----- END MESSAGE -----

Same process as the "Hash me please challenge". Get the page content, split the expression, calculate and redirect into the flag page


```simport requests as req

cookies = {"PHPSESSID" :"somecookie"}
resp = req.post("https://ringzer0ctf.com/challenges/32", cookies=cookies)
dump = resp.text.split('-----<br />')[1].split('<br />')[0]
expr = dump.strip()

expr_values = expr.split(' ')

first = expr_values[0]
second = expr_values[2]
third = expr_values[4]
```

I made a simple guess that the site accepts the answer in 'int' type, so we convert the values.
Since the operations '+' and '-' don't change, calculate the expression and convert to string.

```first_int = int(first)
second_int = int(second, base=16)
third_int = int(third, base=2)

#print(first_int, second_int, third_int)
final_expr = first_int + second_int - third_int
str_expr = str(final_expr)
```

Redirect to the flag page

```get_flag = req.get("https://ringzer0ctf.com/challenges/32/"+ str_expr,cookies=cookies)
print(get_flag.text)
```

And get the flag

```
$ python3 32.py > pagedump
$ grep "FLAG" pagedump
```


## Full script

```import requests as req

cookies = {"PHPSESSID" :"somecookie"}
resp = req.post("https://ringzer0ctf.com/challenges/32", cookies=cookies)
dump = resp.text.split('-----<br />')[1].split('<br />')[0]
expr = dump.strip()

expr_values = expr.split(' ')
#print(expr_values)

first = expr_values[0]
second = expr_values[2]
third = expr_values[4]

first_int = int(first)
second_int = int(second, base=16)
third_int = int(third, base=2)

#print(first_int, second_int, third_int)
final_expr = first_int + second_int - third_int
str_expr = str(final_expr)


get_flag = req.get("https://ringzer0ctf.com/challenges/32/"+ str_expr,cookies=cookies)
print(get_flag.text)
```
