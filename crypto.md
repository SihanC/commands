# Crypto Commands

## Table of Contents
- [Hash](#hash)
    - [Generate MD5 hash](#generate-md5-hash)
    - [Generate SHA hash](#generate-sha-hash)
- [RSA Key](#rsa-key)
    - [Generate RSA private key](#generate-rsa-private-key)
    - [Generate RSA public key](#generate-rsa-public-key)

## Hash <a name="hash"></a>
### Generate MD5 hash <a name="generate-md5-hash"></a>
```console
$ md5 file.txt
```
```console
MD5 (file.txt) = d41d8cd98f00b204e9800998ecf8427e
```

### Generate SHA hash <a name="generate-sha-hash"></a>
默认 `shasum` 用的是 SHA-1, 也就是 160 bit, 输出是 40 个 hex characters.
```console
$ shasum file.txt
```
```console
da39a3ee5e6b4b0d3255bfef95601890afd80709  file.txt
```

用 `-a` 来指定 bit size, 比如 SHA-256:
```console
$ shasum -a 256 file.txt
```

## RSA Key <a name="rsa-key"></a>
### Generate RSA private key <a name="generate-rsa-private-key"></a>
默认生成的是 2048 bit private key.
```console
$ openssl genrsa
```

用 `-out` 指定输出文件:
```console
$ openssl genrsa -out ~/Desktop/private.pem
```

如果不想用默认的 2048 bit, 可以在最后指定 key size:
```console
$ openssl genrsa -out ~/Desktop/private.pem 4096
```

### Generate RSA public key <a name="generate-rsa-public-key"></a>
需要基于已经生成好的 private key:
```console
$ openssl rsa -in private.pem -pubout -out public.pem
```
