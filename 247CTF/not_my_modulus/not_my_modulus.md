# Description
We are trying to decrypt a packet capture taken on our internal network. We know you can decrypt the data using the correct private key, but we simply have too many. Can you identify the correct key?

It givens a simple .pcap file wich contains trace of a TLS connection and a paste with many private keys.

## My solution
So basically, this how TLS protocol works: A client requires a safe connection presenting a list of ciphers and hashes(Client Hello), the server select one cipher,one hash, generates a certificate and send back to the client(Server Hello Certificate), the client confirms the certificate and starts connection(ACK).

https://wiki.wireshark.org/TLS
https://en.wikipedia.org/wiki/Transport_Layer_Security

First, we extract the certificate and store into a file "certificate.der".
https://security.stackexchange.com/questions/123851/how-can-i-extract-the-certificate-from-this-pcap-file

![alt text](https://github.com/ulissesj/CTFs/blob/main/247CTF/not_my_modulus/1_extract.PNG "Extract certificate")

Then, check the modulus of TLS using openSSL.

```
$ openssl x509 -inform der -in certificate.der -noout -modulus | openssl md5 
(stdin)= 22d74b8ffac0d807c2b209cb3bcff3ea
```
https://major.io/2007/09/14/check-the-modulus-of-an-ssl-certificate-and-key-with-openssl/


It's time to compare with the given private keys, so here is the script

```
#!/bin/bash

PUBLIC_KEY_MODULUS=$(openssl x509 -inform der -in certificate.der -noout -modulus | openssl md5)
echo $PUBLIC_KEY_MODULUS
for filename in keys/*; do
   #Extract modulus of every private key
   PRIVATE_KEY_MODULUS=$(openssl rsa -noout -modulus -in $filename | openssl md5)
   #If the modulus macth, we can decript using this private key 
   if [ "$PUBLIC_KEY_MODULUS" = "$PRIVATE_KEY_MODULUS" ]; then
        echo $filename
        break
    fi
done
```
Run the script to get filename.

```
$ ./decode.sh  
(stdin)= 22d74b8ffac0d807c2b209cb3bcff3ea
keys/518dfdb269ef17a932a893a63630644c.key
```

After that in Wireshark we go to Edit->Preferences->Protocol-TLS and add the Master-Secret key file. Filtering TLS and follow TCP stream should now give us the flag.

![alt text](https://github.com/ulissesj/CTFs/blob/main/247CTF/not_my_modulus/2_flag.PNG "Flag")
                                             


