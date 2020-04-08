# Sender's side process

## Analogus to sender writing the message in plain paper
`echo "My gmail password is P@ssw0rd!" > secretmsg`

## Find MD5 hash of above message
`openssl md5 -out secretmsg-hash secretmsg`

## Output is written to file
`MD5(secretmsg)= fa24b8bff7d1060f4e57026fe8651901`

## sender creates a PRIVATE KEY and a PUBLIC LOCK
Generate a public-private key pair
```
openssl genrsa -out private 512
openssl rsa -in private -pubout > public
```
## Sender signs the hash value
Sign the hash which is generated above
`openssl dgst -sha1 -sign private -out sha-sign secretmsg-hash`

```
2231 01ba 92ab 7c72 88c2 e762 4128 a599
f4ed 30a1 56f1 1a05 9b3e 53df fc02 5537
e3d0 11b2 d900 cea4 f657 9b62 4ea3 6404
a222 440e 7884 f1a5 70e4 8ee1 0205 683e
```
# Receiver's side process

## Receiver starts verifying using sender's public key

`openssl dgst -sha1 -verify public -signature sha-sign secretmsg-hash`

If the message hash / signature is not tampered then you should get output as:
```
Verified OK
```
Else output should be:
```
Verification Failure
```