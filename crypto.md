# Crypto Commands

## Table of Contents
- [Generate md5 hash](#generate-md5-hash)
- [Generate sha hash](#generate-sha-hash)
- [Generate RSA Key](#generate-rsa-key)
    - [Generate Private Key](#generate-rsa-private-key)
    - [Generate Public Key](#generate-rsa-public-key)

## Generate md5 hash <a name="generate-md5-hash"></a>
```shell
➜ md5 file.txt
```   
```shell
MD5 (file.txt) = d41d8cd98f00b204e9800998ecf8427e
```   

## Generate sha hash <a name="generate-sha-hash"></a>
By default **shasum** use sha1, which is 160 bit and 40 hex.
```shell
➜  shasum file.txt
```
```shell
da39a3ee5e6b4b0d3255bfef95601890afd80709  file.txt
```
**-a size** 来specify bit size. 比如 **-a 256**就是用sha 256

## Generate RSA Key <a name="generate-rsa-key"></a>
### Generate RSA Private Key <a name="generate-rsa-private-key"></a>
By default generate的是2048 bit.
```shell
➜ openssl genrsa
```
Use **-out file** to output file to a specific location
```shell
➜ openssl genrsa -out ~/Desktop/private.pem
```
在后面还可以跟上要generate的key的size如果不要default的2048 bit的话.
```shell
➜ openssl genrsa -out ~/Desktop/private.pem 4096
```

### Generate RSA Public Key <a name="generate-rsa-public-key"></a>
需要take in已经create的private key.
```shell
➜ openssl rsa -in private.pem -pubout -out public.pem
```