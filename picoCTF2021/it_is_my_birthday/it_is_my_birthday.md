# Description
```
I sent out 2 invitations to all of my friends for my birthday! I'll know if they get stolen because the two invites look similar, and they even have the same md5 hash, but they are slightly different! You wouldn't believe how long it took me to find a collision. Anyway, see if you're invited by submitting 2 PDFs to my website.
```
## Solution

The site gives an option to upload two files and check if they have the same MD5 hash, but they need to be different. After some research it seems to be a PHP vulnerability related to know as **magic hashes**, the hashes are slighty different but compared with "==" it returns true, see reference https://offsec.almond.consulting/super-magic-hash.html

So I created two .PDFs with magical hashes and uploaded to website with argument '-n' to not include breakline
```
$ echo -n "egNJHP66&3E1" > 1.PDF
$ echo -n "KnCM6ogsNA1W" > 2.PDF
```
After uploading, we can see PHP source code with the flag

### picoCTF{c0ngr4ts_u_r_1nv1t3d_aad886b9}

