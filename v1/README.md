# Sender's side process

## Analogus to sender writing the message in plain paper
`echo abcdefghijklmnopqrstuvwxyz > myfile.txt`

## sender creates a PRIVATE KEY

`openssl genrsa -out private.pem 512`

Content of private.pem
```
-----BEGIN RSA PRIVATE KEY-----
MIIBOgIBAAJBALlkymntbY/aMse5qgiD85dJT3pehGUo6OayZGu55pkGSD1thCb3
VA/Fr9eCCRb726FzoGlO8C7CzTBQ0OiZKn8CAwEAAQJALr2tlrVImSsPAHHb35e8
81iFVDm+MW72ASvay5or/Eo9dIV5fwFkGEsgsUKuNZ5Xoufp5a8annQ2Qj0FK0O0
kQIhAPcg2a6B4CJsqlwBoGcw2nYtIg8Yw5hfT+aJnTm4nAtJAiEAwAyW1wwf3mUo
+j7oChqdM4u4inUMDwtBrrJtAtkGf4cCIQC8YfhORIa89yTuOfcyclU2HLWH2JLR
hmZ8EI8fvxCEsQIgNHf8CfqtBkSbAmuHV6NXyYplu6Yoyj9oDYN/1uRWKycCIBWc
x4wX+y86DtaagD+xzpO1NI+IhnvijJcbLFbXKKoZ
-----END RSA PRIVATE KEY-----
```

## sender creates a PUBLIC LOCK
`openssl rsa -in private.pem -pubout > public.pem`

Content of public.pem
```
-----BEGIN PUBLIC KEY-----
MFwwDQYJKoZIhvcNAQEBBQADSwAwSAJBALlkymntbY/aMse5qgiD85dJT3pehGUo
6OayZGu55pkGSD1thCb3VA/Fr9eCCRb726FzoGlO8C7CzTBQ0OiZKn8CAwEAAQ==
-----END PUBLIC KEY-----
```

## sender is SIGNING his content using his PRIVATE key, a signature is generated
`openssl dgst -sha1 -sign private.pem -out sha1.sign myfile.txt`

Content of sha1.sign
```
5953 ef43 4d27 5d53 5d0f 4dfb cb2d da3e
499d 307a e5ea a1e8 a500 6ec0 f91c 383f
e975 751f f561 4225 acd8 713f ceeb b58e
bb04 2ec0 daf9 10f4 1b89 f3d2 89b9 bbc8
```


Output of `hexdump sha1.sign`:
```
0000000 5359 43ef 274d 535d 0f5d fb4d 2dcb 3eda
0000010 9d49 7a30 eae5 e8a1 00a5 c06e 1cf9 3f38
0000020 75e9 1f75 61f5 2542 d8ac 3f71 ebce 8eb5
0000030 04bb c02e f9da f410 891b d2f3 b989 c8bb
0000040
```

## Sender can now send his message i.e. myfile.txt alongwith his signature sha1.sign to receiver

# Receiver's side process

## Receiver starts verifying using sender's public key

```
openssl dgst -sha1 -verify public.pem -signature sha1.sign myfile.txt
```
If the file contents or signature is not tampered then you should get output as:
```
Verified OK
```
Else output should be:
```
Verification Failure
```

## Reference
- https://www.preveil.com/blog/public-and-private-key/
- https://brilliant.org/wiki/public-key-cryptography/
- https://ssd.eff.org/en/module/deep-dive-end-end-encryption-how-do-public-key-encryption-systems-work
- http://blakesmith.me/2010/02/08/understanding-public-key-private-key-concepts.html
- http://www.globalnerdy.com/2017/06/07/the-best-explanation-of-public-key-cryptography-for-non-techies-that-ive-seen/
- https://blog.vrypan.net/2013/08/28/public-key-cryptography-for-non-geeks/
- https://medium.com/@bn121rajesh/rsa-sign-and-verify-using-openssl-behind-the-scene-bf3cac0aade2