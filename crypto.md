# Crypto Commands

## Table of Contents
- [Generate md5 hash](#generate-md5-hash)
- [Generate sha hash](#generate-sha-hash)
- [Generate RSA Key](#generate-rsa-key)
    - [Generate Private Key](#generate-rsa-private-key)
    - [Generate Public Key](#generate-rsa-public-key)

## Generate md5 hash <a name="generate-md5-hash"></a>
```console
$ md5 file.txt
```   
```console
MD5 (file.txt) = d41d8cd98f00b204e9800998ecf8427e
```   

## Generate sha hash <a name="generate-sha-hash"></a>
By default **shasum** use sha1, which is 160 bit and 40 hex.
```console
$ shasum file.txt
```
```console
da39a3ee5e6b4b0d3255bfef95601890afd80709  file.txt
```
**-a size** 来 specify bit size. 比如 **-a 256** 就是用 sha 256

## Generate RSA Key <a name="generate-rsa-key"></a>
### Generate RSA Private Key <a name="generate-rsa-private-key"></a>
By default generate 的是 2048 bit.
```console
$ openssl genrsa
```
Use **-out file** to output file to a specific location
```console
$ openssl genrsa -out ~/Desktop/private.pem
```
在后面还可以跟上要 generate 的 key 的 size 如果不要 default 的 2048 bit 的话.
```console
$ openssl genrsa -out ~/Desktop/private.pem 4096
```

### Generate RSA Public Key <a name="generate-rsa-public-key"></a>
需要 take in 已经 create 的 private key.
```console
$ openssl rsa -in private.pem -pubout -out public.pem
```